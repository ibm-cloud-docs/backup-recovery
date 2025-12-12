---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: microsoft sql, physical server, VDI-based backup, cloned database

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Considerations for Microsoft SQL Server
{: #considerations_for_microsoft_sql_server}


## General
{: #general}

*   If using native or third-party backup software to back up an application (such as MS SQL), note that the following behavior breaks the differential chain for the application on that server:

    *   Performing an app-consistent backup on a VMware SQL VM.

    *   Backing up a physical server using a (non-SQL) volume-based physical server Protection Group. You are allowed to run a file-based physical server Protection Group with a SQL Protection Group or 3rd party SQL protection tools.
        This is a Microsoft limitation. For more information, see [Various Causes of Log Chains Breaking Issue in SQL Server](https://social.technet.microsoft.com/wiki/contents/articles/25439.various-causes-of-log-chains-breaking-issue-in-sql-server.aspx){: external}.

*   The SQL Server VDI log backup and restore operations require Sysadmin privileges which is a Microsoft requirement. For more details, see [Microsoft Documentation on SQL Server VDI backup](https://support.microsoft.com/en-us/topic/sql-server-vdi-backup-and-restore-operations-require-sysadmin-privileges-4797c4bc-ed60-8f1b-7978-8ee1cc701eab){: external}.


### Protection
{: #protection}

*   Ensure that all AG databases you want to protect are selected from the respective replica.


*   SQL Database and logs residing on SMB/NFS can be protected using the MS SQL Native method.


*   SQL Database and logs residing on Clustered Shared Volumes (CSV) can be protected using the MS SQL Native and MS SQL VDI methods.


*   As per Microsoft requirements, VDI backups executed on secondary replicas are copy-only full backups. Copy-only backups on secondary replicas do not impact the log chain. For more information, see [Offload supported backups to secondary replicas of an availability group](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups?view=sql-server-ver15){: external}.


### Restore
{: #restore}

*   Performing a point-in-time restore of an Always On Availability Group replica requires full backups from that replica.

    Log backups are merged across all replicas; full backups are not. During restore, the user sees multiple results for the same database from different replicas. Each replica shows a set of Snapshots for full backups done on that particular replica. For point in time restore to a specific replica, the user must select a full backup Snapshot from that replica only. If a full backup does not exist, point in time restore is not possible.

    Restore the database to the desired MS SQL Server instance on a replica that has a full backup.


*   Using the “Restore to Original SQL Server instance” option is not supported if any of the following occurs:

    *   The Windows host name changed and you attempt to restore a snapshot that was taken before the host name change.
    *   Granular DB restore performed from a higher SQL Server version to a lower version. This is a Microsoft limitation.


    You can, toggle off "**Restore to Original SQL Server Instance**" and perform a database restore.

    For more information, see [Rename computer hosting instance - SQL Server](https://docs.microsoft.com/en-us/sql/database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server?view=sql-server-2017){: external}.

    Restoring a snapshot taken after the name change or upgrade is supported.

*   Restoring MS SQL Server from backups of dynamic disks to the same machine is not supported.

*   Use File-based or VDI-based backup methods to protect TDE databases. Follow the Microsoft TDE best practices for key and cert management.

*   If multiple restore tasks are run on the same target host, the restore tasks will be serialized.

*   **For system database recovery**:

    *   Recovering the system database to an alternate instance is not supported.


    *   When recovering to the original database, the original database is deleted before the recovery. Therefore, a recovery failure will also lead to the loss of the original database.

    *   If you are recovering the system databases individually, then the recovery of the system database must be in the following order:

        1.  Master

        2.  MSDB

        3.  Model

    *   You cannot recover a system database along with a user database.

    *   If you are recovering more than one system databases in a recovery task, then the recovery point for the selected system databases must be the same.


### Clone
{: #clone}

*   Cloning an MS SQL database and renaming the database via SSMS can leave the cloned database in the "Recovery Pending" state during a clone teardown task. This is because the {{site.data.keyword.baas_full}} database clone operation assumes the cloned database name and files are associated with the original clone operation. To prevent a disconnect between the {{site.data.keyword.baas_full_notm}} and SQL Server operation, it is recommended to clone a new database with the intended database name rather than rename a cloned database.


### Migration
{: #migration}

*   {{site.data.keyword.baas_full_notm}} does not support migration of MS SQL FILESTREAM databases.
*   Once a migration task is configured and created, the target SQL Server or database paths cannot be modified for the existing migration task.
*   {{site.data.keyword.baas_full_notm}} does not detect changes/deletions to the target database with a running migration task. Changes made external of the migration task before finalizing can cause the migration task to fail.
*   {{site.data.keyword.baas_full_notm}} does not support migration of MS SQL databases that are replicated to an alternate {{site.data.keyword.baas_full_notm}}.
