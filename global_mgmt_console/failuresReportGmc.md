---

copyright:
  years: 2025,
lastupdated: "2025-12-12"

keywords: report, failures

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Failures
{: #report_failures_gmc}

The Failures report provides a summary and list of objects that had one or more backup run failures. It also helps you identify consecutive failures in the last three backups, and breaks down the failed objects by object type.

Example use case: Which object do I have no successful backup of in the last week?

- [Filter Report Data](#report_failures_filters_gmc)
- [Summary](#report_failures_summary_gmc)
- [Charts](#report_failures_charts_gmc)
- [Report Data](#report_failures_report_data_gmc)

## Filter Report Data
{: #report_failures_filters_gmc}

The report supports multiple filters to pare down the data that you want to view in the report:

| Object Name | Description |
|-------------------------|--------------------------------------------------------------|
| Instance |	Select all the instances for which you want to see the reports. |
| Source	    | Select all the sources for which you want to see the reports. Click the drop-down arrow to select the hostname or IP address of the registered source. |
| Protection Group |	Select all protection groups for which you want to see the reports. |
| Object Type |	Choose the types of objects for which you want to see the reports, such as Generic NAS, Isilon, NetApp, Physical, Pure, VMware. |
| Policy	| Select all policies for which you want to see the reports. |
| Time Range	| Set the time period for your report. |
| Object	| Enter an object name in the search bar to filter the reports by object name. |








## Summary
{: #report_failures_summary_gmc}

The summary provides a summary of the report for the specified period:

- Total Sources: The total number of sources.
- Total Objects: The total number of objects.
- Failed Objects: The total number of objects that experienced one or more backup run failures during the specified date range.
- Without Snapshots: The total number of objects without any snapshots.

## Charts
{: #report_failures_charts_gmc}

The report includes the following chart:

- Success and Failed Objects by Object Type



## Report Data
{: #report_failures_report_data_gmc}

The following table describes the data that is displayed in the Data table. Use the search bar to filter the data by object name, source, or policy.
To add or remove columns, click the gear icon and enable or disable the toggle switch for each data type. The data that is displayed in the Policy column is from the last backup run of the object in the specified time period.


| Column Name | Description |
|-------------------------|--------------------------------------------------------------|
| Object Name |	The name of the object. |
| Source	    | The hostname or IP address of the registered source. |
| Policy      |	The protection policy associated with the Protection Group. |
| Last Failed Run |	The date and time at which the last backup run failed. |
| Failed Backups	| The total number of backup runs that failed. |
| Failures in Last Three Backups	| The total number of failures in the last three backups. |
| Last Failure Reason	| The reason for the failure of the last backup. |
