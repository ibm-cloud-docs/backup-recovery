---

copyright:
  years: 2025
lastupdated: "2025-07-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Install and manage the SAP HANA connector on x86-64 platform
{: #install_and_manage_the_sap_hana_connector_on_x86-64_platform}

This topic covers instructions on how to install the SAP HANA Connector on x86-64 Platform.

## Prerequisites
{: #prerequisites}

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
- [Download and install the {{site.data.keyword.cloud_notm}} Backup and Recovery Linux Agent on the SAP HANA node](/docs/allowlist/backup-recovery?topic=backup-recovery-plan_and_prepare_for_sap_hana_protection#download_and_install_the_cohesity_linux_agent).
- [Download the SAP HANA connector](/docs/allowlist/backup-recovery?topic=backup-recovery-plan_and_prepare_for_sap_hana_protection#download_the_sap_hana_connector_installer) for x86-64 platform.


## Install the SAP HANA connector
{: #install_the_sap_hana_connector}

On the SAP HANA node where you have copied the SAP HANA connector installer, perform the following to install the SAP HANA connector.

For multi-node SAP HANA deployments with shared storage, ensure that:

- You install the SAP HANA connector on the machine where you have installed {{site.data.keyword.cloud_notm}} Backup and Recovery Linux Agent.
- The SAP HANA Connector is installed on the shared NFS path, to which all the SAP HANA nodes have access.

For multi-node SAP HANA deployments with non-shared storage, ensure that the {{site.data.keyword.cloud_notm}} Backup and Recovery Linux Agent and the SAP HANA connector are installed on all the nodes in the SAP HANA HA cluster in the same installation directory.



### Enable displaying log backups in the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster
{: #enable_displaying_log_backups_in_the_cohesity_cluster}

SAP HANA automatically triggers log backups, you can view these log backups on the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster. For viewing the SAP HANA log backups on the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster, you need to authenticate to the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster using the following java binary available in the SAP HANA connector installation directory: `<backint_install_directory>/cohesity_backint_plugin/et_log_backup_tool.jar`

Ensure that you have JDK installed on the SAP HANA node.

After you have successfully installed the SAP HANA connector, run the following command:

```sh
java -jar <backint\_install\_directory>/cohesity\_backint\_plugin/et\_log\_backup\_tool.jar --authenticate
```

**Example**:

```sh
java -jar /opt/cohesity/cohesity\_backint\_plugin/bin/et\_log\_backup\_tool.jar --authenticate
```

- When prompted, authenticate using the **{{site.data.keyword.cloud_notm}} Backup and Recovery cluster's local admin user credentials**. If the password has been changed, make sure to reauthenticate using the updated password.
- After you authenticate to the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster using the `et_log_backup_tool.jar` java binary, ensure that you set the `–et-log-backup` source registration parameter to `true` while registering the SAP HANA source. For more information, see [Register and Manage the SAP HANA Source](/docs/allowlist/backup-recovery?topic=backup-recovery-register_and_manage_the_sap_hana_source).
- After registering the source with {{site.data.keyword.cloud_notm}} Backup and Recovery (with`–et-log-backup=true`), a full backup is mandatory. The initial full backup must be completed before any log backups appear on the {{site.data.keyword.cloud_notm}} Backup and Recovery user interface.
- If you upgrade to version 7.1.2, you need to modify the existing registered source and set the `et-log-backup` source registration parameter to `true` in order to enable the display of log backups in the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster. Only the log backups triggered after enabling `et-log-backup` will be shown on the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster.


## Install the SAP HANA connector using rpm based installer
{: #install_the_sap_hana_connector_using_rpm_based_installer}

Before you install the SAP HANA Connector Using RPM-based installer, ensure that you configure the following environment variables on the SAP HANA machine:


| Environment Variable | Description | Mandatory? | Command |
| --- | --- | --- | --- |
| `SERVICE_USER` | The `<SID>adm` user account that must own the backup files on the {{site.data.keyword.cloud_notm}} Backup and Recovery view. If you have not created the user account, then create an Operating System (OS) user account in the following format: `<SID>adm`. Where `<SID>` is a unique three-character code called SAP System Identification (SID). It is available for every R/3 installation (SAP system) that consists of a database server and several application servers. | Yes | `export SERVICE_USER=<VALUE>` |
| `SYSTEM_DB_KEY` | The SAP HANA `HDBUSERSTORE` key to connect to the SYSTEM DB. | Yes | `export SYSTEM_DB_KEY=<HDBUSERSTORE key>` |
{: caption="Environment Variable " caption-side="bottom"}

To install the SAP HANA connector using rpm-based installer:

1. Log on to the SAP HANA node’s bash CLI using `<SID>adm` user.
2. Make the SAP HANA connector installer `(cohesity-hana-backint-<version>.x86_64.rpm)` executable using the following command:

```sh
    \> chmod +x cohesity-hana-backint-<version>.x86\_64.rpm
```

    During the SAP HANA connector installation, by default, the script installer extracts the files to the `/tmp` directory. If you plan to extract the files to a different directory, you can define the directory path in the `TMPDIR` environment variable as follows: `export TMPDIR=<directory_path>`

3. Install the SAP HANA connector using the following command:

```sh
    \> sudo -E rpm -ivh cohesity-hana-backint-<version>.x86\_64.rpm
```

## Uninstall the SAP HANA connector
{: #uninstall_the_sap_hana_connector}

To uninstall the SAP HANA connector, run the following command with appropriate parameters on SAP HANA node’s bash CLI.

Using the Script installer:

```sh
./cohesity\_secure\_connector\_service\_sap\_hana\_installer -- --uninstall --install-dir=<backint\_install\_directory> --sid=<sid> --workflow=<workflow>
```

Specify the `--standby` option, if you are uninstalling the SAP HANA connector on a standby node.

Using the RPM installer:

```sh
\> sudo -E rpm -evh cohesity-hana-backint-<version>.x86\_64.rpm
```

After you have successfully uninstalled the SAP HANA connector, log on to the SAP HANA node as root user and unlink the soft link from `/opt/cohesity/agent/uda_scripts`.

## Upgrade the SAP HANA connector
{: #upgrade_the_sap_hana_connector}

To upgrade the SAP HANA connector, run the following command on all SAP HANA nodes.

Using the Script installer:

```sh
./cohesity\_secure\_connector\_service\_sap\_hana\_installer -- --upgrade --install-dir=<backint\_install\_directory> --sid=<sid> --systemdb-key=<user\_store\_key> --workflow=<workflow>
```

Using the RPM installer:

```sh
\> sudo -E rpm -Uvh cohesity-hana-backint-<version>.x86\_64.rpm
```

After you have upgraded the SAP HANA connector, log on to the SAP HANA node as root user and create a soft link of `<backint_install_directory>/cohesity_backint_plugin/uda_scripts` folder to `/opt/cohesity/agent/uda_scripts` using the following command:

```sh
ln -s /usr/sap/TEC/home/cohesity/cohesity\_backint\_plugin/uda\_scripts /opt/cohesity/agent/uda\_scripts
```
