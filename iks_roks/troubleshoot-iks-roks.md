---

copyright:
  years: 2025
lastupdated: "2026-02-12"

keywords: data source connector, iks, roks, cluster, run now

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting
{: #data-source-connector-iks-roks-troubleshooting}

## Data Source Connector related issues
{: #dsc-troubleshooting}

### Data Source Connector Not Appearing After Installation
{: #dsc-not-appearing}

If the Data Source Connector does not show up in the {{site.data.keyword.baas_full_notm}} service (or related interface) even after completing the installation, follow these steps in order to identify and resolve the issue.

1. Verify VPE Gateway Settings Check your VPC configuration to ensure that no VPE (Virtual Private Endpoint) gateway is enabled for the BRS (Backup and Recovery Service) instance.
2. Confirm the Correct Connection Type Double-check that the connection type selected during setup matches your actual cluster:
- IKS classic
- IKS VPC
- ROKS classic
- ROKS VPC

Using the wrong type prevents the connector from registering correctly and appearing in the console.

3. Use a Fresh, Valid Connection Token Connection tokens can expire quickly.
- Always generate a new token right before starting the installation.
- Do not reuse an old or previously generated token, as it may already be invalid.
4. Install the Latest Version of the Connector Make sure you are deploying the most recent version available:
- Older versions may have compatibility issues or bugs that prevent proper registration.
5. Check the Data Source Connector Pod for Issues Inspect the connector pod in your Kubernetes/OpenShift cluster for problems during startup:
- Run `kubectl describe pod <pod>` (or equivalent oc command) and review the Events section.
- Look specifically for `Readiness probe failures`, `Volume attachment errors`, and `IAM-related authentication issues during volume mounting/attachment`.
- If you see IAM/permission errors tied to volume attachment, reset the infrastructure API key by running:

```sh
ibmcloud ks api-key reset --region <your-region>
```
{: codeblock}

Then, restart or redeploy the affected pod or pods as needed. This refreshes the credentials used for storage and infrastructure operations.

Next Steps if the issue persists:
- Review pod logs (`kubectl exec -it brs-ds-connector-0 -n ibm-brs-data-source-connector -- cat /cohesity_logs/iris_exec.ERROR`) for more detailed error messages.
- Confirm the connector is successfully authenticated with the Backup and Recovery service (this may take a few extra minutes after deployment).
- Open a support case with logs and the output of the above commands.

### DSC Pod Scheduling and Zonal Constraints
{: #dsc-pod-scheduling}

Data Services (DSC) is deployed as a StatefulSet, which relies on the VPC Block CSI driver. Because VPC Block storage is a zonal resource, a Persistent Volume (PV) created in "Zone A" cannot be attached to a pod running in "Zone B."
If you modify your cluster's zones or experience a zonal outage, your DSC pods may enter a `Pending` state or fail to attach storage.

**Common Symptoms**
- DSC Pods remain in `Pending` status indefinitely.
- Describe pod errors such as: `node(s) had volume node affinity conflict`.
- Errors indicating: `FailedAttachVolume` or `Volume is already in use by another pod`.

**Root Cause Analysis**
This issue typically occurs under three scenarios:
- Zone Removal: A zone was removed from the worker pool, but DSC volumes still reside in that zone.
- Configuration Change: The cluster's zonal layout was modified after the initial DSC installation.
- Zonal Outage: A specific zone is experiencing a service disruption, preventing the CSI driver from mounting the volume.

**Resolution Steps**

Option 1: Restore the Original Zonal Configuration (Recommended)
The fastest way to restore service is to bring the cluster back to the state it was in when DSC was first deployed.
- Step: Readd the removed zones to your worker pool.
- Result: The scheduler recognizes the nodes in those zones and successfully bind the existing VPC Block volumes to the DSC pods.

Option 2: Reinstall DSC
If you must move to a new zonal configuration and do not need to persist the existing data, a fresh installation is required.
- Completely uninstall the DSC deployment.
- Ensure the Persistent Volume Claims (PVCs) and PVs associated with the old zones are deleted.
- Reinstall DSC. This will allow the CSI driver to provision new volumes in the currently active zones.

## Source registration-related issues
{: #registration-troubleshooting}

### Data Source Connector & Cluster Alignment
{: #dsc-allignment}

To ensure successful source registration and maintain data integrity, follow these deployment requirements:
1. **Single Installation Policy**: Each Kubernetes or OpenShift cluster must have only one release of the Data Source Connector installed.
2. **Dedicated Cluster Pairing**: Every cluster should be paired with its own unique Data Source Connection.
3. **Connection Integrity**: While it is technically possible to share a single Data Source Connection across multiple clusters, we strongly recommend a **dedicated connection per cluster**. This alignment prevents data inconsistencies and ensures a successful handshake between the Data connector and the source cluster.

### Incorrect brs-backup-agent image
{: #incorrect-backup-agent}

In case of registration failure, verify that the IBM Cloud Backup and Recovery backup agent is running or not on the Kubernetes/OpenShift cluster.

Check whether the backup agent is running:
```bash
kubectl get pods -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
```

If BRS backup agent is not running and getting ImagePullBackOff, then you need to describe the Velero, or DM pods got from previous step and then describe to see what all images you have used at the time of registration.
```bash
kubectl describe pod velero-* -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
kubectl describe pod cohesity-dm-* -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
```

### Check Health of the Connector
{: #connector-health}

To verify the status of your connection and ensure the connector is operating correctly, follow these steps in the console:
1. Navigate to the Health Status: Go to `System` \> `Data Source Connection`.
2. Verify Connectivity: Locate your specific Data Source Connection and confirm that both the `Connection` and the `Connector` are showing a `Healthy` status.

### HTTP Connection Errors
{: #http-connection-error}

If you encounter an HTTP connection error on the Data Protection > Sources page during registration, follow these verification steps to identify and resolve the issue:

1. **Verify Cluster and Resource Integrity**: Confirm that your Kubernetes or OpenShift cluster is active and has not been deleted or scaled down. Ensure that all core resources associated with the integration are intact and fully functional.
2. **Check Health of Velero and Datamover Pods**: Verify that the Velero and datamover pods are in a Running state within the `brs-backup-agent-<uuid>` namespace. If these pods are offline or experiencing frequent restarts, the connector will be unable to process data requests successfully. Make sure the right images are being used for veloro and data mover pods. See [Incorrect brs-backup-agent image](#incorrect-backup-agent).
3. **Verify Data Source Connector Status**:
   1. Ensure Data source connector is healthy. See [Check Health of the Connector](#connector-health).
   2. Ensure the Data Source Connector (DSC) pods are running correctly within the ibm-brs-data-source-connector namespace. Check for any deployment failures, such as restart loops or ImagePullBackOff errors, to ensure the connector is in a healthy state.
4. **Review Network Security Rules**: Inspect your VPC and cluster-level security groups or Network ACLs to ensure no recent firewall rules or security policies are blocking the connector's traffic. While the required communication on ports **443 (communication to cos storage)** and **29991(communication to cohesity cluster)** is typically enabled by default and does not require manual configuration, you should verify that no custom outbound or inbound restrictions have been implemented that might inadvertently disrupt connectivity on these specific ports.

## Alerts troubleshoot
{: #alert-troubleshooting}

### Monitoring Alerts and Debugging Failed Jobs
{: #monitor-alert}

If you see an alert in the `System` \> `Health` dashboard, follow these steps to identify and resolve the underlying issue.

**Verify environment integrity**:

Before diving into detailed logs, perform a quick high-level check to ensure the foundation of your integration is still valid:
- **Source Registration**: Navigate to the `Data Protection` > `Sources` page to confirm the source registration is still intact and has not been accidentally removed.
- **Resource Availability**: Verify that the underlying Kubernetes or OpenShift cluster resources are active and have not been deleted or scaled down, as this will cause all associated jobs to fail.

**Identifying the Alert**:
When a job fails, a health alert is generated with a description that helps pinpoint the source of the error.
1. **Locate the Details**: The alert description will specify the **Protection Group** name and the **type of job** that failed.
2. **Example Alert**: `Backup run of protection group [Group-Name] of type kKubernetes failed`.
3. **Check the Cause**: The alert will often include a summary of the failure cause to give you an immediate starting point for troubleshooting.
