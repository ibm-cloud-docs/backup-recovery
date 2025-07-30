---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: physical server, protection group, backup agent, policies

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Physical servers
{: #physical_servers}


After installing the Backup agent on a physical Server and registering it with the {{site.data.keyword.baas_full}}, the {{site.data.keyword.baas_full_notm}} can use a file-based Protection Group to back up the physical Server. Protection Groups can run repetitively on a schedule.

To learn more about the relationship between Protection Groups and Policies, see [Manage Protection Policy](/docs/allowlist/backup-recovery?topic=backup-recovery-manage_protection_policy). For instructions on how to back up Physical Servers, see [Manage Protection Groups](/docs/allowlist/backup-recovery?topic=backup-recovery-manage_protection_groups).

For example, you can create a block-based Protection Group with a protection schedule that backs up all the critical Servers every hour as represented by the _Critical_ Protection Group.

During a Protection Group Run, Snapshots of the Servers are saved on the {{site.data.keyword.baas_full_notm}} with a date and time stamp.

For instructions on how to back up Physical Servers, see [Add or Edit a Protection Group for Physical Servers](/docs/allowlist/backup-recovery?topic=backup-recovery-add_or_edit_a_protection_group_for_physical_servers).
