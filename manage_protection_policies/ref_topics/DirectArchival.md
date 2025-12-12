---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Cloudarchive direct
{: #cloudarchive_direct}


CloudArchive Direct lets you archive data directly to an external target without retaining a copy of the data on the {{site.data.keyword.baas_full_notm}}. In CloudArchive Direct, {{site.data.keyword.baas_full_notm}} acts as a local cache to stream the dataset to the external target, eliminating the need to store a full backup copy on the {{site.data.keyword.baas_full_notm}} first.

This approach reduces the capacity requirements on the local {{site.data.keyword.baas_full_notm}} to a small cluster footprint, as only the metadata and index, and not the dataset itself are stored on the local cluster. Storing the metadata and index on the local cluster enables much quicker search and recovery down the road. The external target also stores the metadata and index, in addition to the data itself. On the {{site.data.keyword.baas_full_notm}}, you can register a cloud storage, S3-compatible storage, or NFS-mounted servers as external targets.

For the first archival, depending on the size of data, the {{site.data.keyword.baas_full_notm}} might experience significant capacity utilization, because the data first gets stored on the local storage (Storage Domain) and then aggressively gets down tired to the external target. During this time, the capacity utilization will increase till the data is garbage collected upon the successful transfer.

The current version of IBM Cloud Supports:

*   CloudArchive Direct for VMware VMs data source in addition to the existing support for NAS data sources.

*   Enabling CloudArchive Direct from the Protection Policy settings in the {{site.data.keyword.baas_full_notm}} Console.


The option to enable CloudArchive Direct (native mode - no dedupe, no compression) from the NAS Protection Group page is deprecated in the 6.7 version. If you want to enable the CloudArchive Direct native mode, then you must contact [IBM Cloud Support](../Support/ContactSupport.htm).

## How cloudarchive direct works
{: #how_cloudarchive_direct_works}

When you run a Protection Group for a registered NAS source with CloudArchive Direct enabled, then according to the policy set for the Protection Group, the file system from NAS is read and stored on the external target as a streaming backup using the {{site.data.keyword.baas_full_notm}} as a temporary cache.

![CloudArchive Direct](../Resources/Images/NAS/CloudArchiveDirect.png)

## Cloudarchive direct characteristics
{: #cloudarchive_direct_characteristics}


| Feature | CloudArchive Direct |
| --- | --- |
| Supported Sources | *   VMware<br>    <br>*   NAS sources |
| Mode of enablement | While creating or editing a Protection Policy. |
| Deduplication | Supported |
| Number of Objects per Protection Group | Multiple |
| Data archival | Incremental forever |
| Metadata archival | Incremental forever |
| Granularity level of incremental archival | Block-level—Need to perform only incremental archival of a file if the data or metadata of the file is updated. |
| Native format as primary | No  |
| Tiering of objects using {{site.data.keyword.baas_full_notm}} Data Movement | Supported—can choose to tier the archived data to a cheaper storage tier. |
| CloudRetrieve | Supported |
| Vault unregistration | Not supported |
{: caption="" caption-side="bottom"}

## Supported external targets
{: #supported_external_targets}

For the list of supported external targets, see [Supported Software](../ReleaseNotes/SupportedVersions.htm#CloudArc).


| CloudArchive Direct Workflow | VMware |     |     | NAS |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Standard Storage Tier\* | Archive Storage Tier\* | Hot Tier\* | Standard Storage Tier\* | Archive Storage Tier\* | Hot Tier\* |
| --- | --- | --- | --- | --- | --- | --- |
| Direct Archival | Yes | Yes | Yes | Yes | Yes | Yes |
| Restore | Yes | Yes | Yes | Yes | Yes | Yes |
| File-level restore | Yes | No  | Yes | Yes | Yes | Yes |
| Indexing | Yes | No  | Yes | Yes | Yes | Yes |
| CloudRetrieve | Yes | Yes | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

## Considerations
{: #considerations}

Review and understand the following considerations if you are performing CloudArchive Direct of NAS sources:

*   When you upgrade the {{site.data.keyword.baas_full_notm}} from 6.6 or below versions to the latest version, the existing Protection Group created with CloudArchive Direct native mode will not automatically switch to the new CloudArchive Direct format. Therefore, you must pause the existing Protection Group and then create a new Protection Group for the objects with CloudArchive Direct new format.


Review and understand the following considerations if you are performing CloudArchive Direct of VMware VMs to **Archive Storage tier** (AWS S3-Glacier, AWS S3-Glacier-Deep-Archive, AWS S3-Glacier-Deep-Archive, AWS S3-Glacier-Deep-Archive Gov):

*   File and folder recovery is not supported.

*   Instant recovery of VMware VMs is not supported.

*   Instant Volume Mount is not supported.

*   Test & Dev (Cloning) is not supported.

*   Virtual Disks (VMDK) recovery is not supported.

*   Indexing is not supported.
