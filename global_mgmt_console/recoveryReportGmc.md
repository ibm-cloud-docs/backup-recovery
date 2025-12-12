---

copyright:
  years: 2025,
lastupdated: "2025-12-12"

keywords:

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Recovery
{: #report_recovery_gmc}

The Recovery report provides a summary and list of all the clone and recovery tasks that were executed. It also provides other details such as the time taken for the operation and status of the operation.



Example use case: How many recovery tasks failed in the last week?

- [Filter Report Data](#report_recovery_filter_gmc)
- [Summary](#report_recovery_summary_gmc)
- [Charts](#report_recovery_charts_gmc)
- [Report Data](#report_recovery_report_data_gmc)

## Filter Report Data
{: #report_recovery_filter_gmc}

The report supports multiple filters to pare down the data that you want to view in the report:


| Object Name | Description |
|-------------------------|--------------------------------------------------------------|
| Source	    | Select all the sources for which you want to see the reports. Click the drop-down arrow to select the hostname or IP address of the registered source. |
| Instance |	Select all the instances for which you want to see the reports. |
| Object Type |	Choose the types of objects for which you want to see the reports, such as Generic NAS, Isilon, NetApp, Physical, Pure, VMware. |
| Status	| Filter by the status of the recovery task — Canceled, Failed, Running, Success, and Warning. |
| Time Range	| Set the time period for your report. If you set a time period, the report displays all objects that had a backup run during the selected time period. If an object is no longer protected, the report would still display data if the object had a backup run during the selected time period. If an object is protected and if it did not have a backup run during the selected time period, the report does not display the data specific to this object.|
| Object	| Enter an object name in the search bar to filter the reports by object name. |



## Summary
{: #report_recovery_summary_gmc}

The summary provides a summary of the report for the specified period.

- Success Rate: Total Successful or Total Runs.
- Total Recoveries: The total number of recoveries.
- Successful: The total number of successful runs.
- Success: The total number of recoveries with status Success.
- Warning: The total number of recoveries with status Warning.
- Failed: The total number of recoveries with status Failed.
- Canceled: The total number of recoveries with status Canceled.
- Running: The total number of recoveries with status Running


## Charts
{: #report_recovery_charts_gmc}

The report includes the Recovery Status by Type chart.

## Report Data
{: #report_recovery_report_data_gmc}

The following table describes the data displayed in the Data table. Use the search bar to filter the data by object name, source, system name, task name, or username. To add or remove columns, click the gear icon and enable or disable the toggle switch for each data type.

You can add or remove columns. For more information, see Customize table columns.
{: note}

| Column Name | Description |
|-----------------|--------------------------------------------------------------------------------|
| Start Time | The date and time at which the recovery task started.|
| Object Name | The name of the object.|
| Source | The hostname or IP address of the registered source.|
| Recovery Point | The date and time of the backup run from which the object was recovered.|
| Duration | The time taken by the recovery task.|
| Task Name | The name of the recovery task.|
| Username | The name of the user who initiated the recovery.|



