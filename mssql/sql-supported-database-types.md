---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: backup agent, sql database, workflows, protection method

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Supported database types for microsoft sql server
{: #supported_database_types_for_microsoft_sql_server}


Before proceeding to deploy the Backup agent for SQL data protection, we recommend you correctly identify the type of SQL database you want to protect. Below is a table of SQL database types that are supported by the {{site.data.keyword.baas_full}} SQL agent, and the recommended protection methods.

To understand the concept of each method, see [About Microsoft SQL Server Backup](/docs/backup-recovery?topic=backup-recovery-about_microsoft_sql_server_backup).


| Database Types | Definition | Adapter-based Protection Method |     |     |
| --- | --- | --- | --- | --- |
| Volume-based Protection | File-based Protection | VDI-based Protection |
| Stand Alone | A SQL Server User database. | ![](../Resources/Images/Success.svg) | ![](../Resources/Images/Success.svg) | ![](../Resources/Images/Success.svg) |
| AG  | A database that belongs to an availability group (AG). For each availability database, the availability group maintains a single read-write copy (the primary replica) and one to four read-only copies (secondary replicas).<br><br>For details, see [Always On availability groups: a high-availability and disaster-recovery solution](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-ver15){: external} in the Microsoft documentation. | ![](../Resources/Images/Success.svg) | ![](../Resources/Images/Success.svg) | ![](../Resources/Images/Success.svg) |
{: caption="SQL database types" caption-side="bottom"}


For details on the supported workflows for each protection method, see [Protection Workflows for MS SQL](/docs/backup-recovery?topic=backup-recovery-supported_workflows_and_external_targets).

Now that you can identify the type of SQL database you want to protect, you can register it as a {{site.data.keyword.baas_full_notm}} source.
