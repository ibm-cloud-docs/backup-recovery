---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Creating reports
{: #reports}

[Helios Reporting](https://docs.cohesity.com/WebHelios/Content/Helios/HeliosReports.htm){: external} supports global reporting and has an enhanced library of built-in reports. You can find equivalent reports in the new and improved [Helios Reporting](https://docs.cohesity.com/WebHelios/Content/Helios/HeliosReports.htm){: external}. Please create new schedules with the equivalent reports in [Helios Reporting](https://docs.cohesity.com/WebHelios/Content/Helios/HeliosReports.htm){: external} to start using these new features.

In today’s data-driven business environment, it is more important than ever to analyze your data and identify patterns and trends that help organizations make better decisions. With reporting, {{site.data.keyword.baas_full_notm}} extends its built-in reporting features to give you the flexibility to produce several reports to understand cluster capacity, usage metrics and help you address your organization’s specific needs most effectively.

For information about how long the cluster keeps audit data, see [Data Retention for Audits](../../../Concepts/AuditRetention.htm).

## Run reports
{: #run_reports}

1. Navigate to **Reporting** and click **Proceed to cluster Reporting**.
2. Select a report from the **Select Report** drop-down.
3. If the **Filters** panel is provided for the report, you can optionally filter your results by the options listed. Select the options and click **Apply Filters**.

## Email reports
{: #email_reports}

You can email most reports as needed or on a recurring basis.

To email reports, an email address must be specified for the local System Admin account (admin) of the {{site.data.keyword.baas_full_notm}}. Select **Settings > Access Management** and if necessary, edit the local admin user to provide an email address.

1. Navigate to **Reporting** and click **Proceed to cluster Reporting**.
2. Select a report from the **Select Report** drop-down.
3. If the **Filters** panel is provided for the report, you can optionally filter your results by the options listed. Select the options and click **Apply Filters**.
4. Click the **Email** icon (![](../../../Resources/Images/i/icn/clock-h.svg)) and type one or more email addresses in the **Email Recipients** field. (Not all reports can be emailed.)
5. Select a format for the report in the **Format** drop-down. A CSV format report is sent as an email attachment. An HTML format report is sent in the body of the email.
6. To email the report one time, toggle **Schedule Recurring Email** off.

    To send recurring emails, select one or more days in **Email Every** and set the time the emails should be sent.

7. Optionally, select the **Time Zone** that matches your mailing schedule.

8. The current report filters are displayed. You can use them as is or modify them.
9. Click **Save Schedule**.

If you scheduled emails, click the **Scheduled Emails** tab to display all scheduled emails.

If you did not provide schedule information, the report is emailed immediately.

## Delete a scheduled email
{: #delete_a_scheduled_email}

1. Navigate to **Reporting** and click **Proceed to cluster Reporting**.

2. Cick the **Scheduled Emails** tab.

    This tab lists the scheduled emails for all reports.

3. To edit a scheduled email, click the edit icon (![](../../../Resources/Images/i/icn/edit-h.svg)) and edit the recipients, format, filters and scheduling information and then click **Save Schedule**.
4. To permanently delete a scheduled email, click its delete icon (![](../../../Resources/Images/i/icn/delete-h.svg)).

## Download a report as a csv file
{: #download_a_report_as_a_csv_file}

For most reports, you can download the data generated for the currently displayed report as described below. (You can also schedule an email that includes the report as a CSV attachment as described previously in this topic.)

1. Navigate to **Reporting** and click **Proceed to cluster Reporting**.
2. Select a report from the **Select Report** drop-down.
3. If the **Filters** panel is provided for the report, you can optionally filter your results by the options listed. Select the options and click **Apply Filters**.
4. Click the **CSV** icon (![](../../../Resources/Images/i/icn/download-h.svg)). The file is downloaded immediately to your device.
