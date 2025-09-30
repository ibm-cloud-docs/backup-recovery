---

copyright:
  years: 2025
lastupdated: "2025-09-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Create or edit storage domains
{: #create_or_edit_storage_domains}

A Storage Domain, formerly known as a View Box, is a named storage location on a Cluster. A Storage Domain defines the policy and frequency for deduplication among other configurations such as Compression, Erasure Coding, and Replication Factor. A Storage Domain contains Views. When you configure a Protection Group, you specify a Storage Domain and during a Protection Group Run, the {{site.data.keyword.baas_full_notm}} stores the Snapshots in that Storage Domain.

**To create or edit a Storage Domain:**

1. Navigate to **Settings > Summary** and select the **Storage Domains** tab.
2. In the Storage Domains page, complete one of the following actions:
    *   Click **Add Storage Domain** located at the top right of the page.
    *   Edit an existing Storage Domain by clicking the Actions Menu  and selecting **Edit** 

3. **Storage Domain Name:** Enter a name for the Storage Domain.
4. **Associated External Target**: Select the external target to be associated with the Storage Domain. This associated external target stores all the data landing on this Storage Domain. Each Storage Domain can only be associated with one external target and vice versa. Once set, the associated external target cannot be changed.

    **NOTE**: This field is only available for GCP Next-Gen Cloud Edition.

5. **Authentication Provider:** You can map the Storage Domain to an AD domain if the {{site.data.keyword.baas_full_notm}}, version 6.5.1d or higher, is AD domain-joined . Mapping to a specific AD domain means that users from only that AD domain have permission to view the Storage Domain's contents when accessing through SMB. By default, a Storage Domain is accessible to users from all joined AD domains.
6. **Deduplication:** Optionally toggle on. If Deduplication is on, the {{site.data.keyword.baas_full_notm}} eliminates duplicate blocks of repeating data stored on the {{site.data.keyword.baas_full_notm}} thus reducing the amount of storage space needed to store data. 

    Toggling **Deduplication** on for a non-Deduplication Storage Domain that uses Cloud Tier may result in a large number of reads from the Cloud Tier if Storage Domain data has already been sent to the Cloud Tier.

    **Inline Deduplication:** Optionally toggle on. If Inline Deduplication is on, deduplication occurs when the {{site.data.keyword.baas_full_notm}} writes the data to the {{site.data.keyword.baas_full_notm}} View. If Inline Deduplication is off, deduplication occurs after the {{site.data.keyword.baas_full_notm}} writes the data to the {{site.data.keyword.baas_full_notm}} View. 

7. **Compression:** Optionally toggle on. If Compression is on, the {{site.data.keyword.baas_full_notm}} compresses all the data stored in the Storage Domain thus reducing the amount of storage space needed to store the data. If Compression is on and then turned off later, the current data on the Storage Domain is not decompressed but any new data stored on the Storage Domain is not compressed.

    **Inline Compression:** Optionally toggle on. If Inline Compression is on, compression occurs when the {{site.data.keyword.baas_full_notm}} saves the blocks to the Cluster. If Inline Compression is off, compression occurs after the {{site.data.keyword.baas_full_notm}} writes the data to the {{site.data.keyword.baas_full_notm}}.

    If both Compression and Inline Deduplication are on, {{site.data.keyword.baas_full_notm}} recommends you turn **Inline Compression** on.

8. **Encryption:** Toggle to enable encryption for the Storage Domain. Encryption can be enabled only when you are creating the Storage Domain. Once the Storage Domain is created, the options to enable encryption or modify its encryption settings are disabled in the {{site.data.keyword.baas_full_notm}}.

    By default, {{site.data.keyword.baas_full_notm}} uses **Internal KMS** for encryption. If encryption is enabled at the cluster level, then data within the Storage Domains are also encrypted and you don't have to enable encryption for each Storage Domain. You can register an external KMS and select one of the following from the drop-down list:

    *   AWS

    *   KMIP Compliant


    For more information, see [External Key Management Service](../../KMS/External KMS.htm).

9. Expand **Show Advanced Settings** to see the list of additional View settings.

10. **Physical Quota:** Optionally toggle on to limit the Storage Domain's physical capacity and then set the capacity. The capacity must be a whole number.

    Toggle **Alert** on to trigger an Alert when the Storage Domain reaches the specified threshold.

11. **Logical Quota:** Optionally toggle on to set a default logical capacity limit for Views created in this Storage Domain and set the capacity. Logical data is fully hydrated and expanded.

    Toggle **Alert** on to trigger an Alert when the View reaches the specified threshold.

12. **Cloud Tier:** Optionally toggle on. If Cloud Tier is on, the {{site.data.keyword.baas_full_notm}} moves cold (rarely accessed) data to the designated External Target. Once Cloud Tier is on, it cannot be turned off.

    To turn Cloud Tier off, contact [IBM Cloud Support](../../Support/ContactSupport.htm) to disable the feature.

    If Cloud Tier is on, select an External Target or register a new External Target.

    Set the coldness of data to tier.

    If the **Physical Quota** toggle is off, the cluster’s default capacity and space utilization threshold is used for tiering.

    Optionally toggle on **Override Cluster Threshold** and set the **Space Utilization Threshold** to activate cloud tiering for the Storage Domain.

13. **Fault Tolerance:** Select the number of hard drive, node, chassis, or rack failures that can occur before a Cluster Health Alert is triggered. The available choices are determined by the number of nodes, chassis, or racks in the Cluster. The Cluster's global **Custom Failure Domain** setting determines the failure domain (Node, Chassis or Rack) and the number of failures tolerated at the Storage Domain level. For more details about global cluster level failure domain settings, see [Configure Cluster Settings](../Admin/ConfigureSettings.htm).

    Fault Tolerance is not available for single node {{site.data.keyword.baas_full_notm}} (ROBO). By default, the Storage Domain is configured with RF1.

    The following table provides the fault tolerance options you can select based on the failure domain set at the cluster level and the number of components available in the {{site.data.keyword.baas_full_notm}}.


    | Minimum Failures Tolerated | Description |
    | --- | --- |
    | 0D:0N | No hard disk failure or node failure can occur without triggering the alert. This option provides Redundancy Factor 1 (RF1). If you select this option, you are opting for no replication and indicating that you trust the underlying datastore medium's resiliency. This option is available only in the Virtual Edition clusters.<br><br>If you select this option, Erasure Coding will be disabled. If RF1 is enabled on a multi-node Virtual Edition Cluster, during the cluster upgrade, data will be temporarily unavailable until the associated node comes online. |
    | 1D:0N | Tolerate failure of 1 drive. No node failure is tolerated.<br><br>This is only available on physical ROBO clusters, it provides RF2 like redundancy and is set by default. |
    | 1D:1N | Tolerate failure of 1 drive or 1 node. The available options to select are RF2 or EC x:1. |
    | 2D:1N | Tolerate failure of 2 drives or at least 1 node. The available options to select include RF3 and EC x:2.<br><br>This option is available only if the Cluster has three or more nodes.<br><br>{{site.data.keyword.baas_full_notm}} will always make the best effort to place each EC stripe unit to be on separate nodes when sufficient nodes are available. Only when sufficient nodes are not present the EC stripe units will be placed within the same node but on separate disks. |
    | 2D:2N | Tolerate failure of 2 drives or 2 nodes.<br><br>This setting is only available if there are at least four nodes.<br><br>With this setting, the available options are RF3 or EC x:2. {{site.data.keyword.baas_full_notm}} will always try to place each EC stripe unit on separate nodes when sufficient nodes are available.<br><br>You cannot change the fault tolerance setting to 2D:2N if the metadata usage is >=50% of the available storage. <br><br>The Erasure Coding (EC) setting is not recommended for Clusters with fewer than five nodes because the data cannot be healed after one Node failure. Your data will be at risk until the failed Node is replaced. |
    | 3D:1N | Tolerate failure of 3 drives or 1 node.<br><br>This setting is only available if there are at least three nodes. With this setting, the available options are RF4, EC x:3, or EC x:4. |
    | 3D:2N | Tolerate failure of 3 drives or 2 nodes.<br><br>This setting is only available if there are at least seven nodes. With this setting, the available options are RF4, EC x:3, or EC x:4. |
    | 0D:0C | No hard disk failure or chassis failure can occur without triggering the alert.<br><br>This option provides Redundancy Factor 1 (RF1). If you select this option, you are opting for no replication and indicating that you trust the underlying datastore medium's resiliency. This option is available only in the Virtual Edition clusters.<br><br>If you select this option, Erasure Coding will be disabled. If RF1 is enabled on a multi-node Virtual Edition Cluster, during the cluster upgrade, data will be temporarily unavailable until the associated chassis comes online. |
    | 1D:1C | Tolerate failure of 1 drive or 1 chassis. This setting is only available if there are at least three chassis. With this setting, the available options are RF2 or EC x:1. |
    | 2D:1C | Tolerate failure of 2 drives or 1 chassis. This setting is only available if there are at least three chassis. With this setting, the available options are RF3 or EC x:2. |
    | 2D:2C | Tolerate failure of 2 drives or 2 chassis. This setting is only available if there are at least four chassis. With this setting, the available options are RF3 or EC x:2. |
    | 3D:1C | Tolerate failure of 3 drives or 1 chassis. This setting is only available if there are at least three chassis. With this setting, the available options are RF4, EC x:3, or EC x:4. |
    | 3D:2C | Tolerate failure of 3 drives or 2 chassis. This setting is only available if there are at least seven chassis. With this setting, the available options are RF4, EC x:3, or EC x:4. |
    | 1D:1R | Tolerate failure of 1 drive or 1 rack. This setting is only available if there are at least three racks. With this setting, the available options are RF2 or EC x:1. |
    | 2D:1R | Tolerate failure of 2 drives or 1 rack. This setting is only available if there are at least three racks. With this setting, the available options are RF3 or EC x:2. |
    | 2D:2R | Tolerate failure of 2 drives or 2 racks. This setting is only available if there are at least four racks. With this setting, the available options are RF3 or EC x:2. |
    | 3D:1R | Tolerate failure of 3 drives or 1 rack. This setting is only available if there are at least three racks. With this setting, the available options are RF4, EC x:3, or EC x:4. |
    | 3D:2R | Tolerate failure of 3 drives or 2 racks. This setting is only available if there are at least seven racks. With this setting, the available options are RF4, EC x:3, or EC x:4. |

    The 2D in 2D:1N or 2D:2N has several effects at the same time:

    *   2D allows selecting higher EC X:Y levels with fewer nodes. For example, an X+Y complete EC unit can be formed from n/2 nodes (assuming 2 disks per node taken for the single EC unit).
    *   Survive 2 simultaneous disk (HDD) failures OR 2 simultaneous node failures with 2D:2N with EC X:2. The disk failures are per cluster and not per node.

    *   Data stripe units from a single EC stripe are spread across as many nodes as possible. For example, if there are 10 nodes and EC 5:2 has been configured, the EC stripes will be spread across 7 separate nodes even if the protection level is 2D:1N. If there are 4 nodes, the EC 5:2 bits will be spread across 4 nodes with a 2D:1N protection level, with 3 of the 4 nodes having 2 stripe units each on separate disks.

    *   2 node failures can be tolerated with EC X:2 if the number of nodes is at least X+2.

    *   Tolerate SSD (metadata) failures of up to 2.


    

14. **Erasure Coding:** Toggle on for better fault tolerance and capacity utilization. You can also select an Erasure Coding setting from the drop-down list. The available choices are determined by the **Fault Tolerance** setting and the number of nodes, chassis, or racks in the {{site.data.keyword.baas_full_notm}}. The settings indicate the ratio of the data stripes to the code stripes, expressed as x:y. For example, 2:1, 2:2, 3:1, 3.2, 4:1, 4:2, 4:3, 5:1, 5:2, or 5.3.

   

    The following table shows possible EC settings that can be set for the required resiliency of hard drives and nodes. The table indicates the number of failures tolerated in disks _or_ nodes at any EC or RF level. For example, if a 5 node {{site.data.keyword.baas_full_notm}} is set to EC4:2, failures are tolerated in 2 disks or 1 node.

    

    From {{site.data.keyword.baas_full_notm}} 6.6 onwards, you can use EC 8:2 for Azure, AWS, GCP, Imanis, MS SQL file-based, Physical Server, VMware, NAS, and SAN backup workloads.


    | Number of Nodes  <br>in the Cluster | Number of Failures Tolerated |     | Storage Domain Resilience Settings |     |
    | --- | --- | --- | --- | --- |
    | Disks (D) | Nodes (N)  <br>or SSD | EC  | RF  <br>(IF EC is Disabled) |
    | --- | --- | --- | --- | --- |
    | 3   | 1   | 1   | 2:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 4   | 1   | 1   | 2:1, 3:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 5   | 1   | 1   | 2:1, 3:1, 4:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 6   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 7   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |
    | 8   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2, 6:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |
    | 9   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2, 6:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |
    | 10+ | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |

   

    The following table shows possible EC settings that can be set for the required resiliency of chassis.

    From {{site.data.keyword.baas_full_notm}} 6.6 onwards, you can use EC 8:2 for Azure, AWS, GCP, Imanis, MS SQL file-based, Physical Server, VMware, NAS, and SAN backup workloads.


    | Number of Chassis  <br>in the Cluster | Number of Failures Tolerated |     | Storage Domain Resilience Settings |     |
    | --- | --- | --- | --- | --- |
    | Disks (D) | Nodes (N)  <br>or SSD | EC  | RF  <br>(IF EC is Disabled) |
    | --- | --- | --- | --- | --- |
    | 3   | 1   | 1   | 2:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 4   | 1   | 1   | 2:1, 3:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 5   | 1   | 1   | 2:1, 3:1, 4:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 6   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 7   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |
    | 8   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2, 6:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |
    | 9   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2, 6:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |
    | 10+ | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |

   

    The following table shows possible EC settings that can be set for the required resiliency of racks.

    From {{site.data.keyword.baas_full_notm}} 6.6 onwards, you can use EC 8:2 for Azure, AWS, GCP, Imanis, MS SQL file-based, Physical Server, VMware, NAS, and SAN backup workloads.


    | Number of Racks  <br>in the Cluster | Number of Failures Tolerated |     | Storage Domain Resilience Settings |     |
    | --- | --- | --- | --- | --- |
    | Disks (D) | Racks (R) | EC  | RF  <br>(IF EC is Disabled) |
    | --- | --- | --- | --- | --- |
    | 3   | 1   | 1   | 2:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 4   | 1   | 1   | 2:1, 3:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 5   | 1   | 1   | 2:1, 3:1, 4:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 6   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 7   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |
    | 8   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2, 6:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |
    | 9   | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2, 6:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |
    | 10+ | 1   | 1   | 2:1, 3:1, 4:1, 5:1 | RF2 |
    | 2   | 1   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 2   | 2   | 2:2, 3:2, 4:2, 5:2, 6:2, 8:2 | RF3 |
    | 3   | 1   | 3:3, 4:3, 5:3 | RF4 |
    | 3   | 2   | 3:3, 4:3, 5:3 | RF4 |

    **Post Processing Only:** Toggle OFF to write erasure coding as data comes to the {{site.data.keyword.baas_full_notm}}. When this option is ON, erasure coding is done post-process due to which data ingest performance rate is higher. .

    ECX:4 is available only as a post processing option where the data is initially ingested as RF3 and then converted to X:4. Changing the viewbox configuration for EC X:Y ( with Y < 4) to EC 12:4 or EC 16:4 is not permitted and not displayed in the viewbox editing page. However, you can switch from EC12:4 to EC16:4 and back, but the configuration only takes effect for any new data.

    EC12:4 and EC16:4 are Early Access features. Contact [IBM Cloud Support](../../Support/ContactSupport.htm) to enable the feature.

    

15. Click **Create Storage Domain**.
