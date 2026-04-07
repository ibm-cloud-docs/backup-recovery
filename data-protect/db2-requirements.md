---

copyright:
  years: 2026
lastupdated: "2026-04-07"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Requirements for registering your Db2 database on an {{site.data.keyword.cloud_notm}} VSI cluster
{: #db2_requirements}

To register your Db2 source on an {{site.data.keyword.cloud_notm}} VSI cluster, be sure that you meet the {{site.data.keyword.baas_full_notm}} service prerequisites, and complete the steps that follow.

## Prerequisites
{: #db2_prerequisites}

- An active {{site.data.keyword.cloud_notm}} VSI cluster
- At least one active rigel connection that is attached to an {{site.data.keyword.cloud_notm}} VSI cluster
- A virtual machine with Db2 installed on it
- Db2-connector and backup agent installed on a virtual machine

## Registering the Db2 database on an {{site.data.keyword.cloud_notm}} VSI cluster
{: #db2_registering_database}

To protect your Db2 database on an {{site.data.keyword.cloud_notm}} VSI cluster, you must first register your Db2 database on an {{site.data.keyword.cloud_notm}} VSI cluster as a source. After registering your source, you can also perform the following actions:
- Update the Db2 database VM source configuration
- Refresh the source details
- Unregister the Db2 database vm source

1. Navigate to the data protection and sources.
2. Click Register Source → Db2 → start Registration.
3. Select/create rigel connection.
4. Complete all the details, and click complete.
5. Wait for several minutes to get the entity hierarchy loaded.


**Next** > [Protect and Recover Db2 on an {{site.data.keyword.cloud_notm}} VSI cluster](/docs-draft/backup-recovery?topic=backup-recovery-db2_protect_recover).
