---
copyright:
  years: 2026
lastupdated: "2026-11-02"

keywords: ibm db2, database recovery, restore database, db2 backup, redirected restore

subcollection: backup-recovery

content-type: tutorial
services: backup-recovery
completion-time: 45m

---

{{site.data.keyword.attribute-definition-list}}

# Restore IBM DB2 databases
{: #restore-ibm-db2-database}
{: toc-content-type="tutorial"}
{: toc-services="db2"}
{: toc-completion-time="45m"}

You can restore IBM DB2 objects such as databases in IBM DB2. You can restore to the same IBM DB2 cluster or a different IBM DB2 cluster.
{: shortdesc}

The {{site.data.keyword.baas_full_notm}}DB2 connector uses the restore tool provided by IBM DB2. You can create a recovery configuration and specify the objects such as databases backed up using the protection group.

If you plan to restore the backup to a different IBM DB2 cluster, ensure that you add the following registry variable to the environment variables of the target IBM DB2 instance using the following commands:
{: note}

```bash
db2stop
db2set DB2_RESTORE_GRANT_ADMIN_AUTHORITIES=ON
db2start
```
{: pre}

## Objectives
{: #restore-db2-objectives}

In this tutorial, you learn how to:

- Understand redirected restore operations
- Restore IBM DB2 databases to the same or different cluster
- Configure restore settings for regular and redirected restores
- Cancel a recovery run

## Before you begin
{: #restore-db2-prereqs}

Before you begin this tutorial, ensure that you have:

- Access to the {{site.data.keyword.baas_full_notm}}Dashboard with appropriate permissions
- A protection group with IBM DB2 database backups
- If restoring to a different cluster, the target IBM DB2 cluster must be registered as a source on the {{site.data.keyword.baas_full_notm}}cluster
- For redirected restore operations, a script with redirection statements prepared

## Understanding redirected restore
{: #restore-db2-redirected-restore}
{: step}

This is an Early Access feature. Contact your {{site.data.keyword.baas_full_notm}}account team to enable the feature.
{: note}

{{site.data.keyword.baas_full_notm}}Platform supports redirected restore operation provided by IBM DB2. Using the redirected restore operation, you can perform the following while restoring the IBM DB2 backup:

- Replace the same IBM DB2 cluster using the existing storage groups and tablespace locations
- Redirect all tablespaces to new locations
- Redirect the subset of tablespaces to new locations
- Redirect all storage groups to new locations
- Redirect the subset of storage groups to new locations

Before performing the redirected restore operation using the {{site.data.keyword.baas_full_notm}}Platform, you must create a script with the redirection statements and provide the path of the script in the **Redirection script path** field in the IBM DB2 restore configuration. The script is replayed at the time of redirection for storage group and tablespaces containers.

The script should only contain redirection statements, and the restore statements must not be part of the script.
{: note}

The following is a sample of the script:

```bash
set stogroup paths for DATA_SG on '/var/ibmdb2/DATABASE1/db1.DMSFILETSP' , '/var/ibmdb2/DATABASE1/db1.DMSFILETSP2' , '/var/ibmdb2/DATABASE1/db1.DMSFILETSP3';
set stogroup paths for TEMP_SG on '/var/ibmdb2/DB1/db1.TMPFILETSP' ;
set tablespace containers for 0 using ( path '/var/ibmdb2/DATABASE1/NODE0000/BACKTST2/SYSCATSPACE' );
set tablespace containers for 3 using automatic storage;
```
{: codeblock}

## Select the protection group and recovery point
{: #restore-db2-select-recovery-point}
{: step}

1. In the {{site.data.keyword.baas_full_notm}}Dashboard, select **Data Protection > Recoveries**.

2. Click **Recover** at the top right of the page and then select **Universal Data Adapter**.

3. Search for the protection group that contains the snapshots to recover from by entering characters of the protection group name, or by specifying the wildcard character `*`.

4. From the list of search results, select the protection group that contains the snapshots to recover. By default, the latest recovery point is selected. To select a different recovery point, from the selected lists, mouse over the recovery point, and then click the **edit** icon.

   By default, the option to select the recovery point is displayed in **Timeline** mode. You can also choose to view the option in List mode by clicking the **List** tab in the top-right corner of the page. The **Timeline** mode displays the full backups (recovery points) captured at the scheduled time and the CDP-based data available for point-in-time restore. The **List** tab displays only the full backups captured at a scheduled time.

### Select a recovery point from Timeline mode
{: #restore-db2-timeline-mode}

1. From the **Choose a date** drop-down, select a date.

   The recovery points available on the selected date are displayed in blue dots on the timeline bar and the point-in-time for restore is displayed in a solid green line on the timeline bar.

2. To recover from a full backup, click the corresponding blue dot. For point-in-time restore, specify the exact point-in-time recovery by sliding the timeline bar.

3. Click **Select Recovery Point**.

### Select a recovery point from List mode
{: #restore-db2-list-mode}

1. Select the **List** tab from the top-right corner of the page.

   By default, the page displays the recovery points available for the last 30 days.

2. You can also select a recovery point from a different date range by selecting the date range from the Date drop-down list. The page displays the available recovery points in the specified range.

3. Select the recovery points to be recovered.

4. Click **Select Recovery Point**.

## Specify the objects to restore
{: #restore-db2-specify-objects}
{: step}

In the **Object** field, enter the name of the database or table that you backed up using the selected protection group. Click **+** to add multiple objects.

Click **Next: Recover Options**.

## Configure recovery options
{: #restore-db2-configure-options}
{: step}

### Select the recovery destination
{: #restore-db2-destination}

In the **Recover As** section, based on your requirements, select one of the following options:

| Option | Description |
| ------ | ----------- |
| Replace Original | To restore to the same IBM DB2 cluster. |
| New Object | To restore to a different IBM DB2 cluster. Select the **Target** IBM DB2 cluster to which the data needs to be restored. Also, enable **Overwrite existing object with the same name** and type **Yes**, if you want to overwrite the existing object with the same name. Ensure that the **Target** IBM DB2 cluster is registered as a source on the {{site.data.keyword.baas_full_notm}}cluster. |
{: caption="Recovery destination options" caption-side="bottom"}

### Configure restore settings
{: #restore-db2-restore-settings}

In the **Restore Settings** section, select **Regular Restore** or **Redirected Restore** based on your requirements.

#### Regular restore settings
{: #restore-db2-regular-settings}

If you have selected **Regular Restore**, configure the following:

| Field | Mandatory? | Default | Description |
| ----- | ---------- | ------- | ----------- |
| New Data Path | No | N/A | Enter the path on the target IBM DB2 instance to restore the data directory from the database. You can specify the path(s) as a comma-separated list of directories. If the path is not specified, the data is restored to the default database path, DFTDBPATH. You can view the default database path, DFTDBPATH, using the following command: `db2 get dbm cfg | grep DFTDBPATH` |
| New Log Path | No | N/A | Enter the path on the target IBM DB2 instance to restore the active logs from the restored database. |
| Local Database Directory | No | N/A | Enter the path of the local database directory. |
| Rollforward Database | No | Enabled | Roll forward the database to recover the database by applying transaction logs after restore. |
{: caption="Regular restore settings" caption-side="bottom"}

#### Redirected restore settings
{: #restore-db2-redirected-settings}

If you have selected **Redirected Restore**, configure the following:

| Field | Mandatory? | Default | Description |
| ----- | ---------- | ------- | ----------- |
| New Data Path | No | N/A | Enter the path on the target IBM DB2 instance to restore the data directory from the database. You can specify the path(s) as a comma-separated list of directories. If the path is not specified, the data is restored to the default database path, DFTDBPATH. You can view the default database path, DFTDBPATH, using the following command: `db2 get dbm cfg | grep DFTDBPATH` |
| Redirection Script Path | Yes | N/A | Enter the path of the script with the redirection statements. For more information, see [Understanding redirected restore](#restore-db2-redirected-restore). |
| New Log Path | Yes | N/A | Enter the path on the target IBM DB2 instance to restore the active logs from the restored database. |
| Local Database Directory | No | N/A | Enter the path of the local database directory. |
| Rollforward Database | No | Enabled | Roll forward the database to recover the database by applying transaction logs after restore. |
{: caption="Redirected restore settings" caption-side="bottom"}

### Configure advanced options
{: #restore-db2-advanced-options}

Optional. Enable **Advanced Options** to configure advanced settings:

| Field | Mandatory? | Default | Description |
| ----- | ---------- | ------- | ----------- |
| RPC Timeout In Seconds | No | 120 seconds | The request timeout between the IBM DB2 source and the {{site.data.keyword.baas_full_notm}}cluster. |
| Max IO Read Size To {{site.data.keyword.baas_full_notm}}| No | 4194304 bytes | The maximum size of the data packet that can be transferred to the {{site.data.keyword.baas_full_notm}}cluster. |
| Environment Variables | No | N/A | Provide the environment variables if you want to override the environment's default value. For example, `PATH=/sbin:/bin:/usr/bin` |
{: caption="Advanced options" caption-side="bottom"}

### Configure recovery options
{: #restore-db2-recovery-options}

In the **Recovery Options** section, based on your requirements, configure the following:

| Option | Description |
| ------ | ----------- |
| Mounts | Enter the number of VIPs you want to use while reading data from the {{site.data.keyword.baas_full_notm}}storage. |
| Concurrency | Not applicable for IBM DB2 cluster database recovery. |
| Rename | Allows you to rename the object, if you are recovering the object to **New Location**. If you are overwriting the existing object, do not rename the object using the database alias name. |
| Task Name | Change the default name of the recovery task. |
{: caption="Recovery options" caption-side="bottom"}

## Start the recovery
{: #restore-db2-start-recovery}
{: step}

Click **Recover** to start the recovery process.

The recovery task begins and you can monitor its progress in the **Data Protection > Recoveries** page.

## Cancel a recovery run (optional)
{: #restore-db2-cancel-recovery}
{: step}

When you cancel a recovery run, the IBM DB2 connector cancels the job by terminating the recovery processes.

To cancel a recovery run:

1. In the {{site.data.keyword.baas_full_notm}}Dashboard, select **Data Protection > Recoveries**.

2. Click the action icon next to the recovery run and click **Cancel Run**.

3. In the **Cancel Recovery** dialog box, select one of the following:

   - **Continue Recovery** if you want to continue the recovery run
   - **Cancel Recovery** if you want to cancel the recovery run

## Next steps
{: #restore-db2-next-steps}

After completing this tutorial, you can:

- Monitor the recovery progress in the {{site.data.keyword.baas_full_notm}}Dashboard
- Verify the restored database on the target IBM DB2 cluster
- Configure additional protection groups for other databases
- Set up automated backup schedules for your IBM DB2 databases
