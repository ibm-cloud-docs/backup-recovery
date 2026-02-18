---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-17"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Extended retention
{: #extended_retention}


Extended retention allows specific snapshots to be kept for a longer period than the normal retention defined in the protection policy.

Using extended retention, you can preserve specific backups such as the first snapshot taken every hour, day, week, month, or year for compliance, auditing, or long-term archival purposes. The retention period can be set to a maximum value of **365,000 days**.

**Extended Retention Behavior Matrix**
The following tables describe how extended retention behaves based on the configured **backup schedule** and **extended retention rule**.


**1. High Frequency Backups (Minutes/Hours)**

| Backup | Extended Retention | Description | Example |
| --- | --- | --- | --- |
| Every | Retain the First Snapshot taken every |     |     |
| Minutes | Run | The first snapshot captured after every run is retained for the specified number of days in the extended retention policy. |     |
| Hours | The first snapshot captured during the specified hour is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured during the hour is retained for the number of days specified in the protection policy. | Define a protection policy that captures snapshots every 10 minutes and retains the snapshots for 14 days. Specify an extended retention policy to capture the first snapshot taken every hour and retain the snapshot for 28 days.<br><br>**6:38PM** – The first backup starts. The snapshot captured is retained for an extended period of 28 days because this is the first snapshot in the hour 6PM - 7PM.<br><br>**6:48PM** - The snapshot captured is retained for 14 days.<br><br>**6:58PM** - The snapshot captured is retained for 14 days.<br><br>**7:08PM** - The snapshot captured is retained for 28 days as this is the first snapshot in the hour 7PM - 8PM. |
{: caption="Table 1. Extended Retention Behavior Matrix - High Frequency" caption-side="bottom"}

**2. Daily Backups**

| Backup | Extended Retention | Description | Example |
| --- | --- | --- | --- |
| Days | The first snapshot captured on the specified day is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the protection policy. | Define a protection policy that captures snapshots every 10 minutes and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every day and retain it for 28 days.<br><br>**6:30PM (Day 1)** – The first backup starts. The snapshot captured is retained for an extended period of 28 days because this is the first snapshot for Day 1.<br><br>**6:40PM (Day 1)** - The snapshot captured is retained for 14 days.<br><br> **11:50PM (Day 1)** - The snapshot captured is retained for 14 days.<br><br>**12:00AM (Day 2)** - The snapshot captured is retained for an extended period of 28 days because this is the first snapshot for Day 2. |
| Days | Run | {{site.data.keyword.baas_full}} recommends configuring the Extended Retention policy always more than the scheduled backup policy. |     |
| Days | The first snapshot captured during the specified day is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures snapshots every day and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every 2 days and retain the snapshot for 28 days.<br><br>**7:00PM (Day 1)** – The first backup starts. The snapshot captured is retained for an extended period of 28 days.<br><br>**7:00PM (Day 2)** - The snapshot captured is retained for 14 days.<br><br>**7:00PM (Day 3)** - The snapshot captured is retained for an extended period of 28 days.<br><br>**7.00PM (Day 4)** - The snapshot captured is retained for 14 days. |
| Weeks | The first snapshot captured during the specified week is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the protection policy. | Define a protection policy that captures snapshots every 10 minutes and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every week and retain the snapshot for 28 days.<br><br>Assuming the week starts on Sunday:<br><br>**Week 1, Sunday  6:30PM** - The snapshot captured is retained for 28 days.<br><br>**Week 1, Thursday 8:30PM** - The snapshot captured is retained for 14 days. <br><br>**Week 1, Saturday 11:50PM** - The snapshot captured is retained for 14 days.<br><br>**Week 2, Sunday 12:00AM** - The snapshot captured is retained for 28 days. |
| Months | The first snapshot captured during the specified month is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the protection policy. | Define a protection policy that captures snapshots every 10 minutes and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every month and retain the snapshot for 28 days.<br><br>**January** - The first snapshot captured is retained for 28 days. Every subsequent snapshot captured in the month is retained for 14 days. <br><br>**February** - The first snapshot captured is retained for 28 days. Every subsequent snapshot captured in the month is retained for 14 days. |
{: caption="Table 2. Extended Retention Behavior Matrix - Daily" caption-side="bottom"}

**3. Weekly Backups**

| Backup | Extended Retention | Description | Example |
| --- | --- | --- | --- |
| Week | Run | {{site.data.keyword.baas_full_notm}} recommends scheduling the Extended Retention policy always more than the scheduled backup policy. |     |
| Weeks | The first snapshot captured during the specified week is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures every day and retains it for 14 days. Specify an extended retention policy to capture the first snapshot taken every week and retain the snapshot for 28 days.<br><br>Assuming the week starts on Sunday:<br><br>**Week 1, Sunday  6:30PM** - The snapshot captured is retained for 28 days.<br><br>**Week 2, Sunday 6:30PM** - The snapshot captured is retained for 28 days. |
| Weeks | The first snapshot captured during the specified week is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures snapshots every week and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every two weeks and retain the snapshot for 28 days.<br><br>Assuming the week starts on Sunday:<br><br>**Week 1, Sunday  6:30PM** - The snapshot captured is retained for 28 days.<br><br>**Week 2, Sunday 6:30PM** - The snapshot captured is retained for 14 days.<br><br>**Week 3, Sunday 6:30PM** - The snapshot captured is retained for 28 days. |
| Months | The first snapshot captured during the specified week is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures snapshots every week and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every month and retain the snapshot for 28 days.<br><br>Assuming the week starts on Sunday:<br><br>**Week 1, Sunday  6:30PM** - The snapshot captured is retained for 28 days.<br><br>**Week 2, Sunday 6:30PM** - The snapshot captured is retained for 14 days.<br><br>**Week 3, Sunday 6:30PM** - The snapshot captured is retained for 14 days.<br><br>**Week 4, Sunday 6:30PM** - The snapshot captured is retained for 14 days. |
{: caption="Table 3. Extended Retention Behavior Matrix - Weekly" caption-side="bottom"}

**4. Monthly Backups**

| Backup | Extended Retention | Description | Example |
| --- | --- | --- | --- |
| Month | Run | {{site.data.keyword.baas_full_notm}} recommends scheduling the Extended Retention policy always more than the scheduled backup policy. |     |
| Months | The first snapshot captured during the specified month is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures snapshots every day and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every month and retain the snapshot for 28 days.<br><br>**January** - The first snapshot captured is retained for 28 days. Every subsequent snapshot captured in the month is retained for 14 days. <br><br>**February** - The first snapshot captured is retained for 28 days. Every subsequent snapshot captured in the month is retained for 14 days. |
| Months | The first snapshot captured during the specified month is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures snapshots every month and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every two months and retain the snapshot for 28 days.<br><br>**January, February** - The first snapshot captured is retained for 28 days. Every subsequent snapshot captured in the month is retained for 14 days. <br><br>**March, April** - The first snapshot captured is retained for 28 days. |
{: caption="Table 4. Extended Retention Behavior Matrix - Monthly" caption-side="bottom"}


**Multiple Extended Retention Schedules**

You can create **one or more extended retention schedules** within a Protection Policy.

For example, you might define a protection schedule that captures hourly snapshots and retains them for **1 day**. This schedule maintains 24 sets of snapshots on a rotating basis. Each hour, a new snapshot is retained, and the oldest snapshot is deleted.

If you need to retain a subset of these snapshots for a longer period, you can configure an **additional extended retention schedule**. For instance, you could retain the first snapshot taken each day for **90 days**. In this case, the extended retention schedule maintains 90 daily snapshots on a rotating basis, with the oldest snapshot deleted as a new one is added.

**Multiple Rules Applied to the Same Snapshots**

If multiple extended retention rules apply to the same set of snapshots with different retention periods, the snapshots are retained for the **longest specified duration**.

For example, if two extended retention rules apply to the same snapshots, one with a retention period of 90 days and another with 180 days, the snapshots are retained for **180 days**.

**How Extended Retention Determines Eligible Snapshots**
For backup, archive, replication, and CloudSpin schedules, extended retention selects snapshots based on the following logic:
- **Weekly** – The first successful run that starts on **Sunday or later**.

- **Monthly** – The first successful run that starts on the **first day of the month or later**.

- **Yearly** – The first successful run that starts on the **first day of the year or later**.

For standard backup schedules, IBM Cloud Backup and Recovery also allows you to configure:

- Any day of the week for **weekly** extended retention

- Any day of the month for **monthly** extended retention

**Behavior When Scheduled Backups Fail**

If a scheduled backup that qualifies for extended retention fails, extended retention is applied to the **next successful scheduled snapshot**.

However, if a manual backup is performed in place of the failed scheduled backup after selecting all objects, extended retention is applied to that manual snapshot instead.

**Example**:

- Extended retention is configured to apply to the first snapshot on the first day of each month.

- The scheduled backup on February 1st fails.

- A manual backup is run successfully after selecting all objects (VMs).

- Extended retention is applied to the snapshot created by the manual backup.
