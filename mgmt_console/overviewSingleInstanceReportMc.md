---

copyright:
  years: 2025
lastupdated: "2025-11-03"

keywords: reporting, single instance, data protection, download report

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Individual instance report
{: #report-individual-instance}

{{site.data.keyword.baas_full}} provides one-stop-shop reporting from the Management Console for the {{site.data.keyword.baas_full_notm}}. You have an aggregated view of your IBM deployment regardless of the use case, workload, or deployment type (on-premises, consumed as an IBM-hosted service, or a combination).

The built-in reports are designed to address your most common use cases and are available for immediate use. You can view an instance level summary of your data protection jobs and Storage Systems, or you can analyze data at the granular level by using powerful filtering options. You can filter, view, and download reports for a single instance.

## Data protection
{: #report-data-protection-mc}

### Protection
{: #protection-mc}

Use the protection configuration window to create a [Protection Group](/docs/backup-recovery?topic=backup-recovery-protection-groups).

1. From the **Protect** drop-down list, select the type of **Databases** or **Physical Server** for which you want to create a protection group.
1. Choose the Objects (with Source), enter a Protection Group Name, configure a standard Protection Policy (Gold, Silver, or Bronze) or create a new custom Protection Policy.
2. Click **Protect** to finalize your configurations; the protection group is created.

### Recoveries
{: #recoveries-mc}

{{site.data.keyword.baas_full_notm}} provides the ability to recover files and folders from a Snapshot created earlier by a Protection Group. Files and folders can be recovered to their original location (limitations apply to physical Servers and specific operating systems) or to a newly specified location, which can be within the original source or a different one. For more information, see [Recover Physical Server Files or Folders](/docs/backup-recovery?topic=backup-recovery-Recover).

### Sources
{: #register-source-mc}

Register and view sources that you'd like to back up.
1. First [download and install](/docs/backup-recovery?topic=backup-recovery-agent-download-install) the agent on the source.
2. [Register the source](/docs/backup-recovery?topic=backup-recovery-source-registration-tutorial#baas-config-protect-job).

### Policies
{: #policy-mc}

Protection policies are a collection of reusable settings that define when and how sources, such as physical hosts, VMs, and databases are protected. For more information, see [Policy Creation](/docs/backup-recovery?topic=backup-recovery-baas-policy-creation).

## Data source connections
{: #report-data-source-connection-mc}

Create data source connections to establish connectivity between your sources and this data source. A connection consists of one or more virtual servers that move data between your data sources and IBM Cloud Backup. For more information, see [Deploy data source connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector).

## Health
{: #health-mc}

From the Health dashboard, you can analyze the health of the system. You can configure webhooks, configure alert notification settings, or view the alert resolution summary.
Each alert has a severity rating that indicates the seriousness of the problem:

- Critical – Immediate action is required because it detects a severe problem that might be imminent or major functionality is not working, such as a missing VM backup.
- Warning – Action is required, but the affected functionality is still working, such as the restore task failed due to some external target connectivity, some credentials issues or both.
- Healthy – No action is required.

For more information, see [Alerts](/docs/backup-recovery?group=alerts).

## Viewing reports
{: #report-view-mc}

View reports for a single instance to analyze and improve the user experience.

Procedure

Here is how you see the reports for a single instance for the selected region:

1. In the left navigation pane, click **Instance manager**, and select the region that you want to see the report for.
2. The Management Console shows the report for all the instances for the selected region.
3. From the **All Instances** drop-down list, select the instance for which you want to see the reports.
2. In the left navigation pane, click **Reports**.

## Choosing a report type
{: #choose-report-type-mc}

From the **Select Report** drop-down list, choose a report type to identify the information that you need. Currently, six built-in reports are available for a single instance for {{site.data.keyword.baas_full}}.

[Backup Summary](/docs/backup-recovery?topic=backup-recovery-report_protected_unprotected_objects_gmc) - This report shows an overview of the Protection Groups that are run on the data source Instance for the specified filter criteria. For each Protection Group, this report shows how many times the Protection Group has run, total bytes read and statistics about the last time the Protection Group ran.

[Failed Objects](/docs/backup-recovery?topic=backup-recovery-report_failures_gmc) - This report lists all the Objects for which an error occurred during the last runs and prevented the Objects from being backed up.

[Protected Objects Heatmap](/docs/backup-recovery?topic=backup-recovery-report_protected_objects_gmc) - **Need more information for this section**

[Protection Runs Summary](/docs/backup-recovery?topic=backup-recovery-report_protection_runs_gmc) - **Need more information for this section**

[Protection Summary by Object Type](/docs/backup-recovery?topic=backup-recovery-report_recovery_gmc) - This report shows the protection status of Objects as of the last runs that occurred in the specified date range.

[Unprotected/Protected VMs](/docs/backup-recovery?topic=backup-recovery-report_recovery_gmc) - This report provides lists of VMs protected and not protected by IBM Cloud Backup.

You can complete the following tasks for each of the report type:

- [Filtering report data](#report-filter-data-gmc)
- [Downloading reports](#report-download-gmc)

The report that you download inherits the filters that you applied.

## Filtering report data
{: #report-filter-data-mc}

Each report type helps you view, visualize, and analyze data. You have full control over the data that you choose to include and view in your reports. Use the filters to pare down your report until it shows only the data that you want in the report. The filter options change depending on the type of report.

## Downloading reports
{: #report-download-mc}

Download reports in CSV format from the single instance reports page.

- On any report type, click the **Download** icon and the report gets downloaded to your system.

The time taken to generate a report depends on multiple factors such as the number of clusters selected, other filters applied on the report, and the amount of data. If the report is large, it might take a few moments to download the report.
{: note}
