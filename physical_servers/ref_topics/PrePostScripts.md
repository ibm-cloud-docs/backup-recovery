---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Configure Pre & Post Scripts


{{site.data.keyword.baas_full_notm}} provides you the option to run user-defined scripts as a part of the Protection Group. When creating a Protection Group, you can configure:

*   Pre Scripts: Scripts that will be executed before the protection run.

*   Post Scripts: Scripts that will be executed after the protection run.

*   Post Snapshot Scripts: Scripts that will be executed after the snapshot has been created on the source.


## Supported Sources

You can run the Pre Script and Post Script for the following sources:

*   [Physical Servers](JobServerPhysical_FileBased.htm)

*   Databases - [MS SQL Server](../../MSSQL/SQLProtectVolBased.htm) and [Oracle](../../Oracle/CreatingDataProtectionJobOracle.htm) Database

*   [NAS](JobNAS.htm)

*   [SAN](JobPure.htm)


Depending on whether the Backup agent is installed on the source, the settings required to configure the pre and post scripts in the Protection Group vary.

## Configure Scripts for Sources with Agent Installed

Out of the supported sources for pre and post-scripts, physical servers and databases require Backup agent to be installed on the source.

Following are the settings required for configuring the pre and post scripts for sources with Agent installed:

### Pre-Script

*   **Script Path**: Depending on the agents, specify the full path or name of the script as mentioned in the **Note** section below. A script matching the specified name must be available in the user\_scripts directory found in the agent installation directory on the protected server. (See the table below for details about the directory path.)   The script can be in any format, for example: shell script, batch file, executable (`.exe`), etc. The script must be configured with the permissions to allow execution (`+x`).

    *   For Physical servers, MS SQL server and Oracle, only the script name is required (provided the script is available in the user\_scripts directory found in the agent installation directory).

    *   For NAS and SAN sources, the absolute/full path to the script is required.

    *   On Windows platforms, PowerShell, Python, or Perl scripts (any script that is not a .exe file) must be embedded within a .bat script before they are called.


    {{site.data.keyword.baas_full_notm}} does not validate the content of the script and is not responsible for the content of the script or any actions done by the script. You must verify the script actions before a script or executable is run.

*   **Script Params**: Optional field for passing one or more parameters to the script. All specified parameter values are passed as a single string to the script.

*   **Timeout**: Optional field for specifying how long a script is allowed to execute before the {{site.data.keyword.baas_full_notm}} stops the execution of the script. By default, the timeout is 15 minutes. If no value is specified, there is no timeout and the script is not stopped. This can cause a protection run to hang if the script never completes.

*   **Continue Backup if script fails**: This toggle applies only to Pre-Scripts and is enabled by default. When this toggle is enabled, the Protection Group back ups the objects even if the Pre-Script fails. If this toggle is disabled and the Pre-Script fails, the backup is canceled but the Post-Script still runs.


### Post-Script

*   **Script Path**: Depending on the agents, specify the full path or name of the script as mentioned in the **Note** section below. A script matching the specified name must be available in the specified directory. The script can be in any format, such as a shell script or executable. The script must be configured with the permissions to allow execution (+x).

    *   For Physical servers, MS SQL server and Oracle, only the script name is required (provided the script is available in the user\_scripts directory found in the agent installation directory).

    *   For NAS and SAN sources, the absolute/full path to the script is required.

    *   On Windows platforms, PowerShell, Python, or Perl scripts (any script that is not a .exe file) must be embedded within a .bat script before they are called.


    {{site.data.keyword.baas_full_notm}} does not validate the content of the script and is not responsible for the content of the script or any actions done by the script. You must verify the script actions before a script or executable is run.

*   **Script Params**: Optional field for passing one or more parameters to the script. All specified parameter values are passed as a single string to the script.

*   **Timeout**: Optional field for specifying how long a script is allowed to execute before the {{site.data.keyword.baas_full_notm}} stops the execution of the script. By default, the timeout is 15 minutes. If no value is specified, there is no timeout and the script is not stopped. This can cause a protection run to hang if the script never completes.


More about failures, cancellations and log files:

*   If the backup is successful but the Pre-Script or Post-Script fails, a warning is issued with the script error. A script failure occurs for any of the following conditions: 

    *   script is not found in the expected location

    *   script cannot be executed

    *   script returns a failure return code of non-zero


    A script is successful when the return code is 0.

*   If a Post-Script is configured, the script always executes even if the backup fails.
*   If a protection run is canceled, all subsequent phases are canceled.
    *   For example, if a protection run is canceled while the Pre-Script is running, the Pre-Script is canceled, then the backup task is canceled and then the Post-Script is canceled.
    *   For example, if a protection run is canceled while the backup task is running, the backup task is canceled and then the Post-Script is canceled.
*   Script execution log files can be found in the `user_script_logs` directory found in the agent log directory on the protected server. Both Stdout and Stderr from the script execution are recorded in the script log files. (See the table below for details about the directory path.) 

**Available Environment Variables**

The {{site.data.keyword.baas_full_notm}} populates the following environment variables with values before the Pre-Script or the Post-Script is run. You can use these environment variables in your script.


| Variable  <br>Name | Available from Script | Description |
| --- | --- | --- |
| `COHESITY_JOB_ID` | Pre-Script  <br>and  <br>Post-Script | Specifies the {{site.data.keyword.baas_full_notm}} id assigned to the Protection Group. |
| `COHESITY_JOB_NAME` | Pre-Script  <br>and  <br>Post-Script | Specifies the name of the Protection Group as displayed in the {{site.data.keyword.baas_full_notm}} Console. |
| `COHESITY_BACKUP_TYPE` | Pre-Script  <br>and  <br>Post-Script | `kRegular` indicates an incremental (CBT) backup. Incremental backups utilizing CBT (if supported) are captured of the target protection objects. The first run of a kRegular schedule captures all the blocks.<br><br>`kFull` indicates a full (no CBT) backup. A complete backup (all blocks) of the target protection objects are always captured and Change Block Tracking (CBT) is not utilized.<br><br>`kLog` indicates a database log backup. Capture the database transaction logs to allow rolling back to a specific point in time.<br><br>`kSystem` indicates a system backup. System backups are used to do bare metal recovery of the system to a specific point in time. |
| `COHESITY_JOB_STATUS` | Post-Script | SUCCEEDED indicates the protection run ran successfully.<br><br>ERRORED indicates the protection run reported an error. |

To access an environment variable in PowerShell, use:`$env:<variable name>`.

Example

`Write-Host "COHESITY_BACKUP_HOST_ENTITY =" $env:COHESITY_BACKUP_HOST_ENTITY`

[![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)The following is a sample script for Linux](#)

#!/bin/bash
echo "Hello...This is a sample script".
sleep 60
echo "Executing Script"

[![Closed](../../../Skins/Default/Stylesheets/Images/transparent.gif)The following is a sample script for Windows](#)

net start OracleOraDB12Home1TNSListener
net start OracleServiceMUC99
net start OracleServiceCATPRD
@oradim -startup -sid MUC99
@oradim -startup -sid CATPRD

**Paths to User Scripts and Script Logs for Various Agents**


| Operating Systems | Directory Path Description | Path |
| --- | --- | --- |
| **Linux** | Where Pre-Script and Post-Script must be stored to be found by the {{site.data.keyword.baas_full_notm}}. | `INSTALL_DIR/user_scripts` |
| Where the script execution logs are written by the {{site.data.keyword.baas_full_notm}} when the script is run. | `INSTALL_DIR/logs/user_script_logs/` |
| If the agent is installed using the `cohesityagent` user, the default `INSTALL_DIR` is... | `/home/cohesityagent/cohesityagent/` |
| If the agent is installed using the `root` user, the default `INSTALL_DIR` is... | `/root/cohesityagent` |
| **Windows** | Where Pre-Script and Post-Script must be stored to be found by the {{site.data.keyword.baas_full_notm}}. | `INSTALL_DIR\user_scripts` |
| The default `INSTALL_DIR` is... | `C:\Program Files\Cohesity` |
| Where the script execution logs are written by the {{site.data.keyword.baas_full_notm}} when the script is run. | `C:\ProgramData\Cohesity\user_script_logs` |
| Solaris<br><br>AIX<br><br>RHEL 5.x | Where Pre-Script and Post-Script must be stored to be found by the {{site.data.keyword.baas_full_notm}}. | `INSTALL_DIR/user_scripts` |
| The default `INSTALL_DIR` is... | `/usr/local/cohesity/agent` |
| Where the script execution logs are written by the {{site.data.keyword.baas_full_notm}} when the script is run. | `LOG_DIR/user_script_logs` |
| The default `LOG_DIR` is... | `/var/log/cohesity/agent` |

## Configure Scripts for Sources without Agent Installed

Out of the supported sources for pre and post-scripts, NAS and SAN sources do not require Backup agent to be installed on the source.

Following are the settings required for configuring the pre and post scripts for sources without Agent installed:

*   **Hostname or IP Address (Unix only)**: Specify either the hostname or the IP address of the remote Unix server where the script will run. (Only Unix is currently supported.)

*   **Cluster SSH Public Key**: Click Copy Key to Clipboard to copy the entire string including the ssh-rsa string preceding the key, the public key and the username@cluster\_name string after the key. Open the authorized\_keys file (typically located in the ~/.ssh directory) and paste the entire string to the end of the file.

*   **Username**: Specify the username on the remote Unix machine that will execute the script. The username must match the username specified in the authorized\_keys file.


You can configure a Pre-Script, a Post-Script or both scripts.

### Pre-Script

*   **Script Path**: Depending on the agents, specify the full path or name of the script as mentioned in the **Note** section below. A script matching the specified name must be available in the specified directory. The script can be in any format, such as a shell script or executable. The script must be configured with the permissions to allow execution (+x).

    *   For Physical servers, MS SQL server and Oracle, only the script name is required (provided the script is available in the user\_scripts directory found in the agent installation directory).

    *   For NAS and SAN sources, the absolute/full path to the script is required.


    {{site.data.keyword.baas_full_notm}} does not validate the content of the script and is not responsible for the content of the script or any actions done by the script. You must verify the script actions before a script or executable is run.

*   **Script Params**: Optional field for passing one or more parameters to the script. All specified parameter values are passed as a single string to the script.

*   **Timeout**: Optional field for specifying how long a script is allowed to execute before the {{site.data.keyword.baas_full_notm}} stops the execution of the script. By default, the timeout is 15 minutes. If no value is specified, there is no timeout and the script is not stopped. This can cause a protection run to hang if the script never completes.

*   **Continue Backup if script fails**: This toggle applies only to Pre-Scripts and is enabled by default. When this toggle is enabled, the Protection Group backs up the objects even if the Pre-Script fails. If this toggle is disabled and the Pre-Script fails, the backup is canceled but the Post-Script still runs.


### Post-Script

*   **Script Path**: Depending on the agents, specify the full path or name of the script as mentioned in the **Note** section below. A script matching the specified name must be available in the specified directory. The script can be in any format, such as a shell script or executable. The script must be configured with the permissions to allow execution (+x).

    *   For Physical servers, MS SQL server and Oracle, only the script name is required (provided the script is available in the user\_scripts directory found in the agent installation directory).

    *   For NAS and SAN sources, the absolute/full path to the script is required.


    {{site.data.keyword.baas_full_notm}} does not validate the content of the script and is not responsible for the content of the script or any actions done by the script. You must verify the script actions before a script or executable is run.

*   **Script Params**: Optional field for passing one or more parameters to the script. All specified parameter values are passed as a single string to the script.

*   **Timeout**: Optional field for specifying how long a script is allowed to execute before the {{site.data.keyword.baas_full_notm}} stops the execution of the script. By default, the timeout is 15 minutes. If no value is specified, there is no timeout and the script is not stopped. This can cause a protection run to hang if the script never completes.


More about failures, cancellations and log files:

*   If the backup is successful but the Pre-Script or Post-Script fails, a warning is issued with the script error. A script failure occurs for any of the following conditions: 

    *   script is not found in the expected location

    *   script cannot be executed

    *   script returns a failure return code of non-zero


    A script is successful when the return code is 0.

*   If a Post-Script is configured, the script always executes even if the backup fails.
*   If a protection run is canceled, all subsequent phases are canceled.
    *   For example, if a protection run is canceled while the Pre-Script is running, the Pre-Script is canceled, then the backup task is canceled and then the Post-Script is canceled.
    *   For example, if a protection run is canceled while the backup task is running, the backup task is canceled and then the Post-Script is canceled.
*   Script execution log files can be found in the `user_script_logs` directory found in the agent log directory on the protected server. Both Stdout and Stderr from the script execution are recorded in the script log files. (See the table below for details about the directory path.) 

**Available Environment Variables**

The {{site.data.keyword.baas_full_notm}} populates the following environment variables with values before the Pre-Script or the Post-Script is run. You can use these environment variables in your script.


| Variable  <br>Name | Available from Script | Description |
| --- | --- | --- |
| `COHESITY_JOB_ID` | Pre-Script  <br>and  <br>Post-Script | Specifies the {{site.data.keyword.baas_full_notm}} id assigned to the Protection Group. |
| `COHESITY_JOB_NAME` | Pre-Script  <br>and  <br>Post-Script | Specifies the name of the Protection Group as displayed in the {{site.data.keyword.baas_full_notm}} Console. |
| `COHESITY_BACKUP_TYPE` | Pre-Script  <br>and  <br>Post-Script | `kRegular` indicates an incremental (CBT) backup. Incremental backups utilizing CBT (if supported) are captured of the target protection objects. The first run of a kRegular schedule captures all the blocks.<br><br>`kFull` indicates a full (no CBT) backup. A complete backup (all blocks) of the target protection objects are always captured and Change Block Tracking (CBT) is not utilized.<br><br>`kLog` indicates a database log backup. Capture the database transaction logs to allow rolling back to a specific point in time.<br><br>`kSystem` indicates a system backup. System backups are used to do bare metal recovery of the system to a specific point in time. |
| `COHESITY_JOB_STATUS` | Post-Script | SUCCEEDED indicates the protection run ran successfully.<br><br>ERRORED indicates the protection run reported an error. |
| `COHESITY_BACKUP_VOLUME_SNAPSHOT_SUFFIX` | Pre-Script | Contains the suffix {{site.data.keyword.baas_full_notm}} uses for creating the Pure Storage Volume snapshot. |
| COHESITY\_BACKUP\_GROUP\_SNAPSHOT\_SUFFIX | Post-Script | Contains the suffix {{site.data.keyword.baas_full_notm}} uses for creating the Pure Protection Group snapshot. |
