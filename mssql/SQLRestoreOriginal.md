---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: microsoft sql, recovery point, database, physical server

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Recover microsoft sql server database over the original database
{: #recover_microsoft_sql_server_database_over_the_original_database}

Recovering an MS SQL database over the original database overwrites the original database. The recovered database will have the original database name.

The original database is deleted before the recovery. Therefore a recovery failure will also lead to the loss of the original database.

To recover the database to the original database:

1. In the {{site.data.keyword.baas_full}} Console, select **Data Protection > Recoveries**.

2. Click **Recover** located at the top right of the page and then select **Databases** > **MS SQL**.

    The Restore SQL page appears.

3. Search for the databases you want to recover from by entering characters of the database name, Protection Group name, or by [specifying the wildcard character \*](/docs/backup-recovery?topic=backup-recovery-use_wildcard_*_in_search_for_recovery&interface=ui).

    A list of databases is displayed with details like {{site.data.keyword.baas_full_notm}}, Protection Group, Physical Server, and Create Date. The Create Date option indicates the date and timestamp when the database was created.

    Operations like rename may update the creation date at the SQL Server.

4. From the list of search results, select one or more databases you want to recover. You can select multiple MS SQL databases only if they are protected by the same protection group.

    The option to select multiple databases in a recovery task is an Early Access feature. Contact [IBM Cloud Support](https://cloud.ibm.com/unifiedsupport/supportcenter) for enabling this feature.

    By default, the latest recovery point is selected. To select a different recovery point, from the selected lists, mouse over the recovery point, and then click the edit icon.

    The Edit recovery point for <database\_name> page appears.

    By default, the option to select the recovery point is displayed in **Timeline** mode. You can also choose to view the option in **List** mode by clicking the **List** tab in the top-right corner of the page. The **Timeline** mode displays the full backups (recovery points) captured at the scheduled time and the CDP-based data available for point-in-time restore. The **List** tab displays only the full backups captured at a scheduled time.

    *   To select a recovery point from the Timeline mode:

        1. From the **Choose a date** drop-down, select a date.

            The recovery points available on the selected date are displayed in blue dots on the timeline bar and the point-in-time for restore is displayed in a solid green line on the timeline bar.

        2. To recover from a full backup, click the corresponding blue dot. For point-in-time restore, specify the exact point-in-time recovery by sliding the timeline bar.

        3. By default, the data is recovered from the local cluster. You can also select a recovery point archived to the cloud by clicking the cloud icon if available in the **Location** option.

        4. Click **Select Recovery Point** and then go to step 5.


    *   To select a recovery point from the List mode:

        1. Select the **List** tab from the top-right corner of the Edit recovery point for <database\_name> page.

            By default, the page displays the recovery points available for the last 30 days.

        2. You can also select a recovery point from a different date range by selecting the date range from the Date drop-down list. The page displays the available recovery points in the specified range.

        3. Select the recovery points to be recovered.

        4. By default, the data is recovered from the local cluster. You can also select a recovery point archived to the cloud by clicking the cloud icon if available in the **Location** option.

        5. Click **Select Recovery Point** and then go to step 5.


5. Click **Next: Recover Options**.

6. From the **Targets** option, select **Overwrite Original Database** to recover the database to the original location.

    The original database is deleted before the recovery. Therefore a recovery failure will also lead to the loss of the original database.

7. Specify the following **Recovery Options**:


    | Recovery Options | Description |
    | --- | --- |
    | VDI Restore Settings | Enable this option to specify the WITH clause that you want to use for the restore.<br><br>*   This option is available only for restoring the databases that are protected using [Backup Microsoft SQL Server (VDI-based)](/docs/backup-recovery?topic=backup-recovery-backup_microsoft_sql_server_vdi-based).<br>    <br>*   This option is not available for system database restore. |
    | Keep CDC | Enable this option to restore a protected database that has the change data capture (CDC) enabled. If the protected database is not CDC enabled and the user tries to restore it with Keep CDC then, the database will be restored without CDC.<br><br>*   This option is available only if the database is restored with recovery.<br>    <br>*   This option is not available for system database restore. |
    | Cluster Interface | By default, the **Auto Select** option is enabled and the recovery task automatically selects the correct VLAN. If you disable this option, then select a configured interface group from the **Interface Grou**p drop-down. |
    | Task Name | Change the default name of the recovery task. |
    | Capture Tail Logs | Enable this option to capture records that have not yet been protected. They are captured to ensure all transactions are protected before restoring the database.<br><br>This option is not available for system database restore. |
    {: caption="" caption-side="bottom"}

8. Click **Start Recovery**.


The recovery task is initiated. You can monitor the recovery task from the Recoveries page.
