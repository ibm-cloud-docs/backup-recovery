---

copyright:
  years: 2024, 2026
lastupdated: "2026-04-07"

keywords: backup and recovery, data source connectors,

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Deploy Data Source Connector
{: #deploy_data_source_connector}

To register your data sources with the IBM Cloud Backup and Recovery service, you need to establish connectivity between your source and the service by using a Data Source Connection. A Data Source Connection consists of one or more Data Source Connectors that facilitate the movement of data between your data sources and the IBM Cloud Backup and Recovery service.

The deployment method for Data Source Connectors varies based on your source type:

- **For VM-based sources** (Physical Servers, Microsoft SQL Server, Oracle Server): Deploy the Data Source Connector as a virtual machine (VM) using an installer OVA in your VMware environment, on a vCenter or ESXi host that has access to your data sources and meets the system and firewall requirements.

- **For Kubernetes/OpenShift clusters**: Deploy the Data Source Connector using Helm charts directly on your cluster. See [Install Data Source Connector for Kubernetes/OpenShift](#install_data_source_connector_iks_roks) for detailed instructions.

Alternatively, you can also refer to the terraform [IBM Backup & Recovery for IKS/ROKS with Data Source Connector](https://registry.terraform.io/modules/terraform-ibm-modules/iks-ocp-backup-recovery/ibm/latest){: external} module that offers ready-to-use code and examples for integrating the Data Source connector.

## Data Source Connector Requirements
{: #data_source_connector_requirements}

Before deploying the Data Source Connector, review and understand the following requirements that are needed for the VM(s) that you need to provision:

### Supported Sources
{: #supported_sources}

You can deploy Data Source Connectors for the following sources:

- Physical Server
- Microsoft SQL Server
- Oracle Server
- Kubernetes/OpenShift

### Data Source Connector System Prerequisites
{: #data_source_connector_system_prerequisites}

Make sure that the Data Source Connector VM that you deploy for your Data Source Connection meets the following system requirements:

- 4 CPUs
- 16 GB RAM
- 171 GB disk space
- Outbound Internet connection

### Port Requirements
{: #port_requirements}

Ensure that the following ports are open to allow communication between the Data Source Connector(s) and the data sources.

Physical Servers

| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| Data Source Connector | Physical Windows or Linux Server | 50051 | TCP | Required for Backup and Recovery operations. |
| Local Host (Physical Windows or Linux Server) | Local Host (Physical Windows or Linux Server) | 59999 | TCP | Required for local-to-local communication for self-monitoring and debugging purposes. |
{: caption="Physical Servers" caption-side="bottom"}


Microsoft SQL Servers

| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| Data Source Connector | MS SQL Host | 50051 | TCP | Required for Backup and Recovery operations. |
| MS SQL Host | Data Source Connector | 11113,11117 | TCP | Required for Backup and Recovery operations. |
| MS SQL Host | Agent running on the MS SQL Host | 1433 | TCP | Default TCP port for MS SQL instances. Ensure that the port is open to allow communication between the MS SQL instance and the Agent. |
{: caption="Microsoft SQL Servers" caption-side="bottom"}

Oracle Servers

Ensure that the following ports are open to allow communication between the Data Source Connector(s) and Oracle Server:

| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| Data Source Connector | Oracle Server | 50051 | TCP | Required for Backup and Recovery operations. |
| Oracle Server | Data Source Connector | 111, 2049 | TCP | Required for Backup and Recovery operations in Linux servers. |
| Oracle Server | Data Source Connector | 11113, 11117 | TCP | Required for Backup and Recovery operations in Windows servers. |
| Local Host (Physical Windows or Linux Server) | Local Host (Physical Windows or Linux Server) | 59999 | TCP | Required for local-to-local communication for self-monitoring and debugging purposes. |
{: caption="Oracle Servers" caption-side="bottom"}

IBM Backup Service

Ensure that the following ports are open to allow communication between one or more Data Source Connectors and IBM backup service, as well as the IBM Cloud Storage.



| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| Data Source Connector | IBM Backup Service | 443 | TCP | Required for Backup and Recovery operations. |
| Data Source Connector | IBM Backup Service | 29991 | TCP | Required for Backup and Recovery operations. |
| Data Source Connector | IBM Cloud Storage | 443 | TCP | Required for Backup and Recovery operations. |
{: caption="IBM Backup Service" caption-side="bottom"}

Others

| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| Data Source Connector | Internet/Internal | 123,323 | UDP | Required for time.google.com OR internal NTP |
| Data Source Connector | Internet/Internal | 53  | TCP, UDP | Required for DNS or internal DNS |
{: caption="Others" caption-side="bottom"}



## Create a Data Source Connection in VMware
{: #create_a_data_source_connection_in_vmware}

To create a Data Source Connection:

1. In IBM Cloud Backup and Recovery, navigate to `System` \> `Data Source Connections`.

2. Click `New Connection`.

3. In the `Create Data Source Connection` dialog, select the following:

   1. From the `Deployment Platform` drop-down, select `VMware`.

   2. To deploy the Data Source Connector in your data center, click `Copy OVA URL`. The OVA URL is used to deploy the OVA template in VMware vCenter or ESXi.

   3. Copy the `Connection token` and click `Create`. The Connection token is used to link or claim the Data Source Connector with the created connection.

4. To deploy the Data Source Connector OVA in your data center:

   1. Log in to your vCenter host.

   2. From the `Hosts and Clusters` tab in the vSphere Web Client, right-click on any cluster that can host your VM and select `Deploy OVF Template`. The Deploy OVF Template wizard opens.

   3. On the `Select an OVF template` page, do one of the following and click Next:

      - Paste the link of the OVA ﬁle that you copied in Step 3 (b) in the `URL` field.
      - Select `Local ﬁle`, click `UPLOAD FILES`, and browse to the location of the OVA ﬁle you downloaded in step 3 (b).

   4. On the `Select a name and folder` page, enter the following and click `Next`:

      - In the `Virtual machine name` field, enter a unique name for your Data Source Connector.
      - In the `Select a location for the Virtual Machine` field, select where your VM should reside from the displayed list of inventory locations.

   5. On the `Compute Resources` page, select a compute resource for the Data Source Connector VM and click `Next`.

   6. On the `Review details` page, verify the Data Source Connector information and click `Next`.

   7. On the `Conﬁguration` page, verify `SAAS-CONNECTOR` is selected and click `Next`.

   8. On the `Select storage` page, select a datastore with at least 171 GB of free disk space and click `Next`.

   9. On the `Select networks` page, select a destination network and click `Next`. You can select VLANs from both the `DataNetwork` and the `SecondaryNetwork` fields. The Data Network is used for communication with Data Source, and the Secondary Network is used for communication with your data sources. Based on your requirements:

      - To deploy the Data Source Connector on a single network, select the same VLAN in both

#### DataNetwork and SecondaryNetwork

- To deploy the Data Source Connector on a dual network, select different VLANs in `DataNetwork` and `SecondaryNetwork`, respectively.
- The Data Source Connector must have dual IP addresses if your data sources are in a private non-routable VLAN.
- Once you have deployed the Data Source Connector on a single network, you cannot modify the Data Source Connector to use a dual network or vice versa.


   10.  On the `Customize template` page, enter the network settings: `Network IP Address`, `Network Netmask`, and `Default Gateway`. If you have selected a different VLAN for the secondary network, enter the `Network IP Address`, `Network Netmask`, and `Default Gateway` for the secondary network, as well. Click `Next`.

- To set the network settings using static IP addresses, manually enter the details in the respective fields for both DataNetwork and SecondaryNetwork.
- To set the network settings using DHCP, leave the fields blank in both the DataNetwork and
SecondaryNetwork sections.
- Data Network and Secondary Network must be configured using the same network configuration method. That is static IP addresses or DHCP.


   11. Review the summary on the `Ready to complete` page and click `Finish`.

   12. Once the VM is created, power it on. After it boots, the services in the Data Source Connector VM (including the UI) can take 4-5 minutes to start.

5. Enter the IP address of the Data Source Connector VM in the address bar of your browser and click `Enter`.

6. On the Data Source Connector's User Interface, enter `admin` in the `Username` and `Password` ﬁelds to log in to the Data Source Connector.

On the next screen, you are prompted to change your password. Change your default password and log in again with your new password.

7. Verify the network configuration settings, make necessary changes, and click `Continue`.

8. On the `Data` `Source` `Connector` `Conﬁguration` page, paste the `Connection` `token` in the `Connection` `Claim` `Token` ﬁeld and click `Save`.

It can take another few minutes for the Data Source Connector to authenticate to the IBM Cloud Backup and Recovery Service. Click `Data Source Connection` to list the Data Source Connector(s) that are claimed.

## Create a Data Source Connection in VPC
{: #create_a_data_source_connection_in_vpc}

To create a Data Source Connection:

1. In IBM Cloud Backup and Recovery, navigate to `System` \> `Data Source Connections`.

2. Click `New Connection`.

3. In the `Create Data Source Connection` dialog, select the following:

   a. From the `Deployment Platform` drop-down, select `VPC`.

    - **NOTE:** The VPC option will not be available at launch. It is ok to leave the Deployment Platform as VMware

   b. Copy the `Connection token` and click `Create`. The Connection token is utilized to link or claim the Data Source Connector with the created connection.

   c. To deploy the Data Source Connector in your VPC, Go to the IBM Cloud Catalog

   d. Search for `Backup and Recovery`

   e. Select the `Backup and Recovery Data Source Connector` image and click the catalog tile

   - **NOTE:** If the image mentioned earlier is not available, the User might have to accept an invite. An invite will have been sent to the account admin's email or can be seen in `https://cloud.ibm.com/notifications` by the admin.

   f. Accept the terms and select `Continue`
   h. The `Virtual server for VPC` create page opens, Select appropriate `Location`, `Name`, `Resource Group`, `VPC` and create the Virtual Service.
   - **NOTE:** The `Data Source Connector` needs to have access to the intended workloads. The recommendation is to create it within the same `VPC` as the workloads.


Once the VSI is created and powered on, the services in the Data Source Connector VM (including the UI) can take 4-5 minutes to start.

The initial VSI deployment is your responsibility, after that the OS patching and version upgrades are managed by the Backup and Recovery service.
{: note}

4. Enter the IP address of the Data Source Connector VM in the address bar of your browser and click `Enter`.

5. On the Data Source Connector's User Interface, enter `admin` in the `Username` and `Password` fields to log in to the Data Source Connector.

On the next screen, you are prompted to change your password. Change your default password and log in again with your new password.

6. Verify the network configuration settings, make necessary changes, and click `Continue`.

7. On the `Data` `Source` `Connector` `Conﬁguration` page, paste the `Connection` `token` in the `Connection` `Claim` `Token` field and click `Save`.

It can take another few minutes for the Data Source Connector to authenticate to the IBM Cloud Backup and Recovery Service. Click `Data Source Connection` to list the Data Source Connector(s) that are claimed.

### VPE Gateways
{: #vpe_gateways}

While the VSI can connect to the Backup and Recovery instance, a VPE gateway provides a better performance, To create a VPE gateway, follow these steps.

1. From the IBM Cloud catalog, search for `Virtual private endpoint` and click `Virtual private endpoint for VPC` tile.
2. Select the appropriate `Location`, `Name`, `Resource Group`, `VPC`.
3. Under `Request connection to a service`, Select `IBM Cloud Service`.
4. Select `Cloud service offerings` to be `Backup and Recovery` and the appropriate `Cloud service regions`.
5. Select the appropriate `Backup and Service` instance in the table below.
6. Configure the `Reserved IP` as required and create the VPE Gateway.

## Create a data source connection for Kubernetes/OpenShift
{: #create_data_source_connection_iks_roks}

To create a data source connection for Kubernetes or OpenShift clusters, including detailed guidance on choosing the correct deployment platform and managing connections, see [Create a data source connection](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#data-source-connector-iks-roks-create-data-source-connection) in the Kubernetes/OpenShift getting started guide.

The process involves:
1. Selecting the appropriate deployment platform (ROKS VPC, IKS VPC, ROKS classic, or IKS classic) that matches your cluster infrastructure
2. Creating the connection in the {{site.data.keyword.baas_full_notm}} dashboard
3. Copying the provided Helm install command for use in the next step

You can reuse an existing connection for multiple clusters on the same deployment platform.

## Install Data Source Connector on Kubernetes and OpenShift
{: #install_data_source_connector_iks_roks}

The Data Source Connector is deployed as a StatefulSet with 2 replicas (by default) on your Kubernetes or OpenShift cluster. This establishes the communication channel between your cluster and the {{site.data.keyword.baas_full_notm}} service.

### Resource requirements
{: #install_data_source_connector_resource_requirements}

Ensure that your cluster has sufficient CPU and memory resources. The Data Source Connector will consume resources from your cluster nodes. Additional backup agent components (Datamover and Velero) will be deployed later during cluster registration.

**Data Source Connector resource consumption:**

| Component                                   | Deployment Type | CPU Requests | Memory Requests | Total (default 2 replicas) |
|--------------------------------------------|-----------------|--------------|-----------------|----------------------------|
| Data Source Connector        | StatefulSet (2 replicas) | 2 per replica | 5Gi per replica | 4 CPU, 10Gi Memory |

**Note:** Datamover (DaemonSet) and Velero components are deployed automatically during cluster registration and are not part of the initial Data Source Connector installation.

1. Open [IBM Cloud Shell](https://cloud.ibm.com/shell).
2. Identify the source cluster where you want to install the Data Source Connector. This should be the Kubernetes or OpenShift cluster that you want to back up and protect. You must have admin access to this cluster to install the Data Source Connector.

   List the available clusters:

   ```sh
   ibmcloud ks cluster ls
   ```
   {: codeblock}

   From the output, note the cluster name where the Data Source Connector should be deployed. Ensure you have admin privileges for this cluster.

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

   See [Creating an API_KEY](https://cloud.ibm.com/docs/account?topic=account-userapikey&interface=ui#create_user_key) to create a new API_KEY if you don't have an existing one.


5. Retrieve the Helm install command that you copied earlier in the [Create a data source connection](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#data-source-connector-iks-roks-create-data-source-connection) section and update it based on your cluster type.

   **For IKS Classic clusters:**
   
   You must specify a storage class that is available on Classic clusters and disable SCC (Security Context Constraints) as it's specific to OpenShift:

   ```sh
   helm install <k8-app-name> oci://icr.io/ext/brs/brs-ds-connector-chart --version 7.2.18-release-20260226-49768040 --set secrets.registrationToken=<your-registration-token> --set deploymentPlatform.rocp.sccEnabled=false --set volumeClaimTemplate.storageClass=ibmc-block-bronze --namespace ibm-brs-data-source-connector --create-namespace
   ```
   {: codeblock}

   **For IKS VPC clusters:**
   
   The default storage class `ibmc-vpc-block-metro-5iops-tier` is used automatically. Disable SCC as it's specific to OpenShift:

   ```sh
   helm install <k8-app-name> oci://icr.io/ext/brs/brs-ds-connector-chart --version 7.2.18-release-20260226-49768040 --set secrets.registrationToken=<your-registration-token> --set deploymentPlatform.rocp.sccEnabled=false --namespace ibm-brs-data-source-connector --create-namespace
   ```
   {: codeblock}

   **For ROKS Classic clusters:**
   
   You must specify a storage class that is available on Classic clusters. SCC is enabled by default for OpenShift:

   ```sh
   helm install <k8-app-name> oci://icr.io/ext/brs/brs-ds-connector-chart --version 7.2.18-release-20260226-49768040 --set secrets.registrationToken=<your-registration-token> --set volumeClaimTemplate.storageClass=ibmc-block-bronze --namespace ibm-brs-data-source-connector --create-namespace
   ```
   {: codeblock}

   **For ROKS VPC clusters:**
   
   The default storage class `ibmc-vpc-block-metro-5iops-tier` is used automatically. SCC is enabled by default for OpenShift:

   ```sh
   helm install <k8-app-name> oci://icr.io/ext/brs/brs-ds-connector-chart --version 7.2.18-release-20260226-49768040 --set secrets.registrationToken=<your-registration-token> --namespace ibm-brs-data-source-connector --create-namespace
   ```
   {: codeblock}

   The default storage class `ibmc-vpc-block-metro-5iops-tier` is only available on VPC clusters and will not work on Classic clusters. For Classic clusters, use `ibmc-block-bronze`, `ibmc-block-silver`, or `ibmc-block-gold`.
   {: important}

6. Run the appropriate Helm install command in the IBM Cloud Shell based on your cluster type.

7. Check that the Helm release is installed:

   ```sh
   helm list -n ibm-brs-data-source-connector
   ```
   {: codeblock}

   Check the data source connector pods status:

   ```sh
   kubectl get pods -n ibm-brs-data-source-connector
   ```
   {: codeblock}

   ### Customizing the Helm install command
   {: #customizing-helm-command}

   You can customize the Helm install command with the following optional flags:

   - **`--namespace <namespace-name>`**: Specifies the namespace where the Data Source Connector will be deployed. If not specified, the default namespace is used.

   - **`--create-namespace`**: Creates the specified namespace if it doesn't exist. This flag is useful when deploying to a new namespace.

   - **`--set volumeClaimTemplate.storageClass=<storage-class-name>`**: Specifies the StorageClass to use for provisioning the persistent volume for the Data Source Connector. This parameter sets the storage class specifically for the Data Source Connector's persistent volume. This is required for Classic clusters as the default VPC storage class (`ibmc-vpc-block-metro-5iops-tier`) cannot be used on Classic clusters.

   - **`--set replicaCount=<number>`**: Sets the number of Data Source Connector pod replicas. The default value is 3. Adjust this based on your high availability and workload requirements.

   - **`--set fullnameOverride=<name>`**: Assigns a specific name to the Data Source Connector pods, making them easier to identify and manage.

   - **`--set deploymentPlatform.rocp.sccEnabled=false`**: Disables Security Context Constraints (SCC) for the deployment. This flag should be used for IBM Kubernetes Service clusters, as SCC is specific to OpenShift clusters.

   - **`--set nodeSelector.<key>="<value>"`**: Schedules Data Source Connector pods on nodes with the specified label. This is useful when you want to run the connector on dedicated worker nodes.

   - **`--set "tolerations[<index>].key=<key>"`**: Sets the toleration key for node taints. Tolerations allow pods to be scheduled on nodes with matching taints.

   - **`--set "tolerations[<index>].operator=<operator>"`**: Specifies the toleration operator. Common values are `Equal` (exact match) or `Exists` (key exists).

   - **`--set "tolerations[<index>].value=<value>"`**: Sets the toleration value that must match the taint value on the node.

   - **`--set "tolerations[<index>].effect=<effect>"`**: Specifies the taint effect. Common values are `NoSchedule` (prevents scheduling), `PreferNoSchedule` (tries to avoid scheduling), or `NoExecute` (evicts existing pods).

   ### Adding a dedicated worker pool for Data Source Connector
   {: #add-dedicated-worker-pool}

   It's recommended to create a dedicated worker pool for the Data Source Connector. This helps ensure that the connector pods run on a dedicated worker pool with appropriate taints to ensure workload isolation and performance.

   1. [Deploy a dedicated worker pool](https://cloud.ibm.com/docs/containers?topic=containers-add-workers-vpc#vpc_add_pool){: external}.
   2. (Optional) [Add labels to the worker pool](https://cloud.ibm.com/docs/containers?topic=containers-worker-tag-label&interface=ui#worker_pool_labels){: external} if you want to use node selectors for scheduling.
   3. Add taints to the worker pool:

      ```sh
      ibmcloud ks worker-pool taint set \
          --cluster "${CLUSTER_NAME_ID}" \
          --worker-pool "${WORKER_POOL_NAME}" \
          --taint "${TAINT_KEY}=${TAINT_VALUE}:${TAINT_EFFECT}" -f
      ```
      {: codeblock}

   4. Deploy the Data Source Connector to the dedicated worker pool.

   **Example Helm install command:**

   ```sh
   helm install dsc-test oci://icr.io/ext/brs/brs-ds-connector-chart \
      --version 7.2.18-release-20260226-49768040 \
      --namespace ibm-brs-data-source-connector \
      --create-namespace \
      --set secrets.registrationToken=xxx \
      --set fullnameOverride=dsc \
      --set replicaCount=2 \
      --set nodeSelector.dedicated="data-source-connector" \
      --set "tolerations[0].key=dedicated" \
      --set "tolerations[0].operator=Equal" \
      --set "tolerations[0].value=data-source-connector" \
      --set "tolerations[0].effect=NoSchedule"
   ```

## Upgrading the Data Source Connector
{: #upgrade-data-source-connector}

The {{site.data.keyword.baas_full_notm}} service releases updates on a monthly cadence. It is recommended to upgrade your Data Source Connector when new releases become available to ensure you have the latest features, security patches, and bug fixes.

Upgrades for the Data Source Connector are currently manual. Follow these steps to check your current version and upgrade to a newer version:

### Check your current Data Source Connector version
{: #check-current-version}

To find the currently installed version of the Data Source Connector:

```sh
helm list -n ibm-brs-data-source-connector
```
{: codeblock}

This command displays the installed Helm releases in the `ibm-brs-data-source-connector` namespace, including the chart version (APP VERSION column).

Alternatively, you can check the version using kubectl:

```sh
kubectl get statefulset -n ibm-brs-data-source-connector -o jsonpath='{.items[0].spec.template.spec.containers[0].image}'
```
{: codeblock}

This command shows the container image tag, which includes the version number.

### Check for available versions
{: #check-available-versions}

To view all available versions of the Data Source Connector chart:

```sh
helm search repo oci://icr.io/ext/brs/brs-ds-connector-chart --versions
```
{: codeblock}

You can also check the {{site.data.keyword.baas_full_notm}} service dashboard for version update notifications and release notes.

### Upgrade to a newer version
{: #upgrade-to-newer-version}

Once you've identified the target version, use the Helm upgrade command:

```sh
helm upgrade --install <k8-app-name> oci://icr.io/ext/brs/brs-ds-connector-chart --version <new-version> --reuse-values --set secrets.registrationToken=xxx -n ibm-brs-data-source-connector
```
{: codeblock}

Replace `<k8-app-name>` with your release name and `<new-version>` with the target version number.

The `--reuse-values` flag preserves your existing configuration settings during the upgrade.
{: tip}

**Example upgrade command:**

```sh
helm upgrade --install my-dsc oci://icr.io/ext/brs/brs-ds-connector-chart --version 7.2.19-release-20260301-12345678 --reuse-values --set secrets.registrationToken=xxx -n ibm-brs-data-source-connector
```
{: codeblock}

### Verify the upgrade
{: #verify-upgrade}

After upgrading, verify that the new version is running:

```sh
helm list -n ibm-brs-data-source-connector
```
{: codeblock}

Check the pod status to ensure all pods are running with the new version:

```sh
kubectl get pods -n ibm-brs-data-source-connector
```
{: codeblock}

   **Alternative: Using a values.yaml file**

   As an alternative to using multiple `--set` flags, you can use a `values.yaml` file to pass configuration values. This approach is cleaner and easier to manage, especially when dealing with complex configurations.

   1. Create a `custom-values.yaml` file with the following content:

      ```yaml
      nodeSelector:
        dedicated: data-source-connector

      tolerations:
        - key: "dedicated"
          operator: "Equal"
          value: "data-source-connector"
          effect: "NoSchedule"
      ```
      {: codeblock}

   2. Run the Helm install command using the values file:

      ```sh
      helm install dsc oci://icr.io/ext/brs/brs-ds-connector-chart \
        --version 7.2.18-release-20260226-49768040 \
        --namespace ibm-brs-data-source-connector \
        --create-namespace \
        -f custom-values.yaml \
        --set secrets.registrationToken=*******
      ```
      {: codeblock}
