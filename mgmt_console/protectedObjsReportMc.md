---

copyright:
  years: 2025,
lastupdated: "2025-12-12"

keywords: heatmap, report

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protected objects heatmap
{: #report_protected_objects_mc}

The Protected Objects report provides a summary and list of all protected objects that had a backup run. You can view the backup status and the objects with an active snapshot.

- [Summary](#report_protected_object_heatmap_mc)
- [Filter Report Data](#report_protected_objects_filter_mc)



## Summary
{: #report_protected_object_heatmap_mc}

The summary provides a summary of the report for the specified period:

- Total Runs: The total number of protection runs.
- Success: The total number of successful runs.
- Error: The total number of protection runs with a Failed status.
- Canceled: The total number of protection runs with a Canceled status.
- Total Objects: The total number of objects.

## Filter Report Data
{: #report_protected_objects_filter_mc}

The report supports multiple filters to pare down the data that you want to view in the report:


| Object Name | Description |
|-------------------------|--------------------------------------------------------------|
| Date Range |	Select the time period for which you want to view the reports. |
| Aggregate By	    | Choose how to group the data. Use the drop-down to select the hostname or IP address of the registered source. |
| Time Zone |	Select the time zone for the reports. |
| Object Type |	Choose the object types to include, such as All, MS SQL, Physical Server, or Oracle. |
| Registered Source	    | Select the sources to include. Use the drop-down to choose the hostname or IP address of the registered source. |
| Protection Group Name	| Select the protection policies to include in the report. |
| Object	Name| Enter an object name in the search bar to filter reports by object name. |





