---

copyright:
  years: 2025,
lastupdated: "2025-12-12"

keywords: object type, report

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protection summary by object type
{: #report_protection_summary_objects_mc}

The Protection Summary by Object Type report shows the protection status of Objects as of the last runs that occurred in the specified date range.

- [Filter Report Data](#report_protected_objects_filter_mc)
- [Object Types](#report_protection_summary_object_type_mc)
- [Report Data](#report_protection_summary_report_data_mc)



## Filter Report Data
{: #report_protected_objects_filter_mc}

The report supports multiple filters to pare down the data that you want to view in the report:


| Object Name | Description |
|-------------------------|--------------------------------------------------------------|
| Date Range |	Select all the instances for which you want to see the reports. |
| Object Type |	Choose the types of objects for which you want to see the reports, such as All, MS SQL, Physical Server, or Oracle. |
| Protection Group Name	| Select all policies for which you want to see the reports. |

## Object Types
{: #report_protection_summary_object_type_mc}

The report includes the following details:

- Sources: Total number of sources
- Objects: Total number of objects
- Success: Total number of successful objects
- Error: Total number of failed objects
- Running: Total number of running objects
- Data Read: The total amount of data read, in bytes.
- Logical: The combined total amount of data, in bytes.

The following table describes a summary of the reports.

| Column Name | Description |
|-------------------------|--------------------------------------------------------------|
| Type |	The types of objects, such as Kubernetes or Physical. |
| Sources	    | The total number of sources. |
| Objects      |	The total number of objects. |
| Success |	The total number of objects that were backed up successfully during the specified date range. |
| Error |	The total number of objects that experienced one or more backup run failures during the specified date range. |
| Running |	The total number of objects that were running during the specified date range. |
| Data Read |	The total amount of data read, in bytes, during the specified date range. |
| Logical |	The combined total amount of data, in bytes, during the specified date range. |



## Report Data
{: #report_protection_summary_report_data_mc}

The following table describes the data that is displayed in the Data table.


| Column Name | Description |
|-------------------------|--------------------------------------------------------------|
| Object |	The hostname or IP address of the object. |
| Error Runs	    | The total number of objects that experienced one or more backup run failures during the specified date range. |
| Last Run Status	    | Indicates whether the last run has a status of success, warning, or error. |
| Start Time	    | The date and time when the protection run started. |
| End Time	    | The date and time when the protection run ended. |
| Run Type      |	Indicates whether the backup was run as incremental or full. |
| Data Read |The total amount of data read, in bytes, during the specified date range. |
| Logical |	The combined total of data in the objects that are protected by Data Management.  |







