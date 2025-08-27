---

copyright:
  years: 2025
lastupdated: "2025-08-27"

keywords: backup, sap hana, database, arguments, log

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Backup SAP HANA database
{: #backup_sap_hana_database}


{{site.data.keyword.cloud_notm}} Backup and Recovery provides a simple workflow to back up your SAP HANA databases. After registering the SAP HANA as a source on {{site.data.keyword.cloud_notm}} Backup and Recovery, you can start configuring your backups.

## Backup process
{: #backup_process}

{{site.data.keyword.cloud_notm}} Backup and Recovery provides a SAP HANA database agent for backup and restore of enterprise SAP HANA deployments. The SAP HANA database agent leverages the [backup](https://help.sap.com/docs/PRODUCT_ID/6b94445c94ae495c83a19646e7c3fd56/c3a57273bb571014a747a289a3198e15.html?state=PRODUCTION&version=2.0.04&locale=en-US){: external} capabilities provided by SAP HANA.

After registering the SAP HANA source on {{site.data.keyword.cloud_notm}} Backup and Recovery, you can create a Protection Group and specify the objects such as databases and tables that you want to backup. In the Protection Group, you need to define the arguments that enable you to provide additional arguments required by the [backup](https://help.sap.com/docs/PRODUCT_ID/6b94445c94ae495c83a19646e7c3fd56/c3a57273bb571014a747a289a3198e15.html?state=PRODUCTION&version=2.0.04&locale=en-US){: external} statements provided by SAP HANA.

{{site.data.keyword.cloud_notm}} Backup and Recovery recommends that you configure periodic full backup.

## Backup arguments
{: #backup_arguments}

In the Protection Group, you can define the following arguments for your SAP HANA backup configuration:


| Backup Arguments | Mandatory? | Description |
| --- | --- | --- |
| `--user-store-key` | No  | The hdbuserstore key to connect to the SAP HANA SYSTEMDB.<br><br>For example: `--user-store-key=H00_KEY` |
| `--backup-delta` | No  | The argument is only applicable for Incremental or Differential backups. The supported values are INCREMENTAL or DIFFERENTIAL. If you do not explicitly specify a value, by default, incremental backup is considered.<br><br>For example: `--backup-delta=DIFFERENTIAL` |
| `-r, --retention-days` | Yes | Specify the number of days to retain full or incremental backups. This argument helps manage storage by automatically removing backups older than the defined retention period.<br><br>Ensure that the number of days specified in the argument matches the days defined in the [backup policy](/docs/backup-recovery?topic=backup-recovery-protect_sap_hana_deployments). |
{: caption="Backup arguments " caption-side="bottom"}

## Log backups
{: #log_backups}

SAP HANA automatically triggers log backups, you can view these log backups on the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster and perform the following:

- View the SAP HANA log backups on the {{site.data.keyword.cloud_notm}} Backup and Recovery clusters.
- Replicate the SAP HANA log backups across the {{site.data.keyword.cloud_notm}} Backup and Recovery clusters in your deployment.
- Archive the SAP HANA log backups.

To view SAP HANA log backups on the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster, perform the following:

1. After you have installed or upgraded the SAP HANA Connector, enable SAP HANA log backup replication. For more information, see [Enable Log Backups Replication](/docs/backup-recovery?topic=backup-recovery-plan_and_prepare_for_sap_hana_protection).
2. While registering or updating the SAP HANA source to the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster, set the `–et-log-backup source` registration parameter to `true`. For more information, see [Register SAP HANA as a Source](/docs/backup-recovery?topic=backup-recovery-register_and_manage_the_sap_hana_source#register_sap_hana_as_a_source).
3. Create a new Protection Group and select a Protection Policy with log backups job in the Protection Group for SAP HANA backups. For more information, see Protect SAP HANA Deployments.

    *   To replicate jobs, select the replication option, while creating the Protection Group.
    *   In the source, after setting the source argument `–et-log-backup` to `true`, the log backups will not be displayed on the {{site.data.keyword.cloud_notm}} Backup and Recovery UI or be replicated for older Protection Groups. You must create a new Protection Group with the updated source.

4. Create a Full Backup for the first time.

Replication to the secondary {{site.data.keyword.cloud_notm}} Backup and Recovery cluster will be enabled after the first successful full backup, and log backups will be displayed on the {{site.data.keyword.cloud_notm}} Backup and Recovery UI.

Catalog backups are not displayed on the {{site.data.keyword.cloud_notm}} Backup and Recovery UI.

