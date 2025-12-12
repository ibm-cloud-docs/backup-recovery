---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: microsoft sql, physical server, database clone

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# About cloning microsoft sql server databases
{: #about_cloning_microsoft_sql_server_databases}

A clone task instantiates a backup of an MS SQL database so it can run directly off the {{site.data.keyword.baas_full}}. While cloning an MS SQL database, you can choose a destination server that is registered with {{site.data.keyword.baas_full_notm}} as MS SQL. Cloning physical server to physical server only is supported.

## Ms sql db clone example
{: #ms_sql_db_clone_example}

For example, when cloning a database from a physical SQL server to a SQL server instance running on a different physical server, the process consists of the following actions:

1. The {{site.data.keyword.baas_full_notm}} user must select the following task options:
    *   Clone to a specified SQL server and SQL instance
    *   An MS SQL database and snapshot
2. The cloned database files are then brought online as a new database.
