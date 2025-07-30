---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: vdi, microsoft sql, backup agent

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Microsoft sql server vdi-based recovery recommendations
{: #microsoft_sql_server_vdi-based_recovery_recommendations}

The Virtual Device Interface (VDI) is a Microsoft interface that allows the Backup agent to execute SQL Server Native backup and restore commands. This backup type produces a standard SQL Server Native backup of type (.BAK), transaction logs, (.TRN), and differentials, (.DIFF). These backup files are stored on a {{site.data.keyword.baas_full}} View and take advantage of View compression, deduplication, encryption, replication, and archiving features.

The aim of the SQL DBA is to have a combination of backups so that the database can be restored to any point in time. A good combination of backups consists of having a full backup, differential (in {{site.data.keyword.baas_full_notm}}, incremental) backups, and log backups. For example, all SQL database restores must begin with a full database backup, secondly, a differential can then be applied to the full, and finally, log backups can be applied in sequence to complete the database restore.

When the backups are applied during the database restore process, you are sequentially adding the captured changes to the database: FULL+DIFF+Log1+Log2+Log3 = Restored Database.

All Microsoft VDI backups, their differentials, and their logs are dependent on a full backup to perform a database restore. Microsoft requires that in order to restore a SQL database, you must start with a full backup, then its transaction logs can be applied. This means your backup retention policy must keep a full backup along with its log backups in order to successfully restore a database.

Simply put, a SQL VDI-based database restore requires a full backup to seed the database, then differential and/or log backups are applied to the specified point in time.

We recommend retaining two sets of full backups with their differential and log backups.

A good backup plan always includes a combination of full, differential, and log backups.
