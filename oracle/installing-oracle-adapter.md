---

copyright:
  years: 2026

lastupdated: "2026-09-25"

keywords: Oracle adapter, Oracle data protection, Backup and Recovery agent, install Oracle adapter, RMAN, backup Oracle, recover Oracle, Oracle protection group

subcollection: backup-recovery

content-type: how-to

---

{{site.data.keyword.attribute-definition-list}}

# Installing the Oracle adapter
{: #installing-oracle-adapter}

Install and configure the {{site.data.keyword.baas_full}} Oracle Adapter to deploy Oracle data protection using Oracle Recovery Manager (RMAN) for application-consistent backup and recovery of Oracle databases.
{: shortdesc}

The {{site.data.keyword.baas_full_notm}} Oracle solution extends the scalability of RMAN and provides the features and tools needed to automate backups, recoveries, and data management in a single interface. The Oracle Adapter integrates with RMAN to support Oracle single instance, RAC, and Oracle Multitenant databases. It uses an incremental forever backup approach, eliminating the need for regular full backups.


## Introduction
{: #installing-oracle-adapter-intro}

The {{site.data.keyword.baas_full_notm}} Oracle solution supports several backup methods. This topic focuses on the Oracle Adapter Agent, which provides the following capabilities:

- Fully automated data protection for Oracle databases
- No requirement to write, manage, or update RMAN scripts
- Incremental-forever backups with no need for periodic full backups
- Instant recovery of Oracle databases
- Near-instant, zero-cost clones of Oracle databases for non-production use cases
- Advanced, centralized management and reporting of Oracle backups

### Selecting a deployment option
{: #installing-oracle-adapter-deployment}

Use the following table to choose a deployment option for Oracle data protection based on your environment.

| Operating system | Single instance | RAC | Standby/DataGuard | Multi-tenancy (CDB/PDB) | Oracle TDE |
| ---------------- | --------------- | --- | ----------------- | ----------------------- | ---------- |
| Linux (RHEL, CentOS, OEL 6/7/8), (SuSE/SLES 11, 12, 15) | Oracle Adapter | Oracle Adapter | Oracle Adapter | Oracle Adapter | Oracle Adapter |
| Windows (2012, 2012 R2, 2016, 2019) | Oracle Adapter or RMAN SBT | | | | |
{: caption="Oracle data protection deployment options" caption-side="bottom"}

CDB/PDB multi-tenancy support requires version 6.6.0x or later.
{: note}

### Oracle Adapter considerations
{: #installing-oracle-adapter-considerations}

The {{site.data.keyword.baas_full_notm}} Oracle Adapter uses the RMAN incremental merge backup method, which is an incremental forever technology. It supports all database sizes. However, incremental forever does not always mean faster than a full backup — it depends on the database size, change rate, and your {{site.data.keyword.baas_full_notm}} platform.

The following databases are ideal candidates for Oracle Adapter:

- You can use the {{site.data.keyword.baas_full_notm}} Oracle Adapter for all supported configurations regardless of database size.
- Databases with a change rate greater than 3–5 TB between successive backups may need additional adapter configurations.
- Automatic scheduling of database backups with out-of-box parallelism to meet backup SLA.
- Cold database backups are not supported by the Oracle Adapter.

## Before you begin
{: #installing-oracle-adapter-prereqs}

Before you install the Oracle Adapter, complete the following prerequisite checks:

1. Review and validate all prerequisites:
   - Confirm that the required software versions are met.
   - Verify that all prerequisites for Oracle Adapter are satisfied.
   - Confirm that the required ports are open for communication with the {{site.data.keyword.baas_full_notm}} cluster.

   You can use the following commands to test whether the ports are open. The available commands depend on which packages are installed on the server. The examples below assume a {{site.data.keyword.baas_full_notm}} node VIP of `10.99.1.65`.

   ```sh
   nc -zv 10.99.1.65 11113
   ```
   {: codeblock}

   ```sh
   curl -XGET 10.99.1.65:11113/flagz
   telnet 10.99.1.65 11111
   ```
   {: codeblock}

   ```sh
   nmap -Pn -p 11111 10.99.1.65
   ```
   {: codeblock}

1. Decide which operating system account runs the {{site.data.keyword.baas_full_notm}} agent on the Linux Oracle server:

   Run as root (recommended)
   :   Running the {{site.data.keyword.baas_full_notm}} service as root requires the minimum number of changes to the sudoers file.

   Run as a non-root user
   :   Running as a non-root user requires modifications to the sudoers file to permit the user to run commands as root. If the chosen service account does not own the Oracle binaries, the account must belong to an OS group that enables it to hold SYSDBA or SYSBACKUP privileges.

   AIX can only be installed as a root user.
   {: note}

## Installing the {{site.data.keyword.baas_full_notm}} agent
{: #installing-oracle-adapter-agent}

1. In the {{site.data.keyword.baas_full_notm}} home page, select **Data Protection** > **Sources**.
1. Click **Download {{site.data.keyword.baas_full_notm}} Agent** near the top-right of the page.
   - **Linux**: Both a script installer and an RPM option are available. The script installer is the recommended approach.
   - **AIX**: Offers the Java Agent.
   - **Windows**: Download the Windows Agent.
1. Copy the agent to the Oracle server and complete the installation.

## Registering the Oracle source
{: #installing-oracle-adapter-register}

Registering Oracle is a two-step process. You must first register the server as a physical server, then register it as an Oracle server.

### Registering the Oracle source as a physical server
{: #installing-oracle-adapter-register-physical}

1. In the {{site.data.keyword.baas_full_notm}} Dashboard, select **Data Protection** > **Sources**, then click **Register** > **Physical Server** from the drop-down menu.
1. In the **Hostname** field, enter the server name or IP address.

   If you prefer a configured interface group, you can select it here. Otherwise, {{site.data.keyword.baas_full_notm}} uses Auto Select by default. Click **Register**.

   To register an Oracle RAC, use the scan-name or scan-ip for the source registration. To register an Oracle Veritas Cluster Server (VCS), use the virtual resource name or virtual IP for source registration.
   {: note}

1. In the **Sources** page, select the physical server from the list, click the ellipsis icon at the right, and select **Register as Oracle Server** from the menu.
1. Select the **Authentication Type** for the database server: **OS Authentication** or **Database Authentication**.

   For most implementations, OS Authentication is used. See the Authentication table for authentication requirements by specific operation.

   If you select Database Authentication, enter the username and password of the database account.

   For Database Authentication, a database account with either SYSDBA or SYSBACKUP privileges must exist in all databases on the server being registered. Individual database accounts for separate databases residing on a single database server are not supported.
   {: note}

1. Click **Register** to complete the Oracle source registration.
1. Select the source to verify that all databases are successfully discovered.

   When you first select the source, the default view is the **Protected Objects** screen, which shows no objects because a Protection Group has not been created yet.

1. Click **All Objects** to see the databases that have been discovered.

   Click the eye icon next to the database to view details such as Archive Log Enabled (Required), BCT Enabled (Recommended), Database Type, and Oracle Home.
   {: tip}

   If no databases are discovered or if databases are missing from the Oracle source, see [Troubleshooting tips](#installing-oracle-adapter-troubleshooting-tips) for more information.
   {: note}

## Creating an Oracle protection group
{: #installing-oracle-adapter-protection-group}

A Protection Group uses the schedules and settings defined in the policy to determine when and how backups are captured, archived, or replicated.

1. From the main dashboard, select **Data Protection** > **Protection**, click **Protect** in the upper right corner, and select **Databases** > **Oracle Database** to start creating the Protection Group.
1. Select the Oracle databases that you want to protect. All Oracle servers registered with the {{site.data.keyword.baas_full_notm}} cluster are displayed, and individual databases can be selected. Click the pencil icon to the right of a database to configure specific options for it.
1. When you click the pencil icon, the **Options** screen appears, which allows you to configure Oracle channels and archive log deletion.
1. By default, {{site.data.keyword.baas_full_notm}} selects the number of RMAN channels using the following logic:

   ```
   RMAN channels = MIN(2 × number of IBM Backup and Recovery nodes, 2 × number of target server CPUs)
   ```
   {: codeblock}

   To modify the default, click **Select specific node(s)** and modify the number of channels to allocate.

   The example above illustrates channel allocation for a standalone Oracle server. In a RAC environment, all nodes of the RAC are displayed, and channels can be allocated over individual RAC nodes.
   {: note}

   If Oracle Standard Edition is installed, Oracle does not allow multiple RMAN channels. Setting this to a number higher than 1 does not increase the number of RMAN channels.
   {: note}

   DB Authentication must be configured to use multi-channel, multi-node RMAN channels in a RAC protection group.
   {: note}

1. To configure archive log deletion for the database, enable the **Delete Archive Log** toggle and click **Save**. {{site.data.keyword.baas_full_notm}} does not delete archive logs by default.

   After archive log deletions are enabled, they are configured to be kept on disk for one day and then deleted. To have archive logs deleted immediately after being protected, change this value to `0`. You must set the archive log deletion setting for each database.
   {: note}

2. Enter a **Name** for the Protection Group.
3. Select an existing policy for the Protection Group or click **Create Policy** to define a new one.
4. Specify the **Log Backup** schedule and retention for the database logs.

   If a daily incremental backup is configured, the Protection Group creation screen includes an option to set the **Start Time**. If the interval is less than 24 hours, the start time is not configurable, and backups run based on the number of hours set between backups in the policy.
   {: note}

5. Configure any **Additional Settings** for the Protection Group.

   The Oracle Adapter uses NFS mounts on Linux and AIX. The additional settings include an option for whether NFS mounts persist on the database server after jobs complete. By default, NFS mounts are set to persist. To remove these mounts, set the **Persist Mountpoints** option to **No**.
   {: note}

6. Click **Save**.

## Backing up Oracle databases
{: #installing-oracle-adapter-backup}

Database backups and log backups run on the schedules determined by the policy assigned to the Protection Group. To run an ad-hoc backup, use the **Run Now** option.

### Running an ad-hoc backup
{: #installing-oracle-adapter-backup-adhoc}

1. Hover over the desired Protection Group, click the ellipsis icon, and select **Run Now** from the menu.
1. In the **Run Now** options screen, choose the default option **Backup all Objects in the Protection Group** or select **Backup only selected Objects in the Protection Group**.
1. The default **Backup Type** is Incremental. You can also select **Full** or **Log** backup.

   While the Protection Group is backing up databases, job details can be viewed in the UI by selecting the Protection Group, selecting the desired backup run, and then selecting the desired database.
   {: note}

## Recovering Oracle databases
{: #installing-oracle-adapter-recover}

The following recovery options are available for Oracle Adapter backups:

- Recover the database back to its original location.
- Recover the database as a new database on the same server or to an alternate server.
- Recover the native RMAN backup to a mounted view.
- Recover an individual pluggable database.
- Perform an instant recovery, which creates a database clone on the {{site.data.keyword.baas_full_notm}} cluster and migrates the database back to primary storage while the database is online.

### Running a database recovery
{: #installing-oracle-adapter-recover-steps}

1. From the main dashboard, navigate to **Data Protection** > **Recoveries**.
1. Click **Recover** and select **Databases** > **Oracle** from the drop-down menu.
1. Search for the database to recover by entering the name (partial or full), then select the database and click **Continue**.
1. After the recovery point-in-time is set, configure the number of streams used for the restore. In a RAC environment, you can define multiple channels over multiple nodes to optimize recovery time.
1. Select the desired **Recovery Option**.

   When restoring Oracle databases, recovering to an alternate platform (for example, AIX to Linux) is not supported because an RMAN Convert is required for this type of operation.
   {: note}

   Oracle recovery to later versions is not supported because an Oracle upgrade would be required before opening the databases.
   {: note}

### Alternate database recovery
{: #installing-oracle-adapter-recover-alternate}

When recovering a database as an Alternate Database, the RMAN Duplicate process is invoked, restoring the protected database as a new database with a new Database ID (DBID).

You can recover the database to the same or an alternate server. The entire process is managed by RMAN, removing administrative actions from the end user.

1. Enter the values for both the **Oracle Home** and **Base Directory** fields for the destination Oracle installation.

   These values can be determined by running `echo $ORACLE_HOME` and `echo $ORACLE_BASE` in the destination Oracle environment.
   {: note}

1. Enter the **Database Name**.

   The database name must conform to Oracle naming rules for instances (maximum of 8 alphanumeric characters).
   {: note}

   RAC restores to Alternate Databases are restored as standalone instances. RAC can be restored through a single node using the configured channels option.
   {: note}

### Overwriting the original database
{: #installing-oracle-adapter-recover-overwrite}

The **Overwrite Original Database** option recovers the database back to its original location. No additional details are required from the user.

RAC restores using the Overwrite Original Database option are recovered as RAC. Overwriting the original database destroys the original database.

### Creating a {{site.data.keyword.baas_full_notm}} view with database files
{: #installing-oracle-adapter-recover-view}

While both the Alternate Database and Overwrite Original Database options recover the database as a whole, there are situations where a user may want to recover a tablespace, one or more datafiles, archive logs, a controlfile, or an spfile. For this level of granularity, the native RMAN backup can be made available through an NFS mount by using the **Create {{site.data.keyword.baas_full_notm}} View with DB Files** option.

This process does not physically move any data, so it is extremely fast.
{: note}

To create a {{site.data.keyword.baas_full_notm}} view with database files:

1. Select **Create {{site.data.keyword.baas_full_notm}} View with DB Files**.
1. Search for and specify the Oracle host destination server. The server must be registered with the {{site.data.keyword.baas_full_notm}} cluster.
1. Enter the name of the view to be created.
1. Click **Recover**.
1. After the recovery completes, start an RMAN session and run an RMAN `catalog` command so that RMAN recognizes the backup.
1. Run any RMAN script against this backup.

   When you use the recovery to a view option, one NFS mount is created for every node in the {{site.data.keyword.baas_full_notm}} cluster. All mounts point to the same view and contain identical contents. Use the `RMAN catalog start with` command to ensure all mount paths are cataloged.
   {: note}

### Container databases (CDB) and pluggable databases (PDB)
{: #installing-oracle-adapter-recover-cdb-pdb}

Starting with version 6.6, you can browse individual pluggable databases (PDBs) for recovery into the original container database or into an alternate container database. You can also recover the container database itself.

### Oracle Transparent Data Encryption (TDE)
{: #installing-oracle-adapter-recover-tde}

{{site.data.keyword.baas_full_notm}} supports protection and recovery of Oracle databases encrypted with TDE. {{site.data.keyword.baas_full_notm}} does not manage the TDE keystore, but provides the ability to specify shell environment variables so that the restore can locate the TDE keystore information and successfully restore and access the database.

## Performance and troubleshooting
{: #installing-oracle-adapter-performance}

This section provides an overview of the performance factors and troubleshooting tips to consider when using the {{site.data.keyword.baas_full_notm}} Oracle Adapter for Oracle data protection.

### Performance considerations
{: #installing-oracle-adapter-performance-considerations}

**Cluster size**

As a general rule, a single RMAN channel writing to a single {{site.data.keyword.baas_full_notm}} node provides approximately 125 MB/sec throughput (assuming no bottleneck and no other concurrent workloads). However, the expected steady-state performance is approximately 75 MB/sec. Every four nodes yield roughly 300 MB/sec, provided there are no bottlenecks in the network and the database server has sufficient resources.

Ingest rates vary based on hardware. All-flash arrays have significantly higher throughput than hybrid nodes. Throughput lower than 75 MB/sec for a single channel should be investigated further. Qualify throughput using full backups only.

**RMAN channels**

Optimize the number of RMAN channels to take advantage of the most {{site.data.keyword.baas_full_notm}} cluster nodes while not overtaxing the CPUs on the database server.

**Oracle Block Change Tracking (BCT)**

Enabling BCT on the Oracle side increases the performance of incremental backups. Without BCT enabled, RMAN scans the entire database for changed blocks. Enabling BCT allows Oracle to maintain a file that tracks changed blocks. RMAN uses this file instead of scanning the entire database, which also helps the data protection method for incrementals.

**RMAN parameters**

For the {{site.data.keyword.baas_full_notm}} Oracle SBT plug-in and RMAN NFS target using RMAN backup set, setting the RMAN parameter `FILESPERSET=1` maximizes deduplication on the {{site.data.keyword.baas_full_notm}} cluster. For the {{site.data.keyword.baas_full_notm}} Oracle Adapter, setting `FILESPERSET` through an Oracle Adapter Agent gflag does not affect deduplication, but may help tune the performance of the overall incremental backup time.

**Section size**

In databases that have a datafile significantly larger than other datafiles, a backup encounters a "long pole" effect where all RMAN channels finish backing up the other datafiles while a single channel processes the large datafile, causing the entire backup to run longer. Using the RMAN `SECTION SIZE` parameter, Oracle can break up datafiles into smaller sizes to allow multiple RMAN channels to back up the large datafile in parallel, shortening the overall backup time. Smaller section sizes may also help reduce the incremental merge time for larger databases. Oracle supports `SECTION SIZE` with backupset and image copies (starting from 12c).

To change the value of `SECTION SIZE` or `FILESPERSET` for Oracle Adapter to improve backup performance, contact {{site.data.keyword.baas_full_notm}} Support.
{: note}

### Troubleshooting tips
{: #installing-oracle-adapter-troubleshooting-tips}

**Verify prerequisites and review the installation log**

- Verify all prerequisites in the installation section.
- The installation log is created in the `/tmp` directory of the database server.

**Verify no other backups are running**

- Verify that no other backups are running against the database being protected. Competing backups cause RMAN errors if archive logs have been protected and deleted by another backup.
- If RMAN backup performance is below the expected level for your {{site.data.keyword.baas_full_notm}} node type and number of nodes, investigate the following factors:
    - Network bandwidth between the Oracle server and {{site.data.keyword.baas_full_notm}}. Use `iperf3` to measure it.
    - Number of CPUs on the Oracle server. Four CPUs or fewer is considered low.
    - The primary storage the Oracle database is using. Flash storage provides higher throughput.

**Review debug logs**

As of version 6.6, debug logs are system generated and can be used to troubleshoot Oracle database backup, recovery, and performance issues. These logs can be downloaded as a compressed file for both successful and failed recovery job runs.

To download a debug log, from the {{site.data.keyword.baas_full_notm}} Administration GUI, click the options icon and select **Download Debug Logs**. Debug logs are downloaded to the local host where the {{site.data.keyword.baas_full_notm}} Administration GUI browser is running.



**Review agent log files**

If there is an issue with the agent itself, the agent logs reveal errors and messages from the agent perspective during registration of Oracle source, database recovery, and backup/restore/clone job runs. Review the following log files in the agent install directory:

- `<agent install dir>/log/*INFO*`
- `<agent install dir>/log/*ERROR*`



## Next steps
{: #installing-oracle-adapter-nex-steps}

For more details, see the topics:

- [Prerequisites for the Oracle adapter](/docs/backup-recovery?topic=backup-recovery-prerequisites-oracle-adapter)
- [Troubleshooting the Oracle adapter](/docs/backup-recovery?topic=backup-recovery-troubleshoot-oracle-adapter)
