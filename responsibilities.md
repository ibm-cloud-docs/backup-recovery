---

copyright:
  years: 2024
lastupdated: "2025-09-05"

subcollection: backup-recovery

keywords:

---

{{site.data.keyword.attribute-definition-list}}

# Understanding your responsibilities when using Backup and Recovery
{: #responsibilities-backup-recovery}

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.baas_full}}. For a high-level view of the service types in {{site.data.keyword.cloud}} and the breakdown of responsibilities between the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.baas_full_notm}}. For the overall terms of use, see [{{site.data.keyword.cloud}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms).


## Incident and operations management
{: #incident-and-ops}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Monitoring|{{site.data.keyword.baas_full}} is responsible for monitoring health of the service.|The Client is responsible for monitoring their backup and recovery related tasks with the {{site.data.keyword.cloud}} Monitoring, {{site.data.keyword.cloud}} Activity Tracker Event Routing, or {{site.data.keyword.cloud}} Logs.|
|High Availability|{{site.data.keyword.baas_full}} is responsible for deploying service across availability zones in a Multi-Zone Region (MZR), and storing backups in regionally durable storage.|The Client is responsible for designing application logic to retry connections caused by temporary connection failures (during Backup and Recovery services maintenance and updates).|
|Deployment|{{site.data.keyword.baas_full}} is responsible for deploying, scaling and managing  all backend infrastructure, dependent services etc. required to operate the service.|The client is responsible for deploying and managing client side components including agents and  Backup connectors.|
|Scaling|{{site.data.keyword.baas_full}} is responsible for scaling backed infrastructure to meet overall service workload demands|The Client is responsible for scaling Backup Connectors to meet backup workload requirements|
{: row-headers}
{: caption="Table 1. Responsibilities for incident and operations" caption-side="bottom"}

## Change management
{: #change-management}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Upgrade|{{site.data.keyword.baas_full}} is responsible for upgrade management of all backend infrastructure as well as upgrade management of Backup Connectors|The Client is responsible for managing upgrades of the backup agents|
|Configuration|{{site.data.keyword.baas_full}} is responsible for creating and managing tenants, endpoints, networking , storage, default backup policies corresponding to backup and recovery instance.|Client is responsible for creating backup policies to align with their business and IT needs.|
{: row-headers}
{: caption="Table 2. Responsibilities for change management" caption-side="bottom"}

## Identity and access management
{: #iam-responsibilities}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Access Policies|{{site.data.keyword.baas_full}} is responsible for creating and managing IAM access policies to all backend infrastructure.|The Client is responsible to manage access to the Backup and recovery instances using IBM Cloud IAM access control policies.|
{: row-headers}
{: caption="Table 3. Responsibilities for identity and access management" caption-side="bottom"}

## Security and regulation compliance
{: #security-compliance}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Encryption|{{site.data.keyword.baas_full}} is responsible for the encryption of all data at rest and in transit.|The Client is responsible for choosing and managing appropriate additional security features. If using Key Protect or HPCS, the client is responsible for managing Key Protect or HPCS authorizations and keys.|
{: row-headers}
{: caption="Table 4. Responsibilities for security and regulation compliance" caption-side="bottom"}

## Disaster recovery
{: #disaster-recovery}

| Task | {{site.data.keyword.IBM_notm}} Responsibilities | Your Responsibilities |
|----------|-----------------------|--------|
|Single Zone Failure|{{site.data.keyword.baas_full}} is responsible to ensure that the service is operational if one zone in an MZR is unavailable. <br>Client may experience brief unavailability for certain operations on some service instances. This would be in line with  the advertised service availability|The client is responsible for ensure that their workloads are protected by a backup policy.<br>The client is responsible for restoring their workloads in an alternate zone.|
{: row-headers}
{: caption="Table 5. Responsibilities for disaster recovery" caption-side="bottom"}
