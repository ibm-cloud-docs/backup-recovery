---

copyright:
  years: 2025
lastupdated: "2025-11-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Register Oracle sources
{: #register_oracle_sources}

To start protecting your Oracle Databases, you need to register your Oracle servers and hosts as {{site.data.keyword.baas_full}} as a service source. Confirm you've met the Oracle requirements and then register your Oracle sources.

You'll need to use a Data Source Connection or [create one](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connectorr) to connect with sources in your data center to establish connectivity between the sources and the {{site.data.keyword.baas_full_notm}} as a service.

To register an Oracle server as {{site.data.keyword.baas_full_notm}} as a service source:

1. Confirm that you meet the [Oracle requirements](/docs/backup-recovery?topic=backup-recovery-oracle_requirements) for software version and the required credentials and privileges.
2. In {{site.data.keyword.baas_full_notm}} as a service, navigate to **Sources** and click **\+ Register Source**.
3. In the **Select Source** dialog box, select **Oracle** and click **Start Registration**.
4. In the **Register Oracle Server** dialog box, select an existing healthy Data Source Connection marked Unused, or click **Create Data Source Connection**, follow the instructions in [Deploy Data Source Connection](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) and then click Continue.
5. Enter the **Hostname (FQDN)** or **IP address** of the Oracle server you’re registering. IBM recommends that you use the FQDN.
6. Choose your [Oracle authentication method](/docs/backup-recovery?topic=backup-recovery-oracle_requirements#oracle_authentication_method_requirement): **OS Authentication** (the default) or **DB Authentication**.
   If you choose DB authentication, then all the databases on the system should have the same username and password.
7. Click **Complete**.
   Your Oracle server appears under **Sources** in {{site.data.keyword.baas_full_notm}} as a service.

**Next** > You're ready to [protect your Oracle Databases](/docs/backup-recovery?topic=backup-recovery-protect-oracle-database)!
