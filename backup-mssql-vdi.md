---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: vpc, mssql, tutorial, vdi, vm

subcollection: backup-recovery

content-type: tutorial

services: backup-recovery

account-plan: paid

completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Backup MS SQL (VDI)
{: #backup-mssql-vdi-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="backup-recovery"}
{: toc-completion-time="15m"}

This tutorial shows how to backup Microsoft SQL Server (VDI-based) data using your {{site.data.keyword.baas_full}} service. This entails choosing the Sources and Objects (i.e. Microsoft SQL Server hosts and their data) to protect, along with naming the Protection Group and assigning a pre-existing or custom Protection Policy that defines your backup schedule, data retention duration and more.

## The Scenario
{: #baas-backup-mssql-vdi-scenario}

The scenario for this tutorial simplifies how to backup Microsoft SQL Server (VDI-based) data using your {{site.data.keyword.baas_full}} service.

## Before you start
{: #baas-backup-mssql-vdi-before-you-start}

Ensure that you have what you need to start:

- An account for the [{{site.data.keyword.cloud}} Platform](https://cloud.ibm.com).
- An [instance](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery#baas-provision-instance) of {{site.data.keyword.baas_full_notm}} service is deployed.
- A Data Source Connection that has been created on your instance.
- A [Data Source Connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) (VM or VSI) that is registered with your instance.
  - VM-based sources must use a VM-based data source connector.
  - VSI-based sources must use a VSI-based data source connector.
- A [Source](/docs/backup-recovery?topic=backup-recovery-source-registration-tutorial) (MS SQL physical host, VM or VSI) that is registered with your instance.
- A [Protection Policy](/docs/backup-recovery?topic=backup-recovery-baas-policy-creation) that defines when and how your MS SQL Server data is backed up.
  - Policies can be created when configuring your Protection Group or beforehand on the Data Protection > Policies page.
- A [Windows Agent](/docs/backup-recovery?topic=backup-recovery-agent-download-install) deployed to the source (installed using the Windows .exe installer).
  - Agents can be downloaded from your {{site.data.keyword.baas_full_notm}} instance UI on the Data Protection > Sources page.
- Verify that you have proper access to your {{site.data.keyword.baas_full_notm}} instance.
  - Writer (backup-recovery.dashboard.edit) or Manager (backup-recovery.dashboard.edit) privileges to the {{site.data.keyword.baas_full_notm}} Service are needed to create and manage Protection Jobs (i.e. Backup Jobs) in your instance. These privileges can be assigned by your {{site.data.keyword.cloud_notm}} Platform account owner using an Access Group (multiple users) or Access Policy (specific user) tied to your IAM profile. Details on this are included [here](/docs/backup-recovery?topic=backup-recovery-iam&interface=ui).
- Ensure you have the proper `sysadmin` SQL privileges for the Windows account running your agent so it can access your MS SQL Server instance. Please see the section below titled, “Requirements for Microsoft SQL Server protection”.

For VPC (VSIs), additional configurations are needed:

- A VPC deployed where your sources and data source connectors are provisioned.
- A VPE gateway deployed that is tied to your VPC and {{site.data.keyword.baas_full_notm}} instance (optional).
- A Security Group configured with inbound/outbound firewall rules tied to your VPC.
  - These rules are required to backup and restore your data using the {{site.data.keyword.baas_full_notm}} service as they allow traffic to move to/from your VPC to your protected Sources (e.g. MS SQL, etc.) running as virtual server instances (VSIs).
  - Information on VPCs, Security Groups, Inbound Rules, etc. are listed in the “Resources” section below.
  - A list of required ports is outlined in the link below titled, “Manage firewall ports”.
- A Transit Gateway is needed only if you have multiple sources, connectors, etc. deployed across multiple VPCs that must communicate with each other. For example, if you provision your protected sources in VPC 1 and your data source connectors in VPC 2, a Transit Gateway must be deployed and both VPCs assigned to it so they can communicate during backup and restore operations.

For VPC (VSIs) and VMware (VMs), also perform the following:

- Open inbound firewall port rules at the Operating System-layer of each Source (e.g. Linux, Windows and MS SQL, etc.). A list of required ports is outlined in the link below titled, “Manage firewall ports”.

## Resources
{: #baas-backup-mssql-tutotial-resources}

1. [Getting started with Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started).
2. [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).
3. [About security groups](/docs/vpc?topic=vpc-using-security-groups).
4. [Setting up a security group for your resource](/docs/vpc?topic=vpc-configuring-the-security-group).
5. [Applying security group rules to source and destination IP addresses](/docs/vpc?topic=vpc-security-groups-rules).
6. [About IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-about).

## Launch your {{site.data.keyword.baas_full_notm}} Instance UI
{: #baas-backup-mssql-vdi-instance-ui}

1. Log into the {{site.data.keyword.cloud_notm}} Platform.
2. Expand the Navigation Menu (four horizontal lines, top left) > select Resource List.
3. Expand Storage > select your {{site.data.keyword.baas_full_notm}} instance.
4. Select Launch dashboard > enter your SSO / account credentials to authenticate.

You are now presented with the UI of your {{site.data.keyword.baas_full_notm}} instance. The next section covers how to back up your MS SQL Server database data.

## Configure your Protection Job (Backup Job)
{: #baas-backup-mssql-vdi-config-protect-job}

The linked sections shown below cover information on protecting MS SQL Standalone (including system DBs), Always On Availability Group (AAG) and Transparent Data Encryption (TDE) database types with your {{site.data.keyword.baas_full_notm}} service instance(s).

**Linked sections 1 thru 6** cover requirements and recommendations for preparing your MS SQL sources for protection.

**Linked section 7 and 8** cover registering your MS SQL Server sources, along with configuring the Protection Group (Sources/Objects, Protection Policy, etc.) and executing jobs. These links include detailed steps on how to accomplish the tasks.

Here is a summary of some of the content discussed in **linked sections 6 and 7**:

To protect an **MS SQL standalone** or **system database**, you must first install the backup and recovery windows .exe agent on the associated SQL Server instance, then register said SQL Server instance as a Source in your {{site.data.keyword.baas_full_notm}} instance. The Protection Group can then be configured where in the Add Objects section you’ll select the aforementioned SQL Server Instance as the source and the desired database(s) to be protected. The Protection Group can then be named and a Protection policy created/assigned to define the backup schedule.

To protect an **MSQL AAG database**, you must first install the backup and recovery windows .exe agent on all SQL Server Instances associated with the Availability Group (i.e. the primary and secondary replicas), then register said SQL Server instances as individual Sources in your {{site.data.keyword.baas_full_notm}} instance. At that point, the Availability Group appears as a registered Source. The Protection Group can then be configured where in the Add Objects section you’ll select the Availability Group as the source and desired databases within the AG to be protected. The Protection Group can then be named and a Protection policy created/assigned to define the backup schedule.

To protect an **MSQL TDE-enabled database**, you must first install the backup and recovery windows .exe agent on the associated SQL Server instance, then register said SQL Server instance as a Source in your {{site.data.keyword.baas_full_notm}} instance. The Protection Group can then be configured where in the Add Objects section you’ll select the aforementioned SQL Server Instance as the source and the desired TDE-enabled database(s) to be protected. The Protection Group can then be named and a Protection policy created/assigned to define the backup schedule.

The MS SQL TDE feature configuration is done within your MS SQL deployment outside of IBM Cloud Backup and Recovery Service. As long as the MS SQL TDE feature is deployed correctly per Microsoft’s documentation / best practices, your IBM Cloud Backup and Recovery service instance should recognize your TDE-enabled databases and protect them just like standard non-TDE databases.
{: note}

TDE-enabled databases can also be contained within an Always On Availability Group, and in that scenario, you would follow the AAG protection group configuration steps to protect them.
{: note}

1. [Requirements for Microsoft SQL Server protection](/docs/backup-recovery?topic=backup-recovery-requirements_for_microsoft_sql_server_protection).
2. [Supported database types for Microsoft SQL Server](/docs/backup-recovery?topic=backup-recovery-supported_database_types_for_microsoft_sql_server).
3. [General recommendations for Microsoft SQL Server backup methods](/docs/backup-recovery?topic=backup-recovery-general_recommendations_for_microsoft_sql_server_backup_methods&interface=ui).
4. [Considerations for Microsoft SQL Server](/docs/backup-recovery?topic=backup-recovery-considerations_for_microsoft_sql_server).
5. [Manage firewall ports](/docs/backup-recovery?topic=backup-recovery-manage_firewall_ports)
6. [Port Requirements - MS SQL Server and Data Source Connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector).
7. [Set up standalone Microsoft SQL Server or Microsoft SQL Server AGs](/docs/backup-recovery?topic=backup-recovery-set_up_standalone_microsoft_sql_server_or_microsoft_sql_server_ags).
8. [Backup Microsoft SQL Server (vdi-based)](/docs/backup-recovery?topic=backup-recovery-backup_microsoft_sql_server_vdi-based).

## Next steps
{: #bass-mssql-vdi-backup-next-steps}

Learn how to configure a [Recovery Job](/docs/backup-recovery?topic=backup-recovery-recover-windows-tutorial&interface=ui) to restore the MS SQL data that you’ve backed up to your {{site.data.keyword.baas_full_notm}} instance.
