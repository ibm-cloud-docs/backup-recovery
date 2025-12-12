---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: microsoft sql, recover, backup,

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Recover microsoft sql server
{: #recover_microsoft_sql_server}


{{site.data.keyword.baas_full}} provides the ability to recover the MS SQL databases protected by a Protection Group.

*   [Recover Microsoft SQL Server Database Over the Original Database](/docs/backup-recovery?topic=backup-recovery-recover_microsoft_sql_server_database_over_the_original_database).
*   [Recover Microsoft SQL Server Database as a New Database in the Original Microsoft SQL Server Instance](/docs/backup-recovery?topic=backup-recovery-recover_microsoft_sql_server_database_as_a_new_database_in_the_original_microsoft_sql_server_instance).
*   [Recover Microsoft SQL Server Database as a New Database in an Alternate Microsoft SQL Server Instance](/docs/backup-recovery?topic=backup-recovery-recover_microsoft_sql_server_database_as_a_new_database_in_an_alternate_microsoft_sql_server_instance).

## Prerequisites
{: #prerequisites}

*   Before you can restore MS SQL databases, a full backup must exist.
*   (_Applies for volume-based and VDI-based backed up data._) The cluster must be a member of the same or any trusted AD domain to which the MS SQL database is restored.

*   {{site.data.keyword.baas_full_notm}} VIPs must be reachable (DNS) and accessible (Ping) from the target MS SQL server.

*   For recovery, the server and the cluster must be on the same VLAN.


The following topics provide guidance to help you restore MS SQL databases:
