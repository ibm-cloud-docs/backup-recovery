---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Install and Manage the Agent on Windows Servers
{: #install_and_manage_the_agent_on_windows_servers}

If the Windows firewall is active upon installing the Backup agent, you need to add a rule in the firewall to open the port 50051 for communication with the {{site.data.keyword.baas_full_notm}} nodes. For more information, see [Manage Firewall Ports](../../Setup/OpenPorts.htm).

## Backup agent Component Requirements
{: #backup_agent_component_requirements}


Refer to the following table to determine which components to install with the agent service for the server type you want to register/protect.


| Server | Install Volume CBT | Install File System CBT |
| --- | --- | --- |
| [Physical Server without MS SQL](#PhysicalNoSQL) | Yes  <br>(allows incremental backup) | No  |
| [Physical Server with MS SQL](../../MSSQL/SQLSetupStandalone.htm) | Yes  <br>(allows incremental backup) | Yes  <br>(allows file-based SQL backup) |
| [MS Exchange Server](../../Doc/ExchangeServer.htm) | No  | Yes |
| [VMware VM (using Installed agent) with MS SQL](../../MSSQL/SQLSetupStandalone.htm) | No  | No  |
| [Hyper-V SCVMM server](#SCVMM) | No  | No  |
| [Hyper-V 2016 host](#HyperVHost) | No  | No  |
| [Hyper-V 2012 R2 host](#HyperVHost) | No  | Yes |
| [Nutanix AHV](#Nutanix) | No  | No  |

## Free Space Requirement for block-based Backup
{: #free_space_requirement_for_block-based_backup}

For block-based backup of Windows physical server, the space must be at least equal to the calculation (Initial disk size\*Churn rate\*Backup window time). If you're running multiple backups on a cluster, you'll need enough storage capacity to accommodate the sanpshots for each of the machines using this calculation.

## Download and Install the Agent
{: #download_and_install_the_agent}

Because of weaknesses in the SHA-1 algorithm and to align to industry standards, starting from the {{site.data.keyword.baas_full_notm}} version 6.7 onwards, sha1 signing of {{site.data.keyword.baas_full_notm}} artifacts is dropped. Windows Agent installer which was previously signed with sha1 and sha256 hashing algorithms is now signed with only sha256 algorithm. Due to this, the legacy OS (Windows 2008 R2) users will notice a warning when installing the {{site.data.keyword.baas_full_notm}} Windows agent.

To avoid the warning, install sha2 code signing support on the system where you are installing the agent.

This is only a warning, and you can click **Run** to continue with the installation.

For IBM Cloud Support information, see [Physical Servers](../../ReleaseNotes/SupportedVersions.htm#PhysicalSupport).

To download and install the {{site.data.keyword.baas_full_notm}} Windows Agent:

1.  Navigate to **Data Protection > Sources**.
2.  Click **Download Backup agent**. Ensure the agent has been downloaded to the appropriate server.

    Note that the {{site.data.keyword.baas_full_notm}} Windows Agent does not support file path names that are not compliant with UNICODE or UTF-8 encoding. The table below lists the encodings that are not supported.


    | Language | Locale | Windows Code Page |
    | --- | --- | --- |
    | Simplified Chinese | zh\_CN | 936 |
    | Traditional Chinese. | zh\_TW | 950 |
    | Japanese-SJIS | ja\_JP | 932 |
    | Japanese-EUC | ja\_JP | 20932 |
    | Korean | ko\_KR | 949 |

    The {{site.data.keyword.baas_full_notm}} Windows agent installation may fail even if the encodings are not supported.

3.  As an administrator with local system privileges on that server, run the executable with **Run as administrator** and complete the installation wizard.Install the **Volume CBT (Changed Block Tracker)** component to perform incremental backups. Installing this component requires a reboot. Without a reboot, only full backups can be performed. Not installing this component means only full backups can be performed.The {{site.data.keyword.baas_full_notm}} Volume CBT driver creates the **{{site.data.keyword.baas_full_notm}}\_Bitmap.cty** file at the root of each volume. There are multiple ways to backup Windows physical server but the **{{site.data.keyword.baas_full_notm}}\_Bitmap.cty** file will be available on the system only if the Volume CBT driver is installed. This file is used to maintain the CBT data for incremental block based backups across Windows server reboots. Do not move or delete the “**.cty**” file.

    The **File System CBT (Changed Block Tracker)** component is not required. Install this component if you require file-based SQL backup.

4.  The agent starts automatically.


This installation does not require a restart. You can wait until a normal maintenance window (such as a Patch Tuesday) to restart. However, until you do, some Protection Groups that depend on Volume CBT will run in full backup mode.

## Silently Install the Backup agent on Windows Physical Servers
{: #silently_install_the_backup_agent_on_windows_physical_servers}

You can install the Windows Backup agent on multiple Windows servers by running the following command in an Administrator CMD shell or Administrator PowerShell. (The servers must have access to the agent file.)

`<InstallerExeFile> [/type=<onlyagent|volcbt|fscbt|allcbt>] [/log=”<absolute-path-to-log-file>”] [/dir=”<absolute-path-to-install-location>”] [/username=<value> [/password=<value>]] [/verysilent] [/suppressmsgboxes] [/norestart] [/firewallrule=<value>] [/configfile <value>] /deleteconfigfile <value>] [/enforceusecustomcert=<value>] [/customcertstype=<Value>] [/cohesitycustomcertpath=<Value>] [/userrootcapath=<path-to-root-ca-cert>] [/usercertchainpath=<path-to-agent-cert>] [/userprivatekeypath=<path-to-private-key-generated-for-agent>]`

The default name for `<InstallerExeFile>` is `Cohesity_Agent_<Version>_Win_x64_Installer.exe`.

A sample command is shown below:

```
Cohesity_Agent_5.0.0.0_Win_x64_Installer.exe /log=”C:\logs\install logs.txt” /verysilent /suppressmsgboxes /norestart /dir=”F:\CohesityAgent”
```

The command parameters are described below.


| Parameter | Description |
| --- | --- |
| `/type=<onlyagent\|fscbt\|allcbt\|volcbt \|onlyagent\|volcbtwithsbt\|fscbtwithsbt \|agentwithsbt>` | Specifies the type of installation.<br><br>*   `volcbt` - Installs the Windows Agent Service and Volume CBT component.<br>    <br>*   `fscbt` - Installs the Windows Agent Service and File System CBT component. This parameter enables incremental backups of Windows Server 2012 R2 or individual databases hosted by MS SQL Server.<br>    <br>*   `allcbt` - Installs the Windows Agent Service, Volume CBT component and File System CBT component. This parameter enables incremental backups of Windows Server 2012 R2 or individual databases hosted by MS SQL Server.<br>    <br>*   `onlyagent` - Installs only the Windows Agent Service and not the Volume CBT or File System CBT components.<br>    <br>*   `onlyagent`\- Installs only the Windows Agent Service and not the Volume CBT or File System CBT components.<br>    <br>*   `volcbtwithsbt`\- Agent, Volume CBT and Oracle SBT Library<br>    <br>*   `fscbtwithsbt`\- Agent, File System CBT and Oracle SBT Library<br>    <br>*   `allcbtwithsbt`\- Agent, File System CBT, Volume CBT and Oracle SBT Library<br>    <br>*   `agentwithsbt`\- Agent and Oracle SBT Library<br>    <br><br>The `/type` parameter is supported only for a fresh installation. It is not supported for upgrades. You cannot add or remove components during an upgrade.<br><br>If the `volcbt` parameter is not specified, volume based backups will always be full. Similarly, if the `fscbt` parameter is not specified, SQL and exchange file based backups will be full. |
| `/log=”<absolute-path-to-log-file>”` | Records the events during installation and helps to debug the errors, if any, during the installation process. |
| `/username=<value> [/password=<value>]` | The {{site.data.keyword.baas_full_notm}} Windows Agent Service is installed, by default, in the Local System account. Use these parameters to change the account under which the service will run. Note that the specified user account must have local administrator privileges on the server and should have the `Log on as a service` right. |
| `/verysilent` | Suppresses the installation progress window.<br><br>If you specify this option during the agent installation, you should review the default values for all the other parameters such as `/type, /dir, /username,` etc., and explicitly add them with relevant values if the defaults are not acceptable. |
| `/suppressmsgboxes` | Suppresses any message boxes if `/verysilent` is specified. |
| `/norestart` | Prevents the setup from restarting the system following a successful installation, or after a `Preparing to Install` failure that requests a restart. |
| `/dir=”<absolute-path-to-install-location>”` | Specifies the directory path in which the {{site.data.keyword.baas_full_notm}} Windows Agent Service should be installed.<br><br>The installer will create the necessary folders if they do not exist. If this parameter is not specified, the system will install the Windows Agent Service in the default directory path: `“%SystemDrive%\Program Files\Cohesity”`. |
| `/firewallrule=<yes \| no>` | Specifies whether or not to open Agent port in the firewall. The default value is 'yes'.<br><br>The installer creates an inbound rule in the local firewall of the Windows machine for port 50051 and is bound with the Backup agent Service. Any domain policy settings that may negate this effect will have to be handled at the domain policy level. The installer creates rules only in the Windows Firewall application. If any other firewall application is in force on the machine then the user will have to create appropriate rules in the active firewall application. |
| `/configfile <value>` | Specifies the path to INI file containing credentials for Agent service user account.<br><br>If the `configfile` parameter is not specified, the username and password command line parameters are ignored. |
| `/deleteconfigfile <yes \| no>` | Specifies whether to delete the configfile upon successful installation. |
| `[/enforceusecustomcert=<yes>]` | Enforce the user to use a custom certificate while installing the agent.<br><br>If this is set to YES, then the subsequent fields should have valid values, else the agent will not start.<br><br>If the custom certificate is not found then the agent would not start. |
| `[/customcertstype=usercerts\|cohesitycustomcerts]` | Specifies the type of custom certificates. Possible values include {{site.data.keyword.baas_full_notm}} Custom Certs and User Custom Certs. |
| `[/cohesitycustomcertpath=<path-to-agent-certificate-in-Cohesity-format>` | To be used only when **/customcertstype = cohesitycustomcerts**.<br><br>Specifies the directory path in which the {{site.data.keyword.baas_full_notm}} custom certificate for the agent is present. |
| `[/userrootcapath=<path-to-root-ca-cert> /usercertchainpath=<path-to-agent-cert> /userprivatekeypath=<path-to-private-key-generated-for-agent>]”` | To be used only when the **/customcertstype = usercerts**, then these three parameters should be specified. |

On Windows physical servers, the Backup agent logs are stored in C:/programData/cohesity/.

## Upgrade the Agent on VMware
{: #upgrade_the_agent_on_vmware}

### Windows Agent
{: #windows_agent}

{{site.data.keyword.baas_full_notm}} recommends using the agent installer when installing the persistent agent. If the persistent agent is manually installed, use the following workaround to upgrade the agent:

1.  Log on to the server from which the persistent agent must be upgraded.

2.  Open an elevated command window, and then stop the earlier persistent agent service:

    net stop cohesityagent

3.  Download the Backup agent installer from the {{site.data.keyword.baas_full_notm}} UI.

4.  Double-click the installer .exe file to initiate the installation. Follow the installer UI prompts.

5.  Do not choose to install any CBT drivers. Only upgrade the agent binary.

    This procedure upgrades the Backup agent and creates an entry in **Control Panel** > **Uninstall Program**.


VMware VMs that are not registered as a source on the {{site.data.keyword.baas_full_notm}} need to upgrade the installed Backup agent manually.

1.  Download the Backup agent installer from the {{site.data.keyword.baas_full_notm}} UI.

2.  Double-click the installer .exe file to initiate the installation. Follow the installer UI prompts.
3.  Do not choose to install any CBT drivers. Only upgrade the agent binary.


## Download and Install the Agent on the SCVMM Server
{: #download_and_install_the_agent_on_the_scvmm_server}

Before registering SCVMM, you must install the Backup agent on the SCVMM server or on an existing proxy endpoint that is connected to an SCVMM server.

1.  Navigate to **Data Protection > Sources**.
2.  Click **Download Backup agent**. Ensure the agent has been downloaded to the appropriate server.
3.  As an administrator with local system privileges, run the executable and complete the installation wizard.

    Install the agent without additional components.

4.  The agent starts automatically. Now you can register the Hyper-V SCVMM server as a source.


## Download and Install the Agent on Hyper-V Hosts
{: #download_and_install_the_agent_on_hyper-v_hosts}

Ensure you install the Backup agent on any Hyper-V hosts that you want {{site.data.keyword.baas_full_notm}} to protect.

1.  Navigate to **Data Protection > Sources**.
2.  Click **Download Backup agent**. Ensure the agent has been downloaded to the appropriate hosts.
3.  As an administrator with local system privileges, run the executable and complete the installation wizard.

    For Hyper-V 2016 servers, install the agent without additional components.

    For incremental backups of individual VMs hosted by Hyper-V on Windows Server 2012 R2, install the **File System CBT (Changed Block Tracker)** component.

4.  The agent starts automatically.


Guest Windows VM minimum recommended specifications: 2 GB RAM and the equivalent of a 1 GHz processor.

**Best practice**: For Hyper-V 2016, configure all VMs' Automatic Stop Action to shut down or turn off, instead of save. This results in all powered on VMs having minimal size .vmrs files. Because {{site.data.keyword.baas_full_notm}} does not back up .vmrs files that are greater than 10 MB (.vmrs file size, if set to save, scales to the amount of memory assigned to the VM). VMs in the saved state, by definition, generally have .vmrs files greater than 10 MB. This {{site.data.keyword.baas_full_notm}} Windows Agent registry value entry determines the maximum size of .vmrs file supported per backup: agent\_hyperv\_max\_vm\_config\_file\_size\_mb. Set it to zero to back up .vmrs files of all sizes.

## Upgrade the Agent on SCVMM and Hyper-V Hosts
{: #upgrade_the_agent_on_scvmm_and_hyper-v_hosts}

You can use the {{site.data.keyword.baas_full_notm}} UI to upgrade agents on a non-SCVMM cluster.

1.  Select **Data Protection > Sources**.
2.  Click the Hyper-V source name.
3.  If an upgrade is available, click **Upgrade Agents**.

    This upgrades eligible agents, such as a singleton SCVMM node or a proxy SCVMM.


To upgrade an agent individually, mouse over the corresponding actions menu and click **Upgrade Agent** (![](../../Resources/Images/i/icn/upgrade-h.svg)).

Agents on hosts that belong to an SCVMM cluster must be updated manually. Download a new Backup agent and run the installer.

## Download and Install the Agent on Nutanix AHV
{: #download_and_install_the_agent_on_nutanix_ahv}

File or folder level recovery of VMs in a Nutanix Acropolis Hypervisor requires a Linux or Windows Backup agent installed on the target VM. Follow these steps to install the Backup agent.

1.  Navigate to **Data Protection > Sources**.
2.  Click **Download Backup agent**. Ensure the agent has been downloaded to the appropriate server.
3.  As an administrator with local system privileges on that server, run the executable and complete the installation wizard. Do not install any CBT components.

    If you use the Windows command prompt to install the agent, you can use a command similar to this sample:

    Cohesity\_Agent\_6.3.0.0\_Win\_x64\_Installer.exe /type=onlyagent

    For more information about command parameters, see [Silently Install the Backup agent on Windows Physical Servers](#Silently).

4.  The agent starts automatically.

5.  Configure the firewall and ports as necessary. For more information, see [Manage Firewall Ports](../../Setup/OpenPorts.htm).

## Upgrade the Windows Agent on a Physical Server
{: #upgrade_the_windows_agent_on_a_physical_server}

Scheduled upgrades of agents are supported on {{site.data.keyword.baas_full_notm}}.

You can use the {{site.data.keyword.baas_full_notm}} UI to upgrade agents on a physical server.

1.  Navigate to **Data Protection** > **Sources**.

2.  If an upgrade is available, click **Upgrade Agents**.


To upgrade an agent individually, mouse over the corresponding actions menu (![](../../Resources/Images/i/icn/more-h.svg)) and click **Upgrade Agent**.

## Auto-upgrade of the {{site.data.keyword.baas_full_notm}} Windows Agent
{: #auto-upgrade_of_the_{{site.data.keyword.baas_full_notm}}_windows_agent}

{{site.data.keyword.baas_full_notm}} upgrade process supports auto-upgrade of the agents running on sources.

The option to auto-upgrade Backup agent is not available on the {{site.data.keyword.baas_full_notm}} with multi-tenancy configured. The auto-upgrade option is available only during cluster upgrade.

**To enable auto-upgrade:**

1.  In the {{site.data.keyword.baas_full_notm}} UI, navigate to **Settings** > **Update**, and select **Upgrade** tab.
2.  Select the **Upgrade Backup agent on all servers in Sources** checkbox to upgrade the agents on the source automatically after cluster upgrade.

## Uninstall the Agent
{: #uninstall_the_agent}

The following uninstallation methods are available:

*   Use the Add/Remove Programs in the Control Panel and Start Menu shortcuts in Windows versions where it is supported.
*   Use the uninstaller executable. The details of uninstaller executable are available in the registry value "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Uninstall\\{99246986-0300-4B21-9A1C-DE278D8EC715}\_is1\\UninstallString". The default value is "C:\\Program Files\\Cohesity\\Installer\\unins000.exe", assuming C: is the default operating system installation volume and "C:\\Program Files\\Cohesity" is the default Backup agent install location.

Preserve registry settings when uninstalling the agent

Uninstalling the Backup agent normally clears registration settings from the registry. You can preserve registration settings in the registry by using the `/preservesettings` flag when uninstalling from the command line. Preserving registry settings is not supported when uninstalling from the UI.

Sample command:

unins000.exe /preservesettings
