---

copyright:
  years: 2024, 2026
lastupdated: "2026-08-19"

keywords: IBM cloud backup and recovery, instance setup, deployment, data source connector, VPE

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Setting up Data Source Connector for VPC workloads
{: #instance-setup}

This topic guides you through deploying and configuring the Data Source Connector for {{site.data.keyword.baas_full_notm}} to protect VPC workloads including VSIs, databases (MS SQL, SAP HANA, Oracle, Db2), and physical servers. The Data Source Connector is a VSI that establishes connectivity between your workloads and the {{site.data.keyword.baas_full_notm}} instance.
{: shortdesc}

For Kubernetes or OpenShift clusters, see [Register Kubernetes or OpenShift as a data source](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks) instead, as they use a different connector type (Helm chart).
{: note}

## Before you begin
{: #instance-setup-prereqs}

You need the following:
- An [{{site.data.keyword.cloud}} Platform account](https://cloud.ibm.com)
- A {{site.data.keyword.baas_full_notm}} service instance. If you haven't created one yet, see [Creating a Backup and Recovery service instance](/docs/backup-recovery?topic=backup-recovery-instance-creation).
- Appropriate IAM permissions to create VPC resources
- A VPC that contains the workload that you want to back up

## Overview of setup steps
{: #instance-setup-overview}

Setting up {{site.data.keyword.baas_full_notm}} for VPC workloads involves the following steps:

1. [Create a Data Source Connection](#instance-setup-data-source-connection)
2. [Deploy a Data Source Connector (Connector VSI)](#instance-setup-connector-vsi)
3. [Configure the Connector VSI](#instance-setup-configure-connector)
4. [Create a Virtual Private Endpoint gateway](#instance-setup-vpe)

After you complete these steps, you can proceed to register your sources and configure protection groups.

## Create a Data Source Connection
{: #instance-setup-data-source-connection}

A Data Source Connection is a tunnel between the {{site.data.keyword.baas_full_notm}} instance and your source servers. The Data Source Connector is the component that forms that tunnel.

1. In the {{site.data.keyword.baas_full_notm}} Dashboard, navigate to **System > Data Source Connections** and click **New Connection**.
2. Select **VPC** in the **Deployment Platform** dropdown.
3. Copy or download the connection claim token. You need this token in a later step.
4. Click **Create a VPC Data Source Connector**, which opens the IBM Cloud catalog in a new tab.

## Deploy a Data Source Connector (Connector VSI)
{: #instance-setup-connector-vsi}

The Data Source Connector is a Virtual Server that uses a custom image, which contains the connector software.

1. On the catalog page, review the details about the Data Source Connector image and click **Continue** to progress to the Virtual Server provisioning page.
2. On the VSI provisioning page:
   1. Set the same location as you set for the {{site.data.keyword.baas_full_notm}} instance.
   2. Enter a name for your connector instance.
   3. Configure server settings (you can use the default settings).
   4. Create or select an SSH key.
   5. Select the VPC that contains the workload that you are backing up.
   6. Click **Create virtual server**.

## Make the Connector VSI accessible
{: #instance-setup-connector-access}

For non-production environments, you can use a Floating IP for quick access. For production environments, use a VPN connection or private network access instead.
{: note}

1. Reserve a Floating IP address and attach it to the Connector VSI:
   1. Go to [VPC Infrastructure > Floating IPs](https://cloud.ibm.com/infrastructure/network/floatingIPs).
   2. Click **Reserve** to create a new Floating IP.
   3. Select the same location as your Connector VSI.
   4. In the **Bind to** dropdown, select your Connector VSI instance.
   5. Click **Reserve**.
   
   Alternatively, if you already have an unbound Floating IP, you can bind it to your Connector VSI from the Floating IPs page.
   {: tip}

2. Configure security group rules to allow access to the Connector:
   1. Navigate to your Connector VSI instance page.
   2. Go to the **Networking** tab.
   3. Click on the Security group link.
   4. Add an inbound rule to allow traffic (for testing, you can set it to **Any**, but for production, restrict to specific ports and sources).

3. Access the Connector configuration UI:
   1. Copy the Floating IP address from the Floating IPs page or from your VSI's Networking tab.
   2. Paste the IP address in a new browser tab (e.g., `http://YOUR_FLOATING_IP`).
   3. Wait up to 5 minutes for the Connector UI to be ready.
   
   Your browser may flag the IP as insecure due to not using HTTPS. Continue through this warning.
   {: note}

## Configure the Connector VSI
{: #instance-setup-configure-connector}

1. From the Connector login page, use **username: admin** and **password: admin** to start, then set up a new username and password.
   
   Password requirements: must be at least 8 characters long and cannot include the word **admin**.
   {: note}

2. From the Connector Configuration view, enter a Domain name (for example, `cloud.ibm.com`).
3. Paste the connection claim token from the Data Source Connection creation step into the connection claim text input.
4. You should not need to change any other configuration parameters.
5. Click **Save**.

## Create a Virtual Private Endpoint gateway
{: #instance-setup-vpe}

While the Connector VSI can connect to the Backup and Recovery instance, a VPE gateway provides better performance.

1. Create a [VPE instance](https://cloud.ibm.com/infrastructure/provision/endpointGateway).
2. Enter a service name and select the correct VPC.
3. In the **Cloud Service Offering** dropdown, select **Backup and Recovery**.
4. Select your {{site.data.keyword.baas_full_notm}} instance from the list.
5. Click **Create**.

## Next steps
{: #instance-setup-next-steps}

Now that you have set up your {{site.data.keyword.baas_full_notm}} infrastructure, you can proceed to:

1. Install backup agents on your source servers (if applicable)
2. Register your sources in the {{site.data.keyword.baas_full_notm}} dashboard
3. Create protection groups and policies

For detailed instructions, see:
- [Getting started with Backup and Recovery for VPC (VSIs) workloads](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery)
- [Agent download and installation](/docs/backup-recovery?topic=backup-recovery-agent-download-install)
- [Source registration](/docs/backup-recovery?topic=backup-recovery-registration)
- [Protection groups](/docs/backup-recovery?topic=backup-recovery-protection-groups)
