---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Add or edit a protection group for physical servers
{: #add_or_edit_a_protection_group_for_physical_servers}


In {{site.data.keyword.baas_full_notm}}, [Protection Groups](/docs/backup-recovery?topic=backup-recovery-protection-groups) use [Protection Policies](/docs/backup-recovery?topic=backup-recovery-baas-policy-creation). Protection Policies reflect the backup and archival frequency, and retention requirements for each Protection Run. A Protection Group defines operational requirements, such as which source objects to protect, the Protection Policy to use, and operational considerations like indexing, exclusions and inclusions, and more.

A Protection Group that protects physical servers captures snapshots which are required for the following tasks:

*   Recover files
*   Instant volume mount
*   Replicate snapshots
*   Archive snapshots

### Supported workflows
{: #supported_workflows}

For more information about the supported workflows on protection groups for physical servers (either Block-based or File-based), refer to [Physical Server Workflows](/docs/backup-recovery?topic=backup-recovery-supported_workflows_and_external_targets#physical_server_workflows).

### Before you begin
{: #before_you_begin}

When you create a Protection Group, you can select an existing source, policy, or storage domain. You can also create them while creating the Protection Group. However, you might find it easier to create them prior to creating the Protection Group, as described in the following topics.

*   [Register or Edit a Physical Server](/docs/backup-recovery?topic=backup-recovery-register-or-edit-a-physical-server)
*   [Create or Edit a Standard Policy](/docs/backup-recovery?topic=backup-recovery-create_or_edit_a_standard_policy)
*   [Create or Edit Storage Domains](/docs/backup-recovery?topic=backup-recovery-create_or_edit_storage_domains)

### Compare block-based and file-based backup methods
{: #compare_block-based_and_file-based_backup_methods}

{{site.data.keyword.baas_full}} offers two distinct approaches for physical server backups: Block-based and File-based. While both methods serve the purpose of safeguarding critical data, understanding their differences is essential for choosing the most suitable backup strategy for your organization's needs.


| {{site.data.keyword.baas_full_notm}} Facet | {{site.data.keyword.baas_full_notm}} Block-based Physical Server Backups | {{site.data.keyword.baas_full_notm}} File-based Physical Server Backups |
| --- | --- | --- |
| Granularity | Operates at the disk block level, capturing modified or new data blocks. | Operates at the file level, preserving entire files for backup and recovery. |
| Speed and Performance | Enables faster backup and recovery times by tracking changes at the block level and transferring only modified data blocks. | Offers straightforward backup and recovery operations, relying on the identification and preservation of entire files. |
| Optimized Storage Efficiency | Significantly reduces storage requirements by capturing only the changed data blocks. | File-based backups rely on deduplication within the storage domain for optimization. It provides file-level accessibility, allowing selective recovery. |
| Simplicity and Familiarity | Requires familiarity with block-level operations. | Follows traditional file-level backup approaches that administrators are accustomed to. |
| Flexibility and Accessibility | Focuses on individual data blocks, enabling more granular and incremental backups. | Ensures the accessibility of individual files and directories, simplifying selective recovery. |
| Metadata and Indexing | Relies on metadata which is available once indexing occurs after the backup is complete. | Includes comprehensive metadata and indexing capabilities for quick search and file location. |
| Dependencies | Block-based backups depend on the volume CBT driver on Windows and LVM snapshots in Linux. | No dependency in the case of file-based backups. |
| Supported Configurations | Block-based backups are supported with specific file systems (NTFS for Windows, small group for Linux x64). | File-based backups are less restrictive and can work with a wider range of filesystems and operating systems (AIX etc) |
| Supported Recoveries | Block-based allows for IVM recovery, | File-based only allows for File-level recovery. |
{: caption="" caption-side="bottom"}

In summary, {{site.data.keyword.baas_full_notm}} provides two distinct methodologies for physical server backups: Block-based and File-based. While Block-based backups excel in speed, efficiency, and granularity at the disk block level, File-based backups offer simplicity, flexibility, and file-level accessibility. Choosing the appropriate backup method depends on your organization's specific requirements, data protection goals, and recovery objectives.

The decision between {{site.data.keyword.baas_full_notm}}'s Block-based and File-based Physical Server backups should be based on factors such as backup speed, storage efficiency, data granularity, ease of recovery, and the desired level of accessibility.

By understanding the differences between the two backup approaches, organizations can effectively leverage the backup functionality to enhance their data protection strategies to optimize their overall businesses.

### To add or edit a protection group for physical server
{: #to_add_or_edit_a_protection_group_for_physical_server}

Once you have [registered your physical server](/docs/backup-recovery?topic=backup-recovery-register-or-edit-a-physical-server) as a source, you're ready to use {{site.data.keyword.baas_full_notm}} Data Cloud to protect it.

You can either perform Block-based or File-based backups for the physical servers.

*   Performing a Block-based Backup <!--(JobServerPhysical_Blockbased.htm)

*   [Performing a File-based Backup](/docs/backup-recovery?topic=backup-recovery-protect_a_physical_server_file-based)


{{site.data.keyword.baas_full_notm}} recommends not to run backup and restore protection groups in parallel on the same physical server path.
