---

copyright:
  years: 2024
lastupdated: "2025-12-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Recover physical server files or folders to the original location
{: #recover_physical_server_files_or_folders_to_the_original_location}

To recover files and folders of a physical server to the original location:

1. Select **Data Protection > Recoveries**.
2. Click **Recover** located at the top right of the page and then select **Physical Server > Files or Folders**.
3. Select any one of the following options to search for the file or folder:

    *   **Files and Folders**: Allows you to search and recover files using file or folder names from any backup available on the {{site.data.keyword.baas_full_notm}}.
    *   **Browse or Specify Path for Search**: Allows you to search and recover files using the name of the server or Protection Group from any backup available on the {{site.data.keyword.baas_full_notm}}.


    | If you select.. | Then.. |
    | --- | --- |
    | **Files and Folders** | 1. Enter characters of the file name, path, or by [specifying the wildcard character \*](/docs/backup-recovery?topic=backup-recovery-use_wildcard_*_in_search_for_recovery).<br>    <br>    A list of items displays.<br>    <br>    You can optionally filter the search results by object type, source, object, Protection Group, Storage Domain, files only, or folders only.<br>    <br>2. From the list of search results, select the required files or folders to recover.<br>    <br>3. By default, the latest snapshot is selected for recovery. To recover from a different snapshot:<br>    <br>    The option to change the recover point is not available if multiple items were selected for recovery.<br>    <br>    1. From the selected list, mouse over the snapshot, and then click the **edit** icon.<br>        <br>        The Edit recovery point page appears.<br>        <br>    2. By default, only the snapshots that are indexed are displayed. To display all the available snapshots, toggle off the **Show Indexed Snapshots Only** option.<br>        <br>    3. Select a date range and the snapshot point for recovery.<br>        <br>    4. By default, the snapshot is recovered from the local {{site.data.keyword.baas_full_notm}}. You can also select the snapshot archived to the cloud (icon) by clicking this icon if available in the **Location** option.<br>        <br>    5. When recovering from an archive, consider the following points:<br>        <br>        *   The archive (external target) must be currently registered for the task to succeed.<br>            <br>        *   (_Applies only for cloud archive incremental with periodic full_) Provisioned Capacity Units (PCU) are required for all file-level recoveries. Provisioned capacity ensures that your retrieval capacity for expedited retrievals is available when needed. Each unit of capacity guarantees that at least three expedited retrievals can be performed every five minutes and provides up to 150 MB/s of retrieval throughput.<br>            <br>4. Click **Select Recovery Point** and go to step 4. |
    | **Browse** | 1. Enter the full or partial server or Protection Group name. You can by [specify the wildcard character \*](/docs/backup-recovery?topic=backup-recovery-use_wildcard_*_in_search_for_recovery).<br>    <br>    You can also optionally filter the search results by object type, source, object, Protection Group, Storage Domain.<br>    <br>2. From the list of search results, select the object whose files or folders are to be recovered.<br>    <br>3. By default, the latest snapshot is selected for recovery. To recover from a different snapshot, click the snapshots drop-down in the top-left corner and select the snapshot you need.<br>    <br>4. **Browse on Indexed Data**: Toggle on this setting to browse the indexed data. It helps to speed up browsing of recovery data. By default, it only displays the files and folders that are indexed. Hidden files, folders, paths such as /usr, /var, /sys , /Windows, /ProgramData etc. are excluded from indexing. To display all the available files and folders, including hidden files and folders toggle off the **Browse on Indexed Data** setting.<br>    <br>    *   Changing the snapshot after selecting the items (files or folder) removes the selected items from the cart.<br>    *   Enabling or disabling the **Browse on Indexed Data** option after selecting the items (files or folder) removes the selected items from the cart.<br>    *   Deleted snapshots may be displayed as valid recover points for a brief period until they are removed from the search index by a background process.<br>    <br>5. Browse to the file or folder that you want to recover by clicking a folder and subfolders.<br>    <br>6. Click **Save**.<br>    <br>7. After saving the selection, if you want to recover from a different snapshot or change the location from where the snapshot is recovered, perform the following steps:<br>    <br>    1. From the selected list, mouse over the snapshot, and click the edit icon.<br>        <br>    2. The Edit recovery point page appears.<br>        <br>    3. By default, only the snapshots that are indexed are displayed. To display all the available snapshots, toggle off the **Show Indexed Snapshots Only** option.<br>        <br>    4. Select a date range and the snapshot point for recovery.<br>        <br>    5. By default, the snapshot is recovered from the local {{site.data.keyword.baas_full_notm}}. You can also select a snapshot archived to the cloud (icon) by clicking this icon if available in the **Location** option.<br>        <br>        When recovering from an archive, consider the following points:<br>        <br>        *   The archive (external target) must be currently registered for the task to succeed.<br>            <br>        *   (_Applies only for cloud archive incremental with periodic full_) Provisioned Capacity Units (PCU) are required for all file-level recoveries. Provisioned capacity ensures that your retrieval capacity for expedited retrievals is available when needed. Each unit of capacity guarantees that at least three expedited retrievals can be performed every five minutes and provides up to 150 MB/s of retrieval throughput.<br>            <br>8. Click **Select Recovery Point** and go to step 4. |
    {: caption="" caption-side="bottom"}

    The {{site.data.keyword.baas_full_notm}} attempts to index all files and folders to a drive on both Windows and Linux systems. If the {{site.data.keyword.baas_full_notm}} is unable to find mount point information about files or directories, it indexes and displays these files and directories in the `lvol_N` directory, where `N` is a unique number such as `1`.

    On Windows systems, if the {{site.data.keyword.baas_full_notm}} finds the mount point information about files and directories, it indexes and displays these files and directories with a drive letter such as `C:`.

    Linux LVM indexing supports the following LVM types only: Linear, Striped, Mirrored, Mirrored + Striped, Thin. On Linux systems, how files and directories are indexed and displayed is dependent on the conditions specified in the following table.

    | Server Type | Volume Type |     |
    | --- | --- | --- |
    | Linux Virtual Machine | Simple Volume | The {{site.data.keyword.baas_full_notm}} detects mount points for entries in the `/etc/fstab` file with the following formats:<br><br>UUID=ccd1d599-e68e-4b88-ba9b-6f75b63f1bdc /mnt ext4 auto 0<br><br>UUID="ccd1d599-e68e-4b88-ba9b-6f75b63f1bdc" /mnt ext4 auto 0<br><br>If the {{site.data.keyword.baas_full_notm}} can detect a mount point, it indexes and displays files and directories in the volume with the mount point that was specified in the `/etc/fstab` file. For these example entries, files and directories are indexed with the `/mnt` mount path, such as `/mnt/example/test.txt`.<br><br>If the {{site.data.keyword.baas_full_notm}} cannot detect a mount point, the {{site.data.keyword.baas_full_notm}} indexes the files and directories into a `lvol_N` directory. For example, the `/mnt/example/test.txt` file is indexed as `/lvol_1/example/test.txt`. |
    | Linux Virtual Machine | LVM Volume | The {{site.data.keyword.baas_full_notm}} detects mount points for entries in the `/etc/fstab` file with the following formats:<br><br>UUID=ccd1d599-e68e-4b88-ba9b-6f75b63f1bdc /mnt ext4 auto 0<br><br>/dev/mapper/VG1-root /mnt ext4 defaults 1 1<br><br>/dev/VG1/root /mnt ext4 defaults 1 1  <br><br>If the {{site.data.keyword.baas_full_notm}} can detect a mount point, it indexes and displays files and directories in the volume with the mount point specified in the `/etc/fstab` file. For these example entries, files and directories are indexed with the `/mnt` mount path, such as `/mnt/example/test.txt`.<br><br>If the {{site.data.keyword.baas_full_notm}} cannot detect a mount point, the {{site.data.keyword.baas_full_notm}} indexes the files and directories into a `lvol_N` directory. For example, the `/mnt/example/test.txt` file is indexed as `/lvol_1/example/test.txt`. |
    | Linux Physical | LVM Volume | The Backup agent can only return mount data when the volume is mounted on the Linux physical Server. If the volume is mounted, the {{site.data.keyword.baas_full_notm}} indexes and displays files and directories in the volume with the mount point such as `/mnt/example/test.txt`.<br><br>If the volume is not mounted, the {{site.data.keyword.baas_full_notm}} indexes the files and directories into a `lvol_N` directory. For example, the `/mnt/example/test.txt` file is indexed as `/lvol_1/example/test.txt`. |
    {: caption="" caption-side="bottom"}

4. Once you select the files and folder to recover, select one of the following options:

    *   **Next Recover Option**: If you select this option, then continue with the step 5 to configure the file recovery options.

    *   **Download Files**: If you are recovering a single file, this option downloads the file to your browser’s download folder. For all other selections, this creates a recover task. When the task completes, from the Recoveries page, click the task name and then click **Download Files** to download the generated zip file.

        [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)Character replacement explanation for : and \\ in file and directory names in zip files](#)

        If the original file or directory name contains a ‘:’ , it is replaced with ‘\_COLON\_’ in the archive.
        Example:

        *   The original name is "a:b" then the archived name is "a\_COLON\_b"
        *   The original name is "a:b:c" then the archived name is "a\_COLON\_b\_COLON\_c"

        If the original file or directory name contains a ‘\\’, it is replaced with '\_BACK\_SLASH\_' in the archive. 
        Example:

        *   The original name is “x\\y" then the archived name is "x\_BACK\_SLASH\_y"
        *   The original name is "x\\y\\z" then the archived name is "x\_BACK\_SLASH\_y\_BACK\_SLASH\_z"

5. From the **Recover To**, select **Original Server** to recover the files or folders to a Original server.

6. To recover to a different location in the original server, disable the **Recover to Original Path** option, and then provide the location to which the files or folders are to be recovered in the **Recover To** field. By default, the files and folders will be recovered to the original location.

7. Optional. Edit the default settings of the following **Recovery options**:

    *   **Overwrite Existing File/Folder**: By default, this option is disabled to create the files and folders in the specified location. Enable this option to overwrite the existing files and folders. Any duplicate files are skipped.

        Example for a Windows alternate location with **Overwrite Existing File/Folder** enabled and the target already exists:

        Files to be recovered: "C:\\foo\\1.txt", "C:\\foo\\test\\1.txt", "C:\\bar", "C:\\foo\\bar". Specified alternate location: "C:\\restore". At the end of the copy, the target folder looks like this:

        *   C:\\restore\\1.txt

        *   C:\\restore\\backup-recovery<taskid>\_0\\1.txt

        *   C:\\restore\\bar

        *   C:\\restore\\backup-recovery<taskid>\_1\\bar


        If **Overwrite Existing File/Folder** is enabled, recovering a file to source when the file is in Fuse may cause the open file to be overwritten. Whether overwriting occurs depends on the application using the file.

    *   **Preserve File/Folder Attributes**: By default, this option is enabled and the ACLs, permissions, and timestamps are preserved for all files and folders. If you disable this option, then ACLs and permissions are not preserved. If recovering both folders and files, then folders will receive the new timestamps, but files retain their original timestamps. If recovering only, files, then files will receive the new timestamps.

    *   **Continue on Error**: Enable this option if you want to continue the recovery even if one of the objects encounters an error. By default, this option is disabled and the recovery operation will fail if one of the objects encounters an error.

    *   **Cluster Interface**: By default, the **Auto Select** option is enabled and the recovery task automatically selects the correct VLAN. If you disable this option, then select a configured interface group from the Interface Group drop-down.

    *   **Task Name**: Change the default name of the recovery task.

    *   **Save Success Files**: Enable this option to download the success logs for a recovery run.

8. Click **Start Recovery**.


The recovery task is initiated. You can monitor the recovery task from the Recoveries page.

## Download list information for a recovery run
{: #download_list_information_for_a_recovery_run}

System-generated lists are used to verify success or errors during recovery. You can now download the list information as a CSV file for both successes and failed recovery job runs on the {{site.data.keyword.baas_full_notm}}.

To download the logs for a recovery run:

1. Select **Data Protection** > **Recoveries** > **Recover**.

2. On the **Recoveries** page, select the recovery run for which you want to download the debug logs.

    1. If you select a successful recovery run, on the Recovery Details page, select the **Show Subtask** button and then click **Download Success** Logs\*.

    2. If you select a failed recovery run, on the Recovery Details page, select the **Show Subtask** button and then click **Download Errors**.

    3. If the recovery run is partially successful, select the **Show Subtask** button and then click either the **Download Errors or Download Success Logs**\*.

        \* You must enable the **Save Success Files** option under **Recovery** Options while creating a recovery job to be able to download the success logs for a recovery run.

