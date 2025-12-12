---

copyright:
  years: 2025,
lastupdated: "2025-12-12"

keywords:

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protected objects
{: #report_protected_objects_gmc}

The Protected Objects report provides a summary and list of all protected objects that had a backup run. You can view the backup status and the objects with an active snapshot.

Example use case: Do I have a good backup of my VM in the last month?


- [Filter Report Data](#report_protected_objects_filter_gmc)
- [Summary](#report_protected_objects_summary_gmc)
- [Charts](#report_protected_objects_charts_gmc)
- [Report Data](#report_protected_objects_report_data_gmc)

## Filter Report Data
{: #report_protected_objects_filter_gmc}

The report supports multiple filters to pare down the data that you want to view in the report:



| Object Name | Description |
|-------------------------|--------------------------------------------------------------|
| System |	Select all clusters for which you want to see the reports. |
| Source	    | Select all the sources for which you want to see the reports. Click the drop-down arrow to select the hostname or IP address of the registered source. |
| Type |	Choose the types of objects to include — Generic NAS, Isilon, NetApp, Physical, Pure, VMware, and so on. |
| Backup Status |	Filter by objects with successful backups or unsuccessful backups. |
| Last Run Status	| Filter by the status of the most recent protection run — Canceled, Failed, Running, Success, and Warning. |
| Time Range	| Set the time period for your report. If you set a time period, the report displays all objects that had a backup run during the selected time period. If an object is no longer protected, the report would still display data if the object had a backup run during the selected time period. If an object is protected and if it did not have a backup run during the selected time period, the report does not display the data specific to this object.|
| Object	| Enter an object name in the search bar to filter the reports by object name. |
| Organization | Choose one or more organizations to see the report data specific to the selected organizations. |

## Summary
{: #report_protected_objects_summary_gmc}

The summary provides a summary of the report for the specified period:

- Success Rate: Without Successful Backup / Total Objects.
- Total Objects: The total number of objects.
- With Successful Backup: The total number of objects that have one or more successful backups.
- Without Successful Backup: The total number of objects that did not have any successful protection runs.
- With Snapshots: The total number of objects with snapshots retained. This number can differ from the earlier “With Successful Backups”, for example, all backups fail for an object during the selected date range but the object still has actively retained snapshots from earlier backups (that occurred before the selected date range).
- Without Snapshots: The total number of objects without snapshots.

## Charts
{: #report_protected_objects_charts_gmc}

The report includes the following two charts:

- Protected Objects by Type
- Object Protection by Type

## Report Data
{: #report_protected_objects_report_data_gmc}

The following table describes the data displayed in the Data table. Use the search bar to filter the data by object name, source, or policy.
To add or remove columns, click the gear icon and enable or disable the toggle switch for each data type.


| Column Name | Description |
|-----------------------|----------------------------------------------------------------------------|
| Object Name | The name of the protected object. |
| Source      | The hostname or IP address of the registered source. |
| Policy      | The protection policy associated with the latest run of the object. |
| Last Run    | The date and time at which the last backup for the object ran. |
| Protection Status    | Identifies whether an object is protected or unprotected. |
| Active Snapshots | The total number of active snapshots for the object. |
| Successful Backups | The total number of successful backups for the object. |
| Unsuccessful Backups | The total number of unsuccessful backups for the object. |



