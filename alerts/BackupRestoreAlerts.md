---

copyright:
  years: 2024
lastupdated: "2024-9-26"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Backup and Restore Alerts

## CE02809065 PossibleMetadataInconsistency

Verification operation found possible metadata inconsistency.

Contact [IBM Cloud Support](../../../Support/ContactSupport.htm).

Not Applicable

Critical

28800 seconds

1.3.6.1.4.1.47421.0.388

All

## CE00610001 ProtectionGroupSucceeded

Backup run completed successfully.

This alert is triggered if the Protection Group Run was successful.

No action required.

Not Applicable

Informational

3600 seconds

Not Applicable

All

## CE00610002 ProtectionGroupFailed

The backup run of the protection group failed.

This alert is triggered if a Protection Group Run fails. A protection run can fail for any of the following reasons:

*   There is an issue with primary environment such as a removed VM or a Snapshot failure.
*   The primary storage is full. (The primary storage contains the Objects that are backed up by the {{site.data.keyword.baas_full_notm}}.) 
*   The Backup agent is unreachable while attempting to back up physical servers.
*   The storage on the {{site.data.keyword.baas_full_notm}} is full.

In the {{site.data.keyword.baas_full_notm}} Console, drill-down on a protection run to get details about the failure including the objects (such as VMs, databases, physical servers etc) that were not backed up. Read the Cohesity and VMware documentation for a resolution. If the resolution is not found, see the [Alert: CE00610002 BackupJobFailed](/docs/allowlist/backup-recovery?topic=Alert-CE00610002-BackupJobFailed) Knowledge Base article.

 

| Job Type | Error Codes | Description |
| --- | --- | --- |
| All | `kNonExistent` | If the parent entity cannot be found in the registered sources or there are no eligible sources for the protection run, an alert with the error code is raised. |
| `kInvalid` | The alert with this error code is raised in the following scenarios: When the parameters for the host entity's connector cannot be retrieved. When the retention policy is missing from the protection policy. When the backup parameters for the Sfdc environment are not configured. | |
| `kInternalError` | Failed due to error injection. Should not happen in release builds. | |
| `kDisabled` | The alert with this error code is raised when the Storage Array Snapshots are disabled. | |
| `kSchedulingError` | The alert with this error code was raised when the scheduled protection run was supposed to start earlier, but it couldn't get scheduled. | |
| `kRetry` | For Universal Data Adapter jobs that require an AWS S3 view backup, the Iris user config needed to fetch the S3 details was not found. | |
| `kRejected` | The alert with this error code is raised when a job exceeds its object limit and needs to be split across multiple jobs. | |

Critical

86400 seconds

1.3.6.1.4.1.47421.0.108

All

## CE00610003 ProtectionGroupSlaViolated

The SLA of the protection run is violated.

This alert is triggered if the amount of time that a protection run takes to complete exceeds the amount of time specified in the SLA. This alert can also be triggered due to a failed protection run. A protection run may take longer than the specified SLA for the following reasons:

*   If the primary storage is slow.
*   The network is slow.
*   The {{site.data.keyword.baas_full_notm}} is overloaded.
*   You specified SLA that is too short.

Investigate why the protection run took longer than the specified SLA. If appropriate, adjust the time period specified in the SLA. If the alert is triggered due to a failed protection run, you can override the default behavior and disable the alert when there is a protection run failure. For more information, see the [ProtectionGroupSlaViolated alert triggered for backup jobs which has failed runs](/docs/allowlist/backup-recovery?topic=COH-891920697) Knowledge Base article.

Not Applicable

Warning

86400 seconds

1.3.6.1.4.1.47421.0.121

All

## CE00610004 ProtectionGroupCancelledBlackoutWindow

This alert is triggered if a protection run aborts because the protection run started executing during a blackout period specified for the job and the Abort Running Jobs during Blackouts toggle is set on for this Protection Group.

By default this toggle is set off which means after a protection run starts, it continues to execute even when a black period specified for this job starts. However, a new protection run will not start during a blackout period.

No action required.

Warning

86400 seconds

Not Applicable

All

## CE00610005 ProtectedObjectFailed

The backup failed .

This alert is raised when the {{site.data.keyword.baas_full_notm}} detects that an object (such as a VM) failed to be backed up during a Protection Group Run. One alert is raised for each object (such as a VM) that failed to be backed up. For instructions on how to enable this alert, contact [IBM Cloud Support](../../../Support/ContactSupport.htm). A protection run can fail to back up an object for any of the following reasons:

*   There is an issue with primary environment such as a removed VM or a Snapshot failure.
*   The primary storage is full. (The primary storage contains the Objects that are backed up by the {{site.data.keyword.baas_full_notm}}.) 
*   The Backup agent is unreachable while attempting to back up physical servers.
*   The storage on the {{site.data.keyword.baas_full_notm}} is full.

See the [CE00610005 | BackupRestore - BackupObjectFailed](/docs/allowlist/backup-recovery?topic=Alert-CE00610005-BackupObjectFailed) Knowledge Base article for a resolution.

 

| Job Type | Error Codes | Description |
| --- | --- | --- |
| `kVMware` | `kNonExistent` | The alert with the error code is raised in the following scenarios: When the virtual machines protected by a VM protection group are not found in the VM hierarchy. Due to decommissioned or repaved VM. |
| `kVSphereError` | The alert with this error code is raised in the following scenarios: When a timeout occurs while connecting to the vCenter from the {{site.data.keyword.baas_full_notm}}. Pings to the vCenter server fail due to IP connectivity issues. The account used to register the source doesn't have proper permissions. | |
| `kVixError` | The alert with the error code is raised in the following scenarios: The Cohesity node cannot resolve the DNS name of the ESX host. The Cohesity node cannot connect to the ESX host over port 902. When the Cohesity node connecting to the VMDK to read the changed blocks through the ESX host failed. | |
| `kTransportError` | The alert with the error code is raised when there is a communication issue between the {{site.data.keyword.baas_full_notm}} and the Backup agent on the Host. | |
| `kInvalidRequest` | The alert with the error code is raised in the following scenarios: The source file system inclusion list contains duplicate paths. -   The metadata or input file for the File Directive backup or User-based backup has the same file or folder path listed as duplicate. The exclude path is not a child path of the include path. The File Directive backup or User-based backup does not specify the metadata or input file when running the backup scripts or specifies an invalid or non-existent file name. When the inclusion path is not found, the protection run fails. During a protection run, the {{site.data.keyword.baas_full_notm}} requests the source server to retrieve a list of available volumes. The protection run fails when a path that has been explicitly included is missing. | |
| `kPhysicalFiles` | `kTransportError` | The alert with the error code is raised when there is a communication issue between the {{site.data.keyword.baas_full_notm}} and the Backup agent on the Host. |
| `kInvalidRequest` | The alert with the error code is raised in the following scenarios: The source file system inclusion list contains duplicate paths. -   The metadata or input file for the File Directive backup or User-based backup has the same file or folder path listed as duplicate. The exclude path is not a child path of the include path. The File Directive backup or User-based backup does not specify the metadata or input file when running the backup scripts or specifies an invalid or non-existent file name. When the inclusion path is not found, the protection run fails. During a protection run, the {{site.data.keyword.baas_full_notm}} requests the source server to retrieve a list of available volumes. The protection run fails when a path that has been explicitly included is missing. | |
| `kNonExistent` | The alert with the error code is raised when metadata or input file part of the File Directive backup or User-based Backup is nonexistent on the source server. | |
| `kFsError` | The alert with the error code is raised in the following scenarios: Microsoft Windows permissions issue. The credentials were changed recently. The domain account used to register the host with the {{site.data.keyword.baas_full_notm}} is not in the domain(s) to which the cluster has been joined. The domain account used to register the host with the {{site.data.keyword.baas_full_notm}} is not in a domain that is trusted by the domain(s) to which the cluster has been joined. The domain account used to register the host with the {{site.data.keyword.baas_full_notm}} is in a domain that is denied in the cluster's Active Directory configuration or trusted. | |
| `kCassandra` | `kTransportError` | The alert with the error code is raised when there is a communication issue between the {{site.data.keyword.baas_full_notm}} and the Backup agent on the Host. |
| `kImanisError` | The alert with the error code is raised in the following scenarios: Source connectivity issue. The keyspace being backed up is not present in the data center configuration on the Cassandra cluster. In a Cassandra cluster, keyspaces and tables are spread across nodes; and are created such that data would be spread across nodes based on the configured replication factor. If for instance RF is 3, and snapshot creation activity fails more than three times, backup would be expected to fail. Review yarn application log to determine why snapshot creation failed. | |
| `kNetapp` | `kInvalidRequest` | The alert with the error code is raised in the following scenarios: -   The source file system inclusion list contains duplicate paths. The metadata or input file for the File Directive backup or User-based backup has the same file or folder path listed as duplicate. The exclude path is not a child path of the include path. The File Directive backup or User-based backup does not specify the metadata or input file when running the backup scripts or specifies an invalid or non-existent file name. When the inclusion path is not found, the protection run fails. During a protection run, the {{site.data.keyword.baas_full_notm}} requests the source server to retrieve a list of available volumes. The protection run fails when a path that has been explicitly included is missing. |

Critical

86400 seconds

1.3.6.1.4.1.47421.0.109

All

## CE00610006 PolicyFieldsDeprecated

Policy settings have been deprecated.

This alert is raised after the {{site.data.keyword.baas_full_notm}}is upgraded from a 4.x release to a 5.x release and the cluster detects that some policy settings used in the current policies on the cluster have been deprecated.

Open the listed policy in the {{site.data.keyword.baas_full_notm}} Console, verify the current settings and make any necessary adjustments. See the [ALERT: CE00610006 POLICYFIELDSDEPRECATED](/docs/allowlist/backup-recovery?topic=CE00610006) Knowledge Base article.

Not Applicable

Warning

86400 seconds

Not Applicable

All

## CE00610007 ProtectedObjectCancelledBlackoutWindow

Backup was cancelled.

This alert is triggered when the back up of an object is canceled due to a blackout window. The Protection Group uses a policy that defines a blackout window and the job is configured to abort if it runs during the blackout window.

No action required.

Not Applicable

Warning

86400 seconds

1.3.6.1.4.1.47421.0.124

All

## CE00610009 ProtectedObjectSlaViolated

SLA in backup run violated.

This alert is triggered when the service level agreement violation (SLA) occurs for an individual object in a Protection Group run. A Protection Group run may take longer than the specified SLA for the following reasons:

*   If the primary storage is slow.
*   The network is slow.
*   The {{site.data.keyword.baas_full_notm}} is overloaded.
*   You specified SLA that is too short.

Investigate why the Protection Group run took longer than the specified SLA. If appropriate, adjust the time period specified in the SLA.

Not Applicable

Warning

86400 seconds

1.3.6.1.4.1.47421.0.130

All

## CE00610011 ProtectionGroupProgressingSlowly

Protection group is progressing slowly.

This alert is triggered when throttling is observed from Microsoft Office 365 during backups of OneDrives and SharePoint Online. The alert triggered once a day for a Protection Group. This alert is caused when the backups exceed the download or upload limits defined in the Microsoft Office 365 source.

If you repeatedly observe this issue, contact [Microsoft Support](https://support.microsoft.com/).

Not Applicable

Informational

86400 seconds

1.3.6.1.4.1.47421.0.139

All

## CE00610014 VMMigrationIdentified

Migrated VM(s) identified on the vCenter.

This alert is triggered when the {{site.data.keyword.baas_full_notm}} identifies a VM in a vCenter that was earlier part of another vCenter registered on the {{site.data.keyword.baas_full_notm}}.

No action is required if the migrated VMs are mentioned in the alert. If not, contact [IBM Cloud Support](../../../Support/ContactSupport.htm).

Not Applicable

Critical

86400 seconds

1.3.6.1.4.1.47421.0.255

All

## CE00608002 MissingVMBackup

Missing VM backup.

This Critical Alert is triggered when a descriptor VMDK file has been backed up but the corresponding flat VMDK file is missing from the backup.

No action is required.

Not Applicable

Critical

3600 seconds

Not Applicable

All

## CE00608003 VMCrackingSkipped

VM contents not indexed.

This alert is triggered when the {{site.data.keyword.baas_full_notm}} detects 5 consecutive unsuccessful attempts to index a VM. This alert can be caused by the following conditions:

*   The {{site.data.keyword.baas_full_notm}} is not able to mount the VMDK.
*   The VM Snapshot has an issue.

No action is required.

Not Applicable

Warning

3600 seconds

Not Applicable

All

## CE00610013 ContinuousDataProtectionDisabled

CDP is disabled for VM.

This alert is triggered when CDP is disabled for an entity in a Protection Group. This alert can be caused when an entity cannot be protected using CDP due to the following reasons:

*   CDP is not supported for VMware VMs with ESXi host version less than 6.5.
*   The source of the VM is an ESXi host and not a VMware vCenter.
    
*   The VM is present on an ESXi host, which is not part of an ESXi cluster.
    
*   The VM has physical RDM disks.
    
*   Another {{site.data.keyword.baas_full_notm}} is managing the ESXi cluster on which the VMware VM is running.
    

Fix the validation errors related to the source provided in the alert information. For other errors, contact [IBM Cloud Support](../../../Support/ContactSupport.htm).

Not Applicable

Critical

86400 seconds

232

All

## CE00610017 ProtectionPolicyDeleted

A Protection Policy was deleted.

The alert is triggered when you delete a Protection Policy.

No action required. Using this alert, you can validate if a valid user deleted the Protection Policy.

Not Applicable

Warning

86400 seconds

1.3.6.1.4.1.47421.0.272

All

## CE00610018 ProtectionGroupDeleted

A Protection Group was deleted.

The alert is triggered when you delete a Protection Group.

No action required. Using this alert, you can validate if a valid user deleted the Protection Group.

Not Applicable

Warning

86400 seconds

1.3.6.1.4.1.47421.0.273

All

## CE00610019 PolicyDatalockChanged

The DataLock was changed in a Protection Policy.

The alert is triggered if you enable or disable DataLock for a Protection Policy.

No action required. Using this alert, you can validate if the DataLock was enabled or disabled by a valid user.

Not Applicable

Informational

86400 seconds

1.3.6.1.4.1.47421.0.274

All

## CE00610020 PolicyDatalockDurationChanged

The DataLock retention in the Protection Policy was changed.

The alert is triggered when you change DataLock duration for a Protection Policy.

No action required. Using this alert, you can validate if a valid user modified the DataLock configuration.

Not Applicable

Informational

86400 seconds

1.3.6.1.4.1.47421.0.275

All

## CE00610025 ContinuousDataProtectionAlert

Alert is raised for CDP VM.

This alert is triggered when a user action is required for CDP protected entity. This alert can be caused when a user action is required in one of the following conditions for CDP protected entity:

*   When a user manually manages VM Storage Policy
    
*   When a user performs Pause, Deactivation or Delete operation on a Protection group
    

Perform the required action mentioned in the alert. For more details, contact [IBM Cloud Support](../../../Support/ContactSupport.htm).

Not Applicable

Critical

86400 seconds

1.3.6.1.4.1.47421.0.296

All

## CE00610026 DeferSchedule

Deferred schedule for to resolve conflicts among multiple schedules.

The alert is triggered when there are scheduling conflicts amongst multiple schedules. For example, when multiple schedules are defined in a Protection Policy that can run simultaneously, a specific priority order is applied based on backup type, retention, etc. The alert lets you know that a specific schedule with given retention has been deferred.

No action required.

Not Applicable

Informational

3600 seconds

1.3.6.1.4.1.47421.0.324

All

## CE00610024 SourceConnectivityFailure

Source cannot be reached.

This alert is triggered when the {{site.data.keyword.baas_full_notm}} detects that it is no longer connected with the vCenter. The {{site.data.keyword.baas_full_notm}} failed to connect with the vCenter due to one of the following conditions:

*   The user credentials of the vCenter have been updated recently but are not in sync with the {{site.data.keyword.baas_full_notm}}’s vCenter configuration.
    
*   The vCenter is not reachable.
    

Do one of the following:

*   If you get an “incorrect username and password” alert, update the current vCenter credentials in the {{site.data.keyword.baas_full_notm}}.
    
*   If you get a “timeout” or “not reachable” alert, try the following to ensure the vCenter is accessible by the {{site.data.keyword.baas_full_notm}}:
    
    *   Validate network connectivity between the {{site.data.keyword.baas_full_notm}} and the vCenter.
        
    *   Ensure the vCenter Server is running.
        
    *   Diagnose the error using the vCenter logs.
        

Not Applicable

Critical

7200 seconds

1.3.6.1.4.1.47421.0.292

All

## CE00610030 ProtectionServiceNotInitialized

Protection Service is not initialized.

This alert is triggered when the Protection service is not initialized for more than 60 minutes.

The Protection service may not initialize on time because the master node is unresponsive.

Contact [IBM Cloud Support](../../../Support/ContactSupport.htm).

Not Applicable

Warning

3600 seconds

1.3.6.1.4.1.47421.0.346

All

## CE00610027 ObjectDeletionRejected

Could not delete objects in the run before run expiry. They will be deleted when the run snapshot is expired.

This alert is triggered when the user deletes a specific snapshot for an object in a Protection Group instead of deleting the entire view.

The deletion is rejected because the Protection Group view is marked immutable and therefore individual object deletion can not be performed.

Evaluate if the user can delete the entire view instead of deleting individual snapshots.

Warning

3600 seconds

1.3.6.1.4.1.47421.0.329

All

## CE00610021 ProtectionPolicyModified

A Protection Policy was modified.

This alert is triggered when a Protection Policy is modified. The modification might include any changes apart from DataLock-related changes in the policy.

No action is required.

Not Applicable

Informational

86400 seconds

1.3.6.1.4.1.47421.0.276

All

## CE00610022 ProtectionGroupModified

A Protection Group was modified.

This alert is triggered when a Protection Group is modified. The modification might include changing the Protection Group name, adding or deleting any entities in the Protection Group, etc.

No action is required.

Not Applicable

Informational

86400 seconds

1.3.6.1.4.1.47421.0.277

All

## CE00610023 ProtectionRunModified

The Protection run in a Protection Group was modified.

This alert is triggered when a Protection run is modified. The modification might include deleting a Protection run, enabling a legal hold, etc.

No action is required.

Not Applicable

Informational

86400 seconds

1.3.6.1.4.1.47421.0.278

All

## CE00610016 ObjectBackupSlaViolated

SLA for backup of object is violated.

This alert is triggered when the SLA of object backup is violated. This alert can be generated when the load on the {{site.data.keyword.baas_full_notm}} is higher than anticipated, or the primary source is loaded and the {{site.data.keyword.baas_full_notm}} is not able to back it up fast enough.

Customer can verify if a new workload is recently added to the {{site.data.keyword.baas_full_notm}} or check if the primary source is throttling Cohesity APIs/calls.

Not Applicable

Warning

86400 seconds

1.3.6.1.4.1.47421.0.235

All

## CE00610031 NasSourceSnapshotDeletionFailed

The deletion of a snapshot failed.

This alert is triggered mostly due to transient network connectivity issues between the cluster and the NAS source.

Delete the snapshot created by Cohesity on the NAS source manually.

Not Applicable

Warning

3600 seconds

1.3.6.1.4.1.47421.0.363

All

## CE00610032 IsilonChangelistCleanupFailed

The deletion of a changelist failed.

This alert is triggered mostly due to transient network connectivity issues between the cluster and the Isilon source.

Delete the changelist created by Cohesity on the Isilon source manually.

Not Applicable

Warning

3600 seconds

1.3.6.1.4.1.47421.0.364

All

## CE00610033 OracleRestoreChainBreak

There is restore chain break in previous backup.

This alert is triggered when a gap is discovered in between backups which might cause an unrecoverable time range. This alert can be caused by an unknown backup error or due to some backup being deleted inappropriately (manually or due to the policy).

Run a full/incremental backup to fix the log chain break. If a chain break alert is still triggered, contact [IBM Cloud Support](../../../Support/ContactSupport.htm) or refer to KB for details/resolution.

Not Applicable

Warning

86400 seconds

1.3.6.1.4.1.47421.0.362

All

## CE00610034 RestoreTaskFailed

Restore of the object in the job failed.

This alert is triggered when there is any failure in a restore task.

No action is required.

Not Applicable

Critical

3600 seconds

1.3.6.1.4.1.47421.0.369

All

## CE00610035 RestoreTaskWarning

Restore of the object in a job succeeded with warnings.

This alert is triggered when there is any warning in a restore task. The alert can be caused when a restore task succeeds but with warnings.

No action is required.

Not Applicable

Warning

3600 seconds

1.3.6.1.4.1.47421.0.368

All

## CE02810041 AgentCertIsAboutToExpire

The certificate associated with the agent is about to expire.

This alert is triggered when the certificate on the agent is about to expire in 180 days.

Upgrade the agent.

Not Applicable

Informational

1.3.6.1.4.1.47421.0.401

All

## CE02810041 AgentCertIsAboutToExpire

The certificate associated with the agent is about to expire.

This alert is triggered when the certificate on the agent is about to expire in 90 days.

Upgrade the agent.

Not Applicable

Warning

1.3.6.1.4.1.47421.0.401

All

## CE02810041 AgentCertIsAboutToExpire

The certificate associated with the agent is about to expire.

This alert is triggered when the certificate on the agent is about to expire in 30 days.

Upgrade the agent.

Not Applicable

Critical

1.3.6.1.4.1.47421.0.401

All

## CE00610041 AgentCertIsAboutToExpire

The certificate associated with the agent is about to expire.

This alert is triggered when the certificate associated with the agent will expire within 180 days.

Upgrade the agent.

Not Applicable

The severity of the alert can be any one of the following:

*   Critical if certificate is expiring within 30 days.
    
*   Warning if certificate is expiring within 90 days.
    
*   Informational if certificate is expiring within 180 days.
    

1.3.6.1.4.1.47421.0.401

All

## CE02810036 MagnetoUnableToReachYoda

Protection Service is unable to contact the indexing service.

This alert is triggered when the protection service hasn’t made a successful call to the indexing service for a long time and has a long queue of pending requests built up waiting to be sent. This can happen when the indexing service is down or overloaded.

Contact [IBM Cloud Support](../../../Support/ContactSupport.htm) for assistance.

Not Applicable

Warning

3600 seconds

1.3.6.1.4.1.47421.0.392

All

## CE02810037 ProtectionGroupWarning

This alert is triggered when a Protection Group run succeeds, but with warnings and no errors.

No action required.

Warning

3600 seconds

Not Applicable

All

## CE00610037 ProtectionGroupWarning

Backup run of protection group finished with warning(s).

This alert is triggered when a backup run has finished or was canceled and has warnings attached to it.

 

| Job Type | Error Codes | Description |
| --- | --- | --- |
| All | `kCancelled` | Backup run was canceled due to hitting a blackout window. Backup run was canceled as it timed out. Failed to backup some of the objects in a protection run as they have already been backed up in another protection run. |
| `kSetInodeIdFloorFailed` | We could not set the inode id floor on the backup view. This can cause the next archival to be a full archival. | |
| `kScriptError` | Failed to execute the pre-backup/post-snapshot/post-backup script of the job. | |

Warning

1.3.6.1.4.1.47421.0.392

All

## CE02810038 ProtectionRunExpiring

Protection run is close to expiry and the replication or archival task has failed.

This alert is triggered when the replication or archival task has failed on at least one target, and the primary backup is close to expiry.

Extend the retention period on the primary cluster or retry the failed replication or archival task if the retention is needed beyond current expiry on the primary cluster.

Not Applicable

Warning

1.3.6.1.4.1.47421.0.395

All

## CE00610039 OracleUnresponsiveProgressQuery

The Oracle database is unresponsive consecutively.

The Oracle database might be busy or unresponsive due to memory issues.

On the Oracle database host, verify the Oracle database status.

  

 
| Error Codes | Description |
| --- | --- |
| `kTimeout` | Oracle database might be busy or unresponsive due to memory issues. Please check the database status. |

Warning

1.3.6.1.4.1.47421.0.397

All

## CE00610040 FailedToChangeAuthForBackupView

Failed to set or unset the service authorization configuration for the backup view(s).

Due to transient errors, the change in service authorization configuration for the backup view(s) failed. The operation is retried in the next iteration.

Verify whether the {{site.data.keyword.baas_full_notm}} services are stable.

Not Applicable  

Warning

1.3.6.1.4.1.47421.0.400

All

## CE02809068 ReplicationHandshakeStuck

The handshake for a replication job to the remote cluster is stuck.

The alert is raised if the remote cluster does not respond to the handshake request for a given job in a given time window.

Check the network connection to see if there’s any issue. If not, contact [IBM Cloud Support](../../../Support/ContactSupport.htm).

  

 
| Error Codes | Description |
| --- | --- |
| `kTimeout` | The handshake request from the local cluster to the remote cluster did not get a response in a given time. |

Warning

1.3.6.1.4.1.47421.0.402

All

## CE02809069 ClockSkewBetweenClusterAndDC

A big clock skew between the {{site.data.keyword.baas_full_notm}} and Domain Controller, causing the authentication to fail.

The clock skew between the {{site.data.keyword.baas_full_notm}} (or Hyx node if existent) and the DC controller is more significant than a threshold.

Go to the DC mentioned in the alert message and fix the time synchronization issue.

  

 
| Error Codes | Description |
| --- | --- |
| `kDomainController` | If the clock skew between the {{site.data.keyword.baas_full_notm}} (or Hyx node if existent) and the DC controller is more significant than a threshold, authentication will fail through this DC. Thus, the time skew is periodically monitored. If it gets close to the threshold, The alert is raised as a reminder to sync up the clock on the DC. |

Critical

1.3.6.1.4.1.47421.0.405

All

## CE02806031 HighUnreclaimedGarbageAlert

A critically high amount of unclaimed garbage has been detected in the {{site.data.keyword.baas_full_notm}}. This can cause an increase in the cluster usage.

The garbage percentage concerning the cluster capacity is consistently higher (> 15%) for the last 3 Garbage Collection cycles.

Contact [IBM Cloud Support](../../../Support/ContactSupport.htm) to examining the garbage backlog in the {{site.data.keyword.baas_full_notm}}.

Not Applicable  

Critical

1.3.6.1.4.1.47421.0.403

All

