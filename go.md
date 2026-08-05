---

copyright:
  years: 2024
lastupdated: "2026-08-05"

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

The {{site.data.keyword.baas_full}} SDK for Go provides features to make the most of {{site.data.keyword.baas_full_notm}}.
{: shortdesc}

The {{site.data.keyword.baas_full_notm}} SDK for Go is comprehensive, with many features and capabilities that exceed the scope and space of this guide. For detailed class and method documentation [see the Go API documentation](https://pkg.go.dev/github.com/IBM/ibm-backup-recovery-sdk-go){: external}. Source code can be found in the [GitHub repository](https://github.com/IBM/ibm-backup-recovery-sdk-go){: external}.

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

To connect to {{site.data.keyword.baas_full_notm}}, a client is created and configured by providing credential information (API key and service instance ID). These values can also be automatically sourced from a credentials file or from environment variables.

The credentials can be found by creating a [Service Credential](/docs/cloud-object-storage?topic=cloud-object-storage-service-credentials), or through the CLI.

Here's an example of how to define environment variables in an application runtime at the {{site.data.keyword.baas_full_notm}} portal. The required variables are `IBM_API_KEY_ID` containing your Service Credential `apikey` and an `IBM_AUTH_ENDPOINT` with a value appropriate to your account, like `https://iam.cloud.ibm.com/identity/token`.

### Initializing configuration
{: #go-init-config}

```Go

// Constants for IBM COS values
const (
	apiKey                 = "<api-key>" // example: xxxd12V2QHXbjaM99G9tWyYDgF_0gYdlQ8aWALIQxXx4
	authEndpoint           = "https://iam.cloud.ibm.com/identity/token"
	baasEndpointUrl        = "<endpoint>" // example: https://brs-stage-us-south-01.backup-recovery.cloud.ibm.com/
	managementReportingUrl = "<endpoint>" // example: https://manager.backup-recovery.cloud.ibm.com/heliosreporting/api/v1
	tenantID               = "iqhch7lbr4/"
	connectionId           = "4980716806983529472"

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
	createProtectionGroupName = "sdk-test-create-protection-group-1"
	updateProtectionGroupName = "sdk-test-update-protection-group-1"

	// Recovery constants
	createRecoveryName                        = "sdk-test-create-recovery-1"
	sourceVoulmeGuid                          = "sdk-test-source-volume-guid"
	destinationVoulmeGuid                     = "sdk-test-destination-volume-guid"
	createDownloadFilesAndFoldersRecoveryName = "create-download-files-and-folders-recovery-1"
)

// Variables
var physicalSourceApplications = []string{"kSQL"}

// Create Authenticator

authenticator := new(core.IamAuthenticator)
authenticator.ApiKey = apiKey
authenticator.URL = ibmAuthEndpoint

// Create Client

optionsBaas := new(baas.BackupRecoveryV1Options)
optionsBaas.Authenticator = authenticator
optionsBaas.URL = baasEndpointUrl
client, _ = baas.NewBackupRecoveryV1(optionsBaas)

// Create Management Reporting Client

optionsManagementReportingServiceApIsV1 := new(baas.BackupRecoveryManagementReportingApiV1Options)
optionsManagementReportingServiceApIsV1.URL = managementReportingUrl
optionsManagementReportingServiceApIsV1.Authenticator = authenticator
managementReportingClient, _ = baas.NewBackupRecoveryManagementReportingApiV1(optionsManagementReportingServiceApIsV1)

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

### List Report Components
{: #go-list-report-component}

```Go
func main() {
	getComponentsOptions := &baas.GetComponentsOptions{}
	result, response, err := managementReportingClient.GetComponents(getComponentsOptions)
	if err != nil {
		log.Fatalf("Failed to fetch components list: %v", err)
	}
	fmt.Println(result)
	fmt.Println(response)
}
```
{: codeblock}

### Fetch a Report Component
{: #go-fetch-report-component}

```Go
func main() {
	getComponentByIdOptions := &baas.GetComponentByIdOptions{ID: core.StringPtr("600")}
	result, response, err := managementReportingClient.GetComponentByID(getComponentByIdOptions)

	if err != nil {
		log.Fatalf("Failed to fetch component by Id: %v", err)
	}
	fmt.Println(result)
	fmt.Println(response)
}
```
{: codeblock}

### Fetch Preview of a Component
{: #go-fetch-preview-component}

```Go
func main() {
	getComponentPreviewOptions := &baas.GetComponentPreviewOptions{ID: core.StringPtr("600")}
	result, response, err := managementReportingClient.GetComponentPreview(getComponentPreviewOptions)
	if err != nil {
		log.Fatalf("Failed to fetch component preview: %v", err)
	}
	fmt.Println(result)
	fmt.Println(response)
}
```
{: codeblock}

### Get Resources
{: #go-get-resource}

```Go
func main() {
	getResourcesOptions := &baas.GetResourcesOptions{ResourceType: core.StringPtr("MessageCodeMappings")}
	result, response, err := managementReportingClient.GetResources(getResourcesOptions)
	if err != nil {
		log.Fatalf("Failed to fetch resources: %v", err)
	}
	fmt.Println(result)
	fmt.Println(response)
}
```
{: codeblock}

### List Properties of a Report Type
{: #go-list-properties-report-type}

```Go
func main() {
	getReportTypeOptions := &baas.GetReportTypeOptions{ReportType: core.StringPtr("Failures")}
	result, response, err := managementReportingClient.GetReportType(getReportTypeOptions)
	if err != nil {
		log.Fatalf("Failed to fetch report type: %v", err)
	}
	fmt.Println(result)
	fmt.Println(response)
}
```
{: codeblock}

### List Reports
{: #go-list-reports}

```Go
func main() {
	getReportsOptions := &baas.GetReportsOptions{}
	result, response, err := managementReportingClient.GetReports(getReportsOptions)
	if err != nil {
		log.Fatalf("Failed to fetch reports: %v", err)
	}
	fmt.Println(result)
	fmt.Println(response)
}
```
{: codeblock}

### Fetch a Report
{: #go-fetch-report}

```Go
func main() {
	getReportByIdOptions := &baas.GetReportByIdOptions{ID: core.StringPtr("failures")}
	result, response, err := managementReportingClient.GetReportByID(getReportByIdOptions)
	if err != nil {
		log.Fatalf("Failed to fetch report by id: %v", err)
	}
	fmt.Println(result)
	fmt.Println(response)
}
```
{: codeblock}

### Fetch a Report Preview
{: #go-fetch-report-preview}

```Go
func main() {
	getReportPreviewOptions := &baas.GetReportPreviewOptions{ID: core.StringPtr("recovery"), ComponentIds: []string{900}}
	result, response, err := managementReportingClient.GetReportPreview(getReportPreviewOptions)
	if err != nil {
		log.Fatalf("Failed to fetch report preview: %v", err)
	}
	fmt.Println(result)
	fmt.Println(response)
}
```
{: codeblock}

### Export a Report
{: #go-export-report}

```Go
func main() {
	exportReportOptions := &baas.ExportReportOptions{ID: core.StringPtr("protected")}
	result, response, err := managementReportingClient.ExportReport(exportReportOptions)
	if err != nil {
		log.Fatalf("Failed to export report: %v", err)
	}
	fmt.Println(result)
	fmt.Println(response)
}
```
{: codeblock}

### List Provider Instances
{: #go-list-provider-instances}

```Go
func main() {
	getProviderInstancesOptions := &baas.GetProviderInstancesOptions{}
	result, response, err := managementReportingClient.GetProviderInstances(getProviderInstancesOptions)
	if err != nil {
		log.Fatalf("Failed to get provider instances: %v", err)
	}
	fmt.Println(result)
	fmt.Println(response)
}
```
{: codeblock}

### Create Source Registration for Kubernetes Cluster
{: #go-create-source-registration-kubernetes}

```Go
func main() {
    k8sClusterParams := &baas.KubernetesSourceRegistrationParams{
        Endpoint:                           core.StringPtr("https://c104-e.us-east.containers.cloud.ibm.com:31410"),
        ClientPrivateKey:                   core.StringPtr(""),
        DataMoverImageLocation:             core.StringPtr("icr.io/ext/brs/cohesity-datamover:7.2.15-p2"),
        VeleroImageLocation:                core.StringPtr("icr.io/ext/brs/velero:7.2.15-p2"),
        VeleroOpenshiftPluginImageLocation: core.StringPtr("icr.io/ext/brs/velero-plugin-for-openshift:7.2.15-p2"),
        VeleroAwsPluginImageLocation:       core.StringPtr("icr.io/ext/brs/velero-plugin-for-aws:7.2.15-p2"),
        KubernetesDistribution:             core.StringPtr("kROKS"),
    }
    connectionIdInt, _ := strconv.ParseInt("932952619841170816", 10, 64)
    registerSourceRegistrationOptions := &baas.RegisterProtectionSourceOptions{
        XIBMTenantID:     core.StringPtr("wkk1yqrdce/"),
        Environment:      core.StringPtr("kKubernetes"),
        Name:             core.StringPtr("sdk-iks-registration"),
        KubernetesParams: k8sClusterParams,
        ConnectionID:     core.Int64Ptr(connectionIdInt),
    }
    result, _, err := service.RegisterProtectionSource(registerSourceRegistrationOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Create Protection Group for Kubernetes Namespace
{: #go-create-protection-group-kubernetes}

```Go
func main() {
    kk8sEnv := "kKubernetes"
    createProtectionGroupOptions := &baas.CreateProtectionGroupOptions{
        XIBMTenantID: core.StringPtr("wkk1yqrdce/"),
        Name:         core.StringPtr("sdk-brs-protection-group"),
        PolicyID:     core.StringPtr("8305184241232842:1757331781254:65366"),
        Environment:  core.StringPtr(kk8sEnv),
        KubernetesParams: &baas.KubernetesProtectionGroupParams{

            Objects: []baas.KubernetesProtectionGroupObjectParams{
                {
                    ID: core.Int64Ptr(3120),
                },
            },
        },
        Priority:  core.StringPtr("kMedium"),
        QosPolicy: core.StringPtr("kBackupHDD"),
    }
    groupresult, _, err := service.CreateProtectionGroup(createProtectionGroupOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println("Group Id:", *groupresult.ID)
    fmt.Println(result)
}
```
{: codeblock}

### Create Recovery for Kubernetes Snapshot
{: #go-create-recovery-kubernetes}

```Go
func main() {
    recoverKubernetesParamsRecoverNamespaceParams := &baas.RecoverKubernetesParamsRecoverNamespaceParams{
        TargetEnvironment: core.StringPtr("kKubernetes"),
        KubernetesTargetParams: &baas.RecoverKubernetesNamespaceParamsKubernetesTargetParams{
            Objects: []baas.CommonRecoverObjectSnapshotParams{{
                SnapshotID: core.StringPtr("eyJhX2NsdXN0ZXJJZCI6ODMwNTE4NDI0MTIzMjg0MiwiYl9jbHVzdGVySW5jYXJuYXRpb25JZCI6MTc1NzMzMTc4MTI1NCwiY19qb2JJZCI6MjQ4MDM2LCJlX2pvYkluc3RhbmNlSWQiOjI0ODA1MywiZl9ydW5TdGFydFRpbWVVc2VjcyI6MTc2MDI3MDk2NjI2ODg4MywiZ19vYmplY3RJZCI6MzEyMCwiaF92YXVsdElkIjoxMjQzNDU3fQ=="),
            },
            },
            RenameRecoveredNamespacesParams: &baas.KubernetesTargetParamsForRecoverKubernetesNamespaceRenameRecoveredNamespacesParams{
                Prefix: core.StringPtr("0-copy-sdk-workload"),
            },
            RecoveryTargetConfig: &baas.KubernetesTargetParamsForRecoverKubernetesNamespaceRecoveryTargetConfig{
                RecoverToNewSource: core.BoolPtr(false),
            },
        },
    }

    k8sparams := &baas.RecoveryRequestParamsKubernetesParams{
        RecoverNamespaceParams: recoverKubernetesParamsRecoverNamespaceParams,
        RecoveryAction:         core.StringPtr("RecoverNamespaces"),
    }
    createRecoveryOptions := &baas.CreateRecoveryOptions{
        XIBMTenantID:        core.StringPtr("wkk1yqrdce/"),
        Name:                core.StringPtr("sdk-test-backup-recovery-create-recovery"),
        SnapshotEnvironment: core.StringPtr("kKubernetes"),
        KubernetesParams:    k8sparams,
    }
    result, _, err := service.CreateRecovery(createRecoveryOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### Get Registration Information
{: #go-get-registration}

```Go
func main() {
    listProtectionSourcesOptions := &baas.ListProtectionSourcesRegistrationInfoOptions{
        XIBMTenantID: core.StringPtr("wkk1yqrdce/"),
    }
    result, _, err := service.ListProtectionSourcesRegistrationInfo(listProtectionSourcesOptions)
    if err != nil {
        t.Fatalf("Expected no error, got %v", err)
    }
    fmt.Println(result)
}
```
{: codeblock}

### List Connector Agents
{: #go-list-connector-agent}

```Go
func ListConnectorAgents() {
    apiKey := "<api_key>"
    // Replace with your IBM Cloud API key
    serviceURL := “https://<BRS_Endpoint>/v2”
    // Replace with your BRS service URL (e.g. https://193c8f4f-89b3-4f4f-9079-8b707dd1fdd4.us-east.backup-recovery-tests.cloud.ibm.com)
    // Create authenticator
    authenticator := &core.IamAuthenticator{
        ApiKey: apiKey,
    }

    // Create service client
    backupRecoveryService, err := backuprecoveryv1.NewBackupRecoveryV1(&backuprecoveryv1.BackupRecoveryV1Options{
        URL:           serviceURL,
        Authenticator: authenticator,
    })

    // Construct the ListConnectorAgentsOptions options
    ListConnectorAgentsOptions := &backuprecoveryv1.ListConnectorAgentsOptions{
        XIBMTenantID: core.StringPtr("4ugtn5idq2/"),
        TenantID:     core.StringPtr("4ugtn5idq2/"),
    }

    // Invoke the ListConnectorAgents operation
    result, _, err := backupRecoveryService.ListConnectorAgents(ListConnectorAgentsOptions)

    // Verify the response
    if err != nil {
        fmt.Printf("Error: %v\n", err)
        os.Exit(1)
    }
    fmt.Println("=== list connector agents completed successfully:")
    fmt.Println(*result.ConnectorAgents[0].ConnectionName)
    fmt.Println(*result.ConnectorAgents[0].ConnectionID)
}
```
{: codeblock}

### Get Connector Agent Configuration
{: #go-get-connector-agent}

```Go
func GetConnectorAgentConfig() {
    apiKey := "<api_key>"
    // Replace with your IBM Cloud API key
    serviceURL := “https://<BRS_Endpoint>/v2”
    // Replace with your BRS service URL (e.g. https://193c8f4f-89b3-4f4f-9079-8b707dd1fdd4.us-east.backup-recovery-tests.cloud.ibm.com)
    // Create authenticator
    authenticator := &core.IamAuthenticator{
        ApiKey: apiKey,
    }

    // Create service client
    backupRecoveryService, err := backuprecoveryv1.NewBackupRecoveryV1(&backuprecoveryv1.BackupRecoveryV1Options{
        URL:           serviceURL,
        Authenticator: authenticator,
    })
    // Construct the GetConnectorAgentConfigOptions options
    GetConnectorAgentConfigOptions := &backuprecoveryv1.GetConnectorAgentConfigOptions{
        XIBMTenantID: core.StringPtr("4ugtn5idq2/"),
    }

    // Invoke the GetConnectorAgentConfig
    GetConnectorAgentConfig, _, err := backupRecoveryService.GetConnectorAgentConfig(GetConnectorAgentConfigOptions)

    // Verify the response
    if err != nil {
        fmt.Printf("Error : %v\n", err)
        os.Exit(1)
    }
    fmt.Println("get connector agent config completed successfully:", *GetConnectorAgentConfig.RegistrationToken)

}
```
{: codeblock}

### Register Connector Agent
{: #go-create-connector-agent}

This API requires localhost access (http://localhost:8080). The application must run on the same VSI/container as the connector agent. Build your Go binary for Linux and deploy it to the target environment before execution of RegisterConnectorAgent method.
{: note}

```Go
func RegisterConnectorAgent() {
    apiKey := "<api_key>"
    // Replace with your IBM Cloud API key
    serviceURL := “http://localhost:8080”
    // Create authenticator
    authenticator := &core.IamAuthenticator{
        ApiKey: apiKey,
    }

    // Create service client
    backupRecoveryService, err := backuprecoveryv1.NewBackupRecoveryV1(&backuprecoveryv1.BackupRecoveryV1Options{
        URL:           serviceURL,
        Authenticator: authenticator,
    })

    RegisterConnectorAgentOptions := &backuprecoveryv1.RegisterConnectorAgentOptions{
        RegistrationToken:      core.StringPtr("RegistrationToken"),
        ConnectionName:         core.StringPtr("sdk-connection-test-1"), // new connection name
        JoinExistingConnection: core.BoolPtr(false),
    }

    // Invoke the RegisterConnectorAgent operation
    res, err := backupRecoveryService.RegisterConnectorAgent(RegisterConnectorAgentOptions)

    // Verify the response
    if err != nil {
        fmt.Printf("Error : %v\n", err)
        os.Exit(1)
    }
    fmt.Println("Register Connector Agent completed successfully:", res)

}
```
{: codeblock}

## Next Steps
{: #go-next-steps}

If you haven't already, please see the detailed class and method documentation available at the [Go API documentation](https://ibm.github.io/ibm-backup-recovery-sdk-go/){: external}.
