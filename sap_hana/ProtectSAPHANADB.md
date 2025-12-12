---

copyright:
  years: 2025
lastupdated: "2025-12-12"

keywords: sap hana, protect, deployments, settings

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protect SAP HANA deployments
{: #protect_sap_hana_deployments}

SAP HANA deployments are protected using a Protection Group and a Protection Policy. A Protection Group enables you to:

- Specify the objects such as databases and tables that you want to backup.
- Based on your requirement, define the script arguments for the [backup](https://help.sap.com/docs/PRODUCT_ID/6b94445c94ae495c83a19646e7c3fd56/c3a57273bb571014a747a289a3198e15.html?state=PRODUCTION&version=2.0.04&locale=en-US){: external} statements provided by SAP HANA. For more information, see [Backup Arguments](/docs/backup-recovery?topic=backup-recovery-backup_sap_hana_database#backup_process).

The Protection Group uses the schedules and settings defined in the Protection Policy to determine when and how backups are captured.

A Protection Policy is a group of settings that define the type of backup, backup schedule, and backup snapshot retention. You can select which standard or custom policy to use when creating a Protection Group. For more information, see [About Policies and Protection Groups](/docs/backup-recovery?group=policies-and-protection-groups).

To protect SAP HANA deployments:

1. Do one of the following:

   - Select **Data Protection > Protection**. Click **Protect** on the top-right of the page and then select **Universal Data Adapter**
   - Select **Data Protection > Sources**. Click the actions menu next to the SAP HANA source and then select **Protect**.

1. From the **Source** drop-down list, select the registered SAP HANA source or click **Register Source** to register a SAP HANA source. For more information, see [Register SAP HANA as a Source](/docs/backup-recovery?topic=backup-recovery-register_and_manage_the_sap_hana_source#register_sap_hana_as_a_source).
2. In the **Objects** section, add the object that you plan to protect. The object should be a database name. To back up a database, enter the name of the database. For example, `H00`. Click the plus icon (+) to enter multiple objects that you plan to backup. Then, click **Save Selection**.

3. In the **Protection Group** section, do one of the following:

   - Select **New Group** to create a new Protection Group. In the **Protection Name** field, enter a name from the Protection Group.
   - Select **Existing Group** to use the settings of an existing Protection Group. From the **Group** list, select an existing Protection Group and click **Protect**.

1. From the **Policy** drop-down list, select an existing Protection Policy or create a new Protection Policy.

    SAP HANA automatically schedules log backups, and you can view these log backups on the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster. Ensure that you add a log backup job in the Protection Policy. The Protection Policy with log backup job retains the SAP HANA log backups and is not used for scheduling SAP HANA log backups.

2. In the Settings section, do the following:

    2. In the **Start Time** field, define when the protection run should start. This option is only available if the selected policy is set to Backup Daily. Enter the hour and minutes, or use the up and down arrows on your keyboard.

        Verify the AM or PM setting. The default time zone is the browser's time zone. You can change the time zone of the protection run by selecting a different time zone.

    3. In the **Custom Options** section, perform any one of the following:

       - Do not enter any backup script arguments in the **Full Backup** and **Incremental Backup** fields if you plan to perform full backup or incremental backup respectively.
       - Enter the [Backup Arguments](/docs/backup-recovery?topic=backup-recovery-backup_sap_hana_database#backup_arguments) in the **Full Backup** and **Incremental Backup** fields based on your requirements.

            For example:

            | Use Cases | Sample Arguments |
            | --- | --- |
            | Create a full backup of the SAP HANA cluster by overriding the user store key.<br><br>In the case of SAP HANA cluster backup, it takes the user store key as `H00_KEY` which is overridden with the backup script argument. | In the **Full Backup** field, specify the following argument:<br><br>`--user-store-key=H00_KEY` |
            | Create an incremental backup of the SAP HANA cluster by overriding the user store key.<br><br>In the case of SAP HANA cluster backup, it takes the user store key as `H00_KEY` which is overridden with the backup script argument. | In the **Incremental Backup** field, specify the following argument:<br><br>`--user-store-key=H00_KEY` |
            | Create a differential backup of the SAP HANA cluster.<br><br>In the case of SAP HANA cluster backup, it takes the backup delta as DIFFERENTIAL which is overridden with the backup script argument. | In the **Incremental Backup** field, specify the following argument:<br><br>`--backup-delta=DIFFERENTIAL` |
            {: caption="Use Cases and Sample Arguments" caption-side="bottom"}

        - In the **Mounts** field, enter the number of VIPs you want to use while writing data to the {{site.data.keyword.cloud_notm}} Backup and Recovery storage.

            {{site.data.keyword.cloud_notm}} Backup and Recovery recommends that the number of mounts is equal to the number of nodes or Hybrid Extenders.

1. After you have completed the **Settings**, if you need to change any additional settings on the **New Universal Data Adapter Protection Group** page, scroll down and click Edit on the right. For more information, see [Additional Settings](#additional_settings).
2. Click **Protect**.


## Additional settings
{: #additional_settings}


| Additional Settings | Description |
| --- | --- |
| **Pause Future Run** | Enable **Pause Future Run** only if you do not want to schedule future runs of the Protection Group. |
| **End Time** | **Optional**.<br><br>Click and select the date when the Protection Group stops capturing snapshots. A protection run that starts prior to this date runs until completion even if it completes after this date. |
| **QoS Policy** | Select an appropriate quality of service (QoS) policy. QoS policies are designed for workloads such as backups, which keep a lot of IOs outstanding. We recommend one of the following:<br><br>*   **Backup Target HDD (Default)**: This policy assumes higher latency is OK and the workload will get high throughput if it issues a lot of writes in parallel. The data lands on hard disks if it is sequential and on SSDs if it is random writes.<br>*   **Backup Target SSD**: With this policy, both sequential and random IOs land on SSDs, but the IOs are not treated as high-priority IOs and we assume the workload keeps lots of IOs outstanding.<br><br>For best performance, {{site.data.keyword.cloud_notm}} Backup and Recovery recommends the Backup Target SSD policy. If necessary, you can change the policy at any time later. But it doesn’t change or take effect on the currently running task. |
| **Concurrency** | The maximum number of restore streams that is created to exchange data with the cluster. By default, maximum scaling is set to empty.<br><br>The same value is set to the `parallel_data_backup_backint_channels` parameter in the **global.ini** file for the protected database. Also, the same value is set for the `max_restore_streams` parameter in the SAP HANA parameter file of the database selected for protection. |
| **Alerts** | **Optional**.<br><br>Select one or more of the following settings if you want alerts to be created for the following triggers:<br><br>*   **Success**: Creates an informational alert when a Protection Group completes successfully. Emails are not sent when informational alerts are created.<br>*   **Failure**: Creates a critical alert if the Protection Group fails to complete. Emails are sent when critical alerts are created.<br>*   **SLA Violation**: Creates a warning alert if the Protection Group takes longer than the time period specified in the SLA field. Emails are sent when warning alerts are created. For more information, see [Alerts](https://docs.{{site.data.keyword.cloud_notm}} Backup and Recovery.com/6_6/Web/UserGuide/Content/Dashboard/Monitoring/Alerts.htm?tocpath=Monitoring%7CHealth%7CAlerts%7C_____0){: external}.<br><br>You can add email addresses to a Protection Group to notify the email recipients when alerts are triggered for the protection group. |
| **Priority** | Select a priority for the Protection Group execution. {{site.data.keyword.cloud_notm}} Backup and Recovery supports concurrent backups.<br><br>However, if the number of protection runs exceeds the ability to process runs, this will be the priority of implementation of the protection runs:<br><br>1. High-priority protection runs<br>2. Medium-priority protection runs<br>3. Low-priority protection runs |
| **SLA** | The service-level agreement (SLA) defines how long the administrator expects a Protection Group run to take.<br><br>*   **Incremental**: Enter the number of minutes you expect an incremental backup Protection Group run takes to complete. An incremental backup captures only the differences (changed blocks) since the last run.<br>*   **Full**: Enter the number of minutes you expect a full backup Protection Group run takes to complete. A full backup captures the entire object (all blocks). |
| **Description** | **Optional**.<br><br>Enter a brief description about the Protection Group. |
{: caption="Additional Settings" caption-side="bottom"}

