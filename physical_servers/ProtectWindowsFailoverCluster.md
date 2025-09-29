---

copyright:
  years: 2024
lastupdated: "2025-08-27"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protecting shared and local drives using a file-based backup (volume backup)
{: #protecting_shared_and_local_drives_using_a_file-based_backup_volume_backup}

To create a file-based protection group for WFC, follow the steps listed below:

1. Install Backup agent on each node of the Windows Failover Cluster and register each Node of WFC as a separate Physical Server. For more information, see [Register or Edit a Physical Server](SourcePhysicalAdd.htm).

2. Create a Protection Group (physical server file-based). For more information, see [Add or Edit a Protection Group for Physical Servers](JobServerPhysical.htm).

3. Enable **Protect all local volumes** option on every node when creating a protection group.

4. Provide the volume path in **Exclusions** to exclude any volumes (local drives).


## Recovery
{: #recovery}

The shared drive could be on any of the WFC nodes at the time of backup, You have to search the volume on the backed-up physical servers. Once Volume is found it can be restored to either the original node or any other Physical Server registered with {{site.data.keyword.baas_full_notm}}. For more information, see [Recover Physical Servers](RecoverPhysAddRecoverTask.htm).
