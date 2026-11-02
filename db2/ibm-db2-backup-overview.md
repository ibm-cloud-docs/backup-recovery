---

copyright:
  years: 2026
lastupdated: "2026-11-02"

keywords: ibm db2, database backup, db2 connector, connector agent, backup concepts

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Understanding IBM DB2 backup and recovery
{: #ibm-db2-backup-overview}

IBM DB2 to provide backup and recovery for IBM DB2 deployments on Linux x86_64 platform. Using Connector Agent, you can back up and restore IBM DB2 databases.
{: shortdesc}

IBM backup and recovery service provides DB2 connector that enables you to perform full backups, incremental backups, log backups, restores, and point-in-time restores at the database level.

## Core components
{: #core-components}

The following are the core components for protecting IBM DB2 databases:

IBM DB2 cluster
:   IBM DB2 database deployment that contains the databases you want to protect.

IBM DB2 connector
:   The connector that integrates with DB2 to enable backup and restore operations using native DB2 backup capabilities.

Connector Agent
:   The agent installed on IBM DB2 host that facilitates communication between the backup and recovery service and IBM DB2 deployment.

## How it works
{: #how-it-works}

Bckup and recovery service uses the following workflow to protect DB2 databases:

1. The Connector Agent is installed on the IBM DB2 host.
2. Register the IBM DB2 source with the Backup and recovery service instance.
3. Create a Protection Group that specifies which databases to back up.
4. The Protection Group uses a Protection Policy that defines backup schedules and retention settings.
5. During backup operations, the  IBM DB2 connector uses native DB2 backup commands to capture database data.
6. Backup data is transferred to the cluster for storage and management.

## Backup types
{: #backup-types}

The IBM DB2 connector supports the following backup types:

Full backup
:   A complete backup of the entire database, including all data files and transaction logs.

Incremental backup
:   A backup that captures only the data that has changed since the last full or incremental backup.

Log backup
:   A backup of transaction logs that enables point-in-time recovery.

## Next steps
{: #next-steps}

* [Plan and prepare for IBM DB2 protection](/docs/data-protection?topic=data-protection-plan-and-prepare-ibm-db2)
* [Register IBM DB2 as a source](/docs/data-protection?topic=data-protection-register-ibm-db2-source)
* [Protect IBM DB2 deployments](/docs/data-protection?topic=data-protection-protect-ibm-db2-deployments)
* [Back up IBM DB2 databases](/docs/data-protection?topic=data-protection-backup-ibm-db2-database)
