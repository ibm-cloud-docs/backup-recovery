---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: server protection, microsoft sql, backup agent, sysadmin role,

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Requirements for microsoft sql server protection
{: #requirements_for_microsoft_sql_server_protection}

## Supported software for microsoft sql server protection
{: #supported_software_for_microsoft_sql_server_protection}

For more information, see [Databases](/docs/allowlist/backup-recovery?topic=backup-recovery-supported_database_types_for_microsoft_sql_server).

## Agent minimum permissions for microsoft sql server protection
{: #agent_minimum_permissions_for_microsoft_sql_server_protection}

You can use either the host LOCAL SYSTEM account or an account that meets the requirements to install the Backup agent.

It is recommended to run the Backup agent service with an Active Directory domain user account that is a member of the local administrator of the SQL Server host. The AD domain user account must be a member of the SQL sysadmin server role. The user account must have log-on rights to the SQL Server host in the local security policy of the SQL Server host.

If you do not use the LOCAL SYSTEM account, ensure the following for the chosen account:

*   The account must be a member of the local Windows Administrators group. For example, qa01\\tme-backup is an Active Directory user account in the data center. If the backup admin plans to use this account, qa01\\tme-backup must be part of the local Windows Administrators group on the SQL server.
*   The account must have Log on as a service in the User Rights Assignment on the MS SQL server to install the Backup agent.

*   The account must have the sysadmin role in the MS SQL Server instance.

## Ports used for communication
{: #ports_used_for_communication}

*   On physical servers or VMs with an ephemeral or installed agent, open the ports 445, 11113, 11117, and 50051. For more information, see [Manage Firewall Ports](/docs/allowlist/backup-recovery?topic=backup-recovery-manage_firewall_ports).

*   If the Windows Firewall is used:

    *   Inbound rules:

        *   Add a rule to accept SQL Server traffic and TCP connections on local port 1433.

        *   Set Remote Port to All Ports.

    *   Outbound rules (for MS SQL Server 2016 running on Windows 2016):

        Update the "Block network access for R local user accounts in SQL server instance MSSQLSERVER" rule by going to General tab > Action window > select **Allow the connection**.


## Prerequisites for microsoft sql server protection
{: #prerequisites_for_microsoft_sql_server_protection}

*   Verify MS SQL Server services are running.

*   The **Readable Secondary** field in the Availability Group Properties of the SQL Server Availability Group (AAG) must be set to **Yes** if you have selected **Secondary Only** as the **Backup Preference setting for AAG**.
*   MS SQL database and instance names must meet the Microsoft SQL Server naming requirements. Names containing illegal Windows file characters, such as double quotes, pipe symbols, greater than and less than signs ( " | > < ), a space at the beginning or end of the database name (applicable only for VDI-based backup) are not supported. For more information, see [Naming Files, Paths, and Namespaces](https://learn.microsoft.com/en-us/windows/win32/fileio/naming-a-file){: external}.
*   On the Server's Windows system, {{site.data.keyword.baas_full}} recommends to set the Power Plan to High performance.


## Credentials and privileges for microsoft sql server protection
{: #credentials_and_privileges_for_microsoft_sql_server_protection}

There are three accounts you must consider when installing the Backup agent:

*   **Installation Account**: The account you use to log in to the host and run the installer.

*   **Service Account**: The account under which the Backup agent service runs on the SQL Server host. (Selected at the time of installation.)

*   **SQL Server login account**: The SQL Server account by which the Backup agent has access to the databases. (Configured after installation.)


### Installation account
{: #installation_account}

When installing the Backup agent on a Microsoft Windows host, you must log onto the SQL Server host with an account that has local administrator privileges. This is because the Windows agent installer installs the Backup agent service. Microsoft requires the installation account to have administrator privileges on the host.

### Choose the service account
{: #choose_the_service_account}

The installer gives you the option between two types of service accounts:

*   The LocalSystem account.

*   A user account. Assigning privileges to the service account will differ depending on which type of account you choose. {{site.data.keyword.baas_full_notm}} recommends using either the LocalSystem account or an Active Directory Domain User account. See the chart below for a comparison of service account types.


### Comparison of service account types
{: #comparison_of_service_account_types}


| SERVICE ACCOUNT | TYPE | DEFINITION | RECOMMENDATION |
| --- | --- | --- | --- |
| LocalSystem (Default) | LocalSystem Account (Built-in Host Account) | The LocalSystem account is a predefined local account on the host computer. | Recommended: The Local System account is a predefined local account used by the service control manager. It has extensive privileges on the local computer, and acts as the computer on the network. Its token includes the NT AUTHORITY\\SYSTEM and BUILTIN\\Administrators SIDs; these accounts have access to most system objects. |
| User Account | Domain User Account (Active Directory) | A user whose username and password are stored in Active Directory. | Recommended: A domain user account enables the service to take full advantage of the service security features of Windows and Microsoft Active Directory Domain Services. The service has whatever local and network access is granted to the account, or to any groups of which the account is a member. |
| Local User Account (Host account) | In Windows, a local user is one whose username and encrypted password are stored locally, only on the computer itself. | Not recommended: When you log in as a local user, the computer checks its own list of users and its own password file to see if you are allowed to log in to the computer. The computer itself then applies all the permissions and restrictions that are assigned to you for that computer. |
{: caption="" caption-side="bottom"}

If you are unsure which account to use, choose the LocalSystem account. This account already has most of the required permissions. The agent service account can be changed later if needed.
