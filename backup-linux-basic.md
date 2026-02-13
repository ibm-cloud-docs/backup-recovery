---

copyright:
  years: 2024, 2026
lastupdated: "2026-02-12"

keywords: tutorial, virtual server instance, virtual private cloud, virtual private endpoint gateway, floating IP, SSH key

subcollection: backup-recovery

content-type: tutorial

services: backup-recovery

account-plan: paid

completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with {{site.data.keyword.baas_full}} for VPC (VSIs) workloads
{: #backup-linux-tutorial-basic-user}
{: toc-content-type="tutorial"}
{: toc-services="backup-recovery"}
{: toc-completion-time="30m"}

This tutorial shows you how to get started with {{site.data.keyword.baas_full_notm}} to protect data in a Virtual Server Instance running Ubuntu Linux. You’ll deploy and configure a {{site.data.keyword.baas_full_notm}} instance and create a Data Source Connector that links your Virtual Server and the instance. Then you will create a new protection group and apply a custom policy that defines backup schedules and retention. Once the policy is applied to the protection group, your data will be protected.

## High level steps for the tutorial
{: #baas-backup-linux-basic-user-overview}

This tutorial assumes you’re starting from scratch without any existing resources. It guides you through creating new instances, however, you can choose to use your existing resources instead if you have them. If you create new instances, note that this will increase your monthly costs. To avoid additional charges, be sure to stop your VSI at the end of the tutorial.

|Step|Environment|Task|Note|
|---|---|---|---|
|1. Setting up Backup and Recovery Service (BRS) instance| | | |
| | UI | Deploy Backup and Recovery instance (BRS instance) | |
| | UI | Create Data Source Connection (for a VPC) in BRS | Data source connection is a tunnel between the BRS instance and the source server. The Data Source Connector is the thing that forms that tunnel. |
| | UI | Create the Data Source Connector instance for the VSI | Connector VSI instance |
| | UI or CLI | Reserve (or bind) Floating IP for the Connector VSI | |
| | UI | Configure the Connector VSI in the BRS UI | Set up password and token: First, add domain: ‘cloud.ibm.com’ works as a default. Then, paste the claim token from the ‘Create source connection’ modal. |
| | UI or CLI | Deploy (or identify) Virtual Private Endpoint gateway instance | |
|2. Setting up Source Virtual Server Instance (VSI)| | | |
| Optional | UI or CLI |Set up (or identify) the Source Virtual Server Instance|Source Virtual Server Instance|
| | UI or CLI |Reserve (or bind) Floating IP| |
|3. Setting up BRS agent to Source VSI| | | |
| | UI |Download and install the Linux-based BRS script installer from the BRS > Sources page | Securely copy the agent installer from your local environment to the Source VSI |
|Optional| Terminal|Verify copying|Note: Installer is not yet executable. |
| |Terminal| Configure the Source Server to manage NFS (minimum requirement) | Install NFS (file handler) |
| |Terminal| Install agent to Source VSI | |
|4. Registering Source VSI into BRS| | | |
| |UI|Register the Source VSI in BRS UI (Source page)|Add Source VSI’s Reserved IP address|
|5. Setting up Protection group in BRS| | | |
| |UI - need to check plugin|Create and configure a new Protection group in BRS UI (Protection page)|Physical server > File - Add object|
|Optional|UI - need to check plugin|Verify backup up creation and progress on the BRS instance subpage| |

## Before you start
{: #baas-backup-linux-basic-before-you-start}

Ensure that you have what you need to start:

- An account for the [{{site.data.keyword.cloud}} Platform](https://cloud.ibm.com).
- An [instance](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery#baas-provision-instance) of {{site.data.keyword.baas_full_notm}} service is deployed.
- A Data Source Connection that has been created on your instance.
- A [Data Source Connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) (VM or VSI) that is registered with your instance.
  - VM-based sources must use a VM-based data source connector.
  - VSI-based sources must use a VSI-based data source connector.
- A [Source](/docs/backup-recovery?topic=backup-recovery-source-registration-tutorial) (Linux physical host, VM or VSI) that is registered with your instance.
- A [Protection Policy](/docs/backup-recovery?topic=backup-recovery-baas-policy-creation) that defines when and how your Linux data is backed up.
  - Policies can be created when configuring your Protection Group or beforehand on the Data Protection > Policies page.
- A [Linux Agent](/docs/backup-recovery?topic=backup-recovery-linux_agent_install_manage) deployed to the source (installed using the Linux script installer)
  - Agents can be downloaded from your {{site.data.keyword.baas_full_notm}} instance UI on the Data Protection > Sources page.
- Verify that you have proper access to your {{site.data.keyword.baas_full_notm}} instance.
  - Writer (backup-recovery.dashboard.edit) or Manager (backup-recovery.dashboard.edit) privileges to the {{site.data.keyword.baas_full_notm}} Service are needed to create and manage Protection Jobs (i.e. Backup Jobs) in your instance. These privileges can be assigned by your {{site.data.keyword.cloud_notm}} Platform account owner using an Access Group (multiple users) or Access Policy (specific user) tied to your IAM profile. Details on this are included [here](/docs/backup-recovery?topic=backup-recovery-iam&interface=ui).

For VPC (VSIs), additional configurations are needed:

- A VPC deployed where your sources and data source connectors are provisioned.
- A VPE gateway deployed that is tied to your VPC and {{site.data.keyword.baas_full_notm}} instance (optional).
- A Security Group configured with inbound/outbound firewall rules tied to your VPC.
  - These rules are required to backup and restore your data using the {{site.data.keyword.baas_full_notm}} service as they allow traffic to move to/from your VPC to your protected Sources (e.g. Linux, etc.) running as virtual server instances (VSIs).
  - Information on VPCs, Security Groups, Inbound Rules, etc. are listed in the “Resources” section below.
  - A list of required ports is outlined in the link below titled, “Manage firewall ports”.
- A Transit Gateway is needed only if you have multiple sources, connectors, etc. deployed across multiple VPCs that must communicate with each other. For example, if you provision your protected sources in VPC 1 and your data source connectors are in VPC 2, a Transit Gateway must be deployed and both VPCs assigned to it so they can communicate during backup and restore operations.

For VPC (VSIs) and VMware (VMs), also perform the following:

- Open inbound/outbound firewall port rules at the Operating System-layer of each Source (e.g. Linux, Windows and MS SQL, etc.). A list of required ports is outlined in the link below titled, “Manage firewall ports.”

#1. Setting up a Backup and Recovery (BRS) deployment

1. Deploy a Backup and Recovery instance. 

Create a BRS instance link.
INFO: By default your instance is automatically encrypted, optionally, you can bring your own key by supplying the key CRN. 
INFO: To follow best practices, it is recommended to create you BRS instance in the same location as your source server.
Open BRS instance and [Launch Dashboard]. 


2. Create a Data Source Connection in BRS

In this step you will set up a Data Source Connection inside your BRS instance. (Nóra to Don: would be great to use definition tooltips that details what Data Source connection is and a link)
In the BRS Dashboard navigate to System > Data Source Connections and click [New Connection].
Select VPC in the [Deployment Platform] dropdown.
Copy or download the token as you will need this later.
Then click [Create a VPC Data Source Connector] which will open the IBM Cloud catalog in a new tab.


3. Deploy a BRS Data Source Connector (Connector VSI)

In this step you will create a Data Source Connector instance (Connector VSI) for the previous step’s Data Source Connection.
The Data source connector is a Virtual Server deployed using a custom image containing the connector software. The first catalog page shows details about this image. Click [Continue] to progress to the Virtual Server provisioning page.
On the VSI provisioning page:
Set the same location as you set for the BRS instance.
Fill out details and Server configs (you can leave it on the default) 
Create or select an SSH key
Create or select a VPC which will contain the workload you are backing up
[Create virtual server]


Make the Connector VSI accessible from your local machine [Non production only]

Now, reserve or bind an existing Floating IP address to the Connector VSI to allow public access to the Connector configuration UI. 
Create a new security group rule to open the following ports to the connector VSI:
On the VSI instance, navigate to the Networking tab, 
open Security group link and change the inbound rule to “Any”.
Copy and paste the Floating IP in a new browser tab to open the UI. This can take up to 5 minutes to be ready.
It is possible that your browser will flag the IP being insecure due to not using https, continue through this.


Access and configure Connector VSI in BRS UI

On the login page use (username: admin, password: admin) initially, then set up a new user name and password, password requirements are: must be at least 8 characters long and cannot include the word “admin”.
On the Connector Configuration view, enter a Domain name (for example cloud.ibm.com).
Paste the connection claim token from the modal in step 2.1 into the connection claim text input. [from Nóra to Don: When building include link to 2.1. step]
You should not need to change any other configuration parameters.
[Save]


Create a Virtual Private Endpoint gateway (VPE) instance for the BRS instance 

While the Connector VSI can connect to the Backup and Recovery instance, a VPE gateway will provide a better performance. To create a VPE gateway, follow these steps.
Create a VPE instance
Give a service name, select the correct VPC
In the Cloud Service Offering dropdown, select [Backup and Recovery].
Select the previously created BRS instance from the list.
[Create]



#2. Setting up the Source VSI

Create a Virtual Server Instance for VPC

In this tutorial, you will create a new Ubuntu VSI. Follow these steps:
Provision a new VSI instance.
Specify a server name
In the Image tile click [Change image] and select any version of Ubuntu linux.
You can change the profile if required, however the default will work.
Create or select an SSH key - you will need this to access the virtual server later.
In Networking, select the VPC you deployed the Data Source Connector into.
[Create]


Make the Source VSI accessible from your local machine

Reserve or bind an existing Floating IP address to the Source VSI.


#3. Setting up BRS agent to Source VSI

Download and install the BRS agent

Now that you have configured the Connector with the token you can close the dialog in the Backup and Recovery Dashboard. Navigate to the Data Protection > Sources page.
Click the [Download Agent] button and in the dialog select the Linux - Script installer. This will download the agent to your local machine but we will copy it to your source Virtual Server in the next step.


Install the BRS agent to Source VSI

In terminal, go to the Downloads folder (cd/Downloads) or wherever the agent installer from the previous step was downloaded.
Copy the agent installer from your local environment to your source VSI using scp. For example:(Substitute necessary information in the commands.)

scp -i .ssh/SOURCE_SSH_KEY_NAME Downloads/AGENT_FILE_NAME root@FLOATING_IP_ADDRESS_OF_SOURCE_VSI:/

(Optional) Verify copy by going to the root folder (cd) and listing files (ls). You should be able to see the BRS agent installer.


Format and install agent

Make installer file executable with the following command. Make sure to update the VERSION placeholder with the correct version of your installer file:(Substitute necessary information in the commands.)

chmod +x cohesity_agent_VERSION__linux_x64_installer

Installing the agent requires NFS. Install it on your Source VSI by running the following command:

apt install nfs-common

Finally, install the BRS agent:

sudo ./cohesity_agent_VERSION__linux_x64_installer -- --install


Once this has completed installation you should see confirmation in your Terminal window.

#4. Register the Source VSI to the BRS

Select type and register Source VSI

Back in the BRS instance Dashboard, navigate to the Data Protection > Sources page
Click the [Register Source] button.
Select “Physical” from the dialog.
From the dropdown, select the Data Source Connection created earlier.
Click [Continue]
Copy the Reserved IP address of the Source VSI and paste in the modal. This can be found in the IBM Cloud navigation via Infrastructure → Compute → Virtual Server Instances.
[Complete]



#5. Set up Data Protection in BRS

Set up a new Protection group

In the BRS instance Dashboard, navigate to the Data Protection > Protection page
Click [Protect] and select “Physical server” then “File based”
In New Protection popup, “Add Object”, select your “Physical Servers”. It will be listed as the reserved IP from the previous step. 
[Continue]
Add name for Protection Group.
Select the ‘Bronze’ protection policy.
Bronze policy includes: taking a backup every day, retains it for 30 days and a 90 days DataLock. Learn more about policies
[Protect]

Verify protection group by navigating to the subpage. 

Open the protection group and a backup should have started.



## Next steps
{: #bass-linux-backup-next-steps}

Learn how to configure a [Recovery Job](/docs/backup-recovery?topic=backup-recovery-recover-linux-tutorial&interface=ui) to restore the Linux file and folder data that you’ve backed up to your {{site.data.keyword.baas_full_notm}} instance.
