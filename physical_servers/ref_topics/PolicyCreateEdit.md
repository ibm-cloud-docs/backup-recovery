---

copyright:
  years: 2024
lastupdated: "2024-10-3"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Create or edit a standard policy
{: #create_or_edit_a_standard_policy}

17 September 2024

A protection policy is a reusable set of settings that define how and when objects are protected, replicated and archived. You can select a standard policy to use when configuring a Protection Group. A Protection Group uses the schedules and settings defined in the policy to determine when and how backups are captured, archived or replicated.

Use the default standard policies or create your own custom policies. Policies save time because you do not need to repetitively enter settings.

Policy creation and editing operations treat a month as having 31 days instead of 30 days, which was the definition in releases older than 6.8.1.

**To create or edit a standard policy:**

On the **Create Policy** page, select the target type and configure the advanced settings by clicking the respective icons from the floating menu when creating a policy.

On the **Create Policy** page you can change underlined numeric fields by over typing them or using the up and down arrow keys on your keyboard.

1. Navigate to **Data Protection > Policies**.
2. Click **Create Policy** located at the top right of the page..

    To edit an existing policy in the list, click the actions menu (![](../../Resources/Images/i/icn/more-h.svg)) and select **Edit** (![](../../Resources/Images/i/icn/edit-h.svg "Edit")).

    For {{site.data.keyword.baas_full_notm}} 6.6 and higher versions, the policy **Description** cannot be set or edited. For more details on how to edit the field, see the [Unable to update the Description field in a Protection Policy](/docs/backup-recovery?topic=COH-873338077){: external} Knowledge Base article.

3. **Policy Name:** Specify a protection policy name. The name can contain the alphanumerics, underscores, hyphens, periods, and spaces. This field is required and can be changed later.

4. Specify when backups are captured:

    *   **Backup every xx**: Specify the minutes, hours, weeks, months, or years to set the frequency for how often backups are captured by the Protection Group. This defines the protection schedule for capturing backups.

        The policy schedule can be configured to run on specific days. From the **Every** drop-down, select **Week** and in the **On** field, select the days of the week you want to run the backup. For example, you can schedule a backup to run on Monday, Wednesday, and Friday, and not on Tuesday and Thursday.

        The policy schedule can be configured to run on specific dates as well (calendar-based scheduling). From the **Every** drop-down, select **Month**, and in the **On** field, select the **Date**. You can then choose the date of the month you want to run the backup. For example, you can schedule a backup to run on the 5th of every month.

5. Under **Primary Copy**, specify the following:

    1. **Keep On**: Select **Local** from the drop-down to keep the primary copy data on the {{site.data.keyword.baas_full_notm}}.

        If you are selecting any external targets, then this policy can be only used for performing CloudArchive Direct wherein the primary copy of the data is stored on the external targets. For more information, see [CloudArchive Direct](../../Concepts/DirectArchival.htm).

    2. **Retain for xx days**: Specify how many days, weeks, months, or years backups are retained on the {{site.data.keyword.baas_full_notm}} before they are deleted.

        *   The **Retain** option can be set to a maximum value of 365000.
        *   Retentions at 7-day boundaries turn into weeks.


6. **Optional**: Enable **Data Movement** if required.

    **Data Movement**: This option applies only when you have selected AWS Standard Storage Tier as the external target to store the primary copy. With this option, you can move the data (down-tier) from any higher tier to any lower tier after a specified time period and downtiering can be done in multiple steps, e.g. Standard to S3-IA to S3-OneZone to Glacier. Data Movement helps in space reclamation on the external target. You can select the S3 archive target in the **Move Data** section and set a time interval to transfer the data from the S3 standard tier to the S3 archive bucket. This interval can be defined in days, weeks, months, or years.

    *   The data is archived to the standard (higher) tier in chunks and is referred to as cloud objects. The metadata and the cloud objects of the latest snapshot are always kept on the standard tier and will not be down-tiered to the archive (lower) tier. Only the cloud objects that are not referenced by any of the snapshots in the higher tiers are eligible for downtiering. As a result, some of cloud objects that were archived by the snapshots that are due for downtiering may still be retained in the higher tier. Therefore, the data movement may not necessarily happen at the same time interval you set.

    *   Data movement configuration must be inline with minimum retention requirements for the tier, e.g. moving data from S3-IA to S3-Glacier before 30 days can lead to early deletion charges.


7. Click **More Options** to configure the additional policy settings.

8. **DataLock:** Toggle on to lock the policy and any snapshots that belong to Protection Groups associated with the policy. DataLock is typically used for compliance and regulatory purposes. For more information, see [DataLock](datalock.htm).

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


10. **Continuous Data Protection**: (_Applies only for VMware_) To achieve near zero RPO, click the **Continuous Data Protection** option floating menu and specify the time unit (hours) to capture and retain logs on the {{site.data.keyword.baas_full_notm}}. This value defines the CDP schedule for capturing logs from the VMs.

    You can use this option to restore the VM applications to a point-in-time as opposed to periodic snapshots available with regular backups. For more information, see [Cohesity CDP for VMware](VMWare_CDPBackups.htm).

11. **Quiet Times:** Select this option from the **Backup Options** floating menu to add a quiet time. Quiet Times define time periods when new protection runs are not started. For example, you may want to configure hourly backups that run during weekdays but not on Saturday or Sunday. Quiet Times only prevent new protection runs from starting during the specified time period. Protection runs that started before the quiet time period are not affected and will continue to run. For example, if a protection run starts at 12:45 AM on Saturday morning and there is a weekend quiet time period that starts at 1 AM on Saturday morning, the protection run will continue to run past 1 AM. No new protection runs are started during the quiet times. Similarly, for jobs set on an interval basis, such as incremental jobs running every 6 hours, the job will start immediately after the Quiet Time ends. For example, if jobs are scheduled at 6 AM, 12 PM, 6 PM, and 12 AM, and a Quiet Time is set from 11:30 AM to 1:30 PM, the job originally planned for noon will instead commence at 1:31 PM. Once the run commences at 1:31 PM, the next run will take place at 7:30 PM and not 6 PM (6 hours difference).

    You can add multiple quiet times. For example, you could define a quiet time period for Saturday and Sunday and another quiet time period every morning from 9:00 AM to 11:00 AM.

    If you update the Quiet Time periods in a policy for a running Protection Group, only new protection runs using the policy are affected and any existing Protection runs that have already started will use the original Quiet Time periods.

12. **Retry Options:** Select **Customize Retries** from the **Backup Options** floating menu to customize the retry settings for capturing snapshots. By default, the {{site.data.keyword.baas_full_notm}} attempts to capture Snapshots three times before the protection run fails. The default time between retries is five minutes. You can optionally customize the number of retries and how long to wait between each attempt.

13. **BMR Backup (Physical Server)**: Select **BMR Backup** from the **Backup Options** floating menu to provide a backup schedule and retention period for Bare Machine Recovery (BMR) system data on a physical server.

    1. **Backup every xx**: Select the time unit for how often BMR backups are captured by the Protection Group.

    2. **Retain for xx days**: Specify how many days the BMR backups are retained on the {{site.data.keyword.baas_full_notm}} before they are deleted.

        The **Retain** option can be set to a maximum value of 365000.

    3. **Lock**: Specify how many days, weeks, months, or years the retained BMR backups should be locked. You cannot delete the snapshots during the specified period.


    This option is available only if you have enabled the **DataLock** option at the policy level.

14. **Log Backup (Databases):** Select **Log Backup** from the **Backup Options** floating menu to add a database logs schedule if you plan on restoring databases to a specific point in time between two full database server backups. If this policy has an archival schedule, this retention period also applies to the archived logs.

    For example, if you backup hourly at 5 minutes after the hour but you want the ability to restore a database to its state at 10:15 AM, specify a database logs schedule. The log backup saves all the transaction logs of all the databases so you can roll back to any point in time. Transaction logs are cumulative, so all the logs are backed up. When a database is added to a database server protected by {{site.data.keyword.baas_full_notm}}, its database logs are not backed up until a full database server backup has occurred.

    1. **Backup every xx:** Select the time unit for how often log backups are captured by the Protection Group.

    2. **Retain for xx days:** Specify how many days log backups are retained on the {{site.data.keyword.baas_full_notm}} before they are deleted. If this policy has a Replication schedule, this retention period also applies to the logs replicated to the target cluster.

        The **Retain** option can be set to a maximum value of 365000.

    3. **Lock**: Specify how many days, weeks, months, or years the retained log backups should be locked. You cannot delete the snapshots during the specified period.

        This option is available only if you have enabled the **DataLock** option at the policy level.


    For SQL Protection Groups, if the log backups are configured in the Protection Group, all protection runs are replicated ignoring the set policy. For more information, see the [Each SQL log backup run is replicated irrespective of policy](/docs/backup-recovery?topic=COH-898142405){: external} Knowledge Base article.

15. **Storage Array Snapshot**: Select **Storage Array Snapshot** from the **Backup Options** floating menu to provide a backup schedule and retention period for the Safeguarded snapshots for IBM Storage FlashSystem volume and volume groups. These are point-in-time cyber-resilient copies of IBM Storage FlashSystem volume groups.

    1. **Backup every xx**: Select the time unit for how often The Protection Group captures safeguarded snapshots.

    2. **Retain for xx days**: Specify how many days the Safeguarded snapshots are retained on the {{site.data.keyword.baas_full_notm}} before they are deleted.

16. **Extended Retention**: Click the **Primary Copy** tile and select this option from the **Backup Options** floating menu to retain a subset of snapshots (backups) for longer than defined by the protection schedule.

    1. Specify when to take the first snapshot and how long to retain it. For more information, see [Extended Retention](extended-retention.htm).

    2. **Lock**: Specify how many days, weeks, months, or years the retained backups should be locked. You cannot delete the snapshots during the specified period.

        This option is available only if you have enabled the **DataLock** option at the policy level.

    3. **Full Backup Only**: Toggle on to apply this setting for full backup. By default, the toggle is off and extended retention will not be used.

17. **Add Replication:** Select this option to replicate snapshots to another {{site.data.keyword.baas_full_notm}}. The copied snapshots are stored in the storage domain of the remote cluster. (The storage domain, however, is specified in the Protection Group.) The replication schedule for copying Snapshots cannot be more often than the protection schedule for capturing Snapshots. For example if Snapshots are captured hourly, the {{site.data.keyword.baas_full_notm}} cannot replicate them every 30 minutes but it can replicate them hourly or any value over one hour.

    For CloudArchive Direct, you should not add Replication.

    If database logs are backed up, they will also be replicated (there is not a separate replication schedule for logs). The retention period specified in the **Log Backup (Databases)** setting applies to both the source and target cluster.

    1. **Replicate to:** Select either a **Remote Cluster** or **AWS**.

        From the drop-down, select a cluster source to replicate snapshots. You can also select an AWS S3 storage source if you are backing up AWS VMs using the Cloud Snapshot Manager APIs of the cloud service.

        If you choose a remote cluster, use the **Replication Target** to specify the target cluster to copy the snapshots.

        You can also register a new cluster to replicate to by selecting **Register Remote Cluster**. For detailed instructions, see [Create a Connection to the Remote {{site.data.keyword.baas_full_notm}}](../Platform/ReplicationRegisterRemoteCluster.htm).

    2. **After every run:** Specify how often snapshots are replicated.

    3. **Retain for xx days:**

        Full/Incremental Backups

        Specify how long the snapshots are retained on the remote cluster.

        The Retain option can be set to a maximum value of 365000.

        Log Backups

        Specify how long the log snapshots are retained on the external target.

        The Retain option can be set to a maximum value of 365000.

        All runs are replicated continuously as log backups require a continuous chain of backups.

    4. **Lock**: Specify how many days, weeks, months, or years the retained snapshots should be locked. You cannot delete the snapshots during the specified period.

        This option is available only if you have enabled the **DataLock** option at the policy level.

    5. To add an additional replication schedule, repeat these steps. To replicate to multiple {{site.data.keyword.baas_full_notm}}s, create multiple replication schedules with different replication targets.

        If you specify multiple replication schedules with the same replication target that replicate the same set of snapshots but with different retention periods, the snapshots are retained for the longest time period. For example if you define two replication schedules with the same replication target that retain the same set of snapshots but one schedule specifies a 90 day retention period and other schedule specifies a 180 day retention period, the set of snapshots are retained for 180 days.

18. **Add Archival:** Select this option to add an archive schedule.

    1. **Archive to:** Select an existing external target. You can also register a new external target. For detailed instructions, see [Register an External Target](../Platform/ExternalTargetsRegister.htm).

    2. **Archive only fully successful Runs:** Check this box if you want archive to occur only if the snapshots of all the objects protected by the Protection Group were created. If the creation of any snapshots fails, no snapshots are archived.

    3. **Full Backup Only**: Toggle on to archive only the full backups. By default, the toggle is off.

    4. **Every #:** Specify the archive schedule for making copies of snapshots created by this Protection Group and storing them on a registered external target. (The storage domain, however, is specified in the Protection Group.) The archive schedule for copying snapshots cannot be more often than the protection schedule for capturing snapshots. For example if snapshots are captured daily, the {{site.data.keyword.baas_full_notm}} cannot archive them hourly but it can archive them daily or any value less often than daily.

    5. **Retain for xx days:**

        Full/Incremental Backups

        Specify how long the snapshots are retained on the external target.

        *   Cloud archive snapshots are expired per the retention policy. However, the garbage collection of the expired cloud archive snapshots happens in the background. The expired cloud archive snapshots will be garbage collected from the External Target once there are no active Cloud archive snapshots sharing data with them.
        *   You cannot delete the latest archive snapshot as it might be needed in future. However, if you still want to delete the archive snapshot, you must contact [IBM Cloud Support](../../Support/ContactSupport.htm).

        *   The retention period is applicable only to the database backup and not the log backup.

        *   The **Retain** option can be set to a maximum value of 365000.


        Log Backups

        Specify how long the log backup snapshots are retained on the external target.

        The Retain option can be set to a maximum value of 365000.

        You can add additional archive schedules.

        If you specify multiple archive schedules with the same external target that archive the same set of snapshots with different retention periods, the snapshots are retained for the longest specified time period.

    6. **Lock**: Specify how many days, weeks, months, or years the retained snapshots should be locked. You cannot delete the snapshots during the specified period.

        *   This option is available only if you have enabled the **DataLock** option at the policy level.
        *   The DataLock duration must be lesser than or equal to the retention period.


19. **WORM on External Target**: Enable this option to lock the archives in the targets to prevent modification on the external target. The archives to this target will be WORM compliant. For more information, see [Prerequisites and Considerations for WORM Compliance](#Prerequi).

20. **Add CloudSpin:** Select this option to add a CloudSpin schedule. For more information, see [Clone VMs to AWS Cloud Using CloudSpin](../TestDev/CloneVMsCloudSpinAWS.htm).

    1. **CloudSpin to:** Select an existing cloud source or register a new cloud source. A cloud source is a cloud service instance where the snapshots are converted and copied **to**. For details about the different fields, see the appropriate source topic:
        *   [Register or Edit an Azure Cloud Source](SourceAzureCloud.htm)
        *   [Register an AWS Cloud Source](SourceAWS.htm)
    2. **After every run:** Specify how often snapshots are converted and copied to the cloud source.
    3. **Retain for xx days:** Specify how long the snapshots are retained on the cloud source.

        The **Retain** option can be set to a maximum value of 365000.

    4. **Lock**: Specify how many days, weeks, months, or years the retained snapshots should be locked. You cannot delete the snapshots during the specified period.

        This option is available only if you have enabled the **DataLock** option at the policy level.

    5. **CloudSpin only fully successful Runs:** Check this box displays if you want converting and copying to occur only if the protection run successfully backs up all the protected objects, such as 10 out of 10 VMs. For example, if a protection run backs up only 7 out of 10 VMs, and this check box is selected, the protection run will not be converted and copied. If a protection run backs up only 7 out of 10 VMs, and this check box _is not_ selected, the protection run can be converted and copied to a cloud source.

    6. Specify the appropriate settings for the cloud service:

        [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)AWS](#)

        Select the Cloud Region in AWS where the VM should be copied to. For more information, see [AWS Regions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions){: external}.

        You can optionally add **Tags** to the resources by clicking **Add Tag**. These tags are key-value pairs that will be assigned to all the temporary and permanent resources created during Cloudspin. The maximum number of tags allowed is 40.

        [![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)Azure](#)

        **Resource Group**—Select the Azure Resource Group where the converted VM image should be copied to.

        **Storage Account**—Select an Azure Storage Account to use when copying the converted VM image.

        **Storage Container**—Select an Azure Storage Container in the Storage Account to hold the converted VM image.

21. Click **Create**.

When editing an existing policy that is configured to capture backup snapshots on a minutes or hourly time schedule and you change the schedule to occur daily, weekly or monthly, you are prompted to specify a start time for each of the Protection Groups that use this policy.

The policy is immediately available to use in Protection Groups.

*   For {{site.data.keyword.baas_full_notm}} 6.6 and higher versions, on NoSQL platforms, when you pause the backup job for a longer time than the retention period, the last successful snapshot will not be deleted even after the retention period expired on that snapshot.

*   For {{site.data.keyword.baas_full_notm}} 6.6 and higher versions, the last snapshot is always retained on HDFS/HBase/Hive, MongoDB, Cassandra, and CouchBase platforms.


## Prerequisites and considerations for worm compliance
{: #prerequisites_and_considerations_for_worm_compliance}

### Prerequisites
{: #prerequisites}

During the external target registration, {{site.data.keyword.baas_full_notm}} checks if the external target is WORM-capable by querying the external target properties.

The following are the prerequisites to be set in:

*   Confirm whether the target you select supports WORM. For the list of targets that support WORM, see [Supported Workflows and External Targets](../../ReleaseNotes/SupportedWorkflows.htm).


*   The WORM-supported AWS external target must have versioning and object lock enabled. For more information, see [Using S3 Object Lock](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html){: external}.

*   For the Azure targets, the container must have a version-level immutability set, which means the storage account must also have version-level immutability. For more information, see [Enable version-level immutability support on a container](https://docs.microsoft.com/en-us/azure/storage/blobs/immutable-policy-configure-version-scope?tabs=azure-portal#enable-version-level-immutability-for-a-new-container){: external}.


*   The above properties must be set at the time of bucket creation and cannot be enabled after creation.
*   Avoid setting default retention on S3 buckets and Azure - this is not required as {{site.data.keyword.baas_full_notm}} locks the objects for the duration required. Large default retentions affect garbage collection and increase costs.

*   By default, the objects will be locked in the [Governance retention mode](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-overview.html){: external}. To use the **Compliance** mode, contact [IBM Cloud Support](../../Support/ContactSupport.htm).


### Considerations
{: #considerations}

*   This field is available only if the **DataLock** toggle is enabled.

*   This field is currently available only for AWS and Azure WORM-capable targets.

*   WORM is not supported on external targets already registered on {{site.data.keyword.baas_full_notm}} before 6.8 version.

*   When archiving data to versioned buckets, ensure that you do not edit the storage class of objects directly. Editing the storage class of objects directly will create a new version of the object while the original version remains in the previous storage tier. Object-level locks will not be carried over to the new version.

*   If the **WORM on External Target** option is enabled in the policy after performing the archive, the next archive will be WORM compliant.

*   The {{site.data.keyword.baas_full_notm}} Data Movement feature cannot coexist with WORM. An archival target in a Protection Policy can only have one of the settings enabled.

*   For archival with period full format:

    *   Supports both AWS and Azure WORM-capable targets. For the list of targets that support WORM for archival with periodic full, see [Supported Workflows and External Targets](../../ReleaseNotes/SupportedWorkflows.htm).

    *   If WORM compliance is not met for an archive, the archive is still considered successful and can be recovered.

    *   After archival, to view the WORM Compliance status, perform the following:

        1. Click the **Protection** menu.

        2. Select the required Protection Group.

        3. Select the required run.

        4. Click the **Cloud Archive** tab.

        5. The **WORM Compliance** status and **WORM Expiry Time** will be displayed.

*   **For archival with incremental forever format**:

    *   Supports AWS WORM-capable targets. For the list of targets that support WORM for archival with incremental full, see [Supported Workflows and External Targets](../../ReleaseNotes/SupportedWorkflows.htm).

    *   If WORM compliance is not met for an archive, then the archival will fail.

    *   {{site.data.keyword.baas_full_notm}} does not support WORM for CloudArchive Direct.

    *   With WORM enabled, the storage utilization of external targets may be higher.


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
