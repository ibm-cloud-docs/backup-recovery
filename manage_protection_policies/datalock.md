---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Datalock
{: #datalock}


DataLock is the Cohesity WORM (write once read many) feature that locks and retains files for compliance and regulatory purposes and to protect the files from accidental deletion or modification.

DataLock enables your storage to be WORM-compliant. When it is enabled, your data such as backups and archives cannot be tampered with or deleted. This results in the prevention of ransomware attacks on your clusters.

The {{site.data.keyword.baas_full_notm}} Console displays a lock icon to indicate DataLocked policies and snapshots.

## Features of datalock
{: #features_of_datalock}

*   You can apply DataLock for each schedule (such as backup, extended retention, archive, and replication) defined in the Protection Policy.

*   You can specify DataLock duration in days, weeks, months, or years for each schedule defined in the Protection Policy.


## Considerations
{: #considerations}

*   The DataLock expiry date:

    *   Doesn’t get extended if you extend the snapshot retention time of the runs that were created after the extension.

    *   Gets extended if you extend the snapshot retention time of the runs that were created before the extension.

*   Using DataLock with longer snapshot retention periods and for auto-protected sources can result in the cluster running out of space.

*   DataLock retention must be less than or equal to backup retention.


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
{: caption="" caption-side="bottom"}

## Upgrade
{: #upgrade}


| Upgraded Cluster | New Cluster |
| --- | --- |
| *   No change to the existing system and user-created policies.<br>    <br>*   DataLock is enabled by default when creating a new policy.<br>    <br>*   You can disable DataLock on specific targets (Backup, Replication, Archival, and CloudSpin) when creating a new policy irrespective of the user’s role. | *   DataLock is enabled by default for both system policy and new policy created by a user.<br>    <br>*   You can disable DataLock on specific targets (Backup, Replication, Archival, and CloudSpin) when creating a new policy irrespective of the user’s role. |
{: caption="" caption-side="bottom"}
