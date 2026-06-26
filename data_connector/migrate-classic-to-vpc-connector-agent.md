---
copyright:
  years: 2026
lastupdated: "2026-06-26"

keywords: connector agent, migration, classic, vpc, data source connector, dsc, backup agent

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Migrating from Classic to VPC using Connector Agent
{: #migrate-classic-vpc-connector-agent}

Learn how to migrate your Classic VSI source from Data Source Connector (DSC) to Connector Agent when moving to VPC infrastructure.
{: shortdesc}

This migration process allows you to transition from DSC version 7.2.18 to Connector Agent while maintaining your backup and recovery capabilities during infrastructure migration.

## Before you begin
{: #migrate-classic-vpc-prereqs}

Before you start the migration, ensure that you have the following prerequisites:

* Classic VSI source registered with DSC version 7.2.18
* Backup Agent version 7.2.18 installed on the Classic VSI
* Access to the {{site.data.keyword.baas_full_notm}} cluster
* Bearer Token for authentication with the cluster
* Python 3 installed on your system for running the migration script

## Register the Connector Agent on Classic VSI
{: #register-connector-agent-classic}
{: step}

First, you need to register the Connector Agent on your Classic VSI.

1. Run the Connector Agent on your Classic VSI.

2. Obtain a Bearer Token for authentication with the {{site.data.keyword.baas_full_notm}} cluster.

3. Fetch the Registration Token from your {{site.data.keyword.baas_full_notm}} cluster.

4. Claim the Connector Agent from the data source.

## Replace DSC with Connector Agent
{: #replace-dsc-connector-agent-classic}
{: step}

After registering the Connector Agent, replace the Data Source Connector with the Connector Agent.

### Run the migration script
{: #run-migration-script-classic}

You can use the automated migration script or manually perform the steps.

**Option 1: Use the automated script**

1. Please contact IBM Support for the migration script.

2. Run the script with your cluster configuration:

   ```bash
   python3 migrate_to_connector_agent.py --config <cluster.config> --hostname <connector_agent_name> --host-ip <datasource_ip>
   ```
   {: pre}

   The script output shows the following responses:

   ```text
   <Response [200]>
   <Response [200]>
   5246329030302288073
   <Response [200]>
   Found host 52.117.73.18
   <Response [200]>
   Put worked
   ```
   {: screen}

**Option 2: Update the data source registration manually**

If you prefer to perform the migration manually, complete the following steps:

1. Call the `GET /v2/data-protect/sources/registrations` API to find the `connectionId` and `id` of the data source.

2. Call the `PUT /v2/data-protect/sources/registrations/<id>` API with the `id` from the previous step to update the source registration.

   Replace the Data Source Connector's `connectionId` with the Connector Agent's `connectionId`.

## Verify the migration
{: #verify-migration-classic}
{: step}

After completing the migration, verify that the Data Source Connector is no longer in use.

1. Log in to the {{site.data.keyword.baas_full_notm}} console.

2. Navigate to **System** > **Data Source Connections**.

3. Verify that the UI shows **0 Sources** for the Data Source Connection that was previously used.

This confirms that the source is now using the Connector Agent instead of the Data Source Connector.

## Remove the old Backup Agent
{: #remove-old-agent-classic}
{: step}

After verifying the migration, remove the old Backup Agent version 7.2.18 from the Classic VSI source.

Run the appropriate uninstall command for your Backup Agent version to remove it from the system.

For more information, see [Migrating from Data Source Connector to Connector Agent](/docs/backup-recovery?topic=backup-recovery-migrate-dsc-to-connector-agent).

## Install the new Backup Agent
{: #install-new-agent-classic}
{: step}

Install the new Backup Agent version 7.3.12 on the Classic VSI source.

Run the installer for the new Backup Agent version to complete the installation.

For more information, see [Migrating from Data Source Connector to Connector Agent](/docs/backup-recovery?topic=backup-recovery-migrate-dsc-to-connector-agent).

## Test protection and recovery
{: #test-protection-recovery-classic}
{: step}

After completing the migration, test the protection and recovery operations to ensure everything is working correctly.

1. Run a protection job on the source.

2. Verify that the backup completes successfully.

3. Perform a test recovery operation.

4. Verify that the recovery completes successfully.

Both protection and recovery operations should complete without errors, confirming that the migration to Connector Agent is successful.

## Next steps
{: #migrate-classic-vpc-next-steps}

After successfully migrating to Connector Agent, you can:

* Monitor the performance of your backup and recovery operations
* Configure additional protection policies as needed
* Review the [Connector Agent documentation](/docs/backup-recovery?topic=backup-recovery-connector-agent-setup-tutorial) for advanced configuration options
* Plan your complete migration from Classic to VPC infrastructure
