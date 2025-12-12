---

copyright:
  years: 2024
lastupdated: "2025-12-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Recover Physical Server Files or Folders
{: #Recover Physical Server Files or Folders}


{{site.data.keyword.baas_full}} provides the ability to recover files and folders from a Snapshot created earlier by a Protection Group. Files and folders can be recovered to their original location (limitations apply to physical Servers and specific operating systems) or to a newly specified location, which can be within the original Source or a different one. You can choose to retain the recovered files' and folders' original (at the time of the backup) permissions and attributes. You can also download files and folders from selected Snapshots that were created by a {{site.data.keyword.baas_full_notm}} Protection Group. However, only items that were indexed when the Snapshot was created can be downloaded. Recovering and searching for files and folders in Snapshots created by other backup software is not supported. To learn more, [Add or Edit a Protection Group for Physical Servers](/docs/backup-recovery?topic=backup-recovery-add_or_edit_a_protection_group_for_physical_servers).

To learn more, see [About File Recovery](../../Concepts/RecoverFileConcept.htm).

For more information about the recovery options:

*   [Recover Files or Folders to the Original Location](/docs/backup-recovery?topic=backup-recovery-recover_physical_server_files_or_folders_to_the_original_location)

*   [Recover Physical Server Files or Folders to a New Location](/docs/backup-recovery?topic=backup-recovery-recover_physical_server_files_or_folders_to_a_new_location)


Note the following about indexing and recovering files:

*   The {{site.data.keyword.baas_full_notm}} attempts to index all files and folders to a drive on both Windows and Linux systems. If the {{site.data.keyword.baas_full_notm}} is unable to find mount point information about files or directories, it indexes and displays these files and directories in the `lvol_N` directory, where `N` is a unique number such as `1`.

    On Windows systems, if the {{site.data.keyword.baas_full_notm}} finds the mount point information about files and directories, it indexes and displays these files and directories with a drive letter such as `C:`.

    Linux LVM indexing supports the following LVM types only: Linear, Striped, Mirrored, Mirrored + Striped, Thin. On Linux systems, how files and directories are indexed and displayed is dependent on the conditions specified in the following table.

    | Server Type | Volume Type |     |
    | --- | --- | --- |
    | Linux Virtual Machine | Simple Volume | The {{site.data.keyword.baas_full_notm}} detects mount points for entries in the `/etc/fstab` file with the following formats:<br><br>UUID=ccd1d599-e68e-4b88-ba9b-6f75b63f1bdc /mnt ext4 auto 0<br><br>UUID="ccd1d599-e68e-4b88-ba9b-6f75b63f1bdc" /mnt ext4 auto 0<br><br>If the {{site.data.keyword.baas_full_notm}} can detect a mount point, it indexes and displays files and directories in the volume with the mount point that was specified in the `/etc/fstab` file. For these example entries, files and directories are indexed with the `/mnt` mount path, such as `/mnt/example/test.txt`.<br><br>If the {{site.data.keyword.baas_full_notm}} cannot detect a mount point, the {{site.data.keyword.baas_full_notm}} indexes the files and directories into a `lvol_N` directory. For example, the `/mnt/example/test.txt` file is indexed as `/lvol_1/example/test.txt`. |
    | Linux Virtual Machine | LVM Volume | The {{site.data.keyword.baas_full_notm}} detects mount points for entries in the `/etc/fstab` file with the following formats:<br><br>UUID=ccd1d599-e68e-4b88-ba9b-6f75b63f1bdc /mnt ext4 auto 0<br><br>/dev/mapper/VG1-root /mnt ext4 defaults 1 1<br><br>/dev/VG1/root /mnt ext4 defaults 1 1  <br><br>If the {{site.data.keyword.baas_full_notm}} can detect a mount point, it indexes and displays files and directories in the volume with the mount point specified in the `/etc/fstab` file. For these example entries, files and directories are indexed with the `/mnt` mount path, such as `/mnt/example/test.txt`.<br><br>If the {{site.data.keyword.baas_full_notm}} cannot detect a mount point, the {{site.data.keyword.baas_full_notm}} indexes the files and directories into a `lvol_N` directory. For example, the `/mnt/example/test.txt` file is indexed as `/lvol_1/example/test.txt`. |
    | Linux Physical | LVM Volume | The Backup agent can only return mount data when the volume is mounted on the Linux physical Server. If the volume is mounted, the {{site.data.keyword.baas_full_notm}} indexes and displays files and directories in the volume with the mount point such as `/mnt/example/test.txt`.<br><br>If the volume is not mounted, the {{site.data.keyword.baas_full_notm}} indexes the files and directories into a `lvol_N` directory. For example, the `/mnt/example/test.txt` file is indexed as `/lvol_1/example/test.txt`. |
    {: caption="" caption-side="bottom"}

*    ![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif) When recovering a file or instantly mounting a volume from a Windows VM or Physical Server Backup Source that has Windows ​deduplication installed and enabled for one or more volumes, you must choose a target machine that also has Windows ​deduplication installed (it does not have to be enabled for any volume). (However, this rule does not apply to Nutanix AHV VMs. If AHV VMs are enabled with Windows deduplication, the only supported recovery option is full VM recovery.)(#)

    If the target does not have Windows deduplication installed:

    *   File level recovery might fail with robocopy error code 8 (if no file was recovered) or 9 (if some files were recovered and some failed).
    *   Instant volume mounting will succeed but you might not be able to browse the volume or access all of its contents.

    To determine if Windows deduplication is installed on the Source or target machine, follow the steps given below:

    1.  Open **Server Manager**.
    2.  Select **Roles and Features > File and Storage Services > File and iSCSI Services**.
    3.  Select the **Data Deduplication** check box, if necessary.
    4.  Click **Next** until the **Install** button is enabled and then click **Install**.

    [![](../../Resources/Images/WindowsDedup_thumb_0_100.png "Click to Enlarge")](../../Resources/Images/WindowsDedup.png)

*   When unzipping a zip file that was created by downloading files and folders from an archived Snapshot, if the file or folder name has encoded characters, unzip the zip file using the corresponding encoding. For example if a file name in the zip file has a UTF-8 character, unzip the file using the following command:

    unzip -O UTF-8 Download-Files\_Sep\_20\_2018\_3-17pm\_3090.zip

*   When recovering a Linux file, the Backup agent runs the following commands in sudo:

    *   mount                                                                                                                                                                                                                                                                                                                                                               
    *   umount
    *   findmnt
    *   timeout
    *   blkid
    *   lsof
    *   ls
    *   rsync
    *   losetup
    *   dmsetup
    *   lvs

    *   vgs
    *   lvcreate

    *   lvremove

    *   lvchange
*   Windows behavior (giving each user its own set of mapped networked drives) prevents recovering to an SMB share that was mounted by a different user.


**Considerations:**

*   For Linux Logical Volume Manager (LVM), if all the disks for a volume group are not found by the {{site.data.keyword.baas_full_notm}}, the {{site.data.keyword.baas_full_notm}} will not process that volume group. As a result of that, no volumes of this volume group will be recognized or indexed by the {{site.data.keyword.baas_full_notm}}.

*   File recovery is not supported for ReFS volumes in these environments: physical, VMware, Hyper-V and AHV.

*   Encrypted folders that have been renamed or deleted cannot be recovered.

*   Recovering files/folders with names longer than 200 characters may return an error.This limitation is due to Windows behavior when handling files/folders with long names.

*   Downloading files and folders from tape archive locations is not supported.

*   Recovering files and folders from VMs to physical servers and from physical servers to VMs is not supported.

*   The downloadable zip file can contain regular files and folders only; symlinks are not supported. When unzipping the downloaded files/folders, use a zip utility that supports the ZIP64 format.

*   For RHEL5, if LVM is not configured, running a protection job fails with the “No LVM volume found in idbmantasdev” error. For more information, see the [Registering or running a Linux protection job fails when no LVM volumes are detected](/docs/backup-recovery?topic=Registering-or-running-a-Linux-protection-job-fails-when-no-LVM-volumes-are-detected-with-kFsError-Failed-to-find-LVM-version-installed-on-system) Knowledge Base article.


Before you can recover a file from a Snapshot, the Snapshot must exist. To recover files from a Snapshot, the files must be indexed during the backup process. To learn more, [Add or Edit a Protection Group for Physical Servers](/docs/backup-recovery?topic=backup-recovery-add_or_edit_a_protection_group_for_physical_servers).
