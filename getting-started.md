---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: IBM cloud backup and recovery

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Backup and Recovery
{: #getting-started-backup-recovery}
{{site.data.keyword.baas_full}} is a fully managed service that provides backup solutions for various {{site.data.keyword.cloud}} services and customer workloads running on IBM Cloud.  In this getting started tutorial, you will learn how to back up and recover a workload using the {{site.data.keyword.baas_full_notm}} service.




{: shortdesc}

## Before you begin
{: #baas-getting-started}

You need the following to get started with {{site.data.keyword.baas_full_notm}}:
- An [{{site.data.keyword.cloud}} Platform account](https://cloud.ibm.com)
- An active instance of {{site.data.keyword.baas_full_notm}} 
- You need to launch the dashboard of your backup service instance to create, manage, and monitor backup policies

## Comparison of workload availabilty
{: #baas-compare-workload-availability}

| Workload | VPC VSI | VMware | Regions supported |
| --- | --- | --- | --- |
| File systems | Yes  | Yes | - North America:  United States - Washington DC `us-east`, Dallas `us-south`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok`|
| MS SQL | Yes  | Yes | - North America:  United States - Washington DC `us-east`, Dallas `us-south`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok` |
| SAP HANA | Yes  | Yes |  - North America:  United States - Washington DC `us-east`, Dallas `us-south`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`, Europe - Madrid `eu-es`<br> - Asia: Japan - Tokyo`jp-tok` |
{: caption="Workload availability " caption-side="bottom"}

## Selecting an instance and opening the main menu
{: #baas-getting-started-dashboard}{: step}

1. Start by searching for {{site.data.keyword.baas_full_notm}} in the [IBM Cloud catalog](https://cloud.ibm.com/catalog#highlights), and select your {{site.data.keyword.baas_full_notm}} storage instance from the resource list found under the navigation menu.
2. Click on your instance from the list to open the main page.
3. Showing on the main page are the details of the information you provided when creating a {{site.data.keyword.baas_full_notm}} instance.  From this page you can take additional actions like rename, launch dashboard for [setting up your environment](#baas-setting-up-environment) and [configuring protection](#baas-configure-protection), [view logging](/docs/cloud-logs?topic=cloud-logs-getting-started), [view monitoring](/docs/monitoring?topic=monitoring-getting-started#getting-started) and delete this instance.  Additional information is shown for the [API details](/docs/backup-recovery?topic=backup-recovery-compatibility-api) like your ID and endpoints.
4. Click on view in the Logging window to load your Logs Routing targets, or click [Manage routing](cloud.ibm.com/observability/logs-routing/targets) to define rules to collect and send platform logs to a Cloud Logs instance.
5. Click on view in the Monitoring window to load your Metrics Routing targets, or click [Manage routing](cloud.ibm.com/observability/metrics-routing/routes) to define your IBM Cloud Metrics Routing settings.


## Setting up your environment for Backup and Recovery
{: #baas-setting-up-environment}{: step}

To get started, you'll need to complete the following steps in the dashboard.

1. Create [backup service connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) for virtual machines to move data between your sources and the Backup service.
2. Download and install the [backup agents](/docs/backup-recovery?topic=backup-recovery-agent-download-install) onto the servers you want to back up.
3. [Register sources](/docs/backup-recovery?topic=backup-recovery-source-registration-tutorial) to define the types of applications on those servers.


## Configuring protection
{: #baas-configure-protection}{: step}

To get started, you'll need to complete the following steps in the dashboard.

1. Create [backup policies](/docs/backup-recovery?group=policies-and-protection-groups) as needed.
2. Choose the [registered sources](/docs/backup-recovery?topic=backup-recovery-source-registration-tutorial) to protect.

## Next steps
{: #baas-next-steps}

Now that you are familiar with your {{site.data.keyword.baas_full_notm}} instance from the web-based console, you might be interested in doing a similar workflow from the command line. Check out using the `ibmcloud backup-recovery` command-line utility to create a service instance and interacting with IAM. And you can further use `curl` for accessing COS directly. [Check out the API overview](/docs/backup-recovery?topic=backup-recovery-compatibility-api) to get started.
