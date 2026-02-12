---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Create or edit a standard policy
{: #create_or_edit_a_standard_policy}

A **Protection Policy** is a reusable set of configuration settings that defines how and when data sources are protected, replicated, and archived. Protection Groups use the schedules and settings defined in a policy to determine when backups are captured and how long they are retained.

Using Protection Policies simplifies management by allowing the same settings/Policies to be applied across multiple Protection Groups.

**To create or edit a standard policy:**

1. Navigate to `Data Protection` \> `Policies` in the IBM Cloud Backup and Recovery instance dashboard.

2. Click `Create Policy` to create a new policy.

3. To modify an existing policy, open the `Actions menu` next to the policy and select `Edit`.

4. On the `Create Policy` page, configure the required and advanced settings as described below.

5. Underlined numeric fields can be edited directly by typing a value or by using the up and down arrow keys.

6. Click `Create` (or `Save` when editing) to apply the changes.

In {{site.data.keyword.baas_full_notm}}, the Policy Description field is not available and cannot be edited.
{: note}

## Policy configuration overview
{: #policy-configuration-overview}

When creating or editing a Protection Policy, you configure both required and optional settings that determine how backups are scheduled, retained, and managed.

The following table summarises all available policy options with additional details.

| **Option**               | **Description** |
|--------------------------|-----------------|
| **Policy Name**         | Unique name used to identify the policy. |
| **Backup every xx**     | Defines how frequently backups are captured (minutes, hours, days, weeks, months, or years). |
| **Retain for xx**       | Specifies how long backup snapshots are retained before automatic deletion. |
| **Schedule Type**       | Allows interval-based, day-based, or calendar-based scheduling. |
| **DataLock**            | Prevents deletion of snapshots for a defined lock period for compliance purposes. |
| **Periodic Full Backup**| Adds scheduled full backups in addition to incremental backups. |
| **Quiet Times**         | Defines time periods when new backup jobs should not start. |
| **Retry Options**       | Configures the number of retries and wait time for failed backup jobs. |
| **Log Backup (Databases)** | Configures separate schedules and retention for database transaction logs. |

## Detailed Configuration Options
{: #detailed-configuration}

The following sections describe each option in detail.

**Required Settings**

| **Setting** | **Description** |
|-------------|-----------------|
| **Policy Name** | Specify a unique name for the Protection Policy. <br>- The name can contain alphanumeric characters, underscores, hyphens, periods, and spaces. <br>- This field is required and can be changed later.<br>- The Policy Description field is not supported and cannot be set or modified in IBM Cloud Backup and Recovery. |
| **Backup Schedule (Backup every xx)** | Defines how often backups are captured by Protection Groups using this policy. You can configure schedules in several ways:<br>• **Interval‑based scheduling** – Run backups every specified number of minutes, hours, or days.<br>• **Day‑based scheduling** – Run backups on specific days of the week (for example, Monday, Wednesday, and Friday).<br>• **Calendar‑based scheduling** – Run backups on specific dates within a month (for example, the 5th of every month).<br> These options allow precise control over when backups are captured.|
| **Retention Period (Retain for xx)** | Specifies how long backup snapshots are kept on IBM Cloud Backup and Recovery before they are automatically deleted.. |

**Advanced options**

Click `More Options` on the Create Policy page to configure the following additional settings.

| **Option** | **Description** |
|-----------|-----------------|
| **DataLock** | - Enables retention locking to prevent deletion of backups for a specified period.<br>- Commonly used to meet regulatory and compliance requirements.<br>- Only users with appropriate permissions can enable or modify DataLock settings.<br>- Once locked, snapshots cannot be deleted until the lock period expires. |
| **Periodic Full Backup** | - Adds scheduled full backups in addition to incremental backups.<br>- Full backups can be scheduled by **day**, **week**, **month**, or **year**.<br>- Multiple periodic full backup schedules can be configured with different frequencies.<br> **Recommendation**:<br> {{site.data.keyword.baas_full_notm}} recommends a “first full and incremental forever” approach for most workloads. Periodic full backups are mainly recommended for Oracle (SBT) and SQL (VDI) based backups. |
| **Quiet Times** | - Defines time periods when **new backup jobs should not start**.<br>- Jobs already running when a Quiet Time begins will continue to completion.<br>- Multiple Quiet Time periods can be configured (for example, weekends or specific daily hours).<br>- Jobs scheduled during a Quiet Time will automatically start once the Quiet Time ends. |
| **Retry Options** | - Allows customization of retry behavior for failed backup jobs.<br>- By default, the system retries three times with a five-minute interval between attempts.<br>- You can modify: `Number of retries` and `Wait time between retries`. |
| **Log Backup (Databases)** | For database workloads that require point-in-time recovery:<br>- **Backup every xx** – Defines how frequently database transaction logs are captured.<br>- **Retain for xx days** – Specifies how long log backups are retained.<br>- **Lock** - (Available only when DataLock is enabled) prevents deletion of log backups for the configured period.<br> Log backups allow restoration of databases to a specific point in time between full backups.<br> **Notes**<br>- Log backups begin only after the first full database backup is completed.<br>- Maximum retention value for log backups is **365000 days**.<br>- If a replication schedule exists, the same retention applies to replicated logs. |
| **Extended Retention** | - Allows selected backups to be kept longer than the standard policy retention.<br>- Supports long-term retention schedules such as: weekly, monthly, and yearly.<br> This is useful for compliance and archival requirements.|

## Considerations
{: #iks-roks-considerations}

- By default, backups are incremental unless explicitly configured as full.

- If no previous backup exists for an object, a full backup is automatically initiated.

- Changing a policy from an hourly schedule to a daily or weekly schedule may require specifying a new start time.

- Policy scheduling treats each month as having **31 days**.

- On certain NoSQL platforms, the **last successful snapshot is always retained**, even if the retention period has expired.
