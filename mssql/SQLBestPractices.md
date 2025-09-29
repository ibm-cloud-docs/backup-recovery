---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: microsoft sql, full recovery model

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Best practices for microsoft sql server
{: #best_practices_for_microsoft_sql_server}

{{site.data.keyword.baas_full}} recommends the following best practices when protecting MS SQL:

*   Multiple {{site.data.keyword.baas_full_notm}}s concurrently backing up an MS SQL VM is not recommended because the jobs will interfere with the other's log chain.
*   For MS SQL point in time restores, the Full Recovery Model is recommended as a best practice. For more information, see [Choosing the Recovery Model for a Database](https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms175987(v=sql.105)?redirectedfrom=MSDN){: external}
*   Set MS SQL Server to autostart.
*   To accurately report the size of transaction log data read/written during log backups, the Windows local or domain Administrator account that is registered with {{site.data.keyword.baas_full_notm}} for backup must be added as a user to the 'model' system database. Use the following query:
    `-- Configures accurate reporting of the size of transaction log data read/written during log backup   USE [model]   GO   CREATE USER [HOSTNAME\Administrator] FOR LOGIN [HOSTNAME\Administrator]   GO`
    Where HOSTNAME is the hostname of the SQL server.
    This assumes that the Administrator account used to register MS SQL is already registered in the SQL server instance's Security Logins and has the "sysadmin" role per documentation. If privileges are not correct, the logical bytes read stat ("Size" column on the Job's details page) for MS SQL log backups will be the same as the physical bytes read stat ("Read" column on the Job's details page). If privileges are correct, then the size reported will be the sum of the sizes of all the .ldf files for the database.
