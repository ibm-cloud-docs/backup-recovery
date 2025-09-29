---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: backup-recovery, microsoft sql, VDI-based, File-based

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# General recommendations for microsoft sql server backup methods
{: #general_recommendations_for_microsoft_sql_server_backup_methods}


Normal databases can be protected using the following approaches, listed in the order of recommendation:

The following table can help you select the appropriate backup type:


|     | VDI-based Backup |
| --- | --- |
| Microsoft Approach | Leverages Microsoft SQL Server native backup and restore commands through API calls |
| Built-in Change Block Tracking (CBT) | Leverages SQL server’s differential backups |
| Database clone from backup | Not Supported |
| Implicit Full backup after SQL host restart | \-  |
| Memory Optimized Filestream Database | Supported |
| SQL server-side compression | Supported but not recommended to use, since it may cause low deduplication rate on the {{site.data.keyword.baas_full_notm}}s, which leads to high storage utilization on the {{site.data.keyword.baas_full_notm}}. |
| Parallel Log Backup to Full/ Incremental Backup | Supported |
| Fast Backup (multi-node) | Supported |
| Fast Restore (multi-node) | Supported |
| Transparent Data Encryption (TDE) Databases | Supported |
| Quiesce Support | Not required. The SQL server performs consistent backup |
| Free Space on SQL Host (VSS shadow storage requirement) | \-  |
| Must-Have | Must have Full and incremental (differential). NOTE: Setting extended retention is required for full backup to avoid expiry before the incremental backup expiry. |
| Parallel run-now backups (same type) for the same job | Backup of the selected database runs in parallel with the existing run-now request. |
| Known Issues that lead to backup failure | Agent/SQL host may restart during backup. |
{: caption="SQL server backup approaches" caption-side="bottom"}


REFS is supported by VDI-based and File-based SQL server.
