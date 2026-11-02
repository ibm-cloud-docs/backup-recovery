---
copyright:
  years: 2026
lastupdated: "2026-11-02"

keywords: ibm db2, database protection, backup, protection group, protection policy

subcollection: backup-recovery

content-type: tutorial
services: backup-recovery
account-plan: lite
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Protect IBM DB2 deployments
{: #protect-ibm-db2-deployments}
{: toc-content-type="tutorial"}
{: toc-services="data-protection"}
{: toc-completion-time="30m"}

In {{site.data.keyword.baas_full_notm}}, IBM DB2 deployments are protected using a Protection Group and a Protection Policy. A Protection Group enables you to specify the objects such as databases that you want to backup.
{: shortdesc}

The Protection Group uses the schedules and settings defined in the Protection Policy to determine when and how backups are captured.

A Protection Policy is a group of settings that define the type of backup, backup schedule, and backup snapshot retention. You can select which standard or custom policy to use when creating a Protection Group. For more information, see [About Policies and Protection Groups](/docs/backup-recovery?group=policies-and-protection-groups).

## Before you begin
{: #protect-ibm-db2-prereqs}

* Ensure you have registered your IBM DB2 source. For more information, see [Register IBM DB2 as a Source](register-ibm-db2-source.md).
* Verify you have the necessary permissions to create Protection Groups.
* Determine which databases you want to protect.

## Create a Protection Group
{: #create-protection-group}
{: step}

1. In the {{site.data.keyword.baas_full_notm}}Dashboard, do one of the following:
   * Select **Data Protection > Protection**. Click **Protect** in the upper right of the page and then select **Universal Data Adapter**.
   * Select **Data Protection > Sources**. Click the actions menu (![More icon](../Resources/Images/i/icn/more-h.svg)) next to the IBM DB2 source and then select **Protect**.

2. From the **Source** list, select the registered IBM DB2 source or click **Register Source** to register an IBM DB2 source. Click **+** to add multiple objects. For more information, see [Register IBM DB2 as a Source](register-ibm-db2-source.md).

## Select objects to protect
{: #select-objects}
{: step}

In the **Objects** section, add the object that you plan to protect. The object should be a database name. To back up a database, enter the name of the database. For example, `DEMODB`. Then, click **Save Selection**.

## Configure Protection Group settings
{: #configure-protection-group}
{: step}

In the **Protection Group** section, do one of the following:

* Select **New Group** to create a new Protection Group. In the **Protection Name** field, enter a name for the Protection Group.
* Select **Existing Group** to use the settings of an existing Protection Group. From the **Group** list, select an existing Protection Group and click **Protect**.

## Select a Protection Policy
{: #select-policy}
{: step}

From the **Policy** list, select an existing Protection Policy or create a new Protection Policy.

## Configure backup settings
{: #configure-backup-settings}
{: step}

In the **Backup Settings** section, select **Online Backup** or **Offline Backup** as per your requirements.

### Online backup settings
{: #online-backup-settings}

If you have selected **Online Backup**, configure the following:

| Field | Mandatory? | Default | Description |
| ----- | ---------- | ------- | ----------- |
| Convert Incremental Backup To Full Backup On failure | No | Disabled | Enable if you want to convert the incremental backup to full backup in case of an error. Any failure of an incremental backup will trigger a full backup, increasing the total backup time. |
| Prune Logs | No | Disabled | Prune the backup history up to the last successful full backup to delete unnecessary entries from the history table. Applicable to both log backup methods. |
| Prune Logs hour | No | N/A | Enter the number of hours. DB2 log files older than the provided hour will be deleted or pruned from the local archive location on DB2 nodes as a part of the scheduled log backup. Applicable only to dump and sweep backup method. |
{: caption="Online backup settings" caption-side="bottom"}

### Offline backup settings
{: #offline-backup-settings}

If you have selected **Offline Backup**, configure the following:

| Field | Mandatory? | Default | Description |
| ----- | ---------- | ------- | ----------- |
| Convert Incremental Backup To Full Backup On failure | No | Disabled | Enable if you want to convert the incremental backup to full backup in case of an error. |
| Deactivate database | No | Disabled | Enable to deactivate the database before performing the offline backup. |
| Activate database | No | Disabled | Enable to activate the database after the backup is completed. |
{: caption="Offline backup settings" caption-side="bottom"}

## Configure advanced options
{: #configure-advanced-options}
{: step}

Optional. Enable **Advanced Options** to configure advanced settings:

| Field | Mandatory? | Default | Description |
| ----- | ---------- | ------- | ----------- |
| Number of objects to backup in parallel | No | 6 | The number of objects you want to backup simultaneously. |
| RPC Timeout In Seconds | No | 120 seconds | The request timeout between the IBM DB2 source and the {{site.data.keyword.baas_full_notm}}cluster. |
| Max IO Write Size To {{site.data.keyword.baas_full_notm}}| No | 4194304 bytes | The maximum size of the data packet that can be sent to the {{site.data.keyword.baas_full_notm}}cluster. |
| Environment Variables | No | N/A | Provide the environment variables if you want to override the environment's default value. For example, `PATH=/sbin:/bin:/usr/bin` |
{: caption="Advanced options" caption-side="bottom"}

## Configure general settings
{: #configure-general-settings}
{: step}

In the **Settings** section, do the following:

1. Select the appropriate **Storage Domain**.

2. In the **Start Time** field, define when the protection run should start. This option is only available if the selected policy is set to Backup Daily. Enter the hour and minutes, or use the up and down arrows on your keyboard.

   Verify the AM or PM setting. The default time zone is the browser's time zone. You can change the time zone of the protection run by selecting a different time zone.

3. In the **Mounts** field, enter the number of VIPs you want to use while writing data to the {{site.data.keyword.baas_full_notm}}storage.

## Review and create
{: #review-create}
{: step}

1. After you have completed the **Settings**, if you need to change any additional settings on the **New Universal Data Adapter Protection Group** page, scroll down and click **Edit** on the right. For more information, see [Additional Settings](#additional-settings).

2. Click **Protect**.

## Additional Settings
{: #additional-settings}

The following table describes additional settings that you can configure for your Protection Group:

| Additional Settings | Description |
| ------------------- | ----------- |
| Pause Future Run | Enable **Pause Future Run** only if you do not want to schedule future runs of the Protection Group. |
| End Time | Optional. Click and select the date when the Protection Group stops capturing snapshots. A protection run that starts prior to this date runs until completion even if it completes after this date. |
| QoS Policy | Select an appropriate quality of service (QoS) policy. QoS policies are designed for workloads such as backups, which keep a lot of IOs outstanding. We recommend one of the following: \n * **Backup Target HDD (Default)**: This policy assumes higher latency is OK and the workload will get high throughput if it issues a lot of writes in parallel. The data lands on hard disks if it is sequential and on SSDs if it is random writes. \n * **Backup Target SSD**: With this policy, both sequential and random IOs land on SSDs, but the IOs are not treated as high-priority IOs and we assume the workload keeps lots of IOs outstanding. \n For best performance, {{site.data.keyword.baas_full_notm}}recommends the Backup Target SSD policy. If necessary, you can change the policy at any time later. But it doesn't change or take effect on the currently running task. |
| Concurrency | Maximum number of backup streams that will be created to exchange data with the cluster. By default, maximum scaling is capped at 16. |
| Alerts | Optional. Select one or more of the following settings if you want alerts to be created for the following triggers: \n * **Success**: Creates an informational alert when a Protection Group completes successfully. Emails are not sent when informational alerts are created. \n * **Failure**: Creates a critical alert if the Protection Group fails to complete. Emails are sent when critical alerts are created. \n * **SLA Violation**: Creates a warning alert if the Protection Group takes longer than the time period specified in the SLA field. Emails are sent when warning alerts are created. \n You can add email addresses to a Protection Group to notify the email recipients when alerts are triggered for the protection group. |
| Priority | Select a priority for the Protection Group execution. {{site.data.keyword.baas_full_notm}} supports concurrent backups. However, if the number of protection runs exceeds the ability to process runs, this will be the priority of implementation of the protection runs: \n 1. High-priority protection runs \n 2. Medium-priority protection runs \n 3. Low-priority protection runs |
| SLA | The service-level agreement (SLA) defines how long the administrator expects a Protection Group run to take. \n * **Incremental**: Enter the number of minutes you expect an incremental backup Protection Group run takes to complete. An incremental backup captures only the differences (changed blocks) since the last run. \n * **Full**: Enter the number of minutes you expect a full backup Protection Group run takes to complete. A full backup captures the entire object (all blocks). |
| Description | Optional. Enter a brief description about the Protection Group. |
{: caption="Additional settings" caption-side="bottom"}

## Next steps
{: #protect-ibm-db2-next-steps}

After creating your Protection Group, you can:

* Monitor the protection runs in the {{site.data.keyword.baas_full_notm}}Dashboard
* Restore databases from backups when needed
* Modify Protection Group settings as requirements change
* View backup history and compliance reports
