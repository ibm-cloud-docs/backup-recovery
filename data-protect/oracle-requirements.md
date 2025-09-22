---

copyright:
  years: 2025
lastupdated: "2025-9-22"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Oracle requirements
{: #oracle_requirements}

To register your Oracle servers and protect your databases, be sure you meet the requirements and install the Cohesity Agent on each server.

Before you register your Oracle servers to protect your Oracle Databases, confirm that you meet the software version, [prerequisites](#Oracle_Prerequisites), [credentials](#Oracle_Credentials_and_Privileges), [choose an authentication method](#Oracle_Authentication_Method_Requirement), and set [sudoers permissions](#Oracle_Sudoers_Permissions_for_Linux_Databases) below, then [download and install the Cohesity Linux Agent for Oracle](#Download_and_Install_the_Cohesity_Agent) on the servers you wish to protect.

Also, be sure to review the [limitations](#Oracle_Limitations) at the end.

For information on the supported cloud regions where you can back up this source, see [Supported Workloads and Cloud Regions](supported-workflows.htm).

## Support matrix
{: #support_matrix}

Cohesity DataProtect as a Service supports Oracle Database protection. For information on the supported Oracle versions, see [Supported Software for Cohesity DataProtect as a Service](supported-software-dataprotect.htm#Supported_Software_for_DataProtect_as_a_Service).

## Check firewall ports
{: #check_firewall_ports}

Ensure that the ports listed in the Oracle Servers section in the [Firewall Ports for User-Deployed SaaS Connectors](dataprotect-firewall-ports.htm) topic are open to allow communication between the Cohesity SaaS Connector(s) and Oracle Server.

## Prerequisites
{: #prerequisites}

Make sure the following prerequisites are met before you proceed with Oracle source registration:

*   **UUIDs**. All the Oracle Databases that are protected using Cohesity DataProtect as a Service should have a unique UUID on the Oracle source where the databases reside.
    
*   **Archive Log Mode**. Archive Log mode must be enabled for databases to be opened in Read-Write mode.
    
*   **Read Only Mode**: The Oracle Databases should be opened in Read-Write mode.
    
*   **Version**. The recovery source and target database must be the same Oracle database version. For example, snapshots of an 11g Oracle Database cannot be recovered to a 12c Oracle Database.
    
*   **Oracle Single Instance Deployment**. For an Oracle single-instance database, the database must be entered into the `/etc/oratab` file. Otherwise, Cohesity DataProtect as a Service will not be able to discover this database.
    
*   **Authentication**. If you choose DB authentication, all the databases on the system should have the same username and password or OS Authentication. At the backup level, they can have individual passwords for the databases.
    
*   **Ports**. On the Oracle Server where you [install the Cohesity Linux Agent](#Download_and_Install_the_Cohesity_Agent) (below), open the 50051 port for backup operations (incoming) and 59999 port for self-monitoring and debug pages.
    

## Credentials and privileges
{: #credentials_and_privileges}

Once you register your physical servers with Cohesity DataProtect as a Service as Oracle servers, Cohesity DataProtect as a Service will discover your Oracle databases automatically. For Cohesity DataProtect as a Service to successfully discover your Oracle databases, the user account running the Cohesity Linux Agent must have the appropriate credentials and privileges.

You can install the Cohesity Linux Agent to [run with the ROOT user](#Running_Agent_with_ROOT_User) or [with a separate OS user](#Running_Agent_with_OS_Service_Account_user) (also known as the ‘OS Service Account user’).

When connecting to Oracle databases, Cohesity DataProtect as a Service can use either the Oracle OS Authentication or [Oracle DB Authentication](#Oracle_Authentication_Method_Requirement) method. These two types of Oracle authentication are available whether the Agent is run with the ROOT user or a separate OS Service Account user.

While most Oracle operations are available using either OS or DB authentication, some specific operations specifically require one or the other. For details, see [Oracle Authentication Method Requirement](#Oracle_Authentication_Method_Requirement) below.

### Running agent with root user
{: #running_agent_with_root_user}

You can install Cohesity’s Linux Agent to run with the ROOT user. When you take this approach, the agent runs every command using the ROOT user, except for Oracle commands and utilities like RMAN or SQLPLUS. To run Oracle commands and utilities, the Agent will ‘su’ to the user who is the owner of the Oracle binary in the current Oracle Home. If an Oracle operation is run against a source database that has DB Authentication configured (where the user has previously configured DB credentials for this Oracle source database), DB Authentication will be used to run Oracle commands and utilities. Otherwise, OS Authentication via the Oracle binary owner will be used.

When you install the Cohesity Agent to run with the ROOT user, there is no need to configure additional SUDOERS privileges.

To start the service as a ROOT user, add the following permission to the sudoers file:

`Defaults:<**oracle_binary_user**> !requiretty`.

### Running agent with os service account user
{: #running_agent_with_os_service_account_user}

You can install Cohesity’s Linux Agent to run with a specific OS Service Account user account, as long as it meets the following requirements:

*   The OS user is automatically granted the required sudo privileges. This allows the Cohesity Agent to execute specific privileged commands. For details, see [Oracle Sudoers Permissions for Linux Databases](#Oracle_Sudoers_Permissions_for_Linux_Databases) below.
    
*   The OS user should be part of the OS group with SYSDBA or SYSBACKUP privileges (for example, dba).
    

You can run the Cohesity Agent as a different service user, the _cohesityagent_ user, if this user is part of the OSDBA group in Oracle.

If you choose DB authentication, then all the databases on the system should have the same username and password.

If you wish to add the OS user to the Oracle Database as an OS-authenticated user, use the IDENTIFIED EXTERNALLY clause.

### Oracle authentication method requirement
{: #oracle_authentication_method_requirement}

You can either use either OS user or DB user authentication to connect to your Oracle Databases, but for recovery to _alternate_ servers, you must use OS authentication.

Table: Available Oracle Operations by Authentication Method.

  
| Oracle Operation | Authentication Method | Notes |
| --- | --- | --- |
| Backup | OS Authentication or DB Authentication | None |
| Restore to Original Server (a.k.a. Overwrite Restore) | OS Authentication or DB Authentication | Restoring data to the same server overwrites the original database. |
| Restore to Alternate Server | OS Authentication | DB Recovery or Restore into a different server is available, assuming the Oracle binaries already exist and the target Oracle server has free space to store the newly created database files. |
{: caption="Table 1. " caption-side="bottom"}

### Oracle sudoers permissions for linux databases
{: #oracle_sudoers_permissions_for_linux_databases}

The following tables list the sudoers permissions required for the Cohesity Linux Agent for Oracle.

When you install the Cohesity Agent to run with the ROOT user, there is no need to configure additional SUDOERS privileges.

  
| Operating System | Sudoers Permissions | Sudoers Permissions |
| --- | --- | --- |
|     | Cohesity Linux Agent Commands for both Oracle sources & Linux servers | Additional commands only for Linux servers |
| **Linux** | *   `cp`<br>    <br>*   `chown`<br>    <br>*   `chmod`<br>    <br>*   `mkdir`<br>    <br>*   `rm`<br>    <br>*   `tee`<br>    <br>*   `hostname`<br>    <br>*   `stat`<br>    <br>*   `timeout`<br>    <br>*   `ls`<br>    <br>*   `rsync` | *   `blkid`<br>    <br>*   `lsof`<br>    <br>*   `losetup`<br>    <br>*   `dmsetup`<br>    <br>*   `lvs`<br>    <br>*   `vgs`<br>    <br>*   `lvcreate`<br>    <br>*   `lvremove`<br>    <br>*   `lvchange` |
{: caption="Table 2. " caption-side="bottom"}

## Download and install the cohesity agent
{: #download_and_install_the_cohesity_agent}

The Cohesity Linux Agent can be installed to [run as a ROOT user](#Install_the_Cohesity_Linux_Agent_to_Run_with_ROOT_User) or [as an OS Service Account user](#Install_the_Cohesity_Linux_Agent_to_Run_with_OS_Service_Account_User). Install the Cohesity Linux Agent on each Oracle server that you want to protect.

### Cohesity linux agent best practices
{: #cohesity_linux_agent_best_practices}

We recommend you follow these best practices when you plan to deploy the Cohesity Linux Agent on Oracle servers and hosts:

*   If you choose DB authentication, then all the databases on the system should have the same username and password.
    
*   Create a database user for your Cohesity Oracle backup and restore workflows. (_Optional_)
    
*   Both the Oracle host and the Cohesity Linux Agent should have permission to write to the `adump` and `diag` directories, control file, and the database restores locations.
    
*   Enable Block Change Tracking (BCT) to improve the incremental backup performance of the Oracle server. (_Optional_)
    
*   Assign sudoers to the user running the Cohesity Linux Agent.
    
*   Make the Cohesity Linux Agent user part of the Oracle dba group.
    
*   Given that Oracle Secure Backup (SBT)-based incremental backups are not fully hydrated (unlike imagecopy-based backups), we recommend you take a full database backup regularly.
    

### Install the cohesity linux agent to run with root user
{: #install_the_cohesity_linux_agent_to_run_with_root_user}

To install the Cohesity Linux Agent to run as the ROOT user on your Oracle server:

1. In **DataProtect as a Service**, navigate to the **Sources** page and click **\+ Register Source** in the upper-right corner of the page and then click **Oracle**.
    
2. Click **Start Registration**.
    
3. In the Register Physical dialog box, select an existing SaaS connection marked Unused or click Create SaaS Connection and follow the instructions in [Deploy SaaS Connector](saas-connection.htm), and then click **Continue**.
    
4. Click **Download Cohesity Agent**. Ensure the agent has been downloaded to the appropriate server.
    
    ![](../Resources/Images/data-protect/Oracle/DP_Oracle_Registration_773x693.png)
    
5. Run the executable file with sudo using the following command syntax:  
    `sudo /<``path_to_installer_file``> -- --install -c 0 -S root -G root`
    
    The command options are:
    
    *   **\-S**: The user to run the Agent. Specify '`root`'.
        
    *   **\-G**: The group permission the Agent will use for files and directories installed by the agent. Specify '`root`'.
        
    *   **\-c**: The boolean switch that controls whether the OS user and group should be created. '`0`' means do not create the OS user and group, and '`1`' means the Agent installation will create the specified OS user and group. (If you choose to run with the root user, specify ‘`-c 0`’ as ‘`root`’ already exists.)
        

The Agent starts automatically after the installation, as well as on a subsequent Oracle host reboot.

At the end of the installation, the commands used to start, stop, or get Agent status are displayed for future reference.

### Install the cohesity linux agent to run with os service account user
{: #install_the_cohesity_linux_agent_to_run_with_os_service_account_user}

To install the Cohesity Linux Agent to run as the OS Service Account user on the Oracle server:

1. In **DataProtect as a Service**, navigate to the **Sources** page and click **\+ Register Source** in the upper-right corner of the page and then click **Oracle**.
    
2. Click **Start Registration**.
    
3. Click **Download Cohesity Agent**. Ensure the agent has been downloaded to the appropriate server.
    
4. Grant sudo permission to the user who will install the agent. This user must be part of the OS DBA group. For details, see [Credentials and Privileges](#Oracle_Credentials_and_Privileges) above.
    
    *   If you plan to run the Oracle SQL commands as OS authenticated user, we recommend you perform the installation as the Oracle OS user. Even if the Cohesity Agent user is part of the DBA group, you can run the Oracle SQL commands.
        
    *   Because restoring to alternate locations requires OS authentication, we recommend you use OS instead of DB authentication. The restore to alternate locations will succeed only if the Cohesity Agent is installed with **dba** or **oinstall** as the user group.
        
    *   The Cohesity Agent installer grants sudo permission for the following commands:
        
        `/usr/bin/cp, /usr/bin/chown, /usr/bin/chmod, /usr/bin/mkdir, /usr/bin/rm, /usr/bin/tee, usr/bin/hostname, /usr/bin/stat, /usr/sbin/blkid, /usr/sbin/lsof, /usr/bin/ls, /usr/sbin/losetup, /usr/sbin/dmsetup, /usr/bin/rsync, /usr/bin/timeout, /usr/sbin/lvs, /usr/sbin/vgs, /usr/sbin/lvcreate,/usr/sbin/lvremove, /usr/sbin/lvchange`
        
5. Copy the downloaded file to the target Oracle host and run the executable file as a sudo user using the following command syntax:
    
    **For script-based installer**:
    
    `sudo /<_path_to_installer_fil_e> -- --install`
    
    **For RPM-based installer**:
    
    `sudo rpm -i _path_to_install_file_`
    
    The installer creates the user group, 'cohesity agent,' and installs the Agent.
    

The Agent starts automatically after the installation or on reboot.

## Considerations
{: #considerations}

*   **Oratab**. Only standalone databases listed in the `oratab` file on the Oracle server can be registered and protected. Cohesity DataProtect as a Service cannot discover databases that are not in `oratab`.
    
*   **Auto Protect**. Auto Protect is not supported for Oracle databases.
    
*   **Point-in-Time Restore**. During a point-in-time restore to a time near the end of a full backup, the restore might fail due to a known issue from Oracle.
    

**Next** > [Register your Oracle servers](register-oracle-sources.htm#Register_Oracle_Sources)!
