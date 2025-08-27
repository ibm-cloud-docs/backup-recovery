---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Plan and prepare for physical server protection
{: #plan_and_prepare_for_physical_server_protection}

This topic provides information on minimum permissions, prerequisites and considerations to protect physical servers with {{site.data.keyword.baas_full_notm}}.

## Minimum permissions
{: #minimum_permissions}

Ensure you have the required minimum permissions to register physical servers as a source with the {{site.data.keyword.baas_full}}, and to perform backup and recovery.

## Minimum agent requirements
{: #minimum_agent_requirements}

| Operating System | Minimum CPU | Minimum memory | Free Disk Space | Software Requirement |
| --- | --- | --- | --- | --- |
| Linux | 1 core | 300 MB | 400 MB | nfs-utils, rsync, lsof, wget |
| Debian/Ubuntu | 1 core | 300 MB | 400 MB | nfs-utils, rsync, lsof, wget |
| SLES/openSUSE | 1 core | 300 MB | 400 MB | nfs-utils, rsync, lsof, wget, libcap-progs |
| RHEL/CentOS/OEL | 1 core | 300 MB | 400 MB | nfs-utils, rsync, lsof, wget |
{: caption="" caption-side="bottom"}

## Support matrix
{: #support_matrix}

Before you register your physical servers with {{site.data.keyword.baas_full_notm}}, ensure that IBM Cloud Supports the physical server versions. For more information, see [Supported Software](/docs/backup-recovery?topic=backup-recovery-supported_software)

## Ports used for communication
{: #ports_used_for_communication}

Ensure all necessary ports are open in the firewall between {{site.data.keyword.baas_full_notm}} and physical servers. For more information, see [Manage Firewall Ports](/docs/backup-recovery?topic=backup-recovery-manage_firewall_ports).

## Prerequisites and considerations
{: #prerequisites_and_considerations}

### Physical server protection
{: #physical_server_protection}

#### Prerequisites
{: #prerequisites}

*   The Backup agent running on the hosts should be {{site.data.keyword.baas_full_notm}} version 6.2 onwards.

*   If physical servers are behind NAT, add the NAT server's IP address to the {{site.data.keyword.baas_full_notm}}'s subnets specified in the allowlist.


### Physical server - generic considerations
{: #physical_server_-_generic_considerations}

*   Making a disk offline and then online triggers a full backup for Simple, Striped and Spanned volumes. Mirrored volumes are not affected.

*   Recovering a file or instantly mounting volumes from an AD-joined {{site.data.keyword.baas_full_notm}} to a server that does not belong to that AD domain is not supported.

*   Instant volume mounting does not currently support using drive letters on the target physical Server. Volumes are under C:\\CohesityMounts\\... for Windows and /opt/cohesity/agent/mount\_paths/vol\_mounts/….. for Linux systems.

*   You cannot add different types of physical servers (Windows, Linux) in a single Protection Group as the supported features of {{site.data.keyword.baas_full_notm}} Data Cloud are different for different physical servers.

*   IBM Cloud Supports recovery only from physical server to physical server. Recovering physical server to VM and vice-versa is not supported.

*   IBM Cloud Supports recovering files and folders from Linux to Linux, AIX to AIX, and Solaris to Solaris only. Recovering files and folders from one operating system to another is not supported. For example, recovering files and folders from Linux to Windows is not supported.

*   After creating a directive file-based Protection Group for a physical server, you can simultaneously trigger more than one protection runs of the Protection Group to backup only a specific set of files and folders in each run. However, you cannot simultaneously trigger multiple protection runs to back up the same file or folder.


### Windows - considerations
{: #windows_-_considerations}

*   For file and folder recoveries to Windows machines, if the machine is apart of an AD Domain then the {{site.data.keyword.baas_full_notm}} must be joined to the same AD domain. The only alternative is to disable **SMB Security** in **{{site.data.keyword.baas_full_notm}} Views Global Settings** and allow Guest login on the Windows machine.

*   Windows file-based backup does not support the following:

    *   Backup and recovery of encrypted files (EFS) and alternate data streams (ADS).

    *   Backup of symbolic links within a symbolic link pointing to NAS target.

*   A subsequent Full (No CBT) backup would also occur after a machine restart unless it is an intentional restart of a Windows physical server with the Capture Incremental After Server Restart option toggled on.

*   You cannot add different types of physical servers (Windows, Linux) in a single Protection Group as the supported features of {{site.data.keyword.baas_full_notm}} Data Cloud are different for different physical servers.
*   Source-side deduplication is not supported in Windows 2008 R2 servers.


### Linux
{: #linux}

#### Prerequistes
{: #prerequistes}

*   To use the Linux PowerPC agent, the following prerequisites are required:

    *   Java Runtime Environment (JRE) 8

    *   OpenSSL 1.1.1 should be installed on the source system

    *   Minimum free space of 102400KB is required in /var and /usr directories

    *   Minimum memory of 2GB is recommended
