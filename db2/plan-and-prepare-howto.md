---
copyright:
  years: 2026
lastupdated: "2026-11-02"

keywords: ibm db2, backup, disaster recovery, planning, preparation, prerequisites, linux agent, db2 connector

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Planning and preparing for IBM Db2 protection
{: #plan-prepare-db2}

Before you register your IBM Db2 deployment as a source with {{site.data.keyword.baas_full_notm}}, ensure that you complete the planning and preparation steps to enable successful backup and recovery operations.
{: shortdesc}

## Before you begin
{: #plan-prepare-prereqs}

* The user account of the Db2 instance must have SYSADM, SYSCTRL, or SYSMAINT privilege
* You must have administrative access to your IBM Db2 instances
* Verify network connectivity between your Db2 servers and the backup infrastructure

## Reviewing supported versions
{: #supported-versions}

For more information on supported IBM Db2 versions, see [Supported Software](/docs/backup-recovery?topic=backup-recovery-supported-software#ibm-db2).

## Understanding port requirements
{: #port-requirements}

Review the port requirements for IBM Db2 protection to ensure proper network configuration.

### Outgoing traffic
{: #outgoing-traffic}

| Source | Destination | Port | Protocol | Type of traffic |
| ------ | ----------- | ---- | -------- | --------------- |
| {{site.data.keyword.baas_full_notm}}cluster | IBM Db2 Node | 50051 | TCP/IP | Control path for all workflows |
{: caption="Outgoing traffic port requirements" caption-side="bottom"}

### Bidirectional traffic
{: #bidirectional-traffic}

| Source | Destination | Port | Protocol | Type of traffic |
| ------ | ----------- | ---- | -------- | --------------- |
| IBM Db2 Local Host | IBM Db2 Local Host | 59999 | TCP/IP | Required only if local-to-local communication (used for self-monitoring and debugging purposes) is blocked by the firewall |
| IBM Db2 Local Host | IBM Db2 Local Host | 58881 | TCP/IP | Local data transfer |
| IBM Db2 Local Host | IBM Db2 Local Host | 58882 | TCP/IP | Local data transfer |
{: caption="Bidirectional traffic port requirements" caption-side="bottom"}

### Incoming traffic
{: #incoming-traffic}

| Source | Destination | Port | Protocol | Type of traffic |
| ------ | ----------- | ---- | -------- | --------------- |
| IBM Db2 Node | {{site.data.keyword.baas_full_notm}}cluster | 11113 | TCP/IP | Backup and Recovery |
| IBM Db2 Node | {{site.data.keyword.baas_full_notm}}cluster | 11117 | TCP/IP | Backup and Recovery with secure gRPC |
{: caption="Incoming traffic port requirements" caption-side="bottom"}

## Reviewing considerations
{: #considerations}

Review and understand the following considerations before you protect your IBM Db2 cluster:

* Only database-level backup is supported.
* Once a database is configured to be protected using {{site.data.keyword.baas_full_notm}}, users should not initiate a manual backup on the same database outside of the {{site.data.keyword.baas_full_notm}}cluster.
* The same database should not be part of multiple Protection Groups.
* Backup with compression and encryption options are not supported.
* Using the {{site.data.keyword.baas_full_notm}}Db2 Connector, you can protect an IBM Db2 HADR active/standby cluster. However, you can backup or restore from the active node only.
* Protection of Db2 databases with Purescale is not supported.
* If a hole is introduced in the backup timeline, then point-in-time recovery (PITR) is possible only after the subsequent successful full or incremental backup is completed.
* Source side deduplication is not supported.
* Log archival method configured for a source during source registration cannot be changed. Any change in the log archival method needs re-registration of the source with different archival method.
* {{site.data.keyword.baas_full_notm}}supports backup and restore of Db2 databases running on dual-stack (IPv4 and IPv6) mode or single-stack (only IPv6) mode.
* The {{site.data.keyword.baas_full_notm}}Db2 connector is only compatible with the bash shell and cannot be used with any other shells.
* If you have set a limit on Java heap memory usage, ensure that at least 512 MB is allocated per VIP. For example, if you are using 4 VIPs, the total Java heap memory usage limit should be at least 2 GB.
* {{site.data.keyword.baas_full_notm}}supports data protection for IBM Db2 deployments with SSL-encrypted communication between the IBM Db2 client and server.

### Supported and unsupported Protection Group configurations
{: #protection-group-scope-dup}

Review the following supported and unsupported configurations before you create a Protection Group for IBM Db2.

**Supported:**

* Multiple databases from the same registered IBM Db2 source in a single Protection Group.
* Multiple hosts that belong to the same IBM Db2 topology, for example a DPF (Database Partitioning Feature) cluster or an HADR (High Availability Disaster Recovery) setup, when those hosts are registered together as a single UDA/Db2 source.

**Not supported:**

* A single Protection Group that covers independent IBM Db2 sources or hosts. One Protection Group cannot cover IBM Db2 sources that were registered separately, whether configured through the UI or the API. Each independently registered Db2 source must be protected by its own Protection Group.

## Verifying prerequisites
{: #prerequisites}

Ensure that you complete the following prerequisites before you protect your IBM Db2 deployment.

### Configuring user permissions
{: #configure-permissions}

You can use a root user or non-root user to install the {{site.data.keyword.baas_full_notm}}Linux agent and IBM Db2 Connector. If you choose to use a non-root user, you must add the `sudo` privileges for the non-root user using the following syntax:

```text
<user_name> ALL=(ALL) NOPASSWD:SETENV: /path/to/command1, /path/to/command2, /path/to/command3
```
{: pre}

The following are the `sudo` commands for IBM Db2 Connector:

```text
ibmagent ALL=NOPASSWD:SETENV:/usr/bin/rpm, /usr/bin/find, /usr/bin/stat,/usr/bin/test, /usr/bin/mv, /usr/bin/cat, /usr/bin/mkdir, /usr/bin/rm, /usr/bin/cp, /usr/bin/chown, /usr/bin/chmod, /usr/bin/kinit, /opt/ibm/db2/bin/named_pipe_snap_fs_util, /usr/bin/su, /usr/bin/java, /usr/bin/netstat, /usr/bin/sed, /usr/bin/cmp, /usr/bin/tee, /usr/bin/wc,/usr/bin/truncate,/usr/sbin/runuser, /usr/bin/ps, /usr/bin/cat, /usr/bin/true
```
{: pre}

Where `/opt/ibm/db2` is the IBM Db2 connector installation directory.

If the `ibmagent` user entry is already present in `/etc/sudoers`, append the list of commands to the existing entry.
{: note}

The actual paths for each command might vary depending on the Platform and OS distribution. For more information, see [Download and Install the Linux Agent](/docs/backup-recovery?topic=backup-recovery-agent-download-install).
{: note}

### Verifying system authorities
{: #verify-authorities}

Ensure that the user account of the Db2 instance is assigned the required system authorities to perform backup or restore operations on a Db2 instance.

To perform backup or restore operations on a Db2 instance, the protection user account of the Db2 instance must have one of the following levels of authority:

SYSADM
:   Assigned to the group specified by the `sysadm_group` configuration parameter. Users with SYSADM authority can also perform the functions of users with SYSCTRL and SYSMAINT authorities.

SYSCTRL
:   Assigned to the group specified by the `sysctrl_group` configuration parameter. Users with SYSCTRL authority can also perform the functions of users with SYSMAINT authority.

SYSMAINT
:   Assigned to the group specified by the `sysmaint_group` configuration parameter.

When restoring to a new database server, either SYSADM or SYSCTRL authority is required.
{: note}

### Configuring Kerberos authentication
{: #configure-kerberos}

For Kerberos authentication, the `keytab` file must be available on all nodes at the given location and must be accessible by the Db2 instance and the protection (backup/restore) user.

### Installing Java
{: #install-java}

Install Java 1.8.0_382 or higher version on all the IBM Db2 nodes.

### Configuring database settings
{: #configure-database}

For incremental backup, set `TRACKMOD` to `YES` in the database configuration.

If you are using the Dump and Sweep based log archival (log backup) method, set `LOGARCHMETH1` to a specific path.

For more information, see the [BACKUP DATABASE](https://www.ibm.com/docs/en/db2/11.5?topic=commands-backup-database){: external} command reference. If you have not performed the mentioned database configurations then you must manually configure the database configuration on each database in the cluster and requires database downtime. The following is the sample steps for the database configuration:

```text
db2 connect to <database>
db2 update database configuration using LOGARCHMETH1 'DISK:<log_archive_path>'
db2 "update db cfg using TRACKMOD YES"
db2 terminate
db2 deactivate database <database>
db2 backup database <database> to /dev/null
db2 activate database <database>
```
{: pre}

The database goes into the BACKUP PENDING state after applying the required configurations. As shown in the sample steps, ensure that you take an offline backup of the database to prevent the BACKUP PENDING state. For more information, see the IBM Support article on [BACKUP PENDING state](https://www.ibm.com/support/pages/db2-how-solve-error-sql1116n-sql0290n-due-backup-pending-state-which-prevent-accessing-database-or-table-space){: external}.
{: note}

### Configuring secure gRPC
{: #configure-secure-grpc}

To securely connect to the {{site.data.keyword.baas_full_notm}} cluster while reading or writing data, you can use secure gRPC.

It is mandatory to connect to the {{site.data.keyword.baas_full_notm}} cluster securely using secure gRPC when writing or reading data.
{: important}

Using non-secure gRPC for data transfer between {{site.data.keyword.baas_full_notm}} and database hosts will result in unsecured communication. Enabling secure gRPC will increase data transfer time between {{site.data.keyword.baas_full_notm}} and database hosts compared to non-secure communication.
{: note}

To use secure gRPC:

1. Generate the certificate config file on the {{site.data.keyword.baas_full_notm}} node using the following command:

   ```text
   ~/<release-version>/crux/bin/client_tls_cert_generator_exec -cert_name <clustername> -output_file /home/ibm/<clustername>.cfg
   ```
   {: pre}

2. Copy `/home/ibm/<clustername>.cfg` from the {{site.data.keyword.baas_full_notm}} node to all IBM Db2 nodes at the same location. For example, `/opt/certs/<clustername>.cfg`.

   Ensure that the `root` or `non-root` user used to install the {{site.data.keyword.baas_full_notm}} Linux agent and IBM Db2 Connector has read access to the copied certificate config file.
   {: note}

3. While registering the IBM Db2 source, provide the path of the certificate config file in the **{{site.data.keyword.baas_full_notm}} SSL Settings** field.

## Downloading and installing the {{site.data.keyword.baas_full_notm}} Linux Agent
{: #install-linux-agent}

The {{site.data.keyword.baas_full_notm}} Linux Agent is available as an RPM-based installer. Based on your requirements, download the installer and install it accordingly on the node(s) of the IBM Db2 cluster.

For detailed instructions, see [Download and Install the Linux Agent](/docs/backup-recovery?topic=backup-recovery-linux_agent_install_manage).

Ensure that you install the {{site.data.keyword.baas_full_notm}} Linux Agent using the RPM-based installer.
{: note}

## Downloading and installing the IBM Db2 Connector
{: #install-db2-connector}

{{site.data.keyword.baas_full_notm}} provides IBM Db2 connector RPM for backup and restore of IBM Db2 deployments.

### Downloading the IBM Db2 Connector
{: #download-connector}

{{site.data.keyword.baas_full_notm}} provides an IBM Db2 Connector to backup and restore IBM Db2 databases. The IBM Db2 Connector is available as a datasource agent.

1. Log on to the [{{site.data.keyword.baas_full_notm}}Download](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) portal.
2. Download the respective datasource agent for IBM Db2 Connector.
3. Copy the datasource agent to the IBM Db2 node.

### Installing the {{site.data.keyword.baas_full_notm}}Db2 Connector RPM
{: #install-connector-rpm}

Before you install the {{site.data.keyword.baas_full_notm}}Db2 connector, ensure that you have `sudo` privileges.

To install the {{site.data.keyword.baas_full_notm}}Db2 connector:

1. Log on to the IBM Db2 node where you have copied the {{site.data.keyword.baas_full_notm}}Db2 connector RPM-based installer, `ibm-db2-connector-<version>.x86_64.rpm`.

2. Run the following command to install the {{site.data.keyword.baas_full_notm}}Db2 connector:

   Using `yum`:

   ```bash
   sudo sh -c 'yum localinstall /tmp/ibm/ibm-db2-connector-<version>.x86_64.rpm'
   ```
   {: pre}

   Using `rpm -ivh`:

   ```bash
   sudo rpm -ivh ibm-db2-connector-<version>.x86_64.rpm
   ```
   {: pre}

## Uninstalling the {{site.data.keyword.baas_full_notm}}Db2 Connector
{: #uninstall-connector}

To uninstall the {{site.data.keyword.baas_full_notm}}Db2 connector, log on to the IBM Db2 node where you have installed the {{site.data.keyword.baas_full_notm}}Db2 connector and run the following command:

Using `yum`:

```bash
sudo yum remove ibm-db2-connector-<version>.x86_64.rpm
```
{: pre}

Using `rpm`:

```bash
sudo rpm -e ibm-db2-connector-<version>.x86_64.rpm
```
{: pre}

## Upgrading the {{site.data.keyword.baas_full_notm}}Db2 Connector
{: #upgrade-connector}

To upgrade the {{site.data.keyword.baas_full_notm}}Db2 connector, run the following command on IBM Db2 node:

```bash
sudo rpm -U ibm-db2-connector-<version>.x86_64.rpm
```
{: pre}

## Next steps
{: #plan-prepare-next-steps}

After you complete the planning and preparation steps, you can:

* [Register your IBM Db2 source](/docs/backup-recovery?topic=backup-recovery-register-ibm-db2-source)
* [Protect your IBM Db2 deployments](/docs/backup-recovery?topic=backup-recovery-protect-ibm-db2-deployments)
