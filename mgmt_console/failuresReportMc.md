---

copyright:
  years: 2025,
lastupdated: "2025-12-12"

keywords: report, failures

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Failed objects
{: #report_failed_objects_mc}

This report lists all the Objects for which an error occurred during the last runs and prevented the Objects from being backed up.

- [Filter Report Data](#report_failed_objects_filters_mc)
- [Object Types](#report_object_type_mc)
- [Report Data](#report_data_failed_object_mc)

## Filter Report Data
{: #report_failed_objects_filters_mc}

The report supports multiple filters to pare down the data that you want to view in the report:

| Object Name | Description |
|-------------------------|--------------------------------------------------------------|
| Date Range	    | Select start and end dates. |
| Consecutive Failures |	Enter the number of consecutive failures. |
| Object Type |	Choose the types of objects to include, such as Generic NAS, Isilon, NetApp, Physical, Pure, and VMware. |
| Protection Group Name	| Search by Protection Group name based on selected filters.  |

## Object Types
{: #report_object_type_mc}

The report includes the following details:

- Total number of sources
- Total number of objects
- Total number of failed objects

The following table describes a summary of the reports.

| Column Name | Description |
|-------------------------|--------------------------------------------------------------|
| Type |	The types of objects, such as Kubernetes or Physical. |
| Sources	    | Total number of sources. |
| Total Objects      |	Total number of objects. |
| Failed Objects |	Objects with one or more backup failures during the selected date range. |



## Report Data
{: #report_data_failed_object_mc}

The following table describes the data that is displayed in the data table.


| Column Name | Description |
|-------------------------|--------------------------------------------------------------|
| Object |	The hostname or IP address of the object. |
| Failed Backup	    | The date and time when the last backup attempt failed. |
| Run Type      | Indicates whether the backup was incremental or full. |
| Message |	The reason the object could not be backed up. |



