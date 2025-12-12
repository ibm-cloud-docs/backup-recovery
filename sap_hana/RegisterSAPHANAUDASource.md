---

copyright:
  years: 2025
lastupdated: "2025-12-12"

keywords: register, manage, sopurce, sap hana, update source, refresh configuration, unregister

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Register and manage the SAP HANA source
{: #register_and_manage_the_sap_hana_source}

To protect your SAP HANA databases, you need to register your SAP HANA deployment as a source on {{site.data.keyword.cloud_notm}} Backup and Recovery. After registering your SAP HANA deployment as a source, you can:

- Add more SAP HANA node(s) to run the SAP HANA database agent. Ensure that you have installed the {{site.data.keyword.cloud_notm}} Backup and Recovery Linux Agent and SAP HANA database agent on the node(s).
- Refresh the source details.
- Unregister the SAP HANA source from {{site.data.keyword.cloud_notm}} Backup and Recovery.

## Register SAP HANA as a source
{: #register_sap_hana_as_a_source}

Before you register SAP HANA as a source, ensure that you:

- Review the requirements and limitations.
- Perform the prerequisite tasks.

For more information, see [Plan and Prepare for SAP HANA Protection](/docs/backup-recovery?topic=backup-recovery-plan_and_prepare_for_sap_hana_protection).

To register SAP HANA as a source:

1. Navigate to **Data Protection** > **Sources**.

2. Click **Register** on the top-right of the page and then select **Universal Data Adapter**.

3. In the **Register Universal Data Adapter** page, perform the following:

    1. From the **Source Type** drop-down menu, select **SAP HANA**.

    2. From the **Host OS Type** drop-down menu, select **Linux**.

    3. In the **Hostname/IP Addresses** field, enter the hostname or IP address (IPv4 or IPv6) of the node(s) you have identified to run the SAP HANA database agent. For more information, see [Plan and Prepare for SAP HANA Protection](/docs/backup-recovery?topic=backup-recovery-plan_and_prepare_for_sap_hana_protection).

        {{site.data.keyword.cloud_notm}} Backup and Recovery supports backup and restore of SAP HANA(x86) databases running on dual-stack (IPv4 and IPv6) mode or single-stack (only IPv6) mode.

    4. In the **Datasource Agent Installation Path** field, enter the directory path on the SAP HANA node, where you have installed the SAP HANA database agent. For more information, see [Plan and Prepare for SAP HANA Protection](/docs/backup-recovery?topic=backup-recovery-plan_and_prepare_for_sap_hana_protection).

    5. Ignore the In the **APP Authentication** section. It is not applicable for SAP HANA source registration.

    6. Enable **Custom Options**, perform the following:

    1. Ignore the **Mounts View** option. It is not applicable for SAP HANA source registration.

    2. Optional. In the **Source Registration Options** field, enter the following source registration arguments based on your requirement:

    The parameters are optional. If you do not explicitly specify the values then the values you had provided while installing the SAP HANA database agent is applied.

    | Parameters | Description |
    | --- | --- |
    | `--sourcename` | A unique name to identify the SAP HANA source. Using this argument, you can override the unique name that you provided while installing the SAP HANA database agent. For more information, see [Plan and Prepare for SAP HANA Protection](/docs/backup-recovery?topic=backup-recovery-plan_and_prepare_for_sap_hana_protection).<br><br>For example: `--sourcename=SAPSalesProd` |
    | `--hana-ha-vips` | Optional.<br><br>Provide the IP address configured for “**Virtual IP address resource**” on the SAP HANA HA cluster.<br><br>For example: `--hana-ha-vips=10.11.23.112` |
    | `--backint-protocol-version` | Optional.<br><br>The BACKINT protocol version 1.5 is supported by default from SAP HANA 2.0 SPS 05 or later versions on the IBM PowerPC Platform. By default, the SAP HANA Connector agent supports BACKINT protocol version 1.0. Using this argument, you can specify the SAP HANA Connector agent to use the BACKINT protocol version 1.5 based on your requirement.<br><br>For example: `--backint-protocol-version=1.5` |
    | `--et-log-backup` | Set the value to `true`, if you want to view the SAP HANA log backups on the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster.<br><br>Before registering the source, ensure that you authenticate to the {{site.data.keyword.cloud_notm}} Backup and Recovery cluster using the `et_log_backup_tool.jar` java binary available in the SAP HANA connector installation directory. For more information, see [Enable Displaying Log Backups in the {{site.data.keyword.cloud_notm}} Backup and Recovery Cluster](/docs/backup-recovery?topic=backup-recovery-install_and_manage_the_sap_hana_connector_on_x86-64_platform#enable_displaying_log_backups_in_the_cohesity_cluster).|
    {: caption="Parameters" caption-side="bottom"}

    3. Click **Register**.

## Update the SAP HANA source configuration
{: #update_the_sap_hana_source_configuration}

After registering your SAP HANA as a source on {{site.data.keyword.cloud_notm}} Backup and Recovery, you can:

- Add more node(s) to run the SAP HANA database agent. Ensure that you have installed the {{site.data.keyword.cloud_notm}} Backup and Recovery Linux Agent and SAP HANA database agent on the node(s).
- Modify the **Source Registration Options** based on your requirement.
- Update the **Datasource Agent Installation Path** only if you have uninstalled and reinstalled the SAP HANA database agent in a different directory on the SAP HANA node.

Ensure that the SAP HANA database agent is installed on all the node(s) in the SAP HANA cluster.

To update the SAP HANA source configuration:

1. Navigate to **Data Protection** > **Sources**.

2. Click the actions menu next to the SAP HANA source and select **Edit**.

3. In the **Update Universal Data Adapter** page, update the required configurations.

4. Click **Update**.

## Refresh the SAP HANA source configuration
{: #refresh_the_sap_hana_source_configuration}

You can refresh the SAP HANA Source and re-verify the source with the available source registration arguments.

To refresh the SAP HANA Source Configuration:

1. Navigate to **Data Protection** > **Sources**.

2. Click the actions menu next to the SAP HANA Source and select **Refresh**.

## Unregister the SAP HANA source
{: #unregister_the_sap_hana_source}

If you plan to stop taking the backup of your SAP HANA databases, you can unregister the SAP HANA source from the {{site.data.keyword.cloud_notm}} Backup and Recovery.

To unregister the SAP HANA source:

1. Navigate to **Data Protection** > **Sources**.

2. Click the actions menu next to the SAP HANA source and select **Unregister**.

