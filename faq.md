---

copyright:
  years: 2024, 2026
lastupdated: "2026-04-10"


keywords: faq, frequently asked questions, object storage, S3, HMAC, general

subcollection: backup-recovery

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# FAQ
{: #faq}

Frequently asked questions can produce helpful answers and insight into best practices for working with {{site.data.keyword.baas_full_notm}}.
{: shortdesc}

## What is Logical Data?
{: #baas-terminology-logical-data}
{: faq}

Logical data refers to the actual size of your data before any backup operations are performed. This is the physical size of the data as it exists on your source systems, including all files, databases, and application data. Understanding logical data size helps you estimate storage requirements and plan your backup strategy effectively.

## What is Data in (read)?
{: #baas-terminology-data-in}
{: faq}

Data in (read) represents the amount of data that is read from your source systems during the backup process, before any deduplication or compression is applied. This metric shows the total volume of data transferred from your data sources to the {{site.data.keyword.baas_full_notm}} service during backup operations. It provides insight into the actual data volume being protected.

## What is Data written?
{: #baas-terminology-data-written}
{: faq}

Data written is the amount of data actually stored in the backup repository after deduplication and compression have been applied. This metric is typically much smaller than "Data in (read)" due to the efficiency gains from deduplication (eliminating duplicate data blocks) and compression (reducing data size). The difference between data in and data written demonstrates the storage efficiency of your backup operations.

## What is an Agent name?
{: #baas-terminology-agent-name}
{: faq}

An agent name refers to the backup agent software installed on your data sources (such as physical servers, virtual machines, or database servers). The agent facilitates communication between your data source and the {{site.data.keyword.baas_full_notm}} service, enabling backup and recovery operations. Each agent is uniquely identified by its name for management and monitoring purposes.

## What is a Data Source Connector?
{: #baas-terminology-data-source-connector}
{: faq}

A Data Source Connector (formerly called SaaS connector) establishes the connection between your data sources and the {{site.data.keyword.baas_full_notm}} service. It acts as a bridge that enables secure communication and data transfer for backup and recovery operations. For detailed information on deploying and configuring Data Source Connectors, see [Deploy Data Source Connector](/docs/backup-recovery?topic=backup-recovery-deploy_data_source_connector) and [Factors affecting the Performance of IBM Cloud® Backup and Recovery service](/docs/backup-recovery?topic=backup-recovery-performance-factors).

## What is an SLA?
{: #baas-terminology-sla}
{: faq}

For information about Service Level Agreements (SLA) for {{site.data.keyword.baas_full_notm}}, see [SLAs](/docs/overview?topic=overview-slas).

## What factors affect the performance of {{site.data.keyword.baas_full}} service?
{: #baas-terminology-performance-factors}
{: faq}

Several factors can impact the performance of {{site.data.keyword.baas_full_notm}}, including network bandwidth, data change rate, deduplication ratio, compression efficiency, source system resources, and the number of concurrent backup jobs. For a comprehensive analysis of performance factors and optimization recommendations, see [Factors affecting Performance of {{site.data.keyword.baas_full_notm}}](/docs/backup-recovery?topic=backup-recovery-performance-factors).

## How often does {{site.data.keyword.baas_full}} release updates?
{: #baas-release-cadence}
{: faq}

The {{site.data.keyword.baas_full_notm}} service typically releases updates on a monthly cadence. These updates include new features, security patches, bug fixes, and performance improvements. It is recommended to upgrade your Data Source Connectors and Backup Agent Components when new releases become available to ensure you have the latest capabilities and security enhancements.

## What data sources are supported for backup?
{: #baas-supported-sources}
{: faq}

{{site.data.keyword.baas_full_notm}} supports a wide range of data sources including:
- Physical servers (Windows and Linux)
- Microsoft SQL Server databases
- Oracle databases
- Kubernetes and OpenShift clusters (IKS and ROKS)
- VMware virtual machines

For specific version requirements and configuration details, refer to the documentation for each data source type.

## Can I restore data to a different location or cluster?
{: #baas-restore-flexibility}
{: faq}

Yes, {{site.data.keyword.baas_full_notm}} supports flexible restore options. You can restore data to:
- The original source location
- An alternate location on the same system
- A different system or cluster within the same region
- For Kubernetes/OpenShift: different namespaces or clusters

Ensure that the target environment is compatible with the source, especially regarding storage classes, network configuration, and resource availability.

## How do I monitor backup job status and history?
{: #baas-monitoring}
{: faq}

You can monitor backup operations through the {{site.data.keyword.baas_full_notm}} dashboard, which provides:
- Real-time backup job status
- Historical backup run information
- Success and failure metrics
- Data transfer statistics
- Storage consumption reports
- Alert notifications for failed or incomplete backups

Additionally, you can integrate with IBM Cloud Activity Tracker and IBM Cloud Monitoring for comprehensive observability.

## What is the difference between full and incremental backups?
{: #baas-backup-types}
{: faq}

- **Full backup**: Captures all data from the source, regardless of whether it has changed since the last backup. Full backups provide complete data protection but require more time and storage.
- **Incremental backup**: Captures only the data that has changed since the last backup (full or incremental). Incremental backups are faster and more storage-efficient, making them ideal for frequent backup schedules.

{{site.data.keyword.baas_full_notm}} typically uses a combination of full and incremental backups to optimize both protection and efficiency.

## How is data secured during backup and at rest?
{: #baas-data-security}
{: faq}

{{site.data.keyword.baas_full_notm}} implements multiple layers of security:
- **Data in transit**: All data transferred between your sources and the service is encrypted using TLS/SSL protocols
- **Data at rest**: Backup data is encrypted in the storage repository using industry-standard encryption algorithms
- **Access control**: Integration with IBM Cloud IAM (Identity and Access Management) ensures that only authorized users can access backup data
- **Network isolation**: Support for private endpoints and Virtual Private Endpoints (VPE) for enhanced network security

For more details, see the [Data Security](/docs/backup-recovery?topic=backup-recovery-data-security) documentation.

## Who should I contact about billing and pricing?
{: #baas-billing}
{: faq}

For questions about {{site.data.keyword.baas_full_notm}} pricing, billing, or to discuss your specific requirements, contact IBM Offering Management or your IBM Sales representative. You can also view pricing information in the [IBM Cloud Catalog](https://cloud.ibm.com/catalog/services/backup-and-recovery).

## What should I do if a backup job fails?
{: #baas-backup-failure}
{: faq}

If a backup job fails:
1. Check the job details in the {{site.data.keyword.baas_full_notm}} dashboard for error messages
2. Verify that the Data Source Connector is running and connected
3. Ensure that the backup agent on the source system is operational
4. Check network connectivity between the source and the service
5. Verify that sufficient storage space is available
6. Review the logs for detailed error information
7. If the issue persists, contact IBM Support with the job details and error logs

For common issues and solutions, see the [Troubleshooting](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks-troubleshooting) documentation.

## Can I schedule backups to run at specific times?
{: #baas-backup-scheduling}
{: faq}

Yes, {{site.data.keyword.baas_full_notm}} provides flexible backup scheduling through Protection Policies. You can:
- Define backup schedules with specific days and times
- Set different schedules for full and incremental backups
- Configure retention policies to automatically manage backup lifecycle
- Create multiple policies for different data sources or protection requirements

For more information, see [Creating and configuring protection policies](/docs/backup-recovery?topic=backup-recovery-create-edit-standard-policy).

## How long are backups retained?
{: #baas-retention}
{: faq}

Backup retention is configurable through Protection Policies. You can define:
- Retention duration for different backup types (daily, weekly, monthly, yearly)
- Number of backup copies to retain
- Archival policies for long-term retention
- Automatic deletion of expired backups

Retention policies help you balance data protection requirements with storage costs. Configure retention settings based on your organization's compliance and business continuity requirements.
