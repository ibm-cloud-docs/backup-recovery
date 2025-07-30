---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: microsoft sql, database restore, physical server

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# About restoring microsoft sql server databases
{: #about_restoring_microsoft_sql_server_databases}

A restore task copies a database from the backup copy and optionally applies transaction logs depending on the options you choose. {{site.data.keyword.baas_full}} provides the ability to restore individual databases to a specific point in time.

The following options are supported when restoring an individual database:

*   Restore to the original MS SQL server and original instance and overwrite the database.
*   Restore to the original MS SQL server and original instance and rename the database.
*   Restore to the original MS SQL server and an alternate instance as a specified database name.
*   Restore to an alternate MS SQL server and instance as a specified database name.

Restoring physical server to physical server only is supported.

## Ms sql db restore example
{: #ms_sql_db_restore_example}

For example, when restoring an individual MS SQL database running on a physical server to its original location and overwriting the existing database, it assumes the following about the database:

*   Failure has already occurred.
*   Its MS SQL server is registered as MS SQL on the cluster.
*   It was backed up by a {{site.data.keyword.baas_full_notm}} Protection Group that includes transaction logs.

The process consists of the following actions:

1. The {{site.data.keyword.baas_full_notm}} user must select the following task options:
    *   Restore to the original SQL server and instance and overwrite the database
    *   An MS SQL database and snapshot
    *   Point in time (optional)
    *   Capture tail logs (optional)
2. The {{site.data.keyword.baas_full_notm}} copies the database files from the specified snapshot to the original location.
3. If applicable, transaction logs are applied to the database for the specified point in time.
4. The database is then brought online.

## Resume ms sql database recovery after failure
{: #resume_ms_sql_database_recovery_after_failure}

You can resume a failed MS SQL Database recovery for databases in the following scenarios:

*   If the databases are recovered using the WITH NORECOVERY option
*   If the previous restore operation failed and the database remains in the restoring state.

This feature improves the RTO for database restore operations and is applicable only for point-in-time (PIT) restore operations.

This is an Early Access feature. Contact your {{site.data.keyword.baas_full_notm}} account team to enable the feature.
