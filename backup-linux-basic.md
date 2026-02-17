---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-17"

keywords: tutorial, virtual server instance, virtual private cloud, virtual private endpoint gateway, floating IP, SSH key, first-time user

subcollection: backup-recovery

content-type: tutorial

services: backup-recovery

account-plan: paid

completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Protecting data with {{site.data.keyword.baas_full}} for VPC (VSIs) workloads
{: #backup-linux-tutorial-basic-user}
{: toc-content-type="tutorial"}
{: toc-services="backup-recovery"}
{: toc-completion-time="30m"}

This tutorial shows you how to get started with {{site.data.keyword.baas_full_notm}} to protect data in a Virtual Server Instance that is running Ubuntu Linux. The intended audience is first-time users of the service where you learn to deploy and configure a {{site.data.keyword.baas_full_notm}} instance, and create a Data Source Connector that links your Virtual Server and the instance. Then you can create a new protection group and apply a custom policy that defines backup schedules and retention. Once the policy is applied to the protection group, your data is protected.

## High-level steps for the tutorial
{: #baas-backup-linux-basic-user-overview}

This tutorial assumes you’re starting from scratch without any existing resources. It guides you through creating new instances; however, you can choose to use your existing resources instead if you have them. If you create new instances, note that this increases your monthly costs. To avoid additional charges, be sure to stop your VSI at the end of the tutorial.

|Step|Environment|Task|Note|
|---|---|---|---|
|[Setting up a {{site.data.keyword.baas_full_notm}} instance](#baas-setting-up-deployment)| | | |
| | UI | Deploy your {{site.data.keyword.baas_full_notm}} instance | |
| | UI | Create a Data Source Connection (for a VPC) in {{site.data.keyword.baas_full_notm}} | Data source connection is a tunnel between the {{site.data.keyword.baas_full_notm}} instance and the source server. The Data Source Connector is the thing that forms that tunnel. |
| | UI | Create the Data Source Connector instance for the VSI | Connector VSI instance |
| | UI or CLI | Reserve (or bind) Floating IP for the Connector VSI | |
| | UI | Configure the Connector VSI in the {{site.data.keyword.baas_full_notm}} UI | Set up password and token: First, add the domain: ‘cloud.ibm.com’ works as a default. Then, paste the claim token from the ‘Create source connection’ modal. |
| | UI or CLI | Deploy (or identify) a Virtual Private Endpoint gateway instance | |
|[Setting up Source Virtual Server Instance (VSI)](#baas-setting-up-source-vsi)| | | |
| Optional | UI or CLI |Set up (or identify) the Source Virtual Server Instance|Source Virtual Server Instance|
| | UI or CLI |Reserve (or bind) Floating IP| |
|[Setting up {{site.data.keyword.baas_full_notm}} agent to Source VSI](#baas-setting-up-agent-source-vsi)| | | |
| | UI |Download and install the Linux-based {{site.data.keyword.baas_full_notm}} script installer from the {{site.data.keyword.baas_full_notm}} > Sources page | Securely copy the agent installer from your local environment to the Source VSI |
|Optional| Terminal|Verify copying|Note: The installer is not yet executable. |
| |Terminal| Configure the Source Server to manage NFS (minimum requirement) | Install NFS (file handler) |
| |Terminal| Install agent to Source VSI | |
|[Registering Source VSI into {{site.data.keyword.baas_full_notm}}](#baas-register-source-vsi)| | | |
| |UI|Register the Source VSI in {{site.data.keyword.baas_full_notm}} UI (Source page)|Add Source VSI’s Reserved IP address|
|[Setting up Protection group in {{site.data.keyword.baas_full_notm}}](#baas-set-up-data-protection)| | | |
| |UI - need to check plug-in|Create and configure a new Protection group in {{site.data.keyword.baas_full_notm}} UI (Protection page)|Physical server > File - Add object|
|Optional|UI - need to check plugin|Verify backup up creation and progress on the {{site.data.keyword.baas_full_notm}} instance subpage| |
{: caption="High-level steps for the tutorial" caption-side="bottom"}



## Setting up a {{site.data.keyword.baas_full_notm}} deployment
{: #baas-setting-up-deployment}
{: step}

1. Deploy a Backup and Recovery instance.

   1. Create a {{site.data.keyword.baas_full_notm}} [instance](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery#baas-provision-instance).
      1. Information: By default your instance is automatically encrypted. Optionally, you can bring your own key by supplying the key CRN.
      1. Information: When following best practices, it is recommended that you create your {{site.data.keyword.baas_full_notm}} instance in the same location as your source server.
   2. Open your {{site.data.keyword.baas_full_notm}} instance and **Launch Dashboard**.


1. Create a Data Source Connection in {{site.data.keyword.baas_full_notm}}

   For this step, you are setting up a Data Source Connection from inside your {{site.data.keyword.baas_full_notm}} instance.

   1. In the {{site.data.keyword.baas_full_notm}} Dashboard, navigate to **System > Data Source Connections** and click **New Connection**.
   2. Select VPC in the **Deployment Platform** dropdown.
   3. Copy or download the token as you need this later.
   4. Then click **Create a VPC Data Source Connector**, which opens the IBM Cloud catalog in a new tab.


1. Deploy a {{site.data.keyword.baas_full_notm}} Data Source Connector (Connector VSI)

   In this step, you to need to create a Data Source Connector instance (Connector VSI) for the previous step’s Data Source Connection.

   1. The Data source connector is a Virtual Server deployed that uses a custom image, which contains the connector software. The first catalog page shows the details about this image. Click **Continue** to progress to the Virtual Server provisioning page.
   2. On the VSI provisioning page:
      1. Set the same location as you set for the {{site.data.keyword.baas_full_notm}} instance.
      2. Enter details and Server configs (you can leave it on the default).
      3. Create or select an SSH key.
      4. Create or select a VPC, which contains the workload that you are backing up.
      5. Click **Create virtual server**.


1. Make the connector for VSI accessible from your local machine [Non production only]

   1. [Reserve or bind an existing Floating IP address](https://cloud.ibm.com/infrastructure/network/floatingIPs) to the Connector VSI to allow public access to the Connector configuration UI.
   2. Create a new security group rule to open the following ports to the VSI connector:
      1. On the VSI instance, navigate to the Networking tab,
      2. Open Security group link and change the inbound rule to **Any**.
   3. Copy and paste the Floating IP in a new browser tab to open the UI. This can take up to five minutes to be ready.
   4. It is possible that your browser flags the IP being insecure due to not using https, continue through this.


1. Access and configure Connector VSI in {{site.data.keyword.baas_full_notm}} UI

   1. From the login page, use **username: admin**, **password: admin** to start, then set up a new username and password. Password requirements are: must be at least 8 characters long and cannot include the word **admin**.
   2. From the Connector Configuration view, enter a Domain name (for example, cloud.ibm.com).
   3. Paste the connection claim token from the modal in a previous step into the connection claim text input.
   4. You should not need to change any other configuration parameters.
   5. Click **Save**.


1. Create a Virtual Private Endpoint gateway (VPE) instance for the {{site.data.keyword.baas_full_notm}} instance

   While the Connector VSI can connect to the Backup and Recovery instance, a VPE gateway provides a better performance. To create a VPE gateway, follow these steps.

   1. Create a [VPE instance](https://cloud.ibm.com/infrastructure/provision/endpointGateway).
   2. Give a service name, select the correct VPC.
   3. In the Cloud Service Offering dropdown, select **Backup and Recovery**.
   4. Select the previously created {{site.data.keyword.baas_full_notm}} instance from the list.
   5. Click **Create**.


## Setting up the Source VSI
{: #baas-setting-up-source-vsi}
{: step}

1. Create a Virtual Server Instance for VPC
   For this step, you are creating a new Ubuntu VSI.

   1. Provision a [new VSI instance](https://cloud.ibm.com/infrastructure/provision/vs).
      1. Specify a server name.
      2. In the Image tile, click **Change image** and select any version of Ubuntu linux.
      3. You can change the profile if required; however, the default works.
      4. Create or select an SSH key: you need it to access the virtual server later.
      5. In Networking, select the VPC you deployed the Data Source Connector.
      6. Click **Create**.


Next, make the Source VSI accessible from your local machine by [reserve or bind an existing Floating IP address](https://cloud.ibm.com/infrastructure/network/floatingIPs) to the Source VSI.

## Setting up a {{site.data.keyword.baas_full_notm}} agent to the source VSI
{: #baas-setting-up-agent-source-vsi}
{: step}

1. Download and install the {{site.data.keyword.baas_full_notm}} agent

   1. Now that you have configured the Connector with the token you can close the dialog in the Backup and Recovery Dashboard. Navigate to the Data Protection > Sources page.
   2. Click the **Download Agent** button and in the dialog select the Linux - Script installer. This will download the agent to your local machine but we will copy it to your source Virtual Server in the next step.


1. Install the {{site.data.keyword.baas_full_notm}} agent to Source VSI

   1. From the terminal, go to the Downloads folder (cd/Downloads) or wherever the agent installer from the previous step was downloaded.
   2. Copy the agent installer from your local environment to your source VSI using scp. For example:(Substitute necessary information in the commands.)

   ```sh
   scp -i .ssh/SOURCE_SSH_KEY_NAME Downloads/AGENT_FILE_NAME root@FLOATING_IP_ADDRESS_OF_SOURCE_VSI:/

   ```

   1. (Optional) Verify a copy by going to the root folder (cd) and listing files (ls). You should be able to see the {{site.data.keyword.baas_full_notm}} agent installer.


1. Format and install an agent

   1. Make the installer file an executable with the command:

      ```sh
      chmod +x cohesity_agent_VERSION__linux_x64_installer

      ```

      Make sure to update the VERSION placeholder with the correct version of your installer file:(Substitute necessary information in the commands.)
      {: note}

   2. Installing the agent requires the use of NFS. Install it on your Source VSI by running the command:

      ```sh
      apt install nfs-common

      ```

   3. Install the {{site.data.keyword.baas_full_notm}} agent:

      ```sh
      sudo ./cohesity_agent_VERSION__linux_x64_installer -- --install

      ```

   When the installation has been completed successfully, you should see a confirmation in your Terminal window.
   {: note}

## Register the source VSI to {{site.data.keyword.baas_full_notm}}
{: #baas-register-source-vsi}
{: step}

1. Select type and register Source VSI

   1. Return to the {{site.data.keyword.baas_full_notm}} instance Dashboard, and navigate to the Data Protection > Sources page.
   2. Click the **Register Source** button.
   3. Select **Physical** from the dialog.
   4. From the dropdown, select the Data Source Connection that is created earlier.
   5. Click **Continue**.
   6. [Copy the Reserved IP address](https://cloud.ibm.com/infrastructure/compute/vs) of the Source VSI and paste in the modal. This can be found in the IBM Cloud navigation by **Infrastructure → Compute → Virtual Server Instances**.
   7. Click **Complete**.


## Set up Data Protection in {{site.data.keyword.baas_full_notm}}
{: #baas-set-up-data-protection}
{: step}

1. Set up a new Protection group

   1. In the {{site.data.keyword.baas_full_notm}} instance Dashboard, navigate to the **Data Protection > Protection** page
   2. Click **Protect** and select **Physical server** then **File based**
   3. In the New Protection popup, **Add Object**, select your **Physical Servers**. It is listed as the reserved IP from the previous step.
   4. Click **Continue**
   5. Add a name for the Protection Group.
   6. Select the **Bronze** protection policy.
   7. Bronze policy includes: taking a backup every day, retains it for 30 days and a 90 days DataLock. Learn more about [policies](/docs/backup-recovery?topic=backup-recovery-individual-instance-view#policy-mc).
   8. Click **Protect**

1. Verify protection group by navigating to the subpage.

   1. Open the protection group and check that a backup has started.
