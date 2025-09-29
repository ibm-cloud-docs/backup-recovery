---

copyright:
  years: 2024
lastupdated: "2025-08-27"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Add or edit a protection group for physical servers
{: #add_or_edit_a_protection_group_for_physical_servers}

In {{site.data.keyword.baas_full}} Data Cloud, Protection Groups use Protection Policies. Protection Policies reflect the backup and archival frequency, and retention requirements for each Protection Run. A Protection Group defines operational requirements, such as which source objects to protect, the Protection Policy to use, and operational considerations like indexing, exclusions and inclusions, and more.

A Protection Group that protects physical servers captures snapshots which are required for the following tasks:

*   Recover files
*   Instant volume mount
*   Replicate snapshots
*   Archive snapshots

### Supported workflows
{: #supported_workflows}

For more information about the supported workflows on protection groups for physical servers (either Block-based or File-based), refer to [Physical Server Workflows](../../ReleaseNotes/SupportedWorkflows.htm#PhysicalServerWorkflows).

### Before you begin
{: #before_you_begin}

When you create a Protection Group, you can select an existing source, policy, or storage domain. You can also create them while creating the Protection Group. However, you might find it easier to create them prior to creating the Protection Group, as described in the following topics.

*   [Register or Edit a Physical Server](SourcePhysicalAdd.htm)
*   [Create or Edit a Standard Policy](PolicyCreateEdit.htm)
*   [Create or Edit Storage Domains](../Platform/ViewBoxesCreateEdit.htm)

### To add or edit a protection group for physical server
{: #to_add_or_edit_a_protection_group_for_physical_server}

Once you have [registered your physical server](SourcePhysicalAdd.htm) as a source, you're ready to use {{site.data.keyword.baas_full_notm}} Data Cloud to protect it.

You perform File-based backups for the physical servers.

*   [Performing a File-based Backup](JobServerPhysical_FileBased.htm)


{{site.data.keyword.baas_full_notm}} recommends not to run backup and restore protection groups in parallel on the same physical server path.
