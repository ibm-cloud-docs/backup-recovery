---

copyright:
  years: 2025
lastupdated: "2025-09-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protect Oracle Database

When you create a protection group, you can select an existing source, policy and storage domain or you can also register a new oracle source, or create a new policy or create a new storage domain while creating a new oracle protection group. However, you might find it easier to create them before creating the protection group, as described in the following topics.

*   [Register Oracle Server](/docs/backup-recovery?topic=backup-recovery-register_oracle_sources)
*   [Create or Edit a Standard Policy](/docs/backup-recovery?topic=backup-recovery-create_or_edit_a_standard_policy)
*   [Create or Edit Storage Domains](/docs/backup-recovery?topic=backup-recovery-create_or_edit_storage_domains)

You can now protect Oracle databases through an Oracle adapter and SBT plugin on Linux Oracle servers running in dual-stack mode(IPV4 and IPV6). This applies to both RAC and standalone Oracle servers.

## Create a Protection Group

Follow the steps listed below to create a Protection Group for the {{site.data.keyword.baas_full_notm}} Oracle adapter:

1.  Navigate to **Data Protection > Protection** and do one of the following:
    *   To create a job, click **Protect** > **Databases** > **Oracle Database**, where _Job Type_ is the type of the Protection Group.

    *   Alternatively, click the action icon next to the job and select the appropriate options.
        *   **Run Now**: Start a run of this Protection Group. You have the option to back up one or more objects.

            *   **Backup**: Select the objects you want to backup during the protection group run.

            *   **Backup Type**: If the policy supports multiple backup types, you can select an **incremental**, **full**, or **log** backup.

                If the policy associated with the Protection Group is configured with replication and archive targets, you can select those targets and set the archive parameters.

                If you select **Logs** as the backup type, after the log backup run is completed, the logs will be archived to the target configured in the policy.

                When archiving Oracle archive logs, the logs are stored locally and not archive retention specified in the policy.

        *   **Resume**: Resumes the paused protection job run.

        *   **Edit**: Edits the selected protection group.

        *   **Delete**: Deletes the selected protection group.

2.  In the **New Protection** pop-up, do one of the following:

    *   Protect the Oracle database by specifying a subset of options such as objects, protection group and policy by clicking the edit icon corresponding to the following options:

        1.  **Add Objects**: Select the registered Oracle source or click **Register Source** to register an Oracle source. Select the database instances to protect.

        2.  **Protection Group**: Specify a name for the Protection Group.

        3.  **Policy**: Select an existing policy or click **Create Policy** to create a new protection policy.

        4.  **Storage Domain**: Select the appropriate storage domain.

            This option is visible on the **New Protection** pop-up only if there are more than one storage domain.

        5.  Click **Protect**.

    *   To protect the Oracle database by specifying all the options, click **More Options**.

3.  On the New Oracle Job page, enter the **Job Title** and select the **Sources** from the list.

    You can either specify an existing protection group or create a new one

4.  On the Oracle Servers page, select the database instances to protect, and then click **Add**.

    When creating a new Protection Group, a shield next to the Object name indicates the Object is already been protected by another Protection Group.

    Click the view icon to see details about the object.

    If the object name is in red, it is inaccessible, invalid, or orphaned. If the object is in one of the specified states when the Protection Group runs, no snapshot is created. If the object becomes accessible, a snapshot is created the next time the Protection Group runs.

    The New Oracle Job page returns. Under Objects, verify that the database instances you selected are listed.

5.  Set the policy for your Protection Group. Under **Policy**, select the appropriate Protection Policy, choose one of the defaults (Bronze, Silver, or Gold), or create a New Policy if the available policies do not meet your schedule and retention needs.

    For Oracle databases running on Windows OS, you must create or select a policy with the **Periodic Full Backup** option set.

    For Oracle RAC databases, you can now choose one, some, or all of the RAC system nodes for backups or restores.

    If the object name is in red, it is inaccessible, invalid, or orphaned. If the object is in one of the specified error states, it can still be added to a Protection Group. However, during the Protection Group run, no snapshot for the object will be created. If the object becomes accessible, a snapshot is created the next time when the Protection Group runs.

6.  Click the edit icon next to the selected Object and depending on the operating system of the server, set the following options for the Object, and then click **Save**.


    | Options | Description |     |
    | --- | --- | --- |
    | Linux | Windows |
    | --- | --- | --- |
    | **System selects active node** | {{site.data.keyword.baas_full_notm}} will auto-select an active RAC node and configure the number of RMAN channels for the database object.<br><br>This option is enabled by default. | In the **SBT Library Path** field, specify the path to the {{site.data.keyword.baas_full_notm}} SBT Library. {{site.data.keyword.baas_full_notm}} SBT Library is installed on the Oracle Server along with the Backup agent. This option is enabled by default. |
    | **Select specific nodes** | For the RAC database, If you select this option, you can choose the number of RAC nodes and the number of RMAN channels and port to be used for each RAC node of the database object.<br><br>For a standalone database, if you select this option, you can choose the number of RMAN channels and port to be used for the node of the database object. | You can also select this option and to specify the path to the {{site.data.keyword.baas_full_notm}} SBT Library and to configure the number of RMAN channels for the database object. |
    | **Update Database credentials** | Enable this option to provide the updated database credentials of the user with SYSDBA privilege. | NA  |
    | **Delete Archive Log** | Toggle on and specify the days after which the archived logs on the source database should be deleted. If you specify the value as "0" days, source archived logs will be deleted immediately after each successful {{site.data.keyword.baas_full_notm}} backup.<br><br>If you choose this option, {{site.data.keyword.baas_full_notm}} issues `RMAN DELETE ARCHIVELOG` command to delete archived logs after each successful {{site.data.keyword.baas_full_notm}} backup. Logs are deleted if they are backed up at least once by RMAN and they are older than the days specified in this option. In addition, if you have archived logs not backed up previously by {{site.data.keyword.baas_full_notm}} and the time satisfies the log delete eligibility, then these logs will be deleted.<br><br>However, If you do not enable this option, {{site.data.keyword.baas_full_notm}} will not delete the archived logs after each {{site.data.keyword.baas_full_notm}} backup.<br><br>If you are using the “Oracle RMAN archived log delete policy” to manage archived logs deletion on this database, depending on the policy, it may prevent {{site.data.keyword.baas_full_notm}} from deleting the archived logs as Oracle manages the policy.<br><br>If you have upgraded {{site.data.keyword.baas_full_notm}} Data Cloud (both the {{site.data.keyword.baas_full_notm}} and Backup agent) from a previous version, then the archived logs for the existing Protection Group will not be deleted.<br><br>Starting with release 6.4.1, the archived logs are not deleted by default. |     |
    | **Continue backups if database changes to Primary** | Toggle on to continue the database backups even if the current role of the database in the Data Guard changes from standby to primary.<br><br>If toggled off, then the {{site.data.keyword.baas_full_notm}} stops taking the backup of the database when the current role of the database in the Data Guard changes to the primary. | NA  |

7.  Specify the number of channels to use for each node and a single instance DB.


9.  After selecting a Storage Domain, if you need to change any of the Advanced settings on the New Oracle Job page, scroll down and click **Edit** on the right.


    | Advance Settings | Description |
    | --- | --- |
    | Start Time | Available only if the selected policy is set to Backup Daily. Indicates when the job should run. The current time is displayed by default but you can change it. Enter the hour and minutes or use the up and down arrows on your keyboard. Verify the AM or PM setting.<br><br>The default time zone is the browser's time zone. You can change the time zone of the job by selecting a different time zone. |
    | End Time | Optional. Click and select the date when the Protection Group stops capturing snapshots. A Protection Group run that starts prior to this date runs until completion even if it completes after this date. |
    | QoS Policy | Select an appropriate quality of service (QoS) policy.<br><br>*   **Backup HDD**: The {{site.data.keyword.baas_full_notm}} writes the data directly to an HDD drive for this Protection Group.<br>*   **Backup SSD**: The {{site.data.keyword.baas_full_notm}} writes the data directly to an SSD drive for this Protection Group. Only specify this policy if you need fast ingest speed for a small number of Protection Groups.<br>*   **Backup Auto**: The {{site.data.keyword.baas_full_notm}} writes the data to both SSDs and HDDs. The distribution of data will be based on the current usage of SSD and HDD. This policy tries to achieve similar backup performance as the **Backup SSD** policy and reduces the SSD wear-out compared to the Backup SSD policy. This option is selected by default. For best performance, {{site.data.keyword.baas_full_notm}} recommends the Backup Target SSD policy. If necessary, you can change the policy at any time later. But it doesn’t change or take effect on the currently running task. |
    | Pre & Post Scripts | Edit this option to run scripts on the protected server before and/or after a Protection Group runs. If the Protection Group is protecting oracle databases from different hosts, then the pre and post scripts are executed for each oracle server. For more details, see [Configure Pre & Post Scripts](/docs/backup-recovery?topic=backup-recovery-configure-pre-post-scripts). |
    | Persist Mountpoints | If you enable this option, NFS mounts created during the Oracle database backup job are retained after the job is completed. Disabling this would remove the NFS mounts after the backup job is completed. By default, this option is enabled. You can disable this if needed. |
    | Alerts and Priority | Optional. Select one or more of the following settings if you want alerts to be created for the following triggers:<br><br>*   **Success**: Create an informational alert when a Protection Group completes successfully. Emails are not sent when informational alerts are created.<br>*   **Failure**: Create a critical alert if the Protection Group fails to complete. Emails are sent when critical alerts are created.<br>*   **SLA Violation**: Create a warning alert if the Protection Group takes longer than the time period specified in the SLA field. Emails are sent when warning alerts are created. For more information, see [Alerts](../Dashboard/Monitoring/Alerts.htm). |
    | Priority | Select a priority for the Protection Group execution. IBM Cloud Supports concurrent backups.<br><br>However, if the number of jobs exceeds the ability to process jobs, this will be the priority of implementation of the jobs:<br><br>1.  High-priority jobs<br>2.  Medium-priority jobs<br>3.  Low-priority jobs. |
    | Email Recipients | You can add email addresses to a Protection Group to notify the email recipients when alerts are triggered for the job. |
    | SLA | The service-level agreement (SLA) defines how long the administrator expects a Protection Group run to take.<br><br>*   **Incremental**: Enter the number of minutes you expect an incremental backup Protection Group run takes to complete. An incremental backup captures only the differences (changed blocks) since the last run.<br>*   **Full**: Enter the number of minutes you expect a full backup Protection Group run takes to complete. A full backup captures the entire object (all blocks). |
    | Cluster Interface | Select one of the following options to assign the interface group for the protection group.<br><br>*   **Auto Select**: Toggle on this option if you want to use the same interface group that was selected during the Oracle source registration.<br>*   **Interface Group**: Select the interface group to use for the backup.<br><br>If You select a different interface group from what was selected during the Oracle source registration, the system will use this VLAN for all the objects in the protection job. |
    | Description | Optional. Click the plus sign and enter a description for the Protection Group. |

10.  **Run Now**: Start a run of this Protection Group. You have the option to back up one or more objects.

    If the policy associated with the Protection Group is configured with replication and archive targets, you can select those targets. If the policy supports multiple backup types, you can select an **incremental**, **full**, or **log** backup.

    If you select **Logs** as the backup type, after the log backup run is completed, the logs will be archived to the target configured in the policy.

11.  Click **Protect**.


Your Protection Group runs appear in the list on the Protection Groups page.

## Download Debug Logs for Oracle Backup Jobs

You can now download the debug logs (`rman_shell.INFO, precheck_logs.INFO, alert_log.INFO, tnsnames_ora.INFO, sparse-db-config.txt`) as a compressed file for both successful and failed backup runs.

To download the debug logs:

*   **For a job run**: To download the debug logs for a job run, click the **Options** icon next to a completed or a failed job run and select **Download Debug Logs** to download debug logs for all the databases in the job.


*   **For a Specific database within the job run**: Optionally, you can download logs for a specific database within the job run. Click the **Options** icon next to a database and select **Download Debug Logs**.


You can download the debug logs only if the local snapshot is available on the {{site.data.keyword.baas_full_notm}}.

Once your Protection Group completes its first run, the database will be ready for recovery, replication, and archival.
