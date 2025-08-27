---

copyright:
  years: 2024
lastupdated: "2024-10-7"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Backup microsoft sql server (volume-based)
{: #backup_microsoft_sql_server_volume-based}

12 September 2024

This topic describes how to protect MS SQL with a volume-based job. Volume-based jobs protect MS SQL at the server level, meaning all databases on a server. File-based jobs protect only the specific MS SQL databases that you select. To use a file-based job, follow the steps described in [Microsoft SQL Server (File-based)](SQLProtectFileBased.htm).

## Ms sql protection group support
{: #ms_sql_protection_group_support}

The following table describes supported job types for MS SQL running on physical servers and VMs.


|     | File-based Jobs | Volume-based Jobs | VDI-based Jobs | Both Job Types Using the Same Server but Protecting Distinct Databases |
| --- | --- | --- | --- | --- |
| Physical MS SQL | Multiple file-based jobs protecting all or a subset of DBs | One volume-based job protecting all or a subset of DBs | Multiple VDI-based jobs protecting all or a subset of databases | Supported |
| SQL Server Virtual Machine Backup | NA  | Volume-base with Hypervisor to provide application-consistent backup | NA  | NA  |
{: caption="" caption-side="bottom"}

*   (_Applies to both standalone MS SQL and MS SQL FCI_.) If the VM SQL you registered has physical VMware RDM disks attached, then the MS SQL databases located on these disks will not be backed up and the following message is displayed in the Protection page:

    `Identified <Number> physical RDM disks attached to this VM. Backup of physical RDM disks are not supported using VADP. Databases located on any of these disks are not backed up.`

*   (_Applies only to both standalone MS SQL and MS SQL FCI_.) If the VM SQL you registered has virtual VMware RDM disks attached, then the MS SQL databases located on these disks will not be backed up and the following message is displayed in the Protection page:

    `Identified < Number> virtual RDM disks attached to this VM. Backup of virtual RDM disks are not supported. Databases located on any of these disks are not backed up.`

*   (_Applies only to standalone MS SQL_.) If the VM VM SQL you registered has iSCSI devices attached and if there are SQL databases having their database files on these iSCSI disks, then those SQL databases will not be backed up. The following message is displayed in the Protection page:
    `Found < Number> databases with files on external storage. These databases cannot be backed up. Please use alternate backup methods like native dumps, Cohesity file base backup, or Cohesity's SQL VDI to protect the databases`

Converting an existing volume-based Protection Group to a file-based Protection Group is not supported. You must delete the existing Protection Group and create a new one.

## Protect ms sql overview
{: #protect_ms_sql_overview}

Perform the following steps in the {{site.data.keyword.baas_full_notm}} to protect SQL Server:

1. Register the SQL Server you want to protect as a source with the {{site.data.keyword.baas_full_notm}}. See [Set Up Standalone Microsoft SQL Server or Microsoft SQL Server AGs](SQLSetupStandalone.htm) or [Set Up Microsoft SQL Server FCI](SQLSetupFCI.htm).
2. Select **Dashboard** and from the drop-down, select **MS SQL**.
3. From the **Hosts** tab, you can protect all databases on a host. Select **Protect Databases** from the host's actions menu.

    From the **Databases** tab, you can protect a single database. Select **Protect** from the database's actions menu.

    You can change which objects you want to protect when creating the job.


You can also protect MS SQL by performing the following steps:

1. Navigate to **Data Protection > Protection** .
2. Click **Protect** on the top-right of the page and then select **Databases** > **MS SQL Server**.
3. Select the objects to protect.

## Create a volume-based ms sql protection group
{: #create_a_volume-based_ms_sql_protection_group}

1. Navigate to **Data Protection > Protection**.
2. Click **Protect** on the top-right of the page and then select **Databases** > **MS SQL Server**.
3. In the **New Protection** pop-up, do one of the following:

    *   Protect MS SQL by specifying a subset of options such as objects, protection group and policy by clicking the edit ![](../Resources/Images/i/icn/edit-d.svg) icon corresponding to the following options:

        1. **Add Objects**: Select the registered MS SQL source or click Register Source to register an MS SQL source. Select the objects to protect .

        2. **Protection Group**: Specify a name for the Protection Group.

        3. **Policy**: Select an existing policy or click **Create Policy** to create a new protection policy.

        4. **Storage Domain**: Select the appropriate storage domain.

            This option is visible on the **New Protection** pop-up only if there are more than one storage domain.

        5. **Backup Type**: From the drop-down, select **Volume-based**.

        6. Click **Protect**.

    *   To protect MS SQL by specifying all the options, click **More Options**.

4. **Source:** Select the objects to protect.

    Select the source objects to back up or exclude. By default, all objects in the source are excluded as indicated by the ![](../Resources/Images/checkboxUnselected.png) unselected check box. Expand the source objects and select the objects you want to include in the Protection Group. A selected check box (![](../Resources/Images/checkboxSelected.png)) indicates the object is selected to be backed up.

    If you have registered a physical server that is a part of the registered MS SQL Failover Cluster, then the MS SQL Failover Cluster Instance will not appear as a source in the object list of the registered physical server.

    [![Closed](../../Skins/Default/Stylesheets/Images/transparent.gif)If appropriate for your organization, enable Auto Protect.](#)

    Click![](../Resources/Images/i/icn/autoprotect-on.svg)to turn on **Auto Protect**. By enabling this option:

    *   All the SQL databases that are added to the SQL server in the future are automatically added to the Protection Group and are protected from the next protection run.
    *   All the SQL databases that are removed from the SQL server in the future are automatically removed from the Protection Group and are not protected from the next protection run. The existing backup for the user is preserved until the snapshot expires.


    For example, you create a Protection Group that backs up a SQL Server. The Protection Group runs for two weeks and then a new database is added to the SQL Server. The next time the Protection Group runs, if the object hierarchy has been refreshed on the {{site.data.keyword.baas_full_notm}}, the new database is also backed up even though the new database has not been explicitly selected to be included in the Protection Group. The object hierarchy is automatically refreshed every four hours.

    To manually refresh the object hierarchy, select **Data Protection > Sources**, find the source in the list and click ![](../Resources/Images/i/icn/refresh-h.svg). To exclude an object from Auto Protect, click ![](../Resources/Images/autoprotect-checkbox.png).

    *   ![](../Resources/Images/autoprotect-checkbox.png) Indicates an Auto Protected object.
    *   ![](../Resources/Images/autoprotect-checkbox-excluded.png) Use this option to exclude SQL databases from the protection group. Excluded objects are not protected. The icon indicates that an ancestor of the object is Auto Protected but this object is explicitly excluded.

        To exclude a database from the protection group, specify the absolute path of the database to be excluded from the backup at a protection job level. You can optionally use:

        *   **Wildcard Exclusion** - You can optionally use the Wildcard \* in any location in the search string to exclude all the matching databases from the backup at a protection job level.

        *   **Regex Exclusion** - You can use the RegEx string name match and manual user selection to exclude DBs from backup at a protection job level.


    You can use the exclude SQL database feature only if Auto Protect is enabled for a backup job.

    Protection of dismounted databases, recovery database types, and deleted databases are automatically skipped by the Protection Group.

    After defining the filer click **Apply** to save the selection.

    The following table lists the examples for excluding objects in a MS SQL database:


    | Sample Configuration |     |     |
    | --- | --- | --- |
    | **10.2.157.20 (Host)**<br><br>**\* SQL2012**<br><br>(SQL Server Instance)<br><br>  - UserDB1<br><br>  - UserDB2<br><br>  - TestDB1<br><br>  - TestDB2 | **\* SQL2014**<br><br>(SQL Server Instance)<br><br>  - UserDB3<br><br>  - UserDB4<br><br>  - TestDB3<br><br>  - TestDB4 | **\* MSSQLSERVER**<br><br>(SQL Server Instance)<br><br>  - ProdDB1<br><br>  - ProdDB2 |
    | **Example of Absolute Path** |     |     |
    | *   **10.2.157.20/SQL2012/UserDB1** - Excludes the database 'UserDB1' from the SQL server instance 'SQL2012' on the host 10.2.157.20.<br>*   **10.2.157.20/SQL2012/** - Excludes the 'SQL2012' from the SQL server instance and its databases.<br><br>The absolute instance path must end with '/' and the plain text instance path must contain two or more slashes. |     |     |
    | **Example of Wildcard Exclusion** |     |     |
    | *   **10.2.157.20/SQL2012/User\*** - Excludes the databases 'UserDB1' and 'UserDB2' from the SQL server instance 'SQL2012' on the host 10.2.157.20.<br>*   **10.2.157.20/SQL201?/\*** - Excludes the SQL server instances 'SQL2012' and 'SQL2014' from the host 10.2.157.20. |     |     |
    | **Example of Regex Exclusion** |     |     |
    | **10.2.157.20/SQL.\*/.\*** - Excludes the SQL server instances 'SQL2012' and 'SQL2014' from the host 10.2.157.20. |     |     |

    **Considerations**

    Consider the following when using the **Auto Protect** option on the backup job

    *   If new databases are added on the host, the existing filters are applied on the newly added databases. For example, if the exclude filter matches the newly added databases then these databases will be excluded in the backup job.
    *   If any system databases are excluded by the filters in a SQL server, it will exclude all the system databases belonging to that SQL server.
    *   When defining filters to exclude AG databases, ensure that the filters are configured on all the AG hosts. For example, to exclude an AG database and if the AG database has 4 hosts, then ensure that the filters are defined on all the four AG hosts.
    *   Auto protect functionality is supported only for physical SQL jobs (file-based, Native, and volume-based)
    *   To selectively exclude a SQL database from the protection job, you must upgrade all the cluster nodes to version 6.5.1 and later.

    [![Closed](../../Skins/Default/Stylesheets/Images/transparent.gif)If MS SQL belongs to an AG network, selecting that server displays options for protecting the AG.](#)

    *   Select all servers that belong to this AG network.

        If backing up VMs within an AG, protecting the AG databases requires that all servers within the AG are registered with the {{site.data.keyword.baas_full_notm}} and are selected for protection. Otherwise AG databases are skipped and only local databases will be backed up. New servers added to AG are not automatically protected.

        When using this option when there are multiple AGs, only interlinked AG nodes are automatically selected. For example:

        *   If there are AGs that use only Node1 and Node2 and other AGs that use Node3 and Node4, if you select Node2 as a source, only Node1 is added automatically.
        *   If AG1 uses Node1 and Node2, AG2 uses Node2 and Node4, AG3 uses Node3 and Node5. If you select Node2, then these nodes are selected: Node1, Node2 and Node4. Node3 and Node5 are not selected.

        When adding nodes to an AG that is protected by {{site.data.keyword.baas_full_notm}}, ensure you register the node with {{site.data.keyword.baas_full_notm}} and add it to the Protection Group. Otherwise the AG databases will not be protected.

        When removing nodes from an AG that is protected by {{site.data.keyword.baas_full_notm}}, you can remove it from the Protection Group or keep it in the job; the AG databases remain protected in either case. If the removed node remains in the Protection Group, only its local databases are backed up. If you do not want to back up the removed node, you can safely remove it.

    *   Select only this server.

    Click **Select**.


Click **Add**.

6. If appropriate, set options for specific objects by clicking ![](../Resources/Images/i/icn/edit-h.svg)(**Edit Server Options**) next to the object. A successful SQL log backup automatically truncates the SQL logs.
7. **Name:** Enter a name for the Protection Group. The name can contain alphanumeric characters, underscores, dashes, and periods.

This page displays registered MS SQL objects only, so only MS SQL servers are selected regardless of the other servers within the source object hierarchy.

11. **Policy:** Select an existing protection policy or create a new one by selecting **New Policy**.


**Storage Domain:** Select an existing storage domain or create a new one by selecting **New Storage Domain**.

Once you create a Protection Group, you cannot change the Storage Domain you selected for that job.

The selected storage domain stores the snapshots and logs created by this Protection Group.

15. **Backup Method:** Select **Volume-based**. This method protects MS SQL at the server level, meaning all databases on a server.

    If you changed the backup method to volume-based after objects were selected, you must select a server object.

16. **Make Full Backup Copy-only:** Toggle on if you want full backups to be copy-only backups so they do not affect the differential base.

    Copy-only full backups do not take log backups even if they are scheduled by the policy.

17. **User Databases:** Select how to handle AG databases during backup.
18. **AG Backup Preferences:** Select this option if AG databases will be backed up.

    *   **Use Server Preferences** uses MS SQL preferences.
    *   **Override Preferences** enables you to override MS SQL's preferences with your selection.

    [![Closed](../../Skins/Default/Stylesheets/Images/transparent.gif)AAG replica preference for MS SQL backups](#)

    {{site.data.keyword.baas_full_notm}} uses "replica priority" to select the best replica when more than one qualified replica matches the backup preference. For the following AAG Backup Preference settings, {{site.data.keyword.baas_full_notm}} uses replicas in the indicated order of preference to back up MS SQL databases:


    | Backup Preference Setting | Replica Used for MS SQL Backup |
    | --- | --- |
    | Prefer Secondary or Any | 1\. Sync Secondary Replica<br><br>2\. Async Secondary Replica<br><br>3\. Primary Replica |
    | Secondary Only\* | 1\. Sync Secondary Replica<br><br>2\. Async Secondary Replica |
    | Primary Only | 1\. Primary Replica |

    \* If the AAG uses the **Secondary Only** Backup Preference setting, ensure the AAG replicas are set to "**Readable secondary**\=**Yes**". If all replicas are set to "**Readable secondary**\=**No**" with **Secondary Only** as the Backup Preference setting, then the backup of AAG databases fails with "_unavailable\_for\_vss_".

    You can set **Readable Secondary** field to **No** if the Backup Preference Setting is set to **Prefer Secondary or Any** or **Primary Only**.

    You must apply the exclusion filter on each AAG replica to ensure the database is excluded when a SQL Server failover occurs.

19. **System Databases:** Select whether to back up or skip system databases.
20. **Volume Backup:** Optionally select **Back up only the volumes associated with selected databases** to back up only volumes that are associated with selected databases. This option is not used for MS SQL FCI; only volumes with databases are backed up. For MS SQL running on VMs, all volumes are always backed up.
21. **Start Time:** Indicates when the Protection Group should run. The current time is displayed by default but you can change it. Enter the hour and minutes or use the up and down arrows on your keyboard. Verify the AM or PM setting.

    The default time zone is the browser's time zone. You can change the time zone by selecting a different time zone.

22. **End Date:** Optional. Toggle on and select the date when the Protection Group stops capturing snapshots. A protection run that starts prior to this date runs until completion even if it completes after this date.

23. **QoS Policy:** Select an appropriate quality of service (QoS) policy.

    {{site.data.keyword.baas_full_notm}} recommends specifying **Backup HDD**, which is the default.

    *   **Backup HDD**: The {{site.data.keyword.baas_full_notm}} writes the data directly to an HDD drive for this Protection Group.
    *   **Backup SSD**: The {{site.data.keyword.baas_full_notm}} writes the data directly to an SSD drive for this Protection Group. Only specify this policy if you need fast ingest speed for a small number of Protection Groups.
    *   **Backup Auto**: (Applicable only for Cohesity C6000 series) The {{site.data.keyword.baas_full_notm}} writes the data to both SSDs and HDDs. The distribution of data will be based on the current usage of SSD and HDD. This policy tries to achieve similar backup performance as the **Backup SSD** policy and reduces the SSD wear-out compared to the Backup SSD policy.
    *   **TestAndDev High:** The {{site.data.keyword.baas_full_notm}} writes the data to an SSD drive for this Protection Group. The I/Os with this QoS policy are given higher priority compared to other QoS policies. Use this policy for workloads that require lower I/O latency.

24. **Cancel Runs at Quiet Time Start:** Available only if the selected policy has at least one quiet time period. Toggle it on to specify that all currently executing protection runs should abort if a quiet time period specified for the Protection Group starts. By default this toggle is off, which means after a protection run starts, it continues to execute even when a quiet time period specified for this protection run starts. However, a new protection run will not start during a quiet time period.

25. **Pre & Post Scripts:** Edit this option to run scripts on the protected server before and/or after a Protection Group runs. If configured, the scripts are run every time an object is backed up by a Protection Group run.

    For configuration details, see [Configure Pre & Post Scripts](../Dashboard/Protection/PrePostScripts.htm).

26. **Incremental After Restart:** For Windows physical servers, toggle on if you prefer incremental backup for the first scheduled backup job, which runs after an intentional server restart. If an incremental backup is not possible, a full backup is performed. For example, if the server restarts after a power failure, a full backup is performed.

    Incremental backup consistency across reboots is not guaranteed if I/O operations are performed on the volume outside of the fully functional state of the server. This includes I/O operations that are performed by booting into the recovery mode. For example, if a server is shut down and its volume is attached to another server that does not have the Backup agent, ensure that the next backup is a full backup operation.

    This option is not available for file-based backups.

27. **Indexing:** Indexing is enabled by default and required for file recovery. The {{site.data.keyword.baas_full_notm}} will scan all the files in the Protection Group and create an internal index that can be used later by a recover task to locate files by name.

    For information about customizing indexing, see [Customize Indexing](../Dashboard/Protection/Indexing.htm).


Indexing is not supported for file-based Jobs. Without indexing, file/folder-level recovery and searching are not supported.

29. **Alerts:** Optional. Select one or more of the following settings if you want alerts to be created for the following triggers:

    *   **Success:** Create an informational alert when a Protection Group completes successfully. Emails are not sent when informational alerts are created.
    *   **Failure:** Create a critical alert if the Protection Group fails to complete. Emails are sent when critical alerts are created.
    *   **SLA Violation:** Create a warning alert if the Protection Group takes longer than the time period specified in the SLA field. Emails are sent when warning alerts are created.

    For more information, see [Alerts](../Dashboard/Monitoring/Alerts.htm#top).

30. **Priority:** Select a priority for the Protection Group execution. IBM Cloud Supports concurrent backups, but if the number of Protection Groups exceeds the ability to process them, those with **High** priority are given the highest priority to execute, **Medium** the second-highest priority, and **Low** the lowest priority.
31. **Email Recipients**: You can add email addresses to a Protection Group to notify the email recipients when alerts are triggered for the Protection Group.


    The connection to the SMTP Server must be enabled to send email notification. You can enable the SMTP Server connection in the configuration settings page ( {{site.data.keyword.baas_full_notm}} UI > **Settings** > **Summary** > **Configure** > **Enable SMTP Server**).

32. **SLA:** The service-level agreement (SLA) defines the expected time to complete either a full backup or incremental backup. The backup completion time depends on the Windows volume size on AD, objects changed between incremental backups, network speed, and other Protection Groups running on the {{site.data.keyword.baas_full_notm}}.

    *   **Incremental:** Enter the number of minutes you expect an incremental protection run to complete. An incremental backup captures only the differences (changed blocks) since the last protection run.
    *   **Full:** Enter the number of minutes you expect a full backup protection run to complete. A full backup captures the entire object (all blocks).
33. Click **Protect**.

The Protection page is displayed and includes the new Protection Group.

In the Protection page, the following message will be displayed when you mouse over the run status of the MS SQL databases that are located on the attached disks:

```text
<database_name> has files located on external storage. Cannot backup the database. Please use alternate backup methods like native dumps to protect the databases.
```
{: codeblock}

## Edit an ms sql protection group
{: #edit_an_ms_sql_protection_group}

1. Select **Data Protection > Protection**.
2. Locate the Protection Group you want to edit.
3. Mouse over the action icon (![](../Resources/Images/action_menu.png)) next to the job and and select one of the following:
    1. **Run Now** - Start a run of this Protection Group. You have the option to back up one or more objects.

        1. **Backup all Objects in the Protection Group** - Select this option to backup all the databases in the protection group. Any databases excluded from the backup will not be included in the protection job run.

        2. **Backup only Selected Objects in the Protection Group** - you can manually select the databases to be backed up at the protection job level. However, selection of the database that is excluded at the backup job level is not supported.

    2. **Pause Future Runs**

        1. Select **Pause Future Runs** to stop new runs of this Protection Group from executing. However, if the job is currently executing a run, the current run continues to execute and only future runs of this job are paused.

        2. Select **Resume Future Runs** to restore the Protection Group to a running state. The runs resume as specified in the schedule defined in the policy associated with this job.

    3. **Edit** - Edit job settings of the protection group and click Protect.

    4. **Delete** - Deletes the protection group.

4. Save the job settings by clicking **Protect**.
