---

copyright:
  years: 2024

lastupdated: "2025-07-30"

keywords: policy

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Policy Creation
{: #baas-policy-creation}

In this section, you will learn how to create Protection Policies on your {{site.data.keyword.baas_full}} instance. Protection Polices are a collection of reusable settings that define when and how sources (i.e. physical hosts, VMs, databases, etc.) are protected. This includes the scheduling of the protection job runs (i.e. backup jobs), how long the backup data (i.e. snapshots) are retained for, job retries and more.

## Before you start
{: #baas-policy-creation-before-you-start}

Ensure that you have what you need to start:

- An account for the {{site.data.keyword.cloud_notm}} Platform
- An instance of {{site.data.keyword.baas_full_notm}} service is deployed
- Verify that you have proper access to your {{site.data.keyword.baas_full_notm}} instance. Writer (backup-recovery.dashboard.edit) or Manager (backup-recovery.dashboard.edit) privileges to the {{site.data.keyword.baas_full_notm}} Service are needed to create and manage Protection Jobs (i.e. Backup Jobs) in your instance. These privileges can be assigned by your {{site.data.keyword.cloud_notm}} Platform account owner using an Access Group (multiple users) or Access Policy (specific user) tied to your IAM profile. Details on this are included [here](/docs/backup-recovery?topic=backup-recovery-iam-docs-template&interface=ui).

## {{site.data.keyword.baas_full_notm}} Instance UI
{: #baas-policy-creation-instance}

1. Log into the {site.data.keyword.cloud_notm} Platform.
2. Expand the Navigation Menu (four horizontal lines, top left) > select Resource List.
3. Expand Storage > select your IBM Cloud Backup and Recovery instance.
4. Select Launch dashboard > enter your SSO / account credentials to authenticate.

You will now be presented with the UI of your {{site.data.keyword.baas_full_notm}} Instance. The next section will cover how to create your own Protection Policy.

## Create your own Protection Policy
{: #baas-policy-creation-config-protect-job}

1. [Indexing](/docs/backup-recovery?topic=backup-recovery-customize_indexing)
2. [Create or edit a standard policy](/docs/backup-recovery?topic=backup-recovery-create_or_edit_a_standard_policy)
3. [Manage policies](/docs/backup-recovery?topic=backup-recovery-manage_policies)
4. [Manage protection policy](/docs/backup-recovery?topic=backup-recovery-manage_protection_policy)
5. [Configure Pre & Post Scripts](/docs/backup-recovery?topic=backup-recovery-configure-pre-post-scripts)
6. [Datalock](/docs/backup-recovery?topic=backup-recovery-datalock)
7. [Extended retention](/docs/backup-recovery?topic=backup-recovery-extended_retention)

## Next steps
{: #bass-policy-creation-next-steps}

In the next section, learn about [Protection Groups](/docs/backup-recovery?topic=backup-recovery-protection-groups) that will use the schedules and settings of your newly created Protection Policy to determine when and how your backups are captured.
