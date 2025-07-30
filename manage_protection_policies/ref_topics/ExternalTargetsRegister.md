---

copyright:
  years: 2024
lastupdated: "2024-10-7"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Register an external target
{: #register_an_external_target}

12 September 2024

You can register an external target with the {{site.data.keyword.baas_full_notm}} for Cloud Archive, CloudArchive Direct, or for Cloud Tiering.

## Supported external target
{: #supported_external_target}

The following table provides the mapping of external target types that can serve as Cloud Archive, CloudArchive Direct, and Cloud Tier:

      
| Cloud Provider | Class of Storage | Cloud Archive |     |     | CloudArchive Direct | Cloud Tier2 |
| --- | --- | --- | --- | --- | --- | --- |
| With Periodic Full | With Incremental Forever | Always Full |
| --- | --- | --- | --- | --- | --- | --- |
| **AWS** | Glacier (Legacy) Standard | Supported | Not Supported | NA  | Not Supported | Not Supported |
| Glacier (Legacy) Gov | Supported | Not Supported | NA  | Not Supported | Not Supported |
| Glacier (Legacy) C2S | Supported | Not Supported | NA  | Not Supported | Not Supported |
| S3 Standard | Supported | Supported | NA  | Supported | Supported |
| S3 Gov | Supported | Not Supported | NA  | Not Supported | Supported |
| S3 C2S | Supported | Not Supported | NA  | Not Supported | Supported |
| S3-Glacier Standard<br><br>(Currently S3 Glacier Flexible Retrieval) | Supported | Supported | NA  | Supported | Not Supported |
| S3-Glacier Gov | Supported | Supported | NA  | Supported | Not Supported |
| S3-Glacier-Deep-Archive Standard | Supported | Supported | NA  | Supported | Not Supported |
| S3-Glacier-Deep-Archive Gov | Supported | Supported | NA  | Supported | Not Supported |
| S3-IA Standard | Supported | Supported | NA  | Supported | Not Supported |
| S3-IA Gov | Supported | Supported | NA  | Supported | Not Supported |
| S3-IA C2S | Supported | Not Supported | NA  | Not Supported | Not Supported |
| S3-Intelligent Standard | Supported | Not Supported | NA  | Not Supported | Supported |
| S3-Intelligent Gov | Supported | Not Supported | NA  | Not Supported | Not Supported |
| S3-One-Zone Standard | Supported | Supported | NA  | Supported | Not Supported |
| S3-One-Zone Gov | Supported | Supported | NA  | Supported | Not Supported |
| Snowball Edge[\*](#snowball) | Supported | Not Supported | NA  | Not Supported | Not Supported |
| S3 Glacier Instant Retrieval | Supported | Supported | NA  | Supported | Not Supported |
| S3 Glacier Instant Retrieval Gov | Supported | Supported | NA  | Supported | Not Supported |
| Azure | Archive Blob | Supported | Supported | NA  | Not Supported | Not Supported |
| Archive Blob-Gov | Supported | Not Supported | NA  | Not Supported | Not Supported |
| Cool Blob-Standard | Supported | Supported | NA  | Supported | Not Supported |
| Cool Blob-Gov | Supported | Not Supported | NA  | Not Supported | Not Supported |
| Hot Blob-Standard | Supported | Supported | NA  | Supported | Supported |
| Hot Blob-Gov | Supported | Not Supported | NA  | Not Supported | Supported |
| Google Cloud Platform | Coldline | Supported | Not Supported | NA  | Not Supported | Not Supported |
| Multi-Regional | Supported | Not Supported | NA  | Not Supported | Supported |
| Nearline | Supported | Not Supported | NA  | Not Supported | Not Supported |
| Regional | Supported | Not Supported | NA  | Not Supported | Supported |
| Standard | Supported | Not Supported | NA  | Not Supported | Supported |
| **Google Next-Gen Cloud Edition** | Standard | Not Supported | Not Supported | NA  | Supported | Not Supported |
| Oracle | Archive Storage | Supported | Not Supported | NA  | Not Supported | Not Supported |
| Object Storage | Supported | Not Supported | NA  | Not Supported | Supported |
| Other External Targets | NAS | Supported | Supported[\*\*](#nas_s3) | NA  | Supported[\*\*](#nas_s3) | Not Supported |
| QStar Tape | Not Supported | Not Supported | Supported \*\*\* | Not Supported | Not Supported |
| S3 Compatible Object Storage -Standard Tier (Regular) | Supported | Supported[\*\*](#nas_s3) | NA  | Supported[\*\*](#nas_s3) | Supported |
| S3 Compatible Object Storage -Tape Based | Not Supported | Not Supported | Supported \*\*\* | Not Supported | Not Supported |
{: caption="" caption-side="bottom"}

\[\*\]**AWS Snowball and Snowball Edge**

*   Cohesity does not support AWS legacy Snowball as an external target.
    
*   AWS Snowball Edge can be used as a local S3 target by selecting **Snowball Edge** as the **Storage Class** while registering the AWS external target.
    
*   Contact [IBM Cloud Support](../../Support/ContactSupport.htm) if you want to ship the Snowball Edge device to AWS.
    

\[\*\*\]S3-Compatible Standard Tier and NAS

With incremental forever archival format, the {{site.data.keyword.baas_full_notm}} will download a portion of the data from the S3-compatible Standard tier and NAS external targets for the space reclamation process and reupload it after compaction. This leads to increased network bandwidth usage. Cohesity recommends having sufficient network bandwidth between the cluster and the external target, for example, hosting the external target within the same data center or network as the {{site.data.keyword.baas_full_notm}}.

\[\*\*\*\]**S3-Compatible Tape Based**

For a list of the actions or workflows that are supported for each external target, see [Supported Workflows and External Targets](../../ReleaseNotes/SupportedWorkflows.htm#ExternalTargets). For example, for a list of the external targets that support downloading files and folders, see the **Download Files/Folders from Archive** row in [Supported Workflows and External Targets](../../ReleaseNotes/SupportedWorkflows.htm#ExternalTargets).

For a list of the actions or workflows that are supported for each external target, see _External Targets_ in the _Release Notes_. For example, for a list of the external targets that support downloading files and folders, see the **Download Files/Folders from Archive** row in _External Targets_ section of the _Release Notes_.

## S3-compatible object storage - standard tier
{: #s3-compatible_object_storage_-_standard_tier}

IBM Cloud Supports any S3-compatible device as an S3-Standard tier (Regular as storage class) external target with similar capabilities to an AWS S3-Standard. Any S3-compatible targets that use S3-Standard must be registered as a regular storage class. The S3-compatible uses S3-Standard APIs for archival and recovery. This Standard tier is supported for both incremental with periodic full and incremental forever archival formats. Compression and deduplication are supported for both archival formats.

With incremental forever archival format, the {{site.data.keyword.baas_full_notm}} will download a portion of the data from the S3-compatible Standard tier external target for the space reclamation process and reupload it after compaction. This leads to increased network bandwidth usage. Cohesity recommends having sufficient network bandwidth between the cluster and the external target; for example, hosting the external target within the same data center or network as the {{site.data.keyword.baas_full_notm}}.

Any S3-compatible targets that use Tape storage in the backend are not supported with the Standard tier; instead, you must register them as S3-compatible with the Tape-based Storage Class.

The following table provides the list of S3-Compatible Object Storage - Standard Tier and their details:

      
| Object Storage Vendor/ Cloud Provider | Product Names | Service URL | Port Number | Transport | AWS Signature Version | Additional Details |
| --- | --- | --- | --- | --- | --- | --- |
| Backblaze | B2 Cloud Storage with v2 API version | s3.us-west-000  <br>.backblazeb2.com | 443 | HTTPS | AWS Sign ver4 | Requires V4 signing to provide S3 compatibility. |
| Cohesity | Cohesity Data Cloud | \-  | 3000 | HTTPS | AWS Sign ver4 | \-  |
| Dell EMC | ECS | <fqdn-or-IP>:  <br><portnumber> | 9021 | HTTPS | AWS Sign ver4 | It is recommended to use AWS Sign ver4 with HTTPS port 9021. |
| 9020 | HTTP | AWS Sign ver2 |
| Hitachi Vantara | Hitachi Content Platform | <fqdn-or-IP> | Not Required | HTTPS | AWS Sign ver4 | Both v4 and v2 are supported for HTTPS. |
| <fqdn-or-IP>:8080 | 8080 | HTTP | AWS Sign ver2 | \-  |
| Huawei Software Technologies Company | Object Storage Service | obs.ap-southeast-3.myhuaweicloud. <br>com | Not required | HTTPS | AWS Sign ver2 | \-  |
| IBM | Cloud Object Storage | s3.private.<region>-  <br><site>.cloud-object-storage.appdomain. <br>cloud | Not required | HTTPS | AWS Sign ver4 | Both v4 and v2 work; v4 is the preferred version. |
| 11:11 Systems | 11:11 Cloud Object Storage | us-central-1a.object.ilandcloud.com | Not required | HTTPS | AWS Sign ver4 | \-  |
| us-central-1b.object.ilandcloud.com |
| eu-west-1a.object.ilandcloud.com |
| eu-west-2a.object.ilandcloud.com |
| ap-southeast-1a.object.ilandcloud.com |
| MInIO | MInIO Object Storage | <fqdn-or-IP>:9000 | 9000 | HTTP | AWS Sign ver2 | HTTP is the default protocol, but can be configured for HTTPS. |
| NetApp | StorageGrid | <fqdn-or-IP>:8082 | 8082 | HTTPS | AWS Sign ver2 | 8082 via storage node. |
| StorageGRID Webscale | <fqdn-or-IP>:18082 | 18082 | HTTPS | AWS Sign ver2 | 18082 via gateway node. |
| Pure Storage | Pure Flashblade | \-  | 443 | \-  | AWS Sign ver2 | \-  |
| Wasabi | s3.us-east-1.wasabisys.com  <br>s3-<region>.wasabisys.com s3.wasabisys.com | \-  | Not required | HTTPS | AWS Sign ver4 | Both v4 and v2 work; v4 is the preferred version. |
| Netdepot | \-  | us-central-1-s3. <br>netdepot.com | Not required | HTTPS | AWS Sign ver4 | \-  |
{: caption="" caption-side="bottom"}

## S3-compatible object storage - tape-based
{: #s3-compatible_object_storage_-_tape-based}

IBM Cloud Supports any S3-compatible targets that use tape storage in the backend as an external target and have capabilities similar to those of AWS S3 Glacier. Any S3-compatible targets that use Tape storage in the backend must be registered as Tape-based Storage Class. The tape-based Storage Class uses S3 Glacier APIs for archival and recovery. This supports always full with compression enabled by default and does not support deduplication and incremental archives.

S3-compatible Tape-based targets that deviate in their capabilities compared to AWS S3 Glacier tier are not supported. For the supported workflows of Tape-based, see [Supported Workflows and External Targets](../../ReleaseNotes/SupportedWorkflows.htm).

      
| Object Storage Vendor/ Cloud Provider | Product Names | Service URL | Port Number | Transport | AWS Signature Version | Additional Details |
| --- | --- | --- | --- | --- | --- | --- |
| IBM | Storage Protect | <fqdn-or-IP>:9000 | 9000 | HTTPS | Storage Protect | <fqdn-or-IP>:9000 |
{: caption="" caption-side="bottom"}

**Related topics**:

*   [Register an External Target for Archival](ExternalTargetsArchive.htm)
    
*   [Register an External Target for Cloud Tier](RegisterExternalTargetTier.htm)
