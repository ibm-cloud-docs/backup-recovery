---

copyright:
  years: 2025
lastupdated: "2026-02-16"

keywords: data source connector, iks, roks, cluster

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Protecting a namespace or cluster and scheduling a backup
{: #protecting-namespace-iks-roks}

{{site.data.keyword.baas_full}} protects Kubernetes namespaces and PVCs by assigning them to a policy‑driven Protection Group with options like CSI snapshots, Auto Protect, label-based selection, policy reuse, and application quiescing to help ensure consistent backups for stateful workloads.

## Prerequisites for scheduling backups
{: #data-source-connector-iks-roks-prereq-schedule-bak}

1. You must have a [Protection Policy](/docs/backup-recovery?topic=backup-recovery-baas-policy-creation) in place to create or run a backup. You can either:
   - Use an existing protection policy, or
   - Create a new user-defined policy
2. You need to create a [Protection Group](/docs/backup-recovery?topic=backup-recovery-protection-groups) that uses one of these policies to perform an incremental or full backup.

## How to protect a namespace or cluster and schedule a backup:
{: #protecting-namespace-iks-roks-schedule-backup}

1. Log in to the {{site.data.keyword.baas_full_notm}}.
2. Go to: **Dashboard** \> **Data Protection** \> **Protection**.
3. Locate your Kubernetes source cluster by using the registration endpoint.
4. Click the cluster URL to view available objects.
5. After selecting the cluster, a list of namespaces appears.
6. Choose one of the following:
   - One or more namespaces (you can also exclude namespace based on labels)
   - Entire cluster
7. Click **Protect**.
   IBM Backup and Recovery service supports backup of:
   - Persistent Volume Claims (PVCs)
   - Kubernetes cluster resources
   - Namespace metadata

   PVC protection uses CSI Volume Snapshot capability when enabled.
   {: note}

8. Choose or Create a Protection Group. When prompted, select one of the following options.

   | Option | Description |
   |------|-------------|
   | **Create a New Protection Group** | When creating a new group, configure the following settings (configurable **only during creation**): <ul><li>**Protection Group Name**</li><li>**Protection Policy**</li><li>**Start Time and Time Zone**</li><li>**Leverage CSI Snapshot** (toggle)</li><li>**Pause Future Runs** (optional)</li><li>**Alerts and Email Recipients**</li><li>**Priority** (High / Medium / Low)</li><li>**Include or Exclude Labels**</li><li>**Description**</li></ul> |
   | **Use an Existing Protection Group** | All settings are **prefilled** from the existing group. Fields are **read-only** and cannot be modified at this stage. |

9. Configure the CSI Snapshot Protection.
   - Enable Leverage CSI Snapshot (toggle).
   - When enabled:
      - PVCs are protected by using CSI driver snapshots.
      - Snapshots are crash consistent, capturing the volume state at the snapshot moment.

10. Configure Additional Protection Settings.

    | Category | Description |
    |--------|-------------|
    | **Scheduling** | <ul><li>**Start Time**: Defines when the protection job runs.</li><li>**Time Zone**: Select the appropriate time zone.</li></ul> |
    | **Run Controls** | <ul><li>**Pause Future Runs**: Prevents any future scheduled runs.</li><li>**End Date**: Stops snapshot capture after the selected date (current runs complete).</li></ul> |
    | **Storage & Performance** | **QoS Policy**: <ul><li>Backup HDD (default)</li><li>Backup SSD</li><li>Backup Auto</li></ul> |
    | **Alerts** | Configure alerts for: <ul><li>Success</li><li>Failure</li><li>SLA Violation</li></ul> Add **email recipients** if required. |
    | **Priority** | Sets execution priority when system load is high: <ul><li>High</li><li>Medium (default)</li><li>Low</li></ul> |

11. Configure Include and Exclude Labels:
       - Define the rules to:
          - Include PVCs with specific labels
          - Exclude PVCs with specific labels
       - Label matching options:
          - Match all labels
          - Match any selected label

12. Create or Select a Protection Policy.

    | Option | Description |
    |------|-------------|
    | **Create a New Protection Policy** | A Protection Policy defines how backups are handled. Available options include: <ul><li>**DataLock**: Enables tamper‑proof retention (manageable only by Data Security role users).</li><li>**Backup Schedule & Retention**: Defines backup frequency, retention period, and type.</li><li>**Periodic Full Backups**: Schedule recurring full backups.</li><li>**Extended Retention**: Retain selected snapshots longer.</li><li>**Quiet Times**: Block new runs during defined windows.</li><li>**Retry Options**: Configure snapshot retry behavior.</li></ul> |
    | **Use an Existing Protection Policy** | Go to **Policy Management** \> Select the policy \> Click **Edit** \> Update Kubernetes‑specific settings \> Click **Save**.<br><br>**Note:** Policy changes take effect during the **next scheduled protection run**. |

13. You can assign a new Protection Policy to a Kubernetes object, effective from the next scheduled protection run.
    - In **Backup and Recovery Service**, go to `Sources`, select the Kubernetes source, select the namespace and click Protect.
    - Choose the required policy from the Policy drop-down.
    - Click `Protect` to apply.

14. Exclude namespace based on labels:
    1. In Protection, under Objects click on Labels.
    2. Select a label.
    3. Click on the exclude icon (tooltip: `Exclude Tag Off. Click to exclude this label`).
    4. Selected namespace will be excluded from protection runs.

15. Every namespace can be customized while creating protection run, this includes:
    1. **Inclusion/Exclusion of PVCs**: Include/Exclude PVCs that are available in a namespace.
    2. **Inclusion/Exclusion of Resources**: Include/Exclude resources such as Pods, DaemonSets, ReplicaSets, Secrets, Services, StatefulSets, etc.
    3. **Scripts**: Select the Quiesce Mode and Add Rules (Pre/Post Snapshot Scripts). Once customized, a pencil icon appears next to the namespace.

16. Enable Auto Protect. Auto Protect helps ensure automatic inclusion of new namespaces.

    | Auto Protect Type | Steps |
    |-----------------|-------|
    | **Cluster‑Level Auto Protect** | Go to **Protection** \> Select the Kubernetes cluster \> Toggle **Auto Protect** at cluster level \> Click **Protect** to confirm. |

    When Auto Protect is enabled:
      - Newly created namespaces or existing namespaces that receive the selected protection label are automatically added to the protection group and included in the next scheduled backup run.
      - Namespaces that are deleted from the cluster or have the protection label that is removed are automatically excluded from all future backup operations.
      - Existing backups of removed or unlabeled namespaces remain preserved until their configured retention period expires.

17. Start Protection and Monitor. Click **Protect** to initiate protection. The Protection Service begins backing up selected objects. You can monitor progress at: **Activity** \> **Protection**.

18. **Application Quiescing**: {{site.data.keyword.baas_full_notm}} supports quiescing stateful Kubernetes workloads to help ensure consistent and reliable PVC snapshots. Before capturing a snapshot, it temporarily pauses the application to bring it to a quiesced state. Once the snapshot is successfully taken, the workloads are automatically unquiesced.

    **Supported Quiescing Modes:**

    | Mode | Description |
    |---|---|
    | **Together mode** | All quiescing rules run in parallel within a single volume group. Offers faster performance than sequential execution. |
    | **Independent mode** | Uses multiple volume groups where each group runs its own set of rules independently and in parallel. Delivers the fastest overall backup speed. |
    | **Sequential mode** | Processes all quiescing rules one after another in a single volume group. Resulting in the slowest backup but providing the strictest control. |

    **Configure quiescing for a namespace:**
       1. Select the target namespace and click **Edit**.
       2. Navigate to the **Scripts** section.
       3. Select the desired **Quiesce Mode** (Together, Independent, or Sequential).
       4. Create quiesce rules by specifying:
          - **Pod selector labels**: To identify which pods to quiesce.
          - **Pre-snapshot scripts**: Run before snapshot.
          - **Post-snapshot scripts**: Run after snapshot.
       5. Set the failure behavior (e.g., continue or fail the backup on script/rule error).
       6. Click **Save** to apply the configuration.

    Scripts run inside containers and have configurable timeouts.
    {: note}

19. Manage Protection Lifecycle.

    | Action | Description |
    |------|-------------|
    | **Pause Future Runs** | Protection \> Select job \> Actions menu \> **Pause Future Runs** |
    | **Resume Protection** | Protection \> Select job \> Actions menu \> **Resume** |
    | **Delete Protection** | **Delete Object Only**: Keeps snapshots until expiry.<br>**Delete Object and Snapshots**: Removes object and all backups. |

20. If needed, after you create the protection group, click `Run Now` to start the backup immediately. For more information, see the [Protection group Run Now](/docs/backup-recovery?topic=backup-recovery-protection-group-run-now).
