---

copyright:
  years: 2025
lastupdated: "2025-10-23"

keywords: data source connector, iks, roks, cluster

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Register Kubernetes/OpenShift as a data source
{: #data-source-connector-iks-roks}

This information is provided for experimental use only and is subject to change. Only region Washington DC (us-east) is available now for this feature.
{: experimental}

You must create a data source Connector on the same VPC where the Kubernetes/OpenShift cluster is deployed.
{: important}

Located to the right of this page is a summary of key topics that are found on this page. See on this page.
{: note}

## Quick reference to key sections for new users
{: #iks-roks-tutotial-quick-reference}

1. [Before you begin](#baas-getting-started-iks-roks).

2. Prerequisites for backup and restore:
   - You must have a [BRS instance created or create a new one](#data-source-connector-iks-roks-access-instance)
   - [Create or use existing Data source connector](#data-source-connector-iks-roks-create-configure)
   - [Kubernetes/OpenShit cluster should be registered](#data-source-connector-iks-roks-register)

3. Take a backup of Kubernetes/OpenShift cluster:
   - [Access BRS instance](#data-source-connector-iks-roks-access-instance)
   - [Create and configure Data Source Connector](#data-source-connector-iks-roks-create-configure)
   - [Configure and set up network SGs](#data-source-connector-iks-roks-setup-network-sgs-config)
   - [Register source kubernetes/OpenShift cluster](#data-source-connector-iks-roks-register)
   - [Create or schedule a backup](#protecting-namespace-iks-roks)

4. [Restore backup to Kubernetes/OpenShift cluster](#recovering-restoring-backup):
   - [Access BRS instance](#data-source-connector-iks-roks-access-instance)
   - [Create and configure Data Source Connector](#data-source-connector-iks-roks-create-configure)
   - [Configure and set up network SGs](#data-source-connector-iks-roks-setup-network-sgs-config)
   - [Register source kubernetes/OpenShift cluster](#data-source-connector-iks-roks-register)
   - [Restore backup](#recovering-restoring-backup)

5. [Troubleshooting](#data-source-connector-iks-roks-troubleshooting)

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
    4. Click on an instance of your choice to see more details.
    5. Click the **Launch dashboard** button to open the instance.
2. Alternatively, obtain the instance details and credentials to log in.

## Backup requirements for Kubernetes/OpenShift clusters
{: #data-source-connector-iks-roks-backup-requirements}

Your Kubernetes/OpenShift cluster must be registered with the instance to:

- Take backups
- Restore backups

## Backup and Restore compatibility
{: #data-source-connector-iks-roks-brs-comp}

You can backup and restore data on:

- The same Kubernetes/OpenShift cluster
- A different Kubernetes/OpenShift cluster within the same region
- An application running on a Kubernetes/OpenShift cluster
- Kubernetes/OpenShift namespace, PVCs, and more

Clusters must be compatible, especially in terms of storage configuration.
{: note}

## Create or configure a data source Connector
{: #data-source-connector-iks-roks-create-configure}

### Create a data source Connection
{: #data-source-connector-iks-roks-create-data-source-connection}

1. Open the instance dashboard that was discussed in an earlier step on [Accessing your instances](#data-source-connector-iks-roks-access-instance).
2. Navigate to: `Dashboard → System → Data Source Connections`.
3. Click **New Connection**, and copy the Connection Token. You need this token when configuring the Data Source Connector (DSC), and then click **Create**.

### Configure a Data Source Connector
{: #data-source-connector-iks-roks-create-configure}



1. Create a Virtual Server Instance(VSI) on the same VPC as your Kubernetes/OpenShift cluster.
   - Navigate to `Menu ->Infrastructure → Compute → Virtual Server Instances` and then click on the **Create** button.  Also, select the Data Source Connector image from the catalog images and use the [Backup and Recovery Data Source Connector](https://cloud.ibm.com/catalog/1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc/content/content-ibm-backup-recovery-ds-connector-7-1fd41e29-5a36-42ab-81a9-99e4b452f4f3-global) image.

2. Assign a temporary Floating IP to the VSI for configuration purposes.
   - Select the instance that was newly created in the steps before this.
   - Click the **Networking** tab
   - Select `eth0` interface and click the three-dot icon at the last, then click **Edit floating ips.**
   - Click **Attach**, select either the already existing floating ip or reserve a new one, then click **Attach**.

3. Allow an inbound TCP connection on port 443 to access the Data Source Connector UI. This must be added in the security group of the Data Source Connector. The steps to get into the [Data Source Connector security group](#how-to-identify-sc-dcs) can be found here:

4. Access the Data Source Connector UI in your browser at `https://<floating-ip>`. The floating-ip can be found in the steps before this. Default credentials to log in:

    |  Username  |  Password  |
    |----|----|
    |admin|admin (change this after your first login)|

5. Configure your connector using the details shown in the table.

    |  Domain name  |  Connector Name  |  Connection Token  |
    |----|----|----|
    |`cloud.ibm.com`|Any descriptive name|Paste the token obtained from the [Create a Data Source Connection](#data-source-connector-iks-roks-create-data-source-connection) step|

6. When all the required details are filled and ready to register click **Complete**.
7. You are redirected to the connector status page. Wait for a couple of minutes for the connector to be in a healthy state.
8. Clean up these items after configuration:
   - Remove the Floating IP.
   - Remove any temporary port 443 inbound rules added during setup.

## How to identify the Security Group for the Data Source Connector
{: #how-to-identify-sc-dcs}

1. Go to `Menu -> Infrastructure -> Compute ->Virtual server Instances.`
2. Filter the Virtual server instances based on a region where you created Data Source Connector: Washington DC (us-east).
3. Search for a Data Source Connector VSI name, and click on the one you selected.
4. You find tab options for **Overview**, **Networking**, **Storage**, **Monitoring,** and more. Click on the **Networking** tab.
5. Now you see `eth0` as an interface and in the **Security Group** column, click **option.**
6. Click on the name of the Security Group, a new page opens, and now you can add inbound and outbound rules as shown in the rules provided.

## How to identity Security Group for Kubernetes/OpenShift cluster
{: #how-to-get-iks-roks-sec-grp-cluster}

1. Get the Kubernetes/OpenShift [cluster ID](#how-to-get-iks-roks-cluster-id).
2. Click on `Menu -> Infrastructure -> Network -> Security Groups.`
3. Filter by region: Washington DC(us-east) as per your Kubernetes/OpenShift cluster.
4. Search for [cluster ID](#how-to-get-iks-roks-cluster-id), then you see some Security Groups, but select only for `kube-<clusterID>`.

## How to identity VPE gateway for COS S3 direct endpoint
{: #how-to-get-vpe-gateway-cos-s3}

1. Go to `Menu -> Infrastructure -> Networking -> Virtual Private Endpoint Gateways`
2. Search by the VPC name, you get the subset of VPEGs.
3. Select the one that is being used for Cloud Object Storage s3 direct endpoint.

## How to get the Kubernetes/OpenShift cluster ID
{: #how-to-get-iks-roks-cluster-id}

1. Go to `Menu -> Containers -> Clusters.`
2. Filter by location: Washington DC(us-east) and search for your cluster name or ID.
3. Click on cluster and in the **Overview** page look for the Cluster Details section to get the cluster ID.

## How to get the Kubernetes/OpenShift cluster endpoint
{: #how-to-get-iks-roks-endpoint}

1. Go to `Menu -> Containers -> Clusters.`
2. Filter by location: Washington DC(us-east).
3. The endpoint can be found in the **Overview page** in the Networking section where you can find the information for your private and public endpoints.

## How to get security group for VPE gateway of COS endpoint
{: #how-to-get-security-group-vpeg-cos-endpoint}

1. Go to `Menu -> Infrastructure -> Networking -> Virtual Private Endpoint Gateways`
2. Search by the VPC name, you get all the VPEGs created under the same VPC.
3. Select the one that is being used for Cloud Object Storage (service details column) **s3 direct endpoint**(service endpoint column).
4. In the overview tab, enable Permit DNS resolution binding.
5. Go to the Attached resources tab, scroll down to the security group section. You can find the security group that is attached to this VPEG.

## Add rules to Network Security Groups to allow communication
{: #data-source-connector-iks-roks-setup-network-sgs-config}

To register Kubernetes/OpenShift cluster as a source to your {{site.data.keyword.baas_full_notm}} instance, you need to add rules to a data source Connector security group and Kubernetes/OpenShift cluster security group.

To make these components communicate properly, you need to set up rules in the Security Groups that are used by these components:

1. Adding rules to the Data Source Connector (DSC) Security Group:

- Open [data source connector security group.](#how-to-identify-sc-dcs)
- Click on the **Rules** tab to add the following inbound rules:

  To allow communication from BRS backup agent running on the Kubernetes/OpenShift cluster to DSC:

  **Inbound**:

  |  Protocol  |  Source type  |  Source  |  Destination type  |  Destination  |  Value  |
  |----|----|----|----|----|----|
  |TCP|Security group|[`kube-d2mmidpw06t9d10ei1j0`](#how-to-get-iks-roks-sec-grp-cluster)|Any|0.0.0.0/0|Port 3000|

2. Adding rules to the Kubernetes/OpenShift security group `kube-<clusterID>`: Example: `kube-d2mmidpw06t9d10ei1j0`

   ### Adding rules to the Security Group for Kubernetes/OpenShift cluster:
   {: #how-to-get-iks-roks-sec-grp-cluster}

   You need to add the following rules into [`kube-<clusterID>`](#how-to-get-iks-roks-cluster-id) Security Group to allow communication in-between Data Source Connector and the BRS backup agent running on Kubernetes/OpenShift cluster.

    **Inbound**:

    Port should be starting from 31000 to 31000+number of nodes are there or expected in the cluster.  This can conflict as the application might be already using some of these ports and then user need to free those ports from application as of now. For this example, there are three nodes in the cluster and `whiny-granddad-ability-container` is an example of the security group that is used by the data source connector.
    {:note}

    |  Protocol  |  Source type  |  Source  |  Destination type  |  Destination  |  Value  |
    |----|----|----|----|----|----|
    |TCP|Security group|`whiny-granddad-ability-container`|Any|0.0.0.0/0|Ports 31000-31000+|


    **Outbound**:

    To allow the BRS backup agent to communicate to the Data Source Connector

    |  Protocol  |  Source type  |  Source  |  Destination type  |  Destination  |  Value  |
    |----|----|----|----|----|----|
    |TCP|Any|0.0.0.0/0|Security group|`whiny-granddad-ability-container`|Port 3000|

3. Add rules to VPE gateway of COS endpoint security group
   Normally this VPE gateway getting created once a kubernetes/OpenShift cluster created under VPC

    - Open security group associated with [VPE gateway](#how-to-get-security-group-vpeg-cos-endpoint) for COS endpoint.
    - To allow communication from DSC to COS bucket (configured with BRS instance):

   **Inbound**:

    |  Protocol  |  Source type  |  Source  |  Destination type  |  Destination  |  Value  |
    |----|----|----|----|----|----|
    |TCP|Security group|`whiny-granddad-ability-container`|Any|0.0.0.0/0|Port 443|

## Permitting DNS resolution
{: #permitting-dns-resolution}

To allow Data Source Connector, need to copy data to COS endpoint – If your Data Source Connector is on a private network, then you need to follow the following steps to allow Data Source Connector to copy data

   1. Create or identify [VPE gateway](#how-to-get-vpe-gateway-cos-s3) for COS direct endpoints - this normally created VPE gateway while creating Kubernetes/OpenShift cluster under VPC (default networking).
   2. In the Overview tab, enable `Permit DNS resolution binding` to resolve from Data Source Connector.

## How to register an Kubernetes/OpenShift cluster with {{site.data.keyword.baas_full_notm}} service
{: #data-source-connector-iks-roks-register}

1. Open your {{site.data.keyword.baas_full_notm}} instance.
2. Go to: `Dashboard → Data Protection → Sources → Register Source`.
3. Choose the Kubernetes cluster type.
4. Select the previously created [Data Source Connection](#data-source-connector-iks-roks-create-data-source-connection), or create a new connection.
5. Click **Continue**.
6. Provide these details in the form:

   |  Cluster Endpoint  |  Bearer Token |  Kubernetes Distribution  | Images |
   |----|----|----|----|
   |[Private or Public](#how-to-get-iks-roks-endpoint)|[See How to create a Bearer Token](#data-source-connector-iks-roks-create-bearer-token-cluster)|Kubernetes/OpenShift| [Images to be used for Kubernetes/OpenShift registration](#data-source-connector-iks-roks-images-registration) |

7. Click on **Complete** to finish the registration.
8. You are redirected to the list of data sources where you can see the status of your data source registration.

## How to create a Bearer Token for a Kubernetes/OpenShift cluster
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
   kubectl describe secrets -n default iks-token | grep token: | awk '{print $2}' | head -1
   ```
   {: pre}

### Images to be used for a Kubernetes/OpenShift registration(for 7.2.15)
{: #data-source-connector-iks-roks-images-registration}

1. Data mover:  `icr.io/ext/brs/cohesity-datamover:7.2.15-p2`
2. OpenShift plug-in: `icr.io/ext/brs/cohesity-dataprotect-plugin:7.2.15-p2`
3. Velero: `icr.io/ext/brs/velero:7.2.15-p2`
4. S3 plug-in: `icr.io/ext/brs/velero-plugin-for-aws:7.2.15-p2`
5. Data Protect plug-in: `icr.io/ext/brs/velero-plugin-for-openshift:7.2.15-p2`

## Prerequisites for scheduling backups
{: #data-source-connector-iks-roks-prereq-schedule-bak}

1. You must have a [Protection Policy](/docs/backup-recovery?topic=backup-recovery-baas-policy-creation) in place to create or run a backup. You can either:
   - Use an existing Protection Policy, or
   - Create a new user-defined policy
2. You need to create a [Protection Group](/docs/backup-recovery?topic=backup-recovery-protection-groups) that uses one of these policies to perform an incremental or full backup.

## Protecting a namespace or cluster and Scheduling a Backup
{: #protecting-namespace-iks-roks}

1. Go to: `Dashboard → Data Protection → Sources`.
2. Locate your source cluster (based on the registration endpoint)
3. Click on the cluster URL
4. A list of namespaces will appear. Select the namespaces to protect and click Protect
5. Enter the required details: like Protection Group Name, Protection Policy, and Leverage CSI snapshot.
6. Click on **Submit**.

## Recovering or restoring Backup:
{: #recovering-restoring-backup}

1. Go to: `Dashboard → Data Protection → Recoveries→Kubernetes Cluster`.
2. Click **Create New Recovery**.
3. Select one or more namespaces that you want to recover
4. Choose a recovery location:
   - Original location
   - New location
5. (If recovery is from ROKS to IKS) → Skip the version compatibility check (for now due to missing openshift role binding in IKS cluster)
6. Complete the recovery process.

After completion, the recovered namespace will be visible in the cluster with the configured name/prefix.

## Troubleshooting
{: #data-source-connector-iks-roks-troubleshooting}

You can face issues while registering a Kubernetes/OpenShift cluster as a data source, taking backup or restoring backup with BRS instance:

1. Kubernetes/OpenShift cluster and Data Source Connector should be on the same VPC.
   1. This is the prerequisites to work with BRS instance for Kubernetes/OpenShift cluster, Data Source Connector and Kubernetes/OpenShift should be on the same VPC.
   2. How to verify whether these are on the same VPC:
      - Verifying Kubernetes/OpenShift's VPC: Go to `Menu -> Containers -> Clusters ->` Go to your cluster's Overview and under Cluster Details check for the VPC (ex : roks-private-vpc ).
   3. How to verify whether these are on the same VPC:
      - Verifying Kubernetes/OpenShift’s VPC: Go to `Menu -> Containers -> Clusters.`
      - Verifying Data Source Connector’s VPC

2. Security Group rules are missing in required Security Groups that are used to allow communications between the Data Source Connector, Kubernetes/OpenShift cluster, and {{site.data.keyword.baas_full_notm}} instance.
   1. This can be written based on previous steps that are covered in [Configure a Data Source Connector](#data-source-connector-iks-roks-create-configure).

3. Not using correct brs-backup-agent image
   1. In case of registration failure, verify that the BRS backup agent is running or not on the Kubernetes/OpenShift cluster.
   2. Verify that the BRS backup agent is running or not:

      ```sh
      kubectl get pods -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
      ```
      {: pre}

   3. If BRS backup agent is not running and getting ImagePullBackOff, then you need to describe the Velero, or DM pods got from previous step and then describe to see what all images you have used at the time of registration.

      ```sh
      kubectl describe pod velero-* -n `kubectl get ns | grep brs-backup-agent | awk '{print $1}'`
      ```
      {: pre}

4. Unwanted Security Group rules in the Security Groups of the Data Source Connector.
   - You can face connectivity issues in case of [VPC security groups](#how-to-identify-sc-dcs) has redundant rules specifically when Kubernetes/OpenShift cluster deleted and Security Group still containing `Unknown` source, all these rules need to be cleaned up specially from the Data Source Connector Security Group.

5. Check health of the [Data Source Connector](#data-source-connector-iks-roks-create-configure)
   - [Assign the floating IP](#data-source-connector-iks-roks-create-configure)
   - Access by using floating IP and check whether it shows status as `healthy`

6. If you still face this issue, share the logs by running script.
   - [Download the script](https://ibm.ent.box.com/s/god4selhrt0b73rz2lxzf5sk1tzokylq)
   - Change permission to the executable by using `chmod`
   - Configure kubectl or oc command with your cluster where backup/restore failing
