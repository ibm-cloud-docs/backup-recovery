---

copyright:
  years: 2024
lastupdated: "2025-12-12"

keywords: backup recovery, sdk, guide

subcollection: backup-recovery

---
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:http: .ph data-hd-programlang='http'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:go: .ph data-hd-programlang='go'}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}

# Getting Started with the SDKs
{: #sdk-gs}

{{site.data.keyword.baas_full}} provides SDKGo which can help you to make the most of Backup and Recovery.
{: shortdesc}

This Quick Start guide provides a code example that demonstrates the following operations:

* Create a new Connection
* Register a Protection Source
* Create a new Protection Policy
* Create a new Protection Group
* Create a new Protection Group Run
* Get Object Snapshots
* Create a new Recovery
* Delete Protection Group
* Delete Protection Policy
* Delete Protection Source

## Before you begin
{: #sdk-gs-prereqs}

You need:

* An [{{site.data.keyword.cloud}} Platform account](https://cloud.ibm.com/login)
* An [instance of {{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-provision)
* An [IAM API key](/docs/cloud-object-storage?topic=cloud-object-storage-iam-overview) with Writer access to your {{site.data.keyword.cos_short}}

## Getting the SDK
{: #sdk-gs-install}

Specific instructions for downloading and installing the SDK is available in [Using Go](/docs/backup-recovery?topic=backup-recovery-using-go){: external}{: go}.

## Code Example
{: #sdk-gs-example}

The code examples below provide introductory examples of running the basic operations with {{site.data.keyword.baas_full_notm}}.

In your code, you must remove the angled brackets or any other excess characters that are provided here as illustration.
{: note}

To complete the code example, you need to replace the following values:

|Value|Description|Example|
|---|---|---|
|`<endpoint>`|Regional endpoint for your COS instance|`brs-stage-us-south-01.backup-recovery.cloud.ibm.com/`|
|`<api-key>`|IAM API Key with at least `Writer` permissions|`xxxd12V2QHXbjaM99G9tWyYDgF_0gYdlQ8aWALIQxXx4`|
|

For more information about endpoints, see [Endpoints and storage locations](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints).

Code examples are tested on supported release versions of Golang.
{: Go}

``` Go
package main
import (
    "github.com/IBM/go-sdk-core/v5/core"
    baas "github.com/IBM/ibm-backup-recovery-sdk-go/backuprecoveryv1"
)

// Constants for IBM COS values
const (
    apiKey            = "<api-key>" // example: xxxd12V2QHXbjaM99G9tWyYDgF_0gYdlQ8aWALIQxXx4
    authEndpoint      = "https://iam.cloud.ibm.com/identity/token"
    baasEndpointUrl   = "<endpoint>" // example: https://brs-stage-us-south-01.backup-recovery.cloud.ibm.com/
    tenantID          = "iqhch7lbr4/"
    connectionId      = "4980716806983529472"
    PhysicalSourceEndpoint = "172.26.1.14"
)

func main() {
    //Setting up a new configuration
    authenticator := new(core.IamAuthenticator)
    authenticator.ApiKey = apiKey
    authenticator.URL = ibmAuthEndpoint
    optionsBaas := new(baas.BackupRecoveryV1Options)
    optionsBaas.Authenticator = authenticator
    optionsBaas.URL = baasEndpointUrl
    client, _ = baas.NewBackupRecoveryV1(optionsBaas)

    // Create new Connection
    createDataSourceConnectionOptions := &baas.CreateDataSourceConnectionOptions{
        XIBMTenantID:   core.StringPtr(tenantID),
        ConnectionName: core.StringPtr(connectionName),
    }
    result_, _, err := client.CreateDataSourceConnection(createDataSourceConnectionOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)


    // Registering a Protection Source
    physicalSourceParamsOptions := &baas.PhysicalSourceRegistrationParams{
        Endpoint:     core.StringPtr(physicalSourceEndpoint),
        HostType:     core.StringPtr(physicalSourceHostType),
        PhysicalType: core.StringPtr(physicalSourcePhysicalType),
        Applications: physicalSourceApplications,
    }
    connectionIdInt, err := strconv.ParseInt(connectionID, 10, 64)
    registerSourceRegistrationOptions := &baas.RegisterProtectionSourceOptions{
        XIBMTenantID:   core.StringPtr(tenantID),
        Environment:    core.StringPtr(physicalSourceEnvironment),
        Name:           core.StringPtr(physicalSourceName),
        PhysicalParams: physicalSourceParamsOptions,
        ConnectionID:   core.Int64Ptr(connectionIdInt),
        }
    result, _, err := client.RegisterProtectionSource(registerSourceRegistrationOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)


    // Create  new Protection Policy
    runScheduleOptions := &baas.IncrementalSchedule{
        Unit: core.StringPtr("Weeks"),
        WeekSchedule: &baas.WeekSchedule{
            DayOfWeek: []string{"Monday"},
        },
    }
    incrementalFullRetentionPolicyOptions := &baas.IncrementalBackupPolicy{
        Schedule: runScheduleOptions,
    }
    retentionOptions := &baas.Retention{
        Unit:     core.StringPtr("Weeks"),
        Duration: core.Int64Ptr(2),
        DataLockConfig: &baas.DataLockConfig{
            Mode:     core.StringPtr("Compliance"),
            Unit:     core.StringPtr("Weeks"),
            Duration: core.Int64Ptr(2),
        },
    }
    regularBackupPolicyOptions := &baas.RegularBackupPolicy{
        Incremental: incrementalFullRetentionPolicyOptions,
        Retention:   retentionOptions,
        PrimaryBackupTarget: &baas.PrimaryBackupTarget{
            UseDefaultBackupTarget: core.BoolPtr(true),
        },
    }
    backupPolicyOptions := &baas.BackupPolicy{
        Regular: regularBackupPolicyOptions,
    }
    createProtectionPolicyOptions := &baas.CreateProtectionPolicyOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        Name:         core.StringPtr(createProtectionPolicyName),
        BackupPolicy: backupPolicyOptions,
        Description:  core.StringPtr(createProtectionPolicyName),
        Version:      core.Int64Ptr(1),
    }
    result, _, err := client.CreateProtectionPolicy(createProtectionPolicyOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)


    // Create a new Protection Group
    physicalFileBackupPathOptions := &baas.PhysicalFileBackupPathParams{
        IncludedPath: core.StringPtr("/"),
    }
    fileProtectionGroupObjectOptions := &baas.PhysicalFileProtectionGroupObjectParams{
        ID:        core.Int64Ptr(sourceRegistrationId),
        Name:      core.StringPtr(fileProtectionGroupObjectName),
        FilePaths: []baas.PhysicalFileBackupPathParams{*physicalFileBackupPathOptions},
    }
    fileProtectionGroupOptions := &baas.PhysicalFileProtectionGroupParams{
        Objects: []baas.PhysicalFileProtectionGroupObjectParams{*fileProtectionGroupObjectOptions},
    }
    physicalParamsOptions := &baas.PhysicalProtectionGroupParams{
        ProtectionType:           core.StringPtr("kFile"),
        FileProtectionTypeParams: fileProtectionGroupOptions,
    }
    createProtectionGroupOptions := &baas.CreateProtectionGroupOptions{
        XIBMTenantID:   core.StringPtr(tenantID),
        Name:           core.StringPtr(createProtectionGroupName),
        PolicyID:       core.StringPtr(protectionPolicyId),
        Environment:    core.StringPtr(physicalSourceEnvironment),
        PhysicalParams: physicalParamsOptions,
        Priority:       core.StringPtr("kMedium"),
    }
    result, _, err := client.CreateProtectionGroup(createProtectionGroupOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)


    // Create new Protection Group Run
    createProtectionGroupRunOptions := &baas.CreateProtectionGroupRunOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionGroupId),
        RunType:      core.StringPtr("kRegular"),
    }
    result, _, err := client.CreateProtectionGroupRun(createProtectionGroupRunOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)


    // Get Object Snapshots
    getObjectSnapshotsOptions := &baas.GetObjectSnapshotsOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.Int64Ptr(sourceRegistrationId),
    }
    result, _, err := client.GetObjectSnapshots(getObjectSnapshotsOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)


    // Create a new Recovery
    commonRecoveryObjectSnapshotParams := &baas.CommonRecoverObjectSnapshotParams{
        SnapshotID: core.StringPtr(snapshotId),
    }
    physicalTargetParamsForRecoverVolumeMountTarget := &baas.PhysicalTargetParamsForRecoverVolumeMountTarget{
        ID:   core.Int64Ptr(3),
        Name: core.StringPtr(recoverVolumeMountTargetName),
    }
    recoverVolumeMapping := &baas.RecoverVolumeMapping{
        SourceVolumeGuid:      core.StringPtr(sourceVoulmeGuid),
        DestinationVolumeGuid: core.StringPtr(destinationVoulmeGuid),
    }
    recoverPhysicalVolumeParamsPhysicalTargetParams := &baas.RecoverPhysicalVolumeParamsPhysicalTargetParams{
        MountTarget:   physicalTargetParamsForRecoverVolumeMountTarget,
        VolumeMapping: []baas.RecoverVolumeMapping{*recoverVolumeMapping},
    }
    recoverPhysicalParamsRecoverVolumeParams := &baas.RecoverPhysicalParamsRecoverVolumeParams{
        TargetEnvironment:    core.StringPtr("kPhysical"),
        PhysicalTargetParams: recoverPhysicalVolumeParamsPhysicalTargetParams,
    }
    recoveryPhysicalParams := &baas.RecoverPhysicalParams{
        Objects:             []baas.CommonRecoverObjectSnapshotParams{*commonRecoveryObjectSnapshotParams},
        RecoveryAction:      core.StringPtr("RecoverPhysicalVolumes"),
        RecoverVolumeParams: recoverPhysicalParamsRecoverVolumeParams,
    }
    createRecoveryOptions := &baas.CreateRecoveryOptions{
        XIBMTenantID:        core.StringPtr(tenantID),
        Name:                core.StringPtr(createRecoveryName),
        SnapshotEnvironment: core.StringPtr("kPhysical"),
        PhysicalParams:      recoveryPhysicalParams,
    }
    result, _, err := client.CreateRecovery(createRecoveryOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)


    // Delete Protection Group
    deleteProtectionGroupOptions := &baas.DeleteProtectionGroupOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionGroupId),
    }
    result, err := client.DeleteProtectionGroup(deleteProtectionGroupOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)


    // Delete Protection Policy
    // Protection Policy can only be deleted after delting the associated
    // Protection group
    deleteProtectionPolicyOptions := &baas.DeleteProtectionPolicyOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionPolicyId),
    }
    _, err := client.DeleteProtectionPolicy(deleteProtectionPolicyOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)


    // Delete Protection Source
    // Protection source can only be deleted after delting the associated
    // Protection group
    deleteProtectionSourceOptions := &baas.DeleteProtectionSourceRegistrationOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.Int64Ptr(sourceRegistrationId),
    }
    _, err := client.DeleteProtectionSourceRegistration(deleteProtectionSourceOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
}


func exitErrorf(msg string, args ...interface{}) {
    fmt.Fprintf(os.Stderr, msg+"\n", args...)
    os.Exit(1)
}
```

{: codeblock}
{: go}

## Running the Code Example

{: #sdk-gs-run}

To run the code sample, copy the code blocks above and run the following:

``` sh
go run go_example.go
```

{: codeblock}
{: go}
