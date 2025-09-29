---

copyright:
  years: 2024
lastupdated: "2025-08-27"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protect File Server Role deployed on Windows Server Failover Cluster

A failover cluster is a group of independent computers that work together to increase the availability and scalability of clustered roles (formerly called clustered applications and services). Windows Server Failover Cluster (WSFC) supports multiple roles, this feature supports the protection of a WSFC File Server role.

### Prerequisites

*   Make sure the following prerequisites are met before registering a File Server role as a source in the {{site.data.keyword.baas_full_notm}}:

*   You should create shares for the volumes to be discovered as WSFC File Server role volume on the {{site.data.keyword.baas_full_notm}}.

*   The minimum supported version for WSFC Role is the {{site.data.keyword.baas_full_notm}} version 6.7 and onwards (both {{site.data.keyword.baas_full_notm}} Windows Agent and {{site.data.keyword.baas_full_notm}}).

*   WSFC is not supported in multi-tenancy and DMaaS installations.

*   Only file servers for general use are supported, scale-out file servers also known as active-active are not supported.


### Considerations

Review the following considerations before protecting a WSFC File server role using {{site.data.keyword.baas_full_notm}}:

*   IBM Cloud Supports WSFC running on Windows Server 2012, Windows Server 2012 R2, 2016, and 2019 versions.

*   WSFC File Server role protection is supported only on on-prem deployments. Data Management as a Service (Backup as a Service) and Multi-tenancy are not supported.

*   This feature supports backup and recovery of data associated with the File server role in general use.

*   For a crash-consistent backup, the first backup after failover will be full.

*   For a crash-consistent backup, the backup will fail if there is a failover during backup.

*   If there is no failover, all the backups are incremental (Leveraging CBT) for shared disks. If there is a failover/failback on a shared disk over to the other node, the next backup will be a full backup.

*   A standalone physical server protection Group can also protect all volumes associated with the File Server role. However, {{site.data.keyword.baas_full_notm}} does not recommend protecting volumes associated with the File Server roles through a standalone physical source.

*   Bare Metal Restore (BMR) is not supported for WSFC roles. However, BMR can be done on the Windows Servers which are part of the Failover Cluster.

*   Upgrading the Backup agent on the File Server role registered as a source will also upgrade the agents on all the nodes within the cluster.

*   The backups will run through the owner node of the File Server role cluster as the cluster disks are offline and in a reserve state on other nodes.


### Protecting FileServer role volumes using a File-based Backup

To create a file-based protection group for File Server Role, follow the steps listed below:

1.  Install Backup agent on each node of the Windows Server Failover Cluster and register File Server VIP as Physical Server. For more information, see [Register or Edit a Physical Server](SourcePhysicalAdd.htm).

2.  Create a Protection Group (physical server file-based). For more information, see [Add or Edit a Protection Group for Physical Servers](JobServerPhysical.htm).

3.  Enable **Protect all local volumes** option while creating a protection group if you want to protect all volumes associated with the File Server role.

    *   You should create shares for the volumes to be discovered as WSFC File Server role volume on the {{site.data.keyword.baas_full_notm}}.

    *   For protection with **Crash Consistency** enabled. If there is no failover, all the backups are incremental for the Shared disks. If there is a failover/failback on a shared disk over to the other node, the next backup will be a full backup.

    *   For protection with **Crash Consistency** disabled. All backups are incremental even if there is a failover/failback of shared disks


4.  Provide the file path in Exclusions to exclude any files/folders/drives from backup.


### Recovery

Once we have backed up the WSFC resources as file-based or block-based, recovery options like “Files or Folders”, and “Instance Volume Mount” can be used to restore data.

For more information, see [Recover Physical Servers](RecoverPhysAddRecoverTask.htm) and [Recover Physical Server Files or Folders](recover-filesfolder-physical.htm).
