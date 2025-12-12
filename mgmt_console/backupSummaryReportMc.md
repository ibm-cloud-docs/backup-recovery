---

copyright:
  years: 2025
lastupdated: "2025-12-12"

keywords: report, backup, summary

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Backup summary
{: #report_backup_summary_mc}

This report shows an overview of the Protection Groups that are run on the Data Source Instance for the specified filter criteria. For each Protection Group, this report shows how many times the Protection Group runs, total bytes read and statistics about the last time the Protection Group ran.

- [Filter Report Data](#report_backup_summary_filters_mc)
- [Charts](#report_chart_backup_summary_mc)
- [Report Data](#report_data_backup_summary_mc)

## Filter Report Data
{: #report_backup_summary_filters_mc}

The report supports multiple filters to pare down the data that you want to view in the report:

| Object Name | Description |
|-------------------------|--------------------------------------------------------------|
| Date Range | Start and end dates. |
| Registered Source	    | Select sources to view reports. Use the drop-down to choose the hostname or IP address. |
| Protection Group Name	| Search by the name of the Protection Group based on selected filters.  |
| Run Status |	Choose object types to view reports, such as All Protection Groups, Success, Error, Running, or Canceled. |




## Charts
{: #report_chart_backup_summary_mc}

The report includes the following charts:

- Overview of Last Run Statuses
- Runs Hourly



## Report Data
{: #report_data_backup_summary_mc}

The following table describes the data that is displayed in the Data table.


| Column Name | Description |
|-------------------------|--------------------------------------------------------------|
| Protection Group |	Protection groups that are associated with the protection run for the object. |
| Source	    | Hostname or IP address of the registered source.|
| Runs      |	Number of times the Protection Group has run. |
| Last Run Success/Error |	Total number of backup runs with a status of success or error. |
| Data Read (total)	| Total amount of data read, in bytes.|
| BCO Violation	| Indicates whether the BCO violation check passed. |
| Last Run Status	| Status of the last run (success or error). |
| Bandwidth	| Displays bandwidth data. |


