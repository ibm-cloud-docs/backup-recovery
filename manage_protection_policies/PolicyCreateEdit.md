---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Create or edit a standard policy
{: #create_or_edit_a_standard_policy}

A protection policy is a reusable set of settings that define how and when objects are protected, replicated and archived. You can select a standard policy to use when configuring a Protection Group. A Protection Group uses the schedules and settings defined in the policy to determine when and how backups are captured, archived or replicated.

Use the default standard policies or create your own custom policies. Policies save time because you do not need to repetitively enter settings.

Policy creation and editing operations treat a month as having 31 days instead of 30 days, which was the definition in older releases.

**To create or edit a standard policy:**

On the **Create Policy** page, select the target type and configure the advanced settings by clicking the respective icons from the floating menu when creating a policy.

On the **Create Policy** page you can change underlined numeric fields by over typing them or using the up and down arrow keys on your keyboard.

1. Navigate to **Data Protection > Policies**.
2. Click **Create Policy** located at the top right of the page..

    To edit an existing policy in the list, click the actions menu and select **Edit**.

    

    For {{site.data.keyword.baas_full}}, the policy **Description** cannot be set or edited.
3. **Policy Name:** Specify a protection policy name. The name can contain the alphanumerics, underscores, hyphens, periods, and spaces. This field is required and can be changed later.

4. Specify when backups are captured:

    *   **Backup every xx**: Specify the minutes, hours, weeks, months, or years to set the frequency for how often backups are captured by the Protection Group. This defines the protection schedule for capturing backups.

        The policy schedule can be configured to run on specific days. From the **Every** drop-down, select **Week** and in the **On** field, select the days of the week you want to run the backup. For example, you can schedule a backup to run on Monday, Wednesday, and Friday, and not on Tuesday and Thursday.

        The policy schedule can be configured to run on specific dates as well (calendar-based scheduling). From the **Every** drop-down, select **Month**, and in the **On** field, select the **Date**. You can then choose the date of the month you want to run the backup. For example, you can schedule a backup to run on the 5th of every month.




7. Click **More Options** to configure the additional policy settings.

8. **DataLock:** Toggle on to lock the policy and any snapshots that belong to Protection Groups associated with the policy. DataLock is typically used for compliance and regulatory purposes. For more information, see [DataLock](/docs/backup-recovery?topic=backup-recovery-datalock).

    If you have enabled the **DataLock** option at the policy level, then you have the option to lock the backup retained on the {{site.data.keyword.baas_full_notm}}. In the **Lock** option field available in the **Primary Copy** pane, specify how many days, weeks, months, or years the retained backups must be locked. You cannot delete the snapshots during the specified lock period.

9. **Periodic Full Backup**: Click the **Periodic Full Backup** icon from the floating menu to add a periodic full backup. You can schedule a policy to capture the periodic full backup for:

    *   Day

    *   Week

    *   Month

    *   Year


    You can also schedule multiple periodic full backups with different frequencies and dates.

    *   The policy schedule can be configured to run on specific dates as well (calendar-based scheduling). From the **Every** drop-down, select **Month**, and in the **On** field, select the **Date**. You can then choose the date of the month you want to run the backup. For example, you can schedule a backup to run on the 5th of every month.
    *   If you specify multiple periodic full backup schedules for the same set of snapshots with different retention periods, the snapshots are retained for the longest specified period.

    *   {{site.data.keyword.baas_full_notm}} recommends a first full and incremental forever approach to backup all workloads (sources) with the exception for Oracle(SBT based backup) and SQL (VDI-based) where {{site.data.keyword.baas_full_notm}} recommends periodic full backups.



11. **Quiet Times:** Select this option from the **Backup Options** floating menu to add a quiet time. Quiet Times define time periods when new protection runs are not started. For example, you may want to configure hourly backups that run during weekdays but not on Saturday or Sunday. Quiet Times only prevent new protection runs from starting during the specified time period. Protection runs that started before the quiet time period are not affected and will continue to run. For example, if a protection run starts at 12:45 AM on Saturday morning and there is a weekend quiet time period that starts at 1 AM on Saturday morning, the protection run will continue to run past 1 AM. No new protection runs are started during the quiet times. Similarly, for jobs set on an interval basis, such as incremental jobs running every 6 hours, the job will start immediately after the Quiet Time ends. For example, if jobs are scheduled at 6 AM, 12 PM, 6 PM, and 12 AM, and a Quiet Time is set from 11:30 AM to 1:30 PM, the job originally planned for noon will instead commence at 1:31 PM. Once the run commences at 1:31 PM, the next run will take place at 7:30 PM and not 6 PM (6 hours difference).

    You can add multiple quiet times. For example, you could define a quiet time period for Saturday and Sunday and another quiet time period every morning from 9:00 AM to 11:00 AM.

    If you update the Quiet Time periods in a policy for a running Protection Group, only new protection runs using the policy are affected and any existing Protection runs that have already started will use the original Quiet Time periods.

12. **Retry Options:** Select **Customize Retries** from the **Backup Options** floating menu to customize the retry settings for capturing snapshots. By default, the {{site.data.keyword.baas_full_notm}} attempts to capture Snapshots three times before the protection run fails. The default time between retries is five minutes. You can optionally customize the number of retries and how long to wait between each attempt.

13. **Log Backup (Databases):** Select **Log Backup** from the **Backup Options** floating menu to add a database logs schedule if you plan on restoring databases to a specific point in time between two full database server backups. If this policy has an archival schedule, this retention period also applies to the archived logs.

    For example, if you backup hourly at 5 minutes after the hour but you want the ability to restore a database to its state at 10:15 AM, specify a database logs schedule. The log backup saves all the transaction logs of all the databases so you can roll back to any point in time. Transaction logs are cumulative, so all the logs are backed up. When a database is added to a database server protected by {{site.data.keyword.baas_full_notm}}, its database logs are not backed up until a full database server backup has occurred.

    1. **Backup every xx:** Select the time unit for how often log backups are captured by the Protection Group.

    2. **Retain for xx days:** Specify how many days log backups are retained on the {{site.data.keyword.baas_full_notm}} before they are deleted. If this policy has a Replication schedule, this retention period also applies to the logs replicated to the target cluster.

        The **Retain** option can be set to a maximum value of 365000.

    3. **Lock**: Specify how many days, weeks, months, or years the retained log backups should be locked. You cannot delete the snapshots during the specified period.

        This option is available only if you have enabled the **DataLock** option at the policy level.


    For SQL Protection Groups, if the log backups are configured in the Protection Group, all protection runs are replicated ignoring the set policy.

    

18. Click **Create**.

When editing an existing policy that is configured to capture backup snapshots on a minutes or hourly time schedule and you change the schedule to occur daily, weekly or monthly, you are prompted to specify a start time for each of the Protection Groups that use this policy.

The policy is immediately available to use in Protection Groups.

*   For {{site.data.keyword.baas_full_notm}}, on NoSQL platforms, when you pause the backup job for a longer time than the retention period, the last successful snapshot will not be deleted even after the retention period expired on that snapshot.

*   For {{site.data.keyword.baas_full_notm}}, the last snapshot is always retained on HDFS/HBase/Hive, MongoDB, Cassandra, and CouchBase platforms.


## Considerations
{: #considerations}

*   By default, all backups are incremental unless manually switched to Full. However, if no previous incremental backup exists for a specific object, a full backup will be automatically initiated, potentially generating warnings for certain applications.

*   When scheduling a continuous backup policy, the protection run does not honor the start time.

    For example, specify a policy to capture the continuous backup at 9 AM every day with a periodicity of 7 hours.



| Days | Description |
| --- | --- |
| Day 1 | *   First backup - 9 AM<br>    <br>*   Second backup - 4 PM (after 7 hours)<br>    <br>*   Third backup - 11 PM |
| Day 2 | *   First backup - 6 AM<br>    <br>*   Second backup - 1 PM<br>    <br><br>In this example, on Day 2 the backup did not start at 9 AM. |
{: caption="" caption-side="bottom"}
