---

copyright:
  years: 2025
lastupdated: "2025-09-05"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protect a pure volume or a protection group
{: #protect_a_pure_volume_or_a_protection_group}


A Protection Group that protects Pure Storage volumes captures Snapshots and these Snapshots are required for the following tasks:

*   Recover volumes
*   Replicate Snapshots
*   Archive Snapshots

### Before you begin
{: #before_you_begin}

When you create a Protection Group, you can select an existing source, policy, or storage domain. You can also create them while creating the Protection Group. However, you might find it easier to create them prior to creating the Protection Group, as described in the following topics.

*   [Register or Edit a Pure FlashArray](register-pure.htm)
*   [Create or Edit a Standard Policy](PolicyCreateEdit.htm)
*   [Create or Edit Storage Domains](../Platform/ViewBoxesCreateEdit.htm)

For prerequisites and considerations, see [Plan and Prepare for Pure FlashArray](plan-prepare-pure-storage-array.htm).

**To Protect a Pure Volume or a Protection Group :**

You can now protect Pure volumes or multiple volumes using a Pure Protection Group on the {{site.data.keyword.baas_full_notm}}.

1. Navigate to **Data Protection > Protection**.

2. Click **Protect** on the top-right of the page and then select **SAN**.

3. In the **New Protection** pop-up, you can either select a Volume or a Pure Protection Group:

    **To Protect a Volume**:

    Select the **Volume List** View (![](../../Resources/Images/PPG_VolumeList.png)) on the top-right to list the volumes. From the **Volume** list, select the volumes you want to back up.

    **To Protect a Pure Protection Group**:

    The Pure Protection Group is an Early Access feature. Contact your {{site.data.keyword.baas_full_notm}} account team to enable the feature.

    Select the **Pure Protection Group** View (![](../../Resources/Images/PPG_PPGList.png)) on the top-right to list the **Pure Protection Groups**. From the Pure Protection Group list, select the Pure Protection Group you want to back up.

    After selecting one of the above options, click the edit icon and provide the following information:

    1. **Add Objects**: From the **Objects** list, select the **Pure Protection Groups** you want to backup or click **Register Source** to register a new Pure Protection Group.

    2. **Protection Group**: Specify a name for the Protection Group. The name can contain alphanumeric characters, underscores, dashes, and periods.

    3. **Policy**: Select an existing policy or click **Create Policy** to create a new protection policy. For more information, see [Create or Edit a Standard Policy](PolicyCreateEdit.htm).

    4. **Storage Domain**: Select the appropriate storage domain. The default time zone in the Start Time field is the browser's time zone. You can change the time zone of the job by selecting a different time zone. For more information, see [Manage Storage Domains](../Platform/ViewBoxesManage.htm).

    5. To protect Pure Protection Group by specifying all the options, click **More Options**.


        | Advance Settings | Description |
        | --- | --- |
        | **End Time** | Click and select the date when the Protection Group stops capturing snapshots. A Protection Group run that starts prior to this date runs until completion even if it completes after this date. |
        | **Priority** | Select a priority for the Protection Group execution. IBM Cloud Supports concurrent backups. However, if the number of jobs exceeds the ability to process jobs, this will be the priority of implementation of the jobs:<br><br>1. High<br>2. Medium<br>3. Low |
        | **QoS Policy** | Select an appropriate quality of service (QoS) policy.<br><br>{{site.data.keyword.baas_full_notm}} recommends specifying **Backup HDD**, which is the default.<br><br>*   **Backup HDD**: The {{site.data.keyword.baas_full_notm}} writes the data directly to an HDD drive for this Protection Group.<br>*   **Backup SSD**: The {{site.data.keyword.baas_full_notm}} writes the data directly to an SSD drive for this Protection Group. Only specify this policy if you need fast ingest speed for a small number of Protection Groups.<br>*   **Backup Auto**: (Applicable only for Cohesity C6000 series) The {{site.data.keyword.baas_full_notm}} writes the data to both SSDs and HDDs. The distribution of data will be based on the current usage of SSD and HDD. This policy tries to achieve similar backup performance as the **Backup SSD** policy and reduces the SSD wear-out compared to the Backup SSD policy. |
        | **Retain on Pure Storage Array** | Enter the number of the most recent snapshots to retain on the Pure FlashArray. Retaining the last 5 snapshots is the default. You can change this number or choose to retain All Snapshots. To take incremental snapshots, this value must be at least 1. |
        | **Pre & Post Scripts** | Toggle Enable to run scripts on the Unix server. When creating a Protection Group, you can configure:<br><br>*   **Pre Scripts**: Scripts that will be executed before the protection run.<br>    <br><br>*   **Post Backup Scripts**: Scripts that will be executed after the protection run.<br>    <br>*   **Post Snapshot Scripts**: Scripts that will be executed after the snapshot has been created on the source.<br>    <br><br>If enabled, the scripts are executed during the protection run as configured.<br><br>You can distinguish between the scripts that are run on the different objects (entities) using the COHESITY\_BACKUP\_ENTITY environment variable. The {{site.data.keyword.baas_full_notm}} populates this environment variable with the name of the backed object (or entity). For more information, see Configure Pre & Post Scripts. For more information, see [Configure Pre & Post Scripts](PrePostScripts.htm). |
        | **SLA** | The service-level agreement (SLA) defines how long the administrator expects a Protection Group run to take.<br><br>*   **Full**: Enter the number of minutes you expect a full backup Protection Group run takes to complete. A full backup captures the entire object (all blocks).<br>*   **Incremental**: Enter the number of minutes you expect an incremental backup Protection Group run takes to complete. An incremental backup captures only the differences (changed blocks) since the last run.<br><br>Users with similar SLAs can be grouped together and a custom policy and Protection Group can be created to achieve different SLAs. |
        | **Quiet Times** | Available only if the selected policy has at least one quiet time period. Toggle it on to specify that all currently executing protection runs should abort if a quiet time period specified for the Protection Group starts. If the option is toggled off, then after a protection run starts, it continues to execute even when a quiet time period specified for this Protection Group starts. However, a new protection run will not start during a quiet time period. |
        | **Alerts** | Select one or more of the following **Alerts On** settings if you want alerts to be created for the following triggers:<br><br>*   SLA Violation: Create a warning alert if the Protection Group takes longer than the time period specified in the SLA field. Emails are sent when warning alerts are created. For more information, see [Alerts](../Monitoring/Alerts.htm).<br>*   Failure: Create a critical alert if the Protection Group fails to complete. Emails are sent when critical alerts are created.<br>*   Success: Create an informational alert when the Protection Group completes successfully. Emails are not sent when informational alerts are created.<br><br>To send emails when alerts are triggered for the Protection Group, in the **Email Recipients** field, add the email addresses of the recipients. |
        | **Pause Future Runs** | Toggle on this option to stop protection runs of the Protection Group from executing. Once you enable this option, no protection runs will be scheduled for this Protection Group. |
        | **Description** | Enter a brief description about the Protection Group. |

    6. Click **Protect**.
