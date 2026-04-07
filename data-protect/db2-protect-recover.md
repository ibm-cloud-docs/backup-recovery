---

copyright:
  years: 2026
lastupdated: "2026-04-07"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protect and recover Db2 on an {{site.data.keyword.cloud_notm}} VSI cluster
{: #db2_protect_recover}

## Protect Db2 on an {{site.data.keyword.cloud_notm}} VSI cluster
{: #db2_protect}

1. Go to to data protection → protection.
2. Click protect → Database → Db2.
3. Click Add Objects and choose the respective source from drop-down list.
4. Choose an instance name or specific database to protect from the entity hierarchy.
5. Complete the protection group name and policy.
   1. Click protect and the protection group is created.
   2. Click more options and it opens a dialog box to set more of the protection configuration.
   3. From here, you can choose whether to enable or disable LOGARCHMETH2; by default, LOGARCHMET1 is enabled.

## Recover Db2 on an {{site.data.keyword.cloud_notm}} VSI cluster
{: #db2_recover}

1. Go to data protection → Recoveries.
2. Click Recover → Databases → Db2.
3. Choose the wanted backup from the list by using the multiple filter option.
4. Selected wanted recovery options and start recovery.
5. Recovery progress is shown on the recovery detail page.
