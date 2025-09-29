---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-24"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Install and Manage the Agent on Linux Servers
{: #linux_agent_install_manage}

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

## Download and Install the Linux Agent
{: #linux_agent_download_install}

Installing the agent on a server allows actions such as registering the agent as a source with the {{site.data.keyword.baas_full_notm}} or performing file-level recovery.

### Install the Agent
{: #linux_agent_install}

1.  Navigate to **Data Protection > Sources**.
2.  Click **Download Backup agent**.

    The Download Agents window appears.

3.  Follow the installation instructions for the corresponding installer package.
4.  Configure the firewall and ports as necessary. For more information, see [Manage Firewall Ports](/docs/backup-recovery?topic=backup-recovery-manage_firewall_ports).

### Install RPM, Debian or SUSE RPM Installer Package
{: #linux_agent_install_rpm_debian_suse}

The following table explains the action to be performed for installing the RPM, Debian or SUSE installer package:

In the following commands, replace the Backup agent version with the desired agent version.


| Action | How to |
| --- | --- |
| **Download the Agent installer** | From the Download Agents window, based on your Linux distribution, select **RPM**, **Debian**, or **SUSE RPM** and download it to the server you want to protect. |
| **Navigate to the downloaded directory** | As the root user with local system privileges on that server, change the directory to the location of the installer package. |
| **Install the Custom certificates** | **LINUX**<br><br>If you are using a {{site.data.keyword.baas_full_notm}} certificate then run the following commands to install the user certificate followed by agent installation:<br><br>export ENFORCE\_USE\_CUSTOM\_CERTS=true && export AGENT\_CERT\_FILE=<path-to-agent-certificate-in-Cohesity-format><br><br>If you are using a user certificate then run the following commands to install the user certificate followed by agent installation:<br><br>export ENFORCE\_USE\_CUSTOM\_CERTS=true && export USE\_THIRD\_PARTY\_CERTS=true && export ROOT\_CA\_FILE=<path-to-root-ca-cert> && export PRIVATE\_KEY\_FILE=<path-to-server-private-key> && export CERT\_CHAIN\_FILE=<path-to-server-cert> |
| **Install the Agent** | Run the following command depending on the installer package:<br><br>*   RPM: `rpm -i el-cohesity-agent--1.x86_64.rpm` or `yum localinstall ./el-cohesityagent-7.2-1.x86_64.rpm`<br>*   Debian: `dpkg -i cohesity-agent_-1_amd64.deb`<br>*   SUSE RPM: `rpm -i cohesity-agent--1.x86_64.rpm` |
| **Install the agent as root user** | Run the executable file with the following command syntax:<br><br>/<path\_to\_installer\_file> -- --install -c 0 -S root -G root<br><br>Followings are the command options in details:<br><br>1.  **\-S** this is the user to run the agent, specify root.<br>    <br>2.  **\-G** this is the group permission the Agent will use for files and directories installed by the agent, specify root.<br>    <br>3.  **\-c** this boolean switch controls if the OS user and group should be created. 0 means do not create the OS user and group, 1 means the agent installation will create the specified OS user and group. (When choose to run with the root user, specify ‘-c 0’ as ‘root’ already exists) |
| **Install the agent as non-root user** | To start the service as a non-root user, create a new user or use an existing user with sudo permission and run the following command:<br><br>*   RPM: `export COHESITYUSER=<username> ; rpm -i el-cohesity-agent--1.x86_64`<br>    <br>*   Debian: `COHESITYUSER=<username> dpkg -i cohesity-agent_-1_amd64`<br>    <br>*   SUSE RPM: `export COHESITYUSER=<username> rpm -i cohesity-agent--1.x86_64`  <br>    Where, `<username>` is the user name with sudo permission.<br>    <br>    By default, the installation uses the root user permission for all the files, and the service is started as root. Therefore, it is required to add non-root users to the sudoers list by making the following changes in the `/etc/sudoers` file:<br>    <br>    `<user_name> ALL=(ALL) NOPASSWD:ALL`<br>    <br>    `Defaults:<user_name> !requiretty`<br>    <br>    An example of the sudoers file is as given below:<br>    <br>      <br>    dgoble ALL=(ALL) NOPASSWD:ALL<br>    <br>    Defaults:dgoble !requiretty  <br>    cohbackup ALL=(ALL) NOPASSWD:ALL  <br>    Defaults:cohbackup !requiretty<br>    <br>    You can add sudo privileges for non-root users using the syntax given below:<br>    <br>    `<user_name> ALL=(ALL) NOPASSWD:/path/to/command1, /path/to/command2, /path/to/command3`<br>    <br>    Remove `NOPASSWD` from the command above if you do not want to authenticate every sudo commands with password.<br>    <br>    The example below shows the sudo commands for user cohesityagent:<br>    <br>    `cohesityagent ALL=NOPASSWD: /bin/mount,/bin/umount,/bin/chmod,/bin/chown,/bin/hostname,/bin/mkdir,/bin/rm,/usr/bin/tee,/usr/bin/stat,/bin/cp,/bin/sed,/usr/bin/touch,/sbin/blkid,/usr/bin/lsof,/bin/ls,/sbin/losetup,/sbin/dmsetup,/usr/bin/rsync,/usr/bin/timeout,/usr/bin/getfacl,/usr/bin/setfacl,/tmp/installer_ch_cohesity_linux_agent,/home/cohesityagent/installer,/home/cohesityagent/cohesityagent/data/agent/tmp/installer_ch_cohesity_linux_agent,/bin/systemctl,/home/cohesityagent/cohesityagent/software/crux/bin/cohesity_linux_agent.sh`<br>    <br>    The actual paths for each command may vary in each system. {{site.data.keyword.baas_full_notm}} keeps adding or removing commands across releases. |
| **Location** | *   Installation directory: `/opt/cohesity`<br>*   Log file: `/var/log/cohesity` |

### Install script installer package
{: #linux_agent_install_script}

The following table explains the action to be performed for installing the script installer package:

In the following commands, replace the Backup agent version with the desired agent version.


| Action | How to |
| --- | --- |
| **Download the Agent installer** | From the Download Agents window, based on your Linux distribution, select **Script Installer** and download it to the server you want to protect. |
| **Navigate to the downloaded directory** | As the root user with local system privileges on that server, change the directory to the location of the installer package.<br><br>For SLES 11 SP4, installing the agent as the root user is required. |
| **Make the installer executable** | Make the installer executable, for example: `chmod +x cohesity_agent__linux_x64_installer` |
| **Install the Agent** | Run the executable:<br><br>sudo /cohesity\_agent\_6.6.0d\_u2\_linux\_x64\_installer – --install |
| **Location** | *   Installation directory:`/home/<username>/cohesityagent` or `/root/cohesityagent`<br>*   Log file: `/home/cohesityagent/cohesityagent/logs`<br>    <br>    You can specify the option `-log-dir <installtion_path>` to specify the directory path where the agent logs will be saved. If this option is not provided, the logs will be saved in the default path |

Agent startup behavior following a machine reboot:

*   CentOS and RedHat (distributions with the "systemd" init system) - The agent starts automatically.
*   Ubuntu (distributions with the "upstart" init system) - The agent starts automatically.

If a Linux server's `/etc/sudoers` file is managed by a deployment engine such as Chef, Puppet or others, this could impact {{site.data.keyword.baas_full_notm}}'s interaction with servers that have the Linux agent installed. Take the corresponding actions below:

| Agent Installation - User type | Action required |
| --- | --- |
| As default cohesityagent user | The Backup agent is installed using the cohesityagent user by default. For default installations, the cohesityagent is created by the installer. During installation, the installer updates the `/etc/sudoers` file to allow cohesityagent sudo and no-tty sudo access. Ensure the following settings in the /etc/sudoers file for the cohesityagent user are preserved:<br><br>    cohesityagent  ALL=(ALL) NOPASSWD:ALL<br>    Defaults:cohesityagent !requiretty<br><br>An example of the sudoers file is as given below:<br><br>#includedir /etc/sudoers.d  <br>dgoble ALL=(ALL) NOPASSWD:ALL  <br>cohbackup ALL=(ALL) NOPASSWD:ALL  <br>Defaults:cohbackup !requiretty |
| As non default user, example foo | Ensure the above settings in the `/etc/sudoers` file for the foo user are preserved by replacing the occurrences of cohesityagent with foo. |
| As root user | No changes required |

### cohesity\_linux\_agent.sh
{: #linux_agent_sh}

The table lists how `cohesity_linux_agent.sh` interacts with various `init` systems to issue `start/stop/status` commands.

If {{site.data.keyword.baas_full_notm}} does not detect any of the `init` systems, the installer uses the **undetected** `init` system commands.


| init system | Commands |
| --- | --- |
| systemd | sudo systemctl start cohesity-agent<br><br>sudo systemctl stop cohesity-agent<br><br>sudo systemctl status cohesity-agent |
| upstart | sudo initctl start cohesity-agent<br><br>sudo initctl stop cohesity-agent<br><br>sudo initctl status cohesity-agent |
| SysV | sudo /etc/init.d/cohesity-agent start<br><br>sudo /etc/init.d/cohesity-agent stop<br><br>sudo /etc/init.d/cohesity-agent status |
| undetected | sudo /etc/init.d/cohesity-agent start<br><br>sudo /etc/init.d/cohesity-agent stop<br><br>sudo /etc/init.d/cohesity-agent status |

## Download and Install the Backup agent on RHV Hosts
{: #linux_agent_download_install_backup_rhv_hosts}

IBM Cloud Supports RHV only until version 6.8.1. While you can still protect your RHV sources, the IBM Cloud Support team will not provide assistance.

Ensure you install the Backup agent on any RHV hosts that you want {{site.data.keyword.baas_full_notm}} to protect. For installing the agent on RHV hosts, follow the steps described in the [Download and Install the Backup agent on Linux Physical Servers](#Download)” section based on the Linux distribution

Run the following command on all the RHV hosts after they are connected and configured by the oVirt Manager:

\# iptables -I INPUT \`iptables -L INPUT --line-numbers | grep REJECT | awk '{ print $1 }'\` -p tcp --dport 50051 -j ACCEPT

\# service iptables save
\# service iptables restart

If the firewall is enabled on RHV 4.1 hosts, open the following port 50051 to accept traffic.

To open the ports, run the following command:

firewall-cmd --permanent --add-port 50051/tcp

## Linux Script Installer Options
{: #linux_script_install_options}

The Backup agent installer has the following options available.


| Option | Description |
| --- | --- |
| \--agent-options, -a \[options\] | Options to pass to the Backup agent. Used with single mode operation only. |
| \--create-user, -c \[0\|1\] | Whether to create `--service-user` or not. \[Default: 1\] Used with `--install` option only. |
| \--debug, -d | Enable shell script debugging with set `-x` \[Default: off\] |
| \--full-uninstall, -U | Uninstall the Backup agent including service-user account and its home directory contents. Destructive operation. Use with care. |
| \--help, -h | Print this help. |
| \--install, -i | Install/uninstall the Backup agent. |
| \--install-dir, -I \[dir\] | Backup agent installation dir. \[Default: `/home/<username>/cohesityagent or /root/cohesityagent`\] |
| \--install-log-file, -L \[file\] | Filename where installer logs should be saved. \[Default: /tmp/cohesity\_agent\_installer\_<time\_in\_seconds>\_<pid>.log\] |
| \--log-dir | Backup agent log installation directory. \[Default: `/home/cohesityagent/cohesityagent/logs`\] |
| \--service-group, -G \[group\] | Groupname to use for service. \[Default: `cohesityagent`\] |
| \--service-user, -S \[user\] | Username to use for service. \[Default: `cohesityagent`\] |
| \--single-mode, -s | Install the Backup agent for single mode operation.<br><br>This installs the agent for one time usage/run only. It does not install the Agent to run as service or daemon to run continuously. This copies all required binaries/files to a location and runs the agent. Example use case: You want to recover files to a Linux VM. {{site.data.keyword.baas_full_notm}} pushes the Backup agent and starts it. After performing the usual file/folder recovery by talking to Backup agent and file copying is done, the Agent is removed. The agent is not installed to run as service. It runs for the duration of the recover task only. |
| \--skip-lvm-check | **1** - Skip LVM related checks during installation and start the agent in non-lvm mode (only file-based backup will be supported).<br><br>**0** - Flag is not set.<br><br>The default value is **0**. |
| \--skip-mountpoint-check | Skip checks performed on installation directory's filesystem mountpoint. |
| \--target | Extract directly to a target directory. Directory path can be either absolute or relative. \[Default: `/tmp`\] |
| \--update-uninstall, -u | Uninstall the Backup agent. Preserve user, group and agent config. Useful when upgrading software. |
| \--yes-all, -y | Accept 'yes' answer to all questions in installer. \[Default: off\] This is useful for performing silent install/uninstall. |

## Install the Backup agent to run with the Root User
{: #linux_agent_install_backup_run_root_user}

Follow the steps below to install the Backup agent to run with the root user:

In the following command, replace the Backup agent version with the desired agent version.

For example, run the following command: `sudo ./cohesity_agent__linux_x64_installer -- --install -S root -G root -c 0 -I /opt -y`

The above example does the following:

*   `-S root -G root` sets user "root:root" user:group for the agent.
*   `-c 0` instructs the installer that user `root` already exists on the system, so do not create it.
*   `-I /opt` installs the agent in the /opt directory. If you do not specify the directory, then the default home directory of the user will be used, /root/ in this case.

## Sudo Privileges for Linux Users
{: #linux_agent_sudo_privs}

The Linux system administrator can allowlist set of commands that do not need the root rights to be executed from the Backup agent deployed on the Linux servers.

The following tables list the commands that can be allowlisted for Oracle database, volume backups, and file restores for the Backup agent.

**Oracle Database**


| Argument | Default Behavior | Allowlist Commands |
| --- | --- | --- |
| \--skip-oracle-check | The default value is 0. We will allowlist commands required by Oracle database. | `mkdir`<br><br>`rm`<br><br>`tee`<br><br>`stat`<br><br>`cp`<br><br>`srvctl (from Oracle home)` |
| If set to 1, we will not add any commands to the allowlist. |

**Volume Backup**


| Argument | Default Behavior | Allowlist Commands |
| --- | --- | --- |
| \--skip-lvm-check | The default value is 0. We will allowlist commands required by volume backup. | `lvs`<br><br>`vgs`<br><br>`lvcreate`<br><br>`vgcreate` |
| If set to 1, we will not add any commands to the allowlist. |

**File Restore**


| Argument | Default Behavior | Allowlist Commands |
| --- | --- | --- |
| \--skip-restore-check | The default value is 0. We will allowlist commands required by file restore. | `blkid`<br><br>`lsof`<br><br>`ls`<br><br>`losetup`<br><br>`dmsetup`<br><br>`rsync`<br><br>`timeout` |
| If set to 1, we will not add any commands to the allowlist. |

During a Linux agent upgrade:
If **NOPASSWD:ALL** entry is present in the sudoers file, then all commands can be executed without the root rights.
If **NOPASSWD:ALL** entry is removed from the sudoers file, then you can add the commands listed in the table above for allowlisting.

## Upgrade the Linux Agent
{: #linux_agent_upgrade}

You can upgrade the Linux agent either from the {{site.data.keyword.baas_full_notm}} UI or using the CLI.

In the following commands, replace the Backup agent version with the desired agent version.


| Upgrade from the {{site.data.keyword.baas_full_notm}} UI | Upgrade using CLI |
| --- | --- |
| 1.  Navigate to **Data Protection > Sources**.<br>    <br>2.  Click **Download Backup agent**.<br>    <br>3.  Stop the Agent service on the server.<br>    <br>4.  Click **Upgrade Agents**.<br>    <br>5.  Restart the Agent service. | 1.  Navigate to the Agent installation directory.<br>    <br>2.  Run the following command to install the script installer package:<br>    <br>    sudo cohesity\_agent\_\_linux\_x64\_installer -- --install<br>    <br>3.  Run the following command to install the RPM package:<br>    <br>    rpm -U el-cohesity-agent-\-1.x86\_64.rpm<br>    <br>4.  Run the following command to install the Debian package:<br>    <br>    dpkg -i cohesity-agent\_\-1\_amd64.deb<br>    <br>5.  Run the following command to install the SUSE RPM package:<br>    <br>    rpm -U cohesity-agent-\-1.x86\_64.rpm<br>    <br>6.  Run the following command to install the PowerPC RPM package:<br>    <br>    rpm -U cohesity-java-agent-7.0.1-1.ppc64le.rpm |

## Auto-upgrade of the Backup agent
{: #linux_agent_auto_upgrade}

IBM Cloud Supports auto-upgrade of the Backup agent running on physical servers.

The option to auto-upgrade Backup agent is not available on the {{site.data.keyword.baas_full_notm}} with multi-tenancy configured.

To enable auto-upgrade:

1.  Navigate to **Settings** > **Update**, and select **Upgrade** tab.
2.  Select the **Upgrade Backup agent on all servers in Sources** checkbox.

## Uninstall the Script-Based Agent
{: #linux_agent_uninstall_script_agent}

To uninstall script-based agent, follow one of the methods listed below:

*   If the Backup agent is installed in the default installation directory, run the "uninstall" script that is included with the installed software. For example:

    cd <bin\_directory>
    \[default=/home/cohesityagent/cohesityagent/software/crux/bin\]
    sudo ./uninstall

*   Use the agent installer itself. For example:

    sudo ./cohesity\_agent\_\_linux\_x64\_installer -- --full-uninstall

    Replace the Backup agent version with the desired agent version.


## Uninstall RPM/PKG Agents
{: #linux_agent_uninstall_rpm}

To unistall the RPM/PKG agents, follow the steps listed below:

*   To uninstall the SUSE RPM/RPM package, run the following command:

    rpm -e cohesity-agent

*   To uninstall the Debian package, run the following command:

    dpkg -r cohesity-agent

*   To uninstall the PowerPC package, run the following command:

    rpm -e cohesity-java-agent


## Sample Ubuntu Linux Installation Output
{: #linux_agent_sample_ubuntu_install_output}

Sample output for the following command:

[![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)`sudo ./cohesity_linux_agent_installer -- -i -y`](#)

Verifying archive integrity... All good.

Uncompressing Backup agent  100%

\==> User asked to accept 'yes' answer to all questions in installer.

\------------------------------------------------------------------------------

\==> UserId = uid=0(root) gid=0(root) groups=0(root)

\==> Umask  = 0077

\==> Setting umask to 022

\==> Umask  = 0022

\------------------------------------------------------------------------------

\==> SUDO\_COMMAND\_PREFIX=

/sbin/init: unrecognized option '--version'

\==> INIT system detected: systemd

\==> Checking if user has provided overrides from command line...

\==> Service user : cohesityagent

\==> Finding service user group based user input ...

\==> Service user group : cohesityagent

\==> Finding installation directory ...

\==> Installation directory for agent: /home/cohesityagent/cohesityagent

\==> Generating set\_env.sh ...

\==> Sourcing /tmp/selfgz30348/set\_env.sh

\==> Backup agent Software Version: 5.0.1

\==> Installed Backup agent will be run with user=cohesityagent.

\------------------------------------------------------------------------------

\==> Installing Backup agent version 5.0.1

\------------------------------------------------------------------------------

COHESITY\_USER=cohesityagent

COHESITY\_GROUP=cohesityagent

COHESITY\_USER\_CREATED=1

COHESITY\_HOME\_DIR=/home/cohesityagent

COHESITY\_INSTALL\_DIR=/home/cohesityagent/cohesityagent

\------------------------------------------------------------------------------

\==> User has given '--yes-all' option. Using that for next question...

Do you want to continue installing? (y/n) yes

\==> Continuing installation...

\------------------------------------------------------------------------------

\==> This is x86\_64 system

\==> LVM: Using lvs from path: /sbin/lvs

\==> LVM: 2.02.133(2)(2015-10-30)

\==> Rsync: Using rsync from path: /usr/bin/rsync

\==> Rsync: rsync  version 3.1.1  protocol version 31

\==> Installing on Linux Distribution: "Ubuntu 16.04.2 LTS"

\==> No previous Backup agent is detected on this system.

\==> Agent process linux\_agent\_exec is not running.

\==> Checking mountpoint for install directory: /home/cohesityagent/cohesityagent

\==> Parent of installation directory (existing): /home

\==> Mountpoint of parent installation directory: /

\==> Checking if mountpoint / has required option "rw" set.

(rw,relatime,errors=remount-ro,data=ordered)

\==> Checking if mountpoint / has unsupported option "nosuid" set.

\==> Checking if mountpoint / has unsupported option "noexec" set.

\==> User=cohesityagent does not exist in /etc/passwd

\==> Checking if User=cohesityagent exists using getent command.

\==> User=cohesityagent not found with getend command.

\==> Group=cohesityagent does not exist in /etc/group

\==> Group=cohesityagent not found with getent command.

id: ‘cohesityagent’: no such user

\==> Group=cohesityagent is not found with id command

\==> Adding cohesityagent to sudoers list

\==> Directory structure created under /home/cohesityagent/cohesityagent

\==> Copied software contents to /home/cohesityagent/cohesityagent

\==> Old installation directory is empty. Skipping copying data from older installation.

/home/cohesityagent/cohesityagent/software/crux/bin/linux\_agent\_exec = cap\_dac\_override,cap\_sys\_resource+ep

Synchronizing state of cohesity-agent.service with SysV init with /lib/systemd/systemd-sysv-install...

Executing /lib/systemd/systemd-sysv-install enable cohesity-agent

insserv: warning: current start runlevel(s) (empty) of script \`cohesity-agent' overrides LSB defaults (2 3 4 5).

insserv: warning: current stop runlevel(s) (empty) of script \`cohesity-agent' overrides LSB defaults (2 3 4 5).

Created symlink from /etc/systemd/system/multi-user.target.wants/cohesity-agent.service to /usr/lib/systemd/system/cohesity-agent.service.

\==> Service cohesity-agent added to systemd

\==> Starting cohesity-agent...

● cohesity-agent.service - Backup agent

Loaded: loaded (/usr/lib/systemd/system/cohesity-agent.service; enabled; vendor preset: enabled)

Active: active (running) since Mon 2018-01-29 23:22:01 PST; 5ms ago

Process: 30796 ExecStart=/etc/init.d/cohesity-agent start (code=exited, status=0/SUCCESS)

Process: 30793 ExecStartPre=/bin/chown -R cohesityagent:cohesityagent /var/run/cohesity-agent (code=exited, status=0/SUCCESS)

Process: 30788 ExecStartPre=/bin/mkdir -p /var/run/cohesity-agent (code=exited, status=0/SUCCESS)

Main PID: 30927 (linux\_agent\_exe)

Tasks: 10

Memory: 280.0K

CPU: 142ms

CGroup: /system.slice/cohesity-agent.service

‣ 30927 /home/cohesityagent/cohesityagent/software/crux/bin/linux\_agent\_exec --log\_dir=/home/cohesityagent/cohesityagent/data/logs --max\_log\_size=30 --stop\_logging\_if\_full\_disk=true --logbuflevel=-1 --linux\_agent\_nfs\_mountpoint\_base\_dir=/home/cohesityagent/cohesityagent/data/agent//tmp/nfs\_mounts --linux\_agent\_volume\_mountpoint\_base\_dir=/home/cohesityagent/cohesityagent/data/agent//tmp/vol\_mounts --linux\_agent\_config\_file\_path=/home/cohesityagent/cohesityagent/data/config/cohesity\_agent.cfg --util\_misc\_mount\_command=/bin/mount --util\_misc\_umount\_command=/bin/umount --util\_misc\_findmnt\_command=/bin/findmnt --util\_misc\_sudo\_command=/usr/bin/sudo --yoda\_util\_vmware\_mount\_command=/home/cohesityagent/cohesityagent/software/crux/bin/vm\_mounter.sh --yoda\_mount\_command=/bin/mount --yoda\_umount\_command=/bin/umount --yoda\_findmnt\_command=/bin/findmnt --yoda\_sudo\_command=/usr/bin/sudo --timeout\_command=/usr/bin/timeout --yoda\_blkid\_command=/sbin/blkid --yoda\_lsof\_command=/usr/bin/lsof --yoda\_ls\_command=/bin/ls --linux\_sudo\_command=/usr/bin/sudo --linux\_chmod\_command=/bin/chmod --linux\_timeout\_command=/usr/bin/timeout --linux\_lvs\_command=/sbin/lvs --linux\_vgs\_command=/sbin/vgs --linux\_lvcreate\_command=/sbin/lvcreate --linux\_lvremove\_command=/sbin/lvremove --linux\_agent\_rsync\_cmd=/usr/bin/rsync --linux\_agent\_wget\_command=/usr/bin/wget --yoda\_losetup\_command=/sbin/losetup --yoda\_dmsetup\_command=/sbin/dmsetup --linux\_dump\_memory\_usage=false --yoda\_bindfs\_mount=false --libssl\_non\_fips\_path= --libssl\_path= -v=0

Jan 29 23:21:59 ubuntu16-server cohesity-agent\[30796\]: Starting linux\_agent\_exec...

Jan 29 23:21:59 ubuntu16-server cohesity-agent\[30796\]: GNU coreutils version = 8.25

Jan 29 23:21:59 ubuntu16-server cohesity-agent\[30796\]: Setting coredump limit to unlimited for qa build

Jan 29 23:21:59 ubuntu16-server cohesity-agent\[30796\]: Init system is systemd, will run linux\_agent\_exec in background

Jan 29 23:21:59 ubuntu16-server su\[30918\]: Successful su for cohesityagent by root

Jan 29 23:21:59 ubuntu16-server su\[30918\]: + ??? root:cohesityagent

Jan 29 23:22:01 ubuntu16-server cohesity-agent\[30796\]: Parent process pid=30927

Jan 29 23:22:01 ubuntu16-server cohesity-agent\[30796\]: 30927

Jan 29 23:22:01 ubuntu16-server systemd\[1\]: Started Backup agent.

Jan 29 23:22:01 ubuntu16-server systemd\[1\]: Started Backup agent.

\------------------------------------------------------------------------------

\==> Backup agent is installed successfully.

\------------------------------------------------------------------------------

\------------------------------------------------------------------------------

Agent Help (init system: systemd)

\------------------------------------------------------------------------------

Agent start:

\# sudo systemctl start cohesity-agent

Agent stop:

\# sudo systemctl stop cohesity-agent

Agent status:

\# sudo systemctl status cohesity-agent

\------------------------------------------------------------------------------

cohesity@ubuntu16-server:~/agent$ cohesity@ubuntu16-server:~/agent$ sudo ./cohesity\_linux\_agent\_installer\_qa -- -i -y

\==> Umask  = 0022

\------------------------------------------------------------------------------

\==> SUDO\_COMMAND\_PREFIX=

/sbin/init: unrecognized option '--version'

\==> INIT system detected: systemd

\==> Checking if user has provided overrides from command line...

\==> Service user : cohesityagent

\==> Finding service user group based user input ...

\==> Service user group : cohesityagent

\==> Finding installation directory ...

\==> Installation directory for agent: /home/cohesityagent/cohesityagent

\==> Generating set\_env.sh ...

\==> Sourcing /tmp/selfgz30348/set\_env.sh

\==> Backup agent Software Version: 5.0.1

\==> Installed Backup agent will be run with user=cohesityagent.

\------------------------------------------------------------------------------

\==> Installing Backup agent version 5.0.1

\------------------------------------------------------------------------------

COHESITY\_USER=cohesityagent

COHESITY\_GROUP=cohesityagent

COHESITY\_USER\_CREATED=1

COHESITY\_HOME\_DIR=/home/cohesityagent

COHESITY\_INSTALL\_DIR=/home/cohesityagent/cohesityagent

\------------------------------------------------------------------------------

\==> User has given '--yes-all' option. Using that for next question...

Do you want to continue installing? (y/n) yes

\==> Continuing installation...

\------------------------------------------------------------------------------

\==> This is x86\_64 system

\==> LVM: Using lvs from path: /sbin/lvs

\==> LVM: 2.02.133(2)(2015-10-30)

\==> Rsync: Using rsync from path: /usr/bin/rsync

\==> Rsync: rsync  version 3.1.1  protocol version 31

\==> Installing on Linux Distribution: "Ubuntu 16.04.2 LTS"

\==> No previous Backup agent is detected on this system.

\==> Agent process linux\_agent\_exec is not running.

\==> Checking mountpoint for install directory: /home/cohesityagent/cohesityagent

\==> Parent of installation directory (existing): /home

\==> Mountpoint of parent installation directory: /

\==> Checking if mountpoint / has required option "rw" set.

(rw,relatime,errors=remount-ro,data=ordered)

\==> Checking if mountpoint / has unsupported option "nosuid" set.

\==> Checking if mountpoint / has unsupported option "noexec" set.

\==> User=cohesityagent does not exist in /etc/passwd

\==> Checking if User=cohesityagent exists using getent command.

\==> User=cohesityagent not found with getend command.

\==> Group=cohesityagent does not exist in /etc/group

\==> Group=cohesityagent not found with getent command.

id: ‘cohesityagent’: no such user

\==> Group=cohesityagent is not found with id command

\==> Adding cohesityagent to sudoers list

\==> Directory structure created under /home/cohesityagent/cohesityagent

\==> Copied software contents to /home/cohesityagent/co-bash: cohesity@ubuntu16-server:~/agent$: No such file or directory

hesityagent

\==> Old installation directory is empty. Skipping copying data from older installation.

/home/cohesityagent/cohesityagent/software/crux/bin/linux\_agent\_exec = cap\_dac\_override,cap\_sys\_resource+ep

Synchronizing state of cohesity-agent.service with SysV init with /lib/systemd/systemd-sysv-install...

Executing /lib/systemd/systemd-sysv-install enable cohesity-agent

insserv: warning: current start runlevel(s) (empty) of script \`cohesity-agent' overrides LSB defaults (2 3 4 5).

insserv: warning: current stop runlevel(s) (empty) of script \`cohesity-agent' overrides LSB defaults (2 3 4 5).

Created symlink from /etc/systemd/system/multi-user.target.wants/cohesity-agent.service to /usr/lib/systemd/system/cohesity-agent.service.

\==> Service cohesity-agent added to systemd

\==> Starting cohesity-agent...

● cohesity-agent.service - Backup agent

Loaded: loaded (/usr/lib/systemd/system/cohesity-agent.service; enabled; vendor preset: enabled)

Active: active (running) since Mon 2018-01-29 23:22:01 PST; 5ms ago

## Troubleshoot
{: #linux_agent_troubleshoot}

For troubleshooting information, see [Troubleshooting Backup agent issues on Linux physical servers](/docs/backup-recovery?topic=Troubleshooting-Cohesity-Agent-issues-on-Linux-physical-servers).

Log in to the IBM Cloud Support site to see more knowledge base articles.
