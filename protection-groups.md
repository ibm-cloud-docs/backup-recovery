---

copyright:
  years: 2024, 2025

lastupdated: "2025-07-30"

keywords: protection, groups

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Protection groups
{: #protection-groups}

This tutorial shows how to backup Microsoft SQL Server (VDI-based) data using your {{site.data.keyword.baas_full}} service. This entails choosing the Sources and Objects (i.e. Microsoft SQL Server hosts and their data) to protect, along with naming the Protection Group and assigning a pre-existing or custom Protection Policy that defines your backup schedule, data retention duration and more.

## Before you start
{: #protection-groups-before-you-start}

Ensure that you have what you need to start:

- An account for the [{{site.data.keyword.cloud}} Platform](https://cloud.ibm.com).
- An instance of {{site.data.keyword.baas_full_notm}} service is deployed.
- A [Data Source Connector](/docs/allowlist/backup-recovery?topic=backup-recovery-deploy-data-source-connector) (VM or VSI) that is registered with your instance.
- A [Source](/docs/allowlist/backup-recovery?topic=backup-recovery-baas-registration) (physical host, VM or VSI) that is registered with your instance.
- An [Agent](/docs/allowlist/backup-recovery?topic=backup-recovery-agent-download-install) deployed to the source (installed using the required installer).
- Verify that you have proper access to your {{site.data.keyword.baas_full_notm}} instance.
  - Writer (backup-recovery.dashboard.edit) or Manager (backup-recovery.dashboard.edit) privileges to the {{site.data.keyword.baas_full_notm}} Service are needed to create and manage Protection Jobs (i.e. Backup Jobs) in your instance. These privileges can be assigned by your {{site.data.keyword.cloud_notm}} Platform account owner using an Access Group (multiple users) or Access Policy (specific user) tied to your IAM profile. Details on this are included [here](/docs/allowlist/backup-recovery?topic=backup-recovery-iam-docs-template&interface=ui).

## Launch your {{site.data.keyword.baas_full_notm}} Instance UI
{: #protection-groups-instance-ui}

1. Log into the {{site.data.keyword.cloud_notm}} Platform.
2. Expand the Navigation Menu (four horizontal lines, top left) > select Resource List.
3. Expand Storage > select your {{site.data.keyword.baas_full_notm}} instance.
4. Select Launch dashboard > enter your SSO / account credentials to authenticate.

After completing the steps, you are now presented with the UI of your {{site.data.keyword.baas_full}} instance.

## Add or Edit a Protection Group
{: #protection-groups-add-edit}

Please refer to [Add or edit a protection group for physical servers](/docs/allowlist/backup-recovery?topic=backup-recovery-add_or_edit_a_protection_group_for_physical_servers) to accomplish this task.

A Protection Group is created by a user when using the Protection configuration window that appears when selecting Data Protect > Protect (for Physical Server and Databases such as Microsoft SQL Server). Once you choose the Objects (with Source), enter a Protection Group Name, configure a standard Protection Policy (Gold, Silver, etc.) or create a new custom Protection Policy, then select Protect to finalize your configurations, the Protection Group will be created. More details on these configuration steps are available in the Tutorials on protecting (i.e. backing up) Physical and Microsoft SQL Server data.
{: note}
