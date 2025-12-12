---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: vdi-based backup, microsoft sql, transaction logs, backup agent

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# About Microsoft SQL Server Backup
{: #about_microsoft_sql_server_backup}

IBM Cloud Supports the following MS SQL Server backups:

*   [VDI-based Backup](#VDI-base)

{{site.data.keyword.baas_full}} also supports VM-based Backup and SQL Native Backup.

## VDI-based Backup
{: #vdi_based_backup}


A {{site.data.keyword.baas_full_notm}} MS SQL VDI-based backup captures the native MS SQL server backups. It captures VDI-based backups of a specific database on a SQL Server instance running on a VMware or physical server.

The Virtual Device Interface (VDI) is a Microsoft interface that allows the Backup agent to execute SQL Server Native backup and restore commands. This backup type produces a standard SQL Server Native backup of type (.BAK), transaction logs, (.TRN), and differentials, (.DIFF). These backup files are stored on a {{site.data.keyword.baas_full_notm}} View and take advantage of View compression, deduplication, encryption, replication, and archiving features.

For information on performing MS SQL VDI-based back up, see, see [Backup Microsoft SQL Server (VDI-based)](/docs/backup-recovery?topic=backup-recovery-backup_microsoft_sql_server_vdi-based).

All configurations are for illustrative purposes only. {{site.data.keyword.baas_full_notm}} recommends following MS SQL best practices.

An example of MS SQL running on a Windows server that contains the following:

*   Multiple volumes, C, D and F in the figure below
*   Volume D is running multiple MS SQL databases
*   Volume F contains database logs

The MS SQL VDI-based backup task consists of the following actions:

1.  During an MS SQL protection run, a **'BACKUP DATABASE'** command for the specified MS SQL databases is initiated using MS SQL Server Virtual Device Interface (VDI). The generated backup file is saved on the {{site.data.keyword.baas_full_notm}} with a date and time stamp.

2.  If the policy is configured to take incremental database backups, it translates to differential backups in case of VDI-based backup.

3.  If transaction log backups are specified in the policy, the database transaction logs are backed up to the {{site.data.keyword.baas_full_notm}} using MS SQL Server Virtual Device Interface (VDI). The logs allow you to roll back to a specific point in time. MS SQL Server automatically truncates the SQL logs after a successful SQL log backup and marks the Virtual Log Files (VLFs) for reuse.
