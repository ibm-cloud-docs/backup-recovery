---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-09"

keywords: IBM cloud backup and recovery

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Backup and Recovery
{: #getting-started-backup-recovery}
{{site.data.keyword.baas_full}} is a managed service that provides backup solutions for various {{site.data.keyword.cloud}} services and customer workloads running on IBM Cloud.  In this getting started tutorial, you learn how to back up and recover a workload by using the {{site.data.keyword.baas_full_notm}} service.




{: shortdesc}

Use code **VPC1000** and get **USD 1,000** in no-charge credits to use toward different offerings on IBM Cloud VPC, including: Confidential computing with Intel® SGX®, Bare Metal for VPC, {{site.data.keyword.baas_full_notm}}, or any Block and File storage or networking components. **Valid until 31 December 2025**.
{: important}


## Before you begin
{: #baas-getting-started}

You need the following to get started with {{site.data.keyword.baas_full_notm}}:
- An [{{site.data.keyword.cloud}} Platform account](https://cloud.ibm.com)
- An active instance of {{site.data.keyword.baas_full_notm}} 
- You need to start the dashboard of your backup service instance to create, manage, and monitor backup policies

## Comparison of workload availability
{: #baas-compare-workload-availability}

| Workload | VPC VSI | VMware | Regions supported |
| --- | --- | --- | --- |
| File system | Yes  | Yes | - North America:  United States - Washington DC `us-east`, Dallas `us-south`, Canada - Toronto `ca-tor`<br> - South America:  Brazil - São Paulo `br-sao`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`, Osako`jp-osa`<br> - Australia - Sydney`au-syd`|
| MS SQL | Yes  | Yes | - North America:  United States - Washington DC `us-east`, Dallas `us-south`, Canada - Toronto `ca-tor`<br> - South America:  Brazil - São Paulo `br-sao`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`, Osako`jp-osa`<br> - Australia - Sydney`au-syd` |
| SAP HANA | Yes  | Yes |  - North America:  United States - Washington DC `us-east`, Dallas `us-south`, Canada - Toronto `ca-tor`<br> - South America:  Brazil - São Paulo `br-sao`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`, Osako`jp-osa`<br> - Australia - Sydney`au-syd` |
| Oracle | No  | Yes |  - North America:  United States - Washington DC `us-east`, Dallas `us-south`, Canada - Toronto `ca-tor`<br> - South America:  Brazil - São Paulo `br-sao`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`, Osako`jp-osa`<br> - Australia - Sydney`au-syd` |
{: caption="Workload availability " caption-side="bottom"}

## Selecting an instance and opening the main menu
{: #baas-getting-started-dashboard}{: step}

1. Start by searching for {{site.data.keyword.baas_full_notm}} in the [IBM Cloud catalog](https://cloud.ibm.com/catalog#highlights), and select your {{site.data.keyword.baas_full_notm}} storage instance from the resource list that is found under the navigation menu.
2. Click your instance from the list to open the main page.
3. Showing on the main page are the details of the information that you provided when creating a {{site.data.keyword.baas_full_notm}} instance.  From this page you can take more actions like rename, start dashboard for [setting up your environment](#baas-setting-up-environment) and [configuring protection](#baas-configure-protection), [view logging](/docs/cloud-logs?topic=cloud-logs-getting-started), [view monitoring](/docs/monitoring?topic=monitoring-getting-started#getting-started) and delete this instance.  Additional information is shown for the [API details](https://cloud.ibm.com/apidocs/backup-recovery#br-intro) like your ID and endpoints.
4. Click view in the Logging window to load your Logs Routing targets, or click [Manage routing](cloud.ibm.com/observability/logs-routing/targets) to define rules to collect and send platform logs to a Cloud Logs instance.
5. Click view in the Monitoring window to load your Metrics Routing targets, or click [Manage routing](cloud.ibm.com/observability/metrics-routing/routes) to define your IBM Cloud Metrics Routing settings.


## Setting up your environment for Backup and Recovery
{: #baas-setting-up-environment}{: step}

To get started, you need to complete the following steps in the dashboard.

1. Create [backup service connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) for virtual machines to move data between your sources and the Backup service.
2. Download and install the [backup agents](/docs/backup-recovery?topic=backup-recovery-agent-download-install) onto the servers you want to back up.
   - **NOTE:** Create a VM to be used as a [backup service connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) for virtual machines to move data between your sources and the {{site.data.keyword.baas_full_notm}} service.
3. [Register sources](/docs/backup-recovery?topic=backup-recovery-source-registration-tutorial) to define the types of applications on those servers.

IBM supports setting the KMS Root CRN as part of your instance creation, and setting up a service-to-service authentication between {{site.data.keyword.baas_full_notm}} and Key Protect or Hyper Protect Crypto Services.
{: note}



## Configuring protection
{: #baas-configure-protection}{: step}

To get started, you need to complete the following steps in the dashboard.

1. Create [backup policies](/docs/backup-recovery?group=policies-and-protection-groups) as needed.
2. Choose the [registered sources](/docs/backup-recovery?topic=backup-recovery-source-registration-tutorial) to protect.

## Next steps
{: #baas-next-steps}

Now that you are familiar with your {{site.data.keyword.baas_full_notm}} instance from the web-based console, you might be interested in doing a similar workflow from the command line. Check out using the `ibmcloud backup-recovery` command-line utility to create a service instance and interacting with IAM. And you can further use `curl` for accessing Cloud Object Storage directly. [Check out the API overview](https://cloud.ibm.com/apidocs/backup-recovery#br-intro) to get started.
