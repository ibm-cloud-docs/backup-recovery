---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-30"

keywords:  virtual machine

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Agent download and install
{: #agent-download-install}

An agent is {{site.data.keyword.baas_full}} software installed on physical appliances, VMs or VSIs that interacts locally with the OS / Source data being protected and with the Data Source Connector / Backup and Recovery instance during backup and recovery operations. Windows and Linux agents are currently available with support for additional agent types available in the future.

## Before you start
{: #baas-agent-download-install-before-you-start}

Ensure that you have what you need to start:

- An account for the [{{site.data.keyword.cloud}} Platform](https://cloud.ibm.com).
- An [instance](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery#baas-provision-instance) of {{site.data.keyword.baas_full_notm}} service is deployed.
- A physical host or VM or VSI that agent will be installed on
- Verify that you have proper access to your {{site.data.keyword.baas_full_notm}} instance.
  - Writer (backup-recovery.dashboard.edit) or Manager (backup-recovery.dashboard.edit) privileges to the {{site.data.keyword.baas_full_notm}} Service are needed to create and manage Protection Jobs (i.e. Backup Jobs) in your instance. These privileges can be assigned by your {{site.data.keyword.cloud_notm}} Platform account owner using an Access Group (multiple users) or Access Policy (specific user) tied to your IAM profile. Details on this are included [here](/docs/backup-recovery?topic=backup-recovery-iam&interface=ui).
- Verify the correct ports are opened from an OS (on the source), VMware and/or VPC (Security Group inbound/outbound rules) standpoint. The required ports are included in the “Manage firewall ports” links below as well as found in the other tutorials covering backup and recovery of your sources (e.g. Linux, Windows and MS SQL, etc.).

## Launch your {{site.data.keyword.baas_full_notm}} Instance UI
{: #baas-agent-download-install-instance}

1. Log into the {site.data.keyword.cloud_notm} Platform.
2. Expand the Navigation Menu (four horizontal lines, top left) > select Resource List.
3. Expand Storage > select your {{site.data.keyword.baas_full_notm}} instance.
4. Select Launch dashboard > enter your SSO / account credentials to authenticate.

You will now be presented with the UI of your {{site.data.keyword.baas_full_notm}} Instance. The next section will cover how to download an agent to install on your sources (e.g. Linux, Windows and MS SQL, etc.).

## Agent download and install
{: #baas-agent-download-install-steps}

Please refer to the steps in the following link(s) to accomplish this:

1. [Manage firewall ports – Linux](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector#port_requirements)
2. [Install and Manage the Agent on Linux Servers](/docs/backup-recovery?topic=backup-recovery-linux_agent_install_manage)

    The Linux "Script Installer" must be used when installing an agent on a physical or virtual Linux host (e.g. RHEL, etc.).
    {: note}

3. [Manage firewall ports – Windows and MS SQL](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector#port_requirements)
4. [Install and Manage the Agent on Windows Servers](/docs/backup-recovery?topic=backup-recovery-install_and_manage_the_agent_on_windows_servers)

    The Windows Filesystem (Physical Server for File and Folder) and Microsoft SQL Server both use the same agent (.exe) for installation.
    {: note}
