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

# Recover MS SQL
{: #recover-mssql-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="backup-recovery"}
{: toc-completion-time="30m"}

This tutorial shows how to recover Microsoft SQL Server data stored on your {{site.data.keyword.baas_full}} service back to a target Source (i.e. Microsoft SQL Server hosts). This entails choosing the specific files and/or folders to recover, the snapshot to restore from, and more.

## The Scenario
{: #baas-recover-mssql-scenario}

The scenario for this tutorial simplifies how to recover Microsoft SQL Server data stored on your {{site.data.keyword.baas_full}} service back to a target Source (i.e. Microsoft SQL Server).

## Before you start
{: #baas-recover-mssql-before-you-start}

Ensure that you have what you need to start:

- An account for the [{{site.data.keyword.cloud}} Platform](https://cloud.ibm.com).
- An [instance](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery#baas-provision-instance) of {{site.data.keyword.baas_full_notm}} service is deployed.
- A Data Source Connection that has been created on your instance.
- A [Data Source Connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) (VM or VSI) that is registered with your instance.
  - VM-based sources must use a VM-based data source connector.
  - VSI-based sources must use a VSI-based data source connector.
- A [Source](/docs/backup-recovery?topic=backup-recovery-source-registration-tutorial) (MS SQL physical host, VM or VSI) that is registered with your instance.
- A [Windows Agent](/docs/backup-recovery?topic=backup-recovery-agent-download-install) deployed to the source (installed using the Windows .exe installer).
  - Agents can be downloaded from your {{site.data.keyword.baas_full_notm}} instance UI on the Data Protection > Sources page.
- Verify that you have proper access to your {{site.data.keyword.baas_full_notm}} instance.
  - Writer (backup-recovery.dashboard.edit) or Manager (backup-recovery.dashboard.edit) privileges to the {{site.data.keyword.baas_full_notm}} Service are needed to create and manage Protection Jobs (i.e. Backup Jobs) in your instance. These privileges can be assigned by your {{site.data.keyword.cloud_notm}} Platform account owner using an Access Group (multiple users) or Access Policy (specific user) tied to your IAM profile. Details on this are included [here](/docs/backup-recovery?topic=backup-recovery-iam&interface=ui).
- Ensure you have the proper `sysadmin` SQL privileges for the Windows account running your agent so it can access your MS SQL Server instance. Please see the section below titled, “Requirements for Microsoft SQL Server protection”.
- A Protection Job run (i.e. backup) has already been successfully run that created a snapshot you can restore from.

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
{: #baas-recover-mssql-tutotial-resources}

1. [Getting started with Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started).
2. [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).
3. [About security groups](/docs/vpc?topic=vpc-using-security-groups).
4. [Setting up a security group for your resource](/docs/vpc?topic=vpc-configuring-the-security-group).
5. [Applying security group rules to source and destination IP addresses](/docs/vpc?topic=vpc-security-groups-rules).
6. [About IBM Cloud Transit Gateway](/docs/transit-gateway?topic=transit-gateway-about).

## Launch your {{site.data.keyword.baas_full_notm}} Instance UI
{: #baas-recover-mssql-instance}

1. Log into the {{site.data.keyword.cloud_notm}} Platform.
2. Expand the Navigation Menu (four horizontal lines, top left) > select Resource List.
3. Expand Storage > select your {{site.data.keyword.baas_full_notm}} instance.
4. Select Launch dashboard > enter your SSO / account credentials to authenticate.

You are now presented with the UI of your {{site.data.keyword.baas_full_notm}} instance. The next section covers how to restore your MS SQL Server database data.

## Configure your Recovery Task
{: #baas-recover-mssql-config-protect-job}

The linked sections listed below cover information on restoring protected MS SQL Standalone (including system DBs), Always On Availability Group (AAG) and Transparent Data Encryption (TDE) database types with your IBM Cloud Backup and Recovery Service instance(s).

Some key considerations to keep in mind when recovering the different database types:

For **MS SQL standalone** or **system databases**, you can restore to the original SQL Server instance or a new one. If you choose to restore to the original SQL Server instance you can add a prefix or suffix to the database name so that it does not overwrite or conflict with the original database. If you choose the option to overwrite the original database on the original SQL Server instance, your SQL Admin must first take that database offline on the SQL Server instance and close all connections before the restore / overwrite can occur.

For **MSQL AAG databases**, the IBM Cloud Backup and Recovery service does not have the ability to restore the database directly back into the Availability Group. You must first explicitly choose, then restore the database back to a SQL Server instance (e.g. such as the primary replica) belonging to the availability group. Then, your SQL Admin must separately add the database back into the desired Availability Group using Microsoft SQL Server documentation / beset practices.

For **MSQL TDE-enabled database**, you can restore to the original SQL Server instance or a new one. If you choose to restore to the original SQL Server instance, your IBM Cloud Backup and Recovery service instance should recognize the TDE-based encryption keys and certificates, and proceed with restoring the database in an encrypted state. If you choose to restore to a new target SQL Server instance, then you’ll need to import the encryption keys and certificates over to the new SQL Server instance before the restore can take place.

Additional considerations and recommendations can be found in the linked sections listed below, along with detailed steps on performing database restores.

1. [Recover Microsoft SQL Server](/docs/backup-recovery?topic=backup-recovery-recover_microsoft_sql_server)
2. [Considerations for Microsoft SQL Server](/docs/backup-recovery?topic=backup-recovery-considerations_for_microsoft_sql_server)
3. [Microsoft SQL Server vdi-based recovery recommendations](/docs/backup-recovery?topic=backup-recovery-microsoft_sql_server_vdi-based_recovery_recommendations)
4. [Manage firewall ports](/docs/backup-recovery?topic=backup-recovery-manage_firewall_ports)
5. [Port Requirements - MS SQL Server and Data Source Connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector)
6. [Recover Microsoft SQL Server database as a new database in the original Microsoft SQL Server instance](/docs/backup-recovery?topic=backup-recovery-recover_microsoft_sql_server_database_as_a_new_database_in_the_original_microsoft_sql_server_instance)
7. [Recover Microsoft SQL Server database as a new database in an alternate Microsoft SQL Server instance](/docs/backup-recovery?topic=backup-recovery-recover_microsoft_sql_server_database_as_a_new_database_in_an_alternate_microsoft_sql_server_instance)
