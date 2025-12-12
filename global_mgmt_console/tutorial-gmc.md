---

copyright:
  years: 2025
lastupdated: "2025-12-12"

keywords: global, management, console, tutorial
subcollection: backup-recovery
content-type: tutorial
services: backup-recovery
account-plan: paid
completion-time: 15m

---

{{site.data.keyword.attribute-definition-list}}

# Backup and Recovery Manager
{: #baas-gmc-tutorial}
{: toc-content-type="tutorial"}
{: toc-completion-time="15m"}

This tutorial shows how to access the Backup and Recovery Manager for the {{site.data.keyword.baas_full}} service.

## Before you start

{: #baas-gmc-before-you-start}

Ensure that you have what you need to start:

- An account for the [{{site.data.keyword.cloud}} Platform](https://cloud.ibm.com).

- An [instance](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery#baas-provision-instance) of {{site.data.keyword.baas_full_notm}} service is deployed.

Maintain access to the Backup and Recovery Manager to see the reports and alerts that are discussed in this tutorial.
{: note}

## Access Backup and Recovery Manager for the {{site.data.keyword.baas_full_notm}} service
{: #baas-gmc-instance-ui}
{: step}

1. Log in to the {{site.data.keyword.cloud_notm}} Platform.

2. Expand the Navigation menu (four horizontal lines, upper left) > select Resource List.

3. Expand Storage as an example, and select your {{site.data.keyword.baas_full_notm}} instance.

4. In the left navigation window, click **Instance manager**, and select the region that you want to see the details.

You are now presented with the UI of your {{site.data.keyword.baas_full_notm}} instance for that region. The next section covers how to back view your reports.

### Dashboard
{: #dashboard_viewing_tutorial_gmc}

The dashboard for the Backup and Recovery Manager includes a visual overview of the health, recovery status by type, protection status by type, and alerts for all instances. Certain areas within the dashboard are interactive where you can click with your mouse to see more information:

- health status
- regions map
- objects that are protected and unprotected
- protection status by type for protected and unprotected
- alert types

## Viewing reports
{: #reporting_viewing_tutorial_gmc}
{: step}

View reports in the [Backup and Recovery Manager](/docs/backup-recovery?topic=backup-recovery-reports_gmc) to analyze and improve your work experience with this feature.

Here is how you see the reports for all the instances for the selected region:

1. In the left navigation window, click **Reports**. The Backup and Recovery Manager shows the report for all the instances for the selected region.

   You need to stay in the Backup and Recovery Manager to see the reports for all instances where **All instances** is selected in the Backup and Recovery Manager UI.
   {: note}

2. Click the type of report that you want to see the details: [Failures](/docs/backup-recovery?topic=backup-recovery-report_failures_gmc), [Protected Objects](/docs/backup-recovery?topic=backup-recovery-report_protected_objects_gmc), [Protected or Unprotected Objects](/docs/backup-recovery?topic=backup-recovery-report_protected_unprotected_objects_gmc), [Protection Runs](/docs/backup-recovery?topic=backup-recovery-report_protection_runs_gmc) and [Recovery](/docs/backup-recovery?topic=backup-recovery-report_recovery_gmc).
3. Select the report [filters](/docs/backup-recovery?topic=backup-recovery-reports_gmc#reporting_filter_data_gmc). Time range filter is the only filter with a default value of 24 Hours.

4. Select an export format (PDF, Excel, CSV) from the download icon to download the report

See the section on available [reports and filters](/docs/backup-recovery?group=list-of-reports) for more information.
{: note}

## Accessing alerts
{: #accessing_alerts_tutorial_gmc}
{: step}

1. Click System from the Backup and Recovery Manager
2. Click Health
3. Select the Alerts filters
   - Filter Alert Data
   - The Alerts page supports multiple filters to pare down the data that you want to view
4. Click Export to CSV to download the alerts.

## Next steps

{: #bass-gmc-next-steps}

See the section on [Backup service instances](/docs/backup-recovery?topic=backup-recovery-individual-instance-view) for details on how to access reports and alerts for a selected instance of Backup and Recovery.
