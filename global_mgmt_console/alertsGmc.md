---

copyright:
  years: 2025,
lastupdated: "2025-12-12"

keywords: alerts, severity

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Alerts
{: #alerts_gmc}


IBM Cloud Backup and Recovery Manager dashboard displays all the alerts that are triggered on the clusters that are managed on {{site.data.keyword.baas_full_notm}}. The cluster creates an alert for various reasons:

- It finds a potential problem or when it exceeds the defined threshold.
- A Protection Group is configured to trigger an alert indicating the success or failure of the job.

Each alert has a severity rating that indicates the seriousness of the problem:

- Critical – Immediate action is required because it detects a severe problem that might be imminent or major functionality is not working, such as a missing VM backup.
- Warning – Action is required, but the affected functionality is still working, such as the restore task failed due to some external target connectivity, some credentials issues or both.
- Informational – Immediate action is not required, and the alert provides an informational message.


## Resolving alerts
{: #alerts_resolving_gmc}

In case you are aware of the problem and confirm that the issue has been resolved, or if the issue does not require further attention from the Alerts tab, you can manually resolve those alert(s). You can either create a new resolution of the alert(s) or associate the alert(s) with an existing resolution.

- Create new resolution
- Associate with existing resolution



Once you have reviewed the alert, you can resolve the alert using the page's Resolution section. You can create a new alert resolution or attach an existing one in the Resolution section.

### Create new resolution
{: #alerts_resolving_create_new_gmc}

Procedure

To create a new resolution, complete the following steps:

1. In the Alerts tab, select an alert or multiple alerts you plan to resolve and click Resolve Selected Alerts.
2. In the Resolution dialog, do the following:
   1. Select Create new resolution.
   2. In the Resolution Summary field, add a resolution summary for the alert.
   3. In the Resolution Description field, add a brief description of the resolution.
        Click Resolve.

Results
The resolution is added to the selected alerts, and the alert(s) status is marked as Resolved.


### Associate with existing resolution
{: #alerts_resolving_associate_existing_gmc}

Procedure

To associate the alert(s) with an existing resolution, complete the following steps:

1. In the Alerts tab, select an alert or multiple alerts you plan to resolve and click Resolve Selected Alerts.
2. In the Resolution dialog, do the following:
   1. Select Associate with existing resolution.
   2. From the Resolution Summary drop-down, you can search and select the resolution that you plan to attach to the alert.
   3. Click Resolve.

Results
The existing resolution is attached to the selected alerts, and the alert(s) status is marked as Resolved.



