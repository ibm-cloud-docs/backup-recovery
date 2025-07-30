---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: microsoft sql, database backup, application downtime, database migration

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# About migrating microsoft sql server databases
{: #about_migrating_microsoft_sql_server_databases}

You can use MS SQL database migration for the following use cases:

*   Back up and migrate a database from a single pane of glass.

    Leverage database backup to achieve database migration. No more shipping logs across multiple sites or multiple data centers.

    Upgrade the MS SQL Server version while minimizing application downtime and maintaining the ability to restore to any point in time.

*   Staging a database on the target server for faster recovery with {{site.data.keyword.baas_full}} database migration.

    Stage a database on the target SQL server and perform only incremental updates at the file level to improve RPO.


Database migration for file-based MS SQL backups consists of the following actions, which must be initiated manually:

1. Perform the initial copy of database files to the target MS SQL server.
2. Sync forward to the latest incremental/full snapshot available on the cluster. You can trigger multiple syncs.

    For each sync, {{site.data.keyword.baas_full_notm}} performs incremental file updates to database files on the target MS SQL server. Syncs ensure files are up to date. The files are not attached to the database until you complete the finalize workflow. This ensures that the database is up to date when finalization occurs.

3. Finalize the migration. Finalizing is available only if the target database is synced to the latest snapshot.

    This brings the database online. After the database is online and the finalize step succeeds, the recover task is completed successfully. You cannot perform syncs after this point because the migration workflow is complete.
