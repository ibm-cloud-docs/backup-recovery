---

copyright:
  years: 2026

lastupdated: "2026-06-26"

keywords: data source connector, connector agent, migration, DSC, backup agent, linux

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Migrating from Data Source Connector to Connector Agent
{: #migrate-dsc-to-connector-agent}

Learn how to migrate your Linux source from using Data Source Connector (DSC) to Connector Agent for improved performance and functionality.
{: shortdesc}

## Before you begin
{: #migrate-dsc-prereqs}

Before you migrate to Connector Agent, ensure that you have the following prerequisites:

- Linux source registered with Data Source Connector version 7.2.18
   - Also supported for a Windows/SQL or Linux/Oracle source
- Backup Agent version 7.2.18 installed on the source
- Access to the {{site.data.keyword.baas_full}} console
- Administrator privileges on the Linux source system
- The Connector Agent version 7.3.12 installer package
- The Backup Agent version 7.3.12 installer package

## Upgrade backup agent on the Source
{: #migrate-upgrade-backup-agent}
{: step}


1. Remove Backup Agent version 7.2.18 from source:

   ```bash
   sudo ./cohesity_agent_7.2.18_linux_x64_installer -- --update-uninstall
   ```
   {: pre}

1. Install the Backup Agent version 7.3.12 on the source:

   ```bash
   sudo ./cohesity_agent_0.0.0-7.3.12_linux_x64_installer -- --install -y
   ```
   {: pre}


## Register the Connector Agent
{: #migrate-register-connector-agent}
{: step}

First, register the Connector Agent with your {{site.data.keyword.baas_full}} service.

1. Install and register the Connector Agent version 7.3.12 on your Linux source.

2. After registration, note the Connector Agent's `connectionId`. You need this value to replace the Data Source Connector connection in the next step.

## Replace Data Source Connector with Connector Agent
{: #migrate-replace-dsc}
{: step}

You can replace the Data Source Connector with the Connector Agent by using either the migration script or by manually running the API calls.

### Option 1: Use the migration script
{: #migrate-use-script}

Run the migration script to automate the replacement process:

```bash
python3 migrate_to_connector_agent.py --config <cluster.config> --hostname <connector_agent_name> --host-ip <datasource_ip>
```
{: pre}

The script performs the following actions:
- Locates the host by IP address
- Updates the source registration to use the Connector Agent

Expected output:

```text
Found host 10.241.0.73
Put worked
```
{: screen}

Please contact IBM Support for the migration script.
{: note}

### Option 2: Manually update the source registration
{: #migrate-manual-update}

If you prefer to manually update the source registration, follow these steps:

1. List connector agents for the tenant ID to find the `connectionId` of the registered Connector Agent:

   ```text
   GET https://<cluster_ip>/v2/connector-agents?tenantId=<tenant_id>
   ```
   {: codeblock}

2. List source registrations to find the `connectionId` of the registered source:

   ```text
   GET https://<cluster_ip>/v2/data-protect/sources/registrations
   ```
   {: codeblock}

3. Update the source registration to replace the Data Source Connector's `connectionId` with the Connector Agent's `connectionId`:

   ```text
   PUT https://<cluster_ip>/v2/data-protect/sources/registrations/<host_id>
   ```
   {: codeblock}

## Verify the migration
{: #migrate-verify}
{: step}

After completing the migration, verify that the Data Source Connector is no longer in use.

1. Log in to the {{site.data.keyword.baas_full}} console.

2. Navigate to **System** > **Data Source Connections**.

3. Verify that the UI shows **0 Sources** for the Data Source Connection that was previously used.

This confirms that the source is now using the Connector Agent instead of the Data Source Connector.

## Test protection and recovery
{: #migrate-test}
{: step}

After completing the migration, test the protection and recovery operations to ensure everything is working correctly.

1. Run a protection job on the source.

2. Verify that the backup completes successfully.

3. Perform a test recovery operation.

4. Verify that the recovery completes successfully.

Both protection and recovery operations should complete without errors, confirming that the migration to Connector Agent is successful.

## Next steps
{: #migrate-next-steps}

After successfully migrating to Connector Agent, you can:

- Monitor the performance of your backup and recovery operations
- Configure additional protection policies as needed
- Review the [Connector Agent documentation](/docs/backup-recovery?topic=backup-recovery-connector-agent-setup-tutorial) for advanced configuration options
