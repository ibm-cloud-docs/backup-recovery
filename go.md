---

copyright:
  years: 2024
lastupdated: "2025-07-30"

keywords: backup recovery, go, sdk

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

# Using Go
{: #using-go}

The {{site.data.keyword.cos_full}} SDK for Go provides features to make the most of {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

The {{site.data.keyword.cos_full_notm}} SDK for Go is comprehensive, with many features and capabilities that exceed the scope and space of this guide. For detailed class and method documentation [see the Go API documentation](https://ibm.github.io/ibm-cos-sdk-go/){: external}. Source code can be found in the [GitHub repository](https://github.com/IBM/ibm-backup-recovery-sdk-go){: external}.

## Getting the SDK
{: #go-get-sdk}

Use `go get` to retrieve the SDK to add it to your GOPATH workspace, or project's Go module dependencies. The SDK requires a minimum version of Go 1.10 and maximum version of Go 1.12. Future versions of Go will be supported once our quality control process has been completed.

```sh
go get github.com/IBM/ibm-backup-recovery-sdk-go
```
{: pre}

To update the SDK use `go get -u` to retrieve the latest version of the SDK.

```sh
go get -u github.com/IBM/ibm-backup-recovery-sdk-go
```
{: pre}

### Import packages
{: #go-import-packages}

After you have installed the SDK, you will need to import the packages that you require into your Go applications to use the SDK, as shown in the following example:

```sh
import (
    "github.com/IBM/go-sdk-core/v5/core"
    baas "github.com/IBM/ibm-backup-recovery-sdk-go/backuprecoveryv1"
)
```
{: codeblock}

## Creating a client and sourcing Service credentials
{: #go-client-service-credentials}

To connect to {{site.data.keyword.cos_full_notm}}, a client is created and configured by providing credential information (API key and service instance ID). These values can also be automatically sourced from a credentials file or from environment variables.

The credentials can be found by creating a [Service Credential](/docs/cloud-object-storage?topic=cloud-object-storage-service-credentials), or through the CLI.

Figure 1 shows an example of how to define environment variables in an application runtime at the {{site.data.keyword.cos_full_notm}} portal. The required variables are `IBM_API_KEY_ID` containing your Service Credential `apikey` and an `IBM_AUTH_ENDPOINT` with a value appropriate to your account, like `https://iam.cloud.ibm.com/identity/token`.

![environment variables](images/go-library-fig-1-env-vars.png){: caption="Figure 1. Environment Variables"}

### Initializing configuration
{: #go-init-config}

```Go

// Constants for IBM COS values
const (
    apiKey            = "<api-key>" // example: xxxd12V2QHXbjaM99G9tWyYDgF_0gYdlQ8aWALIQxXx4
    authEndpoint      = "https://iam.cloud.ibm.com/identity/token"
    baasEndpointUrl   = "<endpoint>" // example: https://brs-stage-us-south-01.backup-recovery.cloud.ibm.com/
    tenantID          = "iqhch7lbr4/"
    connectionId      = "4980716806983529472"

    // Connection constants
    connectionName       = "sdk-test-connection-name-2"
    updateConnectionName = "sdk-test-update-connection-name-1"

    // Physical Source constants
    physicalSourceEndpoint     = "172.26.1.14"
    physicalSourceHostType     = "kLinux"
    physicalSourcePhysicalType = "kHost"
    physicalSourceEnvironment  = "kPhysical"
    physicalSourceName         = "sdk-test-register-protection-source-1"

    // Protection Policy constants
    createProtectionPolicyName = "sdk-test-create-protection-policy-3"
    updateProtectionPolicyName = "sdk-test-update-protection-policy-3"

    // Protection Group constants
    createProtectionGroupName           = "sdk-test-create-protection-group-1"
    updateProtectionGroupName           = "sdk-test-update-protection-group-1"

    // Recovery constants
    createRecoveryName                        = "sdk-test-create-recovery-1"
    sourceVoulmeGuid                          = "sdk-test-source-volume-guid"
    destinationVoulmeGuid                     = "sdk-test-destination-volume-guid"
    createDownloadFilesAndFoldersRecoveryName = "create-download-files-and-folders-recovery-1"
)

// Variables
var physicalSourceApplications = []string{"kSQL"}

// Create Client

authenticator := new(core.IamAuthenticator)
authenticator.ApiKey = apiKey
authenticator.URL = ibmAuthEndpoint
optionsBaas := new(baas.BackupRecoveryV1Options)
optionsBaas.Authenticator = authenticator
optionsBaas.URL = baasEndpointUrl
client, _ = baas.NewBackupRecoveryV1(optionsBaas)

```
{: codeblock}

## Code Examples
{: #go-code-examples}

### Get Datasource Connections
{: #go-get-datasource-connections}

```Go
func main() {
    getDataSourceConnectionsOptions := &baas.GetDataSourceConnectionsOptions{
        XIBMTenantID: core.StringPtr(tenantID),
    }
    result, _, err := client.GetDataSourceConnections(getDataSourceConnectionsOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Create Datasource Connection
{: #go-create-datasource-connection}

```Go
func main() {
    createDataSourceConnectionOptions := &baas.CreateDataSourceConnectionOptions{
        XIBMTenantID:   core.StringPtr(tenantID),
        ConnectionName: core.StringPtr(connectionName),
    }
    result, _, err := service.CreateDataSourceConnection(createDataSourceConnectionOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Patch Datasource Connection
{: #go-patch-datasource-connection}

```Go
func main() {
    patchDataSourceConnectionOptions := &baas.PatchDataSourceConnectionOptions{
        XIBMTenantID:   core.StringPtr(tenantID),
        ConnectionID:   core.StringPtr(connectionID),
        ConnectionName: core.StringPtr(updateConnectionName),
    }
    result, _, err := service.PatchDataSourceConnection(patchDataSourceConnectionOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Generate Registration Token
{: #go-generate-registration-token}

```Go
func main() {
    generateDataSourceConnectionRegistrationTokenOptions := &baas.GenerateDataSourceConnectionRegistrationTokenOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ConnectionID: core.StringPtr(connectionID),
    }
    result, _, err := service.GenerateDataSourceConnectionRegistrationToken(generateDataSourceConnectionRegistrationTokenOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Datasource Connectors
{: #go-get-datasource-connectors}

```Go
func main() {
    getDataSourceConnectiorOptions := &baas.GetDataSourceConnectorsOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ConnectionID: core.StringPtr(connectionID),
    }
    result, _, err := service.GetDataSourceConnectors(getDataSourceConnectiorOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Source Registrations
{: #go-get-source-registrations}

```Go
func main() {
    getSourceRegistrationOptions := &baas.GetSourceRegistrationsOptions{
        XIBMTenantID: core.StringPtr(tenantID),
    }
    result, _, err := service.GetSourceRegistrations(getSourceRegistrationOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Creating a Protection Source
{: #go-create-protection-source}

```Go
func main() {
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
}
```
{: codeblock}

### Get Protection Source By Id
{: #go-get-protection-source-by-id}

```Go
func main() {
    getProtectionSourceByIdOptions := &baas.GetProtectionSourceRegistrationOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.Int64Ptr(sourceRegistrationId),
    }
    result, _, err := service.GetProtectionSourceRegistration(getProtectionSourceByIdOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Update Protection Source By Id
{: #go-update-protection-source-by-id}

```Go
func main() {
    physicalSourceParamsOptions := &baas.PhysicalSourceRegistrationParams{
        Endpoint:     core.StringPtr(physicalSourceEndpoint),
        HostType:     core.StringPtr(physicalSourceHostType),
        PhysicalType: core.StringPtr(physicalSourcePhysicalType),
        Applications: nil,
    }
    updateSourceRegistrationOptions := &baas.UpdateProtectionSourceRegistrationOptions{
        XIBMTenantID:   core.StringPtr(tenantID),
        ID:             core.Int64Ptr(sourceRegistrationId),
        Environment:    core.StringPtr(physicalSourceEnvironment),
        Name:           core.StringPtr(physicalSourceName),
        PhysicalParams: physicalSourceParamsOptions,
    }
    result, _, err := service.UpdateProtectionSourceRegistration(updateSourceRegistrationOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Patch Protection Source By Id
{: #go-patch-protection-source-by-id}

```Go
func main() {
    patchDataSourceConnectionOptions := &baas.PatchDataSourceConnectionOptions{
        XIBMTenantID:   core.StringPtr(tenantID),
        ConnectionID:   core.StringPtr(connectionID),
        ConnectionName: core.StringPtr(connectionName),
    }
    result, _, err := service.PatchDataSourceConnection(patchDataSourceConnectionOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Refresh Protection Source By Id
{: #go-refresh-protection-source-by-id}

```Go
func main() {
    refreshProtectionSourceByIdOptions := &baas.RefreshProtectionSourceByIdOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.Int64Ptr(sourceRegistrationId),
    }

    result, err := service.RefreshProtectionSourceByID(refreshProtectionSourceByIdOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Protection Policies
{: #go-get-protection-policies}

```Go
func main() {
    getProtectionPoliciesOptions := &baas.GetProtectionPoliciesOptions{
        XIBMTenantID: core.StringPtr(tenantID),
    }
    result, _, err := service.GetProtectionPolicies(getProtectionPoliciesOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Create Protection Policy
{: #go-create-protection-policy}

```Go
func main() {
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
    result, _, err := service.CreateProtectionPolicy(createProtectionPolicyOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Protection Policy By Id
{: #go-get-protection-policy-by-id}

```Go
func main() {
    getProtectionPolicyByIdOptions := &baas.GetProtectionPolicyByIdOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionPolicyId),
    }
    result, _, err := service.GetProtectionPolicyByID(getProtectionPolicyByIdOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Update Protection Policy By Id
{: #go-update-protection-policy-by-id}

```Go
func main() {
    runScheduleOptions := &baas.IncrementalSchedule{
        Unit: core.StringPtr("Minutes"),
        MinuteSchedule: &baas.MinuteSchedule{
            Frequency: core.Int64Ptr(2),
        },
    }
    incrementalFullRetentionPolicyOptions := &baas.IncrementalBackupPolicy{
        Schedule: runScheduleOptions,
    }
    retentionOptions := &baas.Retention{
        Unit:     core.StringPtr("Weeks"),
        Duration: core.Int64Ptr(2),
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
    updateProtectionPolicyByIdOptions := &baas.UpdateProtectionPolicyOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionPolicyId),
        Name:         core.StringPtr(updateProtectionPolicyName),
        Description:  core.StringPtr(updateProtectionPolicyName),
        BackupPolicy: backupPolicyOptions,
    }
    result, _, err := service.UpdateProtectionPolicy(updateProtectionPolicyByIdOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Protection Groups
{: #go-get-protection-groups}

```Go
func main() {
    getProtectionGroups := &baas.GetProtectionGroupsOptions{
        XIBMTenantID: core.StringPtr(tenantID),
    }
    result, _, err := service.GetProtectionGroups(getProtectionGroups)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Create Protection Group
{: #go-create-protection-group}

```Go
func main() {
    physicalFileBackupPathOptions := &baas.PhysicalFileBackupPathParams{
        IncludedPath: core.StringPtr("/data/"),
    }
    fileProtectionGroupObjectOptions := &baas.PhysicalFileProtectionGroupObjectParams{
        ID:        core.Int64Ptr(sourceRegistrationId),
        Name:      core.StringPtr("sdf"),
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
    result, _, err := service.CreateProtectionGroup(createProtectionGroupOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Protection Group By Id
{: #go-get-protection-group-by-id}

```Go
func main() {
    getProtectionGroupByIdOptions := &baas.GetProtectionGroupByIdOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionGroupId),
    }
    result, _, err := service.GetProtectionGroupByID(getProtectionGroupByIdOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Update Protection Group By Id
{: #go-update-protection-group-by-id}

```Go
func main() {
    physicalFileBackupPathOptions := &baas.PhysicalFileBackupPathParams{
        IncludedPath: core.StringPtr("/data/"),
    }
    fileProtectionGroupObjectOptions := &baas.PhysicalFileProtectionGroupObjectParams{
        ID: core.Int64Ptr(sourceRegistrationId),
        FilePaths: []baas.PhysicalFileBackupPathParams{*physicalFileBackupPathOptions},
    }
    fileProtectionGroupOptions := &baas.PhysicalFileProtectionGroupParams{
        Objects: []baas.PhysicalFileProtectionGroupObjectParams{*fileProtectionGroupObjectOptions},
    }
    physicalParamsOptions := &baas.PhysicalProtectionGroupParams{
        ProtectionType:           core.StringPtr("kFile"),
        FileProtectionTypeParams: fileProtectionGroupOptions,
    }
    updateProtectionGroupOptions := &baas.UpdateProtectionGroupOptions{
        XIBMTenantID:   core.StringPtr(tenantID),
        ID:             core.StringPtr(protectionGroupId),
        Name:           core.StringPtr(updateProtectionGroupName),
        PolicyID:       core.StringPtr(protectionPolicyId),
        Environment:    core.StringPtr(physicalSourceEnvironment),
        PhysicalParams: physicalParamsOptions,
    }
    result, _, err := service.UpdateProtectionGroup(updateProtectionGroupOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Protection Group Runs
{: #go-get-protection-group-runs}

```Go
func main() {
    getProtectionGroupRunsOptions := &baas.GetProtectionGroupRunsOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionGroupId),
    }
    result, _, err := service.GetProtectionGroupRuns(getProtectionGroupRunsOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Create Protection Group Run
{: #go-create-protection-group-run}

```Go
func main() {
    createProtectionGroupRunOptions := &baas.CreateProtectionGroupRunOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionGroupId),
        RunType:      core.StringPtr("kRegular"),
    }
    result, _, err := service.CreateProtectionGroupRun(createProtectionGroupRunOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Update Protection Group Run
{: #go-update-protection-group-run}

```Go
func main() {
    localSnapshotConfigOptions := &baas.UpdateLocalSnapshotConfig{
        EnableLegalHold: core.BoolPtr(false),
    }
    updateProtectionGroupRunParamsOptions := &baas.UpdateProtectionGroupRunParams{
        RunID:               core.StringPtr(runId),
        LocalSnapshotConfig: localSnapshotConfigOptions,
    }
    updateProectionGroupRunOptions := &baas.UpdateProtectionGroupRunOptions{
        XIBMTenantID:                   core.StringPtr(tenantID),
        ID:                             core.StringPtr(protectionGroupId),
        UpdateProtectionGroupRunParams: []baas.UpdateProtectionGroupRunParams{*updateProtectionGroupRunParamsOptions},
    }
    result, _, err := service.UpdateProtectionGroupRun(updateProectionGroupRunOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Perform Action On Protection Group Run
{: #go-perform-action-on-protection-group-run}

```Go
func main() {
    pauseProtectionRunActionParamsOptions := &baas.PauseProtectionRunActionParams{
        RunID: core.StringPtr(runId),
    }
    performActionOnProtectionGroupRunOptions := &baas.PerformActionOnProtectionGroupRunOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionGroupId),
        Action:       core.StringPtr("Pause"),
        PauseParams:  []baas.PauseProtectionRunActionParams{*pauseProtectionRunActionParamsOptions},
    }
    result, _, err := service.PerformActionOnProtectionGroupRun(performActionOnProtectionGroupRunOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Object Snapshots
{: #go-get-object-snapshots}

```Go
func main() {
    getObjectSnapshotsOptions := &baas.GetObjectSnapshotsOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.Int64Ptr(sourceRegistrationId),
    }
    result, response, err := service.GetObjectSnapshots(getObjectSnapshotsOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Recoveries
{: #go-get-recoveries}

```Go
func main() {
    getRecoveriesOptions := &baas.GetRecoveriesOptions{
        XIBMTenantID: core.StringPtr(tenantID),
    }
    result, _, err := service.GetRecoveries(getRecoveriesOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Create Recovery
{: #go-create-recovery}

```Go
func main() {
    commonRecoveryObjectSnapshotParams := &baas.CommonRecoverObjectSnapshotParams{
        SnapshotID: core.StringPtr(snapshotId),
    }
    physicalTargetParamsForRecoverVolumeMountTarget := &baas.PhysicalTargetParamsForRecoverVolumeMountTarget{
        ID: core.Int64Ptr(3),
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
    result, _, err := service.CreateRecovery(createRecoveryOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Recovery By Id
{: #go-get-recovery-by-id}

```Go
func main() {
    getRecoveryByIdOptions := &baas.GetRecoveryByIdOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(recoveryId),
    }
    result, _, err := service.GetRecoveryByID(getRecoveryByIdOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Create Download Files And Folders Recovery
{: #go-create-download-files-and-folders-recovery}

```Go
func main() {
    commonRecoveryObjectSnapshotParams := &baas.CommonRecoverObjectSnapshotParams{
        SnapshotID: core.StringPtr(snapshotId),
    }
    filesAndFoldersObjectOptions := &baas.FilesAndFoldersObject{
        AbsolutePath: core.StringPtr("/"),
    }
    createDownloadFilesAndFoldersRecoveryOptions := &baas.CreateDownloadFilesAndFoldersRecoveryOptions{
        XIBMTenantID:    core.StringPtr(tenantID),
        Name:            core.StringPtr(createDownloadFilesAndFoldersRecoveryName),
        Object:          commonRecoveryObjectSnapshotParams,
        FilesAndFolders: []baas.FilesAndFoldersObject{*filesAndFoldersObjectOptions},
    }
    result, _, err := service.CreateDownloadFilesAndFoldersRecovery(createDownloadFilesAndFoldersRecoveryOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Download Files From Recovery
{: #go-download-files-from-recovery}

```Go
func main() {
    downloadFilesFromRecoveryOptions := &baas.DownloadFilesFromRecoveryOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(recoveryId),
    }
    result, err := service.DownloadFilesFromRecovery(downloadFilesFromRecoveryOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Download Indexed File
{: #go-download-indexed-file}

```Go
func main() {
    downloadIndexedFileOptions := &baas.DownloadIndexedFileOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        SnapshotsID:  core.StringPtr(snapshotId),
        FilePath:     core.StringPtr("./"),
    }
    result, err := service.DownloadIndexedFile(downloadIndexedFileOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Restore Points In Time Range
{: #go-get-restore-points-in-time-range}

```Go
func main() {
    getRestorePointsInTimeRangeOptions := &baas.GetRestorePointsInTimeRangeOptions{
        XIBMTenantID:       core.StringPtr(tenantID),
        StartTimeUsecs:     core.Int64Ptr(1),
        EndTimeUsecs:       core.Int64Ptr(10),
        ProtectionGroupIds: []string{protectionGroupId},
        Environment:        core.StringPtr("kSQL"),
        SourceID:           &sourceRegistrationId,
    }
    result, _, err := service.GetRestorePointsInTimeRange(getRestorePointsInTimeRangeOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Delete Protection Group
{: #go-delete-protection-group}

```Go
func main() {
    deleteProtectionGroupOptions := &baas.DeleteProtectionGroupOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionGroupId),
    }
    result, err := service.DeleteProtectionGroup(deleteProtectionGroupOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Delete Protection Source
{: #go-delete-protection-source}

```Go
func main() {
    deleteProtectionSourceOptions := &baas.DeleteProtectionSourceRegistrationOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.Int64Ptr(sourceRegistrationId),
    }
    result, err := service.DeleteProtectionSourceRegistration(deleteProtectionSourceOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Delete Protection Policy
{: #go-delete-protection-policy}

```Go
func main() {
    deleteProtectionPolicyOptions := &baas.DeleteProtectionPolicyOptions{
        XIBMTenantID: core.StringPtr(tenantID),
        ID:           core.StringPtr(protectionPolicyId),
    }
    result, err := service.DeleteProtectionPolicy(deleteProtectionPolicyOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

## Next Steps
{: #go-next-steps}

If you haven't already, please see the detailed class and method documentation available at the [Go API documentation](https://ibm.github.io/ibm-backup-recovery-sdk-go/){: external}.
