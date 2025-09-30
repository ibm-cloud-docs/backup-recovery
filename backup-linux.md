---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-30"

keywords: static website, hosting, tutorial

subcollection: backup-recovery

content-type: tutorial

services: backup-recovery

account-plan: paid

completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Backup file/folder for Linux
{: #backup-linux-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="backup-recovery"}
{: toc-completion-time="30m"}

This tutorial shows how to backup files and folders stored on your physical and/or virtual (VSI or VMware) Linux hosts using the {{site.data.keyword.baas_full}} service. This entails choosing the Sources and Objects (Linux hosts and their data) to protect, along with naming the Protection Group and assigning a pre-existing or custom Protection Policy that defines your backup schedule, data retention duration and more.

## Before you start
{: #baas-backup-linux-before-you-start}

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

## Resources
{: #baas-backup-linux-tutotial-resources}

1. [Getting started with Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started).
2. [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).
3. [About security groups](/docs/vpc?topic=vpc-using-security-groups).
4. [Setting up a security group for your resource](/docs/vpc?topic=vpc-configuring-the-security-group).
5. [Applying security group rules to source and destination IP addresses](/docs/vpc?topic=vpc-security-groups-rules).
6. [About IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-about).

## Launch your {{site.data.keyword.baas_full_notm}} Instance UI
{: #baas-backup-linux-instance}

1. Log into the {{site.data.keyword.cloud}} Platform.
2. Expand the Navigation Menu (four horizontal lines, top left) > select Resource List.
3. Expand Storage > select your IBM Cloud Backup and Recovery instance.
4. Select Launch dashboard > enter your SSO / account credentials to authenticate.

You will now be presented with the UI of your {{site.data.keyword.baas_full_notm}} Instance. The next section will cover how to back up your Linux file and folder data.

## Configure your Protection Job (Backup Job)
{: #baas-config-protect-job}

Please refer to the steps in the following link(s) to accomplish this:
1. [Plan and prepare for physical server protection](/docs/backup-recovery?topic=backup-recovery-plan_and_prepare_for_physical_server_protection)
2. [Manage firewall ports](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector#port_requirements)
3. [Install and Manage the Agent on Linux Servers](/docs/backup-recovery?topic=backup-recovery-linux_agent_install_manage)
4. [Protect a Physical Server (file-based)](/docs/backup-recovery?topic=backup-recovery-protect_a_physical_server_file-based)

This info is relevant to protecting filesystem data within the OS of both physical hosts and Guest VMs or VSIs.
{: note}

## Next steps
{: #bass-linux-backup-next-steps}

Learn how to configure a [Recovery Job](/docs/backup-recovery?topic=backup-recovery-recover-linux-tutorial&interface=ui) to restore the Linux file and folder data that you’ve backed up to your {{site.data.keyword.baas_full_notm}} instance.
