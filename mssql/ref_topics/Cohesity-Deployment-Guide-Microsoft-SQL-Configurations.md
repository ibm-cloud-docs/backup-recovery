---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Introduction
{: #introduction}

“Successfully restoring a SQL database means you’ve protected the data; successfully recovering a server means you’ve protected the machine; successfully recovering after a disaster means you’ve protected the business.”

A good {{site.data.keyword.baas_full_notm}} deployment allows the business to securely protect its data and infrastructure. It positions the business to take advantage of evolving technologies like cloud services.

You are at the _Deployment considerations_ phase of implementing the {{site.data.keyword.baas_full_notm}} solution for your SQL Server databases. This guide focuses on the deployment and configuration choices you have to make as you set up {{site.data.keyword.baas_full_notm}} for your SQL Servers, _prior_ to deploying the platform to protect your specific databases.

## Audience
{: #audience}

This guide is for IT administrators and database administrators (DBAs) who are familiar with managing data protection for Microsoft SQL Servers.

**{{site.data.keyword.baas_full_notm}} recommends** having familiarity with:

·         [Microsoft SQL Server](https://www.microsoft.com/en-us/sql-server/default.aspx)

·         [Microsoft SQL Server Failover Cluster Instance](https://docs.microsoft.com/en-us/sql/sql-server/failover-clusters/install/sql-server-failover-cluster-installation) (FCI)

·         [Microsoft SQL Server Always On Availability Groups](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-2016) (AAG)

## Approach to Backups
{: #approach_to_backups}

SQL backups must fit into the customer’s requirements. Backups must meet Recovery Time Objectives (RTO) and Recovery Point Objectives (RPO). In addition to these, the backup approach must determine _which_ objects to protect and _how long_ to retain them.

## Deployment Workflow
{: #deployment_workflow}

SQL Server databases can be implemented in many different ways, so it’s important that you carefully evaluate each of the considerations discussed in this guide, to identify the best choices for your organization’s particular needs.

Armed with those choices, you will:

●      [Check the settings on the {{site.data.keyword.baas_full_notm}} that will backup your SQL Servers.](#_bookmark13)

●      [Fine-tune MS SQL Server parameters.](#_bookmark17)

●      [Link the {{site.data.keyword.baas_full_notm}} to the Active Directory (AD) that manages the SQL Server hosts.](#_bookmark22)

●      [Select accounts and grant permissions.](#_bookmark23)

●      [Install and set up the Backup agent for Windows.](#_bookmark32)



●      [Learn more about deploying for specific SQL Server features.](#_bookmark48)

●      [Learn about using SQL Native backups instead of agent-based backups.](#_bookmark53)

When you complete those tasks, see the [MS SQL](/docs/backup-recovery?group=ms-sql) section in the online Help to set up protection for your databases.

## Terminology
{: #terminology}

There are several concepts and terms that are important to understand as you learn about the Microsoft SQL Server Backup features in {{site.data.keyword.baas_full_notm}}.

Table 1: {{site.data.keyword.baas_full_notm}} Terminology

|     |     |
| --- | --- |
| **TERM** | **DEFINITION** |
| **View** | {{site.data.keyword.baas_full_notm}} serves data to clients and hosts from logical containers called Views. Views are exposed as SMB shares. Users can specify storage efficiency, QoS policy settings, and permissions at the View level. |
| **QoS Policy** | The Quality of Service (QoS) policy specified for a {{site.data.keyword.baas_full_notm}} View. By choosing a QoS policy for the appropriate priority and type of workload, you enable {{site.data.keyword.baas_full_notm}} to optimize how it processes data in regard to latency, throughput, and priority. For details, see [Create or Edit a View.](https://docs.cohesity.com/6_6/Web/UserGuide/Content/Dashboard/Platform/CreateaView651.htm) |
| **Protection Group** | A {{site.data.keyword.baas_full_notm}} Protection Group is a backup job that runs repeatedly, based on an associated policy, to back up data from a source and store it on the {{site.data.keyword.baas_full_notm}}. |
| **Protection Policy** | A collection of reusable settings that define how and when sources are protected, replicated and archived. You select a policy when configuring a Protection Group. |
| **Replication** | Replication automatically makes copies of Snapshots captured by a Protection Group on one {{site.data.keyword.baas_full_notm}} and copies them to a second {{site.data.keyword.baas_full_notm}}. A protection group specifies the SQL databases to be backed up and replicated. |
| **SQL VIP** | The IP address of the SQL Instance that moves from one physical node to another when a node fails. |
| **Log Shipping (Technique)** | The technique of providing a series of copies of transaction log backups from a primary database on a primary server instance to a secondary database on a separate secondary server instance. On the secondary database, transaction logs are applied independently and sequentially. |
| **MS SQL Server Log Shipping** | Microsoft’s implementation of the Log Shipping technique. Specifically, it uses a set of stored procedures and SQL jobs to carry out the process. |
{: caption="" caption-side="bottom"}



|     |     |
| --- | --- |
| **TERM** | **DEFINITION** |
| **DB Migration (Technique)** | The process of moving an MS SQL Server database from one SQL Server instance to a different SQL Server instance without data loss. This can be performed manually or in some automated form. |
{: caption="" caption-side="bottom"}

## Backup Strategy Considerations
{: #backup_strategy_considerations}

Data backup is an essential part of protecting the data, but it’s important to really understand what makes a backup strategy successful. It is important to have a second copy of data in case the original copy fails. But that is only part of the story. A good backup strategy is obviously going to create that second copy, but it is more crucial that, when recovery is needed, the data can actually be found and restored quickly.

You can use {{site.data.keyword.baas_full_notm}} for other types of backups and even create new backup strategies designed to better meet your company’s needs.

**{{site.data.keyword.baas_full_notm}} recommends** that you plan your SQL backups using the business’s Service Level Agreement (SLA) and disaster recovery (DR) requirements.

Table 2: Prerequisite Planning for Backups

|     |     |
| --- | --- |
| **PLAN** | **DETAILS** |
| **Current Methods** | Determine how backups are currently being done. Some common methods are: SQL Maintenance Plan, TSQL Scripting, and third-party tools.<br><br>**{{site.data.keyword.baas_full_notm}} recommends** all other backup methods be disabled. If multiple backup methods are being used concurrently, conflicts can occur. |
| **Strategy Planning** | Determine which type of SQL backups are needed for the business. Consider the customer’s SLA and review the business’s Disaster Recovery Plan for RTO and RPO requirements. These will largely define the backup strategy. |
| **Restore Planning** | **{{site.data.keyword.baas_full_notm}} recommends** a periodic restore of a database from its backup. This is an important step in the overall backup strategy because it tests, verifies, and validates the integrity of the backup. A sample set of backups should be restored to a non-production server and evaluated. This step also ensures that at a critical event, the method of restoring a database is valid. |
{: caption="" caption-side="bottom"}



|     |     |
| --- | --- |
| **PLAN** | **DETAILS** |
| **Protecting the Backups** | Determine a strategy for protecting all backups. The backups are very valuable and need to be protected from: corruption, deletion, overwriting, and catastrophic loss. Copies of the backups should be moved to an off-site location. Having off-site copies of the backups not only protects the backups, but is also the foundation for a Disaster Recovery Plan. Use replication to maintain off-site copies, in addition to the backups you archive for long-term retention.<br><br>**{{site.data.keyword.baas_full_notm}} recommends** a periodic restore of a database from its backup. This is an important step in the overall backup strategy because it tests, verifies, and validates the integrity of the backup. A sample set of backups should be restored to a non-production server and evaluated. This step also ensures that at a critical event, the method of restoring a database is valid. |
{: caption="" caption-side="bottom"}

**{{site.data.keyword.baas_full_notm}} recommends:** Use the Backup agent for backups, and disable all other backup jobs or methods, such as SQL Agent maintenance jobs, which would conflict with the backup process.



# Review Disaster Recovery (DR) Considerations
{: #review_disaster_recovery_dr_considerations}


**{{site.data.keyword.baas_full_notm}} recommends** safeguarding all of your backups by using replication and archival.

Taking a SQL backup is only one part of protecting a business. Protecting the business means protecting the data from corruption _and_ from catastrophic disaster. This is done by keeping a series of backups, and then in turn moving some of those backups off-site and archiving them under a long-term retention plan.

Protecting the business starts with:

·         **Capture and Store**: Protection from loss and corruption. Protects the databases with regularly scheduled backups.

·         **Geo-Protect**: Protection from catastrophic loss.

Protects the backups by replicating them to an off-site location.

·         **Cost-effective Archive**: Protects and holds the backups. Protects the business by archiving the backups to the cloud under a long-term retention plan. Stored on a lower-cost storage tier.

## Take Local Snapshots
{: #take_local_snapshots}

**{{site.data.keyword.baas_full_notm}} recommends** maintaining enough backups to meet your business requirements.

Protect from data corruption over time by maintaining a series of local snapshots. By taking and maintaining several snapshots, you are in position to recover data from its state prior to being corrupted. Snapshots are efficient because they use deduplication and compression.

## Replicate Backups Off-Site
{: #replicate_backups_off_site}

**{{site.data.keyword.baas_full_notm}} recommends** replicating backups to a second {{site.data.keyword.baas_full_notm}}.

Protect your entire set of SQL backups from catastrophic loss by replicating them to an off-site location. In a {{site.data.keyword.baas_full_notm}} protection group, you are able to automatically replicate the SQL backups stored in the {{site.data.keyword.baas_full_notm}} to a second off-site {{site.data.keyword.baas_full_notm}}. As part of replication, {{site.data.keyword.baas_full_notm}} always performs source-side compression and deduplication first and sends only the changed data over the network for cost-effective disaster recovery.

## Archive Backups to the Cloud
{: #archive_backups_to_the_cloud}

**{{site.data.keyword.baas_full_notm}} recommends** archiving to the cloud to take advantage of low-cost, long-term storage.

Archive some of the SQL backups to the cloud as a way to address long-term data retention requirements and lower the cost of storage. {{site.data.keyword.baas_full_notm}} provides a policy-based method to archive to public clouds (AWS, Azure, Google Cloud Platform) and any S3-compatible storage.



## Validate SQL Backups
{: #validate_sql_backups}

**{{site.data.keyword.baas_full_notm}} recommends** regularly validating backups by restoring or cloning a database. Further validation of the backup can be done by running a [DBCC CHECKDB](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-checkdb-transact-sql?view=sql-server-2017) (Check Database) command.

Successfully restoring a SQL backup is proof that your backup strategy works. {{site.data.keyword.baas_full_notm}} gives you the advantage of being able to browse and search across all your snapshots, and to restore to different locations on different servers.



# Check {{site.data.keyword.baas_full_notm}} Settings
{: #check_{{site.data.keyword.baas_full_notm}}_settings}

It is important to understand the processing settings in the {{site.data.keyword.baas_full_notm}} that is protecting your SQL databases. Choosing the optimal settings for deduplication, compression, and encryption can dramatically improve the performance of your {{site.data.keyword.baas_full_notm}} backups and archives.

## Deduplication
{: #deduplication}

**{{site.data.keyword.baas_full_notm}} recommends** leaving deduplication enabled and setting it to **Inline**, to reduce storage costs.

**Deduplication**: (Enabled by default.) When Deduplication is on, {{site.data.keyword.baas_full_notm}} eliminates duplicate blocks of repeating data stored on the {{site.data.keyword.baas_full_notm}}, dramatically reducing the amount of storage space needed to store the data.

**Inline Deduplication**: Optionally toggle on. When Inline Deduplication is on, the deduplication occurs as the {{site.data.keyword.baas_full_notm}} is saving the blocks to the partition. (There can also be additional deduplication after the {{site.data.keyword.baas_full_notm}} has saved the blocks to the partition.) When Inline Deduplication is off, deduplication occurs _after_ the {{site.data.keyword.baas_full_notm}} has written the data to the partition and might ultimately be slower.

## Compression
{: #compression}

**{{site.data.keyword.baas_full_notm}} recommends** that you disable SQL Server-side compression and enable Inline Compression in the {{site.data.keyword.baas_full_notm}} platform. This also takes advantage of {{site.data.keyword.baas_full_notm}} deduplication, by allowing it to identify block patterns in the data.

**Compression**: (Enabled by default.) When Compression is on, the {{site.data.keyword.baas_full_notm}} compresses all the data stored in the Storage Domain, reducing the amount of space needed to store the data.

**Inline Compression**: (Enabled by default.) If Inline Compression is on, the compression occurs as the {{site.data.keyword.baas_full_notm}} is saving the blocks to the partition. If Inline Compression is off, the compression occurs _after_ the {{site.data.keyword.baas_full_notm}} has written the data to the partition and might ultimately be slower.

Without SQL Server-side compression, the first backup might appear slower, but the benefits of deduplication will kick in starting with the subsequent backup, and the reduction in the volume of data outweighs any performance drop experienced while taking the first backup.
{: NOTE}


## Encryption
{: #encryption}

**{{site.data.keyword.baas_full_notm}} recommends** that data encryption be enabled for any data being sent to an External Target, to the public cloud, or any time sensitive data is being handled.

### Data-at-Rest Security
{: #data_at_rest_security}

The {{site.data.keyword.baas_full_notm}} [SpanFS](https://www.cohesity.com/products/spanfs/)® file system provides full at-rest encryption based on the strong AES-256 CBC (Cipher Block Chaining) standard.



### Data-in-Flight Security
{: #data_in_flight_security}

The data being transmitted from a {{site.data.keyword.baas_full_notm}} to External Targets is encrypted for security. Examples of data movement include: replicating between remote offices; and transmitting data to public cloud.

### Data-in-Cloud Security
{: #data_in_cloud_security}

{{site.data.keyword.baas_full_notm}}’s CloudArchive includes encryption. This functionality moves data from an on-premises cluster to cloud storage. For cold data, which is rarely accessed but must be retained, this is a far more economical solution. While encryption for CloudArchive operations can be turned off, {{site.data.keyword.baas_full_notm}} strongly recommends keeping it on, especially when working with External Targets in the public cloud.

For more see [{{site.data.keyword.baas_full_notm}} Security Features](/docs/backup-recovery?group=managing-encryption) in the online Help.



# Fine-Tune MS SQL Server Parameters
{: #fine_tune_ms_sql_server_parameters}

|     |     |
| --- | --- |
|     |
|     | ![Cohesity recommends that MS SQL Server hosts meet all Microsoft best practices.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image004.gif) |
{: caption="" caption-side="bottom"}



To ensure that your backups run with the highest possible performance, review and optimize each component in the backup process, starting with the SQL Server host:

·         MS SQL Server

·         Network

·         The {{site.data.keyword.baas_full_notm}} platform

Gauging MS SQL Server Host performance (Physical or VM) is important, as a SQL Server that is underpowered, equipped with poor resources, or overloaded will adversely affect any backup process.

## Host CPU
{: #host_cpu}

**{{site.data.keyword.baas_full_notm}} recommends** that MS SQL Server hosts meet Microsoft SQL Server CPU recommendations and best practices. For a good reference on this, see [Storage and SQL Server capacity planning and](https://docs.microsoft.com/en-us/sharepoint/administration/storage-and-sql-server-capacity-planning-and-configuration) [configuration.](https://docs.microsoft.com/en-us/sharepoint/administration/storage-and-sql-server-capacity-planning-and-configuration)

For example, a SQL Server host (physical or VM) can have an average CPU usage between 1%-15%, with swells of 16%-30% and spikes of 65%-85% during high transaction volumes.

MS SQL Server consumes CPU resources equally across all processors on the host. It derives its transactional power from CPU cycles. If not managed, the operating system and MS SQL Server will compete for CPU cycles.

High CPU usage means fewer cycles to spend on taking SQL backups, leading to longer waits to release worker threads and resolve transactional commits to the database.

## Host Memory
{: #host_memory}

**{{site.data.keyword.baas_full_notm}} recommends** that MS SQL Server hosts meet Microsoft SQL Server memory recommendations and best practices. For a good reference on this, see [Storage and SQL Server](https://docs.microsoft.com/en-us/sharepoint/administration/storage-and-sql-server-capacity-planning-and-configuration) [capacity planning and configuration.](https://docs.microsoft.com/en-us/sharepoint/administration/storage-and-sql-server-capacity-planning-and-configuration)

MS SQL Server assumes that host memory exists solely for its own purpose. If not managed, the operating system and MS SQL Server will compete for host memory. For example, when a production Windows host will have 16GB-64GB of memory.

MS SQL Server aggressively attempts to push all database objects into memory — tables, indexes, stored code, and procedure caches — into memory, it has a large impact on total available memory. In turn, inefficient memory usage indirectly degrades backup generation.

A well provisioned production Windows Server host has a lot of memory. Approximately 6-10 GB is allocated for the operating system, and the rest is reserved for MS SQL Server. A properly configured MS SQL Server uses all available host memory. MS SQL Server automatically moves the most frequently used objects into memory. If memory pressure is too great, the OS will start writing to the system pagefile and this will have a significant impact on performance.



## Host Windows Boot Drive (C:\\)
{: #host_windows_boot_drive_(C:\\)}

**{{site.data.keyword.baas_full_notm}} recommends** that the C:\\ drive be provisioned with 15%-20% free space to accommodate cloning and instant mounts.

The boot drive on a Windows Server host (physical or VM) is usually the C:\\ drive. The amount of free space on the C:\\ drive must be sized properly for database cloning.

**Microsoft requires** that the amount of free space on the host C:\\ drive to be equal to or greater than 15%

\- 20% of the size of the largest cloned database. This is a Microsoft limitation as cloning uses Microsoft Vss (Volume Shadow copy Service) and requires space on the C:\\ drive for the mount.

## SQL Server Database settings
{: #sql_server_database_settings}

**{{site.data.keyword.baas_full_notm}} recommends** the SQL database RECOVERY\_MODEL property be set to FULL.

The Full Recovery Model requires log backups to be taken on the database to offset the growth of the database log file. Taking log backups allows recovery of the database to any specific point in time.

|     |     |
| --- | --- |
|     |
|     | [![NOTE: With the Simple Recovery Model, the database log file remains at one size, and recovery of the database is only available for the last backup point in time. The changes that occur after the most recent backups are left unprotected. {{site.data.keyword.baas_full_notm}} recommends against using the Simple Recovery Model in a production environment.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image005.gif)](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/recovery-models-sql-server?view=sql-server-2017) |
{: caption="" caption-side="bottom"}





# Link Active Directory
{: #link_active_directory}

Active Directory is a common repository for information about objects that reside on the network, such as users, groups, computers, printers, applications, and files. Active Directory also maintains access on each object, which allows you to maintain permissions and control who can access and manage those objects.1

**{{site.data.keyword.baas_full_notm}} recommends** the {{site.data.keyword.baas_full_notm}} be joined to the same Active Directory that manages the customer’s SQL Server hosts.

**{{site.data.keyword.baas_full_notm}} recommends** the service accounts for MS SQL Server and SQL Agent be configured in Active Directory and have domain user permissions.

|     |     |
| --- | --- |
|     |
|     | [![IMPORTANT: To successfully link a cluster to an AD server, the domain must be resolvable, and the DNS server must be reachable. For more information on joining a cluster to Active Directory, see Active Directory in the online Help.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image006.gif)](https://docs.cohesity.com/6_6/Web/UserGuide/Content/Dashboard/Admin/ActiveDirectory.htm) |
{: caption="" caption-side="bottom"}



|     |     |
| --- | --- |
|     |
|     | ![](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image007.gif) |



1 Desmond, Brian, et al. (2013). _Active Directory: Designing, Deploying, and Running Active Directory_. (5th ed., p. 1). O'Reilly.



# Select Accounts and Grant Permissions
{: #select_accounts_and_grant_permissions}

There are three accounts you must consider when installing the Backup agent:

·         **Installation Account**: The account you use to log in to the host and run the installer.

·         **Service Account**: The account under which the Backup agent service runs on the SQL Server host. (Selected at the time of installation.)

·         **SQL Server login account**: The SQL Server account by which the Backup agent has access to the databases. (Configured after installation.)

## Installation Account
{: #installation_account}

When installing the Backup agent on a Microsoft Windows host, you must log onto the SQL Server host with an account that has local administrator privileges. This is because the Windows agent installer installs the Backup agent service.

**Microsoft requires** the installation account have administrator privileges on the host.

## Choose the Service Account
{: #choose_the_service_account}


The installer gives you the option between two types of service accounts:

·         The LocalSystem account

·         A user account

Assigning privileges to the service account will differ depending on which type of account you choose. **Cohesity recommends** using either the LocalSystem account or an Active Directory Domain User account. See the chart below for a comparison of service account types.

Table 3: Comparison of Service Account Types

|     |     |     |     |
| --- | --- | --- | --- |
| **SERVICE ACCOUNT** | **TYPE** | **DEFINITION** | **RECOMMENDATION** |
| **LocalSystem (Default)** | **LocalSystem Account**<br><br>**(Built-in Host Account)** | The LocalSystem account is a predefined local account on the host computer. | **Recommended**:<br><br>The LocalSystem account is a predefined local account used by the service control manager. It has extensive privileges on the local computer, and acts as the computer on the network. Its token includes the NT AUTHORITY\\SYSTEM and<br><br>BUILTIN\\Administrators SIDs; these accounts have access to most system objects.2 |
{: caption="" caption-side="bottom"}

|     |     |
| --- | --- |
|     |
|     | ![](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image007.gif) |
{: caption="" caption-side="bottom"}



2 For more, see Microsoft’s [LocalSystem Account](https://docs.microsoft.com/en-us/windows/desktop/services/localsystem-account) article.



|     |     |     |     |
| --- | --- | --- | --- |
| **SERVICE ACCOUNT** | **TYPE** | **DEFINITION** | **RECOMMENDATION** |
| **User** **Account** | **Domain User Account**<br><br>**(Active Directory)** | A user whose username and password are stored in Active Directory. | **Recommended**:<br><br>A domain user account enables the service to take full advantage of the service security features of Windows and Microsoft Active Directory Domain Services. The service has whatever local and network access is granted to the account, or to any groups of which the account is a member.3 |
| **Local User Account**<br><br>**(Host account)** | In Windows, a local user is one whose username and encrypted password are stored locally, only on the computer itself. | **Not recommended**:<br><br>When you log in as a local user, the computer checks its own list of users and its own password file to see if you are allowed to log in to the computer. The computer itself then applies all the permissions and restrictions that are assigned to you for that computer. |
{: caption="" caption-side="bottom"}

|     |     |
| --- | --- |
|     |
|     | ![TIP: If you are unsure which account to use, choose the LocalSystem account. This account already has most of the required permissions. The agent service account can be changed later if needed.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image008.gif) |
{: caption="" caption-side="bottom"}



### Pros and Cons of Service Account Types
{: #pros_and_cons_of_service_account_types}

Different types of service accounts have advantages and disadvantages that you should be aware of when deploying the Backup agent. See the comparison chart below.

|     |     |
| --- | --- |
|     |
|     | ![](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image007.gif) |



3 For more, see Microsoft’s [Using a Domain User Account as a Service Logon](https://docs.microsoft.com/en-us/windows/desktop/ad/domain-user-accounts) article.



Table 4: Service Account Types

|     |     |     |     |
| --- | --- | --- | --- |
| **INSTALLER ACCOUNT CHOICE** | **ACCOUNT** **TYPE** | **PROS** | **CONS** |
| **LocalSystem (Default)** | **LocalSystem Account**<br><br>**(Built-in Host Account)** | The LocalSystem account is a predefined local account on the host computer and therefore requires the minimum setup and configuration.<br><br>The LocalSystem account already exists on Windows machines.<br><br>Has access to most system objects. | The difficulty of managing each individual LocalSystem account increases as the number of machines increase. For example, 200 machines \= 200 LocalSystem accounts to manage.<br><br>No central location to manage security.<br><br>No central location to manage permissions.<br><br>Additional permissions need to be managed in order to access network computers and storage. |
| **User** **Account** | **Domain User Account**<br><br>**(Active Directory)** | Account username and password are stored in Active Directory.<br><br>Can take full advantage of the service security features of Windows and Microsoft Active Directory Domain Services.<br><br>Centralized location to manage security.<br><br>Centralized location to manage permissions.<br><br>Access to network computers and storage is easily available.<br><br>Very scalable. | This account must be created before agent installation. |
| **Local User Account**<br><br>**(Host account)** | Good for a single system. | Not scalable in a production environment. |



### Assign Service Account Permissions
{: #assign_service_account_permissions}

The agent service account needs permissions (authentication) from several different areas:

·         MS SQL Server Instance

·         Windows SQL Server host

·         MS Active Directory (if applicable)

·         As usage of your {{site.data.keyword.baas_full_notm}} increases, it will need permissions to network entities such as users, groups, applications, computers, storage, shares, files, and other {{site.data.keyword.baas_full_notm}}s.

Table 5: Service Account Permissions Minimums

|     |     |     |     |
| --- | --- | --- | --- |
| **AGENT SERVICE ACCOUNT TYPE** | **AGENT SERVICE ACCOUNT PERMISSIONS** |     |     |
| **IN ACTIVE DIRECTORY** | **IN SQL SERVER** | **ON WINDOWS HOST** |
| **LocalSystem** | N/A | “NTAuthority\\System” login must also have sysadmin role. | Must also be a member of the Administrators group.<br><br>Must have rights to “log on as a service.” |
| **User Account (Active Directory)** | Must also be a member of the Domain Users group.<br><br>Must also be a member of the Administrators group. | Login must also have sysadmin role.4 |
{: caption="" caption-side="bottom"}

|     |     |
| --- | --- |
|     |
|     | ![NOTE: A common deployment error when using the LocalSystem account is to assume that the LocalSystem account belongs to the Administrators group on the host server. This is not always the case. Verify that the account “LocalSystem” belongs to the Administrators group.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image009.gif) |
{: caption="" caption-side="bottom"}



For more, see the [MS SQL Setup](/docs/backup-recovery?group=ms-sql) section in the online Help, and Microsoft’s [About Service Logon](https://docs.microsoft.com/en-us/windows/desktop/ad/about-service-logon-accounts) [Accounts](https://docs.microsoft.com/en-us/windows/desktop/ad/about-service-logon-accounts) article.

|     |     |
| --- | --- |
|     |
|     | ![NOTE: A common deployment error is to forget to assign the SQL Server sysadmin role to the agent service account in SQL Server.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image010.gif) |
{: caption="" caption-side="bottom"}



## Get the Synergy of SQL Server VSS Writer
{: #get_the_synergy_of_sql_server_vss_writer}

The SQL Server Writer account is a login that comes with the SQL Server Instance by default. When a volume-based Protection Group requests a VSS snapshot of a Windows machine, the SQL Server Writer account steps in and launches the Windows VSS Service to execute the request.

|     |     |
| --- | --- |
|     |
|     | ![](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image007.gif) |
{: caption="" caption-side="bottom"}



4 Backup or restore operations that use the Microsoft SQL Server Virtual Device Interface (VDI) require that the server connection for SQL Server that is used to issue the BACKUP or RESTORE commands must be logged on as the sysadmin fixed server role.



The SQL Server Writer service runs under the LOCAL SYSTEM account which has all the required permissions for it to function without interruptions. It is also provisioned as a Database Engine login by default. Note that this login comes out of the box with a SQL Server sysadmin fixed server role, and we recommend you retain this configuration to ensure the smooth functioning of the login.

For more information, see [Configure Windows Service Accounts and Permissions](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-ver15) in the Microsoft SQL documentation.



# Install the Backup agent for Windows
{: #install_the_backup_agent_for_windows}

**{{site.data.keyword.baas_full_notm}} recommends** using agent-based backups as the preferred method. This allows you to have a single protection group that protects multiple SQL databases across multiple SQL instances, and the ability to back up and restore individual databases.

|     |     |
| --- | --- |
|     |
|     | ![NOTE: To recommend the best possible deployment, you have to know how the SQL instance is configured. Not understanding the SQL Server configuration can lead to errors when you register it as a Cohesity.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image011.gif) |
{: caption="" caption-side="bottom"}



**{{site.data.keyword.baas_full_notm}} recommends** that you know the SQL Server host configuration in order to properly register it as a {{site.data.keyword.baas_full_notm}} source.

Before registering a physical Server with the {{site.data.keyword.baas_full_notm}}, you’ll have to download and install the Backup agent for Windows on each Server you plan to register. When you install the Backup agent, you will be prompted to choose file-based or volume-based options, or both. See the next section.

## Choose Agent Methods
{: #choose_agent_methods}

{{site.data.keyword.baas_full_notm}} provides three different agent-based methods to back up and restore your database.

**{{site.data.keyword.baas_full_notm}} recommends** installing both the file- and volume-based CBT (Changed Block Tracker) persistence options.

Table 6: Agent Methods for Backup and Restore

|     |     |     |     |
| --- | --- | --- | --- |
| **AGENT METHODS FOR BACKUP** |     |     |     |
| **METHOD** | **REBOOT REQUIRED?** | **CBT PERSISTENCE****5** | **WHAT IS PROTECTED?** |
| **File-based** | No — does not require a host reboot. | CBT does _not_ persist through a reboot. (A full backup will be taken on the next job run). | DB files: .mdf, .ndf, .ldf |
|     |     |     | The whole volume is protected: .vhd file |
|     |     |     | {{site.data.keyword.baas_full_notm}} will back up |
| **Volume-****based** | Yes — requires a host reboot. | CBT persists through a reboot. | both the database files as well as any other<br><br>files on that given |
|     |     |     | volume. Only volumes |
|     |     |     | where the databases |
|     |     |     | exist are backed up. |
{: caption="" caption-side="bottom"}

|     |     |
| --- | --- |
|     |
|     | ![](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image007.gif) |
{: caption="" caption-side="bottom"}



5 CBT (Changed Block Tracking) persistence is a term used to describe whether or not CBT resumes tracking changes where it left off after a reboot of the server, or if CBT must start new tracking.



|     |     |     |     |
| --- | --- | --- | --- |
| **AGENT METHODS FOR BACKUP** |     |     |     |
| **METHOD** | **REBOOT REQUIRED?** | **CBT PERSISTENCE****5** | **WHAT IS PROTECTED?** |
| **VDI-based**<br><br>**(Virtual Device Interface)** | No — does not require a host reboot. | N/A | DB files: .mdf, .ndf, .ldf |
{: caption="" caption-side="bottom"}

|     |     |
| --- | --- |
|     |
|     | ![NOTE: SQL Filestream databases can only be backed up using the volume-based backup method.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image012.gif) |
{: caption="" caption-side="bottom"}



### Consider File-Based Backups
{: #consider_file_based_backups}

A file-based backup protects only the MS SQL databases you choose. It captures only the database files for those selected databases. This approach contrasts with a volume-based backup, which captures any and all the files contained on the volume.

The advantage of a file-based backup is that a smaller amount of data is captured, which allows the protection group to run more quickly.

To enable file-based backup, install the File System CBT component during Backup agent installation. For more, see [Protect MS SQL Server (File-based)](/docs/backup-recovery?group=protect-a-ms-sql-server) in the online Help.

### Consider Volume-Based Backups
{: #consider_volume_based_backups}

A {{site.data.keyword.baas_full_notm}} MS SQL volume-based backup captures all MS SQL databases running on a VMware or physical Server, as well as all other files that are stored on the corresponding OS volume. This approach contrasts with a file-based MS SQL backup, which includes only the selected databases. The {{site.data.keyword.baas_full_notm}} captures full database server backups and can optionally back up database transaction logs, so you can roll forward to any point in time.

For more, see [Protect MS SQL Server (Volume-based)](/docs/backup-recovery?group=protect-a-ms-sql-server) in the online Help.

### Consider VDI-Based Backups
{: #consider_vdi_based_backups}

The Virtual Device Interface (VDI) is a Microsoft interface that allows the Backup agent to execute SQL Server Native backup and restore commands. This backup type produces a standard SQL Server Native backup of type (.BAK), transaction logs, (.TRN), and differentials, (.DIFF). These backup files are stored on a {{site.data.keyword.baas_full_notm}} View and take advantage of View compression, deduplication, encryption, replication, and archiving features.

The aim of the SQL DBA is to have a combination of backups so that the database can be _restored_ to any point in time. A good combination of backups consists of having a full backup, differential (in {{site.data.keyword.baas_full_notm}}, _incremental_) backups, and log backups.

For example, all SQL database restores must begin with a full database backup, secondly, a differential can then be applied to the full, and finally, log backups can be applied in sequence to complete the database restore.



When the backups are applied during the database restore process, you are sequentially adding the captured changes to the database: FULL+DIFF+Log1+Log2+Log3 = Restored Database.

|     |     |
| --- | --- |
|     |
|     | ![IMPORTANT: All Microsoft VDI backups, their differentials, and their logs are dependent on a full backup to perform a database restore. Microsoft requires that in order to restore a SQL database, you must start with a full backup, then its transaction logs can be applied. This means your backup retention policy must keep a full backup along with its log backups in order to successfully restore a database.
<br>Simply put, a SQL VDI-based database restore requires a full backup to seed the database, then a differential and/or log backups are applied to the specified point in time.
<br>We recommend retaining two sets of full backups with their differential and log backups.
<br>,TIP: A good backup plan always includes a combination of full, differential, and log backups.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image013.gif) |




### Compare Volume, File, and VDI-Based Methods
{: #Compare_volume_file_and_vdi_based_methods}

Use the table below to compare the implications of each method.

Table 7: Comparison of Volume, File, and VDI backup Methods

|     |     |     |     |
| --- | --- | --- | --- |
| **{{site.data.keyword.baas_full_notm}} FACET** | **BACKUP METHOD** |     |     |
| **VOLUME** | **FILE** | **VDI** |
| **Technology Employed** | Backup agent uses Vss, for volume CBT, and VDI for T-logs. | Backup agent uses Vss and {{site.data.keyword.baas_full_notm}} technology for file CBT, and VDI for T-logs. | Backup agent uses SQL Native backup commands. |
| **What does it capture?** | Captures all the volumes attached to the server _and everything on those volumes_.6 | Captures database files. | Captures the database in a SQL Server standard .BAK, .TRN, or .Diff file |
| **Logs** | Yes. Captures a SQL log backup of the database. | Yes. Captures a SQL log backup of the database. | Yes. Captures a SQL Server native log backup of the database. |
| **{{site.data.keyword.baas_full_notm}} Source Registration** | Register as a SQL Server application. | Register as a SQL Server application. | Register as a SQL Server application. |
{: caption="" caption-side="bottom"}

|     |     |
| --- | --- |
|     |
|     | ![](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image007.gif) |
{: caption="" caption-side="bottom"}



6 Often, there are other files, like old .bak files, that sit on the volume along with the active database files. A large amount of other files will increase the duration of the protection group because these files will be captured along with the database files.



|     |     |     |     |
| --- | --- | --- | --- |
| **{{site.data.keyword.baas_full_notm}} FACET** | **BACKUP METHOD** |     |     |
| **VOLUME** | **FILE** | **VDI** |
| **Compatibility with SQL Server High Availability Features** | **Windows Failover Cluster: Yes**, backup will succeed if a failover occurs. Must be registered as a {{site.data.keyword.baas_full_notm}} "SQL Cluster" source.<br><br>**AAG: Yes**, backup on the correct replica will succeed after an AAG failover occurs.<br><br>**Mirroring: Yes**\*. Each SQL instance must have its own protection group.<br><br>**Log Shipping: Yes**\*. Each SQL instance must have its own protection group.<br><br>**\* NOTE**: While {{site.data.keyword.baas_full_notm}} currently does not include specific features for mirroring and log shipping, it does not conflict with these SQL Server features. |     | **Windows Failover Cluster: Yes**, backup will succeed if a failover occurs. Must be registered as a {{site.data.keyword.baas_full_notm}} "SQL Cluster" source.<br><br>**AAG: Yes**, backup on the correct replica will succeed after an AAG failover occurs.<br><br>Mirroring: No. Log Shipping: No. |
| **Method** | This is an MS SQL Server database backup at the **volume level**. Allows for FULL SQL Server backups — all databases, logs, and files/folders on the volumes are backed up. MS SQL Server is registered as a {{site.data.keyword.baas_full_notm}} source and MS SQL application.<br><br>The Backup agent uses Vss, the {{site.data.keyword.baas_full_notm}} Volume CBT, and VDI to take an application- consistent snapshot of the volumes on that server. | This is an MS SQL Server database backup at the **file level**. Allows for FULL SQL Server backups with logs, but only the database files are captured. The Backup agent uses Vss, the {{site.data.keyword.baas_full_notm}} File CBT, and VDI to take an application-consistent snapshot of the volumes on that server. Faster because it's not backing up extraneous files.<br><br>**NOTE**: Cannot browse files or folders as in a volume-based backup. | This is a native MS SQL Server database backup at the **file level**. Allows for FULL SQL Server backups with logs and Differentials, but only the database is captured.<br><br>Agent uses SQL Server native backup and restore commands , to take an application- consistent backup of the database. |
| **Recovery/Restore** | The database host server should be backed up and restored via a different protection group.<br><br>Supports Point-in-Time (PIT) restore of the database from across the entire backup history. |     |     |
{: caption="" caption-side="bottom"}



|     |     |     |     |
| --- | --- | --- | --- |
| **{{site.data.keyword.baas_full_notm}} FACET** | **BACKUP METHOD** |     |     |
| **VOLUME** | **FILE** | **VDI** |
| **Cloning** | **Yes**.<br><br>Can produce a representative volume.<br><br>_Advantage_: Applications can be tested with data that is as close to the production environment as possible.<br><br>Can reproduce a volume.<br><br>_Advantage_: Applications can be tested in an environment that can be destroyed and re- created on demand.<br><br>No load on Production servers.<br><br>The backup is NOT modified and therefore always safe.<br><br>Uses storage resources on {{site.data.keyword.baas_full_notm}}. | **Yes**.<br><br>Can produce a representative database.<br><br>_Advantage_: Applications can be tested with data that is as close to the production environment as possible:<br><br>Can reproduce a database.<br><br>_Advantage_: Applications can be tested with databases that can be destroyed and re-created on demand.<br><br>No load on Production servers.<br><br>The backup is NOT modified and therefore always safe.<br><br>A successful clone is immediate validation of the backup's integrity.<br><br>Uses storage resources on {{site.data.keyword.baas_full_notm}}. | **Cloning is scheduled as a future enhancement.** |
| **Archival** | **CloudArchive:** IBM Cloud Supports the ability to send backups to public cloud providers like AWS, Azure, and Google Cloud Platform. It also supports search and restore from them, back to the local site when needed. |     |     |
| **Replication** | **Managing Copies of backups on another {{site.data.keyword.baas_full_notm}}:** Backups, files, and volumes can be replicated to a second {{site.data.keyword.baas_full_notm}}. |     |     |
{: caption="" caption-side="bottom"}



## Install and Manage the Backup agent for Windows
{: #install_and_manage_the_backup_agent_for_windows}

You are now ready to install the Backup agent for Windows on each SQL Server you plan to back up with {{site.data.keyword.baas_full_notm}}. From the SQL Server host, log in to the {{site.data.keyword.baas_full_notm}} platform, navigate to **Protection > Sources**, and download the Backup agent Windows. Install the Backup agent by launching the downloaded executable. The agent starts automatically.

You can install the Backup agent for Windows on multiple Windows servers through the [Cohesity](https://docs.cohesity.com/6_6/Web/UserGuide/Content/CLI/ConfigureWithCLI.htm) [DataPlatform CLI](https://docs.cohesity.com/6_6/Web/UserGuide/Content/CLI/ConfigureWithCLI.htm) (command line interface). For complete instructions on installing the Backup agent, see [Install and Manage the Agent on Windows Servers](https://docs.cohesity.com/6_6/Web/UserGuide/Content/Dashboard/Protection/AgentWindows.htm) in the online Help.

|     |     |
| --- | --- |
|     |
|     | [![NOTE: You can upgrade the Backup agent through the MS SQL Dashboard in {{site.data.keyword.baas_full_notm}}. When an upgrade becomes available for any agents that you’ve installed, an Upgrade Agent icon will appear next to that agent in the MS SQL Dashboard.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image014.gif)](https://docs.cohesity.com/6_6/Web/UserGuide/Content/MSSQL/SQLDashboard.htm) |
{: caption="" caption-side="bottom"}



## Set Up the Backup agent
{: #set_up_the_backup agent}

By default, the Volume CBT (Changed Block Tracker) component is selected for installation. This component is required to perform incremental backups and requires a reboot to function.

Without a reboot, only volume based full backups can be performed on the server.

The File System CBT (Changed Block Tracker) component is required for MS SQL file-based protection, as well as incremental backups of database files. The installation does not require a reboot for this component to function.

**{{site.data.keyword.baas_full_notm}} recommends** installing both the Volume and File CBT components of the Backup agent.

Figure 1: Backup agent Installation Options

|     |     |
| --- | --- |
|     |
|     | ![](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image015.gif) |
{: caption="" caption-side="bottom"}





# Know SQL Database Types and Source Registration
{: #know_sql_database_types_and_source_registration}

{{site.data.keyword.baas_full_notm}} handles different types of MS SQL Server Databases, but not all databases are registered the same way.

When it comes to SQL protection, you need to handle the variety of server database types associated with SQL Servers. {{site.data.keyword.baas_full_notm}} DataProtect helps you overcome this challenge with its native capability to handle these databases, each of them in their recommended, specifically optimized ways.

## Identify the Type of Database
{: #identify_the_type_of_database}

Before proceeding to deploy the Backup agent for SQL data protection, we recommend you correctly identify the type of SQL database you want to protect.

Below is a table of SQL database types that are supported by the {{site.data.keyword.baas_full_notm}} SQL agent, and their recommended protection methods.

Table 8: MS SQL Server Database Types

|     |     |     |     |     |
| --- | --- | --- | --- | --- |
| **DB TYPES** | **DEFINITION** | **PROTECTION METHOD** |     |     |
| **VOLUME- BASED** | **FILE- BASED** | **VDI-** **BASED** |
| **Stand Alone** | A SQL Server User database | ✔   | ✔   | ✔   |
| **AG** | A database that belongs to an availability group (AG). For each availability database, the availability group maintains a single read-write copy (the primary replica) and one to eight read- only copies (secondary replicas).<br><br>For details, see [Always On availability groups: a](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-ver15) [high-availability and disaster-recovery solution](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-ver15) in the Microsoft documentation. | ✔   | ✔   | ✔   |
| **FCI** | A database that belongs to a Failover Cluster Instance (FCI), which is a single instance of SQL Server that is installed across Windows Server Failover Clustering (WSFC) nodes. | ✔   | ✔   | ✔   |
| **CDC** | A change data capture (CDC) enabled SQL database records activity and is applied to an internal table. This makes the details of the changes available in an easily consumed relational format. For details, see [About Change](https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/about-change-data-capture-sql-server?view=sql-server-ver15) [Data Capture (SQL Server)](https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/about-change-data-capture-sql-server?view=sql-server-ver15) in the Microsoft documentation. | ✔   | ✔   | ✔   |
{: caption="" caption-side="bottom"}



|     |     |     |     |     |
| --- | --- | --- | --- | --- |
| **DB TYPES** | **DEFINITION** | **PROTECTION METHOD** |     |     |
| **VOLUME- BASED** | **FILE- BASED** | **VDI-** **BASED** |
| **TDE** | A Transparent Data Encryption (TDE) enabled SQL database has its data files encrypted, known as encrypting data at rest. TDE performs real-time I/O encryption and decryption of the data and log files. For details, see [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-ver15) [(TDE)](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-ver15) in the Microsoft documentation.<br><br>**NOTE**: {{site.data.keyword.baas_full_notm}} does not manage the keys associated with the TDE database. |     | ✔   | ✔   |
| **File** **Stream** | A Filestream enabled SQL Database is a database which contains a table that stores its data outside the database on the host file system. SQL Filestream leverages the OS file system’s ability to handle large objects ®BLOBS© like image ¨PDFs¨ Binary ¨Bmp¨ Docs¨ and other unstructured data. For details, see [FILESTREAM](https://docs.microsoft.com/en-us/sql/relational-databases/blob/filestream-sql-server?view=sql-server-ver15) [(SQL Server)](https://docs.microsoft.com/en-us/sql/relational-databases/blob/filestream-sql-server?view=sql-server-ver15) in the Microsoft documentation. | ✔   |     | ✔   |
| **SQL**<br><br>**System** | SQL Server maintains a set of four system-level databases (_master, model, msdb, tempdb_), which are essential for the operation of a server instance. System databases must be backed up after every significant update. The system databases that you must always back up include msdb, master, and model.<br><br>For details, see [Backup & restore: system](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server?view=sql-server-ver15) [databases (SQL Server)](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server?view=sql-server-ver15) in the Microsoft documentation. | ✔   | ✔   | ✔   |
{: caption="" caption-side="bottom"}

Now that you can Identify the type of SQL database you want to protect, you can properly register it as a {{site.data.keyword.baas_full_notm}} source.



## Register the Database Properly
{: #register_the_database_properly}

Registering the database appropriately ensures that you are able to make the most of the Protection Group.

Table 9: Database Types by Source Registration

|     |     |     |
| --- | --- | --- |
| **DATABASE TYPES** | **REGISTER AS** | **NOTES** |
| **Stand Alone** | Physical | All {{site.data.keyword.baas_full_notm}} foundational backup and restore features are available. |
| **AG** | All nodes as Physical | All {{site.data.keyword.baas_full_notm}} foundational backup and restore features are available. Protection Group will follow the db through a failover. |
| **FCI** | SQL Cluster | All {{site.data.keyword.baas_full_notm}} foundational backup and restore features are available. Backup Group will follow the db through a failover. Use the SQL Instance VIP. |
| **CDC** | Physical | All {{site.data.keyword.baas_full_notm}} foundational backup and restore features are available. |
| **TDE** | Physical | All {{site.data.keyword.baas_full_notm}} foundational backup and restore features are available. The user must manually handle the certificate. |
| **Filestream** | Physical | Volume-based backup and restore. |
| **SQL System** | Physical | All {{site.data.keyword.baas_full_notm}} foundational backup and restore features are available. Because of the special nature of these databases some {{site.data.keyword.baas_full_notm}} gflags need to be set. Please contact support for assistance. |
{: caption="" caption-side="bottom"}

Once you get your SQL database registered as a source in the {{site.data.keyword.baas_full_notm}} platform, you can immediately start to take full advantage of {{site.data.keyword.baas_full_notm}}’s data protection features.



# Deploy for Specific SQL Server Features
{: #deploy_for_specific_sql_server_features}

There is a direct connection between how you deploy the Backup agent and how you register SQL Server as a source. This relationship determines which types of backups can be taken.

Deploying the Backup agent for the right SQL configuration makes backups and restores more efficient. The most common SQL Server configurations are:

·         Standalone Server (Physical or VM)

·         MS SQL Server Always On Availability Groups (AAG)

·         SQL Cluster, aka Windows Failover Cluster Instance (FCI)

·         SQL VM

See the following sections for important notes and considerations on setting up for each configuration.

## Set Up for a Standalone Server (Physical or VM)
{: #set_up_for_a_standalone_server_physical_or_vm}

IBM Cloud Supports registering MS SQL either as a physical or as a virtual server but not both. Before registering an MS SQL Server, you must download and install the agent on the host server.

## Arrange for MS SQL Server Always On Availability Groups (AAG)
{: #arrange_server_always_on_availability_groups_aag}

The Microsoft SQL Always On Availability Groups feature is a high-availability and disaster-recovery solution. Always On Availability Groups maximizes the availability of a set of user databases.

An availability group supports a discrete set of user databases, known as availability databases, each with its own transactionally consistent replica. Because of the transactional consistency secondary Replicas can be used to generate backups or can be used for reporting.

An availability group supports a set of read-write primary databases and one to four sets of corresponding secondary databases7. For more information, read [Microsoft Always On Availability Groups (SQL Server)](https://docs.microsoft.com/en-us/previous-versions/sql/sql-server-2012/hh510230(v%3Dsql.110))

The benefit of MS SQL Server Always On Availability Groups is that it ensures transactional integrity across all its replicas. AAG databases can failover from Primary to one of the Replicas and back again so that any one of the replicas can function as the Primary database.

{{site.data.keyword.baas_full_notm}} will take a backup using the assigned backup preference even after a failover.

**{{site.data.keyword.baas_full_notm}} recommends** you register each node in the AAG relationship as a Physical Server.

For more on registering a SQL AAG configuration and applying a protection group, see [Cohesity](http://techdocs.cohesity.com/TechGuides/PDFs/Cohesity-Solution-Guide-Microsoft-SQL-AAG.pdf) [DataPlatform with MS SQL Server Always On Availability Group Solution Guide.](http://techdocs.cohesity.com/TechGuides/PDFs/Cohesity-Solution-Guide-Microsoft-SQL-AAG.pdf)

|     |     |
| --- | --- |
|     |
|     | ![](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image007.gif) |




7 _Always On Availability Groups (SQL Server)”,_ Microsoft Corp., https://docs.microsoft.com/en-us/previous- versions/sql/sql-server-2012/hh510230(v=sql.110)



![You may run into this: A protection group for SQL AAG fails the first time after SQL database
fail over within an AAG group; however, it succeeds after the second attempt. This behavior results from the AAG metadata change that is triggered when the SQL database fails over. If the AAG configuration changes, such as adding or removing a database or changing the backup preference or failover, then the next log or full backup will fail. This is a known behavior. The failed backup run will update the entity hierarchy with changed metadata, and the subsequent run should succeed.
](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image016.gif)

## Position for SQL Cluster
{: #position_for_sql_cluster}

Also known as Windows Failover Cluster Instance (FCI), this is a common production configuration. It provides high availability for the databases by moving SQL instances to a second host if the first host becomes unavailable. IBM Cloud Supports SQL Failover Cluster Instances (FCIs). Each SQL FCI node (host) must be registered as a unique source, as each SQL FCI might become active on different nodes.

**{{site.data.keyword.baas_full_notm}} recommends** you register an FCI by using the virtual IP (SQL VIP) for each of the SQL instances in the Windows cluster. Properly registering an FCI as a {{site.data.keyword.baas_full_notm}} MS SQL Cluster source enables {{site.data.keyword.baas_full_notm}} to follow the SQL instance during a failover to new nodes, and then continue to take backups.

|     |     |
| --- | --- |
|     |
|     | ![NOTE: A common mistake is to register a SQL FCI using the physical IPs or the Windows cluster IP instead of the SQL Server’s virtual IPs.,NOTE: {{site.data.keyword.baas_full_notm}} does not support Clustered Shared Volumes (CSV). CSV refers to the storage underneath the SQL FCI. Clustered Shared Volumes enable multiple nodes in a failover cluster to simultaneously have read-write access to the same LUN (disk) that is provisioned as an NTFS volume.,NOTE: While the Backup agent will follow the SQL instance during a failover, it does not support Microsoft Failover Cluster awareness for file services.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image017.gif) |



Before registering an MS SQL Server cluster, you must download and install the Backup agent on each node within the MS SQL cluster. For detailed instructions, see [Set Up an MS SQL FCI](https://docs.cohesity.com/6_6/Web/UserGuide/Content/MSSQL/SQLSetupFCI.htm) in the online Help.

## Strategize for a SQL VM
{: #Strategize for a SQL VM}

IBM Cloud Supports backing up a SQL server by registering the server as a VM backup and as a physical server backup. When selecting a backup type, consider:

·         A VM backup takes a snapshot of the VM at a point in time and backs up the OS hosting SQL as well as any other data in the VM, including the SQL server instance. A VM backup leverages the hypervisor to access the VM, to which an agent is deployed. A restore would restore the entire VM at that point in time.

·         A SQL Server backup using the {{site.data.keyword.baas_full_notm}} physical agent supports backing up specific databases which can be restored to a new server.



You can configure for both methods. Doing so can help meet specific requirements, such as a weekly backup of the VM and a daily backup of the SQL databases.

**{{site.data.keyword.baas_full_notm}} recommends** creating two separate protection groups; one to protect the SQL databases and one to protect the VM.

The protection group to protect the SQL Server databases will be an agent-based backup.

The protection group to protect the VM will be a hypervisor-based backup via Protection Policy and must be configured to take application-consistent backups using a VM Protection Policy. Choosing this approach will:

·         Use VSS integration via VMware Tools.

·         Be Incremental forever with VMware CBT.

·         Facilitate a volume-level recovery.

·         _Not_ have separate log backup for the VM, which means no Point-in-Time (PIT) recovery.



# Consider Non-Agent-Based Backups
{: #consider_non_agent_based_backups}

SQL Native backups are still widely used and a viable backup technology for the foreseeable future. {{site.data.keyword.baas_full_notm}} and SQL Native backups pair very well together.

## SQL Native Backups
{: #sql_native_backups}

Bringing together MS SQL Server Native backups and {{site.data.keyword.baas_full_notm}} allows you to keep your current backup process and take advantage of {{site.data.keyword.baas_full_notm}}’s powerful features like replication, compression, and archiving.

When you deploy a SQL Native backup, **{{site.data.keyword.baas_full_notm}} recommends** using a View to manage and back up files, reduce storage, protect backup files, and archive them for long-term retention.

No agent installation is necessary for an MS SQL Native backup.

|     |     |
| --- | --- |
|     |
|     | ![IMPORTANT: When deploying {{site.data.keyword.baas_full_notm}} with SQL Native backups, be sure to assign READ/WRITE permission for the SQL AGENT service account to your {{site.data.keyword.baas_full_notm}} Views.](Cohesity-Deployment-Guide-Microsoft-SQL-Configurations_files/image018.gif) |
{: caption="" caption-side="bottom"}



For a full discussion of using SQL Native backups with {{site.data.keyword.baas_full_notm}}, see [Streamline Microsoft SQL Server](http://techdocs.cohesity.com/TechGuides/PDFs/Cohesity-Solution-Guide-Microsoft-SQL-Server-Native-Backups-DataPlatform.pdf) [Native Backups with Cohesity DataPlatform.](http://techdocs.cohesity.com/TechGuides/PDFs/Cohesity-Solution-Guide-Microsoft-SQL-Server-Native-Backups-DataPlatform.pdf)
