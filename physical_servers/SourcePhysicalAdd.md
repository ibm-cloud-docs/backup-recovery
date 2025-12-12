---

copyright:
  years: 2024
lastupdated: "2025-12-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Register or Edit a Physical Server


Before registering a physical server with the {{site.data.keyword.baas_full_notm}}, you must download and install the Backup agent on each server you plan to register.

## Overview

1.  Download the Backup agent to the physical server.
2.  Install the agent on the physical server.

    Instructions for silent installation on Windows servers are available here: [Silently Install the Backup agent on Windows Physical Servers](/docs/backup-recovery?topic=backup-recovery-install_and_manage_the_agent_on_windows_servers).

3.  Register the physical server with the {{site.data.keyword.baas_full_notm}}.

## Download and Install the Agent

Installing the agent on a physical server allows it to be registered as a source with the {{site.data.keyword.baas_full_notm}}.

1.  Select **Data Protection > Sources**.
2.  Click **Download Backup agent**.
3.  Follow the installation instructions for the corresponding agent operating system:

    [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)Windows Agent](#)

    For IBM Cloud Support information, see [Physical Servers](../../ReleaseNotes/SupportedVersions.htm#PhysicalSupport).

    1.  Ensure the agent has been downloaded to the server you want to protect.

    2.  As an administrator with local system privileges on that server, run the executable with **Run as administrator** and complete the installation wizard.

        The {{site.data.keyword.baas_full_notm}} Volume CBT driver creates the **Cohesity\_Bitmap.cty** file at the root of each volume. There are multiple ways to backup Windows physical server but the **Cohesity\_Bitmap.cty** file will be available on the system only if the Volume CBT driver is installed. This file is used to maintain the CBT data for incremental block based backups across Windows server reboots.

        Do not move or delete the “**.cty**” file.

        The **File System CBT (Changed Block Tracker)** component is required for MS SQL file-based protection.


    3.  The agent starts automatically.


    This installation does not require a restart. You can wait until a normal maintenance window (such as a Patch Tuesday) to restart. However, until you do, some Protection Groups that depend on Volume CBT will run in full backup mode.

    [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)Linux Agent](#)

    The Backup agent is now available with different installer packages, providing support on multiple Linux distributions. The following table lists the installer packages and Linux distributions on which the installer package is supported.


    | Installer package | Linux distribution |
    | --- | --- |
    | (Default) RPM | RHEL and its derivative |
    | SUSE RPM | SUSE |
    | Script Installer | All supported Linux operating systems |

    For IBM Cloud Support information, see [Physical Servers](../../ReleaseNotes/SupportedVersions.htm#PhysicalSupport).

    Linux agent has dependencies on the following packages, which must be installed on the Linux server:


    | Command/Package | Linux distribution |     |     |     |     |
    | --- | --- | --- | --- | --- | --- |
    | RHEL | SUSE | CentOS | Ubuntu | Debian |
    | --- | --- | --- | --- | --- | --- |
    | rsync | rsync | rsync | rsync | rsync | rsync |
    | mount | mount | mount | mount | mount | mount |
    | lsof | lsof | lsof | lsof | lsof | lsof |
    | umount | umount | umount | umount | umount | umount |
    | cp  | cp  | cp  | cp  | cp  | cp  |
    | chown | chown | chown | chown | chown | chown |
    | chmod | chmod | chmod | chmod | chmod | chmod |
    | mkdir | mkdir | mkdir | mkdir | mkdir | mkdir |
    | rm  | rm  | rm  | rm  | rm  | rm  |
    | tee | tee | tee | tee | tee | tee |
    | hostname | hostname | hostname | hostname | hostname | hostname |
    | stat | stat | stat | stat | stat | stat |
    | blkid | blkid | blkid | blkid | blkid | blkid |
    | ls  | ls  | ls  | ls  | ls  | ls  |
    | losetup | losetup | losetup | losetup | losetup | losetup |
    | dmsetup | dmsetup | dmsetup | dmsetup | dmsetup | dmsetup |
    | timeout | timeout | timeout | timeout | timeout | timeout |
    | lvs | lvs | lvs | lvs | lvs | lvs |
    | vgs | vgs | vgs | vgs | vgs | vgs |
    | lvcreate | lvcreate | lvcreate | lvcreate | lvcreate | lvcreate |
    | lvremove | lvremove | lvremove | lvremove | lvremove | lvremove |
    | lvchange | lvchange | lvchange | lvchange | lvchange | lvchange |
    | libpcap-progs | null | libpcap-progs | null | null | null |
    | nfs-utils<br><br>Required for Instant Volume Mount, file-folder recovery from block-based backup and VMware backup. | nfs-utils | nfs-utils | nfs-utils | nfs-utils | nfs-utils |
    | wget | wget | wget | wget | wget | wget |

    Installing the Linux agent as an LDAP user is supported.

    ### Install RPM, Debian or SUSE RPM Installer Package

    The following table explains the action to be performed for installing the RPM, Debian or SUSE installer package:

    In the following commands, replace the Backup agent version with the desired agent version.


    | Action | How to |
    | --- | --- |
    | **Download the Agent installer** | From the Download Agents window, based on your Linux distribution, select **RPM**, **Debian**, or **SUSE RPM** and download it to the server you want to protect. |
    | **Navigate to the downloaded directory** | As the root user with local system privileges on that server, change the directory to the location of the installer package. |
    | **Install the Custom certificates** | **LINUX**<br><br>If you are using a user certificate then run the following commands to install the user certificate followed by agent installation:<br><br>export ENFORCE\_USE\_CUSTOM\_CERTS=true && export USE\_THIRD\_PARTY\_CERTS=true && export ROOT\_CA\_FILE=<path-to-root-ca-cert> && export PRIVATE\_KEY\_FILE=<path-to-server-private-key> && export CERT\_CHAIN\_FILE=<path-to-server-cert> |
    | **Install the Agent** | Run the following command depending on the installer package:<br><br>*   RPM: `rpm -i el-cohesity-agent--1.x86_64.rpm` or `yum localinstall ./el-cohesityagent-7.2-1.x86_64.rpm`<br>*   Debian: `dpkg -i cohesity-agent_-1_amd64.deb`<br>*   SUSE RPM: `rpm -i cohesity-agent--1.x86_64.rpm` |
    | **Install the agent as root user** | Run the executable file with the following command syntax:<br><br>/<path\_to\_installer\_file> -- --install -c 0 -S root -G root<br><br>Followings are the command options in details:<br><br>1.  **\-S** this is the user to run the agent, specify root.<br>    <br>2.  **\-G** this is the group permission the Agent will use for files and directories installed by the agent, specify root.<br>    <br>3.  **\-c** this boolean switch controls if the OS user and group should be created. 0 means do not create the OS user and group, 1 means the agent installation will create the specified OS user and group. (When choose to run with the root user, specify ‘-c 0’ as ‘root’ already exists) |
    | **Install the agent as non-root user** | To start the service as a non-root user, create a new user or use an existing user with sudo permission and run the following command:<br><br>*   RPM: `export COHESITYUSER=<username> ; rpm -i el-cohesity-agent--1.x86_64`<br>    <br>*   Debian: `COHESITYUSER=<username> dpkg -i cohesity-agent_-1_amd64`<br>    <br>*   SUSE RPM: `export COHESITYUSER=<username> rpm -i cohesity-agent--1.x86_64`  <br>    Where, `<username>` is the user name with sudo permission.<br>    <br>    By default, the installation uses the root user permission for all the files, and the service is started as root. Therefore, it is required to add non-root users to the sudoers list by making the following changes in the `/etc/sudoers` file:<br>    <br>    `<user_name> ALL=(ALL) NOPASSWD:ALL`<br>    <br>    `Defaults:<user_name> !requiretty`<br>    <br>    An example of the sudoers file is as given below:<br>    <br>      <br>    dgoble ALL=(ALL) NOPASSWD:ALL<br>    <br>    Defaults:dgoble !requiretty  <br>    cohbackup ALL=(ALL) NOPASSWD:ALL  <br>    Defaults:cohbackup !requiretty<br>    <br>    You can add sudo privileges for non-root users using the syntax given below:<br>    <br>    `<user_name> ALL=(ALL) NOPASSWD:/path/to/command1, /path/to/command2, /path/to/command3`<br>    <br>    Remove `NOPASSWD` from the command above if you do not want to authenticate every sudo commands with password.<br>    <br>    The example below shows the sudo commands for user cohesityagent:<br>    <br>    `cohesityagent ALL=NOPASSWD: /bin/mount,/bin/umount,/bin/chmod,/bin/chown,/bin/hostname,/bin/mkdir,/bin/rm,/usr/bin/tee,/usr/bin/stat,/bin/cp,/bin/sed,/usr/bin/touch,/sbin/blkid,/usr/bin/lsof,/bin/ls,/sbin/losetup,/sbin/dmsetup,/usr/bin/rsync,/usr/bin/timeout,/usr/bin/getfacl,/usr/bin/setfacl,/tmp/installer_ch_cohesity_linux_agent,/home/cohesityagent/installer,/home/cohesityagent/cohesityagent/data/agent/tmp/installer_ch_cohesity_linux_agent,/bin/systemctl,/home/cohesityagent/cohesityagent/software/crux/bin/cohesity_linux_agent.sh`<br>    <br>    The actual paths for each command may vary in each system. {{site.data.keyword.baas_full_notm}} keeps adding or removing commands across releases. |
    | **Location** | *   Installation directory: `/opt/cohesity`<br>*   Log file: `/var/log/cohesity` |

    

    ### Install script installer package

    The following table explains the action to be performed for installing the script installer package:

    In the following commands, replace the Backup agent version with the desired agent version.


    | Action | How to |
    | --- | --- |
    | **Download the Agent installer** | From the Download Agents window, based on your Linux distribution, select **Script Installer** and download it to the server you want to protect. |
    | **Navigate to the downloaded directory** | As the root user with local system privileges on that server, change the directory to the location of the installer package.<br><br>For SLES 11 SP4, installing the agent as the root user is required. |
    | **Make the installer executable** | Make the installer executable, for example: `chmod +x cohesity_agent__linux_x64_installer` |
    | **Install the Agent** | Run the executable:<br><br>sudo /cohesity\_agent\_6.6.0d\_u2\_linux\_x64\_installer – --install |
    | **Location** | *   Installation directory:`/home/<username>/cohesityagent` or `/root/cohesityagent`<br>*   Log file: `/home/cohesityagent/cohesityagent/logs`<br>    <br>    You can specify the option `-log-dir <installtion_path>` to specify the directory path where the agent logs will be saved. If this option is not provided, the logs will be saved in the default path |


## Register a Physical Server with the {{site.data.keyword.baas_full_notm}}

Registering a physical server makes its content eligible to be protected or backed up on the  {{site.data.keyword.baas_full_notm}}.

Selecting an Interface Group is not relevant when registering a physical server, despite the option being present on the Physical Server registration page on the {{site.data.keyword.baas_full_notm}}.

1.  Select **Data Protection > Sources**.

Click **Register > Physical Server.**

3.  Specify the hostname (FQDN) or server IP (IPv4 or IPv6) of the server to register. {{site.data.keyword.baas_full_notm}} recommends using the FQDN.
4.  Click **Register**.

    When registration finishes, a new entry appears in the physical section on the Sources page.


Now you can [create a Protection Group with the Registered Source](/docs/backup-recovery?topic=backup-recovery-protection-groups).

You can register a Windows physical server on multiple {{site.data.keyword.baas_full_notm}}. In a {{site.data.keyword.baas_full_notm}} failover scenario, when the primary {{site.data.keyword.baas_full_notm}} goes down, you can use the secondary {{site.data.keyword.baas_full_notm}} to backup the file/folder on the physical server. However, you cannot register a Windows physical server as an MS SQL server on multiple {{site.data.keyword.baas_full_notm}}.

## Red Hat High Availability (Pacemaker) Cluster

To protect the Red Hat High Availability (Pacemaker) Cluster, you must first register it as a physical server on the {{site.data.keyword.baas_full_notm}} using the VIP address.

## Manage Physical Servers

### Reregister Backup agent (Windows or Linux)

The reregister functionality on the {{site.data.keyword.baas_full_notm}} allows you to save the agent settings on the cluster and enables you to perform an agent reregistration with the same settings when required.

With reregistration, the existing file-based protection job would continue to work with the new agent without any intervention. However, the block-based protection job may need to be modified if specific volumes under the job have changed before reregistration.


*   Reregister functionality is currently supported for Windows and Linux servers.

*   During agent reregistration, you must use one of these options, **Apply settings for agent preserved on {{site.data.keyword.baas_full_notm}} UI** or **Preserve setting during uninstallation of an agent from the server**.


1.  Select **Data Protection** > **Sources**.

2.  Navigate in the source hierarchy to the Windows or Linux server you want to reregister, mouse over its actions menu, and click **Re-register**.

3.  Click **Apply the Settings for the agent preserved on the <date and time>** checkbox if you want to restore the server settings.

    If this checkbox is not selected, then the settings saved on the cluster are not applied to the newly registered agent.

    During agent uninstallation of a {{site.data.keyword.baas_full_notm}} Windows or Linux agent, there is an option to preserve settings in the windows registry or config file for Linux. If you had selected the **Preserve Settings** option during the uninstall process then deselect the above option during the reregistration workflow.

4.  Click **Reregister**.


### Upgrade Agent on a Physical Server

This option is clickable if an agent upgrade is available for the server. If an agent upgrade is requested while backup jobs or recover tasks are active, the upgrade starts after the jobs and tasks finish.

1.  Navigate to ****Data Protection > Sources****.
2.  Navigate the source hierarchy to the server, mouse over its actions menu and click **Upgrade Agent**.
3.  Click **OK**.

### Edit Physical Server Registration

1.  Navigate to ****Data Protection > Sources****.
2.  Navigate the source hierarchy to the server, mouse over its actions menu and click **Edit**.
3.  Edit the desired information.
4.  Click **Update Server**.

### Unregister Physical Server from the Cluster

1.  Navigate to ****Data Protection > Sources****.
2.  Navigate the source hierarchy to the server, mouse over its actions menu and click **Unregister**.
3.  Click **Unregister**.
