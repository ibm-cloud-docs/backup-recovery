---

copyright:
  years: 2025
lastupdated: "2025-9-19"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Register oracle sources
{: #register_oracle_sources}

To start protecting your Oracle Databases, you need to register your Oracle servers and hosts as {{site.data.keyword.baas_full}} as a Service sources. Confirm you've met the Oracle requirements and then register your Oracle sources.

You'll need to use a Data Source Connection or [create one](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connectorr) to connect with sources in your data center to establish connectivity between the sources and the Cohesity DataProtect as a Service.

To register an Oracle Server as a {{site.data.keyword.baas_full_notm}} as a Service source:

1. Confirm that you meet the [Oracle requirements](oracle-requirements.htm#Oracle_Requirements) for software version and the required credentials and privileges.

2. In DataProtect as a Service, navigate to **Sources** and click **\+ Register Source**.

3. In the **Select Source** dialog box, select **Oracle** and click **Start Registration**.

4. In the **Register Oracle Server** dialog box, select an existing healthy SaaS connection marked Unused, or click **Create SaaS Connection**, follow the instructions in [Deploy SaaS Connection](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) and then click Continue.

5. ![](../Resources/Images/data-protect/Oracle/DP_Oracle_Registration1_686x272.png)

6. Enter the **Hostname (FQDN)** or **IP address** of the Oracle server you’re registering. We recommend that you use the FQDN.

7. Choose your [Oracle authentication method](oracle-requirements.htm#Oracle_Authentication_Method_Requirement): **OS Authentication** (the default) or **DB Authentication**.


    If you choose DB authentication, then all the databases on the system should have the same username and password.

8. Click **Complete**.


Your Oracle server appears under **Sources** in Cohesity DataProtect as a Service.

**Next** > You're ready to [protect your Oracle Databases](protect-oracle-databases.htm#Protect_Oracle_Databases)!
