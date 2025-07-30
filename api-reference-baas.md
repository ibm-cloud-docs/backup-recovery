---

copyright:
  years: 2024
lastupdated: "2025-07-30"

keywords: rest, s3, compatibility, api, buckets

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Agent Tasks
{: #compatibility-api-agent-tasks}

## List Agent Upgrade Tasks
{: #compatibility-api-list-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="List Agent Upgrade Tasks" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-list-agent-tasks-opt-query-parms}

| Query Parameter | Value | Required | Description
|------|-------------|-------------|-------------|
| ids | - | no | Specifies IDs of tasks to be fetched. |
{: caption="Optional query parameters" caption-side="bottom"}

**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```

## Create Agent Upgrade Tasks
{: #compatibility-api-create-agent-upgrade-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="Create Agent Upgrade Tasks" caption-side="bottom"}



### Payload
{: #compatibility-api-create-agent-upgrade-tasks-payload}

| Element | Type | Children | Ancestor | Constraint
|------|-------------|-------------|-------------|-------------|
| description | string | - | - | - |
| name | string | - | - | - |
| agentIds | list(integer) | - | - | - |
| retryTaskId | integer |- | - | - |
| scheduleEndTimeUsecs | integer | - | - | - |
| scheduleTimeUsecs | integer | - | - | - |
{: caption="Payload" caption-side="bottom"}

**Syntax**

```http
POST https://{endpoint}/
```

**Example request**

```http
POST / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
  "name": "Test Upgrade Task 06",
  "description": "Includes Agents for Sources RHEL, Win Server and MS SQL",
  "agentIDs": [45]
}
```


## Download Agent
{: #compatibility-api-download-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="Download Agent" caption-side="bottom"}

### Payload
{: #compatibility-api-download-agent-tasks-payload}

| Element | Type | Children | Ancestor | Constraint
|------|-------------|-------------|-------------|-------------|
| platform | string | - | - | `kWindows`, `kLinux` |
| linuxParams | object | `packageType` | - | - |
{: caption="Payload" caption-side="bottom"}

**Syntax**

```http
POST https://{endpoint}/
```

**Example request**

```http
POST / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
    "platform" : "kWindows"
}
```


## Datasource Registration
{: #compatibility-api-datasource-registration-agent-tasks}

### Get Registration
{: #compatibility-api-get-datasource-registration-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
| requestInitiatorType| `string` | No | Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests.
{: caption="Get Registration" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-optional-query-parms-get-datasource-registration-agent-tasks}

| Query Parameter | Description | Type | Required | Value
|------|-------------|-------------|-------------|----|
| id | Specifies the id of the Protection Source registration. | `number` | true | - |
{: caption="Optional query parameters" caption-side="bottom"}

**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```

## List Datasource Registrations
{: #compatibility-api-list-datasource-registration-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="List Datasource Registrations" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-optional-query-parms-list-datasource-registration-agent-tasks}

| Query Parameter | Value | Required | Description
|------|-------------|-------------|-------------|
| ids | - | no | Ids specifies the list of source registration ids to return. |
| includeSourceCredentials | - | no | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
| useCachedData | - | no | Specifies whether we can serve the GET request from the read replica cache. There is a lag of 15 seconds between the read replica and primary data source.|
| ignoreTenantMigrationInProgressCheck | - | no | If true, tenant migration check will be ignored |
| iincludeExternalMetadatads | - | no | If true, the external entity metadata like maintenance mode config for the registered sources will be included. |
{: caption="Optional query parameters" caption-side="bottom"}

**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```


## Create Datasource Registration
{: #compatibility-api-create-datasource-registration-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="Create Datasource Registration" caption-side="bottom"}

### Payload
{: #compatibility-api-payload-create-datasource-registration-agent-tasks}

| Element | Type | Children | Ancestor | Constraint
|------|-------------|-------------|-------------|-------------|
| environment | string | - | - | `kPhysical`, `kSQL` |
| name | string | - | - | - |
| connectionId | integer | - | - | - |
| connections | list | `connectionId`, `entityId`, `connectorGroupId`, `dataSourceConnectionId` | - | - |
| connectorGroupId | string | - | - | - |
| dataSourceConnectionId | string | - | - | - |
| advancedConfigs | list | `key`,`value` | - | - |
| physicalParams | object | `endpoint`, `forceRegister`, `hostType`, `physicalType`, `applications` | - | - |
{: caption="Payload" caption-side="bottom"}

**Syntax**

```http
POST https://{endpoint}/
```

**Example request**

```http
POST / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
 {
            "environment": "kPhysical",
            "connectionId": 530243354208762051,
            "physicalParams": {
                "endpoint": "172.26.1.18",
                "hostType": "kLinux",
                "physicalType": "kHost"
            }
        }
```



## PUT Datasource Registration
{: #compatibility-api-put-datasource-registration-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="PUT Datasource Registration" caption-side="bottom"}


### Payload
{: #compatibility-api-payload-put-datasource-registration-agent-tasks}

| Element | Type | Children | Ancestor | Constraint
|------|-------------|-------------|-------------|-------------|
| environment | string | - | - | `kPhysical`, `kSQL` |
| name | string | - | - | - |
| connection_id | integer | - | - | - |
| connections | list | `connectionId`, `entityId`, `connectorGroupId`, `dataSourceConnectionId` | - | - |
| connector_group_id | string | - | - | - |
| data_source_connection_id | string | - | - | - |
| advanced_configs | list | `key`,`value` | - | - |
| physical_params | object | `endpoint`, `forceRegister`, `hostType`, `physicalType`, `applications` | - | - |
{: caption="Payload" caption-side="bottom"}

**Syntax**

```http
PUT https://{endpoint}/{registration_id}
```

**Example request**

```http
PUT / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
 {
            "physicalParams": {
                "endpoint": "172.26.1.18",
                "hostType": "kLinux",
                "physicalType": "kHost"
                "applications": ["KSQL"]
            }
        }
```



## DELETE Datasource Registration
{: #compatibility-api-delete-datasource-registration-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="DELETE Datasource Registration" caption-side="bottom"}

**Syntax**

```http
DELETE https://{endpoint}/{registration_id}
```

## Protection Policy
{: #compatibility-api-protection-policy-agent-tasks}

### List Policies
{: #compatibility-api-list-protection-policy-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="List Policies" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-opt-query-parms-list-protection-policy-agent-tasks}

| Query Parameter | Value | Required | Description
|------|-------------|-------------|-------------|
| requestInitiatorType | - | No | Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests. | 
| ids | - | No | Filter policies by a list of policy ids. |
| policyNames | - | No | Filter policies by a list of policy names. |
| types | - | No |  Types specifies the policy type of policies to be returned. |
| excludeLinkedPolicies | - | No | If excludeLinkedPolicies is set to true then only local policies created on cluster will be returned. The result will exclude all linked policies created from policy templates. |
| includeReplicatedPolicies | - | No |  If includeReplicatedPolicies is set to true, then response will also contain replicated policies. By default, replication policies are not included in the response. |
| includeStats | - || No | If includeStats is set to true, then response will return number of protection groups and objects. By default, the protection stats are not included in the response. |
{: caption="Optional query parameters" caption-side="bottom"}


**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```


## Get Policy
{: #compatibility-api-get-protection-policy-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
| requestInitiatorType| `string` | No | Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests. 
{: caption="Get Policy" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-opt-query-parms-get-protection-policy-agent-tasks}

| Query Parameter | Description | Type | Required | Value
|------|-------------|-------------|-------------|----|
| id | Specifies a unique id of the Protection Policy to return. | `string` | true | - |
{: caption="Optional query parameters" caption-side="bottom"}

**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```


## Create Policy
{: #compatibility-api-create-protection-policy-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="Create Policy" caption-side="bottom"}

| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| name | Specifies the name of the Protection Policy. | `string` | - | - | - |
| backup_policy | Specifies the backup schedule and retentions of a Protection Policy. | `object` | `regular`, `log`, `bmr`, `cdp`, `storageArraySnapshot`, `runTimeouts` | - | - |
| description | Specifies the description of the Protection Policy. | `string` | - | - | - |
| blackout_window | List of Blackout Windows. If specified, this field defines blackout periods when new Group Runs are not started. If a Group Run has been scheduled but not yet executed and the blackout period starts, the behavior depends on the policy field AbortInBlackoutPeriod. | `list()` | `day`, `startTime`, `endTime`, `configId` | - | - |
| extended_retention | Specifies additional retention policies that should be applied to the backup snapshots. A backup snapshot will be retained up to a time that is the maximum of all retention policies that are applicable to it. | `list()` | `schedule`, `retention`, `runType`, `configId` | - | - |
| remote_target_policy | Specifies the replication, archival and cloud spin targets of Protection Policy. | `object` | `replicationTargets`, `cloudSpinTargets`, `onpremDeployTargets`, `rpaasTargets` | - | - |
| cascaded_targets_config | Specifies the configuration for cascaded replications. Using cascaded replication, replication cluster(Rx) can further replicate and archive the snapshot copies to further targets. Its recommended to create cascaded configuration where protection group will be created. | `list()` | `sourceClusterId`, `remoteTargets` | - | - |
| retry_options | Retry Options of a Protection Policy when a Protection Group run fails. | `object` | `retries`, `retryIntervalMins` | - | - |
| data_lock | This field is now deprecated. Please use the DataLockConfig in the backup retention. | `string` | - | - | `Compliance`, `Administrative` |
| version | Specifies the current policy verison. Policy version is incremented for optionally supporting new features and differentialting across releases. | `number` | - | - | - |
| is_cbs_enabled | Specifies true if Calender Based Schedule is supported by client. Default value is assumed as false for this feature. | `bool` | - | - | - |
| last_modification_time_usecs | Specifies the last time this Policy was updated. If this is passed into a PUT request, then the backend will validate that the timestamp passed in matches the time that the policy was actually last modified. If the two timestamps do not match, then the request will be rejected with a stale error. | `number` | - | - | - |
| template_id | Specifies the parent policy template id to which the policy is linked to. This field is set only when policy is created from template. | `string` | - | - | - |
{: caption="Create Policy elements" caption-side="bottom"}


**Syntax**

```http
POST https://{endpoint}/
```

**Example request**

```http
POST / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
            "name": "tf-policy-10",
            "backupPolicy": {
                "regular": {
                    "incremental": {
                        "schedule": {
                            "unit": "Days",
                            "daySchedule": {
                                "frequency": 1
                            }
                        }
                    },
                    "retention": {
                        "unit": "Weeks",
                        "duration": 3
                    },
                    "primaryBackupTarget" : {
                        "useDefaultBackupTarget" : true
                    }
                }
            },
            "retryOptions": {
                "retries": 3,
                "retryIntervalMins": 5
            }
}
```



## PUT Policy
{: #compatibility-api-put-protection-policy-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="PUT Policy" caption-side="bottom"}


| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| name | Specifies the name of the Protection Policy. | `string` | - | - | - |
| backup_policy | Specifies the backup schedule and retentions of a Protection Policy. | `object` | `regular`, `log`, `bmr`, `cdp`, `storageArraySnapshot`, `runTimeouts` | - | - |
| description | Specifies the description of the Protection Policy. | `string` | - | - | - |
| blackout_window | List of Blackout Windows. If specified, this field defines blackout periods when new Group Runs are not started. If a Group Run has been scheduled but not yet executed and the blackout period starts, the behavior depends on the policy field AbortInBlackoutPeriod. | `list()` | `day`, `startTime`, `endTime`, `configId` | - | - |
| extended_retention | Specifies additional retention policies that should be applied to the backup snapshots. A backup snapshot will be retained up to a time that is the maximum of all retention policies that are applicable to it. | `list()` | `schedule`, `retention`, `runType`, `configId` | - | - |
| remote_target_policy | Specifies the replication, archival and cloud spin targets of Protection Policy. | `object` | `replicationTargets`, `cloudSpinTargets`, `onpremDeployTargets`, `rpaasTargets` | - | - |
| cascaded_targets_config | Specifies the configuration for cascaded replications. Using cascaded replication, replication cluster(Rx) can further replicate and archive the snapshot copies to further targets. Its recommended to create cascaded configuration where protection group will be created. | `list()` | `sourceClusterId`, `remoteTargets` | - | - |
| retry_options | Retry Options of a Protection Policy when a Protection Group run fails. | `object` | `retries`, `retryIntervalMins` | - | - |
| data_lock | This field is now deprecated. Please use the DataLockConfig in the backup retention. | `string` | - | - | `Compliance`, `Administrative` |
| version | Specifies the current policy verison. Policy version is incremented for optionally supporting new features and differentialting across releases. | `number` | - | - | - |
| is_cbs_enabled | Specifies true if Calender Based Schedule is supported by client. Default value is assumed as false for this feature. | `bool` | - | - | - |
| last_modification_time_usecs | Specifies the last time this Policy was updated. If this is passed into a PUT request, then the backend will validate that the timestamp passed in matches the time that the policy was actually last modified. If the two timestamps do not match, then the request will be rejected with a stale error. | `number` | - | - | - |
| template_id | Specifies the parent policy template id to which the policy is linked to. This field is set only when policy is created from template. | `string` | - | - | -
{: caption="PUT Policy elements" caption-side="bottom"}

**Syntax*

```http
PUT https://{endpoint}/{policy_id}
```

**Example request**

```http
PUT / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
            "name": "tf-policy-10",
            "backupPolicy": {
                "regular": {
                    "incremental": {
                        "schedule": {
                            "unit": "Days",
                            "daySchedule": {
                                "frequency": 2
                            }
                        }
                    },
                    "retention": {
                        "unit": "Weeks",
                        "duration": 5
                    },
                    "primaryBackupTarget" : {
                        "useDefaultBackupTarget" : true
                    }
                }
            },
            "retryOptions": {
                "retries": 3,
                "retryIntervalMins": 5
            }
}
```



## DELETE Policy
{: #compatibility-api-delete-protection-policy-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="DELETE Policy" caption-side="bottom"}

**Syntax**

```http
DELETE https://{endpoint}/{policy_id}
```

## Datasource Connections
{: #compatibility-api-datasource-connections-agent-tasks}

### List Connections
{: #compatibility-api-list-datasource-connections-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be
specified. |
{: caption="List Connections" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-opt-query-parms-list-datasource-connections-agent-tasks}

| Query Parameter | Value | Required | Description
|------|-------------|-------------|-------------|
| connectionIds | - | no | Specifies the unique IDs of the connections which are to be fetched. |
| connectionNames | - | no | Specifies the names of the connections which are to be fetched. |
{: caption="Optional query parameters" caption-side="bottom"}

**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```

## Create Connection
{: #compatibility-api-create-connection-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="Create Connection" caption-side="bottom"}

### Payload
{: #compatibility-api-payload-create-connection-agent-tasks}

| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| connection_name | Specifies the name of the connection. For a given tenant, different connections can't have the same name. However, two (or more) different tenants can each have a connection with the same name. | `string` | - | - | - |
{: caption="Body of the request schema" caption-side="bottom"}



**Syntax**

```http
POST https://{endpoint}/
```

**Example request**

```http
POST / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
 {

            "connectionName": "test-conn-1",
}
```

## PUT Connection
{: #compatibility-api-put-connection-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="PUT Connection" caption-side="bottom"}

### Payload
{: #compatibility-api-payload-put-connection-agent-tasks}

| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| connection_name | Specifies the name of the connection. For a given tenant, different connections can't have the same name. However, two (or more) different tenants can each have a connection with the same name. | `string` | - | - | - |
{: caption="Payload" caption-side="bottom"}

**Syntax**

```http
PUT https://{endpoint}/{connection_id}
```

**Example request**

```http
PUT / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
 {
            "connectionName": "test-conn-2",
}
```



## DELETE Connection
{: #compatibility-api-delete-connection-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="DELETE Connection" caption-side="bottom"}

**Syntax**

```http
DELETE https://{endpoint}/{connection_id}

```

## Protection Group
{: #compatibility-api-protection-group-agent-tasks}

### Get Group
{: #compatibility-api-get-protection-group-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
| requestInitiatorType| `string` | No | Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests. 
{: caption="Get Group" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-opt-query-parms-get-protection-group-agent-tasks}

| Query Parameter | Description | Type | Required | Value
|------|-------------|-------------|-------------|----|
| id | Specifies a unique id of the Protection Group. | `string` | true | - |
| includeLastRunInfo | If true, the response will include last run info. If it is false or not specified, the last run info won't be returned. | `bool` | false | - |
| pruneExcludedSourceIds | If true, the response will not include the list of excluded source IDs in groups that contain this field. This can be set to true in order to improve performance if excluded source IDs are not needed by the user. | `bool` | false | - |
| pruneSourceIds | If true, the response will exclude the list of source IDs within the group specified. | `bool` | false | - |
{: caption="Optional query parameters" caption-side="bottom"}

**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```

## List Groups
{: #compatibility-api-list-groups-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
| request_initiator_type| string | No | Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests. | 
{: caption="List Groups" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-opt-query-parms-list-groups-agent-tasks}

| Query Parameter | Value | Required | Description

|------|-------------|-------------|-------------|
| include_last_run_info | - | No | If true, the response will include last run info. If it is false or not specified, the last run info won't be returned. | 
| prune_excluded_source_ids | - | No | If true, the response will not include the list of excluded source IDs in groups that contain this field. This can be set to true in order to improve performance if excluded source IDs are not needed by the user. |
| prune_source_ids | - | No | If true, the response will exclude the list of source IDs within the group specified. | 
{: caption="Optional query parameters" caption-side="bottom"}


**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```


## Create Group
{: #compatibility-api-create-group-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="Create Group" caption-side="bottom"}

| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| name | Specifies the name of the Protection Group. | `string` | - | - | - |
| policyId | Specifies the unique id of the Protection Policy associated with the Protection Group. The Policy provides retry settings Protection Schedules, Priority, SLA, etc. | `string` | - | - | - |
| priority | Specifies the priority of the Protection Group. | `string` | - | - | `kLow`, `kMedium`, `kHigh` |
| description | Specifies a description of the Protection Group. | `string` | - | - | - |
| startTime | Specifies the time of day. Used for scheduling purposes. | `object` | - | - | `hour`, `minute`, `timeZone` |
| endTimeUsecs | Specifies the end time in micro seconds for this Protection Group. If this is not specified, the Protection Group won't be ended. | `number` | - | - | - |
| lastModifiedTimestampUsecs | Specifies the last time this protection group was updated. If this is passed into a PUT request, then the backend will validate that the timestamp passed in matches the time that the protection group was actually last modified. If the two timestamps do not match, then the request will be rejected with a stale error. | `number` | - | - | - |
| alertPolicy | Specifies a policy for alerting users of the status of a Protection Group. | `object` | `backupRunStatus`, `alertTargets`, `raiseObjectLevelFailureAlert`, `raiseObjectLevelFailureAlertAfterLastAttempt`, `raiseObjectLevelFailureAlertAfterEachAttempt` | - | - | 
| sla | Specifies the SLA parameters for this Protection Group. | `list()` | - | - | - |
| qos_policy | Specifies whether the Protection Group will be written to HDD or SSD. | `string` | - | - | `kBackupHDD`, `kBackupSSD`, `kTestAndDevHigh`, `kBackupAll` |
| abortInBlackouts | Specifies whether currently executing jobs should abort if a blackout period specified by a policy starts. Available only if the selected policy has at least one blackout period. Default value is false. | `bool` | - | - | - |
| pause_in_blackouts | Specifies whether currently executing jobs should be paused if a blackout period specified by a policy starts. Available only if the selected policy has at least one blackout period. Default value is false. This field should not be set to true if 'abortInBlackouts' is sent as true. | `bool`| - | - | - |
| isPaused | Specifies if the the Protection Group is paused. New runs are not scheduled for the paused Protection Groups. Active run if any is not impacted. | `bool` | - | - | - |
| environment | Specifies the environment of the Protection Group. | `string` | - | - | `kPhysical`, `kSQL` |
| advancedConfigs | Specifies the advanced configuration for a protection job. | `list()` | `key`, `value` |
| physical_params | Specifies the parameters for Physical object. | `object` | `protectionType`, `volumeProtectionTypeParams`, `fileProtectionTypeParams` | - | -|
| mssqlParams | Specifies the parameters specific to MSSQL Protection Group. | `object` | `protectionType`, `fileProtectionTypeParams`, `nativeProtectionTypeParams`, `volumeProtectionTypeParams` | - | - |
{: caption="Create Group elements" caption-side="bottom"}


**Syntax**

```http
POST https://{endpoint}/
```

**Example request**

```http
POST / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
    "name" : "test-pg-1",
    "policyId": "5901263190628181:1725393921826:8120",
    "environment": "kPhysical",
    "physicalParams": {
                "protectionType": "kFile",
                "fileProtectionTypeParams": {
                    "objects": [{
                        "id" : 23,
                        "filePaths": [
                            {
                                "includedPath": "/data/"
                            }
                        ]
                    }]
                }
            }
}
```



## PUT Group
{: #compatibility-api-put-group-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="PUT Group" caption-side="bottom"}


| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| name | Specifies the name of the Protection Group. | `string` | - | - | - |
| policyId | Specifies the unique id of the Protection Policy associated with the Protection Group. The Policy provides retry settings Protection Schedules, Priority, SLA, etc. | `string` | - | - | - |
| priority | Specifies the priority of the Protection Group. | `string` | - | - | `kLow`, `kMedium`, `kHigh` |
| description | Specifies a description of the Protection Group. | `string` | - | - | - |
| startTime | Specifies the time of day. Used for scheduling purposes. | `object` | - | - | `hour`, `minute`, `timeZone` |
| endTimeUsecs | Specifies the end time in micro seconds for this Protection Group. If this is not specified, the Protection Group won't be ended. | `number` | - | - | - |
| lastModifiedTimestampUsecs | Specifies the last time this protection group was updated. If this is passed into a PUT request, then the backend will validate that the timestamp passed in matches the time that the protection group was actually last modified. If the two timestamps do not match, then the request will be rejected with a stale error. | `number` | - | - | - |
| alertPolicy | Specifies a policy for alerting users of the status of a Protection Group. | `object` | `backupRunStatus`, `alertTargets`, `raiseObjectLevelFailureAlert`, `raiseObjectLevelFailureAlertAfterLastAttempt`, `raiseObjectLevelFailureAlertAfterEachAttempt` | - | - | 
| sla | Specifies the SLA parameters for this Protection Group. | `list()` | - | - | - |
| qos_policy | Specifies whether the Protection Group will be written to HDD or SSD. | `string` | - | - | `kBackupHDD`, `kBackupSSD`, `kTestAndDevHigh`, `kBackupAll` |
| abortInBlackouts | Specifies whether currently executing jobs should abort if a blackout period specified by a policy starts. Available only if the selected policy has at least one blackout period. Default value is false. | `bool` | - | - | - |
| pause_in_blackouts | Specifies whether currently executing jobs should be paused if a blackout period specified by a policy starts. Available only if the selected policy has at least one blackout period. Default value is false. This field should not be set to true if 'abortInBlackouts' is sent as true. | `bool`| - | - | - |
| isPaused | Specifies if the the Protection Group is paused. New runs are not scheduled for the paused Protection Groups. Active run if any is not impacted. | `bool` | - | - | - |
| environment | Specifies the environment of the Protection Group. | `string` | - | - | `kPhysical`, `kSQL` |
| advancedConfigs | Specifies the advanced configuration for a protection job. | `list()` | `key`, `value` |
| physical_params | Specifies the parameters for Physical object. | `object` | `protectionType`, `volumeProtectionTypeParams`, `fileProtectionTypeParams` | - | -|
| mssqlParams | Specifies the parameters specific to MSSQL Protection Group. | `object` | `protectionType`, `fileProtectionTypeParams`, `nativeProtectionTypeParams`, `volumeProtectionTypeParams` | - | - |
{: caption="PUT Group elements" caption-side="bottom"}

**Syntax**

```http
PUT https://{endpoint}/{policy_id}
```

**Example request**

```http
PUT / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
    "name" : "tf-pg-1",
    "policyId": "5901263190628181:1725393921826:8120",
    "environment": "kPhysical",
    "physicalParams": {
                "protectionType": "kFile",
                "fileProtectionTypeParams": {
                    "objects": [{
                        "id" : 23,
                        "filePaths": [
                            {
                                "includedPath": "/data/service/"
                            }
                        ]
                    }]
                }
            }
}
```



## DELETE Group
{: #compatibility-api-delete-group-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="DELETE Group" caption-side="bottom"}

**Syntax**

```http
DELETE https://{endpoint}/{group_id}
```


## Recovery
{: #compatibility-api-recovey-agent-tasks}

### Get Recovery
{: #compatibility-api-get-recovey-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
| requestInitiatorType| `string` | No | Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests. 
{: caption="Get Recovery" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-opt-query-parms-get-recovey-agent-tasks}

| Query Parameter | Description | Type | Required | Value
|------|-------------|-------------|-------------|----|
| id | Specifies the id of a Recovery. | `string` | true | - |
{: caption="Optional query parameters" caption-side="bottom"}

**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```


## List Recoveries
{: #compatibility-api-list-recoveries-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="Body of the request schema" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-opt-query-parms-list-recoveries-agent-tasks}

| Query Parameter | Description | Value | Required |
|------|-------------|-------------|-------------|
| ids | Filter Recoveries for given ids.| - | false |
| returnOnlyChildRecoveries | Returns only child recoveries if passed as true. This filter should always be used along with 'ids' filter. | - | false |
| startTimeUsecs | Returns the recoveries which are started after the specific time. This value should be in Unix timestamp epoch in microseconds. | - | false |
| endTimeUsecs | Returns the recoveries which are started before the specific time. This value should be in Unix timestamp epoch in microseconds. | - | false |
| snapshotTargetType | Specifies the snapshot's target type from which recovery has been performed. | - | false |
| archivalTargetType | Specifies the snapshot's archival target type from which recovery has been performed. This parameter applies only if 'snapshotTargetType' is 'Archival'. | - | false |
| snapshotEnvironments | Specifies the list of snapshot environment types to filter Recoveries. If empty, Recoveries related to all environments will be returned. | - | false |
| status | Specifies the list of run status to filter Recoveries. If empty, Recoveries with all run status will be returned. | - | false |
| recoveryActions | Specifies the list of recovery actions to filter Recoveries. If empty, Recoveries related to all actions will be returned. | - | false |
{: caption="Optional query parameters" caption-side="bottom"}


**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```


## Create Recovery
{: #compatibility-api-create-recovery-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
| requestInitiatorType | string | No | Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests. |
{: caption="Create Recovery" caption-side="bottom"}


| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| snapshotEnvironment | Specifies the type of snapshot environment for which the Recovery was performed. | `string` | - | - | `kPhysical`, `kSQL` |
| physicalParams | Specifies the recovery options specific to Physical environment. | `object` | `objects`, `recoveryAction`, `recoverVolumeParams`, `recoverFileAndFolderParams`, `mountVolumeParams`, `downloadFileAndFolderParams`, systemRecoveryParams | - | - |
| mssqlParams | Specifies the recovery options specific to Sql environment. | `object` | `recoveryAction`, `recoverAppParams`, `vlanConfig` | - | - |
{: caption="Create Recovery elements" caption-side="bottom"}

**Syntax**

```http
POST https://{endpoint}/
```

**Example request**

```http
POST / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
    "name" : "test-recovery-1",
    "snapshotEnvironment": "kPhysical",
    "physicalParams": {
        "recoveryAction": "RecoverFiles",
        "objects": [
            {
                "snapshotId": "1"
            }
        ],
        "recoverFileAndFolderParams" : {
            "targetEnvironment" : "kPhysical",
            "filesAndFolders" : {
                "absolutePath" : "/data/"
            },
            "physicalTargetParams" :{
                "recoverTarget":{
                    "id" : 23
                }
            },
            "restoreEntityType" : "kRegular",
            "alternateRestoreDirectory": "/data/"
        }
    }
}
```


## Create Download Files and Folders Recovery
{: #compatibility-api-create-download-files-folders-recovery-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
| requestInitiatorType | string | No | Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests. |
{: caption="Create Download Files and Folders Recovery" caption-side="bottom"}


| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| documents | Specifies the list of documents to download using item ids. Only one of filesAndFolders or documents should be used. Currently only files are supported by documents. | `list()` | `itemId`, `isDirectory` | - | - |
| name | Specifies the name of the recovery task. This field must be set and must be a unique name. | `string` | - | - | - |
| object | Specifies the common snapshot parameters for a protected object. | `object` | `snapshotId`, `pointInTimeUsecs`, `protectionGroupId`, `protectionGroupName`, `snapshotCreationTimeUsecs`, `objectInfo`, `snapshotTargetType`, `archivalTargetInfo`, `progressTaskId`, `recoverFromStandby`, `status`, `startTimeUsecs`, `endTimeUsecs`, `messages`, `bytesRestored` | - | - |
| parentRecoveryId | If current recovery is child task triggered through another parent recovery operation, then this field will specify the id of the parent recovery. | `string` | - | - | - |
| filesAndFolders | Specifies the list of files and folders to download. | `list()` | `absolutePath`, `isDirectory` | - | - |
| glacierRetrievalType | Specifies the glacier retrieval type when restoring or downloding files or folders from a Glacier-based cloud snapshot. | `string` | - | - | `kStandard`, `kExpeditedNoPCU`, `kExpeditedWithPCU` |
{: caption="Create Download Files and Folders Recovery elements" caption-side="bottom"}

**Syntax**

```http
POST https://{endpoint}/
```

**Example request**

```http
POST / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
    "name" : "test-recovery-1",
    "object":
        {
            "snapshotId": "1"
        },
    "filesAndFolders" :
        {
            "absolutePath" : "/data/"
        },
}
```


## Download Files From Recovery
{: #compatibility-api-create-download-files-recovery-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
| requestInitiatorType| `string` | No | Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests. 
{: caption="Download Files From Recovery" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-opt-query-parms-create-download-files-recovery-agent-tasks}

| Query Parameter | Description | Type | Required | Value
|------|-------------|-------------|-------------|----|
| snapshotsId | Specifies the snapshot id to download from. | `string` | true | - |
| filePath | Specifies the path to the file to download. If no path is specified and snapshot environment is kVMWare, VMX file for VMware will be downloaded. For other snapshot environments, this field must be specified. | `string` | false | - |
| nvramFile | Specifies if NVRAM file for VMware should be downloaded. | `bool` | false | - |
| retryAttempt | Specifies the number of attempts the protection run took to create this file. | `number` | false | - |
| startOffset | Specifies the start offset of file chunk to be downloaded. | `number` | false | - |
| length | Specifies the length of bytes to download. This can not be greater than 8MB (8388608 byets). | `number` | false | - |
{: caption="Optional query parameters" caption-side="bottom"}

**Syntax**

```http
GET https://{endpoint}/
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```


## Protection Group Run
{: #compatibility-api-protection-group-run-agent-tasks}

### List Group Runs
{: #compatibility-api-list-protection-group-run-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
| request_initiator_type| string | No | Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests. | 
{: caption="List Group Runs" caption-side="bottom"}

### Optional query parameters
{: #compatibility-api-opt-query-parms-list-protection-group-run-agent-tasks}

| Query Parameter | Description | Required |  value |
|------|-------------|-------------|-------------|
| protection_group_runs_id | Specifies a unique id of the Protection Group. | true | - |
| run_id | Specifies the protection run id. | false | - |
| start_time_usecs | Start time for time range filter. Specify the start time as a Unix epoch Timestamp (in microseconds), only runs executing after this time will be returned. By default it is endTimeUsecs minus an hour. | false | - |
| end_time_usecs | End time for time range filter. Specify the end time as a Unix epoch Timestamp (in microseconds), only runs executing before this time will be returned. By default it is current time. | false | - |
| run_types | Filter by run type. Only protection run matching the specified types will be returned. | false | - |
| include_object_details | Specifies if the result includes the object details for each protection run. If set to true, details of the protected object will be returned. If set to false or not specified, details will not be returned. | false | - |
| local_backup_run_status | Specifies a list of local backup status, runs matching the status will be returned.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped. | false | - |
| replication_run_status | Specifies a list of replication status, runs matching the status will be returned.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped. | false | - |
| archival_run_status | Specifies a list of archival status, runs matching the status will be returned.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped. | false | - |
| cloud_spin_run_status | Specifies a list of cloud spin status, runs matching the status will be returned.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped. | false | - |
| num_runs | Specifies the max number of runs. If not specified, at most 100 runs will be returned. | false | - |
| exclude_non_restorable_runs | Specifies whether to exclude non restorable runs. Run is treated restorable only if there is atleast one object snapshot (which may be either a local or an archival snapshot) which is not deleted or expired. Default value is false. | false | - |
| run_tags | Specifies a list of tags for protection runs. If this is specified, only the runs which match these tags will be returned. | false | - |
| use_cached_data | Specifies whether we can serve the GET request from the read replica cache. There is a lag of 15 seconds between the read replica and primary data source. | false | - |
| filter_by_end_time | If true, the runs with backup end time within the specified time range will be returned. Otherwise, the runs with start time in the time range are returned. | false | - |
| snapshot_target_types | Specifies the snapshot's target type which should be filtered. | false | - |
| only_return_successful_copy_run | only successful copyruns are returned. | false | - |
| filter_by_copy_task_end_time | If true, then the details of the runs for which any copyTask completed in the given timerange will be returned. Only one of filterByEndTime and filterByCopyTaskEndTime can be set. | false | - |
{: caption="Optional query parameters" caption-side="bottom"}


**Syntax**

```http
GET https://{endpoint}/{group_id}/runs
```

**Example request**

```http
GET / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
```


## Create Group Run
{: #compatibility-api-create-group-run-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="Create Group Run" caption-side="bottom"}

| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| run_type | Type of protection run. 'kRegular' indicates an incremental (CBT) backup. Incremental backups utilizing CBT (if supported) are captured of the target protection objects. The first run of a kRegular schedule captures all the blocks. 'kFull' indicates a full (no CBT) backup. A complete backup (all blocks) of the target protection objects are always captured and Change Block Tracking (CBT) is not utilized. 'kLog' indicates a Database Log backup. Capture the database transaction logs to allow rolling back to a specific point in time. 'kSystem' indicates system volume backup. It produces an image for bare metal recovery. | `string` | - | - | `kRegular`, `kFull`, `kLog`, `kSystem`, `kHydrateCDP`, `kStorageArraySnapshot` |
| objects | Specifies the list of objects to be protected by this Protection Group run. These can be leaf objects or non-leaf objects in the protection hierarchy. This must be specified only if a subset of objects from the Protection Groups needs to be protected. | `list()` | `id`, `appIds`, `physicalParams` | - | - |
| targets_config | Specifies the replication and archival targets. | `object` | `usePolicyDefaults`, `replications`, `archivals`, `cloudReplications` | - | - |
{: caption="Create Group Run elements" caption-side="bottom"}


**Syntax**

```http
POST https://{endpoint}/{group_id}/runs
```

**Example request**

```http
POST / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
    "runType" : "kRegular"
}
```

## PUT Group Run
{: #compatibility-api-put-group-run-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="Body of the request schema" caption-side="bottom"}


| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| updateProtectionGroupRunParams | Update Parameters | `list()` | `runId`, `localSnapshotConfig`, `replicationSnapshotConfig`, `archivalSnapshotConfig` | - | - |
{: caption="Body of the request schema" caption-side="bottom"}

**Syntax**

```http
PUT https://{endpoint}/{group_id}/runs
```

**Example request**
```
PUT / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
    "UpdateProtectionGroupRunParams": [
        {
            "runId": "11261:1719755265289584",
            "localSnapshotConfig" : {
                "deleteSnapshot" : false
            }
        }
    ]
}
```

## POST Group Run Action
{: #compatibility-api-post-group-run-agent-tasks}

| Header | Type | Required | Description
|------|-------------|-------------|-------------|
| X-IBM-Tenant-ID | `string` | yes | Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified. |
{: caption="POST Group Run Action" caption-side="bottom"}


| Element | Description | Type | Children | Ancestor | Constraint
|------|-------------|------|---------|---------|---------|
| updateProtectionGroupRunParams | Update Parameters | `list()` | `action`, `pauseParams`, `pauseParams`, `resumeParams`, `cancelParams` | - | - |
{: caption="POST Group Run Action elements" caption-side="bottom"}

**Syntax**

```http
POST https://{endpoint}/{group_id}/runs/actions
```

**Example request**

```http
PUT / HTTP/1.1
Authorization: Bearer {token}
Host: protectiondomain0302.us-east.backup-recovery-tests.cloud.ibm.com
X-IBM-Tenant-ID: 79mle1bk3m/
Body:
{
    "action": "Cancel",
    "cancelParams":[{
        "runId": "11261:1719755265289584",
        "localTaskId": "4827366365722429:1717507647142:3703265"
    }]
}
```


----

