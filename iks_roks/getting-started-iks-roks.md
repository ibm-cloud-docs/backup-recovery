---

copyright:
  years: 2025
lastupdated: "2026-02-12"

keywords: data source connector, iks, roks, cluster

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Register Kubernetes/OpenShift as a data source
{: #data-source-connector-iks-roks}

This information is provided for beta use only and is subject to change. Only region Washington DC (us-east) is available now for this feature.
{: beta}

You must create a data source connector on the same VPC where the Kubernetes/OpenShift cluster is deployed.
{: important}

Located to the right of this page is a summary of key topics that are found on this page.
{: note}

## Quick reference to key sections for new users
{: #iks-roks-tutorial-quick-reference}

A. [Before you begin](#baas-getting-started-iks-roks)

B. Prerequisites for backup and restore:
   - You must have a [{{site.data.keyword.baas_full_notm}} instance that is created or create a new one](#data-source-connector-iks-roks-access-instance)
   - [Create or use existing data source connector](#data-source-connector-iks-roks-create-configure)
   - [Kubernetes/OpenShift cluster should be registered](#data-source-connector-iks-roks-register)

C. Take a backup of the Kubernetes/OpenShift cluster:
   - [Access {{site.data.keyword.baas_full_notm}} instance](#data-source-connector-iks-roks-access-instance)
   - [Create and configure data source connector](#data-source-connector-iks-roks-create-configure)
   - [Configure and set up network SGs](#data-source-connector-iks-roks-setup-network-sgs-config)
   - [Register source kubernetes/OpenShift cluster](#data-source-connector-iks-roks-register)
   - [Create or schedule a backup](#protecting-namespace-iks-roks)

D. Restore backup to Kubernetes/OpenShift cluster:
   - [Access {{site.data.keyword.baas_full_notm}} instance](#data-source-connector-iks-roks-access-instance)
   - [Create and configure data source connector](#data-source-connector-iks-roks-create-configure)
   - [Register source kubernetes/OpenShift cluster](#data-source-connector-iks-roks-register)
   - [Restore backup](#recovering-restoring-backup)

E. [Troubleshooting](#data-source-connector-iks-roks-troubleshooting)


## Before you begin
{: #baas-getting-started-iks-roks}

You need the following to get started with {{site.data.keyword.baas_full_notm}} with Kubernetes/OpenShift:
- An [{{site.data.keyword.cloud}} Platform account](https://cloud.ibm.com)
- An active instance of [{{site.data.keyword.baas_full_notm}} service](https://cloud.ibm.com/catalog/services/backup-and-recovery?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2cjaGlnaGxpZ2h0cw%3D%3D)

## Accessing your instances
{: #data-source-connector-iks-roks-access-instance}

1. Verify that your IBM Cloud account has access to the required {{site.data.keyword.baas_full_notm}} service.
    1. Locate and open the [Resource List](https://cloud.ibm.com/resources) for your account.
    2. Filter by Product for Backup.
    3. Search for your instance name, or keep the search field empty to view all instances.
    4. Click an instance of your choice to see more details.
    5. Click the **Launch dashboard** button to open the instance.
2. Alternatively, obtain the instance details and credentials to log in.

## Backup requirements for Kubernetes/OpenShift clusters
{: #data-source-connector-iks-roks-backup-requirements}

Your Kubernetes/OpenShift cluster must be registered with the instance to:

- Take backups
- Restore backups

## Backup and Restore compatibility
{: #data-source-connector-iks-roks-brs-comp}

You can back up and restore data on:

- The same Kubernetes/OpenShift cluster
- A different Kubernetes/OpenShift cluster within the same region
- An application running on a Kubernetes/OpenShift cluster
- Kubernetes/OpenShift namespace, PVCs, and more

Clusters must be compatible, especially in terms of storage configuration.
{: note}

## Create or configure a data source connector
{: #data-source-connector-iks-roks-create-configure}

Ensure the node has sufficient CPU and memory to run the Containerized Data Source Connector and Datamover pods. The following table lists their resource requirements. If both pods run on the same node, use at least a `cx2.8x16` node flavor to meet the combined workload demands.

| Pod Name                                   | CPU Requests | Memory Requests | CPU Limits | Memory Limits |
|--------------------------------------------|--------------|-----------------|------------|----------------|
| Containerized Data Source Connector        | 2            | 5Gi             | 4          | 8Gi            |
| Datamover                                  | 500m         | 128M            | 2          | 4Gi            |


### Create a data source connection
{: #data-source-connector-iks-roks-create-data-source-connection}

1. Open the instance dashboard that was discussed in an earlier step on [Accessing your instances](#data-source-connector-iks-roks-access-instance).
2. Go to: `Dashboard → System → Data Source Connections`.
3. Click `New Connection`.
4. Under the Deployment Platform, select one of the following options:

   - `ROKS CLASSIC`
   - `ROKS VPC`
   - `IKS CLASSIC`
   - `IKS VPC`

5. Click `Done`, then open the three‑dot menu for the newly created connection. Click `Rename Connection` to rename it for easier future identification.



### Configure a data source connector
{: #data-source-connector-iks-roks-create-configure}







1. Configure the connector by using the `helm install` command.
2. In the Cloud Shell terminal, list the available Kubernetes/OpenShift clusters to identify the cluster name:

   ```sh
   ibmcloud ks cluster ls
   ```
   {: codeblock}

   From the output, note the cluster name where the DSC will be deployed.

3. Download and configure the `KUBECONFIG` for the selected cluster with admin privileges:

   ```sh
   ibmcloud ks cluster config --cluster <cluster-name> --admin
   ```
   {: codeblock}

4. The Helm chart is hosted in the IBM Container Registry (ICR). Log in to the Helm/OCI registry by using the following command:

   ```sh
   helm registry login icr.io --username iamapikey --password "${API_KEY}"
   ```
   {: codeblock}

   If successful, you see a login confirmation.



   

   

5. Return to the UI and click the three‑dot menu for your selected connection. Click `Connection Token` to copy the connection token.
6. Now, run the following command in the IBM Cloud Shell.

   ```sh
   # define variables
   export NAMESPACE_NAME="ibm-brs-data-source-connector"
   export RELEASE_NAME="dsc"
   export CHART_LOCATION="oci://icr.io/ext/brs/brs-ds-connector-chart"
   export CHART_VERSION="7.2.17-release-20260108-ed857f1c"
   export REGISTRATION_TOKEN='<paste the connection/registration token here>'

   # install data source connector chart
   helm install ${RELEASE_NAME} ${CHART_LOCATION} --version ${CHART_VERSION} --set secrets.registrationToken=${REGISTRATION_TOKEN} --set fullnameOverride=dsc  --namespace ${NAMESPACE_NAME} --create-namespace
   ```
   {: codeblock}

   

   
7. Check that the Helm release is installed:

   ```sh
   helm list -n ${NAMESPACE_NAME}
   ```
   {: codeblock}







## How to get the Kubernetes/OpenShift cluster ID
{: #how-to-get-iks-roks-cluster-id}

1. Go to `Menu → Containers → Clusters.`
2. Filter by location: Washington DC(us-east) and search for your cluster name or ID.
3. Click cluster and in the **Overview** page look for the Cluster Details section to get the cluster ID.

## How to get the Kubernetes/OpenShift cluster endpoint
{: #how-to-get-iks-roks-endpoint}

1. Go to `Menu → Containers → Clusters.`
2. Filter by location: Washington DC(us-east).
3. The endpoint can be found in the **Overview page** in the Networking section where you can find the information for your private and public endpoints.






## Permitting DNS resolution
{: #permitting-dns-resolution}

To allow the data source connector, you need to copy data to Cloud Object Storage endpoint – If your data source connector is on a private network, then you need to follow the following steps to allow the data source connector to copy data

   1. Create or identify [VPE gateway](#how-to-get-vpe-gateway-cos-s3) for Cloud Object Storage direct endpoints - this normally created VPE gateway while creating Kubernetes/OpenShift cluster under VPC (default networking).
   2. In the Overview tab, enable `Permit DNS resolution binding` to resolve from data source connector.

## How to register an Kubernetes/OpenShift cluster with {{site.data.keyword.baas_full_notm}} service
{: #data-source-connector-iks-roks-register}

1. Open your {{site.data.keyword.baas_full_notm}} instance.
2. Go to: `Dashboard → Data Protection → Sources → Register Source`.
3. Choose the Kubernetes cluster type.
4. Select the previously created [data source connection](#data-source-connector-iks-roks-create-data-source-connection), or create a new connection.
5. Click **Continue**.
6. Provide these details in the form:

   |  Cluster Endpoint  |  Bearer Token |  Kubernetes Distribution  |
   |----|----|----|
   |[Private or Public](#how-to-get-iks-roks-endpoint)|[See How to create a Bearer Token](#data-source-connector-iks-roks-create-bearer-token-cluster)|Kubernetes/OpenShift|

   

7. Click **Complete** to finish the registration.
8. You are redirected to the list of data sources where you can see the status of your data source registration.

## How to create a bearer token for a Kubernetes/OpenShift cluster
{: #data-source-connector-iks-roks-create-bearer-token-cluster}

1. Configure kubectl or oc cli by getting kube config `ibmcloud ks cluster config --cluster <cluster> --admin`
2. Create service account and cluster role binding

    ```sh
    kubectl create serviceaccount brs-sa -n default
    ```
    {: pre}

    ```sh
    kubectl create clusterrolebinding brs-cl-role --clusterrole=cluster-admin --serviceaccount=default:brs-sa
    ```
    {: pre}


3. Create secret:
   1. Create a file: secret.yaml.
   2. Add the following contents to the file.

      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
         name: ibm-token
         namespace: default
         annotations:
         kubernetes.io/service-account.name: ibm
      type: kubernetes.io/service-account-token
      ```
      {: pre}

   3. Run the command:

      ```sh
      kubectl apply -f secret.yaml
      ```
      {: pre}

   4. Get the bearer token and use for the registration

      ```sh
      kubectl describe secrets -n default ibm-token | grep token: | awk '{print $2}' | head -1
      ```
      {: pre}



## Prerequisites for scheduling backups
{: #data-source-connector-iks-roks-prereq-schedule-bak}

1. You must have a [Protection Policy](/docs/backup-recovery?topic=backup-recovery-baas-policy-creation) in place to create or run a backup. You can either:
   - Use an existing protection policy, or
   - Create a new user-defined policy
2. You need to create a [Protection Group](/docs/backup-recovery?topic=backup-recovery-protection-groups) that uses one of these policies to perform an incremental or full backup.



## Protecting a namespace or cluster and scheduling a backup
{: #protecting-namespace-iks-roks}

1. Log in to the {{site.data.keyword.baas_full_notm}}.
2. Go to: **Dashboard** \> **Data Protection** \> **Protection**.
3. Locate your Kubernetes source cluster using the registration endpoint.
4. Click the cluster URL to view available objects.
5. After selecting the cluster, a list of namespaces appears.
6. Choose one of the following:
   - One or more namespaces (you can also exclude namespace based on lables)
   - Entire cluster (if supported by your deployment)

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
| **Create a New Protection Group** | When creating a new group, configure the following:<br><br>- **Protection Group Name**<br>- **Protection Policy**<br>- **Start Time and Time Zone**<br>- **Leverage CSI Snapshot** (toggle)<br>- **Pause Future Runs** (optional)<br>- **Alerts and Email Recipients**<br>- **Priority** (High / Medium / Low)<br>- **Include or Exclude Labels**<br>- **Description**<br><br>These settings are configurable **only during creation** of a new Protection Group. |
| **Use an Existing Protection Group** | All settings are **prefilled** from the existing group.<br>Fields are **read-only** and cannot be modified at this stage. |

9. Configure the CSI Snapshot Protection.

   - Enable Leverage CSI Snapshot (toggle).
   - When enabled:
      - PVCs are protected using CSI driver snapshots.
      - Snapshots are crash consistent, capturing the volume state at the snapshot moment.

10. Configure Additional Protection Settings.

| Category | Description |
|--------|-------------|
| **Scheduling** | **Start Time**: Defines when the protection job runs.<br>**Time Zone**: Select the appropriate time zone. |
| **Run Controls** | **Pause Future Runs**: Prevents any future scheduled runs.<br>**End Date**: Stops snapshot capture after the selected date (current runs complete). |
| **Storage & Performance** | **QoS Policy**:<br>- Backup HDD (default)<br>- Backup SSD<br>- Backup Auto |
| **Alerts** | Configure alerts for:<br>- Success<br>- Failure<br>- SLA Violation<br>Add **email recipients** if required. |
| **Priority** | Sets execution priority when system load is high:<br>- High<br>- Medium (default)<br>- Low |

11. Configure Include and Exclude Labels:

       - Define rules to:
          - Include PVCs with specific labels
          - Exclude PVCs with specific labels
       - Label matching options:
          - Match all labels
          - Match any selected label

12. Create or Select a Protection Policy.

| Option | Description |
|------|-------------|
| **Create a New Protection Policy** | A Protection Policy defines how backups are handled. Available options include:<br><br>- **DataLock** – Enables tamper‑proof retention (manageable only by Data Security role users)<br>- **Backup Schedule & Retention** – Defines backup frequency, retention period, and type<br>- **Periodic Full Backups** – Schedule recurring full backups<br>- **Extended Retention** – Retain selected snapshots longer<br>- **Quiet Times** – Block new runs during defined windows<br>- **Retry Options** – Configure snapshot retry behavior |
| **Use an Existing Protection Policy** | Go to **Policy Management** \> Select the policy \> Click **Edit** \> Update Kubernetes‑specific settings \> Click **Save**.<br><br>**Note:** Policy changes take effect during the **next scheduled protection run**. |

13. You can assign a new Protection Policy to a Kubernetes object, effective from the next scheduled protection run.

   - In **Backup and Recovery Service**, go to `Sources`, select the Kubernetes source, select the namespace and click Protect.
   - Choose the required policy from the Policy drop-down.
   - Click `Protect` to apply.

14. Enable Auto Protect. Auto Protect ensures automatic inclusion of new namespaces.

| Auto Protect Type | Steps |
|-----------------|-------|
| **Cluster‑Level Auto Protect** | Go to **Protection** \> Select the Kubernetes cluster \> Toggle **Auto Protect** at cluster level \> Click **Protect** to confirm. |

15. Start Protection and Monitor. Click **Protect** to initiate protection. The Protection Service begins backing up selected objects. You can monitor progress at: **Activity** \> **Protection**.

16. IBM Backup and Recovery Service supports application quiescing for stateful Kubernetes workloads. The Supported Quiescing Modes are:
   - Together Mode - Parallel execution (fast)
   - Independent Mode - Parallel volume groups (fastest)
   - Sequential Mode - Ordered execution (most controlled)

Quiescing briefly pauses an application to ensure consistent data for backup. {{site.data.keyword.baas_full_notm}} quiesces stateful Kubernetes workloads before taking PVC snapshots, then automatically unquiesces them once the snapshot is complete.
{: note}

Here is how to configure:

   1. Select namespace \> click **Edit**.
   2. Go to Scripts.
   3. Choose Quiesce Mode.
   4. Create rules using:

     - Pod selector labels
     - Pre snapshot scripts
     - Post snapshot scripts
   5. Configure failure behaviour.
   6. Click **Save**.

Scripts run inside containers and have configurable timeouts.
{: note}

17. Manage Protection Lifecycle.

| Action | Description |
|------|-------------|
| **Pause Future Runs** | Protection \> Select job \> Kebab menu \> **Pause Future Runs** |
| **Resume Protection** | Protection \> Select job \> Kebab menu \> **Resume** |
| **Delete Protection** | **Delete Object Only**: Keeps snapshots until expiry<br>**Delete Object and Snapshots**: Removes object and all backups |

18. If needed, after you create the protection group, click `Run Now` to start the backup immediately. For more information see [Protection group Run Now](#draft).

## Recovering or restoring backup:
{: #recovering-restoring-backup}

1. Go to: `Dashboard` \> `Data Protection` \> `Recoveries` and select `Kubernetes Cluster`.
2. Click **Create New Recovery**.
3. Search for and select one or more namespaces or a Protection Group (latest snapshot is selected by default).
4. (Optional) Click the edit icon to choose a different recovery point (local or cloud snapshot).
5. Click **Next: Recover Options** and choose the **Recovery Location**.
   - Original location - restores only missing or deleted resources.
   - New location - restores the full namespace to another registered cluster.
6. Customize recovery options such as rename(prefix/suffix), task name, PVC Selection, Storage class mapping, Namespace based Resource Inclusion/Exclusion(showing only resources present in the namespace), PVC Inclusion/Exclusion based on labels.
7. (If recovering between incompatible clusters, for example, `OpenShift` \> `Kubernetes`) Enable **Skip cluster compatibility check**.
8. Click **Recovery** to start the restore and monitor progress under `Data Protection` \> `Recoveries`.

After completion, the recovered namespace appears in the destination cluster with the configured name or prefix.
{: note}



## Troubleshooting
{: #data-source-connector-iks-roks-troubleshooting}

### Data Source Connector related issues
{: #dsc-troubleshooting}

#### Data Source Connector Not Appearing After Installation
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

#### DSC Pod Scheduling and Zonal Constraints
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

### Source registration-related issues
{: #registration-troubleshooting}

#### Data Source Connector & Cluster Alignment
{: #dsc-allignment}

To ensure successful source registration and maintain data integrity, please follow these deployment requirements:
1. **Single Installation Policy**: Each Kubernetes or OpenShift cluster must have only one release of the Data Source Connector installed.
2. **Dedicated Cluster Pairing**: Every cluster should be paired with its own unique Data Source Connection.
3. **Connection Integrity**: While it is technically possible to share a single Data Source Connection across multiple clusters, we strongly recommend a **dedicated connection per cluster**. This alignment prevents data inconsistencies and ensures a successful handshake between the Data connector and the source cluster.

#### Incorrect brs-backup-agent image
{: #incorrect-backup-agent}

In case of registration failure, verify that the IBM Cloud Backup and Recovery backup agent is running or not on the Kubernetes/OpenShift cluster.

Check if the backup agent is running:
```bash
kubectl get pods -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
```

If BRS backup agent is not running and getting ImagePullBackOff, then you need to describe the Velero, or DM pods got from previous step and then describe to see what all images you have used at the time of registration.
```bash
kubectl describe pod velero-* -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
kubectl describe pod cohesity-dm-* -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
```

#### Check Health of the Connector
{: #connector-health}

To verify the status of your connection and ensure the connector is operating correctly, follow these steps in the console:
1. Navigate to the Health Status: Go to `System` \> `Data Source Connection`.
2. Verify Connectivity: Locate your specific Data Source Connection and confirm that both the `Connection` and the `Connector` are showing a `Healthy` status.

#### HTTP Connection Errors
{: #http-connection-error}

If you encounter an HTTP connection error on the Data Protection > Sources page during registration, please follow these verification steps to identify and resolve the issue:

1. **Verify Cluster and Resource Integrity**: Confirm that your Kubernetes or OpenShift cluster is active and has not been deleted or scaled down. Ensure that all core resources associated with the integration are intact and fully functional.
2. **Check Health of Velero and Datamover Pods**: Verify that the Velero and datamover pods are in a Running state within the `brs-backup-agent-<uuid>` namespace. If these pods are offline or experiencing frequent restarts, the connector will be unable to process data requests successfully.Make sure the right images are being used for veloro and data mover pods. See [Incorrect brs-backup-agent image](#incorrect-backup-agent).
3. **Verify Data Source Connector Status**:
   1. Ensure Data source connector is healthy. See [Check Health of the Connector](#connector-health).
   2. Ensure the Data Source Connector (DSC) pods are running correctly within the ibm-brs-data-source-connector namespace. Check for any deployment failures, such as restart loops or ImagePullBackOff errors, to ensure the connector is in a healthy state.
4. **Review Network Security Rules**: Inspect your VPC and cluster-level security groups or Network ACLs to ensure no recent firewall rules or security policies are blocking the connector's traffic. While the required communication on ports **443 (communication to cos storage)** and **29991(communication to cohesity cluster)** is typically enabled by default and does not require manual configuration, you should verify that no custom outbound or inbound restrictions have been implemented that might inadvertently disrupt connectivity on these specific ports.

### Alerts troubleshoot
{: #alert-troubleshooting}

#### Monitoring Alerts and Debugging Failed Jobs
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
