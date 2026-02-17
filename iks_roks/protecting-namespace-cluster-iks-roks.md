---

copyright:
  years: 2025
lastupdated: "2026-02-17"

keywords: data source connector, iks, roks, cluster

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protecting a namespace or cluster and scheduling a backup
{: #protecting-namespace-iks-roks}

{{site.data.keyword.baas_full}} protects Kubernetes namespaces and PVCs by assigning them to a policy‑driven Protection Group. This allows you to configure options like CSI snapshots, Auto Protect, label-based selection, and application quiescing to help ensure consistent backups for stateful workloads.

## Prerequisites for scheduling backups
{: #data-source-connector-iks-roks-prereq-schedule-bak}

1. You must have a [Protection Policy](/docs/backup-recovery?topic=backup-recovery-baas-policy-creation) in place to create or run a backup. You can either:
   - Use an existing protection policy, or
   - Create a new user-defined policy.
2. You need to create a [Protection Group](/docs/backup-recovery?topic=backup-recovery-protection-groups) that uses one of these policies to perform an incremental or full backup.

## How to protect a namespace or cluster (Basic Flow)
{: #protecting-namespace-iks-roks-schedule-backup-basic}

Follow these steps to quickly protect your Kubernetes resources:

1. Log in to the [IBM Cloud Console](https://cloud.ibm.com/){: external}.
2. Go to **Navigation Menu** \> **Backup and Recovery**.
3. Select your {{site.data.keyword.baas_full_notm}} instance.
4. Click **Launch dashboard**.
5. Go to: **Dashboard** \> **Data Protection** \> **Sources**.
6. Locate your Kubernetes source cluster by using the cluster endpoint.
7. Click the cluster endpoint to view available objects.
8. After selecting the cluster, a list of namespaces appears.
9. Choose the namespaces you want to protect (or select the entire cluster).
10. Click **Protect**.
   
   IBM Backup and Recovery service supports backup of:
   - Persistent Volume Claims (PVCs) (using CSI Volume Snapshots)
   - Kubernetes cluster resources
   - Namespace metadata
   {: note}

11. **Select or Create a Protection Group**: When prompted, choose one of the following options:

   | Option | Description |
   |------|-------------|
   | **Create a New Protection Group** | When creating a new group, configure the following settings (configurable **only during creation**): <ul><li>**Protection Group Name**</li><li>**Protection Policy**</li><li>**Start Time and Time Zone**</li><li>**Leverage CSI Snapshot** (toggle)</li><li>**Pause Future Runs** (optional)</li><li>**Alerts and Email Recipients**</li><li>**Priority** (High / Medium / Low)</li><li>**Include or Exclude Labels**</li><li>**Description**</li></ul> |
   | **Use an Existing Protection Group** | All settings are prefilled from the existing group and are read-only at this stage. |

12. **Select or Create a Protection Policy**:
   - **Create New**: Define backup frequency, retention, and other policy settings.
   - **Use Existing**: Select a policy, click **Edit** to update Kubernetes-specific settings if needed, and save.

13. **Assign Policy**: Assign the selected Protection Policy to the Kubernetes object. Changes take effect during the next scheduled run.

14. **Start Protection**: Click **Protect** to initiate protection. The service begins backing up selected objects according to the schedule. monitor progress at: **Activity** \> **Protection**.

## Advanced Configuration Options
{: #protecting-namespace-iks-roks-advanced}

Once you have set up basic protection, you can customize the configuration to better suit your needs.

### 1. Configure CSI Snapshot Protection
{: #configure-csi-snapshot}

- Enable **Leverage CSI Snapshot** (toggle).
- When enabled:
   - PVCs are protected using CSI driver snapshots.
   - Snapshots are crash-consistent, capturing the volume state at the moment of the snapshot.

### 2. Configure Additional Protection Settings
{: #configure-additional-settings}

When creating a new protection group, you can configure these additional settings:

| Category | Description |
|--------|-------------|
| **Scheduling** | <ul><li>**Start Time**: Defines when the protection job runs.</li><li>**Time Zone**: Select the appropriate time zone.</li></ul> |
| **Run Controls** | <ul><li>**Pause Future Runs**: Prevents any future scheduled runs.</li><li>**End Date**: Stops snapshot capture after the selected date (current runs complete).</li></ul> |
| **Storage & Performance** | **QoS Policy**: <ul><li>Backup HDD (default)</li><li>Backup SSD</li><li>Backup Auto</li></ul> |
| **Alerts** | Configure alerts for: <ul><li>Success</li><li>Failure</li><li>SLA Violation</li></ul> Add **email recipients** if required. |
| **Priority** | Sets execution priority when system load is high: <ul><li>High</li><li>Medium (default)</li><li>Low</li></ul> |

### 3. Auto Protect
{: #auto-protect}

Auto Protect helps ensure automatic inclusion of new namespaces based on labels or cluster-level settings.

| Auto Protect Type | Steps |
|-----------------|-------|
| **Cluster‑Level Auto Protect** | Go to **Data Protection** > Click on **Protect** > From the dropdown, select **Kubernetes Cluster** > Search for and select your cluster (using the cluster endpoint) > Turn **ON** the **Auto Protect** toggle (at cluster level) > Click **Protect**. |

When Auto Protect is enabled:
- **New Namespaces**: Automatically added to the protection group if they match the criteria.
- **Deleted Namespaces**: Automatically excluded from future backups.
- **Existing Backups**: Preserved until retention expires.

### 4. Label-based Inclusion and Exclusion
{: #label-inclusion-exclusion}

You can fine-tune what gets backed up using labels:

- **Exclude Namespaces**:
  1. In the Protection view, select a label associated with a namespace.
  2. Click the exclude icon (tooltip: `Exclude Tag Off. Click to exclude this label`).
  3. Steps namespaces with this label are excluded from protection runs.

- **Customize Individual Namespaces**:
  1. Click the pencil icon next to a namespace to edit.
  2. **Inclusion/Exclusion of PVCs**: Filter PVCs based on labels or manually select them.
     - **Define rules to**:
       - Include PVCs with specific labels
       - Exclude PVCs with specific labels
     - **Label matching options**:
       - Match all labels
       - Match any selected label
  3. **Inclusion/Exclusion of Resources**: Include or exclude specific Kubernetes resources (Pods, Secrets, Services, etc.).

### 5. Application Quiescing
{: #application-quiescing}

{{site.data.keyword.baas_full_notm}} supports quiescing for consistent backups of stateful workloads.

**Supported Quiescing Modes:**

| Mode | Description |
|---|---|
| **Together mode** | Parallel execution within a single volume group. Faster than sequential. |
| **Independent mode** | Parallel execution across multiple volume groups. Fastest backup speed. |
| **Sequential mode** | Serial execution within a single volume group. Slowest but most controlled. |

**To configure quiescing:**
1. Select the target namespace and click **Edit**.
2. Navigate to the **Scripts** section.
3. Select the **Quiesce Mode**.
4. Define rules (Pod selector labels, Pre/Post-snapshot scripts).
5. Set failure behavior.
6. Click **Save**.

Scripts run inside containers and have configurable timeouts.
{: note}

## Managing Protection Lifecycle
{: #managing-protection-lifecycle}

For detailed instructions on managing your backups, see [Managing Protection Group Runs](/docs/backup-recovery?topic=backup-recovery-protection-group-run-now). This includes:

- **Run Now**: Trigger an immediate backup.
- **Pause/Resume**: Temporarily stop or restart scheduled backups.
- **Delete**: Remove protection for an object (namespace). Can be object only or object and snapshots.
