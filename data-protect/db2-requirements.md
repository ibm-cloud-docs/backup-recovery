---

copyright:
  years: 2026
lastupdated: "2026-08-19"

keywords: db2, database, backup, recovery, VSI, registration, requirements, prerequisites, IBM Db2

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Requirements for Db2 source registration in Backup Recovery Instance on {{site.data.keyword.cloud_notm}} VSI cluster
{: #db2_requirements}

To register your Db2 source in a {{site.data.keyword.baas_full_notm}} instance on an {{site.data.keyword.cloud_notm}} cluster, be sure that you meet the service prerequisites, and complete the steps that follow.

## Prerequisites
{: #db2_prerequisites}

To register a DB2 source, you need:

- **A Data Source connector** deployed in the data center that hosts the DB2 source. The Connector facilitates communication between the application server source and the Backup and Recovery Service.
- **The hostname or IP address** of the DB2 source.
- **The Agent** installed on the DB2 source.

Additionally, ensure you have:

- An active {{site.data.keyword.cloud_notm}} VSI cluster
- At least one active rigel connection that is attached to an {{site.data.keyword.cloud_notm}} VSI cluster
- A virtual machine with Db2 installed on it

## Registering the Db2 database on an {{site.data.keyword.cloud_notm}} VSI cluster
{: #db2_registering_database}

To protect your Db2 database on an {{site.data.keyword.cloud_notm}} VSI cluster, you must first register your Db2 database as a source. After registering your source, you can also perform the following actions:
- Update the Db2 database VM source configuration
- Refresh the source details
- Unregister the Db2 database VM source

1. Navigate to the data protection → sources.
2. Click **Register Source** → **Db2** → **start Registration**.
3. Select/create rigel connection.
4. Complete all the details, and click complete.
5. Wait for several minutes to get the entity hierarchy loaded.

**Next** > [Protect and Recover Db2 on an {{site.data.keyword.cloud_notm}} VSI cluster](/docs/backup-recovery?topic=backup-recovery-db2_protect_recover).
