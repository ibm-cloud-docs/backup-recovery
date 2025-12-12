---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: microsoft sql, recover system database

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Recover microsoft sql server system databases
{: #recover_microsoft_sql_server_system_databases}

System databases can be restored in case of system failures. The system databases are restored from the same instance and location where the system databases were backed up. You can also overwrite the existing system database files.

Restoring system databases to a different instance is not supported for the VM-based backup method.

The SQL Server system databases are recovered in the following order:

1. Master

2. MSDB

3. Model


## Recovery workflow
{: #recovery_workflow}

You can recover the system databases to the original SQL Server instance. For the detailed steps on recovering system databases, see:

- [Recover Microsoft SQL Server Database Over the Original Database](/docs/backup-recovery?topic=backup-recovery-recover_microsoft_sql_server_database_over_the_original_databasem)


## Consideration
{: #consideration}

- Recovering the system database to an alternate instance is not supported.
- When recovering to the original database, the original database is deleted before the recovery. Therefore, a recovery failure will also lead to the loss of the original database.
- If you are recovering the system databases individually, then the recovery of the system database must be in the following order:

    1. Master

    2. MSDB

    3. Model

- You cannot recover a system database along with a user database.
- If you are recovering more than one system databases in a recovery task, then the recovery point for the selected system databases must be the same.
