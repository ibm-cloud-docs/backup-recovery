---

copyright:
  years: 2024, 2026
lastupdated: "2026-08-19"

keywords: IBM cloud backup and recovery, instance creation, provisioning, service instance

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Creating a Backup and Recovery service instance
{: #instance-creation}

Before you can protect any workload with {{site.data.keyword.baas_full}}, you need to create a {{site.data.keyword.baas_full_notm}} service instance. This instance provides the management dashboard, backup policies, and storage for your backup operations.
{: shortdesc}

After creating your instance, you'll need to set up the appropriate connector for your workload type. See [Setting up a Backup and Recovery instance](/docs/backup-recovery?topic=backup-recovery-instance-setup) for VPC workloads or [Register Kubernetes or OpenShift as a data source](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks) for Kubernetes/OpenShift clusters.
{: tip}

## Before you begin
{: #instance-creation-prereqs}

You need the following to create a {{site.data.keyword.baas_full_notm}} instance:
- An [{{site.data.keyword.cloud}} Platform account](https://cloud.ibm.com)
- Appropriate IAM permissions to create service instances

## Creating the instance
{: #instance-creation-steps}

1. Go to the [{{site.data.keyword.baas_full_notm}} catalog page](https://cloud.ibm.com/catalog/services/backup-and-recovery).

2. Select a location for your instance.
   
   When following best practices, it is recommended that you create your {{site.data.keyword.baas_full_notm}} instance in the same location as your source server or workload.
   {: tip}

3. Enter a service name for your instance.

4. Select a resource group.

5. (Optional) Add tags to organize your resources.

6. Review the encryption settings.
   
   By default, your instance is automatically encrypted. Optionally, you can bring your own key by supplying the key CRN from IBM Key Protect or Hyper Protect Crypto Services.
   {: note}

7. Click **Create**.

## Accessing your instance
{: #instance-creation-access}

After your instance is created:

1. Navigate to your [{{site.data.keyword.cloud_notm}} Resource list](https://cloud.ibm.com/resources).
2. Expand the **Services and software** section.
3. Locate your {{site.data.keyword.baas_full_notm}} instance and click on it.
4. Click **Launch Dashboard** to open the {{site.data.keyword.baas_full_notm}} management console.

## Next steps
{: #instance-creation-next-steps}

Now that you have created your {{site.data.keyword.baas_full_notm}} instance, proceed to set up the appropriate connector for your workload type:

- **For VPC VSIs, databases (MS SQL, SAP HANA, Oracle, Db2), or physical servers**: See [Setting up a Backup and Recovery instance](/docs/backup-recovery?topic=backup-recovery-instance-setup) to deploy a Data Source Connector VSI.

- **For Kubernetes or OpenShift clusters**: See [Register Kubernetes or OpenShift as a data source](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks) to install the Helm chart connector.

## Related information
{: #instance-creation-related}

- [IAM access management](/docs/backup-recovery?topic=backup-recovery-iam-overview)
- [Service endpoints](/docs/backup-recovery?topic=backup-recovery-service-endpoints)
- [Encryption overview](/docs/backup-recovery?topic=backup-recovery-encryption-overview)
