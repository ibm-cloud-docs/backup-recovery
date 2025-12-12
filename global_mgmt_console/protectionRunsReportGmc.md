---

copyright:
  years: 2025,
lastupdated: "2025-12-12"

keywords:

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protection Runs
{: #report_protection_runs_gmc}

The Protection Runs report provides a summary and list of all backup activities per object per run. You can view the summary and success rate of protection runs. You can also view the snapshot status of the protection run.

Example use case: How many failed protection runs did I have in the last week?

- [Filter Report Data](#report_protection_runs_filter_gmc)
- [Summary](#report_protection_runs_summary_gmc)
- [Charts](#report_protection_runs_charts_gmc)
- [Report Data](#report_protection_runs_report_data_gmc)

## Filter Report Data
{: #report_protection_runs_filter_gmc}

The report supports multiple filters to pare down the data that you want to view in the report:


| Object Name | Description |
|-------------------------|--------------------------------------------------------------|
| Instance |	Select all the instances for which you want to see the reports. |
| Source	    | Select all the sources for which you want to see the reports. Click the drop-down arrow to select the hostname or IP address of the registered source. |
| Protection Group |	Select all protection groups for which you want to see the reports. |
| Object Type |	Choose the types of objects for which you want to see the reports, such as Generic NAS, Isilon, NetApp, Physical, Pure, VMware. |
| Run Status |	Choose the types of objects to include — Generic NAS, Isilon, NetApp, Physical, Pure, VMware, and so on. |
| Snapshot Status |	Filter by the status of the snapshot — Active or Expired. |
| Time Range	| Set the time period for your report. If you set a time period, the report displays all objects that had a backup run during the selected time period. If an object is no longer protected, the report would still display data if the object had a backup run during the selected time period. If an object is protected and if it did not have a backup run during the selected time period, the report does not display the data specific to this object.|
| BCO Status	| Indicates if the backup met its Backup Completion Objective within the defined time window.|
| Object	| Enter an object name in the search bar to filter the reports by object name. |

## Summary
{: #report_protection_runs_summary_gmc}

The glance bar provides a summary of the report for the specified period:
- Success Rate: Total Successful or Total Runs.
- Total Runs: The total number of protection runs.
- Total Successful: The total number of successful runs.
- Success: The total number of protection runs with status Success.
- Warning: The total number of protection runs with status Warning.
- Failed: The total number of protection runs with status Failed.
- Canceled: The total number of protection runs with status Canceled.
- Skipped: The total number of protection runs with status Skipped.
- Running: The total number of protection runs with status Running




## Charts
{: #report_protection_runs_charts_gmc}

The report includes the following two charts:
- Run Status by Policy
- Run Status by Protection Group Type
- Run Status Trend

## Report Data
{: #report_protection_runs_report_data_gmc}

The following table describes the data displayed in the Data table. Use the search bar to filter the data by object name, source, policy, or snapshot status. To add or remove columns, click the gear icon and enable or disable the toggle switch for each data type.



| Column Name | Description |
|----------------|---------------------------------------------------------------------------------------------------------------------------------|
| Start Time | The date and time at which the protection run started. |
| End Time | The date and time at which the protection run was completed.|
| Object Name | The name of the protected object. |
| Source | The hostname or IP address of the registered source. |
| Policy | The protection policy associated with the protection run for the corresponding object.|
| Protection Group | The protection groups associated with the protection run for the corresponding object.|
| Instance | The name of the instance. |
| BCO| Displays the defined Backup Completion Objective. |
| Snapshot Status | The status of the snapshot.|
| Duration | The time taken by the protection run.|
| Logical Data | The combined total of data in the objects that are protected by Data Management. These metrics are different depending on workload type.<br>VMs—The data size reported by VMware is the provisioned amount, not the actual data residing in the VM. For example, if a VM is provisioned for 1 TB but contains only 100 GB of data, VMware reports it as 1 TB.</br><br>All Other Workloads—The data size reported is the actual front end data residing on the server. If a server with 1 TB capacity contains 100 GB of data, the server reports 100 GB.</br><br>**Note:** Data Management does not include unprotected objects in these metrics. Currently, the logical data value that is shown on the Data Management Dashboard is a sum of the logical data values that are captured across all the protection runs. For instance, if the source has 100 GB of logical data, and assuming it remains at 100 GB for the first 10 protection runs, Data Management would report, after 10 runs, the Logical Data to be 1000 GB (1 TB).</br>|


