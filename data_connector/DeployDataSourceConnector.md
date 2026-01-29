---

copyright:
  years: 2024, 2026
lastupdated: "2026-01-29"

keywords: backup and recovery, data source connectors,

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Deploy Data Source Connector
{: #deploy_data_source_connector}

To register your data sources with the IBM Cloud Backup and Recovery service, you need to establish connectivity between your source and the service using a Data Source Connection. A Data Source Connection consists of one or more Data Source Connectors, which are virtual machines (VMs) that facilitate the movement of data between your data sources and the IBM Cloud Backup and Recovery service.

You need to install the VM for the Data Source Connector using an installer OVA in your VMware environment, on a vCenter or ESXi host in your environment that has access to your data sources and meets the Data Source Connection system and ﬁrewall requirements.

## Data Source Connector Requirements
{: #data_source_connector_requirements}

Before deploying the Data Source Connector, review and understand the following requirements needed for the VM(s) that you need to provision:

### Supported Sources
{: #supported_sources}

You can deploy Data Source Connectors for the following sources:

- Physical Server
- Microsoft SQL Server
- Oracle Server


### Data Source Connector System Prerequisites
{: #data_source_connector_system_prerequisites}

Ensure that the Data Source Connector VM that you deploy for your Data Source Connection meets the following system requirements:

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
| MS SQL Host | Agent running on the MS SQL Host | 1433 | TCP | Default TCP port for MS SQL instances. Ensure the port is open to allow communication between the MS SQL instance and the Agent. |
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

   3. Copy the `Connection token` and click `Create`. The Connection token is utilized to link or claim the Data Source Connector with the created connection.

4. To deploy the Data Source Connector OVA in your data center:

   1. Log in to your vCenter host.

   2. From the `Hosts and Clusters` tab in the vSphere Web Client, right-click on any cluster that can host your VM and select `Deploy OVF Template`. The Deploy OVF Template wizard opens.

   3. On the `Select an OVF template` page, do one of the following and click Next:

      - Paste the link of the OVA ﬁle you copied in Step 3 (b) in the `URL` ﬁeld.
      - Select `Local ﬁle`, click `UPLOAD FILES`, and browse to the location of the OVA ﬁle you downloaded in step 3 (b).

   4. On the `Select a name and folder` page, enter the following and click `Next`:

      - In the `Virtual machine name` ﬁeld, enter a unique name for your Data Source Connector.
      - In the `Select a location for the Virtual Machine` ﬁeld, select where your VM should reside from the displayed list of inventory locations.

   5. On the `Compute Resources` page, select a compute resource for the Data Source Connector VM and click

#### Next

   6. On the `Review details` page, verify the Data Source Connector information and click `Next`.

   7. On the `Conﬁguration` page, verify `SAAS-CONNECTOR` is selected and click `Next`.

   8. On the `Select storage` page, select a datastore with at least 171 GB of free disk space and click `Next`.

   9. On the `Select networks` page, select a destination network and click `Next`. You can select VLANs from both the `DataNetwork` and the `SecondaryNetwork` ﬁelds. The Data Network is used for communication with Data Source, and the Secondary Network is used for communication with your data sources. Based on your requirements:

      - To deploy the Data Source Connector on a single network, select the same VLAN in both

#### DataNetwork and SecondaryNetwork

- To deploy the Data Source Connector on a dual network, select different VLANs in `DataNetwork` and `SecondaryNetwork`, respectively.
- The Data Source Connector must have dual IP addresses if your data sources are in a private non-routable VLAN.
- Once you have deployed the Data Source Connector on a single network, you cannot modify the Data Source Connector to use a dual network or vice versa.


   10.  On the `Customize template` page, enter the network settings: `Network IP Address`, `Network Netmask`, and `Default Gateway`. If you have selected a different VLAN for the secondary network, enter the `Network IP Address`, `Network Netmask`, and `Default Gateway` for the secondary network, as well. Click `Next`.

- To set the network settings using static IP addresses, manually enter the details in the respective ﬁelds for both DataNetwork and SecondaryNetwork.
- To set the network settings using DHCP, leave the ﬁelds blank in both the DataNetwork and
SecondaryNetwork sections.
- Data Network and Secondary Network must be conﬁgured using the same network conﬁguration method. That is static IP addresses or DHCP.


   11. Review the summary on the `Ready to complete` page and click `Finish`.

   12. Once the VM is created, power it on. After it boots, the services in the Data Source Connector VM (including the UI) can take 4-5 minutes to start.

5. Enter the IP address of the Data Source Connector VM in the address bar of your browser and click `Enter`.

6. On the Data Source Connector's User Interface, enter `admin` in the `Username` and `Password` ﬁelds to log into the Data Source Connector.

On the next screen, you are prompted to change your password. Change your default password and log in again with your new password.

7. Verify the network conﬁguration settings, make necessary changes, and click `Continue`.

8. On the `Data` `Source` `Connector` `Conﬁguration` page, paste the `Connection` `token` in the `Connection` `Claim` `Token` ﬁeld and click `Save`.

It can take another few minutes for the Data Source Connector to authenticate to the IBM Cloud Backup and Recovery Service. Click on the `Data Source Connection` to list the Data Source Connector(s) that are claimed.

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

   - **NOTE:** If the image mentioned above is not available, the User may have to accept an invite. An invite will have been sent to the account admin's email or can be seen in `https://cloud.ibm.com/notifications` by the admin.

   f. Accept the terms and select `Continue`
   h. The `Virtual server for VPC` create page opens, Select appropriate `Location`, `Name`, `Resource Group`, `VPC` and create the Virtual Service.
   - **NOTE:** The `Data Source Connector` needs to have access to the intended workloads. The recommendation is to create it within the same `VPC` as the workloads.


Once the VSI is created and powered on, the services in the Data Source Connector VM (including the UI) can take 4-5 minutes to start.

The initial VSI deployment is your responsibility, after that the OS patching and version upgrades are managed by the Backup and Recovery service.
{: note}

4. Enter the IP address of the Data Source Connector VM in the address bar of your browser and click `Enter`.

5. On the Data Source Connector's User Interface, enter `admin` in the `Username` and `Password` ﬁelds to log into the Data Source Connector.

On the next screen, you are prompted to change your password. Change your default password and log in again with your new password.

6. Verify the network conﬁguration settings, make necessary changes, and click `Continue`.

7. On the `Data` `Source` `Connector` `Conﬁguration` page, paste the `Connection` `token` in the `Connection` `Claim` `Token` ﬁeld and click `Save`.

It can take another few minutes for the Data Source Connector to authenticate to the IBM Cloud Backup and Recovery Service. Click on the `Data Source Connection` to list the Data Source Connector(s) that are claimed.

### VPE Gateways
{: #vpe_gateways}

While the VSI can connect to the Backup and Recovery instance, a VPE gateway will provide a better performance, To create a VPE gateway, follow these steps.

1. From the IBM Cloud catalog, search for `Virtual private endpoint` and click on the `Virtual private endpoint for VPC` tile,
2. Select the appropriate `Location`, `Name`, `Resource Group`, `VPC`
3. Under `Request connection to a service`, Select `IBM Cloud Service`
4. Select `Cloud service offerings` to be `Backup and Recovery` and the appropriate `Cloud service regions`
5. Select the appropriate `Backup and Service` instance in the table below
6. Configure the `Reserved IP` as required and create the VPE Gateway. 
