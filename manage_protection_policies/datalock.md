---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Datalock
{: #datalock}

**DataLock** is a security and compliance feature in {{site.data.keyword.baas_full}} that enforces WORM (Write Once, Read Many) protection for backup data.

When DataLock is enabled, protected snapshots and archives cannot be modified or deleted until the configured lock period expires. This helps meet regulatory requirements and protects backup data from accidental or malicious deletion.

In the {{site.data.keyword.baas_full_notm}} instance dashboard, a lock icon is displayed next to policies and snapshots that are protected by DataLock.

## Features of datalock
{: #features_of_datalock}

With DataLock, you can:

- Enable retention locking for individual schedules within a Protection Policy (backup, archive, replication).
- Define a DataLock duration in `days`, `weeks`, `months`, or `years`.
- Prevent deletion of snapshots until the lock period expires.
- Apply different lock durations for different schedules within the same policy.

## Considerations
{: #considerations}

- The DataLock expiry date is tied to the original snapshot creation time.
- Extending the retention period of **existing snapshots** extends the DataLock expiry.
- Extending retention for **new snapshots** does not modify the DataLock expiry of previously created snapshots.
- Using long retention periods with DataLock, especially for auto-protected sources, can increase storage consumption.
- The DataLock retention period must always be **less than or equal to** the configured backup retention period.

## Policy
{: #policy}

The following table provides DataLock details when creating a protection policy by a user with a Data Security role (DSO) and a Non-Data Security role.


| Workflow | Data Security | Non-Data Security |
| --- | --- | --- |
| **Create Policy** | *   DataLock is enabled by default when creating a protection policy, replication target, archival target, and CloudSpin.<br>    <br>*   Disable DataLock for backup, replication, archive, and CloudSpin.<br>    <br>*   Edit both backup retention and the Lock retention period. | *   DataLock is enabled by default when creating a protection policy, replication target, archival target, and CloudSpin.<br>    <br>*   Disable DataLock for backup, replication, archive, and CloudSpin.<br>    <br>*   Edit both backup retention and the Lock retention period. |
| **Edit Policy** | *   Able to edit the DataLock setting for backup, replication, archival, and CloudSpin.<br>    <br>*   You can add a new replication, archival or CloudSpin target, and to add DataLock for a target, click **Add DataLock**.<br>    <br>*   Able to edit both backup retention and the Lock retention period.<br>    <br>*   Delete targets irrespective of the DataLock setting. | *   Unable to edit the DataLock setting for backup, replication, archival, and CloudSpin.<br>    <br>*   You can add a new replication, archival or CloudSpin target without DataLock because **Add DataLock** is disabled.<br>    <br>*   Unable to edit the Lock retention period but the backup retention can be modified.<br>    <br>*   Targets with DataLock enabled cannot be deleted. |
{: caption="" caption-side="bottom"}

## Protection
{: #protection}

The following table provides DataLock details when created by a user with a Data Security role (DSO) and a Non-Data Security role.


| Workflow | Data Security | Non-Data Security |
| --- | --- | --- |
| **Run Now** | *   Able to edit the backup retention period.<br>    <br>*   Able to add a new target but the DataLock setting is not available.<br>    <br>*   If you want to specify DataLock for a new target it must be enabled when creating or editing the protection policy.<br>    <br>*   Delete existing targets irrespective of the DataLock setting. | *   Able to edit the backup retention period.<br>    <br>*   Able to add a new target but the DataLock setting is not available.<br>    <br>*   If you want to specify DataLock for a new target it must be enabled when creating the protection policy.<br>    <br>*   Delete existing targets irrespective of the DataLock setting. |
| **Edit Run** | *   Able to edit the backup retention period.<br>    <br>*   Able to add a new target but the DataLock setting is not available. If you want to add DataLock to a new target it must be enabled when creating or editing the protection policy.<br>    <br>*   Unable to delete existing targets.<br>    <br>*   Delete the DataLocked snapshot only if the Lock retention period expires or DataLock is disabled on the snapshot. | *   Able to edit the backup retention period.<br>    <br>*   Able to add a new target but the DataLock setting is not available. If you want to add DataLock to a new target it must be enabled when creating the protection policy.<br>    <br>*   Unable to delete existing targets.<br>    <br>*   Delete the DataLocked snapshot if the Lock retention period expires or DataLock is disabled on the snapshot. |


## Upgrade
{: #upgrade}


| Upgraded Cluster | New Cluster |
| --- | --- |
| *   No change to the existing system and user-created policies.<br>    <br>*   DataLock is enabled by default when creating a new policy.<br>    <br>*   You can disable DataLock on specific targets (Backup, Replication, Archival, and CloudSpin) when creating a new policy irrespective of the user’s role. | *   DataLock is enabled by default for both system policy and new policy created by a user.<br>    <br>*   You can disable DataLock on specific targets (Backup, Replication, Archival, and CloudSpin) when creating a new policy irrespective of the user’s role. |
