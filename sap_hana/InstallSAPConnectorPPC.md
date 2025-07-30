---

copyright:
  years: 2025
lastupdated: "2025-07-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Install and manage the sap hana connector on ibm powerpc platform
{: #install_and_manage_the_sap_hana_connector_on_ibm_powerpc_platform}

This topic covers instructions on how to install the SAP HANA Connector on IBM PowerPC Platform.

This is an Early Access feature. Contact your {{site.data.keyword.cloud_notm}} Backup and Recovery account team to enable the feature.

## Prerequistes
{: #prerequistes}

Before you register your SAP HANA deployment as a source with {{site.data.keyword.cloud_notm}} Backup and Recovery and protect SAP HANA databases, ensure the following prerequisites:

- Identify the node(s) in your SAP HANA cluster with adequate resources to execute the SAP HANA connector.
- SAP HANA node(s) must be using JDK version 1.8 or higher.
- Ensure that the fully qualified domain name (FQDN) of the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster is resolvable from all the nodes in the SAP HANA cluster.
- Synchronize the SAP HANA Linux machine time with a Network Time Protocol (NTP) server. For detailed instructions, see [How to Sync Linux Server Time with an NTP Server](https://dyclassroom.com/reference-server/how-to-sync-linux-server-time-with-ntp-network-time-protocol-server){: external}.
- Ensure that the NTP time is in sync between the client and the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster. Perform the following on the client:

  - Verify if NTP is enabled and synchronized in the client using the following command:

        `timedatectl`

  - If the NTP time is not synchronized, use the following command to synchronize the NTP time

        `timedatectl set-ntp true`

- Use "`bash`" shell to install the SAP HANA Connector and to run backups and restore workflows of SAP HANA databases.
- A database user with database admin user privileges.
- [Download and install the {{site.data.keyword.cloud_notm}} Backup and Recovery Linux Agent](PlanandPrepare.htm#Download2) (`el-java_cohesity-agent-7.0-1.ppc64le.rpm`) on the SAP HANA node.
- [Download the SAP HANA connector Installer](PlanandPrepare.htm#Download), (`cohesity-hana-java-backint-7.0-1.noarch.rpm`), for IBM PowerPC Platform.
- Configure the following environment variables on the SAP HANA node(s):


    | Environment Variable | Description | Command |
    | --- | --- | --- |
    | SERVICE\_USER | The <SID>adm user account that must own the backup files on the {{site.data.keyword.cloud_notm}} Backup and Recovery view. If you have not created the user account, then create an Operating System (OS) user account in the following format: <SID>adm.<br><br>Where <SID> is a unique three-character code called SAP System Identification (SID). It is available for every R/3 installation (SAP system) that consists of a database server and several application servers. | `export SERVICE_USER=qqqadm`<br><br>Where `qqqadm` is the <SID>adm user account and `qqq` is the SAP System Identification (SID). |
    | WORKFLOW | The type of workflow you intend to install the SAP HANA connector. For UDA workflow, specify `UDA`. | `export WORKFLOW=UDA` |
    | SYSTEMDB\_KEY | The SAP HANA HDBUSERSTORE key to connect to the SYSTEM DB.<br><br>You can generate the HDBUSERSTORE key using the following command:<br><br>`hdbuserstore -i set SYS_KEY <HOSTNAME>:<SQL_PORT> <User> <Password>`<br><br>For detailed instruction, see [hdbuserstore Commands](https://help.sap.com/docs/PRODUCT_ID/b3ee5778bc2e4a089d3299b82ec762a7/ddbdd66b632d4fe7b3c2e0e6e341e222.html?state=PRODUCTION&version=2.0.02&locale=en-US){: external} in SAP HANA documentation. | `export SYSTEMDB_KEY=<HDBUSERSTORE key>` |
    | CONTROL\_NODES | The IP address or hostname of the SAP HANA machine. | `export CONTROL_NODES="<IP_ADDRESS>"` |
    | SOURCE\_NAME | A name for the SAP HANA source you want to use while registering the SAP HANA as a source on the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster. | `export SOURCE_NAME="myhostname"` |


## Install the sap hana connector
{: #install_the_sap_hana_connector}

On the SAP HANA node where you have copied the SAP HANA connector (`cohesity-hana-java-backint-7.0-1.noarch.rpm`), perform the following to install the SAP HANA connector.

For multi-node SAP HANA deployments, ensure that:

- You install the SAP HANA connector on the machine where you have installed {{site.data.keyword.cloud_notm}} Backup and Recovery Linux Agent
- The SAP HANA Connector is installed on the shared NFS path, to which all the SAP HANA nodes have access.


To install the SAP HANA connector:

1. Log on to the SAP HANA node’s bash CLI using <SID>adm user.

2. Make the SAP HANA connector installer (`cohesity-hana-java-backint-7.0-1.noarch.rpm`) executable using the following command:

    \> chmod +x cohesity-hana-java-backint-7.0-1.noarch.rpm

3. Install the SAP HANA connector using the following command:

    \> sudo -E rpm -ivh cohesity-hana-java-backint-7.0-1.noarch.rpm


The following is a sample output of the successful installation:

sles-sap:kumadm> export SERVICE\_USER=kumadm
sles-sap:kumadm> export SYSTEMDB\_KEY=SYS\_KEY 
sles-sap:kumadm> sudo -E rpm -ivh cohesity-hana-java-backint-7.0-1.noarch.rpm
Preparing...                         ################################# \[100%\]                                                                                         
Pre-install/Upgrade section                                                                                                                                            
==> Service user is kumadm
==> System db key is SYS\_KEY                                                       
==> WORKFLOW is UDA by default, hence cluster ips and view name are auto  populated during                                                                             
      DB workflow through UDA framework                                                                                                                                
==>                                                                                                                                                                    
==> User kumadm does not exist in /etc/passwd                        
==> Checking if User kumadm exists using getent command.                          
==> User kumadm found with getent command                                          
==> Package installation is happening at /opt/cohesity/hana\_java\_plugin           
Updating / installing...                                                          
   1:cohesity-hana-java-backint-ENG\_27################################# \[100%\]     
==> Post-install/Upgrade section                                                                                                                                       
workflow is set to UDA                                                                                                                                                 
LOG\_DIR\_PATH is not set, hence using /opt/cohesity/hana\_java\_plugin                                                                                                    
Backint log dir location is /opt/cohesity/hana\_java\_plugin/cohesity\_backint\_logs
Service user is set to kumadm                                                      
==> hdbsql is /usr/sap/KUM/HDB01/exe/hdbsql, hdbuserstore is /usr/sap/KUM/HDB01/exe/hdbuserstore                                                                       
Control Nodes is sles-sap, source name is sles-sap                                 
Creating soft link from /opt/cohesity/hana\_java\_plugin/bin/backint.sh to /usr/sap/KUM/SYS/global/hdb/opt/hdbbackint                                                    
Creating soft link from /opt/cohesity/hana\_java\_plugin/uda\_scripts to /opt/cohesity/agent/uda\_scripts                                                                  
updating the LOG\_DIR\_TEMPLATE with /opt/cohesity/hana\_java\_plugin for scripts located at                                                                               
  /opt/cohesity/hana\_java\_plugin location                                                                                                                              
updating the INSTALL\_DIR\_TEMPLATE with /opt/cohesity/hana\_java\_plugin for scripts located at                                                                           
  /opt/cohesity/hana\_java\_plugin location               
Executing backint -V                                                               
{{site.data.keyword.cloud_notm}} Backup and Recovery DataProtect 6.5 for SAP HANA (PPC)                                   
backint.jar, Build Version: b7efc5cd                                 
Successful Installation of UDA HANA Java Connector

## Uninstall the sap hana connector
{: #uninstall_the_sap_hana_connector}

To uninstall the SAP HANA Connector using the root user account, use the following command:

\> sudo -E rpm -evh cohesity-hana-java-backint-7.0-1.noarch.rpm

The following is a sample output of the successful uninstallation:

sles-sap:adm> sudo -E rpm -evh cohesity-hana-java-backint-7.0-1.noarch.rpm
\[sudo\] password for root: 
Preparing...                         ################################# \[100%\]
==> Pre-uninstall section
grep: /opt/cohesity/hana\_java\_plugin/profile/cohesity\_param\_file\_: No such file or directory
Log directory is set to  in /opt/cohesity/hana\_java\_plugin/profile/cohesity\_param\_file\_
Cleaning up / removing...
   1:cohesity-hana-java-backint-ENG\_27################################# \[100%\]
==> Post-uninstall section
==> Deleting RPM install files at /opt/cohesity/hana\_java\_plugin
rm -rf /opt/cohesity/hana\_java\_plugin
UDA HANA Java Connector is successfully uninstalled

## Upgrade the sap hana connector
{: #upgrade_the_sap_hana_connector}

To upgrade the SAP HANA connector, run the following command on all SAP HANA nodes:

sudo -E rpm -Uvh cohesity-hana-java-backint-7.0-1.noarch.rpm
