---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Manage protection groups
{: #manage_protection_groups}


From the Protection page, you can start the backup process by clicking **Protect** in the upper right corner. You can also view protection group settings, run a protection group, edit protection group settings, delete a protection group, deactivate a protection group or failover a protection group using the icons listed in the actions column of the table. (See below for details.) 

This page also provides the following information:

*   Overview of the protection groups that are already created. You can filter these protection groups based on:
    *   Groups
    *   Group types
    *   Policies
    *   SLA
    *   Status
    *   Organizations
*   The run status for all the protection groups in the {{site.data.keyword.baas_full_notm}} that meet the specified filter criteria.

The columns for the table at the bottom of this page are described below.


| Column Name | Description / Action |
| --- | --- |
| **Group Name** | Drill-down on this link to get statistics about this Protection Group such as throughput, megabytes read, a list of jobs runs, job details and a job audit trail.<br><br>You can view these details either in the details view or in calendar mode.<br><br>*   To view the details in the details view mode, click the ![](../../Resources/Images/Details_View_20x20.png) icon on the right.<br>*   To view the details in calendar mode, click the ![](../../Resources/Images/calendar_mode_28x28.png) icon displayed on the right corner of the page.<br>    *   You can view the status of the protection group based on the date you select from the calendar.<br>    *   If there is no protection group run scheduled on a day, then that day will be greyed out in the calendar.<br>    *   If there is a protection group run scheduled on a day, then that day will be highlighted in the calendar.<br>    *   If the scheduled protection group run fails, then the ![](../../Resources/Images/failure_20x20.png) icon will be displayed against the scheduled day. |
| **Organizations** | Drill-down on this link to get details of the organization that is associated with the protection group. |
| **Start time** | The start time and date of the last run of this protection group. |
| **Duration** | The time duration taken to complete the last run of this protection group. |
| **SLA** | Reports the SLA status.<br><br>—The last run of this Protection Group completed within the SLA time period specified for this protection group.<br><br>—The last run of this Protection Group did not complete within the SLA time period specified for this protection group . |
| **Status** | For the last run of this protection group, this column reports the current status:<br><br>*   **Succeeded**—The protection group run completed without error.<br>*   **Canceled**—A protection group run started and then was canceled in the {{site.data.keyword.baas_full_notm}} Console The protection group run did not complete.<br>*   **Canceling**—In the {{site.data.keyword.baas_full_notm}} Console, the cancel option was selected for the protection group run and the cluster is in the process of canceling the run but the run is not yet canceled.<br>*   **Failed**—The protection group run did not complete due to an error.<br>*   ![](../../Resources/Images/ProgressBar_119x29.png)—The protection group run is currently running but is not completed yet.<br><br>Additional information about some protection run issues may be available [here](JobRunIssues.htm). |
{: caption="" caption-side="bottom"}
