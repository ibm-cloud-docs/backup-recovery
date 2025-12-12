---

copyright:
  years: 2025,
lastupdated: "2025-12-12"

keywords:

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protected or unprotected objects
{: #report_protected_unprotected_objects_gmc}

The Protected or Unprotected Objects report provides a summary and list of objects along with their protection status.

You can identify objects that are not associated with a Protection Group. The report does not contain data about IBM® views.

Example use case: Are all the objects in my vCenter protected?


- [Filter Report Data](#report_protected_unprotected_objects_filter_gmc)
- [Summary](#report_protected_unprotected_objects_summary_gmc)
- [Charts](#report_protected_unprotected_objects_charts_gmc)
- [Report Data](#report_protected_unprotected_objects_report_data_gmc)

## Filter Report Data
{: #report_protected_unprotected_objects_filter_gmc}

The report supports multiple filters to pare down the data that you want to view in the report:



| Object Name | Description |
|-------------------------|--------------------------------------------------------------|
| Instance |	Select all the instances for which you want to see the reports. |
| Source	    | Select all the sources for which you want to see the reports. Click the drop-down arrow to select the hostname or IP address of the registered source. |
| Source Type |	Choose the types of sources for which you want to see the reports. |
| Protection Status |	Filter by object protection status — Protected or Unprotected. |
| Object	| Enter an object name in the search bar to filter the reports by object name. |



## Summary
{: #report_protected_unprotected_objects_summary_gmc}

The summary provides a summary of the report for the specified period:

- Protected Objects: The percentage of Protected Objects to Total Objects.
- Total Sources: The total number of sources.
- Total Objects: The total number of objects.
- Protected Objects: The total number of protected objects.
- Unprotected Objects: The total number of unprotected objects.

## Charts
{: #report_protected_unprotected_objects_charts_gmc}

The report includes the following two charts:

- Protection Status by Type
- Unprotected Objects by Source

## Report Data
{: #report_protected_unprotected_objects_report_data_gmc}

TThe following table describes the data displayed in the Data table. Use the search bar to filter the data by object name, protection status, source, or system name. To add or remove columns, click the gear icon, and enable or disable the toggle switch for each data type.

| Column Name | Description |
|-------------------|-----------------------------------------------------------------------------------------------------------------|
| Object Name | The name of the object.|
| Protection Status | The protection status of the object.|
| Source | The name of the registered source.|
| System | The name of the cluster on which the object is registered.|
| Logical Data | The combined total of data in the objects that are protected by Data Management. These metrics are different depending on workload type. <br>- VMs - The data size reported by VMware is the provisioned amount, not the actual data residing in the VM. For example, if a VM is provisioned for 1 TB but contains only 100 GB of data, VMware reports it as 1 TB.</br><br>- All Other Workloads - The data size reported is the actual front end data residing on the server. If a server with 1 TB capacity contains 100 GB of data, the server reports 100 GB.</br><br>Data Management does not include unprotected objects in these metrics. Currently, the logical data value shown on the Data Management Dashboard is a sum of the logical data values captured across all the protection runs. For instance, if the source has 100 GB of logical data, and assuming it remains at 100 GB for the first 10 protection runs, Data Management would report, after 10 runs, the Logical Data to be 1000 GB (1 TB).</br> |



