---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-30"

keywords: source, registration, getting started, basics

subcollection: backup-recovery

ontent-type: tutorial

services: backup-recovery

account-plan: paid

completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Source registration
{: #source-registration-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="backup-recovery"}
{: toc-completion-time="30m"}

A source must be registered with your {{site.data.keyword.baas_full}} service instance before it can be protected (i.e. backed up). Sources can be physical and/or virtual (VSI or VMware) hosts running Linux, Windows Server and MS SQL Server to name a few. Support for additional operating systems and applications will be available in the future.

## The Scenario
{: #source-registration-scenario}

The scenario for this tutorial simplifies how a source must be registered with your {{site.data.keyword.baas_full}} service instance before it can be protected (i.e. backed up).

## Before you start
{: #source-registration-before-you-start}

Ensure that you have what you need to start:

- An account for the [{{site.data.keyword.cloud}} Platform](https://cloud.ibm.com).
- An [instance](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery#baas-provision-instance) of {{site.data.keyword.baas_full_notm}} service is deployed.
- A Data Source Connection that has been created on your instance.
- A [Data Source Connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) (VM or VSI) that is registered with your instance.
  - VM-based sources must use a VM-based data source connector.
  - VSI-based sources must use a VSI-based data source connector.
- An Agent deployed to the source
  - Agents can be downloaded from your {{site.data.keyword.baas_full_notm}} instance UI on the Data Protection > Sources page.
  - The Linux Agent is installed using the [Linux Script installer](/docs/backup-recovery?topic=backup-recovery-install-and-manage-the-agent-on-linux-servers).
  - The Windows Agent is installed using the [Windows .exe installer](/docs/backup-recovery?topic=backup-recovery-install_and_manage_the_agent_on_windows_servers), which protects both Windows Server (e.g. for File and Folder), and MS SQL database source types.
- Verify that you have proper access to your {{site.data.keyword.baas_full_notm}} instance.
  - Writer (backup-recovery.dashboard.edit) or Manager (backup-recovery.dashboard.edit) privileges to the {{site.data.keyword.baas_full_notm}} Service are needed to create and manage Protection Jobs (i.e. Backup Jobs) in your instance. These privileges can be assigned by your {{site.data.keyword.cloud_notm}} Platform account owner using an Access Group (multiple users) or Access Policy (specific user) tied to your IAM profile. Details on this are included [here](/docs/backup-recovery?topic=backup-recovery-iam-docs-template&interface=ui).

For VPC (VSIs), additional configurations are needed:

- A VPC deployed where your sources and data source connectors are provisioned.
- A VPE gateway deployed that is tied to your VPC and {{site.data.keyword.baas_full_notm}} instance (optional).
- A Security Group configured with inbound/outbound firewall rules tied to your VPC.
  - These rules are required to backup and restore your data using the {{site.data.keyword.baas_full_notm}} service as they allow traffic to move to/from your VPC to your protected Sources (e.g. Linux, Windows, and MS SQL etc.) running as virtual server instances (VSIs).
  - Information on VPCs, Security Groups, Inbound Rules, etc. are listed in the “Resources” section below.
  - A list of required ports is outlined in the link below titled, “Manage firewall ports.”
- A Transit Gateway is needed only if you have multiple sources, connectors, etc. deployed across multiple VPCs that must communicate with each other. For example, if you provision your protected sources in VPC 1 and your data source connectors are in VPC 2, a Transit Gateway must be deployed and both VPCs assigned to it so they can communicate during backup and restore operations.

For VPC (VSIs) and VMware (VMs), also perform the following:

- Open inbound firewall port rules at the Operating System-layer of each Source (e.g. Linux, Windows and MS SQL, etc.). A list of required ports is outlined in the link below titled, “Manage firewall ports.”

## Resources
{: #source-registration-tutotial-resources}

1. [Getting started with Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started).
2. [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).
3. [About security groups](/docs/vpc?topic=vpc-using-security-groups).
4. [Setting up a security group for your resource](/docs/vpc?topic=vpc-configuring-the-security-group).
5. [Applying security group rules to source and destination IP addresses](/docs/vpc?topic=vpc-security-groups-rules).
6. [About IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-about).

## {{site.data.keyword.baas_full_notm}} Instance UI
{: #source-registration-instance}

1. Log into the {{site.data.keyword.cloud_notm}} Platform.
2. Expand the Navigation Menu (four horizontal lines, top left) > select Resource List.
3. Expand Storage > select your {{site.data.keyword.baas_full_notm}} instance.
4. Select Launch dashboard > enter your SSO / account credentials to authenticate.

You will now be presented with the UI of your {{site.data.keyword.baas_full_notm}} Instance. The next section will cover how to register a source.

## Registering a source
{: #baas-config-protect-job}

Please refer to the steps in the following link(s) to accomplish this:

1. Review the list of [supported software](/docs/backup-recovery?topic=backup-recovery-supported_software).
2. Create a [data source connection](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) and register your data source connector.
3. Plan and prepare for registering your source.
   1. [Physical Servers](/docs/backup-recovery?topic=backup-recovery-plan_and_prepare_for_physical_server_protection) (includes Linux and Windows Physical Hosts, VMs & VSIs).
   2. [MS SQL Server](/docs/backup-recovery?topic=backup-recovery-requirements_for_microsoft_sql_server_protection)
   3. Manage firewall ports - [Linux](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector#port_requirements)
   4. Manage firewall ports – [Windows Server](/docs/backup-recovery?topic=backup-recovery-install_and_manage_the_agent_on_windows_servers)
   5. Manage firewall ports – [MS SQL Server](/docs/backup-recovery?topic=backup-recovery-requirements_for_microsoft_sql_server_protection)
4. Install and manage the agents.
   1. [Linux](/docs/backup-recovery?topic=backup-recovery-install-and-manage-the-agent-on-linux-servers)
   2. [ Windows Server and MS SQL Server](/docs/backup-recovery?topic=backup-recovery-install_and_manage_the_agent_on_windows_servers)
5. Register or edit your source.
   1. [Physical Server](/docs/backup-recovery?topic=backup-recovery-ensure-adequate-privileges-overview) (includes Physical Hosts, VMs and/or VSIs)
   2. [MS SQL Server](/docs/backup-recovery?topic=backup-recovery-set_up_standalone_microsoft_sql_server_or_microsoft_sql_server_ags)

When registering an MS SQL Server standalone database, you must follow the instructions in the link above to register the SQL Server Instance the database is provisioned on. When registering a database belonging to an MS SQL Server Always On Availability Group (AAG), you must register each SQL Server instance replica (both primary and secondary) that is associated with the Availability Group. As you start registering the replicas, the Availability Group will appear in the list of registered sources.
{: note}
