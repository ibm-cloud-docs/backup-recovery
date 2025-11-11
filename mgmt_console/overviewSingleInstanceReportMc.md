---

copyright:
  years: 2025
lastupdated: "2025-11-11"

keywords: reporting, single instance, data protection, download report

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# View status of service instances
{: #individual-instance-view}

The {{site.data.keyword.baas_full}} UI provides access to a list of available service instances, and detailed views from the backup policy management dashboard.



## Details of an instance
{: #instance-details}

1. You must be logged into your IBM cloud account and have an active {{site.data.keyword.baas_full}} instance.
2. View the details of an instance by either:
   - clicking on it to launch into the backup policy management dashboard, and clicking launch dashboard, or
   - click the overflow menu to the right of your instance, and select launch dashboard.
     - the overflow menu also includes the option to rename the instance, edit tags, and delete the instance
3. The dashboard displays a quick glance of getting started, health, protection, compliance, data source connections, and recoveries.

## Summary
{: #dashboard-summary}

| View | Description | Action |
|------------|-------------------------------------------------|-------------------------------------------|
| Getting started | - Connect Sources<br> - Create backup policies | - Setup a connection<br> - Create backup policies and protect your sources.
| [Health](#dashboard-summary-health) | Displays active alerts within the last 24 hours | Click the panel to open a new page that displays the Alerts, Alert Analytics, Notifications Settings and Resolution Summary |
| Protection | Percentage of objects protected |
| Compliance |  Last run status that met BCO and missed BCO |
| Data Source Connections and Connectors |


### Health
{: #dashboard-summary-health}

| View | Description | Action |
|------------|-------------------------------------------------|-------------------------------------------|
| Alerts | Displays the number of critical, warning and healthy alerts | Alerts can be filtered by Severity: Critical, Info, Warning<br>Active +1:  Active, Note, Resolved<br>Date Range: Past Hour, Past 12 Hours, Past 24 Hours, Past 7 Days, Past 30 Days, Custom(Start Date - End Date)
| Alert Analytics | Displays recent alerts and resolutions | - Filter by Open Alerts or Resolved Alerts<br>- Open Alerts by Category|
| Notification Settings | blank | Add Alert Notification Rule:<br>1. Create a rule name<br>2. When by Alert Category, Alert Severities, Alert Name<br>Select to send Alert Notification by means of a Webhook: Include URL (POST request will be sent on the URL using cURL command) and  (Add parameters to be sent with cURL command ) |
| Resolution Summary | Provides a summary of the resolution | |

### Protection
{: #dashboard-summary-protection}

Displays protection groups found by group type, groups, policy, BCO, status. Indicates number that succeeded, have a warning, failed, runnning, canceled, met BCO, missed BCO.

| View | Description | Action |
|------------|-------------------------------------------------|-------------------------------------------|
| Group Type | Select by Databases or Physical Servers | Databases: Select Oracle, Miscrosoft SQL, SAP HANA x86-x64<br>Physical Servers(file based) |
| Deleted | Deleted<br>Failover Ready<br>Local<br>Paused |  |
| Policy Type | Bronze<br>Gold<br>Silver | |
| BCO Type | Met BCO<br>Missed BCO | |
| Status | Running<br>Succeeded<br>Warning<br>Failed<br>Canceled<br>Canceling<br>Paused | Backup, Replication, Archical, CloudSpin<br>"  "<br>" "<br>" "<br>" "<br>" "<br>" "|

Protection status can be sorted by group, start time, duration, success/error, BCO.
{: note}

Add New protection from the drop-down for databases and physical server.
1. Click Protect and choose databases or physical server
2. Select MS SQL server, Oracle database or SAP HANA:
   - MS SQL server options: add objects, protection group, policy, backup type and more options to add a registered source to protect
   - Oracle database options: add objects, protection group, policy and more options to add a registered source to protect
   - SAP/HANA options: add objects, protection group, policy and more options to add a registered source to protect
3. Select Physical server:
   - File-based: add objects, protection group, policy and more options to add a registered source to protect

#### New Protection
{: #dashboard-summary-new-protection}

1. Complete the required input for the fields in the table.
2. Click **Protect** to create a new protection.

| Protect | Type | Description | Action |
|---------------|----------------|-------------------------------|-----------------------------------------------|
|Add objects | Database: MS SQL Server, Oracle DB, SAP HANA<br>Physical Server: File-based | Register source | Search or click to add a registered source by [creating a Data Source connection](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector). Click to **Continue**.|
| Protection group | Database: MS SQL Server, Oracle DB, SAP HANA<br>Physical Server: File-based | Name the Protection Group | Add a name of your choice |
| Policy | Database: MS SQL Server, Oracle DB, SAP HANA<br>Physical Server: File-based |Add or create a policy | Bronze, Gold, Silver<br>Create Policy: Name the policy, Determine backup every minute, hour, day, week, month and determine data retention |
|Backup Type (VDI-based) | Database: MS SQL Server |
| More options |

## Data protection
{: #report-data-protection-mc}

### Protection
{: #protection-mc}

Use the protection configuration window to create a [Protection Group](/docs/backup-recovery?topic=backup-recovery-protection-groups).

1. From the **Protect** drop-down list, select the type of **Databases** or **Physical Server** for which you want to create a protection group.
1. Choose the Objects (with Source), enter a Protection Group Name, configure a standard Protection Policy (Gold, Silver, or Bronze) or create a new custom Protection Policy.
2. Click **Protect** to finalize your configurations; the protection group is created.

### Recover
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
2. The Global Management Console shows the report for all the instances for the selected region.
3. From the **All Instances** drop-down list, select the instance for which you want to see the reports.
2. In the left navigation pane, click **Reports**.

## Choosing a report type
{: #choose-report-type-mc}

From the **Select Report** drop-down list, choose a report type to identify the information that you need. Currently, six built-in reports are available for a single instance for {{site.data.keyword.baas_full}}.

[Backup Summary](/docs-draft/backup-recovery?topic=backup-recovery-report_backup_summary_mc) - This report shows an overview of the Protection Groups that are run on the data source Instance for the specified filter criteria. For each Protection Group, this report shows how many times the Protection Group has run, total bytes read and statistics about the last time the Protection Group ran.

[Failed Objects](/docs-draft/backup-recovery?topic=backup-recovery-report_failures_gmc) - This report lists all the Objects for which an error occurred during the last runs and prevented the Objects from being backed up.

[Protected Objects Heatmap](/docs-draft/backup-recovery?topic=backup-recovery-report_protected_objects_mc) - **Need more information for this section**

[Protection Runs Summary](/docs/backup-recovery?topic=backup-recovery-report_protection_runs_mc) - **Need more information for this section**

[Protection Summary by Object Type](/docs/backup-recovery?topic=backup-recovery-report_recovery_mc) - This report shows the protection status of Objects as of the last runs that occurred in the specified date range.

[Unprotected/Protected VMs](/docs/backup-recovery?topic=backup-recovery-report_recovery_mc) - This report provides lists of VMs protected and not protected by IBM Cloud Backup.

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
