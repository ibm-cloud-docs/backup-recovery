---

copyright:
  years: 2025
lastupdated: "2025-08-28"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protect a physical server (file-based)
{: #protect_a_physical_server_file-based}


**To add or edit a Protection Group for Physical Server:**

1. Select **Data Protection > Protection** and do one of the following:
    1. To create a Protection Group— Click **Protect** on the top-right of the page and select **Physical Server** > **File-based**.
    2. To edit a Protection Group—Click the action icon (![](../../Resources/Images/action_menu.png)) next to the Protection Group and select **Edit**.
2.  In the **New Protection** pop-up, do one of the following:
    1.  Click **Add Objects** and select the Servers to protect.  

        You can also register a new source with the {{site.data.keyword.baas_full_notm}} so you can protect it.

    2. Select the physical servers to protect. By default, all Servers are excluded as indicated by the unselected check box. A selected check box (![](../../Resources/Images/checkboxSelected.png)) indicates the Server is selected to be protected.

        [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)You can optionally filter the Servers table.](#)

        *   In the search field, enter part of the Server name.
        *   Select an option from the **Filter** drop-down:
            *   **Unprotected**—Displays Servers that are not protected by any Protection Groups in the {{site.data.keyword.baas_full_notm}}—not protected by {{site.data.keyword.baas_full_notm}}.
            *   **Protected by {{site.data.keyword.baas_full_notm}}**—Displays Servers that are protected by any Protection Groups in the {{site.data.keyword.baas_full_notm}}.
            *   **Protected by this Job**—Displays Servers that are currently selected to be backed up by this Protection Group. (This option is available only when editing an existing Job.)

        Selecting the check box adjacent to Servers selects all of the filtered servers.

        [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)The graphical representations are described below.](#)

        *   ![](../../Resources/Images/checkboxUnselected.png) A unchecked box indicates the Object is excluded from any Protection Group.

            A shield ![](../../Resources/Images/i/icn/protected-source.svg) indicates the Object is already protected by another Protection Group. A Windows physical Server cannot be protected by a new protection group unless you remove it from the current protection group.

        *   A checked box ![](../../Resources/Images/checkboxSelected_20x20.png) indicates the Object will be protected by the current Protection Group.

        *   Click the ![](../../Resources/Images/i/icn/view.svg) icon to see details about the object.

        *   If the object name is in red, it is inaccessible, invalid, or orphaned. If the object is in one of the specified states when the Protection Group runs, no snapshot is created. If the object becomes accessible, a snapshot is created the next time the Protection Group runs.


    3. You can select either Windows, VCS, Solaris, Red Hat High Availability (Pacemaker) Cluster, or AIX physical object. Click the edit icon next to the selected server and use one of the following methods to define the file or a folder path in a file-based protection group for the selected object (Windows, VCS, Solaris, or AIX):

        | Options | Descriptions |
        | --- | --- |
        | Follow symlink NAS target<br><br>(applies only to **Windows file-based** backup) | Enable this option if you want to backup the symbolic link pointing to NAS target.<br><br>You can create a symbolic link on the windows server by running the following command:<br><br>`mklink /d /<link> /<target>`<br><br>Where<br><br><**link**\> specifies the name of the symbolic link that is being created.<br><br><**target**\> specifies the path (relative or absolute) that the new symbolic link refers to.<br><br>*   {{site.data.keyword.baas_full_notm}} does not support the backup of symbolic links within a symbolic link.<br>    <br>*   As the symbolic link points to a remote machine, the backup process will be slower than usual.<br>    <br><br>Specify at least one path for each Server you want to protect. By default, all files in the `/ root or c:\` directory are backed up. |
        | Protect all local volumes | Use this option to protect all the local volumes on the specific host. If you select this option, the system dynamically enumerates all the local volumes present during the backup protection group run and protects them.<br><br>In Windows, local volumes are the volumes listed in the "Disk Management" utility of the Windows host.<br><br>If you choose to use the Add Exclusions functionality when using this option, you can either specify:<br><br>*   Individual files or folders within a volume by specifying the fully qualified path names<br>*   Individual files or folders across all the volumes using the wildcards<br>*   Entire volumes by defining the volume path<br><br>If the exclusion path is not present at the time of a backup run, the system ignores this entry and proceeds to the next.<br><br>This option overrides any exclusion path specified as global exclude.<br><br>**Example**<br><br>If you specify `/mnt/banapps/` as a global exclude path but the local mount points are `/mnt/banapps/ar_techsis_views` and `/mnt/banapps/common`, then all the files and folders inside these local mount points will be included.<br><br>If you want the Protection Group to exclude all the local mount points inside `/mnt/banapps/`, then you can either add these local mount points as explicit exclude paths or choose a normal backup and then give "/" as the include path, and `/mnt/banapps/` as the exclude path. |
        | Use the directive file for backup | The directive file defines the locations of the files and folders that you can back up in a protection group. Specify the file path of the directive file (meta-file) on the source while adding it to a protection group. For more information, see [Directive File Backup](JobServerPhysical_DirectiveFileBackup.htm).<br><br>Applies only to **Windows, AIX, and Linux** servers file-based backup |
        | Nested Mount Points | Based on the Physical server type you want to protect, select the mount points to skip.<br><br>**For Windows Physical Server**:<br><br>Skip Nested Mount Points: Enable this option if you do not want to backup the volumes that are mounted to a subfolder within the selected directory structure. By default, this option is enabled.<br><br>**For Linux Physical Server**:<br><br>Nested mount point types to skip: From the drop-down list, select the mount point types that you do not want to back up. By default, all the mount point types available on the server are selected.<br><br>**Example**<br><br>If you select this option and give the path as / root to backup, the Protection Group will exclude all other mount points except / root including local mount points like /usr/, /home/, /opt along with any NFS mount points.<br><br>If you do not select this option, the Protection Group will backup all the local file systems along with all the mounted NFS file systems. Click Add Path to protect an additional path. |
        | Add Exclusion | > You can use this option to add exclusion entries for Individual files and folders and Protect all local volumes. For more information, see [Add Exclusions](JobServerPhysical_AddExclusions.htm). |
        | Add Path | Click Add Path to protect an additional path and then click Save.<br><br>For the Cohesity version below 6.6.0d\_u3, after an initial full backup of a cluster, if a parent directory is added, there is a probability of unaltered files going missing in the latest snapshot. This issue is fixed in Cohesity version 6.6.0d\_u3 and later. However, {{site.data.keyword.baas_full_notm}} recommends running at least one full backup after the upgrade. For more information, see [Incremental forever backups with parent directory addition](/docs/backup-recovery?topic=ncremental-forever-backups-with-parent-directory-addition){: external}. |

3. **Protection Group**: To enter a new name for the protection group, select **New Group** and enter the group name in the **Group Name** field. To use an existing protection group name, select **Existing Group** and select an existing group name from the **Group** drop-down list.

4. **Policy**: Select an existing protection policy or create a new one by selecting **New Policy**.

5. **Storage Domain**: Select an existing storage domain or create a new one by selecting **New Storage Domain**.

    Once you create a Protection Group, you cannot change the Storage Domain you selected for that job.

6. **Start Time**: Available only if the selected policy is set to Backup Daily. Indicates when the Protection Group should run. The current time is displayed by default but you can change it. Enter the hour and minutes or use the up and down arrows on your keyboard. Verify the AM or PM setting.

7. Click **More options** if you want to change or configure any of the additional settings.



    Setting

    Description

    Pause Future Runs

    Toggle this option to stop the protection runs of the Protection Group from executing. However, if the Protection Group is currently executing a protection run, the current run continues to execute and only future runs of the Protection Group are paused.

    End Date

    Optional. Toggle on and select the date when the Protection Group stops capturing snapshots. A protection run that starts prior to this date runs until completion even if it completes after this date.

    QoS Policy

    Select an appropriate quality of service (QoS) policy.

    {{site.data.keyword.baas_full_notm}} recommends specifying a Backup HDD, which is the default.

    *   **Backup HDD**: The {{site.data.keyword.baas_full_notm}} writes the data directly to an HDD drive for this Protection Group

    *   **Backup SSD**: The {{site.data.keyword.baas_full_notm}} writes the data directly to an SSD drive for this Protection Group. Only specify this policy if you need fast ingest speed for a small number of Protection Groups.

    *   **Backup Auto**: (Applicable only for C6K Platform) The {{site.data.keyword.baas_full_notm}} writes the data to both SSDs and HDDs. The distribution of data will be based on the current usage of SSD and HDD. This policy tries to achieve similar backup performance as the **Backup SSD** policy and reduces the SSD wear-out compared to the Backup SSD policy.


    Cancel Runs at Quiet Time StartAvailable only if the selected policy has at least one quiet time period. Toggle it on to specify that all currently executing protection runs should abort if a quiet time period specified for the Protection Group starts. By default this toggle is off, which means after a protection run starts, it continues to execute even when a quiet time period specified for this protection run starts. However, a new protection run will not start during a quiet time period.

    Pre and Post Scripts

    Edit this option to run scripts on the protected server before and/or after a Protection Group runs. If configured, the scripts are run every time an object is backed up by a Protection Group run.

    For configuration details, see [Configure Pre & Post Scripts](PrePostScripts.htm).

    Crash Consistent Backups

    (applies only to **Windows file-based** backup)

    Toggle on this option to read files from the snapshots of volumes on which the files (that need backup) are residing before the Protection Group is executed.

    Optionally, you can enable **Continue with non-consistent backup in case of failure** option to resume the backup in case the crash consistent backups fail.

    Source-Side Deduplication

    Toggle on to enable source-side deduplication for all the servers that are part of the Protection Group. Source-side deduplication is not supported on Windows 2008 R2 servers. For more information, see [Source-Side Deduplication for Physical Server](PhysicalServerSourceSideDedupe.htm).

    Toggle on Source Side Deduplication, click Add Exclusions and select the hostname (IP address) to be excluded from the deduplication protection group.

    The dedupe parameter must be enabled on the storage domains associated with the Linux and Windows servers.

    Cache Optimization

    This feature applies only to AIX and Linux (x86\_64) file-based backups.

    Cache Optimization is a modified source dedup functionality that offers optimized backup performance on the {{site.data.keyword.baas_full_notm}}. For more details, see Source-Side Deduplication and Cache Optimization for Physical Server.

    *   It is recommended to use this only for new jobs. {{site.data.keyword.baas_full_notm}} recommends not to switch to Source-side Dedup or normal incremental once you create a job with **Cache Optimization** functionality enabled. You can either select source-side dedup or CacheOptimization for a job.

    *   To use the Cache Optimization functionality, both the Backup agent and cluster should have version 7.0 and later.


    Indexing

    Indexing is enabled by default and required for file recovery. The {{site.data.keyword.baas_full_notm}} will scan all the files in the Protection Group and create an internal index that can be used later by a recovery task to locate files by name.

    For information about customizing indexing, see [Customize Indexing](Indexing.htm).

    Global Exclude Paths

    {{site.data.keyword.baas_full_notm}} version 6.6 and higher versions support global excludes at the protection group level, a standard entry for all servers. This feature applies to all types of the Supported operating system for physical agents.

    For a file-based Protection Group, specify the exclusion paths for all the servers that are part of the protection group. By default, all files in the / root or c:\\ directory are backed up.

    Click +Add to exclude a particular path or a particular file for all the specified sources. An exclusion will be a combination of global exclude and object-level exclude paths specified on each server. For more information on object-level exclusions, see [Add Exclusions](JobServerPhysical_AddExclusions.htm).

    **Example**

    When a user configures a protection group on the {{site.data.keyword.baas_full_notm}}, they can set up global exclude and include paths to specify which data should be backed up and which should be excluded. For instance,

    *   If /temp is provided as global exclude for a protection group and /temp/data as include path for a source, then /temp/data will be skipped by the {{site.data.keyword.baas_full_notm}}.

    *   If /temp is an include path for a source and /temp/data is provided as a global exclude path for the protection group, then all the data under /temp will be backed up, but the /temp/data folder will be excluded from the backup.


    The following table lists the support file systems and default exclusions for physical server file-based backups.


    | OS  | Supported File Systems | Default Exclusions |
    | --- | --- | --- |
    | Linux | ext3, ext4, xfs, vxfs, ReiserFS, autofs, cvfs, zfs, fuse.mfs, nfs, cifs, gpfs, mmfs | Virtual filesystems are excluded from backups. For example - tmpfs, devtmpfs, CacheFS, procfs, sysfs, etc. |
    | AIX | jfs2, cdrfs, nfs, nfs3, nfs4, jfs, vxfs, mmfs, gpfs, autofs |
    | Solaris | zfs, ufs, nfs, cifs, vxfs |
    | Windows | NTFS | Directories excluded from each physical drive.<br><br>For example - C:/, D:/, etc $Recycle.Bin, Cohesity\_Bitmap.cty, System Volume Information, System State |

    Windows System State backups are supported as part of Bare Machine Recovery (BMR) backups and not Physical Server File-based backups. For more details, please refer to [About Bare Machine Recovery](../../Concepts/BMR.htm).

    Allow Parallel Runs

    Toggle this option to allow parallel backup runs for the Protection Group.

    If the Protection Group is currently executing a protection run and a new run is started (can be manual or scheduled), then the current run continues to back up all the unfinished sources. In parallel, the new runs of the Protection Group will start backups for all the objects or entities that are processed completely in the current protection run.

    Alerts

    Optional. Select one or more of the following settings if you want alerts to be created for the following triggers:

    *   **Success**: Create an informational alert when a Protection Group completes successfully. Emails are not sent when informational alerts are created.

    *   **Failure**: Create a critical alert if the Protection Group fails to complete. Emails are sent when critical alerts are created.

    *   **SLA Violation**: Create a warning alert if the Protection Group takes longer than the time period specified in the SLA field. Emails are sent when warning alerts are created.


    For more information, see [Alerts](../Monitoring/Alerts.htm).

    Priority

    Select a priority for the Protection Group execution. IBM Cloud Supports concurrent backups, but if the number of Protection Groups exceeds the ability to process them, those with High priority are given the highest priority to execute, Medium the second-highest priority, and Low the lowest priority.

    SLA

    The service-level agreement (SLA) defines the expected time to complete either a full backup or incremental backup. The backup completion time depends on the Windows volume size on AD, objects changed between incremental backups, network speed, and other Protection Groups running on the {{site.data.keyword.baas_full_notm}}.

    *   Incremental: Enter the number of minutes you expect an incremental protection run to complete. An incremental backup captures only the differences (changed blocks) since the last protection run.

    *   Full: Enter the number of minutes you expect a full backup protection run to complete. A full backup captures the entire object (all blocks).


    Description

    Optional. Click the plus sign and enter a description for the Protection Group.

    Ignorable Errors

    Select this option to ignore the following errors during a file-based protection group run for physical servers. You can select one or both options as per your requirement.

    **End of File** - If this option is selected, the {{site.data.keyword.baas_full_notm}} ignores the End of File errors during a file-based protection group run and the file will be backed up successfully.

    **Non-Existent** - If this option is selected, the {{site.data.keyword.baas_full_notm}} ignores the Non-Existent errors during a file-based protection group run and the file will be backup successfully.

8. Click **Protect**.

The Protection page is displayed and includes the new Protection Group.

*   If a server is being backed up and files will also be recovered to that same server, the file recover task starts after the backup completes. The converse is also true; if files are being recovered to a server and that same server will be backed up, the backup starts after file recovery completes.

*   For a File-Based backup, the "Protected" and "Protected Size" fields on the source page for physical sources are updated after the source refresh.
