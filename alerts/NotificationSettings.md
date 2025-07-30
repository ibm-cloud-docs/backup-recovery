---

copyright:
  years: 2024
lastupdated: "2024-9-26"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Configure alert notification settings
{: #configure_alert_notification_settings}

9 September 2024

From the **Health** dashboard, you can configure general alert notification rules in the **Settings** tab. You can configure email, Syslog, SNMP, and webhook as the notification output for the alert notification.

In multi-tenant deployments, the rules for alert notifications work as follows:

*   If the notification rules are set up at the organization level, only alerts raised at that level will trigger notifications.
*   If the notification rules are set up at the cluster level, and alerts are raised at the organization level, all organizations' alerts will trigger notifications.

Additionally, notifications will be sent if the alert's category, severity, and name match the specified filters.

## Add alert notification rule
{: #add_alert_notification_rule}

You can add different alert notification rules that send emails, SNMP, Syslog, and/or cURL HTTP POST requests to a webhook URL based on the alert categories, severities, and names. Rules can be configured at both the service provider and the tenancy level.

### Prerequisites
{: #prerequisites}

Before you add an alert notification rule, make sure that you:

*   Configure an SMTP Server on the {{site.data.keyword.baas_full_notm}}, if you plan to send email notifications when an alert is triggered. For details on configuring the SMTP server, see [Configure Settings](../Admin/ConfigureSettings.htm).
*   Configure the SNMP setting on the {{site.data.keyword.baas_full_notm}}, if you plan to send SNMP notifications to your network management system when an alert is triggered. For details on configuring SNMP settings, see [Configure and Test SNMP](../Admin/SNMP.htm).
*   Add a Syslog server to the {{site.data.keyword.baas_full_notm}}, if you plan to send Syslog notifications to the server when an alert is triggered. For details on configuring the Syslog server, see [Add a Syslog Server](../../CLI/AddLogServerCLI.htm).

To add an alert notification rule:

1. Navigate to **System > Health** and click the **Settings** tab.
2. Click **Add Alert Notification Rule**.
3. In the **Add Alert Notification Rule** page, do the following:
    1. **Rule Name**: Type a descriptive name for the rule, for example, Cluster Disk Offline or Backup Job Failure.
    2. **Alert Category**: Optional. Select one or more categories from the drop-down. Otherwise, all alerts in any category will trigger the rule.
    3. **Alert Severities**: Optional. Select one or more severities from the drop-down. Otherwise, all alerts with any severity will trigger the rule.
    4. **Alert Name**: Optional. Select one or more names from the drop-down. Otherwise, any Alert name will trigger the rule. If you selected any categories, the list includes only alerts in those categories.
    5. **Email**: Optional. Click **Add** and from the drop-down menu, perform the following:
        1. Select **To** and type an email address or distribution list of the recipients to whom you plan to send the email notification.
        2. Select **CC** and type an email address or distribution list of the recipients to whom you plan to send a copy of the email notification.
    6. **SNMP**: Optional. Toggle on if you want the rule to send SNMP notifications when the alert is triggered.
    7. **Syslog**: Optional. Toggle on if you want the rule to send Syslog notifications when the alert is triggered.
    8. **Webhook**: Optional. Toggle on and provide the URL and cURL details if you want the rule to send a web callback to a server. Type the URL for the server and type any cURL options you want to use. When an alert is triggered, the {{site.data.keyword.baas_full_notm}} sends an HTTP request to the server. Your application can then interpret the request. For example, the webhook might notify the server about a critical disk alert, and your application might open a trouble ticket to track the problem. For more information, see [Configuring Webhooks](ConfiguringWebhooks.htm).
4. Click **Save**.

## Edit alert notification rule
{: #edit_alert_notification_rule}

You can edit an existing alert notification rule and update the following:

Change the rule name.

*   Select or discard alert categories from the **Alert Categories** list.
*   Select or discard alert severities from the **Alert Severities** list.
*   Select or discard alert names from the **Alert Name** list.
*   Add or remove email addresses
*   Enable or disable SNMP notification
*   Enable or disable Syslog notification
*   Enable or disable Webhook.

The updated alert notification rule is applied from the next alert that is triggered.

To edit an existing alert notification rule:

1. Navigate to **System** > **Health** and click the **Settings** tab.
2. Click the edit icon next to the rule.
3. On the **Edit Alert Notification Rule** page, update the settings as per your requirement.
4. Click **Save**.

## Delete alert notification rule
{: #delete_alert_notification_rule}

You can delete the alert notification rules that you do not want to use for alert notifications.

To delete alert notification rule:

1. Navigate to **System** > **Health** and click the **Settings** tab.
2. Click the delete icon next to the rule.
3. In the **Delete Alert Notification Rule** dialog box, click **Delete**.

