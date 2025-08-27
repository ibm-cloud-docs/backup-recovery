---

copyright:
  years: 2025
lastupdated: "2025-08-27"

keywords: plan, prepare, supported versions, port requirements, traffic, considerations, user store

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Plan and prepare for SAP HANA protection
{: #plan_and_prepare_for_sap_hana_protection}

Before you register your SAP HANA deployment as a source with {{site.data.keyword.cloud_notm}} Backup and Recovery and protect SAP HANA databases, ensure the following prerequisites:

- Identify the node(s) in your SAP HANA cluster with adequate resources to execute the SAP HANA connector.
- Ensure that the fully qualified domain name (FQDN) of the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster is resolvable from all the nodes in the SAP HANA cluster.
- Synchronize the SAP HANA Linux machine time with a Network Time Protocol (NTP) server. For detailed instructions, see [How to Sync Linux Server Time with an NTP Server](https://dyclassroom.com/reference-server/how-to-sync-linux-server-time-with-ntp-network-time-protocol-server){: external}.
- Ensure that the NTP time is in sync between the client and the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster. Perform the following on the client:
  - Verify if NTP is enabled and synchronized in the client using the following command:

        `timedatectl`

  - If the NTP time is not synchronized, use the following command to synchronize the NTP time

        `timedatectl set-ntp true`

- Use "bash" shell to install the SAP HANA Connector and to run backups and restore workflows of SAP HANA databases.
- A database user with database admin user privileges.
- Download and install the {{site.data.keyword.cloud_notm}} Backup and Recovery Linux Agent on the SAP HANA node.
- Download and install the SAP HANA connector.


## Supported versions
{: #supported_versions}

For more information on supported SAP HANA versions, see [SAP HANA ({{site.data.keyword.cloud_notm}} Backup and Recovery Connector Agents)](../ReleaseNotes/SupportedVersions.htm#SAP) versions.

## Port requirements
{: #port_requirements}

### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Port | Protocol | Type of Traffic |
| --- | --- | --- | --- | --- |
| {{site.data.keyword.cloud_notm}} Backup and Recovery cluster | SAP HANA Node | 50051 | TCP/IP | Control path for all workflows |
{: caption="Outgoing traffic " caption-side="bottom"}

### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Port | Protocol | Type of Traffic |
| --- | --- | --- | --- | --- |
| SAP HANA Local Host | SAP HANA Local Host | 59999 | TCP/IP | Required only if local-to-local communication (used for self-monitoring and debugging purposes) is blocked by the firewall. |
{: caption="Bidirectional traffic" caption-side="bottom"}

### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Port | Protocol | Type of Traffic |
| --- | --- | --- | --- | --- |
| SAP HANA Node | {{site.data.keyword.cloud_notm}} Backup and Recovery cluster | 11113 | TCP/IP | Backup and Recovery |
| SAP HANA Node | {{site.data.keyword.cloud_notm}} Backup and Recovery cluster | 11117 | TCP/IP | Backup and Recovery with secure gRPC |
{: caption="Incomming traffic" caption-side="bottom"}

## Considerations
{: #considerations}

Review and understand the following before you protect your SAP HANA database:

- SAP HANA 1.0 is not supported.
- SAP HANA Connector agent does not support auto-discovery and entity hierarchy; hence you cannot browse or search and select objects for backup.
- Indexing is not supported.
- Source-side deduplication is not supported for SAP HANA on PowerPC platform.
- You cannot protect multiple hosts using a single Protection Group.
- You cannot restore multiple objects, search and recover an object.
- Scheduling log backups in the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster is not supported, as SAP HANA automatically performs log backups.
- You cannot cancel a running restore operation.
- PITR restores are only supported to an alternate source, not to the same host from a replicated {{site.data.keyword.cloud_notm}} Backup and Recovery cluster.
- {{site.data.keyword.cloud_notm}} Backup and Recovery supports backup and restore of SAP HANA(x86) databases running on dual-stack (IPv4 and IPv6) mode or single-stack (only IPv6) mode.
- {{site.data.keyword.cloud_notm}} Backup and Recovery supports the recovery of SAP HANA 2.0 backups across both Intel x86-64 and Power PC platforms, allowing you to restore your critical SAP HANA data regardless of the underlying hardware architecture.
- Restoring backups from a higher version of SAP HANA to a lower version is not supported.
- {{site.data.keyword.cloud_notm}} Backup and Recovery supports SAP HANA scale-out deployments that use independent disks for each node.
- Only Pacemaker-based cluster stacks provided by RHEL and SLES are supported for SAP HANA HA cluster data protection.

    The Pacemaker version 0.9.167 is supported.
    {: note}

- For a secure connection between the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster and the SAP HANA database, you need to [generate an SAP HANA certificate](/docs/backup-recovery?topic=backup-recovery-sap_hana_certificate_authentication) for authentication.
- {{site.data.keyword.cloud_notm}} Backup and Recovery supports data protection for SAP HANA deployments with SSL-encrypted communication between the SAP HANA client and server.


## Create the secure user store (hdbuserstore) key
{: #create_the_secure_user_store_hdbuserstore_key}

Create the SAP HANA hdbuserstore key to connect to SYSTEMDB. For detailed instruction, see [Secure User Store (hdbuserstore)](https://help.sap.com/docs/PRODUCT_ID/b3ee5778bc2e4a089d3299b82ec762a7/dd95ac9dbb571014a7d7f0234d762fdb.html?state=PRODUCTION&version=2.0.02&locale=en-US){: external}. Use the following command to create the hdbuserstore key:

hdbuserstore SET <KEY> <host:port> <USERNAME> <PASSWORD>

Ensure that the user account of the secure user store has the following administrator privileges:
\- BACKUP ADMIN
\- DATABASE ADMIN
\- DATABASE START
\- CATALOG READ
\- MONITORING

**Sample:**

hdbuserstore SET SYS\_KEY test-FI:30013 SYSTEM testpassword

After you have created the hdbuserstore key, verify if the key has the required SAP HANA connection information, such as hostname or login credentials. Verify the key using the following command:

tecadm@test-FI:/usr/sap/TEC/home> hdbsql -AU SYS\_KEY
"select DATABASE\_NAME,ACTIVE\_STATUS from m\_databases"

| DATABASE | ACT |
| -------- | --- |
| SYSTEMDB | YES |
| TEC | YES |
| SRC | YES |
| DST | YES |
4 rows selected (overall time 28.150 msec; server time 659 usec)
{: caption="Database ACT " caption-side="bottom"}

tecadm@test-FI:/usr/sap/TEC/home>

## Download and install the {{site.data.keyword.cloud_notm}} Backup and Recovery linux agent
{: #download_and_install_the_cohesity_linux_agent}

The {{site.data.keyword.cloud_notm}} Backup and Recovery Linux Agent is available as script-based and RPM-based installers, based on your requirements download the installer and install it accordingly on the SAP HANA node(s). For detailed instructions, see [Download and Install the Linux Agent](/docs/backup-recovery?topic=backup-recovery-install-and-manage-the-agent-on-linux-servers).

For multi-node SAP HANA deployments, ensure that you install the {{site.data.keyword.cloud_notm}} Backup and Recovery Linux Agent on the master node in the deployment.

## Download the SAP HANA connector installer
{: #download_the_sap_hana_connector_installer}

{{site.data.keyword.cloud_notm}} Backup and Recovery provides a SAP HANA connector for SAP HANA to backup and restore SAP HANA databases. The SAP HANA connector for SAP HANA is available as a script or RPM-based installer on the [{{site.data.keyword.cloud_notm}} Backup and Recovery Download](https://protectiondomain0101.us-east.backup-recovery.cloud.ibm.com/files/bin/installers/cohesity-hana-backint-7.2.15-1.x86_64.rpm){: external} portal. Log on to the [{{site.data.keyword.cloud_notm}} Backup and Recovery Download](https://protectiondomain0101.us-east.backup-recovery.cloud.ibm.com/files/bin/installers/cohesity-hana-backint-7.2.15-1.x86_64.rpm){: external} portal, download the respective SAP HANA Connector installer for {{site.data.keyword.cloud_notm}} Backup and Recovery SAP HANA BACKINT plug-in, and the SAP HANA Connector installer to the SAP HANA host.
