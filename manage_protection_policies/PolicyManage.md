---

copyright:
  years: 2024
lastupdated: "2024-10-8"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Manage policies
{: #manage_policies}

A standard policy must be associated with a Protection Group to protect objects.

Use the default standard policies or create your own custom policies. Policies save time because you do not need to repetitively enter settings. For more information, see [Default Policies](#Default).

From the policy manager, you can create a new protection policy. For more information, see [Create or Edit a Standard Policy](PolicyCreateEdit.htm).

You can also view policy settings, copy, edit and delete policies using the actions for each policy.

| Column Name | Description / Action |
| --- | --- |
| **Policy Name** | Drill-down on this link to view the settings for the policy. |
| **Organization** | The name of the organization to which the policy belongs. |
| **Backup** | Snapshots are stored in the current (local) {{site.data.keyword.baas_full_notm}} and there are one or more extended retention rules. |
| **Archive** | Snapshots are stored in the current (local) {{site.data.keyword.baas_full_notm}} and are archived in the cloud. |
| **Actions** | To manage an existing policy, click the actions menu and select one of the following options:<br><br>*   **Copy** —Create a new policy from an existing policy.<br>*   **Edit** —Configure the settings of an existing policy. Follow the instructions provided in [Create or Edit a Standard Policy](/docs/allowlist/backup-recovery?topic=backup-recovery-create_or_edit_a_standard_policy).<br>*   **Delete** —Delete this policy. If a Protection Group is based on a policy and the policy is deleted, the Protection Group keeps the original settings and is not deleted. |
{: caption="" caption-side="bottom"}

For jobs protecting servers, indexing is set at the Protection Group level.

## Default policies
{: #default_policies}

The following default standard policies are provided:

*   Gold
    *   Take a snapshot every 4 hours and retain it for 7 days.
    *   The first snapshot taken every day is retained for 90 days.
    *   The first snapshot taken every week is retained for 365 days.
    *   The first snapshot taken every month is retained for 1095 days.
    *   Retry capturing snapshots 3 times every 5 minutes before reporting an error
*   Silver
    *   Take a snapshot every 12 hours and retain it for 14 days.
    *   The first snapshot taken every day is retained for 90 days.
    *   The first snapshot taken every week is retained for 365 days.
    *   The first snapshot taken every month is retained for 730 days.
    *   Retry capturing snapshots 3 times every 5 minutes before reporting an error
*   Bronze
    *   Take a snapshot every day and retain it for 30 days.
    *   The first snapshot taken every week is retained for 90 days.
    *   The first snapshot taken every month is retained for 365 days.
    *   Retry capturing snapshots 3 times every 5 minutes before reporting an error
*   Protect Once
    *   Take a FULL snapshot and retain it for 14 days


DataLock is typically used for compliance and regulatory purposes. Datalock is enabled by default when creating the default protection policy. For more information, see [DataLock](datalock.htm).
