---

copyright:
  years: 2025
lastupdated: "2025-12-12"

keywords: extended, retention

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Extended retention
{: #extended_retention}


Snapshots selected for extended retention are retained for a longer time than defined in the protection schedule. You can retain the first snapshot of the backup run taken on the specified hours, every run, days, weeks, months, or years for a longer period of time.

The retention period can be set to a maximum value of 365000 days.

![](../../Resources/Images/extended-retentions/extended-retentions.PNG)

The table below lists the options available for protection policy and extended retention policy and the respective outcome.


| Backup | Extended Retention | Description | Example |
| --- | --- | --- | --- |
| Every | Retain the First Snapshot taken every |     |     |
| Minutes | Run | The first snapshot captured after every run is retained for the specified number of days in the extended retention policy. |     |
| Hours | The first snapshot captured during the specified hour is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured during the hour is retained for the number of days specified in the protection policy. | Define a protection policy that captures snapshots every 10 minutes and retains the snapshots for 14 days. Specify an extended retention policy to capture the first snapshot taken every hour and retain the snapshot for 28 days.<br><br>**6:38PM** – The first backup starts. The snapshot captured is retained for an extended period of 28 days because this is the first snapshot in the hour 6PM - 7PM.<br><br>**6:48PM** - The snapshot captured is retained for 14 days.<br><br>**6:58PM** - The snapshot captured is retained for 14 days.<br><br>**7:08PM** - The snapshot captured is retained for 28 days as this is the first snapshot in the hour 7PM - 8PM. |
| Days | The first snapshot captured on the specified day is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the protection policy. | Define a protection policy that captures snapshots every 10 minutes and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every day and retain it for 28 days.<br><br>**6:30PM (Day 1)** – The first backup starts. The snapshot captured is retained for an extended period of 28 days because this is the first snapshot for Day 1.<br><br>**6:40PM (Day 1)** - The snapshot captured is retained for 14 days.<br><br> **11:50PM (Day 1)** - The snapshot captured is retained for 14 days.<br><br>**12:00AM (Day 2)** - The snapshot captured is retained for an extended period of 28 days because this is the first snapshot for Day 2. |
| Weeks | The first snapshot captured during the specified week is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the protection policy. | Define a protection policy that captures snapshots every 10 minutes and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every week and retain the snapshot for 28 days.<br><br>Assuming the week starts on Sunday:<br><br>**Week 1, Sunday  6:30PM** - The snapshot captured is retained for 28 days.<br><br>**Week 1, Thursday 8:30PM** - The snapshot captured is retained for 14 days. <br><br>**Week 1, Saturday 11:50PM** - The snapshot captured is retained for 14 days.<br><br>**Week 2, Sunday 12:00AM** - The snapshot captured is retained for 28 days. |
| Months | The first snapshot captured during the specified month is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the protection policy. | Define a protection policy that captures snapshots every 10 minutes and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every month and retain the snapshot for 28 days.<br><br>**January** - The first snapshot captured is retained for 28 days. Every subsequent snapshot captured in the month is retained for 14 days. <br><br>**February** - The first snapshot captured is retained for 28 days. Every subsequent snapshot captured in the month is retained for 14 days. |
| Days | Run | {{site.data.keyword.baas_full_notm}} recommends configuring the Extended Retention policy always more than the scheduled backup policy. |     |
| Hours |
| Days | The first snapshot captured during the specified day is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures snapshots every day and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every 2 days and retain the snapshot for 28 days.<br><br>**7:00PM (Day 1)** – The first backup starts. The snapshot captured is retained for an extended period of 28 days.<br><br>**7:00PM (Day 2)** - The snapshot captured is retained for 14 days.<br><br>**7:00PM (Day 3)** - The snapshot captured is retained for an extended period of 28 days.<br><br>**7.00PM (Day 4)** - The snapshot captured is retained for 14 days. |
| Weeks | The first snapshot captured during the specified week is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures every day and retains it for 14 days. Specify an extended retention policy to capture the first snapshot taken every week and retain the snapshot for 28 days.<br><br>Assuming the week starts on Sunday:<br><br>**Week 1, Sunday  6:30PM** - The snapshot captured is retained for 28 days.<br><br>**Week 2, Sunday 6:30PM** - The snapshot captured is retained for 28 days. |
| Months | The first snapshot captured during the specified month is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures snapshots every day and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every month and retain the snapshot for 28 days.<br><br>**January** - The first snapshot captured is retained for 28 days. Every subsequent snapshot captured in the month is retained for 14 days. <br><br>**February** - The first snapshot captured is retained for 28 days. Every subsequent snapshot captured in the month is retained for 14 days. |
| Week | Run | {{site.data.keyword.baas_full_notm}} recommends scheduling the Extended Retention policy always more than the scheduled backup policy. |     |
| Hours |
| Days |
| Weeks | The first snapshot captured during the specified week is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures snapshots every week and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every two weeks and retain the snapshot for 28 days.<br><br>Assuming the week starts on Sunday:<br><br>**Week 1, Sunday  6:30PM** - The snapshot captured is retained for 28 days.<br><br>**Week 2, Sunday 6:30PM** - The snapshot captured is retained for 14 days.<br><br>**Week 3, Sunday 6:30PM** - The snapshot captured is retained for 28 days. |
| Months | The first snapshot captured during the specified week is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures snapshots every week and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every month and retain the snapshot for 28 days.<br><br>Assuming the week starts on Sunday:<br><br>**Week 1, Sunday  6:30PM** - The snapshot captured is retained for 28 days.<br><br>**Week 2, Sunday 6:30PM** - The snapshot captured is retained for 14 days.<br><br>**Week 3, Sunday 6:30PM** - The snapshot captured is retained for 14 days.<br><br>**Week 4, Sunday 6:30PM** - The snapshot captured is retained for 14 days. |
| Month | Run | {{site.data.keyword.baas_full_notm}} recommends scheduling the Extended Retention policy always more than the scheduled backup policy. |     |
| Hours |
| Days |
| Weeks |
| Months | The first snapshot captured during the specified month is retained for the number of days as mentioned in the extended retention policy. Every subsequent snapshot captured is retained for the number of days specified in the backup policy. | Define a backup policy that captures snapshots every month and retains the snapshot for 14 days. Specify an extended retention policy to capture the first snapshot taken every two months and retain the snapshot for 28 days.<br><br>**January, February** - The first snapshot captured is retained for 28 days. Every subsequent snapshot captured in the month is retained for 14 days. <br><br>**March, April** - The first snapshot captured is retained for 28 days. |
{: caption="" caption-side="bottom"}

You can create one or more extended schedules.

For example, you could define a protection schedule that captures snapshots every hour and retains them on the {{site.data.keyword.baas_full_notm}} for 1 day. This protection schedule retains 24 sets of snapshots on a rotating basis, each hour a new set of snapshots are retained and the oldest set of snapshots is deleted.
If you want to retain a subset of these snapshots for a longer period of time, you could define an additional extended retention schedule to retain a single subset of snapshots daily for 90 days. This extended retention schedule retains 90 sets of snapshots on a rotating basis, each day a new set of snapshots are retained and the oldest set of snapshots is deleted.

If you specify multiple extended retention rules for the same set of snapshots with different retention periods, the snapshots are retained for the longest specified time period. For example if you define two extended retention rules that retain the same set of snapshots but one rule specifies a retention period of 90 days and another specifies a retention period of 180 days, the set of snapshots are retained for 180 days.

For backup, archive, replication, and CloudSpin; the extended retention schedule is as follows:

*   **Weekly**: The first successful run that started on a Sunday or later.
*   **Monthly**: The first successful run that started on the first day of the month or later.
*   **Yearly**: The first successful run that started on the first day of the year or later.

For backup schedule, {{site.data.keyword.baas_full_notm}} allows you to set the day for weekly backup and any day of the week for monthly backup.

If a backup fails, the extended retention is applied to the next scheduled snapshot taken by the {{site.data.keyword.baas_full_notm}}. However, if a manual backup was run in place of the failed backup after selecting "all objects" when selecting the VMs, extended retention is applied to the manual snapshot. For example, you've set the extended retention to be applied on the first snapshot on the first of every month. The scheduled backup on Feb 1st fails. After selecting all the objects (VMs), you run a manual backup successfully. The extended retention is applied to the snapshot of the manual backup.
