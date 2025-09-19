---

copyright:
  years: 2025
lastupdated: "2025-09-19"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Deploy saas connector
{: #deploy_saas_connector}

To register on-premises or cloud-based data sources with Cohesity DataProtect as a Service, you need to use a SaaS Connection to establish connectivity between your source and the service. A SaaS Connection consists of one or more SaaS Connectors, which are VMs that act as data movers between your data sources and the Cohesity DataProtect as a Service.

SaaS Connectors are closed virtual appliances managed by Cohesity. Except as specifically instructed by Cohesity, Cohesity does not support customers installing third-party software or OS updates independent of the Cohesity release process, and any attempt to install third-party software or deploy updates sourced from sources other than Cohesity may void your existing contractual terms with Cohesity, including any applicable warranties.

To create a SaaS Connection, you must deploy one or more SaaS Connector VMs. Depending on how SaaS connectors are deployed, SaaS Connectors can be classified as:

*   **User-deployed SaaS Connectors**: The user must deploy the SaaS Connectors manually on the source you want to protect.
    
*   **Cohesity-deployed SaaS Connectors**: Cohesity will auto-deploy the SaaS Connectors on the source you want to protect.
    

The following table provides information about the supported SaaS Connectors.

|     |     |
| --- | --- | 
| **[User-Deployed SaaS Connectors](user-deployed-saas-connection.htm)** | *   [VMware SaaS Connectors](deploy-vmware-saas-connector.htm)<br>    <br>*   [Hyper-V SaaS Connectors](deploy-hyperv-saas-connector.htm) |
| **[Cohesity-Deployed SaaS Connectors](cohesity-deployed-saas-connection.htm)** | *   [AWS SaaS Connectors](create-aws-saas-connector.htm)<br>    <br>*   [Azure SaaS Connectors](azure-vm/create-azure-saas-connection.htm) |
{: caption="Table 1. " caption-side="bottom"}

