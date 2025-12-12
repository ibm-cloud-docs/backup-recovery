---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Supported software
{: #supported_software}






## Physical servers
{: #physical_servers}

Applicable to both file-based and volume-based backups.

For information on prerequisites and considerations, see [Prerequisites and Considerations](/docs/backup-recovery?topic=backup-recovery-plan_and_prepare_for_physical_server_protection).


| Operating System | File System | CPU Architecture | Notes |
| --- | --- | --- | --- |
| Windows Server 2022 Core<br><br>Windows Server 2019 Core<br><br>Windows Server 2016 Core | NTFS | x64 |     |
| Windows 10 Desktop Edition<br><br>Windows 11 | NTFS | x64 |     |
| Windows Server 2022<br><br>Windows Server 2019<br><br>Windows Server 2016<br><br>Windows Server 2012<br><br>Windows Server 2012 R2<br><br>Windows Server 2008 R2 | NTFS | x64 | IBM Cloud Supports Microsoft failover clusters that are compatible with these Windows Server versions.<br><br>Windows Server 2008 R2 data contained in a volume larger than 2040 GB can be recovered via an alternate registered Physical Server running Windows Server 2012 or later. |
| RHEL 6.4, 6.7+, 7.0 - 7.9, 8.0 - 8.10, 9.0 - 9.4 | x64<br> | *   For RHEL 9.x instances, only file-based backup is supported (file recovery, indexing and IVM is not supported).<br>    <br>*   Only file-based backup is supported for Linux on PowerPC.<br>    <br>*   IBM Cloud Supports the little-endian architecture for PowerPC. |
{: caption="Physical servers" caption-side="bottom"}







## Databases
{: #databases}


| Database | Notes |
| --- | --- |
| Microsoft SQL Server 2022<br><br>Microsoft SQL Server 2019<br><br>Microsoft SQL Server 2017<br><br>Microsoft SQL Server 2016<br><br>Microsoft SQL Server Express 2019<br><br>Microsoft SQL Server Express 2017<br><br>Microsoft SQL Server Express 2016 | *   MS SQL Server 32-bit deployment is not supported with the agent-based approach. It is recommended to use the Native SQL dump to {{site.data.keyword.baas_full}} SMB for backups.<br>    <br>*   MS SQL Server 64-bit running on Windows 64-bit is supported.<br>    <br>*   SQL databases and logs residing on Clustered Shared Volumes (CSV) can be protected using {{site.data.keyword.baas_full}}’s VDI based backup method.<br>    <br>*   FILESTREAM databases can be protected using {{site.data.keyword.baas_full_notm}}'s VDI and Volume based backup method.<br>    <br>*   Memory-optimized databases can be protected using {{site.data.keyword.baas_full_notm}}'s VDI based backup method.<br>    <br><br>Supported on the following Operating Systems:<br><br>*   Windows 2022 64-bit<br>    <br>*   Windows 2019 64-bit<br>    <br>*   Windows 2016 64-bit |
| Microsoft SQL Server 2019 | *   Supported on Linux OS RHEL7.6 and higher versions.<br>    <br>*   Only VDI-based backups are currently supported on Linux OS. |
| Oracle Database | IBM Cloud Supports both physical and virtual servers for Oracle databases.<br><br>The following lists the different operating systems and supported {{site.data.keyword.baas_full_notm}} versions.<br><br>*Oracle versions 19c, 21c, 23ai on Rhel 8 and 9|
{: caption="Databases" caption-side="bottom"}






