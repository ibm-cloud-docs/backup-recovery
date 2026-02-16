---

copyright:
  years: 2025

lastupdated: "2026-02-16"

keywords: policy

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Policy Creation
{: #baas-policy-creation}

This section explains how to create Protection Policies on your {{site.data.keyword.baas_full}} instance.

A **Protection Policy** is a reusable set of rules that defines:
*   **When** to back up your data (schedule).
*   **How long** to keep the backups (retention).
*   **Advanced settings** like retries and DataLock.

Policies can be applied to various sources, including physical hosts, VMs, databases, and Kubernetes clusters.

### DataLock Feature
DataLock is a security and compliance feature in IBM Cloud® Backup and Recovery that enforces WORM (Write Once, Read Many) protection for backup data.

When DataLock is enabled, protected snapshots and archives cannot be modified or deleted until the configured lock period expires. This helps meet regulatory requirements and protects backup data from accidental or malicious deletion.

## Before you start
{: #baas-policy-creation-before-you-start}

Ensure you have the following:
1.  An active [IBM Cloud Platform account](https://cloud.ibm.com/){: external}.
2.  A deployed and accessible {{site.data.keyword.baas_full_notm}} service instance.
3.  Appropriate service access privileges (**Writer** or **Manager**) to create or manage policies.
    *   *Note: Access can be assigned by your account administrator via IAM.*

## Accessing the {{site.data.keyword.baas_full_notm}} dashboard
{: #baas-access-dashboard}

To manage policies, you must first access the {{site.data.keyword.baas_full_notm}} dashboard:

1.  Log in to the [IBM Cloud Console](https://cloud.ibm.com/){: external}.
2.  Go to **Navigation Menu** \> **Backup and Recovery**.
3.  Select your {{site.data.keyword.baas_full_notm}} instance.
4.  Click **Launch dashboard**.

## Creating and managing protection policies
{: #baas-policy-creation-config-protect-job}

### Create a protection policy
{: #baas-policy-creation}

1.  In the dashboard, go to **Data Protection** \> **Policies**.
2.  Click **Create Policy**.
3.  Enter a **Policy Name**.
4.  Configure the **Backup Schedule** (frequency and timing).
5.  Set the **Retention Period** for your snapshots.
6.  Configure the **[DataLock](/docs/backup-recovery?topic=backup-recovery-datalock)**.
7.  Click **Create**.

Your new policy is now ready to be assigned to Protection Groups.

### Detailed management tasks
{: #policy-detail-task}

For more specific configurations, refer to these guides:

*   **[Creating and configuring protection policies](/docs/backup-recovery?topic=backup-recovery-create_or_edit_a_standard_policy)**: In-depth steps for schedules and retention.
*   **[Manage policies](/docs/backup-recovery?topic=backup-recovery-manage_policies)**: View, update, disable, or delete policies.
*   **[DataLock](/docs/backup-recovery?topic=backup-recovery-datalock)**: Enable or manage DataLock settings for compliance protection.
*   **[Extended retention](/docs/backup-recovery?topic=backup-recovery-extended_retention)**: Set up long-term archival rules.

## Next steps
{: #bass-policy-creation-next-steps}

Now that you have a policy, go to **[Protection Groups](/docs/backup-recovery?topic=backup-recovery-protection-groups)** to apply it to your workloads and start protecting your data.
