---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: retention period, database server backup, transaction log, microsoft sql

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# About log and full db backup retention periods
{: #about_log_and_full_db_backup_retention_periods}

The retention periods of log files and full database server backups are independent of each other. Consider the following examples to help determine the best retention periods for your database server backups. These retention guidelines also apply to full database server and log replication schedules. Retention periods of log files and full database backups must adhere to Microsoft rules for restoring databases.

1. A point-in-time restore using SQL log backups is dependent on a full backup because a full database server backup must also be available to apply the logs.

    When the {{site.data.keyword.baas_full_notm}} restores to a specific point in time, it must first restore a full database server backup and then apply the appropriate set of logs. If a full database server backup is not available because the retention period of the full database server backup is too short, the {{site.data.keyword.baas_full_notm}} cannot restore to a point in time within the specified log retention period. For example if the log retention period is three weeks and the full database server backup captures snapshots weekly with a retention period of two weeks, the {{site.data.keyword.baas_full_notm}} will not be able to restore to any point in time from three weeks ago. In addition, the logs from three weeks ago cannot be utilized and are using disk space.

2. If the log retention period is shorter than the full database server backup retention period, the {{site.data.keyword.baas_full_notm}} cannot restore to a point in time where the logs are not saved. For example, if the log retention period is two weeks and the full database server backup captures snapshots weekly with a retention period of three weeks, the {{site.data.keyword.baas_full_notm}} will not be able to restore to any point in time between two and three weeks ago. The {{site.data.keyword.baas_full_notm}} can, however, restore to the snapshot from three weeks ago.

3. {{site.data.keyword.baas_full_notm}} recommends specifying a log retention period with extra time to allow for failed full database server backups. For example, if a full database server backup is scheduled weekly with a three-week retention period for both the full database server and logs, and the full database server backup failed two weeks ago, the {{site.data.keyword.baas_full_notm}} can still restore to the point in time from the log. The {{site.data.keyword.baas_full_notm}} restores full database server backup from three weeks ago and applies the appropriate logs to restore the state to a point. You can also restore to a specific transaction in the log.

A full database backup is not a transaction log backup. Ensure that your SQL Server protection policy has transaction log backup for full and bulk-logged recovery model requirements.
