---

copyright:
  years: 2026
lastupdated: "2026-11-02"

keywords: ibm db2, database backup, log backup, offline backup, db2 connector

subcollection: backup-recovery

content-type: tutorial
services: backup-recovery
account-plan: lite
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Back up IBM DB2 databases
{: #backup-ibm-db2-database}
{: toc-content-type="tutorial"}
{: toc-services="db2"}
{: toc-completion-time="30m"}

Learn how to back up your IBM DB2 databases using the Cohesity Platform. After registering IBM DB2 as a source, you can configure backups to protect your database deployments.
{: shortdesc}

## Before you begin
{: #backup-db2-prereqs}

Before you can back up IBM DB2 databases, ensure that you have:

* Registered your IBM DB2 source on the Cohesity Platform
* Appropriate permissions to create Protection Groups
* Configured database settings for log archival (if using log backups)

For information about registering IBM DB2 sources, see [Register IBM DB2 source](/docs/data-protection?topic=data-protection-register-ibm-db2-source).

## Understanding the backup process
{: #backup-db2-process}
{: step}

The Cohesity Platform provides a DB2 Connector for backup and restore of IBM DB2 deployments. The IBM DB2 Connector uses the native backup capabilities provided by IBM DB2.

After you register the IBM DB2 source on the Cohesity Platform, you can create a Protection Group and specify the objects such as databases that you want to back up.

## Choosing a log backup method
{: #backup-db2-log-methods}
{: step}

Cohesity provides two log backup methods for IBM DB2 databases. Review the following options to determine which method best suits your requirements.

### Dump and sweep method
{: #backup-db2-dump-sweep}

In the dump and sweep backup method:

1. IBM DB2 archives log files to local archive locations configured in the `LOGARCHMETH1` option in the database configuration.
2. The Cohesity IBM DB2 connector copies log files from the local archive location to Cohesity storage during scheduled log backup execution.
3. Log files that are copied to Cohesity storage and older than the configured prune hours value are removed from the local archive location as part of the scheduled log backup workflow.

### Log archival to Cohesity View using vendor library
{: #backup-db2-vendor-library}

In the vendor library backup method:

1. The IBM DB2 vendor library approach is used for log archival.
2. In the DB2 database configuration, the `LOGARCHMETH2` option is configured to use the Cohesity DB2 vendor library to archive log files directly to Cohesity storage instead of the local archive location on DB2 nodes.
3. Logs from the Cohesity storage are pruned or deleted based on the retention period configured in the Protection Policy.

### Considerations for vendor library method
{: #backup-db2-vendor-considerations}

Review and understand the following before you use the vendor library-based approach for direct log archival to Cohesity storage:

* The `LOGARCHMETH2` option in the database configuration is used to archive logs directly to the Cohesity view.
* During the first full backup for a database, the Cohesity IBM DB2 Connector performs the following actions:
  * Configures `LOGARCHMETH2` and `LOGARCHOPT2` to use the Cohesity DB2 log archival library and Cohesity storage configurations.
  * Deactivates the database, performs an offline backup on the database, and then activates the database to change the `LOGATHMETH` option. This results in database downtime, as required by IBM DB2. For more information, see [IBM DB2 LOGARCHMETH2 documentation](https://www.ibm.com/docs/en/db2/11.5?topic=parameters-logarchmeth2-secondary-log-archive-method){: external}.
  * The first full database backup takes extra time because of the additional offline backup.

If you want to use only `LOGARCHMETH2` for log archival, make sure to manually turn off `LOGARCHMETH1`-based archival (if configured). The Cohesity IBM DB2 connector does not turn off `LOGARCHMETH1`-based archival.
{: note}

### Using both log archival methods
{: #backup-db2-both-methods}

Review and understand the following before you use `LOGARCHMETH1` and `LOGARCHMETH2` both on a database:

* In IBM DB2 nodes, you can configure `LOGARCHMETH1` and `LOGARCHMETH2` options in the database configuration to back up logs. If there is an archival failure in one of the archive methods, IBM DB2 continues to retry the log archival for failed files until the log files are successfully archived using both methods.
* Using both archival methods on a DB2 database impacts Cohesity log backup or archival performance.

Log archival to Cohesity View using the vendor library backup method cannot be enabled on a registered source. Backup jobs added before the release of version 7.1.1 continue to use dump and sweep-based log backups. To use the log archival to Cohesity View using the vendor library backup method, you must register a new source and enable the backup method during registration.
{: important}

## Configuring offline backups
{: #backup-db2-offline}
{: step}

If you want to periodically perform online and offline backups on the same database, follow these best practices:

1. Create a Protection Group for online backup, for example, `DB1_online_backup`.
2. Create a different Protection Group for offline backup, for example, `DB1_offline_backup`.
3. Keep the Protection Group `DB1_offline_backup` in the **Paused** state.
4. Run the Protection Group `DB1_offline_backup` as per the defined schedule in the protection policy.
5. When you want to perform an offline backup:
   1. Pause the Protection Group for online backup (`DB1_online_backup`).
   2. Wait for the current protection run to finish before triggering the offline backup.
6. Perform the offline backup using the **Run Now** option, and bring the database online.
7. Unpause the Protection Group for online backup (`DB1_online_backup`).

## Next steps
{: #backup-db2-next-steps}

After you configure your backup settings, you can:

* [Restore IBM DB2 databases](/docs/data-protection?topic=data-protection-restore-ibm-db2-database)
* [Monitor backup jobs](/docs/data-protection?topic=data-protection-monitor-backup-jobs)
* [Configure backup policies](/docs/data-protection?topic=data-protection-configure-backup-policies)
