---

copyright:
  years: 2025
lastupdated: "2026-02-20"

keywords: data source connector, iks, roks, cluster, troubleshooting

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting
{: #data-source-connector-iks-roks-troubleshooting}

## Data Source Connector related issues
{: #dsc-troubleshooting}

### Data Source Connector Pod Scheduling and Zonal Constraints
{: #dsc-pod-scheduling}

**Overview**

Data Services (Data Source Connector) is deployed as a StatefulSet, which relies on the VPC Block CSI driver. Because VPC Block storage is a zonal resource, a Persistent Volume (PV) created in "Zone A" cannot be attached to a pod running in "Zone B."
If you modify your cluster's zones or experience a zonal outage, your Data Source Connector pods might enter a `Pending` state or fail to attach storage.

**Common Symptoms**

- Data Source Connector Pods remain in `Pending` status indefinitely.
- Describe pod errors such as: `node(s) had volume node affinity conflict`.
- Errors indicating: `FailedAttachVolume` or `Volume is already in use by another pod`.

**Root Cause Analysis**

This issue typically occurs under three scenarios:
- **Zone Removal**: A zone was removed from the worker pool, but Data Source Connector volumes still reside in that zone.
- **Configuration Change**: The cluster's zonal layout was modified after the initial Data Source Connector installation.
- **Zonal Outage**: A specific zone is experiencing a service disruption, preventing the CSI driver from mounting the volume.

**Resolution Steps**

**Option 1: Restore the Original Zonal Configuration (Recommended)**

The fastest way to restore service is to bring the cluster back to the state it was in when Data Source Connector was first deployed.
- **Step**: Readd the removed zones to your worker pool.
- **Result**: The scheduler recognizes the nodes in those zones and successfully bind the existing VPC Block volumes to the Data Source Connector pods.

**Option 2: Reinstall Data Source Connector**

If you must move to a new zonal configuration and do not need to persist the existing data, a fresh installation is required.
1. Completely uninstall the Data Source Connector deployment.
2. Help ensure the Persistent Volume Claims (PVCs) and PVs associated with the old zones are deleted.
3. Reinstall Data Source Connector. This allows the CSI driver to provision new volumes in the currently active zones.

### Data Source Connector Not Appearing After Installation
{: #dsc-not-appearing}

If the Data Source Connector does not show up in the {{site.data.keyword.baas_full_notm}} service (or related interface) even after completing the installation, follow these steps to identify and resolve the issue.

1. **Verify VPE Gateway Settings**: Check your VPC configuration to help ensure that no VPE (Virtual Private Endpoint) gateway is enabled for the {{site.data.keyword.baas_full_notm}} instance.

2. **Confirm the Correct Connection Type**: Double-check that the connection type that is selected during setup matches your actual cluster:
   - IKS classic
   - IKS VPC
   - ROKS classic
   - ROKS VPC

   Using the wrong type prevents the connector from registering correctly and appearing in the console.

3. **Use a Fresh, Valid Connection Token**: Connection tokens can expire quickly.
   - Always generate a new token right before starting the installation.
   - Do not reuse an old or previously generated token, as it might already be invalid.

4. **Install the Latest Version of the Connector**: Make sure that you are deploying the most recent version available:
   - Older versions might have compatibility issues or bugs that prevent proper registration.

5. **Check the Data Source Connector Pod for Issues**: Inspect the connector pod in your Kubernetes/OpenShift cluster for problems during startup:
   - Run `kubectl describe pod <pod-name>` (or equivalent oc command) and review the Events section.
   - Look specifically for:
      - Readiness probe failures
      - Volume attachment errors
      - IAM-related authentication issues during volume mounting or attachment

   If you see IAM/permission errors that are attached to the volume attachment, reset the infrastructure API key by running:

   ```sh
   ibmcloud ks api-key reset --region <your-region>
   ```
   {: codeblock}

   Then, restart or redeploy the affected pod or pods as needed. This refreshes the credentials that are used for storage and infrastructure operations.

   Next Steps if the issue persists:
   - Review pod logs (`kubectl exec -it brs-ds-connector-0 -n ibm-brs-data-source-connector -- cat /cohesity_logs/iris_exec.ERROR`) for more detailed error messages.
   - Confirm that the connector is successfully authenticated with the Backup and Recovery service (this might take a few extra minutes after deployment).
   - Open a support case with logs and the output of the preceding commands.

## Source registration-related issues
{: #registration-troubleshooting}

### Data Source Connector & Cluster Alignment
{: #dsc-alignment}

To help ensure successful source registration and maintain data integrity, follow these deployment requirements:
1. **Single Installation Policy**: Each Kubernetes or OpenShift cluster must have only one release of the Data Source Connector installed.
2. **Dedicated Cluster Pairing**: Every cluster must be paired with its own unique Data Source Connection.
3. **Connection Integrity**: While it is technically possible to share a single Data Source Connection across multiple clusters, we strongly recommend a **dedicated connection per cluster**. This alignment prevents data inconsistencies and helps ensure a successful handshake between the Data connector and the source cluster.

### Incorrect brs-backup-agent image
{: #incorrect-backup-agent}

If there is a registration failure, verify whether the {{site.data.keyword.baas_full_notm}} backup agent is running on the Kubernetes or OpenShift cluster.

Check whether the backup agent is running:
```bash
kubectl get pods -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
```
{: codeblock}

If the backup agent is not running and showing an `ImagePullBackOff` error, you need to describe the Velero or DM pods from the previous step and check the images that are used at the time of registration.
```bash
kubectl describe pod velero-* -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
kubectl describe pod cohesity-dm-* -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
```
{: codeblock}

### Check the Health of the Connector
{: #connector-health}

To verify the status of your connection and help ensure that the connector is operating correctly, follow these steps in the console:
1. **Navigate to the Health Status**: Go to **System** \> **Data Source Connection**.
2. **Verify Connectivity**: Locate your specific Data Source Connection and confirm that both the `Connection` and the `Connector` are showing a `Healthy` status.

### HTTP Connection Errors
{: #http-connection-error}

If you encounter an HTTP connection error on the **Data Protection** > **Sources** page during registration, follow these verification steps to identify and resolve the issue:

1. **Verify Cluster and Resource Integrity**: Confirm that your Kubernetes or OpenShift cluster is active and has not been deleted or scaled down. Help ensure that all core resources that are associated with the integration are intact and fully functional.
2. **Check the Health of Velero and Datamover Pods**: Verify that the Velero and datamover pods are in a `Running` state within the `brs-backup-agent-<uuid>` namespace. If these pods are offline or experiencing frequent restarts, the connector is unable to process data requests successfully. Make sure that the right images are being used for Velero and data mover pods. See [Incorrect brs-backup-agent image](#incorrect-backup-agent).
3. **Verify Data Source Connector Status**:
   1. Verify that the Data source connector is healthy. See [Check the Health of the Connector](#connector-health).
   2. Verify that the Data Source Connector (DSC) pods are running correctly within the `ibm-brs-data-source-connector` namespace. Check for any deployment failures, such as restart loops or `ImagePullBackOff` errors, to help ensure that the connector is in a healthy state.
4. **Review Network Security Rules**: Inspect your VPC and cluster-level security groups or Network ACLs to help ensure that no recent firewall rules or security policies are blocking the connector's traffic. While the required communication on ports **443** (communication to Cloud Object Storage) and **29991** (communication to backup recovery service cluster) is typically enabled by default and does not require manual configuration, you should verify that no custom outbound or inbound restrictions have been implemented that might inadvertently disrupt connectivity on these specific ports.

## Troubleshooting alerts
{: #alert-troubleshooting}

### Monitoring Alerts and Debugging Failed Jobs
{: #monitor-alert}

If you see an alert in the **System** \> **Health** dashboard, follow these steps to identify and resolve the underlying issue.

**Pre-check: Verify Environment Integrity**

Before diving into detailed logs, perform a quick high-level check to help ensure that the foundational integration is still valid:
- **Source Registration**: Navigate to the **Data Protection** > **Sources** page to confirm the source registration is still intact and has not been accidentally removed.
- **Resource Availability**: Verify that the underlying Kubernetes or OpenShift cluster resources are active and have not been deleted or scaled down, as this will cause all associated jobs to fail.

**Identifying the Alert**

When a job fails, a health alert is generated with a description that helps pinpoint the source of the error.
1. **Locate the Details**: The alert description specifies the **Protection Group** name and the **type of job** that failed.
2. **Example Alert**: `Backup run of protection group [Group-Name] of type kKubernetes failed`.
3. **Check the Cause**: The alert will often include a summary of the failure cause to give you an immediate starting point for troubleshooting.


## Protection Job Failure (Pod Timeout)
{: #protection-job-failure}

When a protection job fails with a "Timed out while waiting for temporary pod to start" error, it typically indicates that the Kubernetes scheduler cannot find a suitable place to run the required backup or restore pod.
Follow the steps below to diagnose and resolve this resource constraint.

**Step 1: Identify the Failing Job**

First, pinpoint which job is stalling within the namespace you are protecting. Run the following command:

```bash
kubectl get jobs -n <your-protected-namespace>
```
{: codeblock}

Look for jobs that have a 0/1 completion status or have been stuck in a "Pending" state for several minutes.

**Step 2: Inspect the Pod Events**

Once you identify the failing job, check the events of the associated pod to see why it isn't starting:

```bash
kubectl describe pod <temporary-pod-name> -n <your-protected-namespace>
```
{: codeblock}

**Step 3: Analyze the Error Message**

If you see a `Warning FailedScheduling` message similar to the one below, you are facing a Volume Limit or Affinity issue:
`0/3 nodes are available: 1 node(s) exceed max volume count, 2 node(s) had volume node affinity conflict.`

**What this means:**

- **Exceed Max Volume Count**: Each worker node has a hard limit on the number of volumes it can attach (12 volumes per node). This error means that the node is "full."
- **Volume Node Affinity Conflict**: The pod needs to access a specific volume that is physically located on a different node, but that node is already at its capacity or otherwise unavailable.

### Recommended Solutions

If your environment has reached its volume capacity, use one of the following methods to resolve the bottleneck:

| Strategy | Action | Recommended For |
| :--- | :--- | :--- |
| **Clean Up Resources** | Delete unused Persistent Volume Claims (PVCs) or orphan volumes that are still occupying slots on your worker nodes. | Quick fixes; environments with high churn. |
| **Scale Out Cluster** | Add a new worker node to the cluster. This provides a fresh set of volume "slots" for the scheduler to use. | Long-term growth; high-density workloads. |
| **Optimize Density** | Evaluate if multiple applications can share volumes or if smaller volumes can be consolidated. | Advanced users optimizing for cost. |
{: caption="Table 1. Recommended solutions for volume limit issues" caption-side="bottom"}

## Deleting and Unregistering Resources
{: #deleting-unregistering-resources}

To maintain data integrity and security, specific dependencies must be cleared before you can delete core resources like Connections, Connectors, or Unregistered Sources.
If your resources are currently associated with a Protection Group, follow the structured workflow below to help ensure a smooth deletion process.

### Understanding Data Lock Constraints
{: #data-lock-constraints}

Before attempting to delete a Protection Group, check if your assigned policy has Data Lock enabled.

- **The Restriction**: If Data Lock is active, the Protection Group and its associated resources cannot be deleted until the lock period expires.
- **How to Check**: Navigate to the **Run Details** page for your job. Look for the **Expiry Date**; this is the earliest date the resource becomes available for deletion.
- **Proactive Tip**: To avoid long waiting times in the future, consider creating a Custom Policy with a shorter Data Lock duration or with Data Lock disabled entirely, rather than using the default settings.

### Recommended Deletion Order
{: #recommended-deletion-order}

Once any applicable Data Locks have expired, resources must be removed in this specific sequence to avoid dependency errors:

1. **Protection Group**: Delete the group first to stop all scheduled tasks.
2. **Unregister Source**: Once the group is gone, you can safely unregister the data source.
3. **Connectors**: Remove the bridge between your source and the connection.
4. **Connection**: Finally, delete the underlying connection credentials or configuration.

## Using the Kubernetes Info Fetcher Script
{: #info-fetcher-script}

The `k8s-info-fetcher.sh` script collects detailed information about your Kubernetes cluster and the BRS Backup Agent (datamover) components. It can also optionally collect information from a specific user namespace.

### Usage
{: #script-usage}

```bash
./k8s-info-fetcher.sh [-u|--user_ns <ns-name>] [-p|--prefix <prefix>]
```

*   `-u, --user_ns`: (Optional) Specify an additional user namespace to fetch information from.
*   `-p, --prefix`: (Optional) Prefix for the backup namespace (default: `brs-backup-agent-`).

### What the Script Collects
{: #what-script-collects}

The script gathers the following data and bundles it into a `.tar` archive:

**1. Cluster‑level information**
*   Nodes
*   Namespaces
*   StorageClasses
*   PersistentVolumes
*   CRDs
*   ClusterRoles
*   ClusterRoleBindings
*   VolumeSnapshotClasses

**2. Backup Agent Namespace Information**
From the namespace matching the prefix (default: `brs-backup-agent-*`), it captures:
*   Pods
*   PVCs
*   Services
*   Deployments
*   DaemonSets
*   Resource Limits
*   Secrets
*   Service Accounts
*   VolumeSnapshots
*   VolumeSnapshotContents
*   **Velero Logs**: Fetched from the Velero deployment.
*   **Datamover Logs**: Copied from `/opt/cohesityagent/data/logs/` inside pods matching `cohesity-dm-`.

**3. User Namespace Information (Optional)**
If a user namespace is specified with `-u`, it captures:
*   Pods
*   Jobs
*   Services
*   Deployments
*   DaemonSets
*   Resource Limits
*   PVCs
*   VolumeSnapshots
*   VolumeSnapshotContents

### How to Run the Script
{: #run-script}

**Step 1: Download the script**

[Download the script](https://support-tools.s3.us-south.cloud-object-storage.appdomain.cloud/k8s_info_fetcher.sh).

**Step 2: Download and configure the cluster**

```bash
ibmcloud ks cluster config --cluster <cluster-id> --admin
```
{: codeblock}

**Step 3: Verify your context**

```bash
kubectl config current-context
```
{: codeblock}

**Step 4: Give script execute permissions**

```bash
chmod +x k8s-info-fetcher.sh
```
{: codeblock}

**Step 5: Run the script**

*   **Standard collection** (Cluster info + Backup Agent info):
    ```bash
    ./k8s-info-fetcher.sh
    ```
    {: codeblock}

*   **Collection with User Namespace** (Cluster + Backup Agent + Your App Namespace):
    ```bash
    ./k8s-info-fetcher.sh -u <your-namespace>
    ```
    {: codeblock}
