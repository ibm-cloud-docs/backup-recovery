---

copyright:
  years: 2024
lastupdated: "2026-08-19"

keywords: about, backup and recovery

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# What is Backup and Recovery?
{: #about-baas}

{{site.data.keyword.cloud_notm}} Backup and Recovery is a fully managed service that provides backup solutions for various IBM Cloud services and customer workloads running on IBM Cloud.
This service lets you define backup schedules to routinely protect data sources by using a secure, agent-based, application-consistent backup service. Backup infrastructure is managed by IBM.

Developers use APIs to interact with the service to set up backup and recovery activities. Additionally, there is a software development kit (SDK) are available for the Go framework. A plug-in is also available for the [{{site.data.keyword.cloud_notm}} Command Line Interface](/docs/cli?topic=cli-getting-started).

The [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external} provides a user interface for most operations and configuration as well.

## Supported Features and Capabilities
{: #baas-features}

### What you can protect
{: #workload-support}
- **Windows and Linux servers**: VMs and VPC VSIs
- **Databases**: Microsoft SQL Server, SAP HANA, IBM Db2, and Oracle (VMware only)
- **Containers**: Kubernetes (IKS) and OpenShift (ROKS) clusters
- **Virtual machines**: VMware environments

### Backup capabilities
{: #backup-capabilities}
- **Flexible scheduling**: Daily, weekly, or custom backup intervals with on-demand options
- **Smart backups**: Full, incremental, and differential backups to save storage space
- **Policy-based protection**: Reusable policies (Gold, Silver, Bronze) or create custom policies
- **Application-aware**: Database-consistent backups with automatic log backups for SQL Server, SAP HANA, and Db2

### Recovery options
{: #recovery-capabilities}
- **Restore anywhere**: Original location, alternate location, or different systems
- **Granular recovery**: Choose specific files, databases, or Kubernetes namespaces
- **Point-in-time recovery**: Restore to any previous backup snapshot or specific timestamp
- **Database cloning**: Create instant copies for testing and development

### Security and compliance
{: #security-features}
- **Encryption**: Automatic encryption at rest and in transit
- **Bring Your Own Key (BYOK)**: Integration with IBM Key Protect
- **Keep Your Own Key (KYOK)**: Integration with IBM Hyper Protect Crypto Services
- **DataLock**: Immutable backups that cannot be deleted or modified for compliance
- **Access control**: Role-based permissions through IBM Cloud IAM

### Storage efficiency
{: #efficiency-features}
- **Deduplication**: Eliminates duplicate data to reduce storage costs
- **Compression**: Automatic compression for smaller backup sizes
- **Incremental backups**: Only backs up changed data after the first full backup

### Monitoring and management
{: #monitoring-features}
- **Real-time monitoring**: Track backup job status and history
- **Alerts**: Notifications for backup failures or policy violations
- **Activity tracking**: Integration with IBM Cloud Activity Tracker for audit logs
- **Reports**: Customizable reports on backup success rates and storage usage

### Integration and automation
{: #integration-features}
- **APIs and SDKs**: Automate backup operations with REST APIs and Go SDK
- **CLI**: Command-line interface for scripting and automation
- **Terraform**: Infrastructure-as-Code support for automated deployments
- **S3-compatible**: Works with AWS CLI, rclone, and other S3 tools
## Next Steps
{: #about-baas-next-steps}

- Documentation on the best way to [get started](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery) with the service.

- Explore [Terraform IBM Modules (TIM)](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-about-tim) for production-ready, pre-built modules for infrastructure automation.
