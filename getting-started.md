---

copyright:
  years: 2024, 2026
lastupdated: "2026-08-19"

keywords: IBM cloud backup and recovery, VPC, VSI, new user, workload availability, deployment

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Getting started: Backing up a Linux VSI
{: #getting-started-backup-recovery}

{{site.data.keyword.baas_full}} is a managed service that provides backup solutions for various {{site.data.keyword.cloud}} services and customer workloads running on IBM Cloud.

This topic shows you how to get started with {{site.data.keyword.baas_full_notm}} to protect data in a Virtual Server Instance running Ubuntu Linux. You learn to set up a source VSI, install the backup agent, register the source, and create a protection group with a backup policy.

Before you begin, you must have a {{site.data.keyword.baas_full_notm}} instance created and set up with a Data Source Connector. If you haven't already done so, see [Creating a Backup and Recovery service instance](/docs/backup-recovery?topic=backup-recovery-instance-creation) and [Setting up a Backup and Recovery instance](/docs/backup-recovery?topic=backup-recovery-instance-setup).
{: important}

Use code **VPC1000** and get **USD 1,000** in no-charge credits to use toward different offerings on IBM Cloud VPC, including: Confidential computing with Intel® SGX®, Bare Metal for VPC, {{site.data.keyword.baas_full_notm}}, or any Block and File storage or networking components. **Valid until 31 December 2026**.
{: important}

**Boost your Backup Budget**: Unlock **USD 5000** in credits to use toward {{site.data.keyword.baas_full_notm}} services. Apply Code **BUYBRS**.
{: important}

## Before you begin
{: #baas-getting-started}

You need the following to get started with {{site.data.keyword.baas_full_notm}}:
- An [{{site.data.keyword.cloud}} Platform account](https://cloud.ibm.com)
- A {{site.data.keyword.baas_full_notm}} service instance. If you haven't created one yet, see [Creating a Backup and Recovery service instance](/docs/backup-recovery?topic=backup-recovery-instance-creation).
- A Data Source Connector configured for your VPC. See [Setting up a Backup and Recovery instance](/docs/backup-recovery?topic=backup-recovery-instance-setup).
- Access to the {{site.data.keyword.baas_full_notm}} dashboard to create, manage, and monitor backup policies

## Workload availability
{: #baas-compare-workload-availability}

| Workload | VPC VSI | VMware | Regions supported |
| --- | --- | --- | --- |
| File system | Yes  | Yes | - North America:  United States - Washington DC `us-east`, Dallas `us-south`, Canada - Toronto `ca-tor`<br> - South America:  Brazil - São Paulo `br-sao`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`, Osaka`jp-osa`<br> - Australia - Sydney`au-syd`|
| MS SQL | Yes  | Yes | - North America:  United States - Washington DC `us-east`, Dallas `us-south`, Canada - Toronto `ca-tor`<br> - South America:  Brazil - São Paulo `br-sao`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`, Osaka`jp-osa`<br> - Australia - Sydney`au-syd` |
| SAP HANA | Yes  | Yes |  - North America:  United States - Washington DC `us-east`, Dallas `us-south`, Canada - Toronto `ca-tor`<br> - South America:  Brazil - São Paulo `br-sao`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`, Osaka`jp-osa`<br> - Australia - Sydney`au-syd` |
| Oracle | No  | Yes |  - North America:  United States - Washington DC `us-east`, Dallas `us-south`, Canada - Toronto `ca-tor`<br> - South America:  Brazil - São Paulo `br-sao`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`, Osaka`jp-osa`<br> - Australia - Sydney`au-syd` |
| Kubernetes/OpenShift | Yes  | No |  - North America:  United States - Washington DC `us-east`, Dallas `us-south`, Canada - Toronto `ca-tor`<br> - South America:  Brazil - São Paulo `br-sao`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`, Osaka`jp-osa`<br> - Australia - Sydney`au-syd` |
| Db2 | Yes (Linux only)  | No |  - North America:  United States - Washington DC `us-east`, Dallas `us-south`, Canada - Toronto `ca-tor`<br> - South America:  Brazil - São Paulo `br-sao`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`, Osaka`jp-osa`<br> - Australia - Sydney`au-syd` |
{: caption="Workload availability " caption-side="bottom"}


## High-level overview of getting started
{: #baas-backup-linux-basic-user-overview}

This section assumes you're starting from scratch without any existing resources. It guides you through creating new instances; however, you can choose to use your existing resources instead if you have them. If you create new instances, note that this increases your monthly costs. To avoid additional charges, be sure to stop your VSI at the end of the getting started steps.

|Step|Environment|Task|Note|
|---|---|---|---|
|**Step 1:** [Set up a Source Virtual Server Instance (VSI)](#baas-setting-up-source-vsi)| | | |
| Optional | UI or CLI |Set up (or identify) the Source Virtual Server Instance.|Source Virtual Server Instance|
| | UI or CLI |Reserve (or bind) Floating IP| |
|**Step 2:** [Set up a {{site.data.keyword.baas_full_notm}} agent to Source VSI](#baas-setting-up-agent-source-vsi)| | | |
| | UI |Download and install the Linux-based {{site.data.keyword.baas_full_notm}} script installer from the {{site.data.keyword.baas_full_notm}} > Sources page. | Securely copy the agent installer from your local environment to the Source VSI. |
|Optional| Terminal|Verify copying|Note: The installer is not yet executable. |
| |Terminal| Configure the Source Server to manage NFS (minimum requirement). | Install NFS (file handler) |
| |Terminal| Install agent to Source VSI | |
|**Step 3:** [Register a Source VSI into {{site.data.keyword.baas_full_notm}}](#baas-register-source-vsi)| | | |
| |UI|Register the Source VSI in {{site.data.keyword.baas_full_notm}} UI (Source page).|Add Source VSI's Reserved IP address|
|**Step 4:** [Set up a Protection group in {{site.data.keyword.baas_full_notm}}](#baas-set-up-data-protection)| | | |
| |UI|Create and configure a new Protection group in {{site.data.keyword.baas_full_notm}} UI (Protection page)|Physical server > File - Add object.|
|Optional|UI|Verify backup creation and progress on the {{site.data.keyword.baas_full_notm}} instance subpage.| |
{: caption="High-level overview of getting started" caption-side="bottom"}


## Set up the Source VSI
{: #baas-setting-up-source-vsi}

1. Create a Virtual Server Instance for VPC
   For this step, you are creating a new Ubuntu VSI.

   1. Provision a [new VSI instance](https://cloud.ibm.com/infrastructure/provision/vs).
      1. Specify a server name.
      2. In the Image tile, click **Change image** and select any version of Ubuntu linux.
      3. You can change the profile if required; however, the default works.
      4. Create or select an SSH key: you need it to access the virtual server later.
      5. In Networking, select the VPC you deployed the Data Source Connector.
      6. Click **Create**.


Next, make the Source VSI accessible from your local machine by [reserving or binding an existing Floating IP address](https://cloud.ibm.com/infrastructure/network/floatingIPs) to the Source VSI.

## Set up a {{site.data.keyword.baas_full_notm}} agent to the source VSI
{: #baas-setting-up-agent-source-vsi}

1. Download and install the {{site.data.keyword.baas_full_notm}} agent

   1. Now that you have configured the Connector with the token you can close the dialog in the Backup and Recovery Dashboard. Navigate to the `Data Protection` \> `Sources` page.
   2. Click the **Download Agent** button and in the dialog select the Linux - Script installer. This will download the agent to your local machine but we will copy it to your source Virtual Server in the next step.


1. Install the {{site.data.keyword.baas_full_notm}} agent to Source VSI

   1. From the terminal, go to the Downloads folder (`cd ~/Downloads`) or wherever the agent installer from the previous step was downloaded.
   2. Copy the agent installer from your local environment to your source VSI using scp. For example: (Substitute necessary information in the commands.)

      ```sh
      scp -i .ssh/SOURCE_SSH_KEY_NAME Downloads/AGENT_FILE_NAME root@FLOATING_IP_ADDRESS_OF_SOURCE_VSI:/

      ```

   1. (Optional) Verify a copy by going to the root folder (cd) and listing files (ls). You should be able to see the {{site.data.keyword.baas_full_notm}} agent installer.


1. Format and install an agent

   1. Make the installer file an executable with the command:

      ```sh
      chmod +x cohesity_agent_VERSION__linux_x64_installer

      ```

      Make sure to update the VERSION placeholder with the correct version of your installer file. For example, if your file is `cohesity_agent_7.0.1__linux_x64_installer`, use that exact file name.
      {: note}

   2. Installing the agent requires the use of NFS. Install it on your Source VSI by running the command:

      ```sh
      apt install nfs-common

      ```

   3. Install the {{site.data.keyword.baas_full_notm}} agent:

      ```sh
      sudo ./cohesity_agent_VERSION__linux_x64_installer -- --install

      ```

      Replace VERSION with your actual version number (for example, `sudo ./cohesity_agent_7.0.1__linux_x64_installer -- --install`).
      {: note}

   When the installation has been completed successfully, you should see a confirmation in your Terminal window.
   {: note}

## Register the source VSI to {{site.data.keyword.baas_full_notm}}
{: #baas-register-source-vsi}

1. Select type and register Source VSI

   1. Return to the {{site.data.keyword.baas_full_notm}} instance Dashboard, and navigate to the **Data Protection > Sources** page.
   2. Click the **Register Source** button.
   3. Select **Physical** from the dialog.
   4. From the dropdown, select the Data Source Connection that is created earlier.
   5. Click **Continue**.
   6. [Copy the Reserved IP address](https://cloud.ibm.com/infrastructure/compute/vs) of the Source VSI and paste in the modal. This can be found in the IBM Cloud navigation by **Infrastructure → Compute → Virtual Server Instances**.
   7. Click **Complete**.


## Set up Data Protection in {{site.data.keyword.baas_full_notm}}
{: #baas-set-up-data-protection}

1. Set up a new Protection group

   1. In the {{site.data.keyword.baas_full_notm}} instance Dashboard, navigate to the **Data Protection > Protection** page
   2. Click **Protect** and select **Physical server** then **File based**.
   3. In the New Protection popup, **Add Object**, select your **Physical Servers**. It is listed as the reserved IP from the previous step.
   4. Click **Continue**.
   5. Add a name for the Protection Group.
   6. Select the **Bronze** protection policy.
   7. Bronze policy includes: taking a backup every day, retains it for 30 days and a 90-day DataLock. Learn more about [policies](/docs/backup-recovery?topic=backup-recovery-individual-instance-view#policy-mc).
   8. Click **Protect**

1. Verify protection group by navigating to the subpage.

   1. Open the protection group and check that a backup has started.

## Next steps
{: #baas-next-steps}

Now that you are familiar with your {{site.data.keyword.baas_full_notm}} instance from the web-based console, you might be interested in doing a similar workflow from the command line. Check out using the `ibmcloud backup-recovery` command-line utility to create a service instance and interacting with IAM. And you can further use `curl` for accessing Cloud Object Storage directly. [Check out the API overview](https://cloud.ibm.com/apidocs/backup-recovery#br-intro) to get started.

Additionally, explore the [IBM Backup and Recovery Service (BRS) Module](https://registry.terraform.io/modules/terraform-ibm-modules/backup-recovery/ibm/latest){: external} for production-ready infrastructure automation.
