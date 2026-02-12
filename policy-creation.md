---

copyright:
  years: 2024

lastupdated: "2026-02-12"

keywords: policy

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Policy Creation
{: #baas-policy-creation}

In this section, you will learn how to create Protection Policies on your {{site.data.keyword.baas_full}} instance. Protection Polices are a collection of reusable settings that define when and how sources (i.e. physical hosts, VMs, databases, etc.) are protected. This includes the scheduling of the protection job runs (i.e. backup jobs), how long the backup data (i.e. snapshots) are retained for, job retries and more.

## Before you start
{: #baas-policy-creation-before-you-start}

Ensure that the following prerequisites are met:
1. You have an active [IBM Cloud Platform account](https://cloud.ibm.com/){: external}.
2. An {{site.data.keyword.baas_full_notm}} service instance is deployed and accessible.
3. To create or manage Protection Policies, you must have one of the following privileges on the {{site.data.keyword.baas_full_notm}} service:
   - Writer (backup-recovery.dashboard.edit)
   - Manager (backup-recovery.dashboard.edit)

These privileges can be assigned by your IBM Cloud account administrator using an Access Group (for multiple users) or an Access Policy (for a specific user) linked to your IAM profile. For more information, see the [IAM access documentation](/docs/backup-recovery?topic=backup-recovery-iam&interface=ui).






## Accessing the {{site.data.keyword.baas_full_notm}} dashboard
{: #baas-access-dashboard}

1. Log in to the [IBM Cloud Platform account](https://cloud.ibm.com/){: external}.
2. Open the Navigation Menu (four horizontal lines in the upper left corner) and select Resource List.
3. Expand Storage, and select your {{site.data.keyword.baas_full_notm}} instance.
4. Enter your SSO or account credentials to authenticate.

After authentication, the {{site.data.keyword.baas_full_notm}} dashboard is displayed, where you can create and manage Protection Policies.




## Creating and managing protection policies
{: #baas-policy-creation-config-protect-job}

Protection Policies define how and when your data is backed up. A policy includes the backup schedule, retention settings, and optional advanced features such as DataLock, indexing, and pre/post scripts, and so on.

### Create a protection policy
{: #baas-policy-creation}

To create a new Protection Policy:
1. In the {{site.data.keyword.baas_full_notm}} dashboard, navigate to `Protection` \> `Policies`.
2. Click `Create Policy` and enter a Policy Name.
3. Configure the backup schedule and frequency.
4. Configure the retention period for backup snapshots.
5. (Optional) Configure advanced options such as [DataLock](/docs/backup-recovery?topic=backup-recovery-datalock), [indexing](/docs/backup-recovery?topic=backup-recovery-customize_indexing), or [pre/post scripts](/docs/backup-recovery?topic=backup-recovery-configure-pre-post-scripts).
6. Click Create to save the policy.

Once created, the policy can be applied to Protection Groups to control how your workloads are protected.

#### Detailed Policy Management Tasks
{: #policy-detail-task}

Use the links below for more detailed instructions on specific policy management tasks:

1. [Create or edit a standard policy](/docs/backup-recovery?topic=backup-recovery-create_or_edit_a_standard_policy) - Detailed steps to configure schedules, retention, and advanced settings.
2. [Manage policies](/docs/backup-recovery?topic=backup-recovery-manage_policies) - View, update, enable, disable, or delete existing policies.
3. [Datalock](/docs/backup-recovery?topic=backup-recovery-datalock) - Enable or manage DataLock settings for compliance protection.
4. [Extended retention](/docs/backup-recovery?topic=backup-recovery-extended_retention) - Configure long-term retention rules.

## Next steps
{: #bass-policy-creation-next-steps}

After creating a Protection Policy, proceed to [Protection Groups](/docs/backup-recovery?topic=backup-recovery-protection-groups) to assign the policy to your workloads and begin protecting your data.
