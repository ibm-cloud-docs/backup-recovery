---

copyright:
  years: 2024
lastupdated: "2024-10-3"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Supported workflows and external targets
{: #supported_workflows_and_external_targets}

This section lists the workflows that are supported for different servers, databases, NAS and other functionality.

*   [Virtual Server (VMs) Workflows](#VirtualServer(VMs)Workflows)
    *   [Protection Workflows for VMware VMs](#VMwareVMsProtection)
    *   [Archival Workflows for VMware VMs](#VMwareVMsArchive)
    *   [Replication Workflows for VMware VMs](#VMwareVMsReplication)
    *   [Protection Workflows for Non-VMware-based VMs](#VirtualServerVMsProtection)
    *   [Archival Workflows for Non-VMware-based VMs](#VirtualServerVMsArchive)
    *   [Replication Workflows for Non-VMware-based VMs](#VirtualServerVMsReplication)
*   [Physical Server Workflows](#PhysicalServerWorkflows)
    *   [Protection Workflows for Physical Server (Block Based)](#PhysicalServerBlockBasedProtection)
    *   [Archival Workflows for Physical Server (Block Based)](#PhysicalServerBlockBasedArchive)
    *   [Replication Workflows for Physical Server (Block Based)](#PhysicalServerBlockBasedReplication)
    *   [Protection Workflows for Physical Server (File Based)](#PhysicalServerFileBasedProtection)
    *   [Archival Workflows for Physical Server (File Based)](#PhysicalServerFileBasedArchive)
    *   [Replication Workflows for Physical Server (File Based)](#PhysicalServerFileBasedReplication)
*   [Database Workflows](#DatabaseWorkflows)
    *   [Protection Workflows for MS SQL](#MSSQLProtection)
    *   [Archival Workflows for MS SQL](#MSSQLArchive)
    *   [Replication Workflows for MS SQL](#MSSQLReplication)
    *   [Protection Workflows for Oracle](#OracleProtection)
    *   [Archival Workflows for Oracle](#OracleArchive)
    *   [Replication Workflows for Oracle](#OracleReplication)
    *   [Protection Workflows for Cassandra, MongoDB, Couchbase, and Hadoop](#AdapterProtection)
    *   [Archival Workflows for Cassandra, MongoDB, Couchbase, and Hadoop](#AdaptersArchive)
    *   [Replication Workflows for Cassandra, MongoDB, Couchbase, and Hadoop](#AdaptersReplication)
*   [UDA Workflows](#UDAWorkflows)
    *   [Protection Workflows for UDA](#UDAProtection)
    *   [Archival Workflows for UDA](#UDAArchive)
    *   [Replication Workflows for UDA](#UDAReplication)
*   [Application Workflows](#ApplicationWorkflows)
    *   [Registration Workflows for Microsoft 365 Applications](#Microsoft_365_Applications)
    *   [Backup and Recovery Workflows for Microsoft 365 Mailboxes](#Microsoft_365_Mailbox)
    *   [Backup and Recovery Workflows for Microsoft 365 Public Folders](#Microsoft_365_Public_Folders)
    *   [Backup and Recovery Workflows for Microsoft 365 OneDrive](#Microsoft_365_OneDrive)
    *   [Backup and Recovery Workflows for Microsoft 365 SharePoint Online Sites](#Microsoft_365_SharePoint_Online_Site)
    *   [Backup and Recovery Workflows for Microsoft 365 Teams](#Microsoft_365_Teams)
    *   [Backup and Recovery Workflows for Microsoft 365 Groups](#Microsoft_365_Groups)
    *   [Cloud Archive Workflows for Microsoft 365 Applications](#Cloud_Archive_Microsoft_365_Applications)
    *   [Protection Workflows for On-Prem Applications](#OnPremProtection)
    *   [Archival Workflows for On-Prem Applications](#OnPremArchive)
    *   [Replication Workflows for On-Prem Applications](#OnPremReplication)
*   [NAS Workflows](#NASWorkflows)
    *   [Protection Workflows for NAS NFS](#NASNFSProtection)
    *   [Archival Workflows for NAS NFS](#NASNFSArchive)
    *   [Replication Workflows for NAS NFS](#NASNFSReplication)
    *   [Protection Workflows for NAS SMB/CIFS](#NASSMBCIFSProtection)
    *   [Archival Workflows for NAS SMB/CIFS](#NASSMBCIFSArchive)
    *   [Replication Workflows for NAS SMB/CIFS](#NASSMBCIFSReplication)
*   [AWS Cloud Workflows](#AWSCloudWorkflows)
    *   [Protection Workflows for AWS Cloud VM (EC2 Instance) - Cohesity Snapshot](#AWScloudprotection)
    *   [Archival Workflows for AWS Cloud VM (EC2 Instance) - Cohesity Snapshot](#AWSEC2Archive)
    *   [Replication Workflows for AWS Cloud VM (EC2 Instance) - Cohesity Snapshot](#AWSEC2Replication)
    *   [Protection Workflows for AWS Cloud - EBS Snapshot](#AWSEBSProtection)
    *   [Archival Workflows for AWS Cloud - EBS Snapshot](#AWSEBSArchive)
    *   [Replication Workflows for AWS Cloud - EBS Snapshot](#AWSEBSReplication)
    *   [Protection Workflows for AWS Cloud - RDS/Aurora Snapshot](#AWSRDSProtection)
    *   [Archival Workflows for AWS Cloud - RDS/Aurora Snapshot](#AWSRDSArchive)
    *   [Replication Workflows for AWS Cloud - RDS/Aurora Snapshot](#AWSRDSReplication)
*   [Azure Cloud Workflows](#AzureCloudWorkflows)
    *   [Protection Workflows for Azure Cloud VM - Cohesity Snapshot](#AzureProtection)
    *   [Archival Workflows for Azure Cloud VM - Cohesity Snapshot](#AzureArchive)
    *   [Replication Workflows for Azure Cloud VM - Cohesity Snapshot](#AzureReplication)
    *   [Protection Workflows for Azure Cloud VMs - CSM](#AzureCSMProtect)
    *   [Archival Workflows for Azure Cloud VMs - CSM](#AzureCSMArchive)
    *   [Replication Workflows for Azure Cloud VMs - CSM](#AzureCSMReplication)
*   [GCP Cloud Workflows](#GCPCloudWorkflows)
    *   [Protection Workflows for GCP Cloud VM - Cohesity Snapshot](#GCPProtection)
    *   [Archival Workflows for GCP Cloud VM - Cohesity Snapshot](#GCPArchive)
    *   [Replication Workflows for GCP Cloud VM - Cohesity Snapshot](#GCPReplication)
*   [Miscellaneous Workflows](#MiscellaneousWorkflows)
    *   [Protection Workflows for Views, Remote Adapters, and Pure Volumes](#MiscProtection)
    *   [Archival Workflows for Views, Remote Adapters, and Pure Volumes](#MiscArchive)
    *   [Replication Workflows for Views, Remote Adapters, and Pure Volumes](#MiscReplication)
*   [AWS External Targets](#AWSExternalTargets)
    *   [External Targets for CloudArchive (with Periodic Full) - Standard](#ExternalTargetCAStan)
    *   [External Targets for CloudArchive (with Periodic Full) - Gov](#ExternalTargetCAGov)
    *   [External Targets for CloudArchive (with Periodic Full) - C2S](#ExternalTargetCAC2S)
    *   [External Targets for CloudArchive (Incremental Forever) and CloudArchive Direct (with Dedup) - Standard](#ExternalTargetCADDStan)
    *   [External Targets for CloudArchive (Incremental Forever) and CloudArchive Direct (with Dedup) - Gov](#ExternalTargetCADDGov)
    *   [External Targets for CloudArchive (Incremental Forever) and CloudArchive Direct (with Dedup) - C2S](#ExternalTargetCADDC2S)
    *   [External Targets for CloudArchive Direct without Dedup- Standard](#ExternalTargetCADStandard)

    *   [External Targets for CloudArchive Direct without Dedup - Gov](#ExternalTargetCADGov)

    *   [External Targets for CloudArchive Direct without Dedup - C2S](#ExternalTargetCADC2S)

    *   [External Targets for CloudTier - Standard](#ExternalTargetCTStandard)
    *   [External Targets for CloudTier - Gov](#ExternalTargetCTGov)
    *   [External Targets for CloudTier - C2S](#ExternalTargetCTC2S)
*   [Microsoft Azure External Targets](#MicrosoftAzureExternalTargets)
    *   [External Targets for CloudArchive (with Periodic Full)](#ExternalTargetAzureCA)
    *   [External Targets for CloudArchive (Incremental Forever) and CloudArchive Direct (with Dedup)](#ExternalTargetAzureCADD)
    *   [External Targets for CloudArchive Direct without Dedup](#ExternalTargetAzureCAD)

    *   [External Targets for CloudTier - Standard](#ExternalTargetAzureCTStandard)
    *   [External Targets for CloudTier - Gov](#ExternalTargetAzureCTGov)
*   [GCP External Targets](#GCPExternalTargets)
    *   [External Targets for CloudArchive (with Periodic Full)](#ExternalTargetGCPCA)
    *   [External Targets for CloudArchive (Incremental Forever) and CloudArchive Direct (with Dedup)](#ExternalTargetGCPCADD)
    *   [External Targets for CloudArchive Direct without Dedup](#ExternalTargetGCPCAD)

    *   [External Targets for CloudTier](#ExternalTargetGCPCT)
*   [Oracle Cloud External Targets](#OracleExternalTargets)
    *   [External Targets for CloudArchive (with Periodic Full)](#ExternalTargetOracleCA)
    *   [External Targets for CloudArchive (Incremental Forever) and CloudArchive Direct (with Dedup)](#ExternalTargetOracleCADD)
    *   [External Targets for CloudArchive Direct without Dedup](#ExternalTargetOracleCAD)

    *   [External Targets for CloudTier](#ExternalTargetOracleCT)
*   [S3 - Compatible, NAS, Tape External Targets](#S3-Compatible,NAS,TapeExternalTargets)
    *   [External Targets for CloudArchive (with Periodic Full) - S3-Compatible, NAS, Tape](#ExternalTargetMiscCA)
    *   [External Targets for CloudArchive (Incremental Forever) and CloudArchive Direct (with Dedup) - S3-Compatible, NAS](#ExternalTargetMiscCADD)
    *   [External Targets for CloudArchive Direct without Dedup - S3 - Compatible, NAS, Tape](#ExternalTargetMiscCAD)

    *   [External Targets for CloudTier - S3-Compatible, NAS, Tape](#ExternalTargetMiscCT)
*   [NAS Tiering Workflows](#NASTieringWorkflows)
    *   [Tiering Workflows for NFS](#TierNFS)
    *   [Tiering Workflows for SMB](#TierSMB)

## Virtual server (vms) workflows
{: #virtual_server_vms_workflows}

### Protection workflows for vmware vms
{: #protection_workflows_for_vmware_vms}


| Categories | Workflows | Sources |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ESXi VM Servers | vCloud Director | Cisco HyperFlex Storage Integration | HPE Nimble Storage Snapshot Integration | Pure FlashArray Storage Snapshot Integration | Netapp Storage Snapshot Integration | NetApp Snapshot Management |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Auto-protect new objects | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| BACKUP | Incremental backup with CBT | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Full backup without CBT | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Backup, archive and replicate now | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CloudSpin to AWS and Azure | Yes | No  | No  | No  | No  | No  | N/A |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover an entire backup job run | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Instant volume mount | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Instant Recovery | Yes | Yes | N/A | N/A | N/A | N/A | N/A |
| Crash-consistent recover for apps | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CLONE | Clone an object of a job run | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Clone all the objects of a job run | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Crash-consistent recover for apps | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| TEARDOWN | Tear down objects | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Tear down instant volume mount | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CANCEL | Cancel a running job run | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Browse for a backed-up object | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Download files/folders | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Recover files | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Download a VMX file | Yes | No  | Yes | Yes | Yes | Yes | Yes |
| VIRTUAL DISKS RECOVERY | Recover Virtual Disks | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| CONVERT AND DEPLOY | Register Azure source and clone VMs to Azure | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Register AWS source and clone VMs to AWS | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
{: caption="" caption-side="bottom"}

### Archival workflows for vmware vms
{: #archival_workflows_for_vmware_vms}


| Categories | Workflows | Sources |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ESXi VM Servers | vCloud Director | Cisco HyperFlex Storage Snapshot Integration | HPE Nimble Storage Snapshot Integration | Pure FlashArray Storage Snapshot Integration | Netapp Storage Snapshot Integration | NetApp Snapshot Management |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Archive a full backup | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Recover an entire backup job run from an archive to a source | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Instant volume mount from an archive | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Crash-consistent recover for apps from an archive | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CLONE | Clone objects of a job run from an archive | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Clone all objects of a job run from an archive | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Crash-consistent clone for apps from an archive | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Cancel a running clone or recover task that is retrieving from an archive | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| FILE RECOVERY | Search for files or folders from an archive | No  | No  | No  | No  | No  | No  | N/A |
| Browse for files or folders from an archive snapshot | No  | No  | No  | No  | No  | No  | N/A |
| Download files and folders from an archive | Yes | No  | Yes | Yes | Yes | Yes | N/A |
| Recover files from an archive to a source (to the same and different sources) | Yes | No  | Yes | Yes | Yes | Yes | N/A |
| Download n VMX file from an archive | No  | No  | No  | No  | No  | No  | N/A |
| VIRTUAL DISKS RECOVERY | Recover Virtual Disks | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Download metadata for all runs of a job | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Download a snapshot (archived data) for a job run | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Clone all objects of a job run from an archive | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Recover all objects of job run from an archive to a source | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Instant volume mount from an archive | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Search files or folders from the metadata of a retrieved job run | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Browse files or folders from a retrieved snapshot | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Browse files or folders from the metadata of a downloaded job run | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Download files and folders from an archive | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Recover files from an archive to a source (to the same and different sources) | Yes | No  | Yes | Yes | Yes | Yes | N/A |
| Download a VMX file from an archive | No  | No  | No  | No  | No  | No  | N/A |
| CLOUD RETRIEVE - VIRTUAL DISKS RECOVERY | Recover Virtual Disks | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
{: caption="" caption-side="bottom"}

### Replication workflows for vmware vms
{: #replication_workflows_for_vmware_vms}


| Categories | Workflows | Sources |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ESXi VM Servers | vCloud Director | Cisco HyperFlex Storage Snapshot Integration | HPE Nimble Storage Snapshot Integration | Pure FlashArray Storage Snapshot Integration | Netapp Storage Snapshot Integration | NetApp Snapshot Management |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Replicate a full backup without CBT to a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| FAILOVER | Failover an inactive Job on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Recover an entire backup job run from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Instant volume mount of replicated data | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Crash-consistent recover for apps from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CLONE | Clone an object of a job run from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Clone all objects of a job run from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Crash-consistent clone for apps from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| TEARDOWN | Tear down object on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Tear down instant volume mount on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CANCEL | Cancel a running replication task | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Cancel a clone or recover task on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| FILE RECOVERY | Search for files or folders on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Browse for replicated object on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Download replicated files and folders from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Recover replicated files from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Download a VMX file from a remote cluster | Yes | No  | Yes | Yes | Yes | Yes | N/A |
| VIRTUAL DISKS RECOVERY | Recover Virtual Disks | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| CONVERT AND DEPLOY | Register Azure source and clone VMs to Azure | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Register AWS source and clone VMs to AWS | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
{: caption="" caption-side="bottom"}

### Protection workflows for non-vmware-based vms
{: #protection_workflows_for_non-vmware-based_vms}


| Categories | Workflows | Sources |     |     |     |
| --- | --- | --- | --- | --- | --- |
| HyperV 2016 / 2019 / 2022 VM Servers | HyperV 2012R2 VM Servers | AHV VM Servers | RHV (KVM) |
| --- | --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes | Yes |
| Refresh Application registration | N/A | N/A | N/A | Yes |
| Auto-protect new objects | Yes | Yes | Yes | No  |
| BACKUP | Incremental backup with CBT | Yes | Yes | Yes | Yes |
| Full backup with CBT | Yes | Yes | N/A | N/A |
| Full backup without CBT | Yes | Yes | Yes | Yes |
| Backup, archive and replicate now | Yes | Yes | Yes | Yes |
| CloudSpin to AWS and Azure | Supported for Azure | Supported for Azure | No  | N/A |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes | Yes |
| Recover an entire backup job run | Yes | Yes | Yes | Yes |
| Instant volume mount | Yes | Yes | No  | No  |
| Instant Recovery | Yes | Yes | Yes | No  |
| Crash-consistent recover for apps | Yes | Yes | No  | No  |
| CLONE | Clone an object of a job run | Yes | Yes | No  | No  |
| Clone all the objects of a job run | Yes | Yes | No  | No  |
| Crash-consistent recover for apps | Yes | Yes | No  | No  |
| TEARDOWN | Tear down objects | Yes | Yes | No  | No  |
| Tear down instant volume mount | Yes | Yes | No  | No  |
| CANCEL | Cancel a running job run | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes | Yes |
| FILE RECOVERY<br><br>{{site.data.keyword.baas_full_notm}} does not support Windows Deduplication Enabled volumes for AHV file recovery. | Search for files or folders | Yes | Yes | Yes | Yes |
| Browse for a backed-up object | Yes | Yes | Yes | Yes |
| Download files/folders | Yes | Yes | Yes | Yes |
| Recover files | Yes | Yes | Yes | No  |
| VIRTUAL DISKS RECOVERY | Recover Virtual Disks | No  | No  | No  | No  |
| CONVERT AND DEPLOY | Register Azure source and clone VMs to Azure | No  | No  | No  | No  |
| Register AWS source and clone VMs to AWS | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Archival workflows for non-vmware-based vms
{: #archival_workflows_for_non-vmware-based_vms}


| Categories | Workflows | Sources |     |     |     |
| --- | --- | --- | --- | --- | --- |
| HyperV 2016 / 2019 / 2022 VM Servers | HyperV 2012R2 VM Servers | AHV VM Servers | RHV (KVM) |
| --- | --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes | Yes | Yes |
| Archive a full backup | Yes | Yes | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes | Yes | Yes | Yes |
| Recover an entire backup job run from an archive to a source | Yes | Yes | Yes | Yes |
| Instant volume mount from an archive | Yes | Yes | No  | No  |
| Crash-consistent recover for apps from an archive | Yes | Yes | No  | No  |
| CLONE | Clone objects of a job run from an archive | Yes | Yes | No  | No  |
| Clone all objects of a job run from an archive | Yes | Yes | No  | No  |
| Crash-consistent clone for apps from an archive | Yes | Yes | No  | No  |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | Yes | Yes | No  | No  |
| Browse for files or folders from an archive snapshot | No  | No  | No  | No  |
| Download files and folders from an archive | Yes | Yes | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | No  | No  | No  | No  |
| Download n VMX file from an archive | No  | No  | No  | N/A |
| VIRTUAL DISKS RECOVERY | Recover Virtual Disks | No  | No  | No  | No  |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Yes | No  | Yes | No  |
| Download metadata for all runs of a job | Yes | No  | Yes | No  |
| Download a snapshot (archived data) for a job run | Yes | No  | Yes | No  |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | No  | No  | No  | No  |
| Clone all objects of a job run from an archive | No  | No  | No  | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Yes | Yes | Yes | No  |
| Recover all objects of job run from an archive to a source | Yes | Yes | Yes | No  |
| Instant volume mount from an archive | No  | No  | No  | No  |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | Yes | Yes | Yes | No  |
| Search files or folders from the metadata of a retrieved job run | Yes | Yes | Yes | No  |
| Browse files or folders from a retrieved snapshot | Yes | Yes | Yes | No  |
| Browse files or folders from the metadata of a downloaded job run | Yes | Yes | Yes | No  |
| Download files and folders from an archive | Yes | Yes | Yes | No  |
| Recover files from an archive to a source (to the same and different sources) | No  | No  | No  | No  |
| Download a VMX file from an archive | N/A | N/A | N/A | No  |
| CLOUD RETRIEVE - VIRTUAL DISKS RECOVERY | Recover Virtual Disks | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Replication workflows for non-vmware-based vms
{: #replication_workflows_for_non-vmware-based_vms}


| Categories | Workflows | Sources |     |     |     |
| --- | --- | --- | --- | --- | --- |
| HyperV 2016 / 2019 / 2022 VM Servers | HyperV 2012R2 VM Servers | AHV VM Servers | RHV (KVM) |
| --- | --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes | Yes | Yes | Yes |
| Replicate a full backup with CBT to a remote cluster | Yes | Yes | N/A | N/A |
| Replicate a full backup without CBT to a remote cluster | Yes | Yes | Yes | Yes |
| FAILOVER | Failover an inactive Job on a remote cluster | No  | No  | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes | Yes | Yes | Yes |
| Recover an entire backup job run from a remote cluster | Yes | Yes | Yes | Yes |
| Instant volume mount of replicated data | Yes | Yes | No  | No  |
| Crash-consistent recover for apps from a remote cluster | No  | No  | No  | No  |
| CLONE | Clone an object of a job run from a remote cluster | Yes | Yes | No  | No  |
| Clone all objects of a job run from a remote cluster | Yes | Yes | No  | No  |
| Crash-consistent clone for apps from a remote cluster | No  | No  | No  | No  |
| TEARDOWN | Tear down object on a remote cluster | Yes | Yes | No  | No  |
| Tear down instant volume mount on a remote cluster | Yes | Yes | No  | No  |
| CANCEL | Cancel a running replication task | Yes | Yes | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | Yes | Yes | Yes | Yes |
| Browse for replicated object on a remote cluster | Yes | Yes | Yes | Yes |
| Download replicated files and folders from a remote cluster | Yes | Yes | Yes | Yes |
| Recover replicated files from a remote cluster | Yes | Yes | Yes | No  |
| VIRTUAL DISKS RECOVERY | Recover Virtual Disks | No  | No  | No  | No  |
| CONVERT AND DEPLOY | Register Azure source and clone VMs to Azure | Yes | Yes | No  | No  |
| Register AWS source and clone VMs to AWS | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

## Physical server workflows
{: #physical_server_workflows}

### Protection workflows for physical server (block based)
{: #protection_workflows_for_physical_server_block_based}


| Categories | Workflows | Sources |     |
| --- | --- | --- | --- |
| Physical Servers (Windows - Block Based) | Physical Servers (Linux - Block Based) |
| --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes |
| Refresh object hierarchy | Yes | Yes |
| Auto-protect new objects | Yes | Yes |
| BACKUP | Incremental backup with CBT | Yes | No  |
| Full backup with CBT | Yes | No  |
| Full backup without CBT | Yes | Yes |
| Backup, archive and replicate now | Yes | Yes |
| RECOVER | Recovering an entity in the Job Run | No  | No  |
| Recover an entire backup job run | No  | No  |
| Instant volume mount | Yes | Yes |
| Crash-consistent recover for apps | No  | No  |
| CLONE | Clone an object of a job run | No  | No  |
| Clone all the objects of a job run | No  | No  |
| Crash-consistent recover for apps | No  | No  |
| TEARDOWN | Tear down objects | No  | No  |
| Tear down instant volume mount | Yes | Yes |
| CANCEL | Cancel a running job run | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes |
| FILE RECOVERY | Search for files or folders | Yes | Yes |
| Browse for a backed-up object | Yes | Yes |
| Download files/folders | Yes | Yes |
| Recover files | Yes | Yes |
{: caption="" caption-side="bottom"}

### Archival workflows for physical server (block based)
{: #archival_workflows_for_physical_server_block_based}


| Categories | Workflows | Sources |     |
| --- | --- | --- | --- |
| Physical Servers (Windows - Block Based) | Physical Servers (Linux - Block Based) |
| --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes |
| Archive a full backup | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | No  | No  |
| Recover an entire backup job run from an archive to a source | No  | No  |
| Instant volume mount from an archive | Yes | Yes |
| Crash-consistent recover for apps from an archive | No  | No  |
| CLONE | Clone objects of a job run from an archive | No  | No  |
| Clone all objects of a job run from an archive | No  | No  |
| Crash-consistent clone for apps from an archive | No  | No  |
| CANCEL | Cancel a running archival task | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | No  | No  |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | Yes | Yes |
| Browse for files or folders from an archive snapshot | No  | No  |
| Download files and folders from an archive | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | Yes | Yes |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Yes | Yes |
| Download metadata for all runs of a job | Yes | Yes |
| Download a snapshot (archived data) for a job run | Yes | Yes |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | No  | No  |
| Clone all objects of a job run from an archive | No  | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | No  | No  |
| Recover all objects of job run from an archive to a source | No  | No  |
| Instant volume mount from an archive | Yes | Yes |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | Yes | Yes |
| Search files or folders from the metadata of a retrieved job run | Yes | Yes |
| Browse files or folders from a retrieved snapshot | Yes | Yes |
| Browse files or folders from the metadata of a downloaded job run | No  | No  |
| Download files and folders from an archive | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | Yes | Yes |
{: caption="" caption-side="bottom"}

### Replication workflows for physical server (block based)
{: #replication_workflows_for_physical_server_block_based}


| Categories | Workflows | Sources |     |
| --- | --- | --- | --- |
| Physical Servers (Windows - Block Based) | Physical Servers (Linux - Block Based) |
| --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes | Yes |
| Replicate a full backup with CBT to a remote cluster | Yes | Yes |
| Replicate a full backup without CBT to a remote cluster | Yes | Yes |
| FAILOVER | Failover an inactive Job on a remote cluster | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | No  | No  |
| Recover an entire backup job run from a remote cluster | No  | No  |
| Instant volume mount of replicated data | Yes | Yes |
| Crash-consistent recover for apps from a remote cluster | No  | No  |
| CLONE | Clone an object of a job run from a remote cluster | No  | No  |
| Clone all objects of a job run from a remote cluster | No  | No  |
| Crash-consistent clone for apps from a remote cluster | No  | No  |
| TEARDOWN | Tear down object on a remote cluster | No  | No  |
| Tear down instant volume mount on a remote cluster | Yes | Yes |
| CANCEL | Cancel a running replication task | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | Yes | Yes |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | Yes | Yes |
| Browse for replicated object on a remote cluster | Yes | Yes |
| Download replicated files and folders from a remote cluster | Yes | Yes |
| Recover replicated files from a remote cluster | Yes | Yes |
{: caption="" caption-side="bottom"}

### Protection workflows for physical server (file based)
{: #protection_workflows_for_physical_server_file_based}


| Categories | Workflows | Sources |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Physical Servers (Windows - File Based) | Physical Servers (Linux - File Based) | AIX | Solaris | HPUX |
| --- | --- | --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes | Yes | Yes |
| Refresh Application registration | N/A | N/A | No  | No  | No  |
| Auto-protect new objects | Yes | No  | N/A | N/A | N/A |
| BACKUP | Incremental backup with CBT | Yes | No  | No  | No  | No  |
| Full backup with CBT | Yes | No  | No  | No  | No  |
| Full backup without CBT | Yes | Yes | No  | No  | No  |
| Backup, archive and replicate now | Yes | Yes | Yes | Yes | Yes |
| RECOVER | Recovering an entity in the Job Run | No  | No  | No  | No  | No  |
| Recover an entire backup job run | No  | No  | No  | No  | No  |
| Instant volume mount | No  | No  | N/A | N/A | N/A |
| Crash-consistent recover for apps | No  | No  | N/A | N/A | N/A |
| CLONE | Clone an object of a job run | No  | No  | No  | No  | No  |
| Clone all the objects of a job run | No  | No  | No  | No  | No  |
| Crash-consistent recover for apps | No  | No  | No  | No  | No  |
| TEARDOWN | Tear down objects | No  | No  | N/A | N/A | N/A |
| Tear down instant volume mount | No  | No  | N/A | N/A | N/A |
| CANCEL | Cancel a running job run | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | No  | No  | No  |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders | Yes | Yes | Yes | Yes | Yes |
| Browse for a backed-up object | Yes | Yes | Yes | Yes | Yes |
| Download files/folders | Yes | Yes | Yes | Yes | Yes |
| Recover files | Yes | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Archival workflows for physical server (file based)
{: #archival_workflows_for_physical_server_file_based}


| Categories | Workflows | Sources |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Physical Servers (Windows - File Based) | Physical Servers (Linux - File Based) | AIX | Solaris | HPUX |
| --- | --- | --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes | Yes | Yes | Yes |
| Archive a full backup | Yes | Yes | Yes | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | No  | No  | No  | No  | No  |
| Recover an entire backup job run from an archive to a source | No  | No  | No  | No  | No  |
| Instant volume mount from an archive | No  | No  | No  | No  | No  |
| Crash-consistent recover for apps from an archive | No  | No  | No  | No  | No  |
| CLONE | Clone objects of a job run from an archive | No  | No  | No  | No  | No  |
| Clone all objects of a job run from an archive | No  | No  | No  | No  | No  |
| Crash-consistent clone for apps from an archive | No  | No  | No  | No  | No  |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | No  | No  | No  | No  | No  |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | Yes | Yes | Yes | Yes | Yes |
| Browse for files or folders from an archive snapshot | No  | No  | No  | No  | No  |
| Download files and folders from an archive | Yes | Yes | Yes | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | Yes | Yes | Yes | No  | No  |
| Download n VMX file from an archive | N/A | N/A | No  | No  | No  |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Yes | Yes | Yes | Yes | Yes |
| Download metadata for all runs of a job | Yes | Yes | Yes | Yes | Yes |
| Download a snapshot (archived data) for a job run | Yes | Yes | Yes | Yes | Yes |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | No  | No  | No  | No  | No  |
| Clone all objects of a job run from an archive | No  | No  | No  | No  | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | No  | No  | No  | No  | No  |
| Recover all objects of job run from an archive to a source | No  | No  | No  | No  | No  |
| Instant volume mount from an archive | No  | No  | No  | No  | No  |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | Yes | Yes | Yes | Yes | Yes |
| Search files or folders from the metadata of a retrieved job run | Yes | Yes | Yes | Yes | Yes |
| Browse files or folders from a retrieved snapshot | Yes | Yes | Yes | Yes | Yes |
| Browse files or folders from the metadata of a downloaded job run | No  | No  | No  | No  | No  |
| Download files and folders from an archive | Yes | Yes | Yes | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | Yes | Yes | No  | No  | No  |
| Download a VMX file from an archive | N/A | N/A | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Replication workflows for physical server (file based)
{: #replication_workflows_for_physical_server_file_based}


| Categories | Workflows | Sources |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Physical Servers (Windows - File Based) | Physical Servers (Linux - File Based) | AIX | Solaris | HPUX |
| --- | --- | --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes | Yes | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A |
| Replicate a full backup with CBT to a remote cluster | Yes | Yes | N/A | N/A | N/A |
| Replicate a full backup without CBT to a remote cluster | Yes | Yes | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A |
| FAILOVER | Failover an inactive Job on a remote cluster | No  | No  | No  | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | No  | No  | No  | No  | No  |
| Recover an entire backup job run from a remote cluster | No  | No  | No  | No  | No  |
| Recover a SQL DB from a remote cluster to another SQL server with the same SQL version | N/A | N/A | No  | No  | No  |
| Recover a SQL DB from a remote cluster to different SQL server with a different SQL version | N/A | N/A | No  | No  | No  |
| Instant volume mount of replicated data | No  | No  | No  | No  | No  |
| Crash-consistent recover for apps from a remote cluster | No  | No  | No  | No  | No  |
| CLONE | Clone an object of a job run from a remote cluster | No  | No  | No  | No  | No  |
| Clone all objects of a job run from a remote cluster | No  | No  | No  | No  | No  |
| Crash-consistent clone for apps from a remote cluster | No  | No  | No  | No  | No  |
| TEARDOWN | Tear down object on a remote cluster | No  | No  | No  | No  | No  |
| Tear down instant volume mount on a remote cluster | No  | No  | No  | No  | No  |
| CANCEL | Cancel a running replication task | Yes | Yes | Yes | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | Yes | Yes | No  | No  | No  |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | Yes | Yes | Yes | Yes | Yes |
| Browse for replicated object on a remote cluster | Yes | Yes | Yes | Yes | Yes |
| Download replicated files and folders from a remote cluster | Yes | Yes | Yes | Yes | Yes |
| Recover replicated files from a remote cluster | Yes | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

## Database workflows
{: #database_workflows}

### Protection workflows for ms sql
{: #protection_workflows_for_ms_sql}


| Categories | Workflows | Sources |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| MS SQL VM | MS SQL Volume Based | MS SQL FileStream (Volume Based) Database | MS SQL FCI | MS SQL AAG | MS SQL File Based | MS SQL Native |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Refresh Application registration | Yes using VM edit workflow | Yes | Yes | Yes | Yes using VM edit workflow | Yes | Yes |
| Auto-protect new objects | Yes | Yes | Yes | Yes | Yes | Yes but no exclusion | Yes but no exclusion |
| BACKUP | Incremental backup with CBT | Yes as log-based incremental | Yes as log-based incremental | Yes as log-based incremental | Yes | Yes | Yes | N/A |
| Full backup with CBT | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Incremental backup with SQL server differential | N/A | N/A | N/A | N/A | N/A | N/A | Yes |
| Full backup without CBT | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Ability to run a log backup in parallel to a full backup. | N/A | N/A | N/A | N/A | N/A | Yes | Yes |
| Backup, archive and replicate now | Supported at DB and server level both | Supported at DB and server level both | Supported at DB and server level both | Supported at DB and server level both | Supported at DB and server level both | Supported at DB and server level both | Supported at DB and server level both |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes for DBs but not full server | Yes for DBs but not full server | Yes for DB and VM | Yes for DB and VM | Yes at DB level | Yes at DB level |
| Recover an entire backup job run | Yes as crash-consistent VM not as SQL restore | No  | No  | No  | Yes as crash-consistent VM not as SQL restore | No  | No  |
| Recover a SQL DB to the original server and instance | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover a SQL DB to the original server but a different instance | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover a SQL DB to a different server that has the same SQL version | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Restore DB to Alternate Instance with overwite | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover a SQL DB to a different server that has a different SQL version (Based on MS requirements) | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Instant volume mount | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Crash-consistent recover for apps | Yes | Yes using IVM (physical) | Yes using IVM (physical) | Yes | Yes using IVM (physical) | N/A | N/A |
| CLONE | Clone an object of a job run | Yes | Yes | Yes | Cloning DB from FCI to standalone server / node Supported. cloning db to fci instance not supported | Yes | Yes | No  |
| Clone all the objects of a job run | Yes as crash-consistent VM, not as SQL clone | No  | No  | No  | Yes as crash-consistent VM, not as SQL clone | N/A | N/A |
| Clone db to FCI instance | N/A | N/A | N/A | N/A | N/A | Yes | N/A |
| Crash-consistent recover for apps | Yes | No  | No  | No  | Yes using IVM (physical) | N/A | N/A |
| DB Migration | SQL DB Migration without auto sync | N/A | N/A | N/A | N/A | N/A | Yes | N/A |
| SQL DB Migration with auto sync | N/A | N/A | N/A | N/A | N/A | Yes | N/A |
| TEARDOWN | Tear down objects | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Tear down instant volume mount | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| CANCEL | Cancel a running job run | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes | Yes | Yes | Yes at server level | Yes at server level |
| FILE RECOVERY | Search for files or folders | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Browse for a backed-up object | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Download files/folders | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Recover files | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Download a VMX file | Yes | N/A | N/A | N/A | Yes for VM | N/A | N/A |
{: caption="" caption-side="bottom"}

### Archival workflows for ms sql
{: #archival_workflows_for_ms_sql}


| Categories | Workflows | Sources |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| MS SQL VM | MS SQL Volume Based | MS SQL FileStream (Volume Based) Database | MS SQL FCI | MS SQL AAG | MS SQL File Based | MS SQL Native (VDI) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes except for log backup | Yes except for log backup | Yes except for log backup | Yes except for log backup | Yes except for log backup | Yes | Yes |
| Archiving an incremental backup with SQL server differential | N/A | N/A | N/A | N/A | N/A | N/A | Yes |
| Archive a full backup | Yes except for log backup | Yes except for log backup | Yes except for log backup | Yes except for log backup | Yes except for log backup | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes for files & VMs but not DBs | Yes for files but not DBs | Yes for files but not DBs | Yes for files but not DBs | Yes for files & VMs but not DBs | Yes at DB level | Yes at DB level |
| Recover an entire backup job run from an archive to a source | No  | No  | No  | No  | No  | No  | No  |
| Instant volume mount from an archive | No for SQL (use VM or physical) | No for SQL (use VM or physical) | No for SQL (use VM or physical) | No for SQL (use VM or physical) | No for SQL (use VM or physical) | N/A | N/A |
| Crash-consistent recover for apps from an archive | Yes for files & VMs but not DBs | Yes for files but not DBs | Yes for files but not DBs | Yes for files but not DBs | Yes for files & VMs but not DBs | N/A | N/A |
| CLONE | Clone objects of a job run from an archive | No  | No  | No  | No  | No  | Yes | No  |
| Clone all objects of a job run from an archive | No  | No  | No  | No  | No  | No  | No  |
| Clone db to FCI | N/A | N/A | N/A | N/A | N/A | No  | N/A |
| Crash-consistent clone for apps from an archive | No  | No  | No  | No  | No  | N/A | N/A |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | No  | No  | No  | No  | No  | Yes | Yes |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Browse for files or folders from an archive snapshot | No  | No  | No  | No  | No  | N/A | N/A |
| Download files and folders from an archive | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Recover files from an archive to a source (to the same and different sources) | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | No  | No  |
| Download metadata for all runs of a job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | No  | No  |
| Download a snapshot (archived data) for a job run | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | No  | No  |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | No  | No  |
| Clone all objects of a job run from an archive | No  | No  | No  | No  | No  | No  | No  |
| Clone DB to FCI | N/A | N/A | N/A | N/A | N/A | Yes | N/A |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | No  | No  |
| Recover all objects of job run from an archive to a source | No  | No  | No  | No  | No  | No  | No  |
| Instant volume mount from an archive | Not Supported for SQL (use VM or physical, volume based) | Not Supported for SQL (use VM or physical, volume based) | Not Supported for SQL (use VM or physical, volume based) | Not Supported for SQL (use VM or physical, volume based) | Not Supported for SQL (use VM or physical, volume based) | No  | No  |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | N/A | N/A |
| Search files or folders from the metadata of a retrieved job run | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | N/A | N/A |
| Browse files or folders from a retrieved snapshot | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | N/A | N/A |
| Browse files or folders from the metadata of a downloaded job run | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | N/A | N/A |
| Download files and folders from an archive | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | N/A | N/A |
| Recover files from an archive to a source (to the same and different sources) | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | Supported for volume based job | N/A | N/A |
{: caption="" caption-side="bottom"}

### Replication workflows for ms sql
{: #replication_workflows_for_ms_sql}


| Categories | Workflows | Sources |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| MS SQL VM | MS SQL Volume Based | MS SQL FileStream (Volume Based) Database | MS SQL FCI | MS SQL AAG | MS SQL File Based | MS SQL Native |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes as log-based incremental | Yes as log-based incremental | Yes as log-based incremental | Yes as log-based incremental | Yes as log-based incremental | Yes | N/A |
| Replicate an incremental backup with SQL server differential to a remote cluster | N/A | N/A | N/A | N/A | N/A | N/A | Yes |
| Replicate a full backup with CBT to a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | N/A |
| Replicate a full backup without CBT to a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| FAILOVER | Failover an inactive Job on a remote cluster | No  | No  | No  | No  | No  | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes | Yes only for DBs & not full server for physical | Yes only for DBs & not full server for physical | Yes only for DBs & not full server for physical | Yes only for DBs & not full server for physical | Yes | Yes |
| Recover an entire backup job run from a remote cluster | Yes as crash-consistent, not as SQL restore | No  | No  | No  | No  | No  | No  |
| Recover a SQL DB from a remote cluster to another SQL server with the same SQL version | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover a SQL DB from a remote cluster to different SQL server with a different SQL version | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Instant volume mount of replicated data | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Crash-consistent recover for apps from a remote cluster | Yes | No  | No  | Yes | Yes | No  | No  |
| CLONE | Clone an object of a job run from a remote cluster | Yes | Yes | Yes | Yes | Yes for DB and VM | Yes for DB | No  |
| Clone all objects of a job run from a remote cluster | Yes as crash-consistent VM, not as SQL clone | No  | No  | No  | No  | No  | No  |
| Clone db to FCI | N/A | N/A | N/A | N/A | N/A | Yes | N/A |
| Crash-consistent clone for apps from a remote cluster | Yes | No  | No  | No  | Yes for DB and VM | No  | No  |
| TEARDOWN | Tear down object on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes for DB | Yes for DB |
| Tear down instant volume mount on a remote cluster | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| CANCEL | Cancel a running replication task | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Browse for replicated object on a remote cluster | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Download replicated files and folders from a remote cluster | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Recover replicated files from a remote cluster | Yes | Yes | Yes | Yes | Yes | N/A | N/A |
| Download a VMX file from a remote cluster | Yes | N/A | N/A | N/A | Supported but only VM | N/A | N/A |
{: caption="" caption-side="bottom"}

### Protection workflows for oracle
{: #protection_workflows_for_oracle}


| Categories | Workflows | Sources |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Oracle Standalone | Oracle RAC | Oracle on Windows | Oracle on AIX | Oracle on Solaris |
| --- | --- | --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes | Yes | Yes |
| Refresh Application registration | Yes | Yes | Yes | Yes | Yes |
| Auto-protect new objects | No  | No  | No  | No  | No  |
| BACKUP | Incremental backup with CBT | Yes | Yes | Yes | Yes | Yes |
| Full backup with CBT | Yes | Yes | Yes | Yes | Yes |
| Full backup without CBT | Yes | Yes | Yes | Yes | Yes |
| Ability to run a log backup in parallel to a full backup. | Yes | Yes | Yes | Yes | Yes |
| Backup, archive and replicate now | Yes | Yes | Yes | Yes | Yes |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes | Yes | Yes |
| Recover an entire backup job run | No  | No  | No  | No  | No  |
| Recover a DB to the original server and instance | Yes | Yes | Yes | Yes | Yes |
| Recover a DB to the original server but a different instance | Yes | Yes | Yes | Yes | No  |
| Recover a DB to a different server that has the same version | Yes | Yes | Yes | Yes | No  |
| Recover DB to Alternate Instance | N/A | N/A | N/A | N/A | No  |
| Instant volume mount | Yes | Yes | Yes | Yes | Yes |
| Instant Recovery | Yes | Yes | N/A | No  | No  |
| CLONE | Clone an object of a job run | Yes | Yes | N/A | Yes | No  |
| Clone all the objects of a job run | No  | No  | No  | No  | No  |
| TEARDOWN | Tear down objects | Yes | Yes | No  | Yes | No  |
| CANCEL | Cancel a running job run | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | Yes | Yes | No  |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Archival workflows for oracle
{: #archival_workflows_for_oracle}


| Categories | Workflows | Sources |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Oracle Standalone | Oracle RAC | Oracle on Windows | Oracle on AIX | Oracle on Solaris |
| --- | --- | --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes | Yes | Yes | Yes |
| Archive a full backup | Yes | Yes | Yes | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes | Yes | Yes | Yes | Yes |
| Recover an entire backup job run from an archive to a source | No  | No  | No  | No  | No  |
| CLONE | Clone objects of a job run from an archive | Yes | Yes | No  | Yes | No  |
| Clone all objects of a job run from an archive | No  | No  | No  | No  | No  |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | Yes | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes | Yes | Yes |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | No  | No  | No  | No  | No  |
| Download metadata for all runs of a job | Yes | Yes | Yes | Yes | Yes |
| Download a snapshot (archived data) for a job run | Yes | Yes | Yes | Yes | Yes |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | Yes | Yes | No  | Yes | No  |
| Clone all objects of a job run from an archive | No  | No  | No  | No  | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Yes | Yes | Yes | Yes | Yes |
| Recover all objects of job run from an archive to a source | No  | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Replication workflows for oracle
{: #replication_workflows_for_oracle}


| Categories | Workflows | Sources |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Oracle Standalone | Oracle RAC | Oracle on Windows | Oracle on AIX | Oracle on Solaris |
| --- | --- | --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes | Yes | Yes | Yes | Yes |
| Replicate a full backup with CBT to a remote cluster | Yes | Yes | Yes | Yes | Yes |
| Replicate a full backup without CBT to a remote cluster | Yes | Yes | Yes | Yes | Yes |
| FAILOVER | Failover an inactive Job on a remote cluster | No  | No  | No  | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes | Yes | Yes | Yes | Yes |
| Recover an entire backup job run from a remote cluster | No  | No  | No  | No  | No  |
| CLONE | Clone an object of a job run from a remote cluster | Yes | Yes | N/A | Yes | No  |
| Clone all objects of a job run from a remote cluster | No  | No  | N/A | No  | No  |
| TEARDOWN | Tear down object on a remote cluster | Yes | Yes | N/A | N/A | N/A |
| CANCEL | Cancel a running replication task | Yes | Yes | Yes | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | Yes | Yes | No  | No  | No  |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | N/A | N/A | N/A | N/A | N/A |
| Browse for replicated object on a remote cluster | N/A | N/A | N/A | N/A | N/A |
| Download replicated files and folders from a remote cluster | N/A | N/A | N/A | N/A | N/A |
| Recover replicated files from a remote cluster | N/A | N/A | N/A | N/A | N/A |
{: caption="" caption-side="bottom"}

### Protection workflows for cassandra, mongodb, couchbase, and hadoop
{: #protection_workflows_for_cassandra_mongodb_couchbase_and_hadoop}


| Categories | Workflows | Sources |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Cassandra | MongoDB | Couchbase-UDA Connector | Hadoop-HDFS | Hadoop-Hive | Hadoop-HBase |
| --- | --- | --- | --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes | Yes | Yes | Yes |
| Refresh Application registration | Yes | Yes | Yes | Yes | Yes | Yes |
| Auto-protect new objects | Yes | Yes | Yes | Yes | Yes | Yes |
|     | Full backup without CBT | Yes | Yes | Yes | Yes | Yes | Yes |
|     | Backup, archive and replicate now | Yes | Yes | Yes | Yes | Yes | Yes |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover an entire backup job run | Yes | Yes | Yes | Yes | Yes | Yes |
| CANCEL | Cancel a running job run | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders | N/A | N/A | N/A | Yes | N/A | N/A |
| Browse for a backed-up object | N/A | N/A | N/A | No  | N/A | N/A |
| Download files/folders | N/A | N/A | N/A | No  | N/A | N/A |
| Recover files | N/A | N/A | N/A | Yes | N/A | N/A |
{: caption="" caption-side="bottom"}

### Archival workflows for cassandra, mongodb, couchbase, and hadoop
{: #archival_workflows_for_cassandra_mongodb_couchbase_and_hadoop}


| Categories | Workflows | Sources |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Cassandra | MongoDB | Couchbase-UDA Connector | Hadoop-HDFS | Hadoop-Hive | Hadoop-HBase |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes | Yes | Yes | Yes | Yes |
| Archive a full backup | Yes | Yes | Yes | Yes | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover an entire backup job run from an archive to a source | Yes | Yes | Yes | Yes | Yes | Yes |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | N/A | N/A | N/A | Yes | N/A | N/A |
| Browse for files or folders from an archive snapshot | N/A | N/A | N/A | No  | N/A | N/A |
| Download files and folders from an archive | N/A | N/A | N/A | No  | N/A | N/A |
| Recover files from an archive to a source (to the same and different sources) | N/A | N/A | N/A | Yes | N/A | N/A |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | No  | No  | No  | No  | No  | No  |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | No  | No  | No  | No  | No  | No  |
| Clone all objects of a job run from an archive | No  | No  | No  | No  | No  | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover all objects of job run from an archive to a source | Yes | Yes | Yes | Yes | Yes | Yes |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | N/A | N/A | N/A | No  | N/A | N/A |
| Search files or folders from the metadata of a retrieved job run | N/A | N/A | N/A | No  | N/A | N/A |
| Browse files or folders from a retrieved snapshot | N/A | N/A | N/A | No  | N/A | N/A |
| Browse files or folders from the metadata of a downloaded job run | N/A | N/A | N/A | No  | N/A | N/A |
| Download files and folders from an archive | N/A | N/A | N/A | No  | N/A | N/A |
| Recover files from an archive to a source (to the same and different sources) | N/A | N/A | N/A | No  | N/A | N/A |
{: caption="" caption-side="bottom"}

### Replication workflows for cassandra, mongodb, couchbase, and hadoop
{: #replication_workflows_for_cassandra_mongodb_couchbase_and_hadoop}


| Categories | Workflows | Sources |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Cassandra | MongoDB | Couchbase-UDA Connector | Hadoop-HDFS | Hadoop-Hive | Hadoop-HBase |
| --- | --- | --- | --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A |
| Replicate a full backup without CBT to a remote cluster | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A |
| FAILOVER | Failover an inactive Job on a remote cluster | No  | No  | No  | No  | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover an entire backup job run from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes |
| Crash-consistent recover for apps from a remote cluster | No  | No  | No  | No  | No  | No  |
| TEARDOWN | Tear down object on a remote cluster | No  | No  | No  | No  | No  | No  |
| CANCEL | Cancel a running replication task | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | Supported(Clone not supported) | Supported(Clone not supported) | Supported(Clone not supported) | Supported(Clone not supported) | Supported(Clone not supported) | Supported(Clone not supported) |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | N/A | N/A | N/A | Yes | N/A | N/A |
| Browse for replicated object on a remote cluster | N/A | N/A | N/A | No  | N/A | N/A |
| Download replicated files and folders from a remote cluster | N/A | N/A | N/A | No  | N/A | N/A |
| Recover replicated files from a remote cluster | N/A | N/A | N/A | Yes | N/A | N/A |
{: caption="" caption-side="bottom"}

## Uda workflows
{: #uda_workflows}

### Protection workflows for uda
{: #protection_workflows_for_uda}


| Categories | Workflows | Sources |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Universal Data Adapter | UDA: CockroachDB on Linux | UDA: DB2 on Linux | UDA: SAP IQ on Linux | UDA: SAP ASE on Linux | UDA: SAP ASE on Windows | UDA: SAP on Oracle | UDA: PostgreSQL |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | No  | No  | No  | No  | No  | No  | N/A | No  |
| Refresh Application registration | No  | No  | No  | No  | No  | No  | N/A | No  |
| Auto-protect new objects | No  | No  | No  | No  | No  | No  | N/A | No  |
| BACKUP | Full backup without CBT | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Ability to run a log backup in parallel to a full backup. | Yes | Yes | Yes | Yes | Yes | Yes | N/A | Yes |
| Backup, archive and replicate now | Yes | Yes | Yes | Yes | Yes | Yes | No  | Yes |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes | Yes | Yes | Yes | N/A | Yes |
| Recover an entire backup job run | No  | No  | No  | No  | No  | No  | Yes | No  |
| Recover DB to Alternate Instance with overwite | Yes | Yes | Yes | Yes | Yes | Yes | N/A | Yes |
| CANCEL | Cancel a running job run | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning | Yes except for cloning |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Archival workflows for uda
{: #archival_workflows_for_uda}


| Categories | Workflows | Sources |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Universal Data Adapter | UDA: CockroachDB on Linux | UDA: DB2 on Linux | UDA: SAP IQ on Linux | UDA: SAP ASE on Linux | UDA: SAP ASE on Windows | UDA: SAP on Oracle | UDA: PostgreSQL |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | No  | No  | No  | No  | No  | N/A | No  |
| Archive a full backup | Yes | No  | No  | No  | No  | No  | N/A | No  |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes | No  | No  | No  | No  | No  | N/A | No  |
| Recover an entire backup job run from an archive to a source | Yes | No  | No  | No  | No  | No  | N/A | No  |
| CANCEL | Cancel a running archival task | Yes | No  | No  | No  | No  | No  | N/A | No  |
| Cancel a running clone or recover task that is retrieving from an archive | Yes except for cloning | No  | No  | No  | No  | No  | N/A | No  |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | No  | No  | No  | No  | No  | N/A | No  |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | No  | No  | No  | No  | No  | No  | N/A | No  |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | No  | No  | No  | No  | No  | No  | N/A | No  |
| Clone all objects of a job run from an archive | No  | No  | No  | No  | No  | No  | N/A | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Yes | No  | No  | No  | No  | No  | N/A | No  |
| Recover all objects of job run from an archive to a source | Yes | No  | No  | No  | No  | No  | N/A | No  |
{: caption="" caption-side="bottom"}

### Replication workflows for uda
{: #replication_workflows_for_uda}


| Categories | Workflows | Sources |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Universal Data Adapter | UDA: CockroachDB on Linux | UDA: DB2 on Linux | UDA: SAP IQ on Linux | UDA: SAP ASE on Linux | UDA: SAP ASE on Windows | UDA: SAP on Oracle | UDA: PostgreSQL |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes but CBT is N/A | No  | No  | No  | No  | No  | No  | No  |
| Replicate a full backup with CBT to a remote cluster | N/A | N/A | N/A | N/A | N/A | N/A | No  | N/A |
| Replicate a full backup without CBT to a remote cluster | Yes but CBT is N/A | No  | No  | No  | No  | No  | No  | No  |
| FAILOVER | Failover an inactive Job on a remote cluster | No  | No  | No  | No  | No  | No  | N/A | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes | No  | No  | No  | No  | No  | N/A | No  |
| Recover an entire backup job run from a remote cluster | No  | No  | No  | No  | No  | No  | N/A | No  |
| Crash-consistent recover for apps from a remote cluster | No  | No  | No  | No  | No  | No  | N/A | No  |
| TEARDOWN | Tear down object on a remote cluster | No  | No  | No  | No  | No  | No  | N/A | No  |
| CANCEL | Cancel a running replication task | Yes | No  | No  | No  | No  | No  | No  | No  |
| Cancel a clone or recover task on a remote cluster | Supported(Clone not supported) | No  | No  | No  | No  | No  | No  | No  |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | No  | No  | No  | No  | No  | No  | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | N/A | N/A | N/A | N/A | N/A | N/A | No  | N/A |
| Browse for replicated object on a remote cluster | N/A | N/A | N/A | N/A | N/A | N/A | No  | N/A |
| Download replicated files and folders from a remote cluster | N/A | N/A | N/A | N/A | N/A | N/A | No  | N/A |
| Recover replicated files from a remote cluster | N/A | N/A | N/A | N/A | N/A | N/A | No  | N/A |
{: caption="" caption-side="bottom"}

## Application workflows
{: #application_workflows}

### Registration workflows for microsoft 365 applications
{: #registration_workflows_for_microsoft_365_applications}


| Workflows | Sources |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Microsoft 365 Mailbox | Microsoft 365 OneDrive | Microsoft 365 SharePoint | Microsoft 365 PublicFolders | Microsoft 365 Groups | Microsoft 365 Teams |
| --- | --- | --- | --- | --- | --- | --- |
| Register a source and auto-protect new objects | Yes | Yes | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes | Yes | Yes | Yes |
| Register Commercial Tenant | Yes | Yes | Yes | Yes | Yes | Yes |
| Register GCC Tenant | Yes | Yes | Yes | Yes | Yes | Yes |
| Register GCCHigh Tenant | Yes | Yes | Yes | No  | Yes | Yes |
| Auto/express registration | Yes | Yes | Yes | Yes | Yes | Yes |
| Multiple Service Accounts | Yes | Yes | Yes | Yes | Yes | Yes |
| Browse top/first level source entries | No  | Yes | Yes | Yes | No  | No  |
| Browse the source hierarchy | No  | No  | No  | No  | No  | No  |
| Auto-protect source hierarchy | Yes | Yes | Yes | Yes | Yes | Yes |
| Exclude individual sources from auto-protect | Yes | Yes | Yes | Yes | Yes | Yes |
| Select individual sources for protection | Yes | Yes | Yes | Yes | Yes | Yes |
| Filter Source View based on selective Azure AD user properties | Yes | Yes | No  | No  | No  | No  |
| Auto Protect source based on Azure AD properties | No  | No  | No  | No  | No  | No  |
| Protect sources based on Azure AD security group membership | No  | No  | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Backup and recovery workflows for microsoft 365 mailboxes
{: #backup_and_recovery_workflows_for_microsoft_365_mailboxes}


| Categories | Workflows | Supported |
| --- | --- | --- |
| Backup | Incremental Indexing | Yes |
| Group Mailboxes | Yes |
| Backup User Mailboxes (licensed) | In-place Archive | Yes |
| Recoverable Items | No  |
| Include / exclude standard folders | Yes |
| Exclude custom folder paths | Yes |
| Backup Shared Mailboxes / Room / Equipment Mailboxes | Include / exclude standard folders | Yes |
| Exclude custom folder paths | Yes |
| Calendar / Contacts / Notes / Tasks | No  |
| Search / Discover Backup Image | Browse Mailbox | No  |
| Search eMails | Yes |
| Search Calendar / Contacts / Notes / Tasks | No  |
| Recover | Recovery to on-premise Exchange Setup | No  |
| Recover PST | Download mailbox as PST | Yes |
| Download mailbox content as PST | Yes |
| Recover Mailbox | Restore Mailbox to the original location | Yes |
| Restore Mailbox to an alternate mailbox on the same Microsoft 365 source | Yes |
| Restore Mailbox to an alternate mailbox on a different Microsoft 365 source | Yes |
| Recover Mailbox Content | Restore eMails to the original location | Yes |
| Restore eMails to an alternate mailbox on the same Microsoft 365 source | Yes |
| Restore eMails to an alternate mailbox on a different Microsoft 365 source | Yes |
| Restore calendar / contacts / notes / tasks to the original location | No  |
| Restore calendar / contacts / notes / tasks to an alternate mailbox on the same Microsoft 365 source | No  |
| Restore calendar / contacts / notes / tasks to an alternate mailbox on a different Microsoft 365 source | No  |
{: caption="" caption-side="bottom"}

### Backup and recovery workflows for microsoft 365 public folders
{: #backup_and_recovery_workflows_for_microsoft_365_public_folders}


| Categories | Workflows | Supported |
| --- | --- | --- |
| Backup | Incremental Indexing | Yes |
| Include / exclude standard folders | No  |
| Exclude custom folder paths | Yes |
| Calendar / Contacts / Notes / Tasks | Yes |
| Search / Discover Backup Image | Browse Public Folders | No  |
| Search eMails in a Public Folder Hierarchy | Yes |
| Search Calendar / Contacts / Notes / Tasks | Yes |
| Recover PST | Download Root Public Folder as PST | No  |
| Download Public Folder content as PST | No  |
| Recover Mailbox | Restore Root Public Folder to the original location | Yes |
| Restore Root Public Folder to an alternate location on the same Microsoft 365 source | Yes |
| Restore Root Public Folder to an alternate location on a different Microsoft 365 source | Yes |
| Recover Mailbox Content | Restore eMails to the original root Public Folder | Yes |
| Restore eMails to an alternate Root Public Folder on the same Microsoft 365 source | Yes |
| Restore eMails to an alternate Root Public Folder on a different Microsoft 365 source | Yes |
| Restore calendar / contacts / notes / tasks to the original location | Yes |
| Restore calendar / contacts / notes / tasks to an alternate Root Public Folder on the same Microsoft 365 source | Yes |
| Restore calendar / contacts / notes / tasks to an alternate Root Public Folder on a different Microsoft 365 source | Yes |
{: caption="" caption-side="bottom"}

### Backup and recovery workflows for microsoft 365 onedrive
{: #backup_and_recovery_workflows_for_microsoft_365_onedrive}


| Categories | Workflows | Supported |
| --- | --- | --- |
| Backup | User OneDrive (licensed) | Yes |
| Recycle Bin | No  |
| Exclude custom folder paths | Yes |
| Access Permissions | No  |
| Sharing Permissions | Yes |
| Incremental Indexing | Yes |
| All versions of the file since last successful backup | No  |
| Document library | No  |
| OneNote | No  |
| Preservation Hold Library | No  |
| Search / Discover Backup Image | Browse OneDrive | Yes |
| Search files / folders | Yes |
| Download | Download single file | Yes |
| Download folder | Download all OneDrive content | No  |
| Recover OneDrive | Restore OneDrive to the original location | Yes |
| Restore OneDrive to an alternate mailbox on the same Microsoft 365 source | Yes |
| Restore OneDrive to an alternate mailbox on a different Microsoft 365 source | Yes |
| Recover OneDrive Content | Restore files / folders to the original location | Yes |
| Restore files / folders to an alternate OneDrive on the same Microsoft 365 source | Yes |
| Restore files / folders to an alternate OneDrive on a different Microsoft 365 source | Yes |
{: caption="" caption-side="bottom"}

### Backup and recovery workflows for microsoft 365 sharepoint online sites
{: #backup_and_recovery_workflows_for_microsoft_365_sharepoint_online_sites}


| Categories | Workflows | Supported |
| --- | --- | --- |
| Backup | Hub Site / Site / Site Collection / Subsite / Team Site | Yes |
| Incremental Indexing | No  |
| Recycle Bin | No  |
| Exclude custom folder paths | Yes |
| Access Permissions | Yes |
| Sharing Permissions | Yes |
| All versions of the file since last successful backup | No  |
| Site Assets | Yes |
| Site Columns | Yes |
| Site Customization | Yes |
| Site Hierarchy | Yes |
| Site Membership | Yes |
| Site Permissions | Yes |
| Site Template | Yes |
| Lists / List Items | No  |
| Workflows | No  |
| Application Data / Wiki Data | Yes |
| Application Tabs | No  |
| Backup Document Library Items | Doc Lib / Files / Folders | Yes |
| Columns / Content / Content Type / Permissions / Views | Yes |
| Document Library Permissions (Inherited & Unique) | Yes |
| Access Permissions, Columns, Content, Content Type, Shared Permission, Views | Yes |
| Search / Discover Backup Image | Browse Site | Yes |
| Browse Site Hierarchy | No  |
| Search files / folders | Yes |
| Search Sites | Yes |
| Recover | Recovery to on-premise Setup | No  |
| Site Permissions | No  |
| Download | Download single file | Yes |
| Download folder | Yes |
| Download Site | No  |
| Hub Site / Site / Team Site | Restore Site to the original location | Yes |
| Restore Site to an alternate site on the same Microsoft 365 source (no customization of name) | Yes |
| Restore Site to an alternate Site on a different Microsoft 365 source (no customization of name) | Yes |
| Site Content | Restore doclibs / files / folders to the original location | Yes |
| Restore doclibs / files / folders to an alternate Site on the same Microsoft 365 source (without permissions) | Yes |
| Restore doclibs / files / folders to an alternate Site on a different Microsoft 365 source (without permissions) | Yes |
{: caption="" caption-side="bottom"}

### Backup and recovery workflows for microsoft 365 teams
{: #backup_and_recovery_workflows_for_microsoft_365_teams}


| Categories | Workflows | Supported |
| --- | --- | --- |
| Backup | Channel Files | Yes |
| Access Permissions | Yes |
| Sharing Permissions | Yes |
| All versions of the file since last successful backup | No  |
| Archived Posts / Conversation | No  |
| Non-archived Posts / Conversation | No  |
| Index Channel Files | No  |
| Index archived Posts / Conversations | No  |
| Index Non-archived Posts / Conversations | No  |
| Application Data / Wiki Data | Yes |
| Application Tabs | No  |
| Backup Private Channel | Channel Files | Yes |
| Access Permissions | Yes |
| Sharing Permissions | Yes |
| All versions of the file since last successful backup | No  |
| Index Channel Files | No  |
| Archived Posts / Conversation | No  |
| Index archived Posts / Conversations | No  |
| Non-archived Posts / Conversation | No  |
| Index Non-archived Posts / Conversations | No  |
| Application Data / Wiki Data | Yes |
| Application Tabs | No  |
| Backup Shared Channel | Channel Files | No  |
| Access Permissions | No  |
| Sharing Permissions | No  |
| All versions of the file since last successful backup | No  |
| Index Channel Files | No  |
| Archived Posts / Conversation | No  |
| Index archived Posts / Conversations | No  |
| Non-archived Posts / Conversation | No  |
| Index Non-archived Posts / Conversations | No  |
| Application Data / Wiki Data | Yes |
| Application Tabs | No  |
| Search / Discover Backup Image | Browse Teams | No  |
| Browse Teams Hierarchy | No  |
| Search Teams | Yes |
| Search Public Channel / Private Channels | No  |
| Search files / folder in a channel | Yes |
| Search 1:1 / 1:n chats | No  |
| Recover Teams | Download | No  |
| Restore Teams to original location | Yes |
| Restore Teams to an alternate location on the same Microsoft 365 source | Yes |
| Restore Teams to an alternate location on different Microsoft 365 source | Yes |
| Recover Public Channel | Download channel | No  |
| Download file / folder | No  |
| Download Archived Post / Conversation | No  |
| Download non-archived Post / Conversation | No  |
| Restore Channel to original location | No  |
| Restore channel to an alternate location on the same Microsoft 365 source | No  |
| Restore channel to an alternate location on a different Microsoft 365 source | No  |
| Channel Content | No  |
| Restore files / folders to the original channel | Yes |
| Restore files / folders to an alternate channel | Yes |
| Restore Archived Post / Conversation to original channel | No  |
| Restore Archived Post / Conversation to alternate channel | No  |
| Restore non-Archived Post / Conversation to original channel | No  |
| Restore non-Archived Post / Conversation to an alternate channel | No  |
| Recover Private Channel | Download channel | No  |
| Download file/folder | No  |
| Download Archived Post / Conversation | No  |
| Download non-archived Post / Conversation | No  |
| Restore Channel to original location | No  |
| Restore channel to an alternate location on the same Microsoft 365 source | No  |
| Restore channel to an alternate location on a different Microsoft 365 source | No  |
| Channel Content | No  |
| Restore files / folders to the original channel | Yes |
| Restore files / folders to an alternate channel | Yes |
| Restore Archived Post / Conversation to original channel | No  |
| Restore Archived Post / Conversation to alternate channel | No  |
| Restore non-Archived Post / Conversation to original channel | No  |
| Restore non-Archived Post / Conversation to an alternate channel | No  |
{: caption="" caption-side="bottom"}

### Backup and recovery workflows for microsoft 365 groups
{: #backup_and_recovery_workflows_for_microsoft_365_groups}


| Categories | Workflows | Supported |
| --- | --- | --- |
| Backup | Group Mailbox | Yes |
| Group Site | Yes |
| Search / Discover Backup Image | Microsoft Group | Yes |
| Recover | Group Mailbox | Yes |
| Group Site | Yes |
{: caption="" caption-side="bottom"}

### Cloud archive workflows for microsoft 365 applications
{: #cloud_archive_workflows_for_microsoft_365_applications}


| Workflows | Sources |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Microsoft 365 Mailbox | Microsoft 365 OneDrive | Microsoft 365 SharePoint | Microsoft 365 PublicFolders | Microsoft 365 Groups | Microsoft 365 Teams |
| --- | --- | --- | --- | --- | --- | --- |
| Cloud Archive | Yes | Yes | Yes | Yes | No  | No  |
{: caption="" caption-side="bottom"}

### Protection workflows for on-prem applications
{: #protection_workflows_for_on-prem_applications}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| SAP HANA | Microsoft Exchange OnPrem | Microsoft Active Directory |
| --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | N/A | Yes | Yes |
| Refresh object hierarchy | N/A | Yes | Yes |
| Refresh Application registration | N/A | Yes | Yes |
| Auto-protect new objects | N/A | Yes | N/A |
| BACKUP | Incremental backup with CBT | N/A | Yes | Yes |
| Full backup with CBT | N/A | Yes | Yes |
| Full backup without CBT | Yes | Yes | Yes |
| Backup, archive and replicate now | Yes | Yes | Yes |
| RECOVER | Recovering an entity in the Job Run | N/A | Yes | No  |
| Recover an entire backup job run | Yes | No  | Yes |
| Instant volume mount | N/A | No  | Yes |
| Crash-consistent recover for apps | N/A | No  | No  |
| TEARDOWN | Tear down objects | N/A | Yes | Yes |
| Tear down instant volume mount | N/A | No  | Yes |
| CANCEL | Cancel a running job run | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes except for cloning | Yes except for cloning | Yes except for cloning |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | N/A | Yes | Yes |
| FILE RECOVERY | Search for files or folders | N/A | No  | No  |
| Browse for a backed-up object | N/A | No  | Yes |
| Download files/folders | N/A | No  | Yes |
| Recover files | N/A | No  | Yes |
{: caption="" caption-side="bottom"}

### Archival workflows for on-prem applications
{: #archival_workflows_for_on-prem_applications}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| SAP HANA | Microsoft Exchange OnPrem | Microsoft Active Directory |
| --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | N/A | Yes | Yes |
| Archive a full backup | N/A | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | N/A | Yes | No  |
| Recover an entire backup job run from an archive to a source | N/A | Yes | Yes |
| Instant volume mount from an archive | N/A | No  | Yes |
| Crash-consistent recover for apps from an archive | N/A | No  | No  |
| CANCEL | Cancel a running archival task | N/A | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | N/A | Yes except for cloning | Yes except for cloning |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | N/A | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | N/A | No  | No  |
| Browse for files or folders from an archive snapshot | N/A | No  | No  |
| Download files and folders from an archive | N/A | No  | No  |
| Recover files from an archive to a source (to the same and different sources) | N/A | No  | No  |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | N/A | No  | No  |
| Download metadata for all runs of a job | N/A | Yes | Yes |
| Download a snapshot (archived data) for a job run | N/A | Yes | Yes |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | N/A | No  | No  |
| Clone all objects of a job run from an archive | N/A | No  | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | N/A | Yes | No  |
| Recover all objects of job run from an archive to a source | N/A | Yes | Yes |
| Instant volume mount from an archive | N/A | No  | Yes |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | N/A | No  | No  |
| Search files or folders from the metadata of a retrieved job run | N/A | No  | No  |
| Browse files or folders from a retrieved snapshot | N/A | No  | Yes |
| Browse files or folders from the metadata of a downloaded job run | N/A | No  | No  |
| Download files and folders from an archive | N/A | No  | Yes |
| Recover files from an archive to a source (to the same and different sources) | N/A | No  | Yes |
{: caption="" caption-side="bottom"}

### Replication workflows for on-prem applications
{: #replication_workflows_for_on-prem_applications}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| SAP HANA | Microsoft Exchange OnPrem | Microsoft Active Directory |
| --- | --- | --- | --- | --- |
|     | Replicate a full backup with CBT to a remote cluster | No  | Yes | Yes |
|     | Replicate a full backup without CBT to a remote cluster | No  | Yes | Yes |
| FAILOVER | Failover an inactive Job on a remote cluster | N/A | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | N/A | Yes | No  |
| Recover an entire backup job run from a remote cluster | N/A | No  | Yes |
| Instant volume mount of replicated data | N/A | No  | Yes |
| Crash-consistent recover for apps from a remote cluster | N/A | No  | No  |
| TEARDOWN | Tear down object on a remote cluster | N/A | Yes | Yes |
| Tear down instant volume mount on a remote cluster | N/A | No  | Yes |
| CANCEL | Cancel a running replication task | No  | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | No  | Supported(Clone not supported) | Supported(Clone not supported) |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | No  | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | No  | No  | No  |
| Browse for replicated object on a remote cluster | No  | No  | Yes |
| Download replicated files and folders from a remote cluster | No  | No  | Yes |
| Recover replicated files from a remote cluster | No  | No  | Yes |
{: caption="" caption-side="bottom"}

## Nas workflows
{: #nas_workflows}

### Protection workflows for nas nfs
{: #protection_workflows_for_nas_nfs}


| Categories | Workflows | Sources |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| NetApp - NFS | Generic NAS - NFS | Isilon - NFS | Pure FlashBlade - NFS | GPFS - NFS | Elastifile - NFS |
| --- | --- | --- | --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes | Yes | Yes | Yes |
| Auto-protect new objects | Yes | N/A | No  | No  | No  | No  |
| BACKUP | Incremental backup with CBT | No  | No  | No  | No  | No  | No  |
| Incremental Backup with CFT | Supported (Snapdiff) | Yes | Supported (Changelist) | Yes | Yes | Yes |
| Full backup without CBT | Yes | Yes | Yes | Yes | Yes | Yes |
| Backup, archive and replicate now | Yes | Yes | Yes | Yes | Yes | Yes |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover an entire backup job run | No  | No  | No  | No  | No  | No  |
| Instant Recovery | Yes | Yes | Yes | Yes | Yes | Yes |
| CLONE | Clone an object of a job run | No  | No  | No  | No  | No  | No  |
| Clone all the objects of a job run | No  | No  | No  | No  | No  | No  |
| CANCEL | Cancel a running job run | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Browse for a backed-up object | Yes | Yes | Yes | Yes | Yes | Yes |
| Download files/folders | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover files | Yes | Yes | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Archival workflows for nas nfs
{: #archival_workflows_for_nas_nfs}


| Categories | Workflows | Sources |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| NetApp - NFS | Generic NAS - NFS | Isilon - NFS | Pure FlashBlade - NFS | GPFS - NFS | Elastifile - NFS |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes | Yes | Yes | Yes | Yes |
| Archive a full backup | Yes | Yes | Yes | Yes | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover an entire backup job run from an archive to a source | No  | No  | No  | No  | No  | No  |
| CLONE | Clone objects of a job run from an archive | Yes | Yes | Yes | Yes | Yes | Yes |
| Clone all objects of a job run from an archive | No  | No  | No  | No  | No  | No  |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | Yes | Yes | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Browse for files or folders from an archive snapshot | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Download files and folders from an archive | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | Yes | Yes | Yes | Yes | Yes | Yes |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Yes | Yes | Yes | Yes | Yes | Yes |
| Download metadata for all runs of a job | Yes | Yes | Yes | Yes | Yes | Yes |
| Download a snapshot (archived data) for a job run | Yes | Yes | Yes | Yes | Yes | Yes |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | Yes | Yes | Yes | Yes | Yes | Yes |
| Clone all objects of a job run from an archive | No  | No  | No  | No  | No  | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover all objects of job run from an archive to a source | No  | No  | No  | No  | No  | No  |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Search files or folders from the metadata of a retrieved job run | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Browse files or folders from a retrieved snapshot | Yes | Yes | Yes | Yes | Yes | Yes |
| Browse files or folders from the metadata of a downloaded job run | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Download files and folders from an archive | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Recover files from an archive to a source (to the same and different sources) | Yes | Yes | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Replication workflows for nas nfs
{: #replication_workflows_for_nas_nfs}


| Categories | Workflows | Sources |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| NetApp - NFS | Generic NAS - NFS | Isilon - NFS | Pure FlashBlade - NFS | GPFS - NFS | Elastifile - NFS |
| --- | --- | --- | --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A |
| Replicate a full backup without CBT to a remote cluster | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A |
| FAILOVER | Failover an inactive Job on a remote cluster | No  | No  | No  | No  | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery |
| Recover an entire backup job run from a remote cluster | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery |
| CLONE | Clone an object of a job run from a remote cluster | No  | No  | No  | No  | No  | No  |
| Clone all objects of a job run from a remote cluster | No  | No  | No  | No  | No  | No  |
| CANCEL | Cancel a running replication task | Yes | Yes | Yes | Yes | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Browse for replicated object on a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes |
| Download replicated files and folders from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover replicated files from a remote cluster | Yes | Yes | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Protection workflows for nas smb/cifs
{: #protection_workflows_for_nas_smbcifs}


| Categories | Workflows | Sources |     |     |     |
| --- | --- | --- | --- | --- | --- |
| NetApp - SMB | Generic NAS - SMB | Isilon - SMB | Pure FlashBlade - SMB |
| --- | --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes | Yes |
| Auto-protect new objects | Yes | N/A | No  | No  |
| BACKUP | Incremental backup with CBT | No  | No  | No  | No  |
| Incremental Backup with CFT | Supported (Snapdiff) | No  | Supported (Changelist) | No  |
| Full backup without CBT | Yes | Yes | Yes | Yes |
| Backup, archive and replicate now | Yes | Yes | Yes | Yes |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes | Yes |
| Recover an entire backup job run | No  | No  | No  | No  |
| Instant Recovery | Yes | Yes | Yes | Yes |
| CLONE | Clone an object of a job run | No  | No  | No  | No  |
| Clone all the objects of a job run | No  | No  | No  | No  |
| CANCEL | Cancel a running job run | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Browse for a backed-up object | Yes | Yes | Yes | Yes |
| Download files/folders | Yes | Yes | Yes | Yes |
| Recover files | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Archival workflows for nas smb/cifs
{: #archival_workflows_for_nas_smbcifs}


| Categories | Workflows | Sources |     |     |     |
| --- | --- | --- | --- | --- | --- |
| NetApp - SMB | Generic NAS - SMB | Isilon - SMB | Pure FlashBlade - SMB |
| --- | --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes | Yes | Yes |
| Archive a full backup | Yes | Yes | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes | Yes | Yes | Yes |
| Recover an entire backup job run from an archive to a source | No  | No  | No  | No  |
| CLONE | Clone objects of a job run from an archive | Yes | Yes | Yes | Yes |
| Clone all objects of a job run from an archive | No  | No  | No  | No  |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Browse for files or folders from an archive snapshot | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Download files and folders from an archive | Yes | Yes | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | Yes | Yes | Yes | Yes |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Yes | Yes | Yes | Yes |
| Download metadata for all runs of a job | Yes | Yes | Yes | Yes |
| Download a snapshot (archived data) for a job run | Yes | Yes | Yes | Yes |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | Yes | Yes | Yes | Yes |
| Clone all objects of a job run from an archive | No  | No  | No  | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Yes | Yes | Yes | Yes |
| Recover all objects of job run from an archive to a source | No  | No  | No  | No  |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Search files or folders from the metadata of a retrieved job run | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Browse files or folders from a retrieved snapshot | Yes | Yes | Yes | Yes |
| Browse files or folders from the metadata of a downloaded job run | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Download files and folders from an archive | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Recover files from an archive to a source (to the same and different sources) | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Replication workflows for nas smb/cifs
{: #replication_workflows_for_nas_smbcifs}


| Categories | Workflows | Sources |     |     |     |
| --- | --- | --- | --- | --- | --- |
| NetApp - SMB | Generic NAS - SMB | Isilon - SMB | Pure FlashBlade - SMB |
| --- | --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A |
| Replicate a full backup without CBT to a remote cluster | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A | Yes but CBT is N/A |
| FAILOVER | Failover an inactive Job on a remote cluster | No  | No  | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery |
| Recover an entire backup job run from a remote cluster | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery | Yes for single vol recovery |
| CLONE | Clone an object of a job run from a remote cluster | No  | No  | No  | No  |
| Clone all objects of a job run from a remote cluster | No  | No  | No  | No  |
| CANCEL | Cancel a running replication task | Yes | Yes | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | Yes | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Browse for replicated object on a remote cluster | Yes | Yes | Yes | Yes |
| Download replicated files and folders from a remote cluster | Yes | Yes | Yes | Yes |
| Recover replicated files from a remote cluster | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

## Aws cloud workflows
{: #aws_cloud_workflows}

### Protection workflows for aws cloud vm (ec2 instance) - cohesity snapshot
{: #protection_workflows_for_aws_cloud_vm_ec2_instance_-_cohesity_snapshot}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| AWS Instances (Native Backup) | AWS-Gov Instances (Native Backup) | AWS-C2S Instances (Native Backup) |
| --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes |
| Auto-protect new objects | Yes | Yes | Yes |
| BACKUP | Incremental backup with CBT | Yes | Yes | Yes |
| Backup, archive and replicate now | Yes | Yes | Yes |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes |
| Recover an entire backup job run | Yes | Yes | Yes |
| Instant volume mount | No  | No  | No  |
| Crash-consistent recover for apps | No  | No  | No  |
| CLONE | Clone an object of a job run | No  | No  | No  |
| Clone all the objects of a job run | No  | No  | No  |
| Crash-consistent recover for apps | No  | No  | No  |
| TEARDOWN | Tear down objects | No  | No  | No  |
| Tear down instant volume mount | No  | No  | No  |
| CANCEL | Cancel a running job run | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders | Yes | Yes | Yes |
| Browse for a backed-up object | Yes | Yes | Yes |
| Download files/folders | Yes | Yes | Yes |
| Recover files | Yes | Yes | Yes |
| CONVERT AND DEPLOY | Register AWS source and clone VMs to AWS | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Archival workflows for aws cloud vm (ec2 instance) - cohesity snapshot
{: #archival_workflows_for_aws_cloud_vm_ec2_instance_-_cohesity_snapshot}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| AWS Instances (Native Backup) | AWS-Gov Instances (Native Backup) | AWS-C2S Instances (Native Backup) |
| --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes | Yes |
| Archive a full backup | Yes | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes | Yes | Yes |
| Recover an entire backup job run from an archive to a source | Yes | Yes | Yes |
| Instant volume mount from an archive | No  | No  | No  |
| CLONE | Clone objects of a job run from an archive | No  | No  | No  |
| Clone all objects of a job run from an archive | No  | No  | No  |
| Crash-consistent clone for apps from an archive | No  | No  | No  |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | Yes | Yes | Yes |
| Browse for files or folders from an archive snapshot | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Download files and folders from an archive | Yes | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | No  | No  | No  |
| Download n VMX file from an archive | No  | No  | No  |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Yes | Yes | Yes |
| Download metadata for all runs of a job | Yes | Yes | Yes |
| Download a snapshot (archived data) for a job run | Yes | Yes | Yes |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | No  | No  | No  |
| Clone all objects of a job run from an archive | No  | No  | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Yes | Yes | Yes |
| Recover all objects of job run from an archive to a source | Yes | Yes | Yes |
| Instant volume mount from an archive | No  | No  | No  |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | Yes | Yes | Yes |
| Search files or folders from the metadata of a retrieved job run | Yes | Yes | Yes |
| Browse files or folders from a retrieved snapshot | Yes | Yes | Yes |
| Browse files or folders from the metadata of a downloaded job run | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Download files and folders from an archive | Yes | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Replication workflows for aws cloud vm (ec2 instance) - cohesity snapshot
{: #replication_workflows_for_aws_cloud_vm_ec2_instance_-_cohesity_snapshot}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| AWS Instances (Native Backup) | AWS-Gov Instances (Native Backup) | AWS-C2S Instances (Native Backup) |
| --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes | Yes | No  |
| FAILOVER | Failover an inactive Job on a remote cluster | Supported(Only during Failback) | Supported(Only during Failback) | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes | Yes | No  |
| Recover an entire backup job run from a remote cluster | Yes | Yes | No  |
| Instant volume mount of replicated data | No  | No  | No  |
| CLONE | Clone an object of a job run from a remote cluster | No  | Supported(Only during Failback) | No  |
| Clone all objects of a job run from a remote cluster | No  | Supported(Only during Failback) | No  |
| Crash-consistent clone for apps from a remote cluster | No  | No  | No  |
| TEARDOWN | Tear down object on a remote cluster | No  | No  | No  |
| Tear down instant volume mount on a remote cluster | No  | No  | No  |
| CANCEL | Cancel a running replication task | Yes | Yes | No  |
| Cancel a clone or recover task on a remote cluster | Yes | Yes | No  |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | No  |
| FILE RECOVERY | Search for files or folders on a remote cluster | Yes | Yes | No  |
| Browse for replicated object on a remote cluster | Yes | Yes | No  |
| Download replicated files and folders from a remote cluster | Yes | Yes | No  |
| Recover replicated files from a remote cluster | Yes | No  | No  |
{: caption="" caption-side="bottom"}

### Protection workflows for aws cloud - ebs snapshot
{: #protection_workflows_for_aws_cloud_-_ebs_snapshot}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| AWS Instances (CSM) | AWS-Gov Instances (CSM) | AWS-C2S Instances (CSM) |
| --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes |
| Auto-protect new objects | Yes | Yes | Yes |
| BACKUP | Incremental backup with CBT | Supported(AWs does incremental backups) | Supported(AWS does incremental backups) | Supported(AWS does incremental backups) |
| Backup, archive and replicate now | Supported (Only Backup is Applicable) | Supported (Only Backup is Applicable) | Supported (Only Backup is Applicable) |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes |
| Recover an entire backup job run | Yes | Yes | Yes |
| CANCEL | Cancel a running job run | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders | No  | No  | No  |
| Browse for a backed-up object | No  | No  | No  |
| Download files/folders | No  | No  | No  |
| Recover files | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Archival workflows for aws cloud - ebs snapshot
{: #archival_workflows_for_aws_cloud_-_ebs_snapshot}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| AWS Instances (CSM) | AWS-Gov Instances (CSM) | AWS-C2S Instances (CSM) |
| --- | --- | --- | --- | --- |
| RECOVER | Recover a single object of a job run from an archive to a source | N/A | N/A | N/A |
| Recover an entire backup job run from an archive to a source | N/A | N/A | N/A |
| Instant volume mount from an archive | N/A | N/A | N/A |
| Crash-consistent recover for apps from an archive | N/A | N/A | N/A |
| CLONE | Clone objects of a job run from an archive | N/A | N/A | N/A |
| Clone all objects of a job run from an archive | N/A | N/A | N/A |
| clone db to FCI | N/A | N/A | N/A |
| Crash-consistent clone for apps from an archive | N/A | N/A | N/A |
| CANCEL | Cancel a running archival task | N/A | N/A | N/A |
| Cancel a running clone or recover task that is retrieving from an archive | N/A | N/A | N/A |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | N/A | N/A | N/A |
| FILE RECOVERY | Search for files or folders from an archive | N/A | N/A | N/A |
| Browse for files or folders from an archive snapshot | N/A | N/A | N/A |
| Download files and folders from an archive | N/A | N/A | N/A |
| Recover files from an archive to a source (to the same and different sources) | N/A | N/A | N/A |
| Download n VMX file from an archive | N/A | N/A | N/A |
| VIRTUAL DISKS RECOVERY | Recover Virtual Disks | N/A | N/A | N/A |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | N/A | N/A | N/A |
| Download metadata for all runs of a job | N/A | N/A | N/A |
| Download a snapshot (archived data) for a job run | N/A | N/A | N/A |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | N/A | N/A | N/A |
| Clone all objects of a job run from an archive | N/A | N/A | N/A |
| clone DB to FCI | N/A | N/A | N/A |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | N/A | N/A | N/A |
| Recover all objects of job run from an archive to a source | N/A | N/A | N/A |
| Instant volume mount from an archive | N/A | N/A | N/A |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | N/A | N/A | N/A |
| Search files or folders from the metadata of a retrieved job run | N/A | N/A | N/A |
| Browse files or folders from a retrieved snapshot | N/A | N/A | N/A |
| Browse files or folders from the metadata of a downloaded job run | N/A | N/A | N/A |
| Download files and folders from an archive | N/A | N/A | N/A |
| Recover files from an archive to a source (to the same and different sources) | N/A | N/A | N/A |
| Download a VMX file from an archive | N/A | N/A | N/A |
| CLOUD RETRIEVE - VIRTUAL DISKS RECOVERY | Recover Virtual Disks | N/A | N/A | N/A |
{: caption="" caption-side="bottom"}

### Replication workflows for aws cloud - ebs snapshot
{: #replication_workflows_for_aws_cloud_-_ebs_snapshot}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| AWS Instances (CSM) | AWS-Gov Instances (CSM) | AWS-C2S Instances (CSM) |
| --- | --- | --- | --- | --- |
| CLOUD REPLICATION | Create a snapshot in AWS and replicate to another region | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Protection workflows for aws cloud - rds/aurora snapshot
{: #protection_workflows_for_aws_cloud_-_rdsaurora_snapshot}


| Categories | Workflows | Sources |     |     |     |
| --- | --- | --- | --- | --- | --- |
| AWS RDS | AWS-Gov RDS | AWS Aurora | AWS-Gov Aurora |
| --- | --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes | Yes |
| Auto-protect new objects | No  | No  | No  | No  |
| BACKUP | Incremental backup with CBT | Supported(AWS does incremental backups) | Supported(AWS does incremental backups) | Supported(AWS does incremental backups) | Supported(AWS does incremental backups) |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes | Yes |
| Recover an entire backup job run | No  | No  | No  | No  |
| CANCEL | Cancel a running job run | Yes | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders | No  | No  | No  | No  |
| Browse for a backed-up object | No  | No  | No  | No  |
| Download files/folders | No  | No  | No  | No  |
| Recover files | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Archival workflows for aws cloud - rds/aurora snapshot
{: #archival_workflows_for_aws_cloud_-_rdsaurora_snapshot}


| Categories | Workflows | Sources |     |     |     |
| --- | --- | --- | --- | --- | --- |
| AWS RDS | AWS-Gov RDS | AWS Aurora | AWS-Gov Aurora |
| --- | --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | N/A | N/A | N/A | N/A |
| Archiving an incremental backup with SQL server differential | N/A | N/A | N/A | N/A |
| Archive a full backup | N/A | N/A | N/A | N/A |
| RECOVER | Recover a single object of a job run from an archive to a source | N/A | N/A | N/A | N/A |
| Recover an entire backup job run from an archive to a source | N/A | N/A | N/A | N/A |
| Instant volume mount from an archive | N/A | N/A | N/A | N/A |
| Crash-consistent recover for apps from an archive | N/A | N/A | N/A | N/A |
| CLONE | Clone objects of a job run from an archive | N/A | N/A | N/A | N/A |
| Clone all objects of a job run from an archive | N/A | N/A | N/A | N/A |
| clone db to FCI | N/A | N/A | N/A | N/A |
| Crash-consistent clone for apps from an archive | N/A | N/A | N/A | N/A |
| CANCEL | Cancel a running archival task | N/A | N/A | N/A | N/A |
| Cancel a running clone or recover task that is retrieving from an archive | N/A | N/A | N/A | N/A |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | N/A | N/A | N/A | N/A |
| FILE RECOVERY | Search for files or folders from an archive | N/A | N/A | N/A | N/A |
| Browse for files or folders from an archive snapshot | N/A | N/A | N/A | N/A |
| Download files and folders from an archive | N/A | N/A | N/A | N/A |
| Recover files from an archive to a source (to the same and different sources) | N/A | N/A | N/A | N/A |
| Download n VMX file from an archive | N/A | N/A | N/A | N/A |
| VIRTUAL DISKS RECOVERY | Recover Virtual Disks | N/A | N/A | N/A | N/A |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | N/A | N/A | N/A | N/A |
| Download metadata for all runs of a job | N/A | N/A | N/A | N/A |
| Download a snapshot (archived data) for a job run | N/A | N/A | N/A | N/A |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | N/A | N/A | N/A | N/A |
| Clone all objects of a job run from an archive | N/A | N/A | N/A | N/A |
| clone DB to FCI | N/A | N/A | N/A | N/A |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | N/A | N/A | N/A | N/A |
| Recover all objects of job run from an archive to a source | N/A | N/A | N/A | N/A |
| Instant volume mount from an archive | N/A | N/A | N/A | N/A |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | N/A | N/A | N/A | N/A |
| Search files or folders from the metadata of a retrieved job run | N/A | N/A | N/A | N/A |
| Browse files or folders from a retrieved snapshot | N/A | N/A | N/A | N/A |
| Browse files or folders from the metadata of a downloaded job run | N/A | N/A | N/A | N/A |
| Download files and folders from an archive | N/A | N/A | N/A | N/A |
| Recover files from an archive to a source (to the same and different sources) | N/A | N/A | N/A | N/A |
| Download a VMX file from an archive | N/A | N/A | N/A | N/A |
| CLOUD RETRIEVE - VIRTUAL DISKS RECOVERY | Recover Virtual Disks | N/A | N/A | N/A | N/A |
{: caption="" caption-side="bottom"}

### Replication workflows for aws cloud - rds/aurora snapshot
{: #replication_workflows_for_aws_cloud_-_rdsaurora_snapshot}


| Categories | Workflows | Sources |     |     |     |
| --- | --- | --- | --- | --- | --- |
| AWS RDS | AWS-Gov RDS | AWS Aurora | AWS-Gov Aurora |
| --- | --- | --- | --- | --- | --- |
| CLOUD REPLICATION | Create a snapshot in AWS and replicate to another region | Yes | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Protection workflows for aws s3
{: #protection_workflows_for_aws_s3}


| Categories | Workflows | Sources |
| --- | --- | --- |
| AWS S3 |
| --- | --- | --- |
| REGISTRATION | Register a source | Yes |
| Refresh object hierarchy | Yes |
| Auto-protect new objects | No  |
| BACKUP | Incremental backup with CFT | Yes |
| Backup, archive and replicate now | Yes, only Backup |
| RECOVER | Recovering an entity in the Job Run | Yes |
| Recover an entire backup job run | No  |
| CANCEL | Cancel a running job run | Yes |
| Cancel a running clone or recover tasks | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes |
| FILE RECOVERY | Search for files or folders | No  |
| Browse for a backed-up object | No  |
| Download files/folders | No  |
| Recover files | No  |
{: caption="" caption-side="bottom"}

### Archival workflows for aws s3
{: #archival_workflows_for_aws_s3}


| Categories | Workflows | Sources |
| --- | --- | --- |
| AWS S3 |
| --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes |
| Archive a full backup | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes |
| Recover an entire backup job run from an archive to a source | Yes |
| CLONE | Clone objects of a job run from an archive | No  |
| Clone all objects of a job run from an archive | No  |
| Crash-consistent clone for apps from an archive | No  |
| CANCEL | Cancel a running archival task | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | Yes |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes |
| FILE RECOVERY | Search for files or folders from an archive | No  |
| Browse for files or folders from an archive snapshot | No  |
| Download files and folders from an archive | No  |
| Recover files from an archive to a source (to the same and different sources) | No  |
| Download n VMX file from an archive | No  |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | No  |
| Download metadata for all runs of a job | No  |
| Download a snapshot (archived data) for a job run | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | No  |
| Recover all objects of job run from an archive to a source | No  |
| Instant volume mount from an archive | No  |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | No  |
| Search files or folders from the metadata of a retrieved job run | No  |
| Browse files or folders from a retrieved snapshot | No  |
| Browse files or folders from the metadata of a downloaded job run | No  |
| Download files and folders from an archive | No  |
| Recover files from an archive to a source (to the same and different sources) | No  |
{: caption="" caption-side="bottom"}

### Replication workflows for aws s3
{: #replication_workflows_for_aws_s3}


| Categories | Workflows | Sources |
| --- | --- | --- |
| AWS S3 |
| --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | No  |
| FAILOVER | Failover an inactive Job on a remote cluster | Yes (only during Failback) |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes |
| Recover an entire backup job run from a remote cluster | Yes |
| CANCEL | Cancel a running replication task | Yes |
| Cancel a clone or recover task on a remote cluster | Yes |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | No  |
| FILE RECOVERY | Search for files or folders on a remote cluster | No  |
| Browse for replicated object on a remote cluster | No  |
| Download replicated files and folders from a remote cluster | No  |
| Recover replicated files from a remote cluster | No  |
| CLOUD REPLICATION | Create a snapshot in AWS and replicate to another region | No  |
{: caption="" caption-side="bottom"}

## Azure cloud workflows
{: #azure_cloud_workflows}

### Protection workflows for azure cloud vm - cohesity snapshot
{: #protection_workflows_for_azure_cloud_vm_-_cohesity_snapshot}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| Azure VMs (Native Backup) | Azure-Gov VMs (Native Backup) | Azure stack VMs (Native Backup) |
| --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes | Yes |
| Refresh object hierarchy | Yes | Yes | Yes |
| Auto-protect new objects | Yes | Yes | Yes |
| BACKUP | Incremental backup with CBT | Yes | Yes | Yes |
| Backup, archive and replicate now | Yes | Yes | Yes |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes | Yes |
| Recover an entire backup job run | Yes | Yes | Yes |
| Instant volume mount | No  | No  | No  |
| Crash-consistent recover for apps | No  | No  | No  |
| CLONE | Clone an object of a job run | No  | No  | No  |
| Clone all the objects of a job run | No  | No  | No  |
| Crash-consistent recover for apps | No  | No  | No  |
| TEARDOWN | Tear down objects | No  | No  | No  |
| Tear down instant volume mount | No  | No  | No  |
| CANCEL | Cancel a running job run | Yes | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders | Yes | Yes | Yes |
| Browse for a backed-up object | Yes | Yes | Yes |
| Download files/folders | Yes | Yes | Yes |
| Recover files | No  | No  | No  |
| CONVERT AND DEPLOY | Register Azure source and clone VMs to Azure | No  | No  | No  |
| Register AWS source and clone VMs to AWS | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Archival workflows for azure cloud vm - cohesity snapshot
{: #archival_workflows_for_azure_cloud_vm_-_cohesity_snapshot}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| Azure VMs (Native Backup) | Azure-Gov VMs (Native Backup) | Azure stack VMs (Native Backup) |
| --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes | Yes |
| Archive a full backup | Yes | Yes | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes | Yes | Yes |
| Recover an entire backup job run from an archive to a source | Yes | Yes | Yes |
| Instant volume mount from an archive | No  | No  | No  |
| CLONE | Clone objects of a job run from an archive | No  | No  | No  |
| Clone all objects of a job run from an archive | No  | No  | No  |
| Crash-consistent clone for apps from an archive | No  | No  | No  |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | Yes | Yes | Yes |
| Browse for files or folders from an archive snapshot | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Download files and folders from an archive | Yes | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | No  | No  | No  |
| Download n VMX file from an archive | No  | No  | No  |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Yes | Yes | Yes |
| Download metadata for all runs of a job | Yes | Yes | Yes |
| Download a snapshot (archived data) for a job run | Yes | Yes | Yes |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | No  | No  | No  |
| Clone all objects of a job run from an archive | No  | No  | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Yes | Yes | Yes |
| Recover all objects of job run from an archive to a source | Yes | Yes | Yes |
| Instant volume mount from an archive | No  | No  | No  |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | Yes | Yes | Yes |
| Search files or folders from the metadata of a retrieved job run | Yes | Yes | Yes |
| Browse files or folders from a retrieved snapshot | Yes | Yes | Yes |
| Browse files or folders from the metadata of a downloaded job run | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) | Supported (Protection Run must be indexed) |
| Download files and folders from an archive | Yes | Yes | Yes |
| Recover files from an archive to a source (to the same and different sources) | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Replication workflows for azure cloud vm - cohesity snapshot
{: #replication_workflows_for_azure_cloud_vm_-_cohesity_snapshot}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| Azure VMs (Native Backup) | Azure-Gov VMs (Native Backup) | Azure stack VMs (Native Backup) |
| --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes | Yes | Yes |
| FAILOVER | Failover an inactive Job on a remote cluster | No  | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes | Yes | Yes |
| Recover an entire backup job run from a remote cluster | Yes | Yes | Yes |
| Instant volume mount of replicated data | No  | No  | No  |
| Crash-consistent recover for apps from a remote cluster | No  | No  | No  |
| CLONE | Clone an object of a job run from a remote cluster | No  | No  | No  |
| Clone all objects of a job run from a remote cluster | No  | No  | No  |
| Crash-consistent clone for apps from a remote cluster | No  | No  | No  |
| TEARDOWN | Tear down object on a remote cluster | No  | No  | No  |
| Tear down instant volume mount on a remote cluster | No  | No  | No  |
| CANCEL | Cancel a running replication task | Yes | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | Yes | Yes | Yes |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | Yes | Yes | Yes |
| Browse for replicated object on a remote cluster | Yes | Yes | Yes |
| Download replicated files and folders from a remote cluster | Yes | Yes | Yes |
| Recover replicated files from a remote cluster | Yes | Yes | Yes |
{: caption="" caption-side="bottom"}

### Protection workflows for azure cloud vms - csm
{: #protection_workflows_for_azure_cloud_vms_-_csm}


| Categories | Workflows | Sources |     |
| --- | --- | --- | --- |
| Azure VMs(CSM) | Azure-Gov VMs (CSM) |
| --- | --- | --- | --- |
| REGISTRATION | Register a source | Yes | Yes |
| Refresh object hierarchy | Yes | Yes |
| Auto-protect new objects | Yes | Yes |
| BACKUP | Incremental backup with CBT | Supported(Azure does incremental backups) | Supported(Azure does incremental backups) |
| Backup, archive and replicate now | Supported (Only Backup is Applicable) | Supported (Only Backup is Applicable) |
| RECOVER | Recovering an entity in the Job Run | Yes | Yes |
| Recover an entire backup job run | Yes | Yes |
| CANCEL | Cancel a running job run | Yes | Yes |
| Cancel a running clone or recover tasks | Yes | Yes |
| FILE RECOVERY | Search for files or folders | No  | No  |
| Browse for a backed-up object | No  | No  |
| Download files/folders | No  | No  |
| Recover files | No  | No  |
| CONVERT AND DEPLOY | Register Azure source and clone VMs to Azure | No  | No  |
| Register AWS source and clone VMs to AWS | No  | No  |
{: caption="" caption-side="bottom"}

### Archival workflows for azure cloud vms - csm
{: #archival_workflows_for_azure_cloud_vms_-_csm}


| Categories | Workflows | Sources |     |
| --- | --- | --- | --- |
| Azure VMs(CSM) | Azure-Gov VMs (CSM) |
| --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | N/A | N/A |
| Archiving an incremental backup with SQL server differential | N/A | N/A |
| Archive a full backup | N/A | N/A |
| RECOVER | Recover a single object of a job run from an archive to a source | N/A | N/A |
| Recover an entire backup job run from an archive to a source | N/A | N/A |
| Instant volume mount from an archive | N/A | N/A |
| Crash-consistent recover for apps from an archive | N/A | N/A |
| CLONE | Clone objects of a job run from an archive | N/A | N/A |
| Clone all objects of a job run from an archive | N/A | N/A |
| clone db to FCI | N/A | N/A |
| Crash-consistent clone for apps from an archive | N/A | N/A |
| CANCEL | Cancel a running archival task | N/A | N/A |
| Cancel a running clone or recover task that is retrieving from an archive | N/A | N/A |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | N/A | N/A |
| FILE RECOVERY | Search for files or folders from an archive | N/A | N/A |
| Browse for files or folders from an archive snapshot | N/A | N/A |
| Download files and folders from an archive | N/A | N/A |
| Recover files from an archive to a source (to the same and different sources) | N/A | N/A |
| Download n VMX file from an archive | N/A | N/A |
| VIRTUAL DISKS RECOVERY | Recover Virtual Disks | N/A | N/A |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | N/A | N/A |
| Download metadata for all runs of a job | N/A | N/A |
| Download a snapshot (archived data) for a job run | N/A | N/A |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | N/A | N/A |
| Clone all objects of a job run from an archive | N/A | N/A |
| clone DB to FCI | N/A | N/A |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | N/A | N/A |
| Recover all objects of job run from an archive to a source | N/A | N/A |
| Instant volume mount from an archive | N/A | N/A |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | N/A | N/A |
| Search files or folders from the metadata of a retrieved job run | N/A | N/A |
| Browse files or folders from a retrieved snapshot | N/A | N/A |
| Browse files or folders from the metadata of a downloaded job run | N/A | N/A |
| Download files and folders from an archive | N/A | N/A |
| Recover files from an archive to a source (to the same and different sources) | N/A | N/A |
| Download a VMX file from an archive | N/A | N/A |
| CLOUD RETRIEVE - VIRTUAL DISKS RECOVERY | Recover Virtual Disks | N/A | N/A |
{: caption="" caption-side="bottom"}

### Replication workflows for azure cloud vms - csm
{: #replication_workflows_for_azure_cloud_vms_-_csm}


| Categories | Workflows | Sources |     |
| --- | --- | --- | --- |
| Azure VMs(CSM) | Azure-Gov VMs (CSM) |
| --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | N/A | N/A |
| Replicate an incremental backup with SQL server differential to a remote cluster | N/A | N/A |
| Replicate a full backup with CBT to a remote cluster | N/A | N/A |
| Replicate a full backup without CBT to a remote cluster | N/A | N/A |
| FAILOVER | Failover an inactive Job on a remote cluster | N/A | N/A |
| RECOVER | Recover a single object of a job run from a remote cluster | N/A | N/A |
| Recover an entire backup job run from a remote cluster | N/A | N/A |
| Recover a SQL DB from a remote cluster to another SQL server with the same SQL version | N/A | N/A |
| Recover a SQL DB from a remote cluster to different SQL server with a different SQL version | N/A | N/A |
| Instant volume mount of replicated data | N/A | N/A |
| Crash-consistent recover for apps from a remote cluster | N/A | N/A |
| CLONE | Clone an object of a job run from a remote cluster | N/A | N/A |
| Clone all objects of a job run from a remote cluster | N/A | N/A |
| clone db to FCI | N/A | N/A |
| Crash-consistent clone for apps from a remote cluster | N/A | N/A |
| TEARDOWN | Tear down object on a remote cluster | N/A | N/A |
| Tear down instant volume mount on a remote cluster | N/A | N/A |
| CANCEL | Cancel a running replication task | N/A | N/A |
| Cancel a clone or recover task on a remote cluster | N/A | N/A |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | N/A | N/A |
| FILE RECOVERY | Search for files or folders on a remote cluster | N/A | N/A |
| Browse for replicated object on a remote cluster | N/A | N/A |
| Download replicated files and folders from a remote cluster | N/A | N/A |
| Recover replicated files from a remote cluster | N/A | N/A |
| Download a VMX file from a remote cluster | N/A | N/A |
| VIRTUAL DISKS RECOVERY | Recover Virtual Disks | N/A | N/A |
| CONVERT AND DEPLOY | Register Azure source and clone VMs to Azure | N/A | N/A |
| Register AWS source and clone VMs to AWS | N/A | N/A |
| CLOUD REPLICATION | Create a snapshot in AWS and replicate to another region | N/A | N/A |
| Create a snapshot in Azure and replicate to another region | N/A | N/A |
{: caption="" caption-side="bottom"}

## Gcp cloud workflows
{: #gcp_cloud_workflows}

### Protection workflows for gcp cloud vm - cohesity snapshot
{: #protection_workflows_for_gcp_cloud_vm_-_cohesity_snapshot}


| Categories | Workflows | Sources |
| --- | --- | --- |
| GCP VM Instances (Native Backup) |
| --- | --- | --- |
| REGISTRATION | Register a source | Yes |
| Refresh object hierarchy | Yes |
| Auto-protect new objects | No  |
| BACKUP | Incremental backup with CBT | Yes |
| Backup, archive and replicate now | Yes |
| CloudSpin to AWS and Azure | No  |
| RECOVER | Recovering an entity in the Job Run | Yes |
| Recover an entire backup job run | Yes |
| Instant volume mount | No  |
| Crash-consistent recover for apps | No  |
| CLONE | Clone an object of a job run | No  |
| Clone all the objects of a job run | No  |
| Crash-consistent recover for apps | No  |
| TEARDOWN | Tear down objects | No  |
| Tear down instant volume mount | No  |
| CANCEL | Cancel a running job run | Yes |
| Cancel a running clone or recover tasks | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes |
| FILE RECOVERY | Search for files or folders | Yes |
| Browse for a backed-up object | Yes |
| Download files/folders | Yes |
| Recover files | No  |
| CONVERT AND DEPLOY | Register Azure source and clone VMs to Azure | No  |
| Register AWS source and clone VMs to AWS | No  |
{: caption="" caption-side="bottom"}

### Archival workflows for gcp cloud vm - cohesity snapshot
{: #archival_workflows_for_gcp_cloud_vm_-_cohesity_snapshot}


| Categories | Workflows | Sources |
| --- | --- | --- |
| GCP VM Instances (Native Backup) |
| --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes |
| Archive a full backup | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | Yes |
| Recover an entire backup job run from an archive to a source | Yes |
| Instant volume mount from an archive | No  |
| CLONE | Clone objects of a job run from an archive | No  |
| Clone all objects of a job run from an archive | No  |
| Crash-consistent clone for apps from an archive | No  |
| CANCEL | Cancel a running archival task | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | Yes |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes |
| FILE RECOVERY | Search for files or folders from an archive | Yes |
| Browse for files or folders from an archive snapshot | Supported (Protection Run must be indexed) |
| Download files and folders from an archive | Yes |
| Recover files from an archive to a source (to the same and different sources) | No  |
| Download n VMX file from an archive | No  |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Yes |
| Download metadata for all runs of a job | Yes |
| Download a snapshot (archived data) for a job run | Yes |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | No  |
| Clone all objects of a job run from an archive | No  |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | Yes |
| Recover all objects of job run from an archive to a source | Yes |
| Instant volume mount from an archive | No  |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | Yes |
| Search files or folders from the metadata of a retrieved job run | Yes |
| Browse files or folders from a retrieved snapshot | Yes |
| Browse files or folders from the metadata of a downloaded job run | Supported (Protection Run must be indexed) |
| Download files and folders from an archive | Yes |
| Recover files from an archive to a source (to the same and different sources) | No  |
{: caption="" caption-side="bottom"}

### Replication workflows for gcp cloud vm - cohesity snapshot
{: #replication_workflows_for_gcp_cloud_vm_-_cohesity_snapshot}


| Categories | Workflows | Sources |
| --- | --- | --- |
| GCP VM Instances (Native Backup) |
| --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes |
| FAILOVER | Failover an inactive Job on a remote cluster | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | Yes |
| Recover an entire backup job run from a remote cluster | Yes |
| Instant volume mount of replicated data | No  |
| Crash-consistent recover for apps from a remote cluster | No  |
| CLONE | Clone an object of a job run from a remote cluster | No  |
| Clone all objects of a job run from a remote cluster | No  |
| Crash-consistent clone for apps from a remote cluster | No  |
| TEARDOWN | Tear down object on a remote cluster | No  |
| Tear down instant volume mount on a remote cluster | No  |
| CANCEL | Cancel a running replication task | Yes |
| Cancel a clone or recover task on a remote cluster | Yes |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | Yes |
| Browse for replicated object on a remote cluster | Yes |
| Download replicated files and folders from a remote cluster | Yes |
| Recover replicated files from a remote cluster | No  |
{: caption="" caption-side="bottom"}

## Miscellaneous workflows
{: #miscellaneous_workflows}

### Protection workflows for views, remote adapters, and pure volumes
{: #protection_workflows_for_views_remote_adapters_and_pure_volumes}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| Views | Remote Adapters | Pure Volumes |
| --- | --- | --- | --- | --- |
| REGISTRATION | Register a source | N/A | N/A | Yes |
| Refresh object hierarchy | N/A | N/A | Yes |
| BACKUP | Incremental backup with CBT | Yes | Yes | Yes |
| Full backup without CBT | N/A | N/A | Yes |
| Backup, archive and replicate now | Yes | Yes | Yes |
| RECOVER | Recovering an entity in the Job Run | N/A | N/A | Yes |
| Recover an entire backup job run | N/A | N/A | No  |
| CLONE | Clone an object of a job run | Yes | Yes | N/A |
| Clone all the objects of a job run | Yes | Yes | N/A |
| CANCEL | Cancel a running job run | N/A | Yes | Yes |
| Cancel a running clone or recover tasks | No  | Yes | Yes |
| DELETE SNAPSHOT | Delete a local snapshot of a job run | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders | Yes | No  | N/A |
| Browse for a backed-up object | No  | No  | N/A |
| Download files/folders | Yes | No  | N/A |
| Recover files | No  | No  | N/A |
{: caption="" caption-side="bottom"}

### Archival workflows for views, remote adapters, and pure volumes
{: #archival_workflows_for_views_remote_adapters_and_pure_volumes}

If the **Object Key Pattern** of the S3 View you want to archive is **Object ID**, then you must use the **Incremental Forever** archival format while archiving the S3 View. You cannot archive the S3 View using the **Incremental with Periodic Full** format.


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| Views | Remote Adapters | Pure Volumes |
| --- | --- | --- | --- | --- |
| ARCHIVAL | Archive an incremental backup | Yes | Yes | Yes |
| Archive a full backup | N/A | N/A | Yes |
| RECOVER | Recover a single object of a job run from an archive to a source | N/A | N/A | Yes |
| Recover an entire backup job run from an archive to a source | N/A | N/A | No  |
|     | Clone all objects of a job run from an archive | Yes | Yes | N/A |
| CANCEL | Cancel a running archival task | Yes | Yes | Yes |
| Cancel a running clone or recover task that is retrieving from an archive | No  | Yes | Yes |
| DELETE SNAPSHOT | Delete an archived snapshot of a job run from the external target | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders from an archive | Yes | No  | N/A |
| Browse for files or folders from an archive snapshot | No  | No  | N/A |
| Download files and folders from an archive | Yes | No  | N/A |
| CLOUD RETRIEVE - SEARCH & RETRIEVE | Search for an object in an external target | Yes | Yes | Yes |
| Download metadata for all runs of a job | Yes | Yes | Yes |
| Download a snapshot (archived data) for a job run | Yes | Yes | Yes |
| CLOUD RETRIEVE - CLONE | Clone objects of a job run from an archive | Yes | Yes | N/A |
| Clone all objects of a job run from an archive | Yes | Yes | N/A |
| CLOUD RETRIEVE - RECOVER | Recover objects of a job run from an archive to a source | N/A | N/A | Yes |
| Recover all objects of job run from an archive to a source | N/A | N/A | No  |
| CLOUD RETRIEVE - FILE RECOVERY | Search files or folders from downloaded snapshot | No  | No  | N/A |
| Search files or folders from the metadata of a retrieved job run | No  | No  | N/A |
| Browse files or folders from a retrieved snapshot | No  | No  | N/A |
| Browse files or folders from the metadata of a downloaded job run | No  | No  | N/A |
| Download files and folders from an archive | No  | No  | N/A |
{: caption="" caption-side="bottom"}

### Replication workflows for views, remote adapters, and pure volumes
{: #replication_workflows_for_views_remote_adapters_and_pure_volumes}


| Categories | Workflows | Sources |     |     |
| --- | --- | --- | --- | --- |
| Views | Remote Adapters | Pure Volumes |
| --- | --- | --- | --- | --- |
| REPLICATION | Replicate an incremental backup with CBT to a remote cluster | Yes | Yes | Yes |
| Replicate a full backup without CBT to a remote cluster | N/A | N/A | Yes |
| FAILOVER | Failover an inactive Job on a remote cluster | Yes | No  | No  |
| RECOVER | Recover a single object of a job run from a remote cluster | N/A | N/A | Yes |
| Recover an entire backup job run from a remote cluster | N/A | N/A | No  |
| CLONE | Clone an object of a job run from a remote cluster | Yes | Yes | N/A |
| Clone all objects of a job run from a remote cluster | Yes | Yes | N/A |
| CANCEL | Cancel a running replication task | Yes | Yes | Yes |
| Cancel a clone or recover task on a remote cluster | No  | Yes | Yes |
| DELETE SNAPSHOT | Delete replicated snapshot of a job run on a remote cluster | Yes | Yes | Yes |
| FILE RECOVERY | Search for files or folders on a remote cluster | No  | No  | N/A |
| Browse for replicated object on a remote cluster | No  | No  | N/A |
| Download replicated files and folders from a remote cluster | No  | No  | N/A |
| Recover replicated files from a remote cluster | No  | No  | N/A |
{: caption="" caption-side="bottom"}

## Aws external targets
{: #aws_external_targets}

### External targets for cloudarchive (with periodic full) - standard
{: #external_targets_for_cloudarchive_with_periodic_full_-_standard}


| Workflows | External Targets |     |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| AWS - S3 | AWS - S3-IA | AWS - S3-Intelligent | AWS - S3-One-Zone | AWS Snowball Edge | AWS - Glacier(Legacy) | AWS - S3 Glacier IR | AWS - S3-Glacier | AWS - S3 Glacier Deep Archive |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Enable/disable deduplication at external target level | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Manual management of encryption keys | Yes | Yes | Yes | Yes | No  | Yes | Yes | Yes | Yes |
| Enable/disable compression at external target level | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Unregister external target | Yes | Yes | Yes | Yes | No  | Yes | Yes | Yes | Yes |
| Perform search for CloudRetrieve operations | Yes | Yes | Yes | Yes | No  | Yes | Yes | Yes | Yes |
| Download jobs (CloudRetrieve) | Yes | Yes | No  | Yes | No  | Yes | Yes | Yes | Yes |
| Download files/folders from archive | Yes | Yes | No  | Yes | No  | Yes | Yes | Yes | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | Yes | No  | Yes | No  | Yes | Yes | Yes | No  |
| Object level recovery to source | Yes | Yes | Yes | Yes | No  | Yes | Yes | Yes | Yes |
| Recover files/folders to source | Yes | Yes | No  | Yes | No  | Yes | Yes | No  | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | Yes | Yes | Yes | Yes | No  | Yes | Yes | Yes | Yes |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | Yes | No  | Yes | No  | Yes | Yes | No  | No  |
| WORM Support | Yes | Yes | No  | Yes | No  | No  | Yes | Yes | Yes |
| Cloud Vendor Provided Life Cycle Management (LCM) | Yes | Yes | No  | Yes | No  | No  | Yes | Yes | No  |
| Cohesity Data Movement (LCM) | No  | No  | No  | No  | No  | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive (with periodic full) - gov
{: #external_targets_for_cloudarchive_with_periodic_full_-_gov}


| Workflows | External Targets |     |     |
| --- | --- | --- | --- |
| AWS Gov - S3 | AWS Gov - S3-Glacier | AWS Gov - S3 Glacier Deep Archive |
| --- | --- | --- | --- |
| Enable/disable deduplication at external target level | Yes | Yes | Yes |
| Manual management of encryption keys | Yes | Yes | Yes |
| Enable/disable compression at external target level | Yes | Yes | Yes |
| Unregister external target | Yes | Yes | Yes |
| Perform search for CloudRetrieve operations | Yes | Yes | Yes |
| Download jobs (CloudRetrieve) | Yes | Yes | Yes |
| Download files/folders from archive | Yes | Yes | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | Yes | No  |
| Object level recovery to source | Yes | Yes | Yes |
| Recover files/folders to source | Yes | No  | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs |     |     |     |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | No  | No  |
| WORM Support | No  | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | Yes | Yes | No  |
| Cohesity Data Movement (LCM) | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive (with periodic full) - c2s
{: #external_targets_for_cloudarchive_with_periodic_full_-_c2s}


| Workflows | External Targets |     |
| --- | --- | --- |
| AWS C2S - S3 | AWS C2S - Glacier |
| --- | --- | --- |
| Enable/disable deduplication at external target level | Yes | Yes |
| Manual management of encryption keys | Yes | Yes |
| Enable/disable compression at external target level | Yes | Yes |
| Unregister external target | Yes | Yes |
| Perform search for CloudRetrieve operations | Yes | Yes |
| Download jobs (CloudRetrieve) | Yes | Yes |
| Download files/folders from archive | Yes | Yes |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | Yes |
| Object level recovery to source | Yes | Yes |
| Recover files/folders to source | Yes | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | Yes | Yes |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | No  |
| WORM Support | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  |
| Cohesity Data Movement (LCM) | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive (incremental forever) and cloudarchive direct (with dedup) - standard
{: #external_targets_for_cloudarchive_incremental_forever_and_cloudarchive_direct_with_dedup_-_standard}

*   Supported sources for CloudArchive (Incremental Forever) are VMWare, NAS, Views, Physical, O365, Oracle, SQL, Amazon EC2.

*   Supported sources for CloudArchive Direct (with Dedup) are VMware, and NAS.

*   WORM is not supported for CloudArchive Direct .

*   CloudArchive Direct (with Dedup) for AWS S3 Glacier FLR is supported in NAS, but not supported in VMware.

*   Recover files/folders to source from archived snapshots is only supported for VMWare, HyperV, NAS, Cohesity Views, and Physical (Linux, Windows, Solaris, HPUX and AIX) sources.



| Workflows | External Targets |     |     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| AWS - S3 | AWS - S3-Intelligent | AWS - S3-One-Zone | AWS - S3-IA | AWS - S3 Glacier IR | AWS - S3 Glacier | AWS - S3 Glacier Deep Archive | AWS - Glacier(Legacy) | AWS Snowball Edge |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Enable/disable compression at external target level | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| Unregister external target | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| Manual management of encryption keys | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| Once enabled, going back to legacy archival | No  | No  | No  | No  | No  | No  | No  | No  | No  |
| Perform search for CloudRetrieve operations | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| Download jobs (CloudRetrieve) | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| Download files/folders from archive | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| Object level recovery to source | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| Recover files/folders to source | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | Yes | No  | Yes | Yes | Yes | Yes | No  | No  | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| WORM Support | Yes | No  | Yes | Yes | Yes | Yes | Yes | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  | No  | No  | No  | No  | No  | No  | No  |
| Cohesity Data Movement (LCM) | Yes | No  | Yes | Yes | Yes | Yes | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive (incremental forever) and cloudarchive direct (with dedup) - gov
{: #external_targets_for_cloudarchive_incremental_forever_and_cloudarchive_direct_with_dedup_-_gov}

*   Supported sources for CloudArchive (Incremental Forever) are VMWare, NAS, Views, Physical, O365, Oracle, SQL, Amazon EC2.

*   Supported sources for CloudArchive Direct (with Dedup) are VMware, and NAS.

*   CloudArchive Direct (with Dedup) for AWS Gov S3 Glacier FLR is supported in NAS, but not supported in VMware.



| Workflows | External Targets |     |     |
| --- | --- | --- | --- |
| AWS Gov - S3 | AWS Gov - S3 Glacier | AWS Gov - S3 Glacier Deep Archive |
| --- | --- | --- | --- |
| Enable/disable compression at external target level | Yes | Yes | Yes |
| Unregister external target | Yes | Yes | Yes |
| Manual management of encryption keys | Yes | Yes | Yes |
| Once enabled, going back to legacy archival | No  | No  | No  |
| Perform search for CloudRetrieve operations | Yes | Yes | No  |
| Download jobs (CloudRetrieve) | Yes | Yes | Yes |
| Download files/folders from archive | Yes | Yes | Yes |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | Yes | Yes |
| Object level recovery to source | Yes | Yes | Yes |
| Recover files/folders to source | Yes | Yes | Yes |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | Yes | Yes | Yes |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | Yes | Yes |
| WORM Support | No  | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  | No  |
| Cohesity Data Movement (LCM) | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive (incremental forever) and cloudarchive direct (with dedup) - c2s
{: #external_targets_for_cloudarchive_incremental_forever_and_cloudarchive_direct_with_dedup_-_c2s}

*   Supported sources for CloudArchive (Incremental Forever) are VMWare, NAS, Views, Physical, O365, Oracle, SQL, Amazon EC2.

*   Supported sources for CloudArchive Direct (with Dedup) are VMware, and NAS.



| Workflows | External Targets |     |
| --- | --- | --- |
| AWS C2S - S3 | AWS C2S - Glacier |
| --- | --- | --- |
| Enable/disable compression at external target level | No  | No  |
| Unregister external target | No  | No  |
| Manual management of encryption keys | No  | No  |
| Once enabled, going back to legacy archival | No  | No  |
| Perform search for CloudRetrieve operations | No  | No  |
| Download jobs (CloudRetrieve) | No  | No  |
| Download files/folders from archive | No  | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | No  | No  |
| Object level recovery to source | No  | No  |
| Recover files/folders to source | No  | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | No  | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | No  | No  |
| WORM Support | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  |
| Cohesity Data Movement (LCM) | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive direct without dedup- standard
{: #external_targets_for_cloudarchive_direct_without_dedup-_standard}

Supported sources for CloudArchive Direct (without Dedup) is NAS.

The option to enable CloudArchive Direct (native mode - no dedupe, no compression) from the NAS Protection Group page is deprecated in the 6.8 and above versions. If you want to enable the CloudArchive Direct native mode, then you must contact[IBM Cloud Support](../Support/ContactSupport.htm).


| Workflows | External Targets |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AWS - S3 | AWS - S3-IA | AWS - S3-Intelligent | AWS - S3-One-Zone | AWS Snowball Edge | AWS - Glacier | AWS - S3 Glacier Deep Archive |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Enable/disable deduplication at external target level | No  | No  | No  | No  | No  | No  | No  |
| Enable encryption at external target level | Yes | Yes | Yes | Yes | No  | No  | No  |
| Once enabled, disable encryption at external target level | No  | No  | No  | No  | No  | No  | No  |
| Enable/disable compression at external target level | Yes | Yes | Yes | Yes | No  | No  | No  |
| Delete external target | No  | No  | No  | No  | No  | No  | No  |
| Enable secure archive at external target level | Yes | Yes | Yes | Yes | No  | No  | No  |
| Once enabled, disable Secure Archive at external target level | No  | No  | No  | No  | No  | No  | No  |
| Perform search for CloudRetrieve operations | No  | No  | No  | No  | No  | No  | No  |
| Download jobs (CloudRetrieve) | No  | No  | No  | No  | No  | No  | No  |
| Download files/folders from archive | Yes | Yes | Yes | Yes | No  | No  | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | No  | No  | No  | No  | No  | No  | No  |
| Recover files/folders to source | Yes | Yes | Yes | Yes | No  | No  | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | No  | No  | No  | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive direct without dedup - gov
{: #external_targets_for_cloudarchive_direct_without_dedup_-_gov}

Supported sources for CloudArchive Direct (without Dedup) is NAS.

The option to enable CloudArchive Direct (native mode - no dedupe, no compression) from the NAS Protection Group page is deprecated in the 6.8 and above versions. If you want to enable the CloudArchive Direct native mode, then you must contact[IBM Cloud Support](../Support/ContactSupport.htm).


| Workflows | External Targets |     |
| --- | --- | --- |
| AWS Gov - S3 | AWS Gov - S3 Glacier Deep Archive |
| --- | --- | --- |
| Enable/disable deduplication at external target level | No  | No  |
| Enable encryption at external target level | No  | No  |
| Once enabled, disable encryption at external target level | No  | No  |
| Enable/disable compression at external target level | No  | No  |
| Delete external target | No  | No  |
| Enable secure archive at external target level | No  | No  |
| Once enabled, disable Secure Archive at external target level | No  | No  |
| Perform search for CloudRetrieve operations | No  | No  |
| Download jobs (CloudRetrieve) | No  | No  |
| Download files/folders from archive | No  | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | No  | No  |
| Object level recovery to source | No  | No  |
| Recover files/folders to source | No  | No  |
| Object level recovery to source | No  | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive direct without dedup - c2s
{: #external_targets_for_cloudarchive_direct_without_dedup_-_c2s}

Supported sources for CloudArchive Direct (without Dedup) is NAS.

The option to enable CloudArchive Direct (native mode - no dedupe, no compression) from the NAS Protection Group page is deprecated in the 6.8 and above versions. If you want to enable the CloudArchive Direct native mode, then you must contact[IBM Cloud Support](../Support/ContactSupport.htm).


| Workflows | External Targets |     |
| --- | --- | --- |
| AWS C2S - S3 | AWS C2S - Glacier |
| --- | --- | --- |
| Enable/disable deduplication at external target level | No  | No  |
| Enable encryption at external target level | No  | No  |
| Once enabled, disable encryption at external target level | No  | No  |
| Enable/disable compression at external target level | No  | No  |
| Delete external target | No  | No  |
| Enable secure archive at external target level | No  | No  |
| Once enabled, disable Secure Archive at external target level | No  | No  |
| Perform search for CloudRetrieve operations | No  | No  |
| Download jobs (CloudRetrieve) | No  | No  |
| Download files/folders from archive | No  | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | No  | No  |
| Object level recovery to source | No  | No  |
| Recover files/folders to source | No  | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | No  | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudtier - standard
{: #external_targets_for_cloudtier_-_standard}


| Workflows | External Targets |     |     |     |
| --- | --- | --- | --- | --- |
| AWS - S3 Standard | AWS - S3-Intelligent Standard | AWS S3-STS Standard | AWS Snowball Edge Standard |
| --- | --- | --- | --- | --- |
| Enable encryption at external target level | Yes | Yes | Yes | Yes |
| Once enabled, disable encryption at external target level | No  | No  | No  | No  |
| Unregister external target | Yes | Yes | Yes | Yes |
| Enable compression at external target level | Yes | Yes | Yes | Yes |
| Once enabled, disable compression at external target level | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudtier - gov
{: #external_targets_for_cloudtier_-_gov}


| Workflows | External Targets |     |     |
| --- | --- | --- | --- |
| AWS Gov - S3 | AWS - S3-Intelligent Gov | AWS S3-STS Gov |
| --- | --- | --- | --- |
| Enable encryption at external target level | Yes | Yes | Yes |
| Once enabled, disable encryption at external target level | No  | No  | No  |
| Unregister external target | Yes | Yes | Yes |
| Enable compression at external target level | Yes | Yes | Yes |
| Once enabled, disable compression at external target level | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudtier - c2s
{: #external_targets_for_cloudtier_-_c2s}


| Workflows | External Targets |     |
| --- | --- | --- |
| AWS C2S - S3 | AWS S3-STS C2S |
| --- | --- | --- |
| Enable encryption at external target level | Yes | Yes |
| Once enabled, disable encryption at external target level | No  | No  |
| Unregister external target | Yes | Yes |
| Enable compression at external target level | Yes | Yes |
| Once enabled, disable compression at external target level | No  | No  |
{: caption="" caption-side="bottom"}

## Microsoft azure external targets
{: #microsoft_azure_external_targets}

### External targets for cloudarchive (with periodic full)
{: #external_targets_for_cloudarchive_with_periodic_full}


| Workflows | External Targets |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| Azure Hot Blob | Azure Cool Blob | Azure Archive | Azure Gov Hot Blob | Azure Gov Cool Blob | Azure Gov Archive Blob |
| --- | --- | --- | --- | --- | --- | --- |
| Enable/disable deduplication at external target level | Yes | Yes | Yes | Yes | Yes | Yes |
| Manual management of encryption keys | Yes | Yes | Yes | Yes | Yes | Yes |
| Enable/disable compression at external target level | Yes | Yes | Yes | Yes | Yes | Yes |
| Unregister external target | Yes | Yes | Yes | Yes | Yes | Yes |
| Perform search for CloudRetrieve operations | Yes | Yes | Yes | Yes | Yes | Yes |
| Download jobs (CloudRetrieve) | Yes | Yes | Yes | Yes | Yes | Yes |
| Download files/folders from archive | Yes | Yes | No  | Yes | Yes | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | Yes | No  | Yes | Yes | No  |
| Object level recovery to source | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover files/folders to source | Yes | Yes | No  | Yes | Yes | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | Yes | Yes | Yes | Yes | Yes | Yes |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | Yes | No  | Yes | Yes | No  |
| WORM Support | Yes | Yes | Yes | No  | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  | No  | No  | No  | No  |
| Cohesity Data Movement (LCM) | No  | No  | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive (incremental forever) and cloudarchive direct (with dedup)
{: #external_targets_for_cloudarchive_incremental_forever_and_cloudarchive_direct_with_dedup}

*   Supported sources for CloudArchive (Incremental Forever) are VMWare, NAS, Views, Physical, O365, Oracle, SQL, Amazon EC2.

*   Supported sources for CloudArchive Direct (with Dedup) are VMware, and NAS.



| Workflows | External Targets |     |     |     |
| --- | --- | --- | --- | --- |
| Azure Hot Blob | Azure Cool Blob | Azure Archive | Azure Gov |
| --- | --- | --- | --- | --- |
| Enable/disable compression at external target level | Yes | Yes | Yes | No  |
| Unregister external target | Yes | Yes | Yes | No  |
| Manual management of encryption keys | Yes | Yes | Yes | No  |
| Once enabled, going back to legacy archival | No  | No  | No  | No  |
| Perform search for CloudRetrieve operations | Yes | Yes | Yes | No  |
| Download jobs (CloudRetrieve) | Yes | Yes | Yes | No  |
| Download files/folders from archive | Yes | Yes | Yes | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | Yes | Yes | No  |
| Object level recovery to source | Yes | Yes | Yes | No  |
| Recover files/folders to source | Yes | Yes | Yes | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | Yes | Yes | No  | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | Yes | Yes | No  |
| WORM Support | No  | No  | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  | No  | No  |
| Cohesity Data Movement (LCM) | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive direct without dedup
{: #external_targets_for_cloudarchive_direct_without_dedup}

Supported sources for CloudArchive Direct (without Dedup) is NAS.

The option to enable CloudArchive Direct (native mode - no dedupe, no compression) from the NAS Protection Group page is deprecated in the 6.8 and above versions. If you want to enable the CloudArchive Direct native mode, then you must contact[IBM Cloud Support](../Support/ContactSupport.htm).


| Workflows | External Targets |     |     |     |
| --- | --- | --- | --- | --- |
| Azure Hot Blob | Azure Cool Blob | Azure Archive | Azure Gov |
| --- | --- | --- | --- | --- |
| Enable/disable deduplication at external target level | No  | No  | No  | No  |
| Enable encryption at external target level | Yes | Yes | No  | No  |
| Once enabled, disable encryption at external target level | No  | No  | No  | No  |
| Enable/disable compression at external target level | Yes | Yes | No  | No  |
| Delete external target | No  | No  | No  | No  |
| Enable secure archive at external target level | Yes | Yes | No  | No  |
| Once enabled, disable Secure Archive at external target level | No  | No  | No  | No  |
| Perform search for CloudRetrieve operations | No  | No  | No  | No  |
| Download jobs (CloudRetrieve) | No  | No  | No  | No  |
| Download files/folders from archive | Yes | Yes | No  | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | No  | No  | No  | No  |
| Recover files/folders to source | Yes | Yes | No  | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudtier - standard
{: #external_targets_for_cloudtier_-_standard}


| Workflows | External Targets |
| --- | --- |
| Azure Hot Blob - Standard |
| --- | --- |
| Enable encryption at external target level | Yes |
| Once enabled, disable encryption at external target level | No  |
| Unregister external target | Yes |
| Enable compression at external target level | Yes |
| Once enabled, disable compression at external target level | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudtier - gov
{: #external_targets_for_cloudtier_-_gov}


| Workflows | External Targets |
| --- | --- |
| Azure Hot Blob - Gov |
| --- | --- |
| Enable encryption at external target level | Yes |
| Once enabled, disable encryption at external target level | No  |
| Unregister external target | Yes |
| Enable compression at external target level | Yes |
| Once enabled, disable compression at external target level | No  |
{: caption="" caption-side="bottom"}

## Gcp external targets
{: #gcp_external_targets}

### External targets for cloudarchive (with periodic full)
{: #external_targets_for_cloudarchive_with_periodic_full}


| Workflows | External Targets |     |     |
| --- | --- | --- | --- |
| Google - Standard | Google - Nearline | Google - Coldline |
| --- | --- | --- | --- |
| Enable/disable deduplication at external target level | Yes | Yes | Yes |
| Manual management of encryption keys | Yes | Yes | Yes |
| Enable/disable compression at external target level | Yes | Yes | Yes |
| Unregister external target | Yes | Yes | Yes |
| Perform search for CloudRetrieve operations | Yes | Yes | Yes |
| Download jobs (CloudRetrieve) | Yes | Yes | Yes |
| Download files/folders from archive | Yes | Yes | Yes |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | Yes | Yes |
| Object level recovery to source | Yes | Yes | Yes |
| Recover files/folders to source | Yes | Yes | Yes |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | Yes | Yes | Yes |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | Yes | Yes |
| WORM Support | No  | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  | No  |
| Cohesity Data Movement (LCM) | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive (incremental forever) and cloudarchive direct (with dedup)
{: #external_targets_for_cloudarchive_incremental_forever_and_cloudarchive_direct_with_dedup}

*   Supported sources for CloudArchive (Incremental Forever) are VMWare, NAS, Views, Physical, O365, Oracle, SQL, Amazon EC2.

*   Supported sources for CloudArchive Direct (with Dedup) are VMware, and NAS.



| Workflows | External Targets |     |     |
| --- | --- | --- | --- |
| Google - Nearline | Google-Standard | Google - Coldline |
| --- | --- | --- | --- |
| Enable/disable compression at external target level | No  | No  | No  |
| Unregister external target | No  | No  | No  |
| Manual management of encryption keys | No  | No  | No  |
| Once enabled, going back to legacy archival | No  | No  | No  |
| Perform search for CloudRetrieve operations | No  | No  | No  |
| Download jobs (CloudRetrieve) | No  | No  | No  |
| Object level recovery to source | No  | No  | No  |
| Download files/folders from archive | No  | No  | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | No  | No  | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | No  | No  | No  |
| Recover files/folders to source | No  | No  | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | No  | No  | No  |
| WORM Support | No  | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  | No  |
| Cohesity Data Movement (LCM) | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive direct without dedup
{: #external_targets_for_cloudarchive_direct_without_dedup}

Supported sources for CloudArchive Direct (without Dedup) is NAS.

The option to enable CloudArchive Direct (native mode - no dedupe, no compression) from the NAS Protection Group page is deprecated in the 6.8 and above versions. If you want to enable the CloudArchive Direct native mode, then you must contact[IBM Cloud Support](../Support/ContactSupport.htm).


| Workflows | External Targets |     |     |
| --- | --- | --- | --- |
| Google - Nearline | Google-Standard | Google - Coldline |
| --- | --- | --- | --- |
| Enable/disable deduplication at external target level | No  | No  | No  |
| Enable encryption at external target level | Yes | Yes | Yes |
| Once enabled, disable encryption at external target level | No  | No  | No  |
| Enable/disable compression at external target level | Yes | Yes | Yes |
| Delete external target | No  | No  | No  |
| Enable secure archive at external target level | Yes | Yes | Yes |
| Once enabled, disable Secure Archive at external target level | No  | No  | No  |
| Perform search for CloudRetrieve operations | No  | No  | No  |
| Download jobs (CloudRetrieve) | No  | No  | No  |
| Download files/folders from archive | Yes | Yes | Yes |
| Download files/folders from archive for downloaded CloudRetrieve jobs | No  | No  | No  |
| Recover files/folders to source | Yes | Yes | Yes |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | No  | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudtier
{: #external_targets_for_cloudtier}


| Workflows | External Targets |
| --- | --- |
| Google Standard |
| --- | --- |
| Enable encryption at external target level | Yes |
| Once enabled, disable encryption at external target level | No  |
| Unregister external target | Yes |
| Enable compression at external target level | Yes |
| Once enabled, disable compression at external target level | No  |
{: caption="" caption-side="bottom"}

## Oracle cloud external targets
{: #oracle_cloud_external_targets}

### External targets for cloudarchive (with periodic full)
{: #external_targets_for_cloudarchive_with_periodic_full}


| Workflows | External Targets |     |
| --- | --- | --- |
| Oracle Standard | Oracle Archive |
| --- | --- | --- |
| Enable/disable deduplication at external target level | Yes | Yes |
| Manual management of encryption keys | Yes | Yes |
| Enable/disable compression at external target level | Yes | Yes |
| Unregister external target | Yes | Yes |
| Perform search for CloudRetrieve operations | Yes | Yes |
| Download jobs (CloudRetrieve) | Yes | Yes |
| Download files/folders from archive | Yes | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | No  |
| Object level recovery to source | Yes | Yes |
| Recover files/folders to source | Yes | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | Yes | Yes |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | No  |
| WORM Support | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  |
| Cohesity Data Movement (LCM) | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive (incremental forever) and cloudarchive direct (with dedup)
{: #external_targets_for_cloudarchive_incremental_forever_and_cloudarchive_direct_with_dedup}

*   Supported sources for CloudArchive (Incremental Forever) are VMWare, NAS, Views, Physical, O365, Oracle, SQL, Amazon EC2.

*   Supported sources for CloudArchive Direct (with Dedup) are VMware, and NAS.



| Workflows | External Targets |     |
| --- | --- | --- |
| Oracle Standard | Oracle Archive |
| --- | --- | --- |
| Enable/disable compression at external target level | No  | No  |
| Unregister external target | No  | No  |
| Manual management of encryption keys | No  | No  |
| Once enabled, going back to legacy archival | No  | No  |
| Perform search for CloudRetrieve operations | No  | No  |
| Download jobs (CloudRetrieve) | No  | No  |
| Download files/folders from archive | No  | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | No  | No  |
| Object level recovery to source | No  | No  |
| Recover files/folders to source | No  | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | No  | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | No  | No  |
| WORM Support | No  | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  |
| Cohesity Data Movement (LCM) | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudarchive direct without dedup
{: #external_targets_for_cloudarchive_direct_without_dedup}

Supported sources for CloudArchive Direct (without Dedup) is NAS.

The option to enable CloudArchive Direct (native mode - no dedupe, no compression) from the NAS Protection Group page is deprecated in the 6.8 and above versions. If you want to enable the CloudArchive Direct native mode, then you must contact[IBM Cloud Support](../Support/ContactSupport.htm).


| Workflows | External Targets |     |
| --- | --- | --- |
| Oracle Standard | Oracle Archive |
| --- | --- | --- |
| Enable/disable deduplication at external target level | No  | No  |
| Enable encryption at external target level | Yes | No  |
| Once enabled, disable encryption at external target level | No  | No  |
| Enable/disable compression at external target level | Yes | No  |
| Delete external target | No  | No  |
| Enable secure archive at external target level | Yes | No  |
| Once enabled, disable Secure Archive at external target level | No  | No  |
| Perform search for CloudRetrieve operations | No  | No  |
| Download jobs (CloudRetrieve) | No  | No  |
| Download files/folders from archive | Yes | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | No  | No  |
| Recover files/folders to source | Yes | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudtier
{: #external_targets_for_cloudtier}


| Workflows | External Targets |
| --- | --- |
| Oracle - Object Storage |
| --- | --- |
| Enable encryption at external target level | Yes |
| Once enabled, disable encryption at external target level | No  |
| Unregister external target | Yes |
| Enable compression at external target level | Yes |
| Once enabled, disable compression at external target level | No  |
{: caption="" caption-side="bottom"}

## S3 - compatible, nas, tape external targets
{: #s3_-_compatible_nas_tape_external_targets}

### External targets for cloudarchive (with periodic full) - s3-compatible, nas, tape
{: #external_targets_for_cloudarchive_with_periodic_full_-_s3-compatible_nas_tape}

File-level recovery of objects archived to QStar tape is an Early Access feature.


| Workflows | External Targets |     |     |     |
| --- | --- | --- | --- | --- |
| NAS | QStar Tape | S3 - Compatible (E.g., Cohesity) | Tape-Based S3 - Compatible (E.g., IBM Storage Protect) \* |
| --- | --- | --- | --- | --- |
| Enable/disable deduplication at external target level | Yes | No  | Yes | No  |
| Manual management of encryption keys | Yes | Yes | Yes | Yes |
| Enable/disable compression at external target level | Yes | Yes | Yes | Yes |
| Unregister external target | Yes | Yes | Yes | Yes |
| Perform search for CloudRetrieve operations | Yes | No  | Yes | No  |
| Download jobs (CloudRetrieve) | Yes | No  | Yes | No  |
| Download files/folders from archive | Yes | No  | Yes | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | No  | Yes | No  |
| Object level recovery to source | Yes | Yes | Yes | Yes |
| Recover files/folders to source | Yes | Yes | Yes | No  |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | Yes | No  | Yes | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | No  | Yes | No  |
| WORM Support | No  | No  | Yes | No  |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  | No  | No  |
| {{site.data.keyword.baas_full_notm}} Data Movement (LCM) | No  | No  | No  | No  |
{: caption="" caption-side="bottom"}

\*The archival format is _Always Full_ with compression enabled by default.

### External targets for cloudarchive (incremental forever) and cloudarchive direct (with dedup) - s3-compatible, nas
{: #external_targets_for_cloudarchive_incremental_forever_and_cloudarchive_direct_with_dedup_-_s3-compatible_nas}

Supported sources for CloudArchive Direct with Dedup are VMware, and NAS.


| Workflows | External Targets |     |
| --- | --- | --- |
| NAS | S3 - Compatible (E.g., Cohesity) \* |
| --- | --- | --- |
| Enable/disable compression at external target level | Yes | Yes |
| Unregister external target | Yes | Yes |
| Manual management of encryption keys | Yes | Yes |
| Once enabled, going back to legacy archival | No  | No  |
| Perform search for CloudRetrieve operations | Yes | Yes |
| Download jobs (CloudRetrieve) | Yes | Yes |
| Download files/folders from archive | Yes | Yes |
| Download files/folders from archive for downloaded CloudRetrieve jobs | Yes | Yes |
| Object level recovery to source | Yes | Yes |
| Recover files/folders to source | Yes | Yes |
| Object level recovery to source from archive for downloaded CloudRetrieve jobs | Yes | Yes |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | Yes | Yes |
| WORM Support | No  | Yes |
| Cloud Vendor Provided Life Cycle Management (LCM) | No  | No  |
| {{site.data.keyword.baas_full_notm}} Data Movement (LCM) | No  | No  |
{: caption="" caption-side="bottom"}

\* - Tape Based S3-Compatible targets are not supported for CloudArchive (Incremental Forever) and CloudArchive Direct (with Dedup).

### External targets for cloudarchive direct without dedup - s3 - compatible, nas, tape
{: #external_targets_for_cloudarchive_direct_without_dedup_-_s3_-_compatible_nas_tape}

Supported sources for CloudArchive Direct (without Dedup) is NAS.

The option to enable CloudArchive Direct (native mode - no dedupe, no compression) from the NAS Protection Group page is deprecated in the 6.8 and above versions. If you want to enable the CloudArchive Direct native mode, then you must contact[IBM Cloud Support](../Support/ContactSupport.htm).


| Workflows | External Targets |     |
| --- | --- | --- |
| NAS | QStar Tape |
| --- | --- | --- |
| Enable/disable deduplication at external target level | No  | No  |
| Enable encryption at external target level | Yes | No  |
| Once enabled, disable encryption at external target level | No  | No  |
| Enable/disable compression at external target level | Yes | No  |
| Delete external target | No  | No  |
| Enable secure archive at external target level | Yes | No  |
| Once enabled, disable Secure Archive at external target level | No  | No  |
| Perform search for CloudRetrieve operations | No  | No  |
| Download jobs (CloudRetrieve) | No  | No  |
| Download files/folders from archive | Yes | No  |
| Download files/folders from archive for downloaded CloudRetrieve jobs | No  | No  |
| Recover files/folders to source | Yes | No  |
| Recover files/folders to source from archive for downloaded CloudRetrieve jobs | No  | No  |
{: caption="" caption-side="bottom"}

### External targets for cloudtier - s3-compatible, nas, tape
{: #external_targets_for_cloudtier_-_s3-compatible_nas_tape}


| Workflows | External Targets |     |
| --- | --- | --- |
| S3 - Compatible (E.g., Cohesity) \* | Netapp - StorageGrid |
| --- | --- | --- |
| Enable encryption at external target level | Yes | Yes |
| Once enabled, disable encryption at external target level | No  | No  |
| Unregister external target | Yes | Yes |
| Enable compression at external target level | Yes | Yes |
| Once enabled, disable compression at external target level | No  | No  |
{: caption="" caption-side="bottom"}

\* - Tape Based S3-Compatible targets are not supported for Cloud Tiering.

## Nas tiering workflows
{: #nas_tiering_workflows}

### Tiering workflows for nfs
{: #tiering_workflows_for_nfs}


| Categories | Workflows | External Targets |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- | --- |
| NetApp - NFS | Generic NAS - NFS | Isilon - NFS | Pure FlashBlade - NFS | GPFS - NFS | Elastifile - NFS |
| --- | --- | --- | --- | --- | --- | --- | --- |
| NAS TIERING | Down-Tiering without leaving anything at source | Supported (Only as generic NAS) | Yes | Yes | No  | No  | No  |
| Downtiering with leaving the symlink behind at the source | Supported (Only as generic NAS) | Yes | Yes | No  | No  | No  |
| Orphan data delete | Supported (Only as generic NAS) | Yes | Yes | No  | No  | No  |
| Capacity Planner | Supported (Only as generic NAS) | Yes | Yes | No  | No  | No  |
| Tiering Audit Log | Supported (Only as generic NAS) | Yes | Yes | No  | No  | No  |
| Bucket Customization | Supported (Only as generic NAS) | Yes | Yes | No  | No  | No  |
{: caption="" caption-side="bottom"}

### Tiering workflows for smb
{: #tiering_workflows_for_smb}


| Categories | Workflows | External Targets |     |     |     |
| --- | --- | --- | --- | --- | --- |
| NetApp - SMB | Generic NAS - SMB | Isilon - SMB | Pure FlashBlade - SMB |
| --- | --- | --- | --- | --- | --- |
| NAS TIERING | Down-Tiering without leaving anything at source | Supported (Only as generic NAS) | Yes | Yes | No  |
| Downtiering with leaving the symlink behind at the source | No  | Yes | Yes | No  |
| Orphan data delete | No  | Yes | Yes | No  |
| Capacity Planner | Supported (Only as generic NAS) | Yes | Yes | No  |
| Tiering Audit Log | Supported (Only as generic NAS) | Yes | Yes | No  |
| Bucket Customization | Supported (Only as generic NAS) | Yes | Yes | No  |
{: caption="" caption-side="bottom"}
