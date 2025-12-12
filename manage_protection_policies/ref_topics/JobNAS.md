---

copyright:
  years: 2024
lastupdated: "2025-12-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protect nas
{: #protect_nas}

NAS Protection Group captures snapshots and these snapshots are required for the following tasks:

*   Recover volumes
*   Replicate snapshots
*   Archive snapshots
*   Files and folders recovery (CloudArchive Direct)


*   {{site.data.keyword.baas_full_notm}} recommends a first full and incremental forever backup approach to back up your NAS sources.

*   NAS backup and restore do not support Junction points and ADS (Alternate Data Stream).


## Before you begin
{: #before_you_begin}

When you create a Protection Group, you can select an existing source, policy, or storage domain. You can also create them while creating the Protection Group. However, you might find it easier to create them prior to creating the Protection Group, as described in the following topics.

*   [Register or Edit NAS](SourceNASAdd.htm)
*   [Create or Edit a Standard Policy](PolicyCreateEdit.htm)
*   [Create or Edit Storage Domains](../Platform/ViewBoxesCreateEdit.htm)

## Create a protection group for nas volumes
{: #create_a_protection_group_for_nas_volumes}

1. Navigate to **Data Protection > Protection** and do one of the following:

*   To create a Protection Group—Click **Protect** on the top-right of the page and then select the type of Protection Group.
*   To edit a Protection Group—Click the action icon (![](../../Resources/Images/action_menu.png)) next to the Protection Group and select **Edit**.

3. If creating a new Protection Group, select **Protect > NAS**.
4. In the **New Protection** pop-up, do one of the following:

    *   Protect the NAS volumes database by specifying a subset of options such as objects, protection group and policy by clicking the edit ![](../../Resources/Images/i/icn/edit-d.svg) icon corresponding to the following options:

        1. **Add Objects**: Select the registered NAS source or click **Register Source** to register a NAS source. From the **Objects** list, select the volumes you want to back up.

        2. **Protection Group**: Specify a name for the Protection Group.

        3. **Policy**: Select an existing policy or click **Create Policy** to create a new protection policy.

        4. **Storage Domain**: Select the appropriate storage domain.

            This option is visible on the **New Protection** pop-up only if there are more than one storage domain.

        5. Click **Protect**.

    *   To protect NAS volumes by specifying all the options, click **More Options**.

5. Click **Add Objects** and from the **Registered Source** drop-down list, select the NAS source that contains the volumes you want to back up. You can also register a new source with the {{site.data.keyword.baas_full_notm}} so you can protect it.

6. From the **Objects** list, select the volumes to back up. You can select volumes that are of NFS exports, SMB shares or the volumes that are exposed to both NFS and SMB protocols (mixed volumes). By default, all volumes in the source are excluded as indicated by the ![](../../Resources/Images/checkboxUnselected.png) unselected check box. Select the volumes you want to include in the Protection Group. A selected check box (![](../../Resources/Images/checkboxSelected.png)) indicates the volume is selected to be backed up.

    [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)If appropriate for your organization, enable Auto Protect.](#)

    Click![](../../Resources/Images/i/icn/autoprotect-on.svg)to turn on Auto Protect. Turn on this option to protect new volumes that are added to a selected parent SVM. When a parent SVM is selected and this option is enabled, all volumes that are added to the SVM in the future are automatically added to this Protection Group and become protected on the next run. For example, you create a Protection Group that backs up a SVM. The Protection Group runs for two weeks and then a new volume is added to the SVM. The next time the Protection Group runs, if the object hierarchy has been refreshed on the {{site.data.keyword.baas_full_notm}}, the new volume is also backed up even though the new volume has not been explicitly selected to be included in the Protection Group. The object hierarchy is automatically refreshed every four hours. To manually refresh the object hierarchy, select **Data Protection > Sources**, find the source in the list and click ![](../../Resources/Images/i/icn/refresh-h.svg). To exclude an object from Auto Protect, click ![](../../Resources/Images/autoprotect-checkbox.png).

    *   ![](../../Resources/Images/autoprotect-checkbox.png) Indicates an Auto Protected object.
    *   ![](../../Resources/Images/autoprotect-checkbox-excluded.png) Excluded objects are not protected. The icon indicates that an ancestor of the object is Auto Protected but this object is explicitly excluded.


[![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)You can optionally filter the volumes displayed.](#)

*   In the Search field, enter part of volume or SVM name.
*   In the **Show All** drop-down, select one of the following options:
    *   **Selected objects**—Displays the volumes that are currently selected for protection.
    *   **Unprotected**—Displays the volumes that are not protected by any Protection Groups in the {{site.data.keyword.baas_full_notm}}—not protected by {{site.data.keyword.baas_full_notm}}.
    *   **Protected** —Displays the volumes that are protected by all the Protection Groups in the {{site.data.keyword.baas_full_notm}}.
*   You can also filter with the following options:
    *   **NFS** -Displays NFS volumes only.

        For Generic NAS and NetApp ONTAP sources, **NFSv3** and **NFSv4.1** filters are supported from {{site.data.keyword.baas_full_notm}} 6.6.0d version onwards. To enable this feature, contact [{{site.data.keyword.baas_full_notm}} Support](/docs/backup-recovery?topic=backup-recovery-troubleshooting-cos){: external}t.

    *   **SMB** - Displays SMB volumes only.

*   Selecting the check box adjacent to **Volumes** selects all of the filtered volumes.


[![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)The graphical representations are described below.](#)

*   If you are creating a new Protection Group and the![](../../Resources/Images/i/icn/protected-source.svg) shield appears next to the volume, this volume is protected by another Protection Group.

    ![](../../Resources/Images/ObjectProtectedNAS.png)

*   If you are editing an existing Protection Group and the ![](../../Resources/Images/i/icn/protected-source.svg) shield appears next to the volume and the volume is selected (![](../../Resources/Images/checkboxSelected.png)), it will be protected by the current Protection Group. (It may also be protected by another Protection Group.)

    ![](../../Resources/Images/ObjectProtectedSelectedNAS.png)

    [![](../../Resources/Images/CreateBackupJobProtectShieldNASEdit_thumb_0_48.png)](../../Resources/Images/CreateBackupJobProtectShieldNASEdit.png)

*   Click the ![](../../Resources/Images/i/icn/view.svg) icon to see details about the object.


11. Select the backup preference mode of the volume you want to backup.

12. Click **Continue** to save and go back to New Protection page.
13. **Protection Group**: Enter a name for the Protection Group. The name can contain alphanumeric characters, underscores, dashes and periods. This field is required.
14. **Policy:** Select an existing protection policy or create a new one by selecting **New Policy**.

15. **Storage Domain:** Select an existing storage domain or create a new one by selecting **New Storage Domain**.

16. If you want to change or configure any of the additional settings , then select **More Options** and perform the below steps or else, Click **Protect**.

17. Optionally, from the **Protection Group** field, you can select an existing Protection Group settings for this Protection Group by enabling **Existing Group**, and then by selecting an existing Protection Group from the **Group** drop-down list.

18. **Start Time:** Indicates when the Protection Group should run. The current time is displayed by default but you can change it. Enter the hour and minutes or use the up and down arrows on your keyboard. Verify the AM or PM setting.

    The default time zone is the browser's time zone. You can change the time zone by selecting a different time zone.

19. **End Date:** Optional. Toggle on and select the date when the Protection Group stops capturing snapshots. A protection run that starts prior to this date runs until completion even if it completes after this date.

20. **QoS Policy:** Select an appropriate quality of service (QoS) policy.

    {{site.data.keyword.baas_full_notm}} recommends specifying **Backup HDD**, which is the default.

    *   **Backup HDD**: The {{site.data.keyword.baas_full_notm}} writes the data directly to an HDD drive for this Protection Group.
    *   **Backup SSD**: The {{site.data.keyword.baas_full_notm}} writes the data directly to an SSD drive for this Protection Group. Only specify this policy if you need fast ingest speed for a small number of Protection Groups.
    *   **Backup Auto**: The {{site.data.keyword.baas_full_notm}} writes the data to both SSDs and HDDs. The distribution of data will be based on the current usage of SSD and HDD. This policy tries to achieve similar backup performance as the **Backup SSD** policy and reduces the SSD wear-out compared to the Backup SSD policy.
    *   **TestAndDev High:** The {{site.data.keyword.baas_full_notm}} writes the data to an SSD drive for this Protection Group. The I/Os with this QoS policy are given higher priority compared to other QoS policies. Use this policy for workloads that require lower I/O latency.

    

21. **Pre & Post Scripts:** Toggle **Enable** to run scripts on a Unix server before and/or after a Protection Group runs. If configured, scripts are run every time an object (or entity) is backed by a protection run. For example if there are five mount/shares that are backed up as part of the NAS Protection Group and the Pre Script is configured, then five instances of the Pre Script is executed in parallel. You can distinguish between the scripts that are run on the different objects (entities) using the `COHESITY_BACKUP_ENTITY` environment variable. The {{site.data.keyword.baas_full_notm}} populates this environment variable with the name of the backed object (or entity).

    The specified Unix server does not need to be attached to the NAS target, it just needs to be a Linux VM or machine that can either invoke APIs on or ssh in to a NAS target.

    [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)Configure the settings for running the Pre and Post scripts.](#)

    **Hostname or IP Address (Unix only):** Specify either the hostname or the IP address of the remote Unix server where the script will run. (Only Unix is currently supported.) 

    **Cluster SSH Public Key:** Click **Copy Key to Clipboard** to copy the entire string including the `ssh-rsa` string preceding the key, the public key and the `username@cluster_name` string after the key. Open the `authorized_keys` file (typically located in the `~/.ssh` directory) and paste the entire string to the end of the file.

    **Username:** Specify the username on the remote Unix machine that will execute the script. The username must match the username specified in the `authorized_keys` file.

    You can configure a Pre-Script, a Post-Script or both scripts.

    **Pre-Script**

    *   **Script Full Path:** Specify the full path and name of the script to run. A script matching the specified name must be available in the specified directory. The script can be in any format, such as a shell script or executable. The script must be configured with the permissions to allow execution (`+x`).

        {{site.data.keyword.baas_full_notm}} does not validate the content of the script and is not responsible for the content of the script or any actions done by the script. You must verify the script actions before a script or executable is run.

    *   **Script Params**: Optional field for passing one or more parameters to the script. All specified parameter values are passed as a single string to the script.

    *   **Timeout**: Optional field for specifying how long a script is allowed to execute before the {{site.data.keyword.baas_full_notm}} stops the execution of the script. By default, the timeout is 15 minutes. If no value is specified, there is no timeout and the script is not stopped. This can cause a protection run to hang if the script never completes.

    *   **Continue Backup if script fails**: This toggle applies only to Pre-Scripts and is enabled by default. When this toggle is enabled, the Protection Group back ups the objects even if the Pre-Script fails. If this toggle is disabled and the Pre-Script fails, the backup is canceled but the Post-Script still runs.


    **Post-Script**

    *   **Script Full Path:** Specify the full path and name of the script to run. A script matching the specified name must be available in the specified directory. The script can be in any format, such as a shell script or executable. The script must be configured with the permissions to allow execution (`+x`).

        {{site.data.keyword.baas_full_notm}} does not validate the content of the script and is not responsible for the content of the script or any actions done by the script. You must verify the script actions before a script or executable is run.

    *   **Script Params**: Optional field for passing one or more parameters to the script. All specified parameter values are passed as a single string to the script.

    *   **Timeout**: Optional field for specifying how long a script is allowed to execute before the {{site.data.keyword.baas_full_notm}} stops the execution of the script. By default, the timeout is 15 minutes. If no value is specified, there is no timeout and the script is not stopped. This can cause a protection run to hang if the script never completes.


    More about failures, cancellations and logging:

    *   If the backup is successful but the Pre-Script or Post-Script fails, a warning is issued with the script error. A script failure occurs for any of the following conditions: 

        *   script is not found in the expected location

        *   script cannot be executed

        *   script returns a failure return code of non-zero


        A script is successful when the return code is 0.

    *   If a Post-Script is configured, the script always executes even if the backup fails.
    *   If a protection run is canceled, all subsequent phases are canceled.
        *   For example, if a protection run is canceled while the Pre-Script is running, the Pre-Script is canceled, then the backup task is canceled and then the Post-Script is canceled.
        *   For example, if a protection run is canceled while the backup task is running, the backup task is canceled and then the Post-Script is canceled.
    *   The logging information reported by the script is available from the Protection Group details page. To open the page, log in to the {{site.data.keyword.baas_full_notm}} Console, select **Data Protection > Protection**, and under **Last Run** click on a date. In the Run Details page, click the green `>` icon next to the protected object (entity) to view the job details.

    **Available Environment Variables**

    The {{site.data.keyword.baas_full_notm}} populates the following environment variables with values before the Pre-Script or the Post-Script is run. You can use these environment variables in your script.


    | Variable  <br>Name | Available from Script | Description |
    | --- | --- | --- |
    | `COHESITY_JOB_ID` | Pre-Script  <br>and  <br>Post-Script | Specifies the {{site.data.keyword.baas_full_notm}} id assigned to the Protection Group. |
    | `COHESITY_JOB_NAME` | Pre-Script  <br>and  <br>Post-Script | Specifies the name of the Protection Group as displayed in the {{site.data.keyword.baas_full_notm}} Console. |
    | `COHESITY_BACKUP_TYPE` | Pre-Script  <br>and  <br>Post-Script | `kRegular` indicates an incremental (CBT) backup. Incremental backups utilizing CBT (if supported) are captured of the target protection objects. The first run of a kRegular schedule captures all the blocks.<br><br>`kFull` indicates a full (no CBT) backup. A complete backup (all blocks) of the target protection objects are always captured and Change Block Tracking (CBT) is not utilized.<br><br>`kLog` indicates a database log backup. Capture the database transaction logs to allow rolling back to a specific point in time.<br><br>`kSystem` indicates a system backup. System backups are used to do bare metal recovery of the system to a specific point in time. |
    | `COHESITY_BACKUP_ENTITY` | Pre-Script  <br>and  <br>Post-Script | Specifies the name of the object (or entity) being backed up that triggered the script execution. |
    | `COHESITY_BACKUP_HOST_ENTITY` | Pre-Script  <br>and  <br>Post-Script | Specifies the name of the host that contains the object (or entity) being backed up that triggered the script execution. |
    | `COHESITY_CLUSTER_IP_ADDRESSES` | Pre-Script  <br>and  <br>Post-Script | Specifies a comma separated list of IP Addresses of the nodes that make up the {{site.data.keyword.baas_full_notm}}. |
    | `COHESITY_JOB_STATUS` | Post-Script | SUCCEEDED indicates the protection run ran successfully.<br><br>ERRORED indicates the protection run reported an error. |

22. **Skip Files on Errors:** Toggled on by default. The Protection Group continues to run even if it encounters errors on files, such as permissions errors. If files are skipped, the protection run details page indicates a warning status and provides additional information. If toggled off, the Protection Group stops when it encounters an error.
23. **Encryption**: Toggle on this option to enable in-flight encryption of data traffic between the {{site.data.keyword.baas_full_notm}} and external NAS source while backing up NFS AND SMB Volumes. This option is disabled by default.

    Encrypting in-flight data will have a minor impact on the performance. To use the encryption functionality for NFS volumes, you must join the {{site.data.keyword.baas_full_notm}} to the active directory as the Kerberos server or add the Kerberos provider in the {{site.data.keyword.baas_full_notm}} [Access Management](../Admin/AccessManagement.htm) section.

24. **File DataLock**: Toggle on this option to preserve the AccessTime of the files and folders of the selected volumes. The AccessTime provides the lock period of the files and folders that are applied when these files and folders are recovered to the {{site.data.keyword.baas_full_notm}} View. For more information on File DataLock, see [NAS File DataLock](../../NAS/NASFileDataLock.htm).

    Select one of the following **DataLock** modes:

    *   **Compliance**: In this mode, files and folders cannot be deleted or modified during the retntion period. After the lock expires, no user can modify the files folder. However, a root user can delete the files and folders. Only a user with Data Security role can edit the File DataLock settings in the Compliance mode.

    *   **Enterprise**: In this mode, files and folders cannot be modified by any user before or after the lock retention period expiration. However, a root user can delete the files and folders before or after the lock retention period. An admin user or a user with a Data Security role can edit the File DataLock settings in the Enterprise mode. This mode is useful for giving storage administrators additional control over the file.


    You can further configure the File DataLock settings to:

    *   [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)Set the lock period for the new files and folders created on the recovered Cohesity View](#)

        Provide the following information to set the lock period for the new files and folders that will be created on the recovered {{site.data.keyword.baas_full_notm}} View. These settings are not applicable for the existing NAS source files and folders recovered on the {{site.data.keyword.baas_full_notm}} View.

        *   **Default Lock Period**: Provide the time period during which the files should remain locked. When the time period ends, files can be deleted but not changed. By default, the files are locked forever.

        *   **Autolock Files**: By enabling this option, the files are automatically locked if they are not modified in the time period you specify. By default, this option is disabled.

        *   **Manually Lock Using**: Select when the files are to be locked:

            *   **Read-Only**: Select this option to lock files when the file permission is set to read only, for example; by setting the Windows file properties attribute or using the Linux chmod command. By default, the files are locked when the file permission is set to read only.

            *   **FutureATime**: Select this option to lock files when the last access time (atime) is set, for example, by using Windows PowerShell commands or the Linux touch command.

                Optionally you can set a Minimum Retention and/or a Maximum Retention period to ensure that a file's last access time cannot be set outside this period. A request to do so results in a permission denied error if manual locking mode is FutureATime. Otherwise, the request will be silently rejected and the file will be locked for the minimum retention duration.


    *   [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)Override the locking period of the existing files and folders recovered on the Cohesity View:](#)

        The following option can be applied to all the existing files and folders that will be recovered on the {{site.data.keyword.baas_full_notm}} View.

        *   **Override (Lock all files until a specific date )**: Enable this option and select a date to override the locking period of the existing files and folders.

            When you recover the files to the {{site.data.keyword.baas_full_notm}} View, {{site.data.keyword.baas_full_notm}} applies the farthest lock period between the override lock period you specify and the source file's lock period.

            For example;

            *   The lock period of the source file is 09/09/2023 and the override lock period you specified is 09/08/2022. When you recover the file to the {{site.data.keyword.baas_full_notm}} View, then the lock period of the source file is applied to the recovered file, which is 09/09/2023.
            *   The lock period of the source file is 09/09/2022 and the override lock period you specified is 09/08/2024. When you recover the file to the {{site.data.keyword.baas_full_notm}} View, then the override lock period you specified is applied to the recovered file, which is 09/08/2024.

            In **Compliance** mode, a user with Data Security privileges can extend the override date into the future, but no user can shorten the date.

            In **Enterprise** mode, a user with admin or Data Security privileges can extend or shorten the override date.



    *   You cannot enable or disable File DataLock once the Protection Group is created.

    *   If you have disabled File DataLock while creating a Protection Group, then when you edit this Protection Group, the File DataLock option will not be displayed.

    *   You can delete Protection Groups and snapshots having File DataLock enabled.

        File DataLock also preserves the ATimes of the hardlinks and symlinks.


25. **Filter IPs**: Enable this option to filter the IP addresses of the NAS source. By filtering IP addresses, you can restrict the communication of {{site.data.keyword.baas_full_notm}} to specific IP addresses or subnets of the NAS source.

    To filter the IP addresses of the NAS source, select one of the following options:

    *   **Allow IPs**: Select this option and specify the IP addresses of the NAS source through which the communication to {{site.data.keyword.baas_full_notm}} must happen. You can provide the IP addresses in a comma-separated list or in a CIDR format.
    *   **Deny IPs**: Select this option and specify the IP addresses of the NAS source through which the communication to {{site.data.keyword.baas_full_notm}} must not happen. You can provide the IP addresses in a comma-separated list or in a CIDR format.

    *   This option does not apply to the NAS Generic Mount Point Protection Group creation.

    *   If you have specified the IP filtering at the source-level also, then the IP filtering specified at the Protection Group\-level will take precedence over the IP filtering specified at the source-level.

26. **Exclusions and Inclusions:** Everything is included by default. Toggle on **Exclusions and Inclusions** if you want to exclude or include locations. By creating exclusion and inclusion rules, you can limit the Protection Group to a specific set of files and directories and therefore minimize the disk space used to store the data.

    To add an exclusion or inclusion, you must prefix a forward slash (‘/’) or suffix an asterisk (‘\*’) to the path or to a particular file within the protected object. For example, ‘**/test**’ or ‘**\*.txt**’.

    **Add Inclusion**: Click to include a particular path or a particular file within the protected object.

    For example, consider four directories - **test**, **test1**, **test2**, and **test3** under the protected object with path `/ifs/TestShare1/Folder1`. The table below lists the input types for an inclusion list of folders and their respective outcome:


    | Input | Outcome |
    | --- | --- |
    | /   | Everything inside the protection path is backed up, if exclusions are not defined. |
    | /test | When inclusion path is not suffixed by '/', Protection Group will include all folders starting from protection path.<br><br>In this example, /test will behave as /test\* and Protection Group will include '**test**', '**test1**', '**test2**’, and '**test3**' from protection path for backup. |
    | /test/ | When inclusion path is suffixed by '/' , it will only include specific folder.<br><br>In this example, /test/ behaves as /test/\* and Protection Group will only include '**test**' from protection path for backup. |

    Do not provide the folder name along with the backup path while including files and folders for a Protection Group. The backup path is considered by default. For example, `/ifs/TestShare1/Folder1/test` does not include any folder.

    If exclusions are defined, they will take precedence over inclusions.

    **Add Exclusion**: Click to exclude a particular path or a particular file within the protected object. You can also specify regular expressions for excluding files in the format `regex:<regex pattern>`

    For example, consider there are three directories-**Test**, **Test1**, and **Folder1** and a file, **TestFile.tmp** under the protected object with path **/ifs/TestShare1**.

    *   **Test1** folder has **TestFile1.tmp**, **TestFile2.tmp** files.

    *   **Folder1** folder has **TestFile3.tmp**, **TestFile4.tmp files**, and **SubFolder1** which further has a **Test** folder with **File.txt**, **File1.txt** files in it.


    The following figure illustrates the directory structure under the protected object with path **/ifs/TestShare1**.

    [![](../../Resources/Images/NAS/NASExlcusion_thumb_0_48.png)](../../Resources/Images/NAS/NASExlcusion.png)

    The table below lists the input types for an exclusion list of folders and their respective outcome:


    | Input | Description | Outcome |
    | --- | --- | --- |
    | /Test | When the exclusion path is not suffixed by a '**/**', the Protection Group will exclude all files and folders with **Test** as the prefix . For exclusions, **/Test** behaves as **/Test\***.<br><br>Exclusion will not be effective on the sub-directory level. | Excludes **Test** and **Test1** folders, and **TestFile1.tmp** file from the protection path.<br><br>**TestFile3.tmp** and **TestFile4.tmp** files in the **Folder1** folder will not be excluded. |
    | /Test/ | When the exclusion path is suffixed by '**/**' , the Protection Group excludes only the files and folders with the name **Test**. For exclusions, **/Test/** behaves as **/Test/\***.<br><br>Exclusion will not be effective on the sub-directory level. | Excludes the **Test** folder of the parent directory.<br><br>The **Test** sub-folder in **/Folder1/SubFolder1** will not be excluded. |
    | \*/Test | Excludes all the files and directories with the name '**Test**'.<br><br>Exclusion will be effective on all levels of sub-directories. | Excludes the **Test** folder of the parent directory and the **Test** sub-folder in **/Folder1/SubFolder1**. |
    | \*.tmp | Excludes all the files that with ‘**.tmp**’.<br><br>Exclusion will be effective on all levels of sub-directories. | Excludes **TestFile.tmp**, **TestFile1.tmp**, **TestFile2.tmp**, **TestFile3.tmp**, **TestFile4.tmp** files. |
    | regex:.\*\\.tmp | Excludes the files and folders with ‘**.tmp**’.<br><br>Exclusion will be effective on all levels of sub-directories. | Excludes **TestFile.tmp**, **TestFile1.tmp**, **TestFile2.tmp**, **TestFile3.tmp**, **TestFile4.tmp** files. |
    | regex:/Folder1/.\*/Test/.\*\\.txt$ | Excludes all files with **.txt** extension from all the **Test** folders directory in **/Folder1/**. | Excludes **File.txt** and **File1.txt** files in the **/Folder1/SubFolder/Test** folder. |

    Do not provide the folder name along with the backup path while excluding files and folders for a Protection Group. The backup path is considered by default. For example, `/ifs/TestShare1/Folder1/test` does not exclude any folder.

    {{site.data.keyword.baas_full_notm}} automatically excludes the following NetApp system files:

    *   .vtoc\_internal and .bplusvtoc\_internal files
    *   .copy-offload directory and .tokens file
27. **Indexing**: Toggle on this option to index the data of the Protection Group. Indexing improves files and folders search.

    {{site.data.keyword.baas_full_notm}} performs incremental indexing where only the data that has changed since the previous snapshot is scanned and indexed.

    [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)You can add custom indexing rules.](#)

    By default, a rule to include the root directory (`/`) is defined and therefore by default the entire file system is indexed. When a directory is indexed, all of the child sub-directories are also indexed. (Indexing is recursive.)

    By creating exclusion and inclusion rules, you can limit indexing to a specific set of files and directories and therefore minimizing the disk space used to store the index. For example, you could add two additional rules to the first default rule:

    *   include `/`

    *   exclude `/var`

    *   include `/var/log`


    In this example, all files in the `/` root directory are indexed except the files found in the `/var` directory. However, the files in the `/var/log` are indexed because there is an explicit include rule.

    For notes about indexing support, see [Indexing and File Recovery](../../ReleaseNotes/considerations.htm#Indexing) .

28. **Quiet Time:** This option is available only if the selected policy has at least one quiet time period. Toggle the **Stop in-progress runs when quiet time starts** option to pause or abort the runs during quiet time. If this option is toggled off, after a protection run starts, execution continues even when the quiet time period specified for this Protection Group starts. However, a new protection run will not start during a quiet time period.

    *   **Pause Runs** - Select this option to pause the currently executing protection runs and resume the runs when quiet time ends.

    *   **Abort Runs** - Select this option to abort the currently executing protection runs when the quiet time period specified for the Protection Group starts.

        If the quiet time window for multiple protection runs is scheduled at the same time, then when the protection run is resumed, prioritization of the protection run does not work

29. **Alerts:** Optional. Select one or more of the following settings if you want alerts to be created for the following triggers:

    *   **Success:** Create an informational alert when a Protection Group completes successfully. Emails are not sent when informational alerts are created.
    *   **Failure:** Create a critical alert if the Protection Group fails to complete. Emails are sent when critical alerts are created.
    *   **SLA Violation:** Create a warning alert if the Protection Group takes longer than the time period specified in the SLA field. Emails are sent when warning alerts are created.

    For more information, see [Alerts](../Monitoring/Alerts.htm#top).

30. **Priority:** Select a priority for the Protection Group execution. IBM Cloud Supports concurrent backups, but if the number of Protection Groups exceeds the ability to process them, those with **High** priority are given the highest priority to execute, **Medium** the second-highest priority, and **Low** the lowest priority.
31. **Email Recipients**: You can add email addresses to a Protection Group to notify the email recipients when alerts are triggered for the Protection Group.


    The connection to the SMTP Server must be enabled to send email notification. You can enable the SMTP Server connection in the configuration settings page ( {{site.data.keyword.baas_full_notm}} UI > **Settings** > **Summary** > **Configure** > **Enable SMTP Server**).

32. **SLA:** The service-level agreement (SLA) defines the expected time to complete either a full backup or incremental backup. The backup completion time depends on the Windows volume size on AD, objects changed between incremental backups, network speed, and other Protection Groups running on the {{site.data.keyword.baas_full_notm}}.

    *   **Incremental:** Enter the number of minutes you expect an incremental protection run to complete. An incremental backup captures only the differences (changed blocks) since the last protection run.
    *   **Full:** Enter the number of minutes you expect a full backup protection run to complete. A full backup captures the entire object (all blocks).
33. **Description:** Optional. Click the plus sign and enter a description for the Protection Group.
34. Click **Protect**.

The Protection page is displayed and includes the new Protection Group.
