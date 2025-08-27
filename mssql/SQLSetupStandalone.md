---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: microsoft sql, server management studio, physical server,

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Set up standalone microsoft sql server or microsoft sql server ags
{: #set_up_standalone_microsoft_sql_server_or_microsoft_sql_server_ags}

This topic describes how to set up standalone MS SQL servers and MS SQL Server Always On Availability Groups (AGs) to be protected by a {{site.data.keyword.baas_full}}.

The **Readable Secondary** field in the Availability Group Properties of the SQL Server Availability Group (AG) must be set to "Yes" or "Read-intent only".

The following are the different methods to register your physical or virtual MS SQL Servers allowing you to protect your MS SQL server databases:

*   **Registering a SQL Server using the {{site.data.keyword.baas_full_notm}} SQL Agent**:

    This option is the preferred method to protect your MS SQL server databases on physical or virtual hardware. This option can be used to protect any SQL server configurations such as MS MSQL standalone, FCI, and AG. For more information, see [Set Up MS SQL on Virtual or Physical Servers using the Agent](#set_up_ms_sql_on_virtual_or_physical_servers_using_the_agent).

    To set up an MS SQL FCI, follow the steps described in [Set Up Microsoft SQL Server FCI](SQLSetupFCI.htm).

*   **Registering a Hypervisor for SQL Server**:

    {{site.data.keyword.baas_full_notm}} provides an additional option to protect your MS SQL server at the hypervisor level. While this option is available, it is not the preferred method to protect your SQL server databases. Following are the two available registration methods:

    *   **Persistent Agent**—For more information, see [Set Up MS SQL on VMs as a VMware vCenter application with the Installed Agent](#set_up_ms_sql_on_vms_as_a_vmware_vcenter_application_with_the_installed_agent).
    *   **Auto-Deployed Agent (non-Persistent)**—For more information, see [Set Up MS SQL on VMs as a VMware vCenter application with the Auto-deployed Agent](#set_up_ms_sql_on_vms_as_a_vmware_vcenter_application_with_the_auto-deployed_agent).

## Ensure prerequisites are met
{: #ensure_prerequisites_are_met}

Ensure the prerequisites listed in [Prerequisites for Microsoft SQL Server Protection](/docs/backup-recovery?topic=backup-recovery-requirements_for_microsoft_sql_server_protection) are met.

## Set up ms sql on virtual or physical servers using the agent
{: #set_up_ms_sql_on_virtual_or_physical_servers_using_the_agent}

This option can be used for both physical or virtual servers that are in standalone or AG configurations. {{site.data.keyword.baas_full_notm}} recommends you use this method to protect the MS SQL Servers with the {{site.data.keyword.baas_full_notm}}. For more information, see [Set Up MS SQL on Virtual or Physical Servers using the Agent](/docs/backup-recovery?topic=backup-recovery-install-and-manage-the-agent-on-windows-servers).

For IBM Cloud Support information, see [Physical Servers](/docs/backup-recovery?topic=backup-recovery-physical_servers).

### Download and install the agent
{: #download_and_install_the_agent}

Install the Backup agent on each SQL server that you want {{site.data.keyword.baas_full_notm}} to protect.

1. Select **Data Protection > Sources**.
2. Click **Download Backup agent**. Ensure the agent has been downloaded to the appropriate server.
3. As an administrator with local system privileges on that server, run the executable and complete the installation using the following steps:
4. **Volume CBT (Changed Block Tracker):** Install this component to leverage {{site.data.keyword.baas_full_notm}}'s built-in CBT technology. Without this component, only full backups can be performed. Installing this component requires a one-time reboot to load the {{site.data.keyword.baas_full_notm}} CBT driver. Until a reboot, only full backups can be performed.

    **File System CBT (Changed Block Tracker):** Install this component to perform MS SQL file-based protection.

    Installing the Volume CBT or File System CBT is not necessary for MS SQL VDI-based protection.

5. **Service Account Credentials:** You can use either the LOCAL SYSTEM account or an account that meets the requirements to install the Backup agent.

    If you do not use the LOCAL SYSTEM account, ensure the following:

    *   The account must be a member of the local Windows Administrators group. For example, qa01\\tme-backup is an Active Directory user account in the data center. If the backup admin plans to use this account, qa01\\tme-backup must be part of the local Windows Administrators group on the SQL server.
    *   The account must have Log on as a service in the User Rights Assignment on the MS SQL server to install the Backup agent.

6. Wait for the installation to finish.

    In the SQL Server Management Studio (SSMS), ensure that the account used to install the Backup agent has **Server Roles: sysadmin** in the SQL server instances.

7. The agent starts automatically.

8. Reboot the machine if you have installed the Volume CBT component.

9. Repeat the agent installation process on each SQL server you want to protect. This includes any standalone MS SQL servers and Microsoft SQL Server nodes with AGs.

### Register a physical ms sql server
{: #register_a_physical_ms_sql_server}

If the server contains one or more databases that belong to an Always On Availability Group (AAG), all remaining servers within the AAG must also be registered with the {{site.data.keyword.baas_full_notm}} as MS SQL Server in order to be backed up. Navigate to **Data Protection > Sources > click on the required source > All Objects > Show AAG Nodes** to display the registered AAG nodes and any warnings about unregistered servers. If a replica is added to an existing AAG, the replica is automatically backed up by the next Protection Group run that protects that AAG, even if the AAG node list does not display it.

1. Select **Data Protection > Sources**.
2. Click **Register** > **Physical Server**.
3. Specify the hostname (FQDN) or server IP of the server to register. {{site.data.keyword.baas_full_notm}} recommends using the FQDN.
4. Select the **Server Type**: Windows or Linux.

    For Linux, provide the username and password for the Database Authentication.

    This is an Early Access feature. Contact [IBM Cloud Support](https://cloud.ibm.com/unifiedsupport/supportcenter) to enable the feature.

5. Click **Register**.

    When registration finishes, a new entry appears in the physical section in the Sources page.

6. Next to the newly added physical server, mouse over the actions menu and click **Register as MS SQL Server**.

    When registration finishes successfully, the MS SQL Server information displays when you click the **View** icon.


You can throttle the CPU capacity on the MS SQL physical servers (running on Windows 2012R2 and later) you registered with the {{site.data.keyword.baas_full_notm}}. Throttling optimizes the CPU capacity utilized when a Protection Group with source-side deduplication is run on the server. However, for MS SQL Protection Group, source-side deduplication is not enabled by default. You must contact [IBM Cloud Support](https://cloud.ibm.com/unifiedsupport/supportcenter) for enabling the source-side deduplication for a Protection Group. CPU throttling does not affect the MS SQL Server if source-side deduplication in a SQL Protection Group is not enabled. For more information on enabling CPU throttling, see [Network Bandwidth and CPU Throttling for Physical Server](/docs/backup-recovery?topic=backup-recovery-network_bandwidth_and_cpu_throttling_for_physical_server).

## Set up ms sql on vms as a vmware vcenter application with the installed agent
{: #set_up_ms_sql_on_vms_as_a_vmware_vcenter_application_with_the_installed_agent}

This method must be used only when you are registering SQL Server at the hypervisor-level. You can use this option for standalone SQL or SQL servers in AG running on the VMware vCenter.

### Download and install the agent
{: #download_and_install_the_agent}

Before registering the MSSQL server at the hypervisor-level, you must first install the Backup agent on each SQL server that you want {{site.data.keyword.baas_full_notm}} to protect.

You can optionally use the auto-deployed Backup agent, which does not require agent installation. {{site.data.keyword.baas_full_notm}}, however, recommends installing the agent because this avoids additional configuration such as disabling Windows User Account Control (UAC). If you still want to use the auto-deployed agent, follow the steps described in [Set Up MS SQL on VMs as a VMware vCenter application with the Auto-deployed Agent](#set_up_ms_sql_on_vms_as_a_vmware_vcenter_application_with_the_auto-deployed_agent).

1. Select **Data Protection > Sources**.
2. Click **Download Backup agent**. Ensure the agent has been downloaded to the appropriate server.
3. As an administrator with local system privileges on that server, run the executable and complete the installation wizard.
4. You do not need to install any components. Backups leverage VMware VADP CBT or Hyper-V 2016 RCT.
5. **Service Account Credentials:** You can use either the LOCAL SYSTEM account or an account that meets the requirements to install the Backup agent.

    If you do not use the LOCAL SYSTEM account, ensure the following:

    *   The account must be a member of the local Windows Administrators group. For example, qa01\\tme-backup is an Active Directory user account in the data center. If the backup admin plans to use this account, qa01\\tme-backup must be part of the local Windows Administrators group on the SQL server.
    *   The account must have Log on as a service in the User Rights Assignment on the MS SQL server to install the Backup agent.

6. Wait for installation to finish.

    In the SQL Server Management Studio (SSMS), ensure that the account used to install the Backup agent has **Server Roles: sysadmin** in the SQL server instances.

7. The agent starts automatically.

8. Repeat the agent installation process on each SQL server you want to protect. This includes any standalone MS SQL servers and MS SQL Server nodes with AGs.

### Register ms sql
{: #register_ms_sql}

If the server contains one or more databases that belong to an Always On Availability Group (AAG), all remaining servers within the AAG must also be registered with the {{site.data.keyword.baas_full_notm}} as MS SQL Server in order to be backed up. Navigate to **Data Protection > Sources > click on the required source > All Objects > Show AAG Nodes** to display the registered AAG nodes and any warnings about unregistered servers. If a replica is added to an existing AAG, the replica is automatically backed up by the next Protection Group run that protects that AAG, even if the AAG node list does not display it.

1. Select **Data Protection > Sources**.
2. Navigate the hierarchy to the server that is running MS SQL.
3. Mouse over the server's actions menu and click **Register Applications**.
4. Select **Installed Agent**.

    If the agent is not already installed, registration fails.

5. Select MS SQL.
6. Click **Register**.

    The agent starts automatically.

    The server's MS SQL and agent information display when you click the **View** icon.


## Set up ms sql on vms as a vmware vcenter application with the auto-deployed agent
{: #set_up_ms_sql_on_vms_as_a_vmware_vcenter_application_with_the_auto-deployed_agent}

If the server contains one or more databases that belong to an Always On Availability Group (AAG), all remaining servers within the AAG must also be registered with the {{site.data.keyword.baas_full_notm}} as MS SQL Server in order to be backed up. Navigate to **Data Protection > Sources > click on the required source > All Objects > Show AAG Nodes** to display the registered AAG nodes and any warnings about unregistered servers. If a replica is added to an existing AAG, the replica is automatically backed up by the next Protection Group run that protects that AAG, even if the AAG node list does not display it.

1. Select **Data Protection > Sources**.
2. Navigate the hierarchy to the server that is running MS SQL.
3. Mouse over the server's actions menu and click **Register Applications**.
4. Select **Auto-Deployed Agent**.

    Enter the Windows administrator **Username** and **Password** for the server that is running MS SQL. Windows administrator privileges are required. Ensure that the user account used for auto-deploying the agent is added to all the MS SQL server's SQL instances' **Security > Logins** with Windows Integrated Authentication. Perform one of the following:

    Preferred option - Check and update winexe restrictions:

    1. Ensure winexe is not blocked by simple file sharing mode.

        Go to **Start > Run > secpol.msc > Local Policies > Security Options** and set **Network Access: Sharing and security model for local accounts** to Classic – local users authenticate as themselves.

    2. Ensure winexe is not blocked by Windows Remote User Account Control.

        Configure the registry by going to **Start > Run > cmd** and press Ctrl-Shift-Enter. Enter the following command:

        reg add "HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Policies\\system" /v LocalAccountTokenFilterPolicy /t REG\_DWORD /d 1 /f

        For more information, see [User Account Control](https://support.microsoft.com/en-us/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows){: external}.


    Second option - Disable the UAC property Admin Approval Mode:

    1. Click **Start**, type secpol.msc in the Search programs and files box, and then press Enter.
    2. If the User Account Control dialog box appears, confirm that the action it displays is what you want, and then click **Yes**.
    3. In the console tree, expand Local Policies, and click **Security Options**.
    4. In the details pane, scroll down and double-click the **Group Policy** setting that you want to change. This opens the UAC policy settings that a local administrator can modify.
    5. Disable **User Account Control: Run all administrators in Admin Approval Mode** for the Administrator account.
    6. Ensure that **User Account Control: Admin Approval Mode for Built-in Administrator Account** is set to Disabled.
    7. Ensure that **User Account Control: Behavior of elevation prompt for administrators in Admin Approval Mode** is set to Elevate without Prompting.
    8. Restart/reboot the Windows server for the changes to take effect.
    9. Provide the Administrator/Domain Administrator user that has Admin Approval Mode disabled.

5. Select MS SQL.
6. Click **Register**.

    The server's MS SQL information displays when you click the **View** icon.

## Troubleshoot
{: #troubleshoot}

*   The log back up of AG MS SQL Server fails with the following error if there is a break in the log chain.

    Log chain break error: Discovered a break in the log chain for <Database Names>

    **Resolution**: To resolve this issue, ensure that no other third-party applications are running a log backup, and then perform a full backup run to reset the log chain.

*   The log back up of the AG MS SQL Server fails with the following error if a database is added or removed from the AG MS SQL Server.

    AG relationship error: Discovered a AG relationship error for database <Database Names>

    **Resolution**: To resolve this error, perform a full backup run.


For more troubleshooting information, see the following Knowledge Base article:

*   [Collecting troubleshooting information for MS SQL issues](/docs/backup-recovery?topic=Collecting-troubleshooting-information-for-MS-SQL-issues){: external}

Log in to the [IBM Cloud Support](https://cloud.ibm.com/unifiedsupport/supportcenter) to see more Knowledge Base articles.
