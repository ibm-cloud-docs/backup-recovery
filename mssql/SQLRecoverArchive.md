---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: microsoft sql, recover, data protection

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Restore to original {{site.data.keyword.baas_full_notm}} (from where ms sql was archived)
{: #restore_to_original_{{site.data.keyword.baas_full_notm}}_from_where_ms_sql_was_archived}

## Recover/clone an entire snapshot (protection group run) or vm from the archive to the target vcenter
{: #recoverclone_an_entire_snapshot_protection_group_run_or_vm_from_the_archive_to_the_target_vcenter}

1. In the {{site.data.keyword.baas_full}} Console, select **Data Protection > Recoveries**.
2. Click **Recover** \> **Virtual Machines** \> **VMs**.
3. Select a Job or VM (crash consistent) to recover.
4. Select the Snapshot and recovery location.

## Instant mount a volume of a virtual or physical server from the archive to the target server
{: #instant_mount_a_volume_of_a_virtual_or_physical_server_from_the_archive_to_the_target_server}

1. In the {{site.data.keyword.baas_full_notm}} Console, select **Data Protection > Recoveries**.
2. Click **Recover** \> **Virtual Machines** \> **Instant Volume Mount**.
3. Select the Server for instant volume mount.
4. Select the recover point, volumes to mount and the mount target.
5. Copy the database files to the desired MS SQL server.
6. Manually attach the database to the desired MS SQL instance.

## Download individual files
{: #download_individual_files}

1. In the {{site.data.keyword.baas_full_notm}} Console, select **Data Protection > Recoveries**.
2. Click **Recover** \> **Virtual Machines** \> **Files or Folders**.
3. Select the database files to download.
4. Select the recover point and any additional recovery options.
5. Copy the database files to the desired MS SQL server.
6. Manually attach the database to the desired MS SQL instance.
