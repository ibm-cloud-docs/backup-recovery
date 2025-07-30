---

copyright:
  years: 2025
lastupdated: "2025-07-30"

keywords: about, core components

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Overview
{: #sap_hana}

{{site.data.keyword.cloud_notm}} Backup and Recovery natively integrates with SAP HANA to provide backup and recovery for clustered SAP HANA deployments. Using {{site.data.keyword.cloud_notm}} Backup and Recovery Connector Agent, you can backup and restore the SAP HANA database. The {{site.data.keyword.cloud_notm}} Backup and Recovery Connector Agent provides a simple workflow to back up and restore your SAP HANA deployment.

To protect SAP HANA database using the {{site.data.keyword.cloud_notm}} Backup and Recovery BACKINT plug-in Remote Adapter Jobs, see [About SAP HANA Database Backups](/docs/allowlist/backup-recovery?topic=backup-recovery-backup_sap_hana_database).

{{site.data.keyword.cloud_notm}} Backup and Recovery provides a SAP HANA Connector for backup and restore of enterprise SAP HANA deployments. The SAP HANA Connector leverages the [backup](https://help.sap.com/docs/PRODUCT_ID/6b94445c94ae495c83a19646e7c3fd56/c3a57273bb571014a747a289a3198e15.html?state=PRODUCTION&version=2.0.04&locale=en-US){: external} and [restore](https://help.sap.com/docs/PRODUCT_ID/6b94445c94ae495c83a19646e7c3fd56/c3c66b63bb571014b3e5ad8618cda1ad.html?state=PRODUCTION&version=2.0.02&locale=en-US){: external} capabilities provided by SAP HANA.

The SAP HANA Connector enables you to back up and restore both TenantDB and SystemDB. It provides:

- Backup:
  - Full backup
  - Incremental backup
  - Differential backup
  - Automatic Log backup

- Restore:
  - Restore the database to its most recent state.
  - Restore the database to the point in time.
  - Restore the database to a specific data backup.
  - Alternate restore of SAP HANA database
    - Alternate restore to new database or active database on the same SAP HANA system.
    - Alternate restore to a new or active database on a different SAP HANA system.


## Core components
{: #core_components}

The following are the core components for protecting SAP HANA databases:

- SAP HANA Linux machine
- SAP HANA Connector
- {{site.data.keyword.cloud_notm}} Backup and Recovery Connector Agent
- {{site.data.keyword.cloud_notm}} Backup and Recovery View

You can integrate the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster with your SAP HANA deployments using the SAP HANA Connector and {{site.data.keyword.cloud_notm}} Backup and Recovery Connector Agent.
{: note}


