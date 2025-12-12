---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Configuring webhooks
{: #configuring_webhooks}

Webhooks are HTTP callbacks which are usually triggered by some event. Webhooks are configured on one website and when an event occurs on this website, an HTTP request is made to the configured URL which invokes action on the other website.

You can enable webhooks for alerts by creating an alert notification rule. When the alert is triggered and meets the criteria in the rule, the cluster sends an HTTP request to the specified website. Your application can then interpret the request. For example, the webhook might notify the website about a critical disk alert and your application might open a trouble ticket to track the problem.

To create an alert notification rule and enable a webhook:

1. Navigate to **System > Health** and select **Alerts** tab.

    The **Alerts** summary page is displayed.

2. Select **Settings** tab, and click **Add Alert Notification Rule**.
3. Configure the following details:


    | Parameter | Description |
    | --- | --- |
    | Rule Name | Alert rule name. |
    | Alert Category/Severity/Name | Choose the alert category/severity and name from the list. You can specify one or multiple values. |
    | Send Alert Notification Via Email | The configured email addresses will receive alert email notifications. |
    | Send Alert Notification via Webhook | The URL to which the system will connect when the above alert notification rule is matched. That is the alerts will be notified to the configured URL. |

4. Toggle on **Webhook** and provide the URL and cURL options.

    Ensure that the Webhook configuration follows the [rfc3986](https://www.rfc-editor.org/rfc/rfc3986#page-12){: external} standards.

    [![](../../Resources/Images/webhookstoggle_thumb_400_0.png "Click to expand")](../../Resources/Images/webhookstoggle.png)

5. Click **Save**.

## Alert request
{: #alert_request}

When an alert is triggered, a sample payload as shown below will be available at the configured URL:

CURL Request:

Curl -XPOST https://test-service-now.com/api/x\_hesin\_cohesity\_c/cohesitywebhook

The Payload sent to the above URL:

{
	"alertType": "1060",
	"alertId": "437441:1665640208440874",
	"alertName": "DnsServerUnReachable",
	"alertDescription": "DNS server is not reachable.",
	"alertSeverity": "WARNING",
	"alertCategory": "Networking",
	"alertCause": "DNS servers \[10.2.0.1\] unreachable. Please update DNS Server.",
	"alertHelpText": "Check and provide alternate DNS server.",
	"languageCode": "en-us",
	"alertUrl": "https://cluster-ip/health/alert-info/437441:1665640208440874",
	"clusterName": "cohesity", 
	"clusterId": "4398906079065976", 
	"alertCode": "CE03601060", 
	"alertProperties": 
	{ 
		"cluster\_id": "4398906079065976", 
		"alert\_message": "DNS servers \[10.2.0.1\] unreachable. Please update DNS Server." 
	},
	"firstOccurrenceUsecs": "1665640208440874",
	"lastOccurrenceUsecs": "1665640208440874"
}			

_prod-cluster.eng.cohesity.com_ refers to the IP address of the cluster.

## Customized template for url request and data payload
{: #customized_template_for_url_request_and_data_payload}

Webhook also supports template options for both URL requests and data payload.

Data payload can be sent in any customized format as a template with placeholders. These placeholders are replaced with the actual values of the generated alert.

The placeholders can be any of the following:

 $alertType ,  $alertId ,  $alertName ,  $alertDescription ,  $alertSeverity ,  $alertCategory ,  $alertCause ,  $alertHelpText ,  $languageCode ,  $alertUrl ,  $clusterName ,  $clusterId ,  $alertCode ,  $alertProperties ,  $firstOccurrenceUsecs ,  $lastOccurranceUsecs

The following is an example of a template for a Slack channel:

\--template {"blocks": \[{ "type": "section", "fields": \[{"type": "plain\_text", "text": "alert name" }, {"type": "mrkdwn", "text": "\*$alertName\*"}, {"type": "plain\_text", "text": "alertSeverity"}, {"type": "mrkdwn", "text": "\*$alertSeverity\*"}, {"type": "plain\_text", "text": "alertCategory"}, {"type": "mrkdwn", "text": "\*$alertCategory\*"}, {"type": "plain\_text", "text": "alertDescription" }, {"type": "mrkdwn", "text": "\*$alertDescription\*" }, {"type": "plain\_text", "text": "alertCause" }, {"type": "mrkdwn", "text":"\*$alertCause\*"}\]}\]}

Webhook URL path can also be customized with placeholders.

**Example**:

`"http://example.webhooksite.com?alertName=$alertName"`, where `alertName=$alertName` is the placeholder.

**\[content type\]**

If the content-type is not specified, by default, it is application/json. If a content-type is specified it must be acceptable by the webhook server.

**Example**:

\-H content-type:text/html

**\[curl options\]**

For security reasons, valid webhook options can only be `-H( or --header)`, `-d(or --data)`and `--template`. If `--template` is provided then `-d(or --data)` is ignored. If `--template` is not provided then the payload in `--data` is merged with the default alert JSON content. Ensure that you provide a valid JSON entry for the `-d(or --data)` option.

**Example**:

\-d {"accountId": 12345, "accountName": "test" }

## Resolving alerts
{: #resolving_alerts}

When an alert is triggered at the {{site.data.keyword.baas_full_notm}} side, the above data as shown in the sample payload must be notified to the configured URL. Using the above payload, the alerts can be resolved at the target based on the **alertID**. This eliminates the need to switch back to the {{site.data.keyword.baas_full_notm}} to manage and resolve alerts.

To resolve alerts, the alertURL from the [sample payload](#Alert) must be extracted and used in the API request as shown below:

URL Request:

https://prod-cluster.eng.cohesity.com/irisservices/api/v1/public/alertResolutions

Method : POST

Payload :

{"resolutionDetails":{"resolutionSummary":"Resolved at 11/1, high priority"},"alertIdList":\["157373326:1577990357283489"\]}

_alertIdList_ refers to the alertID in the payload. _Resolved at 11/1, high priority_ is a user defined string.

