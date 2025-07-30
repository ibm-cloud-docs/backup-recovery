---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: microsoft sql, windows cluster, backup agent,

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Auto-discover microsoft sql server topology when registering microsoft sql server
{: #auto-discover_microsoft_sql_server_topology_when_registering_microsoft_sql_server}

This is an Early Access feature. Contact your {{site.data.keyword.baas_full}} account team to enable the feature.

To register an MS SQL Server, MS SQL FCI, or an MS SQL AG of a Windows cluster, you can provide the IP address of the server, FQDN of the server, or VIP of the SQL FCI. The {{site.data.keyword.baas_full_notm}} discovers the entire MS SQL topology on the Windows cluster and displays the relevant subset based on the MS SQL address you provide while registering. From the topology list, you can select the servers you want to register with the {{site.data.keyword.baas_full_notm}}. To display the entire MS SQL topology, use the Windows Cluster IP/FQDN.

## Prerequisites
{: #prerequisites}

Before you register the MS SQL, ensure to meet the following:

*   MS SQL prerequisites. For more information, see [Requirements for Microsoft SQL Server Protection](/docs/backup-recovery?topic=backup-recovery-requirements_for_microsoft_sql_server_protection).
*   [Download and Install the Agent](#download_and_install_the_agent).

## Download and install the agent
{: #download_and_install_the_agent}

Install the Backup agent on each SQL server that you want {{site.data.keyword.baas_full_notm}} to protect.

1. Select **Data Protection > Sources**.
2. Click **Download Backup agent**. Ensure the agent has been downloaded to the appropriate server.
3. As an administrator with local system privileges on that server, run the executable and complete the installation using the following steps:
4. **Service Account Credentials:** You can use either the LOCAL SYSTEM account or an account that meets the requirements to install the Backup agent.

    If you do not use the LOCAL SYSTEM account, ensure the following:

    *   The account must be a member of the local Windows Administrators group. For example, qa01\\tme-backup is an Active Directory user account in the data center. If the backup admin plans to use this account, qa01\\tme-backup must be part of the local Windows Administrators group on the SQL server.
    *   The account must have Log on as a service in the User Rights Assignment on the MS SQL server to install the Backup agent.

5. Wait for the installation to finish.

    In the SQL Server Management Studio (SSMS), ensure that the account used to install the Backup agent has **Server Roles: sysadmin** in the SQL server instances.

6. The agent starts automatically.

7. Repeat the agent installation process on each SQL server you want to protect. This includes any standalone MS SQL servers and Microsoft SQL Server nodes with AGs.

## Register ms sql server
{: #register_ms_sql_server}

1. Select **Data Protection > Sources**.
2. Click **Register** > **Databases** > **MS SQL Server (early access: auto-detect)**.

    The Register an MS SQL Server page appears.

3. In the **Hostname or IP Address** field, enter the IP address of the server, FQDN of the server, or VIP of the SQL FCI. {{site.data.keyword.baas_full_notm}} recommends you to provide the FQDN of the server.

4. Select the **Server Type**: Windows.

    This is an Early Access feature. Contact [IBM Cloud Support](https://cloud.ibm.com/unifiedsupport/supportcenter) to enable the feature.

5. Click **Register**.

    The {{site.data.keyword.baas_full_notm}} auto-discovers the entire MS SQL topology on the cluster. Based on the MS SQL address you provide while registering, the {{site.data.keyword.baas_full_notm}} displays the relevant subset of that MS SQL server. The {{site.data.keyword.baas_full_notm}} also displays which version of the Backup agent is running on each of the servers.

6. From the topology list, select the entities you want to register with the {{site.data.keyword.baas_full_notm}} and select **Complete Registration**.

    The selected entities are registered with the {{site.data.keyword.baas_full_notm}} and are displayed on the source page of the {{site.data.keyword.baas_full_notm}} UI.
