---

copyright:
  years: 2025
lastupdated: "2025-08-27"

keywords: recover, database, sap hana, prerequisites, recovery, arguments, sampe

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Recover SAP HANA database
{: #recover_sap_hana_database}

You can restore the SAP HANA cluster or object(s) such as databases in SAP HANA. You can restore to the same SAP HANA cluster or a different SAP HANA cluster.

The SAP HANA database agent leverages the [restore](https://help.sap.com/docs/PRODUCT_ID/6b94445c94ae495c83a19646e7c3fd56/c3c66b63bb571014b3e5ad8618cda1ad.html?state=PRODUCTION&version=2.0.02&locale=en-US){: external} statements provided by SAP HANA. You can create a Recovery configuration and specify the objects such as databases backed up using the Protection Group.

In the Recovery configuration, you need to specify the object you plan to [restore](https://help.sap.com/docs/PRODUCT_ID/6b94445c94ae495c83a19646e7c3fd56/c3c66b63bb571014b3e5ad8618cda1ad.html?state=PRODUCTION&version=2.0.02&locale=en-US){: external} and define the script parameters required by the restore statements provided by SAP HANA.

## Recovery arguments
{: #recovery_arguments}

In the Recovery configuration, you can define the following script parameters for your SAP HANA recovery:

The recovery arguments are optional arguments, you can use these arguments based on your requirements.


| Recovery Arguments | Mandatory ? | Description |
| --- | --- | --- |
| `--is-specific-data-backup-restore` | Optional (available only for backward compatibility) | Allows you to restore the database to a specific data backup of a snapshot that you selected while performing the restore.<br><br>For example:<br><br>\--is-specific-data-backup-restore=<bool><br><br>\--is-specific-data-backup-restore=true<br><br>\--is-specific-data-backup-restore=false<br><br>*   By default `--is-specific-data-backup-restore` is set to **false**. Hence the selected restore would be a point-in-time restore of the selected snapshot. <br>    <br>*   If the selected snapshot is **Full**, set `--is-specific-data-backup-restore` to **true**, then the specific full data backup is restored. If the selected snapshot is an Incremental or Differential backup, then the Full backup, which is less than the selected snapshot time, would be restored. |
| `--source-sid` | Mandatory for alternate restore use cases. | Provide the SAP HANA source system SID for the SAP HANA host used for the alternate restore use case.<br><br>For example:<br><br>`--source-sid=<sid>`<br><br>`--source-sid=H00`<br><br>The `--source-sid` argument is applicable only for alternate restore use cases. |
| `--user-store-key` | Optional | The hdbuserstore key to connect to the SAP HANA SYSTEMDB.<br><br>For example: `--user-store-key=H00_KEY` |
| `\--start-database` | Optional | Set the value to `true` if you want to activate the database after the recovery is completed. Supported values are `true` or `false`. By default the value is set to `true`. |
{: caption="Recovery arguments" caption-side="bottom"}

## Prerequisites
{: #prerequisites}

Ensure that:

- The Protection Group associated with the object that you plan to restore is paused or in not running state.
- The target database exists, or you manually create the target database before you start the restore process.
- The SAP HANA version used for recovery must always be the same or newer than the version used for backup of data and log.
- All the required the SAP HANA services are up and running. You can use the following sample command to verify the required services:

```sh
    sap-hana-2:cohadm> sapcontrol -nr ${TINSTANCE} -function GetProcessList
    03.09.2024 14:58:23
    GetProcessList
    OK
    name, description, dispstatus, textstatus, starttime, elapsedtime, pid
    hdbdaemon, HDB Daemon, GREEN, Running, 2024 09 02 16:49:20, 22:09:03, 14346
    hdbcompileserver, HDB Compileserver, GREEN, Running, 2024 09 02 16:49:26, 22:08:57, 14488
    hdbindexserver, HDB Indexserver-COH, GREEN, Running, 2024 09 03 13:26:33, 1:31:50, 24735
    hdbnameserver, HDB Nameserver, GREEN, Running, 2024 09 02 16:49:21, 22:09:02, 14374
    hdbpreprocessor, HDB Preprocessor, GREEN, Running, 2024 09 02 16:49:26, 22:08:57, 14491
    hdbwebdispatcher, HDB Web Dispatcher, GREEN, Running, 2024 09 02 16:50:31, 22:07:52, 15062
    hdbxsengine, HDB XSEngine-COH, GREEN, Running, 2024 09 03 13:26:53, 1:31:30, 25060
    hdbindexserver, HDB Indexserver-TESTDB, GREEN, Running, 2024 09 03 13:30:39, 1:27:44, 31263
    hdbxsengine, HDB XSEngine-TESTDB, GREEN, Running, 2024 09 03 13:31:03, 1:27:20, 31567
```

    To stop and start the required services, use the following command:
```sh
    `sap-hana-2:cohadm> HDB stop`
    `sap-hana-2:cohadm> HDB start`
```

## Recover SAP HANA databases
{: #recover_sap_hana_databases}

To restore SAP HANA:

1. Navigate to **Data Protection > Recoveries**.
2. Click **Recover** at the top right of the page and then select **Universal Data Adapter**.
3. Search for the Protection Group that contains the snapshots to recover from by entering characters of the Protection Group name, or by specifying the wildcard character **\***.
4. From the list of search results, select the Protection Group that contains the snapshots to recover.
5. In the **Object** field, enter the name of the database or table that you had backed up using the selected Protection Group. By default, the latest snapshot is selected for recovery. You can also select a point-in-time snapshot or specific data backup that you want to recover. Click the pencil icon and perform the following:
    *   For point-in-time snapshots, in the **Edit recovery point for <Protection\_Group\_Name>** dialog, click the **Timeline** tab, choose the **Date** and select the **point-in-time** from the **timeline** slider and click **Select Recovery Point**.

    *   For specific data backup snapshots, in the **Edit recovery point for <Protection\_Group\_Name>** dialog, click the **list** tab and select the specific **Snapshot Time** that you want and click **Select Recovery Point**.

6. Click **Next: Recover Options**.
7. In the **Recover As** section, based on your requirements, select any one of the following options:


    | Options | Description |
    | --- | --- |
    | Replace Original | To restore to the same SAP HANA database. |
    | New Object | To restore to a different database on the same SAP HANA cluster or a different SAP HANA cluster.<br><br>Select the **Target** SAP HANA cluster to which the data needs to be restored. Also, ignore **Overwrite existing object with the same name** toggle option. It is not applicable for SAP HANA. |
    {: caption="Recovery As options" caption-side="bottom"}

8. In the **Recovery Options** section, based on your requirements, select any one of the following options:


    | Options | Description |
    | --- | --- |
    | Mounts | Enter the number of VIPs you want to use while reading data from the {{site.data.keyword.cloud_notm}} Backup and Recovery storage. |
    | Concurrency | The maximum number of restore streams that is created to exchange data with the cluster. By default, maximum scaling is set to empty.<br><br>The same value is set to the `parallel_data_backup_backint_channels` parameter in the **global.ini** file for the protected database. Also, the same value is set for the `max_restore_streams` parameter in the SAP HANA parameter file of the database selected for protection. |
    | Custom Options | Specify the [Recovery Arguments](#recovery_arguments) based on your requirement. For examples, see [Sample Recovery Use Cases](#sample_recovery_use_cases). |
    | Rename | Provide the new name for the object, if you want to restore the object with a new name. |
    | Task Name | Change the default name of the recovery task. |
    {: caption="Recovery Options" caption-side="bottom"}

9. Click **Start Recovery**.


### Sample recovery use cases
{: #sample_recovery_use_cases}


| Use Case | Sample Recovery Arguments |
| --- | --- |
| Overwrite Point In Time Restore (PITR) to the same SAP HANA database. | In the **Recover As** section, select **Replace Original**. Under the **Recovery Options** section, do not specify any arguments in the **Custom Options** field. |
| Overwrite Point In Time Restore (PITR) the same SAP HANA database overriding the user store key. | In the **Recover As** section, select **Replace Original**. Under the **Recovery Options** section, specify `--user-store-key=H00_KEY` in the **Custom Options** field. |
| Restore specific data backup from the snapshot to the same SAP HANA database. | In the **Recover As** section, select **Replace Original**. Under the **Recovery Options** section, specify `--user-store-key=H00_KEY --is-specific-data-backup-restore=true` in the **Custom Options** field. |
| Point In Time Restore (PITR) to a different database on the same SAP HANA system. | Perform the following:<br><br>1. In the **Recover As** section, select **New Object** and choose the same source name in the **Target** drop-down.<br>    <br>2. Under the **Recovery Options** section, specify `--user-store-key=H00_KEY --source-sid=H00` in the **Custom Options** field and specify the name for the new database in the **Rename** field. |
| Point In Time Restore (PITR) to a different database on a different SAP HANA system | Perform the following:<br><br>1. In the **Recover As** section, select **New Object** and choose the target source name in the **Target** drop-down.<br>    <br>2. Under the **Recovery Options** section, specify `--user-store-key=H00_KEY --source-sid=H00` in the **Custom Options** field and specify the name for the new database in the **Rename** field. |
| Restore specific data backup from the snapshot to a new database on the same SAP HANA system. | Perform the following:<br><br>1. In the **Recover As** section, select **New Object** and choose the same source name in the **Target** drop-down.<br>    <br>2. Under the **Recovery Options** section, specify the following in the **Custom Options** field and specify the name for the new database in the **Rename** field:<br>    <br>    `--user-store-key=H00_KEY`<br>    <br>    `--source-sid=H00`<br>    <br>    `--is-specific-data-backup-restore=true` |
{: caption="Sample recovery use cases" caption-side="bottom"}

