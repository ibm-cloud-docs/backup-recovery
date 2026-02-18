---

copyright:
  years: 2025
lastupdated: "2026-02-16"

keywords: data source connector, iks, roks, cluster, run now

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Protection group Run Now
{: #protection-group-run-now}

The **Run Now** feature allows you to immediately start a backup for the objects included in a Protection Group, without waiting for a scheduled policy.
This is useful for:
- Testing backups
- Taking an on‑demand snapshot before changes
- Rerunning backups after a failure
- Capturing incremental changes quickly

When you click **Run Now**, the system presents different backup options based on the protection group configuration and the state of the last backup.
You can manually start a protection run for the Kubernetes namespaces (objects) in your cluster. It provides you with options to:
- Back up all objects or only selected objects.
- Select an incremental or full backup.

## Starting a protection run
{: #protection-starting-run}

1. In the {{site.data.keyword.baas_full_notm}} instance dashboard, navigate to **Data Protection** \> **Protection**.
2. On the **Protection** page, locate the protection group you want to run the backup for.
3. Click the **Actions menu** (three dots) and select **Run Now**.
4. In the pop-up window, click the pencil icon to edit the run settings, and perform the following:
   - In the **Backup** list, select one of the following:
      - **Backup all Objects**: Backs up every object (namespace) defined in the group.
      - **Backup only selected Objects**: Allows you to choose specific objects to back up.
   - In the **Backup Type** list, select the backup type (Full or Incremental). By default, an archival backup is performed if configured.
5. Click **Run Now** to start the job.

## Pause future runs
{: #iks-roks-pause-run}

You can pause future scheduled runs for the objects in a protection group. If a protection run is currently executing, it will continue to completion, and only subsequent scheduled runs will be paused.

To pause future runs:
1. In the {{site.data.keyword.baas_full_notm}} instance dashboard, navigate to **Data Protection** \> **Protection**.
2. On the **Protection** page, locate the protection group you want to pause.
3. Click the **Actions menu** and select **Pause Future Runs**.

## Resume paused protection runs
{: #iks-roks-resume-run}

You can resume a paused protection run at any time. When you resume, if the previous backup snapshots have expired, a full backup may be triggered.

To resume a paused protection run:
1. In the {{site.data.keyword.baas_full_notm}} instance dashboard, navigate to **Data Protection** \> **Protection**.
2. On the **Protection** page, locate the protection group you want to resume.
3. Click the **Actions menu** and select **Resume**.

## Delete the object
{: #iks-roks-delete-object}

You can delete a protected object (namespace) if you:
- No longer want to retain the backups and snapshots generated for it.
- Want to reclaim the storage space used by the object.

You can delete an object in two ways:
- **Delete Object Only**: Removes the object from {{site.data.keyword.baas_full_notm}} management but preserves all existing backups until they expire naturally.
- **Delete Object and Snapshots**: Removes the object and immediately deletes all its local and archived snapshots.

After deleting objects and snapshots, the reclaimed storage space may take some time to appear on the {{site.data.keyword.baas_full_notm}} Dashboard.

To delete an object:
1. In the {{site.data.keyword.baas_full_notm}} instance dashboard, navigate to **Data Protection** \> **Protection**.
2. On the **Protection** page, locate the protection group containing the object.
3. Click the **Actions menu** and select **Delete**. (Note: The protection group is deleted only if the backup is paused).
4. In the **Delete Protection Group?** dialog box, select one of the following options:
   - Delete Object Only
   - Delete Object and Snapshots

## Backup types available under Run Now
{: #iks-roks-backup-types-run-now}

When triggering a manual backup, you may see the following backup types available:

| Backup Type | Details |
|--------|---------|
| **Full Backup** | Captures all selected data regardless of previous backup history. <ul><li>**Scope**: Can run for all objects, selected objects, or objects that succeeded in the last run.</li><li>**Suggested use**: Complete data copy, first backup of a new protection group, or to reset the incremental chain.</li></ul> |
| **Incremental Backup** | Captures only changed data since the last successful backup. <ul><li>**Scope**: Can run for all objects, selected objects, or succeeded objects (only if previous run had partial or full success).</li><li>**Suggested use**: Quick backups to protect recent changes.</li></ul> |

### Selection Options

The following options are available when choosing which objects to back up:

| Selection Option | Description | Availability |
|------------------|-------------|-------------------|
| **All Objects in the Protection Group** | Runs a backup (full or incremental) on every object that is defined in the group. Use when you want consistent coverage of the entire environment. | Always available. |
| **Selected Objects in the Protection Group** | Allows manual selection of objects to back up. Useful when backing up only a specific namespace or testing backups for a particular object. | Always available. |
| **All Objects that succeeded in the Last Run** | Uses the success list from the last run and backs up only those objects. Helps ensure consistency and avoids running backups on unknown or incomplete object sets. | **Appears when:** <ul><li>Last run succeeded.</li><li>Last run had partial success (e.g., 2 out of 5 namespaces succeeded).</li></ul> **Hidden when:** <ul><li>Previous run was cancelled.</li><li>Previous run failed completely.</li><li>No successful run exists.</li></ul> |
