---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: microsoft sql, clone, databases

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Clone microsoft sql server databases
{: #clone_microsoft_sql_server_databases}

{{site.data.keyword.baas_full}} now provides the ability to clone an MS SQL database to a Standalone SQL Server, FCI, and AG. A major advantage is that the cloned MS SQL database files reside on the Storage Domain and require no additional storage from the MS SQL host. Multiple clones from a single source of MS SQL Database backup require little or no additional storage space in the {{site.data.keyword.baas_full_notm}}.

Cloning an MS SQL database is supported with Volume-based and file base backup methods.

## Clone an ms sql database
{: #clone_an_ms_sql_database}

1. Navigate to **Test & Dev** > **Clone** > **Databases**.
2. Search and select the MS SQL database. You can optionally perform search by [specifying the wildcard character \*](/docs/allowlist/backup-recovery?topic=backup-recovery-use_wildcard_*_in_search_for_recovery&interface=ui). You can also optionally narrow the search results by specifying filter criteria.
3. Provide the following information listed in the table, and then click **Clone**.


    | Options | Description |
    | --- | --- |
    | Task Name | (_Optional_) Change the default name for the Clone task. |
    | Objects Being Cloned | By default, the database you selected for cloning is displayed. You can click the change icon to select a different database. |
    | Clone Point | By default, the latest snapshot is cloned. Optionally click the Change icon to select a different snapshot to clone. The selected Restore Point is listed |
    | SQL Host | Select the host to which you want to clone the database. By default, you can select only the physical servers as the host. If you want to select MS SQL FCI as the SQL host, Contact [IBM Cloud Support](https://cloud.ibm.com/unifiedsupport/supportcenter).<br><br>Cloning the MS SQL database to an MS SQL FCI is supported only if the following criteria are met:<br><br>*   Microsoft SQL Server version must be 2012 and above, supported on SMB file server.<br>*   The MS SQL database to be cloned backed up with a file-based protection group. |
    | SQL Instance | Select the instance for the cloned database. |
    | Rename Database? | Enable this option if you want to rename the cloned database created by this task. |
    | {{site.data.keyword.baas_full_notm}} Network interface | Select one of the following options:<br><br>*   **Auto Select**: By default, the **Auto Select** option is enabled and the clone task automatically selects the correct VLAN.<br>*   **Interface Group**: select a configured interface group from the **Interface Group** drop-down. |
    {: caption="" caption-side="bottom"}


## Cancel a database clone
{: #cancel_a_database_clone}

You can cancel a running clone task for an individual database. Canceling the task may leave the database in an unusable state. You can delete the database manually and then start another clone task as needed.

1. Navigate to **Test & Dev**.
2. Click a Task Name link in the table to display the details page.
3. Click the **Cancel** button. (The Cancel button is available only if the task is running.)
4. Respond to the confirmation prompt.

## Tear down a database clone
{: #tear_down_a_database_clone}

To delete an MS SQL database clone from the {{site.data.keyword.baas_full_notm}} Storage Domain, perform the following steps:

1. Navigate to **Test & Dev**.
2. Click the cloned task or hover your mouse over the actions menu located next to the cloned task you want to delete.
3. Click **Tear Down Clone** > **Yes, Tear Down**.

The MS SQL database clone will be deleted from the {{site.data.keyword.baas_full_notm}} Storage Domain.

The teardown of an MS SQL clone database removes it from the SQL server; the user does not have to do this beforehand. However, in the rare instance you created other additional databases with files on the mounted drive that the cloned database is on, those additional databases should be deleted first through the SQL Server Management Studio.
