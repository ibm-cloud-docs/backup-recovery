---

copyright:
  years: 2025,
lastupdated: "2025-10-13"

keywords: reporting

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Management console reports
{: #reports_gmc}

{{site.data.keyword.baas_full}} provides one-stop-shop reporting from the Management Console for the {{site.data.keyword.baas_full_notm}}. You have an aggregated view of your IBM deployment regardless of the use case, workload, or deployment type (on-premises, consumed as an IBM-hosted service, or a combination).

The built-in reports are designed to address your most common use cases and are available for immediate use. You can view an overall summary of your data protection jobs and storage systems, or you can analyze data at the granular level by using powerful filtering options. You can filter, view, and download reports.

If you log in to the Management Console by using SSO and your user account is not available on the Access Management page, then you cannot schedule reports.
{: note}

## Viewing reports
{: #reporting_viewing_gmc}

View reports in the Management Console to analyze and improve user experience.

Procedure

Here is how you see the reports for all the instances for the selected region:

1. In the left navigation pane, click **Instance manager**, and select the region you want to see the report for.
2. The Management Console shows the report for all the instances for the selected region.
2. In the left navigation pane, click **Reports**.



## Choosing a report type
{: #reporting_report_type_gmc}

Choose a report type to identify the information that you need.

Currently, five built-in reports are available from the Management Console for {{site.data.keyword.baas_full}}.


[Failures](/docs/backup-recovery?topic=backup-recovery-report_failures_gmc) - The Failures report provides a summary and list of objects that had one or more backup run failures. It also helps you identify consecutive failures in the last three backups, and breaks down the failed objects by object type.

[Protected objects](/docs/backup-recovery?topic=backup-recovery-report_protected_objects_gmc) - The Protected Objects report provides a summary and list of all protected objects that had a backup run. You can view the backup status and the objects with an active snapshot.

[Protected or unprotected objects](/docs/backup-recovery?topic=backup-recovery-report_protected_unprotected_objects_gmc) - The Protected / Unprotected Objects report provides a summary and list of objects along with their protection status.



[Protection Runs](/docs/backup-recovery?topic=backup-recovery-report_protection_runs_gmc) - The Protection Runs report provides a summary and list of all backup activities per object per run. You can view the summary and success rate of protection runs. You can also view the snapshot status of the protection run.

[Recovery](/docs/backup-recovery?topic=backup-recovery-report_recovery_gmc) - The Recovery report provides a summary and list of all the clone and recovery tasks that were executed. It also provides other details such as the time taken for the operation and status of the operation.

You can complete the following tasks for each of the report type:

- [Filtering report data](#reporting_filter_data_gmc)
- [Download Reports](#reporting_downloading_gmc)
- [Reset to Default View](#reporting_resetting_default_view_gmc)

The report that you download inherits the filters that you applied.



## Filtering report data
{: #reporting_filter_data_gmc}

Reporting from the Management Console for {{site.data.keyword.baas_full_notm}} provides a comprehensive view of the data under Management console.
Each report type helps you view, visualize, and analyze data. The following table describes the key features of the Management Console reports:

| Name | Description |
|-------------------------|--------------------------------------------------------------|
| Filters |	Each report provides various filters that help you pare down the report until it shows only the data that you want in the report. The filter options change depending on the type of report. For more information, see [Filtering Report Data](#reporting_filter_data_gmc) |
| Summary	    | The summary provides a summary of the report for the time period that you specify in the filter. |
| Charts |	Each report includes charts that provide a graphical representation of data. |
| Data table |	The Data table in the report provides deeper insights to help you analyze the data. You can customize the columns in the table. For more information, see [Customize Table Columns](#reporting_customize_table_columns_gmc). |


You have full control over the data that you choose to include and view in your reports. Use the filters to pare down your report until it shows only the data that you want in the report. The filter options change depending on the type of report.

## Customizing table columns
{: #reporting_customize_table_columns_gmc}

Each report type from the Management Console for {{site.data.keyword.baas_full_notm}} provides comprehensive data. In each report type, data is displayed in a tabular format. You can add and remove columns from the Data table. The changes that you make to columns in a table persist until you change them again or until you restore the report to the default view.

Procedure

Here is how you customize the table columns for a report type:

1. On the **Reports** page, click the report type for which you want to customize the data.
2. From the report data table, click the gear icon.
   1. To add a column, enable the toggle.
   2. To remove a column, disable the toggle.

## Downloading reports
{: #reporting_downloading_gmc}

Download reports in different file formats from the Management Console reports page.

On any report type, click the Download icon and select one of the following file formats: PDF, Excel, or CSV.
The report in the selected file format gets downloaded to your system.

The time taken to generate a report depends on multiple factors such as the number of clusters selected, other filters applied on the report, and the amount of data. If the report is large, it might take a few moments to download the report.
{: note}



## Resetting reports to the default view
{: #reporting_resetting_default_view_gmc}

After you filter a report or customize report data table columns, you can reset the report to the default view.

To switch to the default from the Management Console for {{site.data.keyword.baas_full_notm}} reports page view, click Restore to default display icon. The page refreshes and reverts to the default view.

## Reporting APIs
{: #reporting_apis_gmc}

The Management Console for {{site.data.keyword.baas_full_notm}} architecture is API driven. You can programmatically interface with the Data Management Reporting service.

For more information about using the Management Console for {{site.data.keyword.baas_full_notm}} Reporting APIs, click the Help Center icon A question mark icon from the menu and select Get REST APIs.
