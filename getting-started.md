---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: IBM cloud backup and recovery

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with Backup and Recovery
{: #getting-started-backup-recovery}
{{site.data.keyword.baas_full}} is a fully managed service(s) that provides backup solutions for various {{site.data.keyword.cloud}} services and customer workloads running on IBM Cloud.
{: shortdesc}

## Before you begin
{: #baas-getting-started}

You need the following to get started with {{site.data.keyword.baas_full_notm}}:
- An [{{site.data.keyword.cloud}} Platform account](https://cloud.ibm.com)
- An instance of {{site.data.keyword.baas_full_notm}} 

## Overview
{: #baas-overview}

The {{site.data.keyword.baas_full_notm}} landing page helps you get started with various back up solutions. You can choose to back up either application-consistent workloads or specific infrastructure services. When you select an option on the landing page, you’ll see the relevant backup service appear for those services.

For additional backup services like {{site.data.keyword.cloud}}  databases, see the Additional services drop-down in the main Backup and Recovery navigation menu.


## Comparison of workload availabilty
{: #baas-compare-workload-availability}

| Workload | VPC VSI | VMware | Regions supported |
| --- | --- | --- | --- |
| File systems | Yes  | Yes | - North America:  United States - Washington DC `us-east`, Dallas `us-south`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`<br> - Asia: Japan - Tokyo`jp-tok`|
| MS SQL | Yes  | Yes | - North America:  United States - Washington DC `us-east`, Dallas `us-south`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`<br> - Asia: Japan - Tokyo`jp-tok` |
| SAP HANA | Yes  | Yes |  - North America:  United States - Washington DC `us-east`, Dallas `us-south`<br> - Europe:  Europe - Frankfurt `eu-de`, Europe - London `eu-gb`<br> - Asia: Japan - Tokyo`jp-tok` |
{: caption="Workload availability " caption-side="bottom"}


## Creating a service instance for Backup and Recovery
{: #baas-provision-instance}

1. Log in to [the console](https://cloud.ibm.com/){: external}.
1. Navigate to the catalog, by clicking **Catalog** in the top navigation bar.
1. In the left menu, Click the **Storage** category. Click the **Backup and Recovery** tile.
1. Give the service instance a name.
1. Click **Create** and you are redirected to your new instance.

## Next steps
{: #baas-next-steps}

Now that you are familiar with your {{site.data.keyword.baas_full_notm}} instance from the web-based console, you might be interested in doing a similar workflow from the command line. Check out using the `ibmcloud backup-recovery` command-line utility to create a service instance and interacting with IAM. And you can further use `curl` for accessing COS directly. [Check out the API overview](/docs/allowlist/backup-recovery?topic=backup-recovery-compatibility-api) to get started.
