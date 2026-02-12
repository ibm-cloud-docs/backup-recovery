---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Manage policies
{: #manage_policies}

A Protection Policy must be associated with a **Protection Group** to protect workloads and capture backups.

You can use the provided default policies or create custom policies to meet specific scheduling and retention requirements.

From the Policies page in the IBM Cloud Backup and Recovery instance dashboard, you can create, view, copy, edit, and delete Protection Policies. Each policy includes an Actions menu that allows you to view settings, copy, edit, or delete the policy as needed.



| Column Name | Description / Action |
| --- | --- |
| **Policy Name** | Click the policy name to view detailed settings. |
| **Organization** | Organization or account to which the policy belongs. |
| **Backup** | Indicates that snapshots are stored locally in {{site.data.keyword.baas_full_notm}} with one or more retention rules applied. |
| **Archive** | Indicates that snapshots are archived to a configured cloud archive target. |
| **Actions** | Provides options to manage the policy (Copy, Edit, Delete).<br><br>*   **Copy** —Create a new policy using the settings of an existing policy as a template.<br>*   **Edit** —Configure the settings of an existing policy. Follow the instructions provided in [Create or Edit a Standard Policy](/docs/backup-recovery?topic=backup-recovery-create_or_edit_a_standard_policy).<br>*   **Delete** —Delete this policy. If a Protection Group is based on a policy and the policy is deleted, the Protection Group keeps the original settings and is not deleted. |
{: caption="Policy settings " caption-side="bottom"}

For jobs protecting servers, indexing is set at the Protection Group level.

## Default policies
{: #default_policies}

{{site.data.keyword.baas_full_notm}} provides several predefined Protection Policies that can be used without additional configuration.

*   Gold Policy
    *   Capture a snapshot every `4 hours`
    *   Retain snapshots for `7 days`
    *   Extended retention:
       - First snapshot each day retained for `90 days`
       - First snapshot each week retained for `365 days`
       - First snapshot each month retained for `1095 days`
    *  Retry behavior: 3 retries with 5-minute intervals

*   Silver
    *   Capture a snapshot every `12 hours`
    *   Retain snapshots for `14 days`
    *   Extended retention:
       - Daily snapshot retained for `90 days`
       - Weekly snapshot retained for `365 days`
       - Monthly snapshot retained for `730 days`
    *   Retry behavior: `3 retries` with `5-minute intervals`

*   Bronze
    *   Capture a snapshot `once per day`
    *   Retain snapshots for `30 days`
    *   Extended retention:
       - Weekly snapshot retained for `90 days`
       - Monthly snapshot retained for `365 days`
    *   Retry behavior: `3 retries` with `5-minute intervals`
*   Protect Once
    *   Capture a `single full backup`
    *   Retain for `14 days`


DataLock is enabled by default for certain default policies to support compliance requirements. For more details, see the DataLock documentation.
{: note}
