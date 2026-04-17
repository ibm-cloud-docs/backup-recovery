---

copyright:
  years: 2024, 2026
lastupdated: "2026-04-17"

keywords: backup recovery, cli, guide

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

# {{site.data.keyword.cloud_notm}} Backup Recovery CLI
{: #backup-recovery-cli-intro}

The {{site.data.keyword.baas_full_notm}} plug-in extends the {{site.data.keyword.cloud_notm}} command line interface (CLI) with an API wrapper for working with backup and recovery.

## Installation and configuration
{: #backup-recovery-install-config}

The plug-in is compatible with Linux (x86_64 and arm64), Windows® (x64), and macOS® (amd64 and arm64) platforms that run on 64-bit processors.

## Install the plug-in by using the plug-in install command.
{: #backup-recovery-install-plug-in}

 ```sh
ibmcloud plugin install backup-recovery
 ```

## Enable tracing in the command line interface

Tracing can be enabled by setting `IBMCLOUD_TRACE` environment variable to `true` (case ignored). When trace is enabled, more debugging information is printed to the terminal.

On Linux/macOS terminal:
```sh
export IBMCLOUD_TRACE=true
```

On the Windows prompt:
```sh
SET IBMCLOUD_TRACE=true
```
To disable tracing, set the `IBMCLOUD_TRACE` environment variable to `false` (case ignored).

### `ibmcloud backup-recovery config`
{: #backup-recovery-cli-config-command}

Global parameters can also be stored in a persistent configuration so that they do not need to be manually specified each time the plug-in is invoked. Each parameter can be configured with the `config` command and its subcommands.

```sh
ibmcloud backup-recovery config
```

#### Understanding service-url
{: #backup-recovery-service-url-explained}

The `service-url` is the API endpoint (private or public) of your {{site.data.keyword.baas_full_notm}} instance. This URL is required to connect the CLI to your specific BRS instance.

**Service URL format:**
- **Private endpoint:** `https://<instance_ID>.private.<region>.backup-recovery.cloud.ibm.com/v2`
- **Public endpoint:** `https://<instance_ID>.<region>.backup-recovery.cloud.ibm.com/v2`

Where `<region>` is the IBM Cloud region where your Backup & Recovery instance is deployed (for example, `us-east`, `us-south`, `eu-de`, `eu-gb`, `eu-es`, `ca-tor`, `br-sao`, `jp-tok`, `jp-osa`, `au-syd`). For the complete list of supported regions and workload availability, see [Integrated service availability](/docs/backup-recovery?topic=backup-recovery-service-availability).

**To retrieve your instance endpoints:**

1. Run the following IBM Cloud CLI command:

   ```sh
   ibmcloud resource service-instance <INSTANCE_NAME_OR_ID> --output json
   ```
   {: pre}

2. Locate the endpoint information in the `extensions.endpoints` section of the JSON response:

   ```json
   {
       "extensions": {
           "endpoints": {
              "public": "<instance_ID>.us-east.backup-recovery.cloud.ibm.com",
              "private": "<instance_ID>.private.us-east.backup-recovery.cloud.ibm.com"
           }
       }
   }
   ```
   {: codeblock}

3. Add the `https://` prefix and append `/v2` suffix to the endpoint when setting the service-url.

For more details, see [Service endpoints](/docs/backup-recovery?topic=backup-recovery-endpoints).

#### Understanding management-reporting-service-url
{: #backup-recovery-management-reporting-url-explained}

The `management-reporting-service-url` (also referred to as the Backup Recovery Management URL) is the endpoint for the {{site.data.keyword.baas_full_notm}} Manager's reporting and monitoring APIs. This URL provides access to centralized reporting, monitoring, and alert management capabilities across all your backup and recovery instances in a region.

**Management Reporting Service URL format by region:**
```
  https://manager.<region>.backup-recovery.cloud.ibm.com/heliosreporting/api/v1
  ```

- **US region:**
  ```
  https://manager.us.backup-recovery.cloud.ibm.com/heliosreporting/api/v1
  ```

- **EU region:**
  ```
  https://manager.eu.backup-recovery.cloud.ibm.com/heliosreporting/api/v1
  ```

- **AP region:**
  ```
  https://manager.ap.backup-recovery.cloud.ibm.com/heliosreporting/api/v1
  ```

These are **fixed regional endpoints** that do not require an instance ID. They provide access to:
- Report components and report management
- Centralized monitoring dashboards
- Alert aggregation across instances
- Resource listings (policies, protection groups, registered sources)

For more information about the Backup and Recovery Manager and its reporting capabilities, see [Accessing the IBM Cloud Backup and Recovery Manager](/docs/backup-recovery?topic=backup-recovery-getting-started-backup-recovery-gmc).

### `ibmcloud backup-recovery config set`
{: #backup-recovery-cli-config-set-command}

Set a new config value for a specific option. Each subcommand of the `set` command maps to a global option. Each subcommand accepts a single argument, the string representation of the value to store for the option.

```sh
ibmcloud backup-recovery config set <option> <value>
```

#### Examples
{: #backup-recovery-config-set-command-examples}

Set the service-url:
```sh
ibmcloud backup-recovery config set service-url 'https://fae93403-de8e-45c3-972d-a3a3ad873a62.us-east.backup-recovery-tests.cloud.ibm.com/v2'
```
{: pre}

Expected output:
```
OK
```
{: screen}

Set the management-reporting-service-url (choose the URL for your region):

For US region:
```sh
ibmcloud backup-recovery config set management-reporting-service-url 'https://manager.backup-recovery.cloud.ibm.com/heliosreporting/api/v1'
```
{: pre}

For EU region:
```sh
ibmcloud backup-recovery config set management-reporting-service-url 'https://manager.eu.backup-recovery.cloud.ibm.com/heliosreporting/api/v1'
```
{: pre}

For AP region:
```sh
ibmcloud backup-recovery config set management-reporting-service-url 'https://manager.ap.backup-recovery.cloud.ibm.com/heliosreporting/api/v1'
```
{: pre}

Expected output:
```
OK
```
{: screen}

### `ibmcloud backup-recovery config get`
{: #backup-recovery-cli-config-get-command}

Print the currently set value for a specific option. Each subcommand of the `get` command maps to a global option.

```sh
ibmcloud backup-recovery config get <option>
```

#### Available options
{: #backup-recovery-config-get-options}

- `service-url` - The API endpoint for your Backup & Recovery instance
- `management-reporting-service-url` - The endpoint for Backup & Recovery Manager reporting APIs
- `connector-service-url` - The endpoint for connector services
- `management-sre-service-url` - The endpoint for management SRE services
- `management-sre-service-apikey` - API key for management SRE services
- `management-sre-service-username` - Username for management SRE services
- `management-sre-service-password` - Password for management SRE services
- `management-sre-service-authentication-url` - Authentication URL for management SRE services

#### Examples
{: #backup-recovery-config-get-command-examples}

Get the service-url:
```sh
ibmcloud backup-recovery config get service-url
```
{: pre}

Expected output:
```
https://fae93403-de8e-45c3-972d-a3a3ad873a62.us-east.backup-recovery-tests.cloud.ibm.com/v2
```
{: screen}

Get the management-reporting-service-url:
```sh
ibmcloud backup-recovery config get management-reporting-service-url
```
{: pre}

Expected output:
```
https://manager.backup-recovery.cloud.ibm.com/heliosreporting/api/v1
```
{: screen}

### `ibmcloud backup-recovery config unset`
{: #backup-recovery-cli-config-unset-command}

Unset the currently set value for a specific option. Each subcommand of the `unset` command maps to a global option.

The available options are the same as listed in the `config get` command above.

```sh
ibmcloud backup-recovery config unset <option>
```

#### Examples
{: #backup-recovery-config-unset-command-examples}

```sh
ibmcloud backup-recovery config unset service-url

ibmcloud backup-recovery config unset management-reporting-service-url
```
{: pre}

### `ibmcloud backup-recovery config list`
{: #backup-recovery-cli-config-list-command}

List out all of the currently set config values.

```sh
ibmcloud backup-recovery config list
```

#### Examples
{: #backup-recovery-config-list-command-examples}

```sh
ibmcloud backup-recovery config list
```
{: pre}

## Protection-source
{: #backup-recovery-protection-source-cli}

Commands for ProtectionSource resource.

```sh
ibmcloud backup-recovery protection-source --help
```


### `ibmcloud backup-recovery protection-source list`
{: #backup-recovery-cli-protection-source-list-command}

If no parameters are specified, all Protection Sources that are registered on the {{site.data.keyword.baas_full_notm}} Cluster are returned. In addition, an Object subtree gathered from each Source is returned. For example, the {{site.data.keyword.baas_full_notm}} Cluster interrogates a Source VMware vCenter Server and creates a hierarchical Object subtree that mirrors the Inventory tree on vCenter Server. The contents of the Object tree are returned as a "nodes" hierarchy of "protectionSource"s. Specifying parameters can alter the results that are returned.

```sh
ibmcloud backup-recovery protection-source list --xibm-tenant-id XIBM-TENANT-ID [--exclude-office365-types EXCLUDE-OFFICE365-TYPES] [--get-teams-channels=GET-TEAMS-CHANNELS] [--after-cursor-entity-id AFTER-CURSOR-ENTITY-ID] [--before-cursor-entity-id BEFORE-CURSOR-ENTITY-ID] [--node-id NODE-ID] [--page-size PAGE-SIZE] [--has-valid-mailbox=HAS-VALID-MAILBOX] [--has-valid-onedrive=HAS-VALID-ONEDRIVE] [--is-security-group=IS-SECURITY-GROUP] [--id ID] [--num-levels NUM-LEVELS] [--exclude-types EXCLUDE-TYPES] [--exclude-aws-types EXCLUDE-AWS-TYPES] [--exclude-kubernetes-types EXCLUDE-KUBERNETES-TYPES] [--include-datastores=INCLUDE-DATASTORES] [--include-networks=INCLUDE-NETWORKS] [--include-vm-folders=INCLUDE-VM-FOLDERS] [--include-sfdc-fields=INCLUDE-SFDC-FIELDS] [--include-system-v-apps=INCLUDE-SYSTEM-V-APPS] [--environments ENVIRONMENTS] [--environment ENVIRONMENT] [--include-entity-permission-info=INCLUDE-ENTITY-PERMISSION-INFO] [--sids SIDS] [--include-source-credentials=INCLUDE-SOURCE-CREDENTIALS] [--encryption-key ENCRYPTION-KEY] [--include-object-protection-info=INCLUDE-OBJECT-PROTECTION-INFO] [--prune-non-critical-info=PRUNE-NON-CRITICAL-INFO] [--prune-aggregation-info=PRUNE-AGGREGATION-INFO] [--request-initiator-type REQUEST-INITIATOR-TYPE] [--use-cached-data=USE-CACHED-DATA] [--all-under-hierarchy=ALL-UNDER-HIERARCHY]
```


#### Command options
{: #backup-recovery-protection-source-list-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--exclude-office365-types` ([]string)
:   Specifies the Object types to be filtered out for Office 365 that match the passed in types such as 'kDomain', 'kOutlook', 'kMailbox', etc. For example, set this parameter to 'kMailbox' to exclude Mailbox Objects from being returned.

    Allowable list items are: `kDomain`, `kOutlook`, `kMailbox`, `kUsers`, `kUser`, `kGroups`, `kGroup`, `kSites`, `kSite`.

`--get-teams-channels` (bool)
:   Filter policies by a list of policy ids.

`--after-cursor-entity-id` (int64)
:   Specifies the entity id starting from which the items are to be returned.

`--before-cursor-entity-id` (int64)
:   Specifies the entity id up to which the items are to be returned.

`--node-id` (int64)
:   Specifies the entity id for the Node at any level within the Source entity hierarchy whose children are to be paginated.

`--page-size` (int64)
:   Specifies the maximum number of entities to be returned within the page.

`--has-valid-mailbox` (bool)
:   If set to true, users with valid mailbox will be returned.

`--has-valid-onedrive` (bool)
:   If set to true, users with valid onedrive will be returned.

`--is-security-group` (bool)
:   If set to true, Groups that are security enabled will be returned.

`--id` (int64)
:   Return the Object subtree for the passed in Protection Source id.

`--num-levels` (float64)
:   Specifies the expected number of levels from the root node to be returned in the entity hierarchy response.

`--exclude-types` ([]string)
:   Filter out the Object types (and their subtrees) that match the passed in types such as 'kVCenter', 'kFolder', 'kDatacenter', 'kComputeResource', 'kResourcePool', 'kDatastore', 'kHostSystem', 'kVirtualMachine', etc. For example, set this parameter to 'kResourcePool' to exclude Resource Pool Objects from being returned.

    Allowable list items are: `kVCenter`, `kFolder`, `kDatacenter`, `kComputeResource`, `kClusterComputeResource`, `kResourcePool`, `kDatastore`, `kHostSystem`, `kVirtualMachine`, `kVirtualApp`, `kStandaloneHost`, `kStoragePod`, `kNetwork`, `kDistributedVirtualPortgroup`, `kTagCategory`, `kTag`.

`--exclude-kubernetes-types` ([]string)
:   Specifies the Object types to be filtered out for Kubernetes that match the passed in types such as 'kService'. For example, set this parameter to 'kService' to exclude services from being returned.

    Allowable list items are: `kService`.

`--include-datastores` (bool)
:   Set this parameter to true to also return kDatastore object types that are found in the Source in addition to their Object subtrees. By default, datastores are not returned.

`--include-networks` (bool)
:   Set this parameter to true to also return kNetwork object types that are found in the Source in addition to their Object subtrees. By default, network objects are not returned.

`--include-vm-folders` (bool)
:   Set this parameter to true to also return kVMFolder object types that are found in the Source in addition to their Object subtrees. By default, VM folder objects are not returned.

`--include-sfdc-fields` (bool)
:   Set this parameter to true to also return fields of the object found in the Source in addition to their Object subtrees. By default, Sfdc object fields are not returned.

`--include-system-v-apps` (bool)
:   Set this parameter to true to also return system VApp object types that are found in the Source in addition to their Object subtrees. By default, VM folder objects are not returned.

`--environments` ([]string)
:   Return only Protection Sources that match the passed in environment type such as 'kVMware', 'kSQL', 'kView' 'kPhysical', 'kPuppeteer', 'kPure', 'kNetapp', 'kGenericNas', 'kHyperV', 'kAcropolis'. For example, set this parameter to 'kVMware' to only return the Sources (and their Object subtrees) found in the 'kVMware' (VMware vCenter Server) environment.

    Allowable list items are: `kVMware`, `kHyperV`, `kSQL`, `kView`, `kPuppeteer`, `kPhysical`, `kPure`, `kNimble`, `kNetapp`, `kAgent`, `kGenericNas`, `kAcropolis`, `kPhysicalFiles`, `kIsilon`, `kGPFS`, `kKVM`, `kExchange`, `kHyperVVSS`, `kOracle`, `kFlashBlade`, `kO365`, `kO365Outlook`, `kHyperFlex`, `kKubernetes`, `kElastifile`, `kAD`, `kCassandra`, `kMongoDB`, `kCouchbase`, `kHdfs`, `kHBase`, `kUDA`, `kSfdc`.

`--environment` (string)
:   This field is deprecated. Use environments instead.

`--include-entity-permission-info` (bool)
:   If specified, then a list of entities with permissions that are assigned to them are returned.

`--sids` ([]string)
:   Filter the object subtree for the sids given in the list.

`--include-source-credentials` (bool)
:   If specified, then credential for the registered sources will be included. The credential is first encrypted with internal key and then reencrypted with user supplied 'encryption_key'.

`--encryption-key` (string)
:   Key to be used to encrypt the source credential. If include_source_credentials is set to true this key must be specified.

`--include-object-protection-info` (bool)
:   If specified, the object protection of entities(if any) will be returned.

`--prune-non-critical-info` (bool)
:   Specifies whether to prune noncritical information within entities. In case of VMs, virtual disk information is pruned. In case of Office365, metadata about user entities is pruned. This can be used to limit the size of the response by caller.

`--prune-aggregation-info` (bool)
:   Specifies whether to prune the aggregation information about the number of entities protected/unprotected.

`--request-initiator-type` (string)
:   Specifies the type of the request. Possible values are UIUser and UIAuto, which means the request is triggered by the user or is an auto refresh request. Services like magneto use this to determine the priority of the requests, so that it can more intelligently handle overload situations by prioritizing higher priority requests.

`--use-cached-data` (bool)
:   Specifies whether we can serve the GET request to the read replica cache. Setting this to true helps ensure that the API request is served to the read replica. Setting this to false will serve the request to the master.

`--all-under-hierarchy` (bool)
:   AllUnderHierarchy specifies whether objects of all the tenants under the hierarchy of the logged in user's organization should be returned.

#### Example
{: #backup-recovery-protection-source-list-examples}

```sh
ibmcloud backup-recovery protection-source list \
    --xibm-tenant-id tenantId \
    --exclude-office365-types kDomain,kOutlook,kMailbox,kUsers,kUser,kGroups,kGroup,kSites,kSite \
    --get-teams-channels=true \
    --after-cursor-entity-id 26 \
    --before-cursor-entity-id 26 \
    --node-id 26 \
    --page-size 26 \
    --has-valid-mailbox=true \
    --has-valid-onedrive=true \
    --is-security-group=true \
    --id 26 \
    --num-levels 72.5 \
    --exclude-types kVCenter,kFolder,kDatacenter,kComputeResource,kClusterComputeResource,kResourcePool,kDatastore,kHostSystem,kVirtualMachine,kVirtualApp,kStandaloneHost,kStoragePod,kNetwork,kDistributedVirtualPortgroup,kTagCategory,kTag \
    --exclude-kubernetes-types kService \
    --include-datastores=true \
    --include-networks=true \
    --include-vm-folders=true \
    --include-sfdc-fields=true \
    --include-system-v-apps=true \
    --environments kVMware,kHyperV,kSQL,kView,kPuppeteer,kPhysical,kPure,kNimble,kNetapp,kAgent,kGenericNas,kAcropolis,kPhysicalFiles,kIsilon,kGPFS,kKVM,kExchange,kHyperVVSS,kOracle,kFlashBlade,kO365,kO365Outlook,kHyperFlex,kKubernetes,kElastifile,kAD,kCassandra,kMongoDB,kCouchbase,kHdfs,kHBase,kUDA,kSfdc \
    --environment kPhysical \
    --include-entity-permission-info=true \
    --sids sid1 \
    --include-source-credentials=true \
    --encryption-key encryptionKey \
    --include-object-protection-info=true \
    --prune-non-critical-info=true \
    --prune-aggregation-info=true \
    --request-initiator-type requestInitiatorType \
    --use-cached-data=true \
    --all-under-hierarchy=true
```
{: pre}

### `ibmcloud backup-recovery protection-source registrations-list`
{: #backup-recovery-cli-protection-source-registrations-list-command}

Get the list of Protection Source registrations.

```sh
ibmcloud backup-recovery protection-source registrations-list --xibm-tenant-id XIBM-TENANT-ID [--ids IDS] [--include-source-credentials=INCLUDE-SOURCE-CREDENTIALS] [--encryption-key ENCRYPTION-KEY] [--use-cached-data=USE-CACHED-DATA] [--include-external-metadata=INCLUDE-EXTERNAL-METADATA] [--ignore-tenant-migration-in-progress-check=IGNORE-TENANT-MIGRATION-IN-PROGRESS-CHECK]
```


#### Command options
{: #backup-recovery-protection-source-registrations-list-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--ids` ([]int64)
:   Ids specify the list of source registration ids to return. If left empty, every source registration is returned by default.

`--include-source-credentials` (bool)
:   If true, the encrypted credential for the registered sources will be included. The credential is first encrypted with internal key and then reencrypted with user-supplied encryption key.

`--encryption-key` (string)
:   Specifies the key to be used to encrypt the source credential. If includeSourceCredentials is set to true this key must be specified.

`--use-cached-data` (bool)
:   Specifies whether we can serve the GET request from the read replica cache. There is a lag of 15 seconds between the read replica and the primary data source.

`--include-external-metadata` (bool)
:   If true, the external entity metadata like maintenance mode config for the registered sources will be included.

`--ignore-tenant-migration-in-progress-check` (bool)
:   If true, the tenant migration check is ignored.

#### Example
{: #backup-recovery-protection-source-registrations-list-examples}

```sh
ibmcloud backup-recovery protection-source registrations-list \
    --xibm-tenant-id tenantId \
    --ids 38,39 \
    --include-source-credentials=true \
    --encryption-key encryptionKey \
    --use-cached-data=true \
    --include-external-metadata=true \
    --ignore-tenant-migration-in-progress-check=true
```
{: pre}

### `ibmcloud backup-recovery protection-source register`
{: #backup-recovery-cli-protection-source-register-command}

Register a Protection Source.

```sh
ibmcloud backup-recovery protection-source register --xibm-tenant-id XIBM-TENANT-ID --environment ENVIRONMENT [--name NAME] [--is-internal-encrypted=IS-INTERNAL-ENCRYPTED] [--encryption-key ENCRYPTION-KEY] [--connection-id CONNECTION-ID] [--connections CONNECTIONS] [--connector-group-id CONNECTOR-GROUP-ID] [--advanced-configs ADVANCED-CONFIGS] [--data-source-connection-id DATA-SOURCE-CONNECTION-ID] [--physical-params PHYSICAL-PARAMS | --physical-params-endpoint PHYSICAL-PARAMS-ENDPOINT --physical-params-force-register=PHYSICAL-PARAMS-FORCE-REGISTER --physical-params-host-type PHYSICAL-PARAMS-HOST-TYPE --physical-params-physical-type PHYSICAL-PARAMS-PHYSICAL-TYPE --physical-params-applications PHYSICAL-PARAMS-APPLICATIONS] [--kubernetes-params KUBERNETES-PARAMS | --kubernetes-params-auto-protect-config KUBERNETES-PARAMS-AUTO-PROTECT-CONFIG --kubernetes-params-client-private-key KUBERNETES-PARAMS-CLIENT-PRIVATE-KEY --kubernetes-params-cohesity-dataprotect-plugin-image-location KUBERNETES-PARAMS-COHESITY-DATAPROTECT-PLUGIN-IMAGE-LOCATION --kubernetes-params-data-mover-image-location KUBERNETES-PARAMS-DATA-MOVER-IMAGE-LOCATION --kubernetes-params-datamover-service-type KUBERNETES-PARAMS-DATAMOVER-SERVICE-TYPE --kubernetes-params-default-vlan-params KUBERNETES-PARAMS-DEFAULT-VLAN-PARAMS --kubernetes-params-endpoint KUBERNETES-PARAMS-ENDPOINT --kubernetes-params-init-container-image-location KUBERNETES-PARAMS-INIT-CONTAINER-IMAGE-LOCATION --kubernetes-params-kubernetes-distribution KUBERNETES-PARAMS-KUBERNETES-DISTRIBUTION --kubernetes-params-kubernetes-type KUBERNETES-PARAMS-KUBERNETES-TYPE --kubernetes-params-priority-class-name KUBERNETES-PARAMS-PRIORITY-CLASS-NAME --kubernetes-params-resource-annotations KUBERNETES-PARAMS-RESOURCE-ANNOTATIONS --kubernetes-params-resource-labels KUBERNETES-PARAMS-RESOURCE-LABELS --kubernetes-params-san-fields KUBERNETES-PARAMS-SAN-FIELDS --kubernetes-params-service-annotations KUBERNETES-PARAMS-SERVICE-ANNOTATIONS --kubernetes-params-velero-aws-plugin-image-location KUBERNETES-PARAMS-VELERO-AWS-PLUGIN-IMAGE-LOCATION --kubernetes-params-velero-image-location KUBERNETES-PARAMS-VELERO-IMAGE-LOCATION --kubernetes-params-velero-openshift-plugin-image-location KUBERNETES-PARAMS-VELERO-OPENSHIFT-PLUGIN-IMAGE-LOCATION --kubernetes-params-vlan-info-vec KUBERNETES-PARAMS-VLAN-INFO-VEC]
```


#### Command options
{: #backup-recovery-protection-source-register-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--environment` (string)
:   Specifies the environment type of the Protection Source. Required.

    Allowable values are: `kPhysical`, `kSQL`.

`--name` (string)
:   A user specified name for this source.

`--is-internal-encrypted` (bool)
:   Specifies whether credentials are encrypted by an internal key.

`--encryption-key` (string)
:   Specifies the key that the user has encrypted the credential with.

`--connection-id` (int64)
:   Specifies the id of the connection from where this source is reachable. This should only be set for a source being registered by a tenant user.

`--connections`
:   Specifies the list of connections for the source.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--connections=@path/to/file.json`.

`--connector-group-id` (int64)
:   Specifies the connector group id of connector groups.

`--kubernetes-params` (string)
:   Specifies the parameters to register a Kubernetes source. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--kubernetes-params=@path/to/file.json`.

`--kubernetes-params-auto-protect-config` (string)
:   Specifies the parameters to auto protect the source after registration. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-client-private-key` (string)
:   Specifies the bearer token or private key of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-cohesity-dataprotect-plugin-image-location` (string)
:   Specifies the custom {{site.data.keyword.baas_full}} Data protect plug-in image location of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-data-mover-image-location` (string)
:   Specifies the datamover image location of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-datamover-service-type` (string)
:   Specifies the data mover service type of Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

    Allowable values are: `kNodePort`, `kLoadBalancer`, `kClusterIp`.

`--kubernetes-params-default-vlan-params` (string)
:   Specifies VLAN params that is associated with the backup/restore operation. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-endpoint` (string)
:   Specifies the endpoint of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-init-container-image-location` (string)
:   Specifies the initial container image location of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-kubernetes-distribution` (string)
:   Specifies the distribution type of Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

    Allowable values are: `kOpenshift`, `kMainline`, `kVMwareTanzu`, `kRancher`, `kEKS`, `kGKE`, `kAKS`, `kIKS`, `kROKS`.

`--kubernetes-params-kubernetes-type` (string)
:   Specifies the type of Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

    Allowable values are: `kCluster`, `kNamespace`, `kService`, `kPVC`, `kPersistentVolumeClaim`, `kPersistentVolume`, `kLabel`.

`--kubernetes-params-priority-class-name` (string)
:   Specifies the priority class name for {{site.data.keyword.baas_full}} resources. This option provides a value for a sub-field of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-resource-annotations` (string)
:   Specifies resource annotations to be applied on {{site.data.keyword.baas_full}} resources. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-resource-labels` (string)
:   Specifies a resource label to be applied on {{site.data.keyword.baas_full}} resources. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-san-fields` (string)
:   Specifies the SAN field for agent certificate. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-service-annotations` (string)
:   Specifies the service annotation object of the Kubernetes source. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-velero-aws-plugin-image-location` (string)
:   Specifies the velero AWS plug-in image location of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-velero-image-location` (string)
:   Specifies the velero image location of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-velero-openshift-plugin-image-location` (string)
:   Specifies the velero open shift plug-in image for the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-vlan-info-vec` (string)
:   Specifies VLAN information that is provided during registration. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--advanced-configs`
:   Specifies the advanced configuration for a protection source.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--advanced-configs=@path/to/file.json`.

`--data-source-connection-id` (string)
:   Specifies the id of the connection from where this source is reachable. This should only be set for a source being registered by a tenant user. Also, this is the 'string' of connectionId. This property was added to accommodate for ID values that exceed 2^53 - 1, which is the max value for which JS maintains precision.

`--physical-params`
:   Specifies parameters to register the physical server. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params=@path/to/file.json`.

`--physical-params-endpoint` (string)
:   Specifies the endpoint IP address, URL, or hostname of the physical host. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

`--physical-params-force-register` (bool)
:   The agent running on a physical host fails the registration if it is already registered as part of another cluster. By setting this option to true, the agent can be forced to register with the current cluster. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

`--physical-params-host-type` (string)
:   Specifies the type of host. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Allowable values are: `kLinux`, `kWindows`.

`--physical-params-physical-type` (string)
:   Specifies the type of physical server. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Allowable values are: `kGroup`, `kHost`, `kWindowsCluster`, `kOracleRACCluster`, `kOracleAPCluster`, `kUnixCluster`.

`--physical-params-applications` ([]string)
:   Specifies the list of applications to be registered with Physical Source. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Allowable list items are: `kSQL`, `kOracle`.

#### Examples
{: #backup-recovery-protection-source-register-examples}

```sh
ibmcloud backup-recovery protection-source register \
    --xibm-tenant-id tenantId \
    --environment kPhysical \
    --name register-protection-source \
    --is-internal-encrypted=true \
    --encryption-key encryptionKey \
    --connection-id 26 \
    --connections '[{"connectionId": 26, "entityId": 26, "connectorGroupId": 26, "dataSourceConnectionId": "DatasourceConnectionId"}]' \
    --connector-group-id 26 \
    --advanced-configs '[{"key": "configKey", "value": "configValue"}]' \
    --data-source-connection-id DatasourceConnectionId \
    --kubernetes-params '{"autoProtectConfig": {"errorMessage": "exampleString", "isDefaultAutoProtected": true, "policyId": "exampleString", "protectionGroupId": "exampleString", "storageDomainId": 26}, "clientPrivateKey": "exampleString", "dataMoverImageLocation": "exampleString", "datamoverServiceType": "kNodePort", "defaultVlanParams": {"disableVlan": true, "interfaceName": "exampleString", "vlanId": 38}, "endpoint": "exampleString", "initContainerImageLocation": "exampleString", "kubernetesDistribution": "kOpenshift", "kubernetesType": "kCluster", "priorityClassName": "exampleString", "resourceAnnotations": [{"key": "exampleString", "value": "exampleString"}], "resourceLabels": [{"key": "exampleString", "value": "exampleString"}], "sanFields": ["exampleString","anotherTestString"], "serviceAnnotations": [{"key": "exampleString", "value": "exampleString"}], "veleroAwsPluginImageLocation": "exampleString", "veleroImageLocation": "exampleString", "veleroOpenshiftPluginImageLocation": "exampleString", "vlanInfoVec": [{"serviceAnnotations": [{"key": "exampleString", "value": "exampleString"}], "vlanParams": {"disableVlan": true, "interfaceName": "exampleString", "vlanId": 38}}]}' \
    --physical-params '{"endpoint": "xxx.xx.xx.xx", "forceRegister": true, "hostType": "kLinux", "physicalType": "kGroup", "applications": ["kSQL","kOracle"]}'
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery protection-source register \
    --xibm-tenant-id tenantId \
    --environment kPhysical \
    --name register-protection-source \
    --is-internal-encrypted=true \
    --encryption-key encryptionKey \
    --connection-id 26 \
    --connections '[connectionConfig]' \
    --connector-group-id 26 \
    --advanced-configs '[keyValuePair]' \
    --data-source-connection-id DatasourceConnectionId \
    --physical-params-endpoint exampleString \
    --physical-params-force-register=true \
    --physical-params-host-type kLinux \
    --physical-params-physical-type kGroup \
    --physical-params-applications kSQL,kOracle
```
{: pre}

### `ibmcloud backup-recovery protection-source registration-get`
{: #backup-recovery-cli-protection-source-registration-get-command}

Get a Protection Source registration.

```sh
ibmcloud backup-recovery protection-source registration-get --id ID --xibm-tenant-id XIBM-TENANT-ID [--request-initiator-type REQUEST-INITIATOR-TYPE]
```


#### Command options
{: #backup-recovery-protection-source-registration-get-cli-options}

`--id` (int64)
:   Specifies the id of the Protection Source registration. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--request-initiator-type` (string)
:   Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests.

    Allowable values are: `UIUser`, `UIAuto`, `Helios`.

#### Example
{: #backup-recovery-protection-source-registration-get-examples}

```sh
ibmcloud backup-recovery protection-source registration-get \
    --id 26 \
    --xibm-tenant-id tenantId \
    --request-initiator-type UIUser
```
{: pre}

### `ibmcloud backup-recovery protection-source registration-update`
{: #backup-recovery-cli-protection-source-registration-update-command}

Update Protection Source registration.

```sh
ibmcloud backup-recovery protection-source registration-update --id ID --xibm-tenant-id XIBM-TENANT-ID --environment ENVIRONMENT [--name NAME] [--is-internal-encrypted=IS-INTERNAL-ENCRYPTED] [--encryption-key ENCRYPTION-KEY] [--connection-id CONNECTION-ID] [--connections CONNECTIONS] [--connector-group-id CONNECTOR-GROUP-ID] [--advanced-configs ADVANCED-CONFIGS] [--data-source-connection-id DATA-SOURCE-CONNECTION-ID] [--last-modified-timestamp-usecs LAST-MODIFIED-TIMESTAMP-USECS] [--physical-params PHYSICAL-PARAMS | --physical-params-endpoint PHYSICAL-PARAMS-ENDPOINT --physical-params-force-register=PHYSICAL-PARAMS-FORCE-REGISTER --physical-params-host-type PHYSICAL-PARAMS-HOST-TYPE --physical-params-physical-type PHYSICAL-PARAMS-PHYSICAL-TYPE --physical-params-applications PHYSICAL-PARAMS-APPLICATIONS] [--kubernetes-params KUBERNETES-PARAMS | --kubernetes-params-auto-protect-config KUBERNETES-PARAMS-AUTO-PROTECT-CONFIG --kubernetes-params-client-private-key KUBERNETES-PARAMS-CLIENT-PRIVATE-KEY --kubernetes-params-cohesity-dataprotect-plugin-image-location KUBERNETES-PARAMS-COHESITY-DATAPROTECT-PLUGIN-IMAGE-LOCATION --kubernetes-params-data-mover-image-location KUBERNETES-PARAMS-DATA-MOVER-IMAGE-LOCATION --kubernetes-params-datamover-service-type KUBERNETES-PARAMS-DATAMOVER-SERVICE-TYPE --kubernetes-params-default-vlan-params KUBERNETES-PARAMS-DEFAULT-VLAN-PARAMS --kubernetes-params-endpoint KUBERNETES-PARAMS-ENDPOINT --kubernetes-params-init-container-image-location KUBERNETES-PARAMS-INIT-CONTAINER-IMAGE-LOCATION --kubernetes-params-kubernetes-distribution KUBERNETES-PARAMS-KUBERNETES-DISTRIBUTION --kubernetes-params-kubernetes-type KUBERNETES-PARAMS-KUBERNETES-TYPE --kubernetes-params-priority-class-name KUBERNETES-PARAMS-PRIORITY-CLASS-NAME --kubernetes-params-resource-annotations KUBERNETES-PARAMS-RESOURCE-ANNOTATIONS --kubernetes-params-resource-labels KUBERNETES-PARAMS-RESOURCE-LABELS --kubernetes-params-san-fields KUBERNETES-PARAMS-SAN-FIELDS --kubernetes-params-service-annotations KUBERNETES-PARAMS-SERVICE-ANNOTATIONS --kubernetes-params-velero-aws-plugin-image-location KUBERNETES-PARAMS-VELERO-AWS-PLUGIN-IMAGE-LOCATION --kubernetes-params-velero-image-location KUBERNETES-PARAMS-VELERO-IMAGE-LOCATION --kubernetes-params-velero-openshift-plugin-image-location KUBERNETES-PARAMS-VELERO-OPENSHIFT-PLUGIN-IMAGE-LOCATION --kubernetes-params-vlan-info-vec KUBERNETES-PARAMS-VLAN-INFO-VEC]
```


#### Command options
{: #backup-recovery-protection-source-registration-update-cli-options}

`--id` (int64)
:   Specifies the id of the Protection Source registration. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--environment` (string)
:   Specifies the environment type of the Protection Source. Required.

    Allowable values are: `kPhysical`, `kSQL`.

`--name` (string)
:   A user specified name for this source.

`--is-internal-encrypted` (bool)
:   Specifies whether credentials are encrypted by an internal key.

`--encryption-key` (string)
:   Specifies the key that the user has encrypted the credential with.

`--connection-id` (int64)
:   Specifies the id of the connection from where this source is reachable. This should only be set for a source being registered by a tenant user.

`--connections`
:   Specifies the list of connections for the source.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--connections=@path/to/file.json`.

`--connector-group-id` (int64)
:   Specifies the connector group id of connector groups.

`--kubernetes-params` (string)
:   Specifies the parameters to register a Kubernetes source. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--kubernetes-params=@path/to/file.json`.

`--kubernetes-params-auto-protect-config` (string)
:   Specifies the parameters to auto protect the source after registration. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-client-private-key` (string)
:   Specifies the bearer token or private key of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-cohesity-dataprotect-plugin-image-location` (string)
:   Specifies the custom {{site.data.keyword.baas_full}} Dataprotect plug-in image location of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-data-mover-image-location` (string)
:   Specifies the datamover image location of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-datamover-service-type` (string)
:   Specifies the data mover service type of Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

    Allowable values are: `kNodePort`, `kLoadBalancer`, `kClusterIp`.

`--kubernetes-params-default-vlan-params` (string)
:   Specifies VLAN params that is associated with the backup/restore operation. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-endpoint` (string)
:   Specifies the endpoint of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-init-container-image-location` (string)
:   Specifies the initial container image location of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-kubernetes-distribution` (string)
:   Specifies the distribution type of Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

    Allowable values are: `kOpenshift`, `kMainline`, `kVMwareTanzu`, `kRancher`, `kEKS`, `kGKE`, `kAKS`, `kIKS`, `kROKS`.

`--kubernetes-params-kubernetes-type` (string)
:   Specifies the type of kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

    Allowable values are: `kCluster`, `kNamespace`, `kService`, `kPVC`, `kPersistentVolumeClaim`, `kPersistentVolume`, `kLabel`.

`--kubernetes-params-priority-class-name` (string)
:   Specifies the priority class name for {{site.data.keyword.baas_full}} resources. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-resource-annotations` (string)
:   Specifies resource annotations to be applied on {{site.data.keyword.baas_full}} resources. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-resource-labels` (string)
:   Specifies a resource label to be applied on {{site.data.keyword.baas_full}} resources. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-san-fields` (string)
:   Specifies the SAN field for agent certificate. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-service-annotations` (string)
:   Specifies the service annotation object of the Kubernetes source. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-velero-aws-plugin-image-location` (string)
:   Specifies the velero AWS plug-in image location of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-velero-image-location` (string)
:   Specifies the velero image location of the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-velero-openshift-plugin-image-location` (string)
:   Specifies the velero open shift plug-in image for the Kubernetes source. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--kubernetes-params-vlan-info-vec` (string)
:   Specifies VLAN information that is provided during registration. It should be a JSON string or a path to a JSON file. This option provides a value for a subfield of the JSON option 'kubernetes-params'. It is mutually exclusive with that option.

`--advanced-configs`
:   Specifies the advanced configuration for a protection source.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--advanced-configs=@path/to/file.json`.

`--data-source-connection-id` (string)
:   Specifies the id of the connection from where this source is reachable. This should only be set for a source being registered by a tenant user. Also, this is the 'string' of connectionId. This property was added to accommodate for ID values that exceed 2^53 - 1, which is the max value for which JS maintains precision.

`--last-modified-timestamp-usecs` (int64)
:   Specifies the end time of attempt in Unix epoch Timestamp(in microseconds) for an object.

`--physical-params`
:   Specifies parameters to register the physical server. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params=@path/to/file.json`.

`--physical-params-endpoint` (string)
:   Specifies the endpoint IP address, URL, or hostname of the physical host. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

`--physical-params-force-register` (bool)
:   The agent running on a physical host fails the registration if it is already registered as part of another cluster. By setting this option to true, agent can be forced to register with the current cluster. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

`--physical-params-host-type` (string)
:   Specifies the type of host. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Allowable values are: `kLinux`, `kWindows`.

`--physical-params-physical-type` (string)
:   Specifies the type of physical server. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Allowable values are: `kGroup`, `kHost`, `kWindowsCluster`, `kOracleRACCluster`, `kOracleAPCluster`, `kUnixCluster`.

`--physical-params-applications` ([]string)
:   Specifies the list of applications to be registered with Physical Source. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Allowable list items are: `kSQL`, `kOracle`.

#### Examples
{: #backup-recovery-protection-source-registration-update-examples}

```sh
ibmcloud backup-recovery protection-source registration-update \
    --id 26 \
    --xibm-tenant-id tenantId \
    --environment kPhysical \
    --name update-protection-source \
    --is-internal-encrypted=true \
    --encryption-key encryptionKey \
    --connection-id 26 \
    --connections '[{"connectionId": 26, "entityId": 26, "connectorGroupId": 26, "dataSourceConnectionId": "DatasourceConnectionId"}]' \
    --connector-group-id 26 \
    --advanced-configs '[{"key": "configKey", "value": "configValue"}]' \
    --data-source-connection-id DatasourceConnectionId \
    --last-modified-timestamp-usecs 26 \
    --physical-params '{"endpoint": "xxx.xx.xx.xx", "forceRegister": true, "hostType": "kLinux", "physicalType": "kGroup", "applications": ["kSQL","kOracle"]}' \
    --kubernetes-params '{"autoProtectConfig": {"errorMessage": "exampleString", "isDefaultAutoProtected": true, "policyId": "exampleString", "protectionGroupId": "exampleString", "storageDomainId": 26}, "clientPrivateKey": "exampleString", "dataMoverImageLocation": "exampleString", "datamoverServiceType": "kNodePort", "defaultVlanParams": {"disableVlan": true, "interfaceName": "exampleString", "vlanId": 38}, "endpoint": "exampleString", "initContainerImageLocation": "exampleString", "kubernetesDistribution": "kOpenshift", "kubernetesType": "kCluster", "priorityClassName": "exampleString", "resourceAnnotations": [{"key": "exampleString", "value": "exampleString"}], "resourceLabels": [{"key": "exampleString", "value": "exampleString"}], "sanFields": ["exampleString","anotherTestString"], "serviceAnnotations": [{"key": "exampleString", "value": "exampleString"}], "veleroAwsPluginImageLocation": "exampleString", "veleroImageLocation": "exampleString", "veleroOpenshiftPluginImageLocation": "exampleString", "vlanInfoVec": [{"serviceAnnotations": [{"key": "exampleString", "value": "exampleString"}], "vlanParams": {"disableVlan": true, "interfaceName": "exampleString", "vlanId": 38}}]}'
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery protection-source registration-update \
    --id 26 \
    --xibm-tenant-id tenantId \
    --environment kPhysical \
    --name update-protection-source \
    --is-internal-encrypted=true \
    --encryption-key encryptionKey \
    --connection-id 26 \
    --connections '[connectionConfig]' \
    --connector-group-id 26 \
    --advanced-configs '[keyValuePair]' \
    --data-source-connection-id DatasourceConnectionId \
    --last-modified-timestamp-usecs 26 \
    --physical-params-endpoint exampleString \
    --physical-params-force-register=true \
    --physical-params-host-type kLinux \
    --physical-params-physical-type kGroup \
    --physical-params-applications kSQL,kOracle
```
{: pre}

### `ibmcloud backup-recovery protection-source registration-patch`
{: #backup-recovery-cli-protection-source-registration-patch-command}

Patches a Protection Source.

```sh
ibmcloud backup-recovery protection-source registration-patch --id ID --xibm-tenant-id XIBM-TENANT-ID --environment ENVIRONMENT
```


#### Command options
{: #backup-recovery-protection-source-registration-patch-cli-options}

`--id` (int64)
:   Specifies the id of the Protection Source registration. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--environment` (string)
:   Specifies the environment type of the Protection Source to be patched. Currently, the only environment that is supported is kCassandra. Required.

    Allowable values are: `kPhysical`, `kSQL`.

#### Example
{: #backup-recovery-protection-source-registration-patch-examples}

```sh
ibmcloud backup-recovery protection-source registration-patch \
    --id 26 \
    --xibm-tenant-id tenantId \
    --environment kPhysical
```
{: pre}

### `ibmcloud backup-recovery protection-source registration-delete`
{: #backup-recovery-cli-protection-source-registration-delete-command}

Delete Protection Source Registration.

```sh
ibmcloud backup-recovery protection-source registration-delete --id ID --xibm-tenant-id XIBM-TENANT-ID [--force]
```


#### Command options
{: #backup-recovery-protection-source-registration-delete-cli-options}

`--id` (int64)
:   Specifies the ID of the Protection Source Registration. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`-f`, `--force` (bool)
:   Force the command to execute without confirmation.

#### Example
{: #backup-recovery-protection-source-registration-delete-examples}

```sh
ibmcloud backup-recovery protection-source registration-delete \
    --id 26 \
    --xibm-tenant-id tenantId
```
{: pre}

### `ibmcloud backup-recovery protection-source refresh`
{: #backup-recovery-cli-protection-source-refresh-command}

Refresh a Protection Source.

```sh
ibmcloud backup-recovery protection-source refresh --id ID --xibm-tenant-id XIBM-TENANT-ID
```


#### Command options
{: #backup-recovery-protection-source-refresh-cli-options}

`--id` (int64)
:   Specifies the id of the Protection Source. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

#### Example
{: #backup-recovery-protection-source-refresh-examples}

```sh
ibmcloud backup-recovery protection-source refresh \
    --id 26 \
    --xibm-tenant-id tenantId
```
{: pre}

## agent-upgrade-task
{: #backup-recovery-agent-upgrade-task-cli}

Commands for AgentUpgradeTask resource.

```sh
ibmcloud backup-recovery agent-upgrade-task --help
```


### `ibmcloud backup-recovery agent-upgrade-task list`
{: #backup-recovery-cli-agent-upgrade-task-list-command}

Get the list of agent upgrade tasks.

```sh
ibmcloud backup-recovery agent-upgrade-task list --xibm-tenant-id XIBM-TENANT-ID [--ids IDS]
```


#### Command options
{: #backup-recovery-agent-upgrade-task-list-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--ids` ([]int64)
:   Specifies IDs of tasks to be fetched.

#### Example
{: #backup-recovery-agent-upgrade-task-list-examples}

```sh
ibmcloud backup-recovery agent-upgrade-task list \
    --xibm-tenant-id tenantId \
    --ids 26,27
```
{: pre}

### `ibmcloud backup-recovery agent-upgrade-task create`
{: #backup-recovery-cli-agent-upgrade-task-create-command}

Create a schedule-based agent upgrade task.

```sh
ibmcloud backup-recovery agent-upgrade-task create --xibm-tenant-id XIBM-TENANT-ID [--agent-ids AGENT-IDS] [--description DESCRIPTION] [--name NAME] [--retry-task-id RETRY-TASK-ID] [--schedule-end-time-usecs SCHEDULE-END-TIME-USECS] [--schedule-time-usecs SCHEDULE-TIME-USECS]
```


#### Command options
{: #backup-recovery-agent-upgrade-task-create-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--agent-ids` ([]int64)
:   Specifies agent IDs to be upgraded in the task.

`--description` (string)
:   Specifies the description of the task.

`--name` (string)
:   Specifies the name of the task.

`--retry-task-id` (int64)
:   Specifies the task that needs to be retried.

`--schedule-end-time-usecs` (int64)
:   Specifies the time before which the upgrade task should start execution as a Unix epoch Timestamp (in microseconds). If this is not specified the task will start anytime after scheduleTimeUsecs.

`--schedule-time-usecs` (int64)
:   Specifies the start time of the task specified by the user as a Unix epoch Timestamp (in microseconds). If no schedule is specified, the agents are upgraded immediately.

#### Example
{: #backup-recovery-agent-upgrade-task-create-examples}

```sh
ibmcloud backup-recovery agent-upgrade-task create \
    --xibm-tenant-id tenantId \
    --agent-ids 26,27 \
    --description 'Upgrade task' \
    --name create-upgrade-task \
    --retry-task-id 26 \
    --schedule-end-time-usecs 26 \
    --schedule-time-usecs 26
```
{: pre}

## protection-policy
{: #backup-recovery-protection-policy-cli}

Commands for ProtectionPolicy resource.

```sh
ibmcloud backup-recovery protection-policy --help
```


### `ibmcloud backup-recovery protection-policy list`
{: #backup-recovery-cli-protection-policy-list-command}

Lists protection policies based on filtering query parameters.

```sh
ibmcloud backup-recovery protection-policy list --xibm-tenant-id XIBM-TENANT-ID [--request-initiator-type REQUEST-INITIATOR-TYPE] [--ids IDS] [--policy-names POLICY-NAMES] [--types TYPES] [--exclude-linked-policies=EXCLUDE-LINKED-POLICIES] [--include-replicated-policies=INCLUDE-REPLICATED-POLICIES] [--include-stats=INCLUDE-STATS]
```


#### Command options
{: #backup-recovery-protection-policy-list-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--request-initiator-type` (string)
:   Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests.

    Allowable values are: `UIUser`, `UIAuto`, `Helios`.

`--ids` ([]string)
:   Filter policies by a list of policy ids.

`--policy-names` ([]string)
:   Filter policies by a list of policy names.

`--types` ([]string)
:   Types specifies the policy type of policies to be returned.

    Allowable list items are: `Regular`, `Internal`.

`--exclude-linked-policies` (bool)
:   If excludeLinkedPolicies is set to true, then only local policies that are created on the cluster will be returned. The result excludes all linked policies that are created from policy templates.

`--include-replicated-policies` (bool)
:   If includeReplicatedPolicies is set to true, then response also contains replicated policies. By default, replication policies are not included in the response.

`--include-stats` (bool)
:   If includeStats is set to true, then response returns a number of protection groups and objects. By default, the protection stats are not included in the response.

#### Example
{: #backup-recovery-protection-policy-list-examples}

```sh
ibmcloud backup-recovery protection-policy list \
    --xibm-tenant-id tenantId \
    --request-initiator-type UIUser \
    --ids policyId1 \
    --policy-names policyName1 \
    --types Regular,Internal \
    --exclude-linked-policies=true \
    --include-replicated-policies=true \
    --include-stats=true
```
{: pre}

### `ibmcloud backup-recovery protection-policy create`
{: #backup-recovery-cli-protection-policy-create-command}

Create the Protection Policy and returns the newly created policy object.

```sh
ibmcloud backup-recovery protection-policy create --xibm-tenant-id XIBM-TENANT-ID --name NAME [--backup-policy BACKUP-POLICY | --backup-policy-regular BACKUP-POLICY-REGULAR --backup-policy-log BACKUP-POLICY-LOG --backup-policy-bmr BACKUP-POLICY-BMR --backup-policy-cdp BACKUP-POLICY-CDP --backup-policy-storage-array-snapshot BACKUP-POLICY-STORAGE-ARRAY-SNAPSHOT --backup-policy-run-timeouts BACKUP-POLICY-RUN-TIMEOUTS] [--description DESCRIPTION] [--blackout-window BLACKOUT-WINDOW] [--extended-retention EXTENDED-RETENTION] [--remote-target-policy REMOTE-TARGET-POLICY | --remote-target-policy-replication-targets REMOTE-TARGET-POLICY-REPLICATION-TARGETS --remote-target-policy-archival-targets REMOTE-TARGET-POLICY-ARCHIVAL-TARGETS --remote-target-policy-cloud-spin-targets REMOTE-TARGET-POLICY-CLOUD-SPIN-TARGETS --remote-target-policy-onprem-deploy-targets REMOTE-TARGET-POLICY-ONPREM-DEPLOY-TARGETS --remote-target-policy-rpaas-targets REMOTE-TARGET-POLICY-RPAAS-TARGETS] [--cascaded-targets-config CASCADED-TARGETS-CONFIG] [--retry-options RETRY-OPTIONS | --retry-options-retries RETRY-OPTIONS-RETRIES --retry-options-retry-interval-mins RETRY-OPTIONS-RETRY-INTERVAL-MINS] [--data-lock DATA-LOCK] [--version VERSION] [--is-cbs-enabled=IS-CBS-ENABLED] [--last-modification-time-usecs LAST-MODIFICATION-TIME-USECS] [--template-id TEMPLATE-ID]
```


#### Command options
{: #backup-recovery-protection-policy-create-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--name` (string)
:   Specifies the name of the Protection Policy. Required.

`--backup-policy`
:   Specifies the backup schedule and retentions of a Protection Policy. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy=@path/to/file.json`.

`--description` (string)
:   Specifies the description of the Protection Policy.

`--blackout-window`
:   List of Blackout Windows. If specified, this field defines blackout periods when new Group Runs are not started. If a Group Run has been scheduled but not yet executed and the blackout period starts, the behavior depends on the policy field AbortInBlackoutPeriod.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--blackout-window=@path/to/file.json`.

`--extended-retention`
:   Specifies more retention policies that should be applied to the backup snapshots. A backup snapshot is retained up to a time that is the maximum of all retention policies that are applicable to it.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--extended-retention=@path/to/file.json`.

`--remote-target-policy`
:   Specifies the replication, archival, and cloud spin targets of Protection Policy. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy=@path/to/file.json`.

`--cascaded-targets-config`
:   Specifies the configuration for cascaded replications. Using cascaded replication, replication cluster(Rx) can further replicate and archive the snapshot copies to further targets. It's recommended creating cascaded configuration where protection group is created.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--cascaded-targets-config=@path/to/file.json`.

`--retry-options`
:   Retry Options of a Protection Policy when a Protection Group run fails. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--retry-options=@path/to/file.json`.

`--data-lock` (string)
:   This field is now deprecated. Use the DataLockConfig in the backup retention.

    Allowable values are: `Compliance`, `Administrative`.

`--version` (int64)
:   Specifies the current policy version. Policy version is incremented for optionally supporting new features and differentiating across releases.

`--is-cbs-enabled` (bool)
:   Specifies true if Calender Based Schedule is supported by the client. The default value is assumed as false for this feature.

`--last-modification-time-usecs` (int64)
:   Specifies the last time this Policy was updated. If this is passed into a PUT request, then the backend validates that the timestamp passed in matches the time that the policy was actually last modified. If the two timestamps do not match, then the request is rejected with a stale error.

`--template-id` (string)
:   Specifies the parent policy template id to which the policy is linked to.

`--backup-policy-regular`
:   Specifies the Incremental and Full policy settings and also the common Retention policy settings.". This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-regular=@path/to/file.json`.

`--backup-policy-log`
:   Specifies log backup settings for a Protection Group. This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-log=@path/to/file.json`.

`--backup-policy-bmr`
:   Specifies the BMR schedule in case of physical source protection. This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-bmr=@path/to/file.json`.

`--backup-policy-cdp`
:   Specifies CDP (Continuous Data Protection) backup settings for a Protection Group. This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-cdp=@path/to/file.json`.

`--backup-policy-storage-array-snapshot`
:   Specifies storage snapshot management backup settings for a Protection Group. This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-storage-array-snapshot=@path/to/file.json`.

`--backup-policy-run-timeouts`
:   Specifies the backup timeouts for different type of runs(kFull, kRegular, and so on). This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-run-timeouts=@path/to/file.json`.

`--remote-target-policy-replication-targets`
:   This option provides a value for a subfield of the JSON option 'remote-target-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy-replication-targets=@path/to/file.json`.

`--remote-target-policy-archival-targets`
:   This option provides a value for a subfield of the JSON option 'remote-target-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy-archival-targets=@path/to/file.json`.

`--remote-target-policy-cloud-spin-targets`
:   This option provides a value for a subfield of the JSON option 'remote-target-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy-cloud-spin-targets=@path/to/file.json`.

`--remote-target-policy-onprem-deploy-targets`
:   This option provides a value for a subfield of the JSON option 'remote-target-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy-onprem-deploy-targets=@path/to/file.json`.

`--remote-target-policy-rpaas-targets`
:   This option provides a value for a subfield of the JSON option 'remote-target-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy-rpaas-targets=@path/to/file.json`.

`--retry-options-retries` (int64)
:   Specifies the number of times to retry capturing Snapshots before the Protection Group Run fails. This option provides a value for a subfield of the JSON option 'retry-options'. It is mutually exclusive with that option.

    The minimum value is `0`.

`--retry-options-retry-interval-mins` (int64)
:   Specifies the number of minutes before retrying a failed Protection Group. This option provides a value for a subfield of the JSON option 'retry-options'. It is mutually exclusive with that option.

    The minimum value is `1`.

#### Examples
{: #backup-recovery-protection-policy-create-examples}

```sh
ibmcloud backup-recovery protection-policy create \
    --xibm-tenant-id tenantId \
    --name create-protection-policy \
    --backup-policy '{"regular": {"incremental": {"schedule": {"unit": "Minutes", "minuteSchedule": {"frequency": 1}, "hourSchedule": {"frequency": 1}, "daySchedule": {"frequency": 1}, "weekSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}, "monthSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"], "weekOfMonth": "First", "dayOfMonth": 10}, "yearSchedule": {"dayOfYear": "First"}}}, "full": {"schedule": {"unit": "Days", "daySchedule": {"frequency": 1}, "weekSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}, "monthSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"], "weekOfMonth": "First", "dayOfMonth": 10}, "yearSchedule": {"dayOfYear": "First"}}}, "fullBackups": [{"schedule": {"unit": "Days", "daySchedule": {"frequency": 1}, "weekSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}, "monthSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"], "weekOfMonth": "First", "dayOfMonth": 10}, "yearSchedule": {"dayOfYear": "First"}}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}], "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "primaryBackupTarget": {"targetType": "Local", "archivalTargetSettings": {"targetId": 26, "tierSettings": {"awsTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAmazonS3Standard"}]}, "azureTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAzureTierHot"}]}, "cloudPlatform": "AWS", "googleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kGoogleStandard"}]}, "oracleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kOracleTierStandard"}]}}}, "useDefaultBackupTarget": true}}, "log": {"schedule": {"unit": "Minutes", "minuteSchedule": {"frequency": 1}, "hourSchedule": {"frequency": 1}}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}, "bmr": {"schedule": {"unit": "Days", "daySchedule": {"frequency": 1}, "weekSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}, "monthSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"], "weekOfMonth": "First", "dayOfMonth": 10}, "yearSchedule": {"dayOfYear": "First"}}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}, "cdp": {"retention": {"unit": "Minutes", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}, "storageArraySnapshot": {"schedule": {"unit": "Minutes", "minuteSchedule": {"frequency": 1}, "hourSchedule": {"frequency": 1}, "daySchedule": {"frequency": 1}, "weekSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}, "monthSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"], "weekOfMonth": "First", "dayOfMonth": 10}, "yearSchedule": {"dayOfYear": "First"}}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}, "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}]}' \
    --description 'Protection Policy' \
    --blackout-window '[{"day": "Sunday", "startTime": {"hour": 1, "minute": 15, "timeZone": "America/Los_Angeles"}, "endTime": {"hour": 1, "minute": 15, "timeZone": "America/Los_Angeles"}, "configId": "Config-Id"}]' \
    --extended-retention '[{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "runType": "Regular", "configId": "Config-Id"}]' \
    --remote-target-policy '{"replicationTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "awsTargetConfig": {"region": 26, "sourceId": 26}, "azureTargetConfig": {"resourceGroup": 26, "sourceId": 26}, "targetType": "RemoteCluster", "remoteTargetConfig": {"clusterId": 26}}], "archivalTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "targetId": 5, "tierSettings": {"awsTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAmazonS3Standard"}]}, "azureTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAzureTierHot"}]}, "cloudPlatform": "AWS", "googleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kGoogleStandard"}]}, "oracleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kOracleTierStandard"}]}}, "extendedRetention": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "runType": "Regular", "configId": "Config-Id"}]}], "cloudSpinTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "target": {"awsParams": {"customTagList": [{"key": "custom-tag-key", "value": "custom-tag-value"}], "region": 3, "subnetId": 26, "vpcId": 26}, "azureParams": {"availabilitySetId": 26, "networkResourceGroupId": 26, "resourceGroupId": 26, "storageAccountId": 26, "storageContainerId": 26, "storageResourceGroupId": 26, "tempVmResourceGroupId": 26, "tempVmStorageAccountId": 26, "tempVmStorageContainerId": 26, "tempVmSubnetId": 26, "tempVmVirtualNetworkId": 26}, "id": 2}}], "onpremDeployTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "params": {"id": 4}}], "rpaasTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "targetId": 5, "targetType": "Tape"}]}' \
    --cascaded-targets-config '[{"sourceClusterId": 26, "remoteTargets": {"replicationTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "awsTargetConfig": {"region": 26, "sourceId": 26}, "azureTargetConfig": {"resourceGroup": 26, "sourceId": 26}, "targetType": "RemoteCluster", "remoteTargetConfig": {"clusterId": 26}}], "archivalTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "targetId": 5, "tierSettings": {"awsTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAmazonS3Standard"}]}, "azureTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAzureTierHot"}]}, "cloudPlatform": "AWS", "googleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kGoogleStandard"}]}, "oracleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kOracleTierStandard"}]}}, "extendedRetention": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "runType": "Regular", "configId": "Config-Id"}]}], "cloudSpinTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "target": {"awsParams": {"customTagList": [{"key": "custom-tag-key", "value": "custom-tag-value"}], "region": 3, "subnetId": 26, "vpcId": 26}, "azureParams": {"availabilitySetId": 26, "networkResourceGroupId": 26, "resourceGroupId": 26, "storageAccountId": 26, "storageContainerId": 26, "storageResourceGroupId": 26, "tempVmResourceGroupId": 26, "tempVmStorageAccountId": 26, "tempVmStorageContainerId": 26, "tempVmSubnetId": 26, "tempVmVirtualNetworkId": 26}, "id": 2}}], "onpremDeployTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "params": {"id": 4}}], "rpaasTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "targetId": 5, "targetType": "Tape"}]}}]' \
    --retry-options '{"retries": 0, "retryIntervalMins": 1}' \
    --data-lock Compliance \
    --version 38 \
    --is-cbs-enabled=true \
    --last-modification-time-usecs 26 \
    --template-id protection-policy-template
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery protection-policy create \
    --xibm-tenant-id tenantId \
    --name create-protection-policy \
    --description 'Protection Policy' \
    --blackout-window '[blackoutWindow]' \
    --extended-retention '[extendedRetentionPolicy]' \
    --cascaded-targets-config '[cascadedTargetConfiguration]' \
    --data-lock Compliance \
    --version 38 \
    --is-cbs-enabled=true \
    --last-modification-time-usecs 26 \
    --template-id protection-policy-template \
    --backup-policy-regular regularBackupPolicy \
    --backup-policy-log logBackupPolicy \
    --backup-policy-bmr bmrBackupPolicy \
    --backup-policy-cdp cdpBackupPolicy \
    --backup-policy-storage-array-snapshot storageArraySnapshotBackupPolicy \
    --backup-policy-run-timeouts '[cancellationTimeoutParams]' \
    --remote-target-policy-replication-targets '[replicationTargetConfiguration]' \
    --remote-target-policy-archival-targets '[archivalTargetConfiguration]' \
    --remote-target-policy-cloud-spin-targets '[cloudSpinTargetConfiguration]' \
    --remote-target-policy-onprem-deploy-targets '[onpremDeployTargetConfiguration]' \
    --remote-target-policy-rpaas-targets '[rpaasTargetConfiguration]' \
    --retry-options-retries 0 \
    --retry-options-retry-interval-mins 1
```
{: pre}

### `ibmcloud backup-recovery protection-policy get`
{: #backup-recovery-cli-protection-policy-get-command}

Returns the Protection Policy details based on provided Policy Id.

```sh
ibmcloud backup-recovery protection-policy get --id ID --xibm-tenant-id XIBM-TENANT-ID [--request-initiator-type REQUEST-INITIATOR-TYPE]
```


#### Command options
{: #backup-recovery-protection-policy-get-cli-options}

`--id` (string)
:   Specifies a unique id of the Protection Policy to return. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--request-initiator-type` (string)
:   Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests.

    Allowable values are: `UIUser`, `UIAuto`, `Helios`.

#### Example
{: #backup-recovery-protection-policy-get-examples}

```sh
ibmcloud backup-recovery protection-policy get \
    --id exampleString \
    --xibm-tenant-id tenantId \
    --request-initiator-type UIUser
```
{: pre}

### `ibmcloud backup-recovery protection-policy update`
{: #backup-recovery-cli-protection-policy-update-command}

Specifies the request to update the existing Protection Policy. On successful update, returns the updated policy object.

```sh
ibmcloud backup-recovery protection-policy update --id ID --xibm-tenant-id XIBM-TENANT-ID --name NAME [--backup-policy BACKUP-POLICY | --backup-policy-regular BACKUP-POLICY-REGULAR --backup-policy-log BACKUP-POLICY-LOG --backup-policy-bmr BACKUP-POLICY-BMR --backup-policy-cdp BACKUP-POLICY-CDP --backup-policy-storage-array-snapshot BACKUP-POLICY-STORAGE-ARRAY-SNAPSHOT --backup-policy-run-timeouts BACKUP-POLICY-RUN-TIMEOUTS] [--description DESCRIPTION] [--blackout-window BLACKOUT-WINDOW] [--extended-retention EXTENDED-RETENTION] [--remote-target-policy REMOTE-TARGET-POLICY | --remote-target-policy-replication-targets REMOTE-TARGET-POLICY-REPLICATION-TARGETS --remote-target-policy-archival-targets REMOTE-TARGET-POLICY-ARCHIVAL-TARGETS --remote-target-policy-cloud-spin-targets REMOTE-TARGET-POLICY-CLOUD-SPIN-TARGETS --remote-target-policy-onprem-deploy-targets REMOTE-TARGET-POLICY-ONPREM-DEPLOY-TARGETS --remote-target-policy-rpaas-targets REMOTE-TARGET-POLICY-RPAAS-TARGETS] [--cascaded-targets-config CASCADED-TARGETS-CONFIG] [--retry-options RETRY-OPTIONS | --retry-options-retries RETRY-OPTIONS-RETRIES --retry-options-retry-interval-mins RETRY-OPTIONS-RETRY-INTERVAL-MINS] [--data-lock DATA-LOCK] [--version VERSION] [--is-cbs-enabled=IS-CBS-ENABLED] [--last-modification-time-usecs LAST-MODIFICATION-TIME-USECS] [--template-id TEMPLATE-ID]
```


#### Command options
{: #backup-recovery-protection-policy-update-cli-options}

`--id` (string)
:   Specifies a unique id of the Protection Policy to update. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--name` (string)
:   Specifies the name of the Protection Policy. Required.

`--backup-policy`
:   Specifies the backup schedule and retentions of a Protection Policy. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy=@path/to/file.json`.

`--description` (string)
:   Specifies the description of the Protection Policy.

`--blackout-window`
:   List of Blackout Windows. If specified, this field defines blackout periods when new Group Runs are not started. If a Group Run has been scheduled but not yet executed and the blackout period starts, the behavior depends on the policy field AbortInBlackoutPeriod.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--blackout-window=@path/to/file.json`.

`--extended-retention`
:   Specifies more retention policies that should be applied to the backup snapshots. A backup snapshot is retained up to a time that is the maximum of all retention policies that are applicable to it.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--extended-retention=@path/to/file.json`.

`--remote-target-policy`
:   Specifies the replication, archival, and cloud spin targets of Protection Policy. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy=@path/to/file.json`.

`--cascaded-targets-config`
:   Specifies the configuration for cascaded replications. Using cascaded replication, replication cluster(Rx) can further replicate and archive the snapshot copies to further targets. It's recommended creating cascaded configuration where protection group is created.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--cascaded-targets-config=@path/to/file.json`.

`--retry-options`
:   Retry Options of a Protection Policy when a Protection Group run fails. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--retry-options=@path/to/file.json`.

`--data-lock` (string)
:   This field is now deprecated. Use the DataLockConfig in the backup retention.

    Allowable values are: `Compliance`, `Administrative`.

`--version` (int64)
:   Specifies the current policy version. Policy version is incremented for optionally supporting new features and differentiating across releases.

`--is-cbs-enabled` (bool)
:   Specifies true if Calender Based Schedule is supported by the client. The default value is assumed as false for this feature.

`--last-modification-time-usecs` (int64)
:   Specifies the last time this Policy was updated. If this is passed into a PUT request, then the backend validates that the timestamp passed in matches the time that the policy was actually last modified. If the two timestamps do not match, then the request is rejected with a stale error.

`--template-id` (string)
:   Specifies the parent policy template id to which the policy is linked to.

`--backup-policy-regular`
:   Specifies the Incremental and Full policy settings and also the common Retention policy settings.". This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-regular=@path/to/file.json`.

`--backup-policy-log`
:   Specifies log backup settings for a Protection Group. This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-log=@path/to/file.json`.

`--backup-policy-bmr`
:   Specifies the BMR schedule in case of physical source protection. This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-bmr=@path/to/file.json`.

`--backup-policy-cdp`
:   Specifies CDP (Continuous Data Protection) backup settings for a Protection Group. This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-cdp=@path/to/file.json`.

`--backup-policy-storage-array-snapshot`
:   Specifies storage snapshot management backup settings for a Protection Group. This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-storage-array-snapshot=@path/to/file.json`.

`--backup-policy-run-timeouts`
:   Specifies the backup timeouts for different type of runs(kFull, kRegular, etc.). This option provides a value for a subfield of the JSON option 'backup-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--backup-policy-run-timeouts=@path/to/file.json`.

`--remote-target-policy-replication-targets`
:   This option provides a value for a subfield of the JSON option 'remote-target-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy-replication-targets=@path/to/file.json`.

`--remote-target-policy-archival-targets`
:   This option provides a value for a subfield of the JSON option 'remote-target-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy-archival-targets=@path/to/file.json`.

`--remote-target-policy-cloud-spin-targets`
:   This option provides a value for a subfield of the JSON option 'remote-target-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy-cloud-spin-targets=@path/to/file.json`.

`--remote-target-policy-onprem-deploy-targets`
:   This option provides a value for a subfield of the JSON option 'remote-target-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy-onprem-deploy-targets=@path/to/file.json`.

`--remote-target-policy-rpaas-targets`
:   This option provides a value for a subfield of the JSON option 'remote-target-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--remote-target-policy-rpaas-targets=@path/to/file.json`.

`--retry-options-retries` (int64)
:   Specifies the number of times to retry capturing Snapshots before the Protection Group Run fails. This option provides a value for a subfield of the JSON option 'retry-options'. It is mutually exclusive with that option.

    The minimum value is `0`.

`--retry-options-retry-interval-mins` (int64)
:   Specifies the number of minutes before retrying a failed Protection Group. This option provides a value for a subfield of the JSON option 'retry-options'. It is mutually exclusive with that option.

    The minimum value is `1`.

#### Examples
{: #backup-recovery-protection-policy-update-examples}

```sh
ibmcloud backup-recovery protection-policy update \
    --id exampleString \
    --xibm-tenant-id tenantId \
    --name update-protection-policy \
    --backup-policy '{"regular": {"incremental": {"schedule": {"unit": "Minutes", "minuteSchedule": {"frequency": 1}, "hourSchedule": {"frequency": 1}, "daySchedule": {"frequency": 1}, "weekSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}, "monthSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"], "weekOfMonth": "First", "dayOfMonth": 10}, "yearSchedule": {"dayOfYear": "First"}}}, "full": {"schedule": {"unit": "Days", "daySchedule": {"frequency": 1}, "weekSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}, "monthSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"], "weekOfMonth": "First", "dayOfMonth": 10}, "yearSchedule": {"dayOfYear": "First"}}}, "fullBackups": [{"schedule": {"unit": "Days", "daySchedule": {"frequency": 1}, "weekSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}, "monthSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"], "weekOfMonth": "First", "dayOfMonth": 10}, "yearSchedule": {"dayOfYear": "First"}}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}], "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "primaryBackupTarget": {"targetType": "Local", "archivalTargetSettings": {"targetId": 26, "tierSettings": {"awsTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAmazonS3Standard"}]}, "azureTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAzureTierHot"}]}, "cloudPlatform": "AWS", "googleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kGoogleStandard"}]}, "oracleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kOracleTierStandard"}]}}}, "useDefaultBackupTarget": true}}, "log": {"schedule": {"unit": "Minutes", "minuteSchedule": {"frequency": 1}, "hourSchedule": {"frequency": 1}}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}, "bmr": {"schedule": {"unit": "Days", "daySchedule": {"frequency": 1}, "weekSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}, "monthSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"], "weekOfMonth": "First", "dayOfMonth": 10}, "yearSchedule": {"dayOfYear": "First"}}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}, "cdp": {"retention": {"unit": "Minutes", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}, "storageArraySnapshot": {"schedule": {"unit": "Minutes", "minuteSchedule": {"frequency": 1}, "hourSchedule": {"frequency": 1}, "daySchedule": {"frequency": 1}, "weekSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}, "monthSchedule": {"dayOfWeek": ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"], "weekOfMonth": "First", "dayOfMonth": 10}, "yearSchedule": {"dayOfYear": "First"}}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}, "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}]}' \
    --description 'Protection Policy' \
    --blackout-window '[{"day": "Sunday", "startTime": {"hour": 1, "minute": 15, "timeZone": "America/Los_Angeles"}, "endTime": {"hour": 1, "minute": 15, "timeZone": "America/Los_Angeles"}, "configId": "Config-Id"}]' \
    --extended-retention '[{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "runType": "Regular", "configId": "Config-Id"}]' \
    --remote-target-policy '{"replicationTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "awsTargetConfig": {"region": 26, "sourceId": 26}, "azureTargetConfig": {"resourceGroup": 26, "sourceId": 26}, "targetType": "RemoteCluster", "remoteTargetConfig": {"clusterId": 26}}], "archivalTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "targetId": 5, "tierSettings": {"awsTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAmazonS3Standard"}]}, "azureTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAzureTierHot"}]}, "cloudPlatform": "AWS", "googleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kGoogleStandard"}]}, "oracleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kOracleTierStandard"}]}}, "extendedRetention": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "runType": "Regular", "configId": "Config-Id"}]}], "cloudSpinTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "target": {"awsParams": {"customTagList": [{"key": "custom-tag-key", "value": "custom-tag-value"}], "region": 3, "subnetId": 26, "vpcId": 26}, "azureParams": {"availabilitySetId": 26, "networkResourceGroupId": 26, "resourceGroupId": 26, "storageAccountId": 26, "storageContainerId": 26, "storageResourceGroupId": 26, "tempVmResourceGroupId": 26, "tempVmStorageAccountId": 26, "tempVmStorageContainerId": 26, "tempVmSubnetId": 26, "tempVmVirtualNetworkId": 26}, "id": 2}}], "onpremDeployTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "params": {"id": 4}}], "rpaasTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "targetId": 5, "targetType": "Tape"}]}' \
    --cascaded-targets-config '[{"sourceClusterId": 26, "remoteTargets": {"replicationTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "awsTargetConfig": {"region": 26, "sourceId": 26}, "azureTargetConfig": {"resourceGroup": 26, "sourceId": 26}, "targetType": "RemoteCluster", "remoteTargetConfig": {"clusterId": 26}}], "archivalTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "targetId": 5, "tierSettings": {"awsTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAmazonS3Standard"}]}, "azureTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kAzureTierHot"}]}, "cloudPlatform": "AWS", "googleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kGoogleStandard"}]}, "oracleTiering": {"tiers": [{"moveAfterUnit": "Days", "moveAfter": 26, "tierType": "kOracleTierStandard"}]}}, "extendedRetention": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "runType": "Regular", "configId": "Config-Id"}]}], "cloudSpinTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "target": {"awsParams": {"customTagList": [{"key": "custom-tag-key", "value": "custom-tag-value"}], "region": 3, "subnetId": 26, "vpcId": 26}, "azureParams": {"availabilitySetId": 26, "networkResourceGroupId": 26, "resourceGroupId": 26, "storageAccountId": 26, "storageContainerId": 26, "storageResourceGroupId": 26, "tempVmResourceGroupId": 26, "tempVmStorageAccountId": 26, "tempVmStorageContainerId": 26, "tempVmSubnetId": 26, "tempVmVirtualNetworkId": 26}, "id": 2}}], "onpremDeployTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "params": {"id": 4}}], "rpaasTargets": [{"schedule": {"unit": "Runs", "frequency": 3}, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnRunSuccess": true, "configId": "Config-Id", "backupRunType": "Regular", "runTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "logRetention": {"unit": "Days", "duration": 0, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "targetId": 5, "targetType": "Tape"}]}}]' \
    --retry-options '{"retries": 0, "retryIntervalMins": 1}' \
    --data-lock Compliance \
    --version 38 \
    --is-cbs-enabled=true \
    --last-modification-time-usecs 26 \
    --template-id protection-policy-template
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery protection-policy update \
    --id exampleString \
    --xibm-tenant-id tenantId \
    --name update-protection-policy \
    --description 'Protection Policy' \
    --blackout-window '[blackoutWindow]' \
    --extended-retention '[extendedRetentionPolicy]' \
    --cascaded-targets-config '[cascadedTargetConfiguration]' \
    --data-lock Compliance \
    --version 38 \
    --is-cbs-enabled=true \
    --last-modification-time-usecs 26 \
    --template-id protection-policy-template \
    --backup-policy-regular regularBackupPolicy \
    --backup-policy-log logBackupPolicy \
    --backup-policy-bmr bmrBackupPolicy \
    --backup-policy-cdp cdpBackupPolicy \
    --backup-policy-storage-array-snapshot storageArraySnapshotBackupPolicy \
    --backup-policy-run-timeouts '[cancellationTimeoutParams]' \
    --remote-target-policy-replication-targets '[replicationTargetConfiguration]' \
    --remote-target-policy-archival-targets '[archivalTargetConfiguration]' \
    --remote-target-policy-cloud-spin-targets '[cloudSpinTargetConfiguration]' \
    --remote-target-policy-onprem-deploy-targets '[onpremDeployTargetConfiguration]' \
    --remote-target-policy-rpaas-targets '[rpaasTargetConfiguration]' \
    --retry-options-retries 0 \
    --retry-options-retry-interval-mins 1
```
{: pre}

### `ibmcloud backup-recovery protection-policy delete`
{: #backup-recovery-cli-protection-policy-delete-command}

Deletes a Protection Policy based on given policy id.

```sh
ibmcloud backup-recovery protection-policy delete --id ID --xibm-tenant-id XIBM-TENANT-ID [--force]
```


#### Command options
{: #backup-recovery-protection-policy-delete-cli-options}

`--id` (string)
:   Specifies a unique id of the Protection Policy to delete. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--force` (bool)
:   Force the command to execute.

#### Example
{: #backup-recovery-protection-policy-delete-examples}

```sh
ibmcloud backup-recovery protection-policy delete \
    --id exampleString \
    --xibm-tenant-id tenantId
```
{: pre}

## Protection-group
{: #backup-recovery-protection-group-cli}

Commands for ProtectionGroup resource.

```sh
ibmcloud backup-recovery protection-group --help
```


### `ibmcloud backup-recovery protection-group list`
{: #backup-recovery-cli-protection-group-list-command}

Get the list of Protection Groups.

```sh
ibmcloud backup-recovery protection-group list --xibm-tenant-id XIBM-TENANT-ID [--request-initiator-type REQUEST-INITIATOR-TYPE] [--ids IDS] [--names NAMES] [--policy-ids POLICY-IDS] [--include-groups-with-datalock-only=INCLUDE-GROUPS-WITH-DATALOCK-ONLY] [--environments ENVIRONMENTS] [--is-active=IS-ACTIVE] [--is-deleted=IS-DELETED] [--is-paused=IS-PAUSED] [--last-run-local-backup-status LAST-RUN-LOCAL-BACKUP-STATUS] [--last-run-replication-status LAST-RUN-REPLICATION-STATUS] [--last-run-archival-status LAST-RUN-ARCHIVAL-STATUS] [--last-run-cloud-spin-status LAST-RUN-CLOUD-SPIN-STATUS] [--last-run-any-status LAST-RUN-ANY-STATUS] [--is-last-run-sla-violated=IS-LAST-RUN-SLA-VIOLATED] [--include-last-run-info=INCLUDE-LAST-RUN-INFO] [--prune-excluded-source-ids=PRUNE-EXCLUDED-SOURCE-IDS] [--prune-source-ids=PRUNE-SOURCE-IDS] [--use-cached-data=USE-CACHED-DATA] [--source-ids SOURCE-IDS]
```


#### Command options
{: #backup-recovery-protection-group-list-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--request-initiator-type` (string)
:   Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests.

    Allowable values are: `UIUser`, `UIAuto`, `Helios`.

`--ids` ([]string)
:   Filter by a list of Protection Group ids.

`--names` ([]string)
:   Filter by a list of Protection Group names.

`--policy-ids` ([]string)
:   Filter by Policy ids that are associated with Protection Groups. Only Protection Groups that are associated with the specified Policy ids are returned.

`--include-groups-with-datalock-only` (bool)
:   Whether to only return Protection Groups with a datalock.

`--environments` ([]string)
:   Filter by environment types such as 'kVMware', 'kView'. Only Protection Groups protecting the specified environment types are returned.

    Allowable list items are: `kPhysical`, `kSQL`.

`--is-active` (bool)
:   Filter by Inactive or Active Protection Groups. If not set, all Inactive and Active Protection Groups are returned. If true, only Active Protection Groups are returned. If false, only Inactive Protection Groups are returned. When you create a Protection Group on a Primary Cluster with a replication schedule, the Cluster creates an Inactive copy of the Protection Group on the Remote Cluster. In addition, when an Active and running Protection Group is deactivated, the Protection Group becomes Inactive.

`--is-deleted` (bool)
:   If true, return only Protection Groups that have been deleted but still have Snapshots that are associated with them. If false, return all Protection Groups except those Protection Groups that have been deleted and still have Snapshots that are associated with them. A Protection Group that is deleted with all its Snapshots is not returned for either of these cases.

`--is-paused` (bool)
:   Filter by paused or non paused Protection Groups, If not set, all paused and non paused Protection Groups are returned. If true, only paused Protection Groups are returned. If false, only non paused Protection Groups are returned.

`--last-run-local-backup-status` ([]string)
:   Filter by last local backup run status.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `Paused`.

`--last-run-replication-status` ([]string)
:   Filter by last remote replication run status.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `Paused`.

`--last-run-archival-status` ([]string)
:   Filter by last cloud archival run status.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `Paused`.

`--last-run-cloud-spin-status` ([]string)
:   Filter by last cloud spin run status.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `Paused`.

`--last-run-any-status` ([]string)
:   Filter by last any run status.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `Paused`.

`--is-last-run-sla-violated` (bool)
:   If true, return Protection Groups for which last run SLA was violated.

`--include-last-run-info` (bool)
:   If true, the response includes last run information. If it is false or not specified, the last run information won't be returned.

`--prune-excluded-source-ids` (bool)
:   If true, the response will not include the list of excluded source IDs in groups that contain this field. This can be set to true in order to improve performance if excluded source IDs are not needed by the user.

`--prune-source-ids` (bool)
:   If true, the response excludes the list of source IDs within the group specified.

`--use-cached-data` (bool)
:   Specifies whether we can serve the GET request from the read replica cache. There is a lag of 15 seconds between the read replica and the primary data source.

`--source-ids` ([]int64)
:   Filter by Source ids that are associated with Protection Groups. Only Protection Groups associated with the specified Source ids, are returned.

#### Example
{: #backup-recovery-protection-group-list-examples}

```sh
ibmcloud backup-recovery protection-group list \
    --xibm-tenant-id tenantID \
    --request-initiator-type UIUser \
    --ids protectionGroupId1 \
    --names policyName1 \
    --policy-ids policyId1 \
    --include-groups-with-datalock-only=true \
    --environments kPhysical,kSQL \
    --is-active=true \
    --is-deleted=true \
    --is-paused=true \
    --last-run-local-backup-status Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,Paused \
    --last-run-replication-status Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,Paused \
    --last-run-archival-status Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,Paused \
    --last-run-cloud-spin-status Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,Paused \
    --last-run-any-status Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,Paused \
    --is-last-run-sla-violated=true \
    --include-last-run-info=true \
    --prune-excluded-source-ids=true \
    --prune-source-ids=true \
    --use-cached-data=true \
    --source-ids 26,27
```
{: pre}

### `ibmcloud backup-recovery protection-group create`
{: #backup-recovery-cli-protection-group-create-command}

Create a Protection Group.

```sh
ibmcloud backup-recovery protection-group create --xibm-tenant-id XIBM-TENANT-ID --name NAME --policy-id POLICY-ID --environment ENVIRONMENT [--priority PRIORITY] [--description DESCRIPTION] [--start-time START-TIME | --start-time-hour START-TIME-HOUR --start-time-minute START-TIME-MINUTE --start-time-time-zone START-TIME-TIME-ZONE] [--end-time-usecs END-TIME-USECS] [--last-modified-timestamp-usecs LAST-MODIFIED-TIMESTAMP-USECS] [--alert-policy ALERT-POLICY | --alert-policy-backup-run-status ALERT-POLICY-BACKUP-RUN-STATUS --alert-policy-alert-targets ALERT-POLICY-ALERT-TARGETS --alert-policy-raise-object-level-failure-alert=ALERT-POLICY-RAISE-OBJECT-LEVEL-FAILURE-ALERT --alert-policy-raise-object-level-failure-alert-after-last-attempt=ALERT-POLICY-RAISE-OBJECT-LEVEL-FAILURE-ALERT-AFTER-LAST-ATTEMPT --alert-policy-raise-object-level-failure-alert-after-each-attempt=ALERT-POLICY-RAISE-OBJECT-LEVEL-FAILURE-ALERT-AFTER-EACH-ATTEMPT] [--sla SLA] [--qos-policy QOS-POLICY] [--abort-in-blackouts=ABORT-IN-BLACKOUTS] [--pause-in-blackouts=PAUSE-IN-BLACKOUTS] [--is-paused=IS-PAUSED] [--advanced-configs ADVANCED-CONFIGS] [--physical-params PHYSICAL-PARAMS | --physical-params-protection-type PHYSICAL-PARAMS-PROTECTION-TYPE --physical-params-volume-protection-type-params PHYSICAL-PARAMS-VOLUME-PROTECTION-TYPE-PARAMS --physical-params-file-protection-type-params PHYSICAL-PARAMS-FILE-PROTECTION-TYPE-PARAMS] [--mssql-params MSSQL-PARAMS | --mssql-params-file-protection-type-params MSSQL-PARAMS-FILE-PROTECTION-TYPE-PARAMS --mssql-params-native-protection-type-params MSSQL-PARAMS-NATIVE-PROTECTION-TYPE-PARAMS --mssql-params-protection-type MSSQL-PARAMS-PROTECTION-TYPE --mssql-params-volume-protection-type-params MSSQL-PARAMS-VOLUME-PROTECTION-TYPE-PARAMS] [ --kubernetes-params KUBERNETES-PARAMS ]
```


#### Command options
{: #backup-recovery-protection-group-create-cli-options}

`--kubernetes-params` (string)
:   Specifies the parameters that are related to Kubernetes Protection Groups. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-enable-indexing`
:   Specifies whether indexing of files and folders is allowed or not while backing up namespace. If allowed files and folder can be recovered.

`--kubernetes-params-exclude-label-ids` (string)
:   Array of arrays of label IDs that specify labels to exclude. Optionally specify a list of labels to exclude from protecting by listing protection source ids of labels in this two-dimensional array. Using this two-dimensional array of label IDs, the Cluster generates a list of namespaces to exclude from protecting, which are derived from intersections of the inner arrays and union of the outer array. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-exclude-object-ids` (string)
:   Specifies the objects to be excluded in the Protection Group.

`--kubernetes-params-exclude-params` (string)
:   Specifies the parameters to in/exclude objects (for example, volumes). An object satisfying any of these criteria will be included by this filter. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-include-params` (string)
:   Specifies the parameters to in/exclude objects (for example, volumes). An object satisfying any of these criteria will be included by this filter. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-label-ids` (string)
:   Array of array of label IDs that specify labels to protect. Optionally specify a list of labels to protect by listing protection source ids of labels in this two-dimensional array. Using this two-dimensional array of label IDs, the cluster generates a list of namespaces to protect, which are derived from intersections of the inner arrays and union of the outer array. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-leverage-csi-snapshot`
:   Specifies whether CSI snapshots should be used for backup of namespaces.

`--kubernetes-params-non-snapshot-backup`
:   Specifies whether snapshot backup fails, nonsnapshot backup will be proceeded.

`--kubernetes-params-objects` (string)
:   Specifies the objects included in the Protection Group. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-vlan-params` (string)
:   Specifies VLAN params associated with the backup/restore operation. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-volume-backup-failure`
:   Specifies whether to process with backup if volumes back up fails.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--name` (string)
:   Specifies the name of the Protection Group. Required.

`--policy-id` (string)
:   Specifies the unique id of the Protection Policy that is associated with the Protection Group. The Policy provides retry settings Protection Schedules, Priority, SLA, and so on. Required.

`--environment` (string)
:   Specifies the environment type of the Protection Group. Required.

    Allowable values are: `kPhysical`, `kSQL`.

`--priority` (string)
:   Specifies the priority of the Protection Group.

    Allowable values are: `kLow`, `kMedium`, `kHigh`.

`--description` (string)
:   Specifies a description of the Protection Group.

`--start-time`
:   Specifies the time of day. Used for scheduling purposes. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--start-time=@path/to/file.json`.

`--end-time-usecs` (int64)
:   Specifies the end time in micro seconds for this Protection Group. If this is not specified, the Protection Group won't be ended.

`--last-modified-timestamp-usecs` (int64)
:   Specifies the last time that this protection group was updated. If this is passed into a PUT request, then the backend validates that the timestamp passed in matches the time that the protection group was actually last modified. If the two timestamps do not match, then the request is rejected with a stale error.

`--alert-policy`
:   Specifies a policy for alerting users of the status of a Protection Group. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--alert-policy=@path/to/file.json`.

`--sla`
:   Specifies the SLA parameters for this Protection Group.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--sla=@path/to/file.json`.

`--qos-policy` (string)
:   Specifies whether the Protection Group will be written to HDD or SSD.

    Allowable values are: `kBackupHDD`, `kBackupSSD`, `kTestAndDevHigh`, `kBackupAll`.

`--abort-in-blackouts` (bool)
:   Specifies whether currently executing jobs should abort if a blackout period specified by a policy starts. Available only if the selected policy has at least one blackout period. The default value is false. This field should not be set to true if 'pauseInBlackouts' is set to true.

`--pause-in-blackouts` (bool)
:   Specifies whether currently executing jobs should be paused if a blackout period specified by a policy starts. Available only if the selected policy has at least one blackout period. The default value is false. This field should not be set to true if 'abortInBlackouts' is sent as true.

`--is-paused` (bool)
:   Specifies whether the Protection Group is paused. New runs are not scheduled for the paused Protection Groups. Active run if any is not impacted.

`--advanced-configs`
:   Specifies the advanced configuration for a protection job.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--advanced-configs=@path/to/file.json`.

`--physical-params`
:   This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params=@path/to/file.json`.

`--mssql-params`
:   Specifies the parameters specific to the MSSQL Protection Group. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params=@path/to/file.json`.

`--start-time-hour` (int64)
:   Specifies the hour of the day (0-23). This option provides a value for a subfield of the JSON option 'start-time'. It is mutually exclusive with that option.

    The maximum value is `23`. The minimum value is `0`.

`--start-time-minute` (int64)
:   Specifies the minute of the hour (0-59). This option provides a value for a subfield of the JSON option 'start-time'. It is mutually exclusive with that option.

    The maximum value is `59`. The minimum value is `0`.

`--start-time-time-zone` (string)
:   Specifies the time zone of the user. If not specified, the default value is assumed as America/Los_Angeles. This option provides a value for a subfield of the JSON option 'start-time'. It is mutually exclusive with that option.

    The default value is `America/Los_Angeles`.

`--alert-policy-backup-run-status` ([]string)
:   Specifies the run status for which the user would like to receive alerts. This option provides a value for a subfield of the JSON option 'alert-policy'. It is mutually exclusive with that option.

    Allowable list items are: `kSuccess`, `kFailure`, `kSlaViolation`, `kWarning`. The minimum length is `1` item.

`--alert-policy-alert-targets`
:   Specifies a list of targets to receive the alerts. This option provides a value for a subfield of the JSON option 'alert-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--alert-policy-alert-targets=@path/to/file.json`.

`--alert-policy-raise-object-level-failure-alert` (bool)
:   Specifies whether object level alerts are raised for backup failures after the backup run. This option provides a value for a subfield of the JSON option 'alert-policy'. It is mutually exclusive with that option.

`--alert-policy-raise-object-level-failure-alert-after-last-attempt` (bool)
:   Specifies whether object level alerts are raised for backup failures after the last backup attempt. This option provides a value for a subfield of the JSON option 'alert-policy'. It is mutually exclusive with that option.

`--alert-policy-raise-object-level-failure-alert-after-each-attempt` (bool)
:   Specifies whether object level alerts are raised for backup failures after each backup attempt. This option provides a value for a subfield of the JSON option 'alert-policy'. It is mutually exclusive with that option.

`--physical-params-protection-type` (string)
:   Specifies the Physical Protection Group type. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Allowable values are: `kFile`, `kVolume`.

`--physical-params-volume-protection-type-params`
:   Specifies the parameters, which are specific to Volume based physical Protection Groups. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params-volume-protection-type-params=@path/to/file.json`.

`--physical-params-file-protection-type-params`
:   Specifies the parameters, which are specific to Physical related Protection Groups. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params-file-protection-type-params=@path/to/file.json`.

`--mssql-params-file-protection-type-params`
:   Specifies the params to create a File based MSSQL Protection Group. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params-file-protection-type-params=@path/to/file.json`.

`--mssql-params-native-protection-type-params`
:   Specifies the params to create a Native based MSSQL Protection Group. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params-native-protection-type-params=@path/to/file.json`.

`--mssql-params-protection-type` (string)
:   Specifies the MSSQL Protection Group type. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

    Allowable values are: `kFile`, `kVolume`, `kNative`.

`--mssql-params-volume-protection-type-params`
:   Specifies the params to create a Volume based MSSQL Protection Group. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params-volume-protection-type-params=@path/to/file.json`.

#### Examples
{: #backup-recovery-protection-group-create-examples}

```sh
ibmcloud backup-recovery protection-group create \
    --xibm-tenant-id tenantId \
    --name create-protection-group \
    --policy-id xxxxxxxxxxxxxxxx:xxxxxxxxxxxxx:xx \
    --environment kPhysical \
    --priority kLow \
    --description 'Protection Group' \
    --start-time '{"hour": 0, "minute": 0, "timeZone": "America/Los_Angeles"}' \
    --end-time-usecs 26 \
    --last-modified-timestamp-usecs 26 \
    --alert-policy '{"backupRunStatus": ["kSuccess","kFailure","kSlaViolation","kWarning"], "alertTargets": [{"emailAddress": "alert1@domain.com", "language": "en-us", "recipientType": "kTo"}], "raiseObjectLevelFailureAlert": true, "raiseObjectLevelFailureAlertAfterLastAttempt": true, "raiseObjectLevelFailureAlertAfterEachAttempt": true}' \
    --sla '[{"backupRunType": "kIncremental", "slaMinutes": 1}]' \
    --qos-policy kBackupHDD \
    --abort-in-blackouts=true \
    --pause-in-blackouts=true \
    --is-paused=true \
    --advanced-configs '[{"key": "configKey", "value": "configValue"}]' \
    --physical-params '{"protectionType": "kFile", "volumeProtectionTypeParams": {"objects": [{"id": 3, "volumeGuids": ["volumeGuid1"], "enableSystemBackup": true, "excludedVssWriters": ["writerName1","writerName2"]}], "indexingPolicy": {"enableIndexing": true, "includePaths": ["~/dir1"], "excludePaths": ["~/dir2"]}, "performSourceSideDeduplication": true, "quiesce": true, "continueOnQuiesceFailure": true, "incrementalBackupAfterRestart": true, "prePostScript": {"preScript": {"path": "~/script1", "params": "param1", "timeoutSecs": 1, "isActive": true, "continueOnError": true}, "postScript": {"path": "~/script2", "params": "param2", "timeoutSecs": 1, "isActive": true}}, "dedupExclusionSourceIds": [26,27], "excludedVssWriters": ["writerName1","writerName2"], "cobmrBackup": true}, "fileProtectionTypeParams": {"excludedVssWriters": ["writerName1","writerName2"], "objects": [{"excludedVssWriters": ["writerName1","writerName2"], "id": 2, "filePaths": [{"includedPath": "~/dir1/", "excludedPaths": ["~/dir2"], "skipNestedVolumes": true}], "usesPathLevelSkipNestedVolumeSetting": true, "nestedVolumeTypesToSkip": ["volume1"], "followNasSymlinkTarget": true, "metadataFilePath": "~/dir3"}], "indexingPolicy": {"enableIndexing": true, "includePaths": ["~/dir1"], "excludePaths": ["~/dir2"]}, "performSourceSideDeduplication": true, "performBrickBasedDeduplication": true, "taskTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "quiesce": true, "continueOnQuiesceFailure": true, "cobmrBackup": true, "prePostScript": {"preScript": {"path": "~/script1", "params": "param1", "timeoutSecs": 1, "isActive": true, "continueOnError": true}, "postScript": {"path": "~/script2", "params": "param2", "timeoutSecs": 1, "isActive": true}}, "dedupExclusionSourceIds": [26,27], "globalExcludePaths": ["~/dir1"], "globalExcludeFS": ["~/dir2"], "ignorableErrors": ["kEOF","kNonExistent"], "allowParallelRuns": true}}' \
    --mssql-params '{"fileProtectionTypeParams": {"aagBackupPreferenceType": "kPrimaryReplicaOnly", "advancedSettings": {"clonedDbBackupStatus": "kError", "dbBackupIfNotOnlineStatus": "kError", "missingDbBackupStatus": "kError", "offlineRestoringDbBackupStatus": "kError", "readOnlyDbBackupStatus": "kError", "reportAllNonAutoprotectDbErrors": "kError"}, "backupSystemDbs": true, "excludeFilters": [{"filterString": "filterString", "isRegularExpression": false}], "fullBackupsCopyOnly": true, "logBackupNumStreams": 38, "logBackupWithClause": "backupWithClause", "prePostScript": {"preScript": {"path": "~/script1", "params": "param1", "timeoutSecs": 1, "isActive": true, "continueOnError": true}, "postScript": {"path": "~/script2", "params": "param2", "timeoutSecs": 1, "isActive": true}}, "useAagPreferencesFromServer": true, "userDbBackupPreferenceType": "kBackupAllDatabases", "additionalHostParams": [{"disableSourceSideDeduplication": true, "hostId": 26}], "objects": [{"id": 6}], "performSourceSideDeduplication": true}, "nativeProtectionTypeParams": {"aagBackupPreferenceType": "kPrimaryReplicaOnly", "advancedSettings": {"clonedDbBackupStatus": "kError", "dbBackupIfNotOnlineStatus": "kError", "missingDbBackupStatus": "kError", "offlineRestoringDbBackupStatus": "kError", "readOnlyDbBackupStatus": "kError", "reportAllNonAutoprotectDbErrors": "kError"}, "backupSystemDbs": true, "excludeFilters": [{"filterString": "filterString", "isRegularExpression": false}], "fullBackupsCopyOnly": true, "logBackupNumStreams": 38, "logBackupWithClause": "backupWithClause", "prePostScript": {"preScript": {"path": "~/script1", "params": "param1", "timeoutSecs": 1, "isActive": true, "continueOnError": true}, "postScript": {"path": "~/script2", "params": "param2", "timeoutSecs": 1, "isActive": true}}, "useAagPreferencesFromServer": true, "userDbBackupPreferenceType": "kBackupAllDatabases", "numStreams": 38, "objects": [{"id": 6}], "withClause": "withClause"}, "protectionType": "kFile", "volumeProtectionTypeParams": {"aagBackupPreferenceType": "kPrimaryReplicaOnly", "advancedSettings": {"clonedDbBackupStatus": "kError", "dbBackupIfNotOnlineStatus": "kError", "missingDbBackupStatus": "kError", "offlineRestoringDbBackupStatus": "kError", "readOnlyDbBackupStatus": "kError", "reportAllNonAutoprotectDbErrors": "kError"}, "backupSystemDbs": true, "excludeFilters": [{"filterString": "filterString", "isRegularExpression": false}], "fullBackupsCopyOnly": true, "logBackupNumStreams": 38, "logBackupWithClause": "backupWithClause", "prePostScript": {"preScript": {"path": "~/script1", "params": "param1", "timeoutSecs": 1, "isActive": true, "continueOnError": true}, "postScript": {"path": "~/script2", "params": "param2", "timeoutSecs": 1, "isActive": true}}, "useAagPreferencesFromServer": true, "userDbBackupPreferenceType": "kBackupAllDatabases", "additionalHostParams": [{"enableSystemBackup": true, "hostId": 8, "volumeGuids": ["volumeGuid1"]}], "backupDbVolumesOnly": true, "incrementalBackupAfterRestart": true, "indexingPolicy": {"enableIndexing": true, "includePaths": ["~/dir1"], "excludePaths": ["~/dir2"]}, "objects": [{"id": 6}]}}' \
    --kubernetes-params '{"enableIndexing": true, "excludeLabelIds": [26,27,26,27],[26,27, 26,27], "excludeObjectIds": [26,27], "excludeParams": {"labelCombinationMethod": "AND", "labelVector": [{}], "objects": [26,27]}, "includeParams": {"labelCombinationMethod": "AND", "labelVector": [{}], "objects": [26,27]}, "labelIds": [26,27,26,27],[26,27, 26,27], "leverageCSISnapshot": true, "nonSnapshotBackup": true, "objects": [{"backupOnlyPvc": true, "excludePvcs": [{}], "excludedResources": ["exampleString","anotherTestString"], "id": 26, "includePvcs": [{}], "includedResources": ["exampleString","anotherTestString"], "quiesceGroups": [{"quiesceMode": "kQuiesceTogether", "quiesceRules": [{"podSelectorLabels": [{}], "postSnapshotHooks": [{"commands": ["exampleString","anotherTestString"], "container": "exampleString", "failOnError": true, "timeout": 26}], "preSnapshotHooks": [{"commands": ["exampleString","anotherTestString"], "container": "exampleString", "failOnError": true, "timeout": 26}]}]}]}], "vlanParams": {"disableVlan": true, "interfaceName": "exampleString", "vlanId": 38}, "volumeBackupFailure": true}'
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery protection-group create \
    --xibm-tenant-id tenantId \
    --name create-protection-group \
    --policy-id xxxxxxxxxxxxxxxx:xxxxxxxxxxxxx:xx \
    --environment kPhysical \
    --priority kLow \
    --description 'Protection Group' \
    --end-time-usecs 26 \
    --last-modified-timestamp-usecs 26 \
    --sla '[slaRule]' \
    --qos-policy kBackupHDD \
    --abort-in-blackouts=true \
    --pause-in-blackouts=true \
    --is-paused=true \
    --advanced-configs '[keyValuePair]' \
    --start-time-hour 0 \
    --start-time-minute 0 \
    --start-time-time-zone America/Los_Angeles \
    --alert-policy-backup-run-status kSuccess,kFailure,kSlaViolation,kWarning \
    --alert-policy-alert-targets '[alertTarget]' \
    --alert-policy-raise-object-level-failure-alert=true \
    --alert-policy-raise-object-level-failure-alert-after-last-attempt=true \
    --alert-policy-raise-object-level-failure-alert-after-each-attempt=true \
    --physical-params-protection-type kFile \
    --physical-params-volume-protection-type-params physicalVolumeProtectionGroupParams \
    --physical-params-file-protection-type-params physicalFileProtectionGroupParams \
    --mssql-params-file-protection-type-params mssqlFileProtectionGroupParams \
    --mssql-params-native-protection-type-params mssqlNativeProtectionGroupParams \
    --mssql-params-protection-type kFile \
    --mssql-params-volume-protection-type-params mssqlVolumeProtectionGroupParams
```
{: pre}

### `ibmcloud backup-recovery protection-group get`
{: #backup-recovery-cli-protection-group-get-command}

Returns the Protection Group corresponding to the specified Group id.

```sh
ibmcloud backup-recovery protection-group get --id ID --xibm-tenant-id XIBM-TENANT-ID [--request-initiator-type REQUEST-INITIATOR-TYPE] [--include-last-run-info=INCLUDE-LAST-RUN-INFO] [--prune-excluded-source-ids=PRUNE-EXCLUDED-SOURCE-IDS] [--prune-source-ids=PRUNE-SOURCE-IDS]
```


#### Command options
{: #backup-recovery-protection-group-get-cli-options}

`--id` (string)
:   Specifies a unique id of the Protection Group. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--request-initiator-type` (string)
:   Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests.

    Allowable values are: `UIUser`, `UIAuto`, `Helios`.

`--include-last-run-info` (bool)
:   If true, the response includes last run information. If it is false or not specified, the last run information won't be returned.

`--prune-excluded-source-ids` (bool)
:   If true, the response will not include the list of excluded source IDs in groups that contain this field. This can be set to true in order to improve performance if excluded source IDs are not needed by the user.

`--prune-source-ids` (bool)
:   If true, the response excludes the list of source IDs within the group specified.

#### Example
{: #backup-recovery-protection-group-get-examples}

```sh
ibmcloud backup-recovery protection-group get \
    --id exampleString \
    --xibm-tenant-id tenantID \
    --request-initiator-type UIUser \
    --include-last-run-info=true \
    --prune-excluded-source-ids=true \
    --prune-source-ids=true
```
{: pre}

### `ibmcloud backup-recovery protection-group update`
{: #backup-recovery-cli-protection-group-update-command}

Update the specified Protection Group.

```sh
ibmcloud backup-recovery protection-group update --id ID --xibm-tenant-id XIBM-TENANT-ID --name NAME --policy-id POLICY-ID --environment ENVIRONMENT [--priority PRIORITY] [--description DESCRIPTION] [--start-time START-TIME | --start-time-hour START-TIME-HOUR --start-time-minute START-TIME-MINUTE --start-time-time-zone START-TIME-TIME-ZONE] [--end-time-usecs END-TIME-USECS] [--last-modified-timestamp-usecs LAST-MODIFIED-TIMESTAMP-USECS] [--alert-policy ALERT-POLICY | --alert-policy-backup-run-status ALERT-POLICY-BACKUP-RUN-STATUS --alert-policy-alert-targets ALERT-POLICY-ALERT-TARGETS --alert-policy-raise-object-level-failure-alert=ALERT-POLICY-RAISE-OBJECT-LEVEL-FAILURE-ALERT --alert-policy-raise-object-level-failure-alert-after-last-attempt=ALERT-POLICY-RAISE-OBJECT-LEVEL-FAILURE-ALERT-AFTER-LAST-ATTEMPT --alert-policy-raise-object-level-failure-alert-after-each-attempt=ALERT-POLICY-RAISE-OBJECT-LEVEL-FAILURE-ALERT-AFTER-EACH-ATTEMPT] [--sla SLA] [--qos-policy QOS-POLICY] [--abort-in-blackouts=ABORT-IN-BLACKOUTS] [--pause-in-blackouts=PAUSE-IN-BLACKOUTS] [--is-paused=IS-PAUSED] [--advanced-configs ADVANCED-CONFIGS] [--physical-params PHYSICAL-PARAMS | --physical-params-protection-type PHYSICAL-PARAMS-PROTECTION-TYPE --physical-params-volume-protection-type-params PHYSICAL-PARAMS-VOLUME-PROTECTION-TYPE-PARAMS --physical-params-file-protection-type-params PHYSICAL-PARAMS-FILE-PROTECTION-TYPE-PARAMS] [--mssql-params MSSQL-PARAMS | --mssql-params-file-protection-type-params MSSQL-PARAMS-FILE-PROTECTION-TYPE-PARAMS --mssql-params-native-protection-type-params MSSQL-PARAMS-NATIVE-PROTECTION-TYPE-PARAMS --mssql-params-protection-type MSSQL-PARAMS-PROTECTION-TYPE --mssql-params-volume-protection-type-params MSSQL-PARAMS-VOLUME-PROTECTION-TYPE-PARAMS]
  [--kubernetes-params KUBERNETES-PARAMS | --enableIndexing PHYSICALINDEXING --exclude-object-ids EXCLUDE-OBJECT-IDS --include-object-ids INCLUDE-OBJECT-IDS]
```


#### Command options
{: #backup-recovery-protection-group-update-cli-options}

`--id` (string)
:   Specifies the id of the Protection Group. Required.

`--kubernetes-params` (string)
:   Specifies the parameters, which are related to Kubernetes Protection Groups. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-enable-indexing`
:   Specifies whether indexing of files and folders is allowed or not while backing up namespace. If allowed files and folder can be recovered.

`--kubernetes-params-exclude-label-ids` (string)
:   Array of arrays of label IDs that specify labels to exclude. Optionally specify a list of labels to exclude from protecting by listing protection source ids of labels in this two-dimensional array. Using this two-dimensional array of label IDs, the Cluster generates a list of namespaces to exclude from protecting, which are derived from intersections of the inner arrays and union of the outer array. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-exclude-object-ids` (string)
:   Specifies the objects to be excluded in the Protection Group.

`--kubernetes-params-exclude-params` (string)
:   Specifies the parameters to in/exclude objects (for example, volumes). An object satisfying any of these criteria will be included by this filter. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-include-params` (string)
:   Specifies the parameters to in/exclude objects (for example, volumes). An object satisfying any of these criteria will be included by this filter. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-label-ids` (string)
:   Array of array of label IDs that specify labels to protect. Optionally specify a list of labels to protect by listing protection source ids of labels in this two-dimensional array. Using this two-dimensional array of label IDs, the cluster generates a list of namespaces to protect, which are derived from intersections of the inner arrays and union of the outer array. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-leverage-csi-snapshot`
:   Specifies whether CSI snapshots should be used for backup of namespaces.

`--kubernetes-params-non-snapshot-backup`
:   Specifies whether snapshot backup fails, nonsnapshot backup is proceeded.

`--kubernetes-params-objects` (string)
:   Specifies the objects included in the Protection Group. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-vlan-params` (string)
:   Specifies VLAN params that is associated with the backup/restore operation. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-volume-backup-failure`
:   Specifies whether to process with backup if volumes back up fails.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--name` (string)
:   Specifies the name of the Protection Group. Required.

`--policy-id` (string)
:   Specifies the unique id of the Protection Policy that is associated with the Protection Group. The Policy provides retry settings for Protection Schedules, Priority, SLA, and so on. Required.

`--environment` (string)
:   Specifies the environment type of the Protection Group. Required.

    Allowable values are: `kPhysical`, `kSQL`.

`--priority` (string)
:   Specifies the priority of the Protection Group.

    Allowable values are: `kLow`, `kMedium`, `kHigh`.

`--description` (string)
:   Specifies a description of the Protection Group.

`--start-time`
:   Specifies the time of day. Used for scheduling purposes. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--start-time=@path/to/file.json`.

`--end-time-usecs` (int64)
:   Specifies the end time in micro seconds for this Protection Group. If this is not specified, the Protection Group won't be ended.

`--last-modified-timestamp-usecs` (int64)
:   Specifies the last time that this protection group was updated. If this is passed into a PUT request, then the backend validates that the timestamp passed in matches the time that the protection group was actually last modified. If the two timestamps do not match, then the request is rejected with a stale error.

`--alert-policy`
:   Specifies a policy for alerting users of the status of a Protection Group. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--alert-policy=@path/to/file.json`.

`--sla`
:   Specifies the SLA parameters for this Protection Group.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--sla=@path/to/file.json`.

`--qos-policy` (string)
:   Specifies whether the Protection Group will be written to HDD or SSD.

    Allowable values are: `kBackupHDD`, `kBackupSSD`, `kTestAndDevHigh`, `kBackupAll`.

`--abort-in-blackouts` (bool)
:   Specifies whether currently executing jobs should abort if a blackout period specified by a policy starts. Available only if the selected policy has at least one blackout period. The default value is false. This field should not be set to true if 'pauseInBlackouts' is set to true.

`--pause-in-blackouts` (bool)
:   Specifies whether currently executing jobs should be paused if a blackout period specified by a policy starts. Available only if the selected policy has at least one blackout period. The default value is false. This field should not be set to true if 'abortInBlackouts' is sent as true.

`--is-paused` (bool)
:   Specifies whether the Protection Group is paused. New runs are not scheduled for the paused Protection Groups. Active run if any is not impacted.

`--advanced-configs`
:   Specifies the advanced configuration for a protection job.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--advanced-configs=@path/to/file.json`.

`--physical-params`
:   This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params=@path/to/file.json`.

`--mssql-params`
:   Specifies the parameters specific to the MSSQL Protection Group. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params=@path/to/file.json`.

`--start-time-hour` (int64)
:   Specifies the hour of the day (0-23). This option provides a value for a subfield of the JSON option 'start-time'. It is mutually exclusive with that option.

    The maximum value is `23`. The minimum value is `0`.

`--start-time-minute` (int64)
:   Specifies the minute of the hour (0-59). This option provides a value for a subfield of the JSON option 'start-time'. It is mutually exclusive with that option.

    The maximum value is `59`. The minimum value is `0`.

`--start-time-time-zone` (string)
:   Specifies the time zone of the user. If not specified, the default value is assumed as America/Los_Angeles. This option provides a value for a subfield of the JSON option 'start-time'. It is mutually exclusive with that option.

    The default value is `America/Los_Angeles`.

`--alert-policy-backup-run-status` ([]string)
:   Specifies the run status for which the user would like to receive alerts. This option provides a value for a subfield of the JSON option 'alert-policy'. It is mutually exclusive with that option.

    Allowable list items are: `kSuccess`, `kFailure`, `kSlaViolation`, `kWarning`. The minimum length is `1` item.

`--alert-policy-alert-targets`
:   Specifies a list of targets to receive the alerts. This option provides a value for a subfield of the JSON option 'alert-policy'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--alert-policy-alert-targets=@path/to/file.json`.

`--alert-policy-raise-object-level-failure-alert` (bool)
:   Specifies whether object level alerts are raised for backup failures after the backup run. This option provides a value for a subfield of the JSON option 'alert-policy'. It is mutually exclusive with that option.

`--alert-policy-raise-object-level-failure-alert-after-last-attempt` (bool)
:   Specifies whether object level alerts are raised for backup failures after the last backup attempt. This option provides a value for a subfield of the JSON option 'alert-policy'. It is mutually exclusive with that option.

`--alert-policy-raise-object-level-failure-alert-after-each-attempt` (bool)
:   Specifies whether object level alerts are raised for backup failures after each backup attempt. This option provides a value for a subfield of the JSON option 'alert-policy'. It is mutually exclusive with that option.

`--physical-params-protection-type` (string)
:   Specifies the Physical Protection Group type. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Allowable values are: `kFile`, `kVolume`.

`--physical-params-volume-protection-type-params`
:   Specifies the parameters, which are specific to Volume based physical Protection Groups. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params-volume-protection-type-params=@path/to/file.json`.

`--physical-params-file-protection-type-params`
:   Specifies the parameters, which are specific to Physical related Protection Groups. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params-file-protection-type-params=@path/to/file.json`.

`--mssql-params-file-protection-type-params`
:   Specifies the params to create a File based MSSQL Protection Group. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params-file-protection-type-params=@path/to/file.json`.

`--mssql-params-native-protection-type-params`
:   Specifies the params to create a Native based MSSQL Protection Group. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params-native-protection-type-params=@path/to/file.json`.

`--mssql-params-protection-type` (string)
:   Specifies the MSSQL Protection Group type. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

    Allowable values are: `kFile`, `kVolume`, `kNative`.

`--mssql-params-volume-protection-type-params`
:   Specifies the params to create a Volume based MSSQL Protection Group. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params-volume-protection-type-params=@path/to/file.json`.

#### Examples
{: #backup-recovery-protection-group-update-examples}

```sh
ibmcloud backup-recovery protection-group update \
    --id exampleString \
    --xibm-tenant-id tenantId \
    --name update-protection-group \
    --policy-id xxxxxxxxxxxxxxxx:xxxxxxxxxxxxx:xx \
    --environment kPhysical \
    --priority kLow \
    --description 'Protection Group' \
    --start-time '{"hour": 0, "minute": 0, "timeZone": "America/Los_Angeles"}' \
    --end-time-usecs 26 \
    --last-modified-timestamp-usecs 26 \
    --alert-policy '{"backupRunStatus": ["kSuccess","kFailure","kSlaViolation","kWarning"], "alertTargets": [{"emailAddress": "alert1@domain.com", "language": "en-us", "recipientType": "kTo"}], "raiseObjectLevelFailureAlert": true, "raiseObjectLevelFailureAlertAfterLastAttempt": true, "raiseObjectLevelFailureAlertAfterEachAttempt": true}' \
    --sla '[{"backupRunType": "kIncremental", "slaMinutes": 1}]' \
    --qos-policy kBackupHDD \
    --abort-in-blackouts=true \
    --pause-in-blackouts=true \
    --is-paused=true \
    --advanced-configs '[{"key": "configKey", "value": "configValue"}]' \
    --physical-params '{"protectionType": "kFile", "volumeProtectionTypeParams": {"objects": [{"id": 3, "volumeGuids": ["volumeGuid1"], "enableSystemBackup": true, "excludedVssWriters": ["writerName1","writerName2"]}], "indexingPolicy": {"enableIndexing": true, "includePaths": ["~/dir1"], "excludePaths": ["~/dir2"]}, "performSourceSideDeduplication": true, "quiesce": true, "continueOnQuiesceFailure": true, "incrementalBackupAfterRestart": true, "prePostScript": {"preScript": {"path": "~/script1", "params": "param1", "timeoutSecs": 1, "isActive": true, "continueOnError": true}, "postScript": {"path": "~/script2", "params": "param2", "timeoutSecs": 1, "isActive": true}}, "dedupExclusionSourceIds": [26,27], "excludedVssWriters": ["writerName1","writerName2"], "cobmrBackup": true}, "fileProtectionTypeParams": {"excludedVssWriters": ["writerName1","writerName2"], "objects": [{"excludedVssWriters": ["writerName1","writerName2"], "id": 2, "filePaths": [{"includedPath": "~/dir1/", "excludedPaths": ["~/dir2"], "skipNestedVolumes": true}], "usesPathLevelSkipNestedVolumeSetting": true, "nestedVolumeTypesToSkip": ["volume1"], "followNasSymlinkTarget": true, "metadataFilePath": "~/dir3"}], "indexingPolicy": {"enableIndexing": true, "includePaths": ["~/dir1"], "excludePaths": ["~/dir2"]}, "performSourceSideDeduplication": true, "performBrickBasedDeduplication": true, "taskTimeouts": [{"timeoutMins": 26, "backupType": "kRegular"}], "quiesce": true, "continueOnQuiesceFailure": true, "cobmrBackup": true, "prePostScript": {"preScript": {"path": "~/script1", "params": "param1", "timeoutSecs": 1, "isActive": true, "continueOnError": true}, "postScript": {"path": "~/script2", "params": "param2", "timeoutSecs": 1, "isActive": true}}, "dedupExclusionSourceIds": [26,27], "globalExcludePaths": ["~/dir1"], "globalExcludeFS": ["~/dir2"], "ignorableErrors": ["kEOF","kNonExistent"], "allowParallelRuns": true}}' \
    --mssql-params '{"fileProtectionTypeParams": {"aagBackupPreferenceType": "kPrimaryReplicaOnly", "advancedSettings": {"clonedDbBackupStatus": "kError", "dbBackupIfNotOnlineStatus": "kError", "missingDbBackupStatus": "kError", "offlineRestoringDbBackupStatus": "kError", "readOnlyDbBackupStatus": "kError", "reportAllNonAutoprotectDbErrors": "kError"}, "backupSystemDbs": true, "excludeFilters": [{"filterString": "filterString", "isRegularExpression": false}], "fullBackupsCopyOnly": true, "logBackupNumStreams": 38, "logBackupWithClause": "backupWithClause", "prePostScript": {"preScript": {"path": "~/script1", "params": "param1", "timeoutSecs": 1, "isActive": true, "continueOnError": true}, "postScript": {"path": "~/script2", "params": "param2", "timeoutSecs": 1, "isActive": true}}, "useAagPreferencesFromServer": true, "userDbBackupPreferenceType": "kBackupAllDatabases", "additionalHostParams": [{"disableSourceSideDeduplication": true, "hostId": 26}], "objects": [{"id": 6}], "performSourceSideDeduplication": true}, "nativeProtectionTypeParams": {"aagBackupPreferenceType": "kPrimaryReplicaOnly", "advancedSettings": {"clonedDbBackupStatus": "kError", "dbBackupIfNotOnlineStatus": "kError", "missingDbBackupStatus": "kError", "offlineRestoringDbBackupStatus": "kError", "readOnlyDbBackupStatus": "kError", "reportAllNonAutoprotectDbErrors": "kError"}, "backupSystemDbs": true, "excludeFilters": [{"filterString": "filterString", "isRegularExpression": false}], "fullBackupsCopyOnly": true, "logBackupNumStreams": 38, "logBackupWithClause": "backupWithClause", "prePostScript": {"preScript": {"path": "~/script1", "params": "param1", "timeoutSecs": 1, "isActive": true, "continueOnError": true}, "postScript": {"path": "~/script2", "params": "param2", "timeoutSecs": 1, "isActive": true}}, "useAagPreferencesFromServer": true, "userDbBackupPreferenceType": "kBackupAllDatabases", "numStreams": 38, "objects": [{"id": 6}], "withClause": "withClause"}, "protectionType": "kFile", "volumeProtectionTypeParams": {"aagBackupPreferenceType": "kPrimaryReplicaOnly", "advancedSettings": {"clonedDbBackupStatus": "kError", "dbBackupIfNotOnlineStatus": "kError", "missingDbBackupStatus": "kError", "offlineRestoringDbBackupStatus": "kError", "readOnlyDbBackupStatus": "kError", "reportAllNonAutoprotectDbErrors": "kError"}, "backupSystemDbs": true, "excludeFilters": [{"filterString": "filterString", "isRegularExpression": false}], "fullBackupsCopyOnly": true, "logBackupNumStreams": 38, "logBackupWithClause": "backupWithClause", "prePostScript": {"preScript": {"path": "~/script1", "params": "param1", "timeoutSecs": 1, "isActive": true, "continueOnError": true}, "postScript": {"path": "~/script2", "params": "param2", "timeoutSecs": 1, "isActive": true}}, "useAagPreferencesFromServer": true, "userDbBackupPreferenceType": "kBackupAllDatabases", "additionalHostParams": [{"enableSystemBackup": true, "hostId": 8, "volumeGuids": ["volumeGuid1"]}], "backupDbVolumesOnly": true, "incrementalBackupAfterRestart": true, "indexingPolicy": {"enableIndexing": true, "includePaths": ["~/dir1"], "excludePaths": ["~/dir2"]}, "objects": [{"id": 6}]}}' \
    --kubernetes-params '{"enableIndexing": true, "excludeLabelIds": [26,27,26,27],[26,27, 26,27], "excludeObjectIds": [26,27], "excludeParams": {"labelCombinationMethod": "AND", "labelVector": [{}], "objects": [26,27]}, "includeParams": {"labelCombinationMethod": "AND", "labelVector": [{}], "objects": [26,27]}, "labelIds": [26,27,26,27],[26,27, 26,27], "leverageCSISnapshot": true, "nonSnapshotBackup": true, "objects": [{"backupOnlyPvc": true, "excludePvcs": [{}], "excludedResources": ["exampleString","anotherTestString"], "id": 26, "includePvcs": [{}], "includedResources": ["exampleString","anotherTestString"], "quiesceGroups": [{"quiesceMode": "kQuiesceTogether", "quiesceRules": [{"podSelectorLabels": [{}], "postSnapshotHooks": [{"commands": ["exampleString","anotherTestString"], "container": "exampleString", "failOnError": true, "timeout": 26}], "preSnapshotHooks": [{"commands": ["exampleString","anotherTestString"], "container": "exampleString", "failOnError": true, "timeout": 26}]}]}]}], "vlanParams": {"disableVlan": true, "interfaceName": "exampleString", "vlanId": 38}, "volumeBackupFailure": true}'
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery protection-group update \
    --id exampleString \
    --xibm-tenant-id tenantId \
    --name update-protection-group \
    --policy-id xxxxxxxxxxxxxxxx:xxxxxxxxxxxxx:xx \
    --environment kPhysical \
    --priority kLow \
    --description 'Protection Group' \
    --end-time-usecs 26 \
    --last-modified-timestamp-usecs 26 \
    --sla '[slaRule]' \
    --qos-policy kBackupHDD \
    --abort-in-blackouts=true \
    --pause-in-blackouts=true \
    --is-paused=true \
    --advanced-configs '[keyValuePair]' \
    --start-time-hour 0 \
    --start-time-minute 0 \
    --start-time-time-zone America/Los_Angeles \
    --alert-policy-backup-run-status kSuccess,kFailure,kSlaViolation,kWarning \
    --alert-policy-alert-targets '[alertTarget]' \
    --alert-policy-raise-object-level-failure-alert=true \
    --alert-policy-raise-object-level-failure-alert-after-last-attempt=true \
    --alert-policy-raise-object-level-failure-alert-after-each-attempt=true \
    --physical-params-protection-type kFile \
    --physical-params-volume-protection-type-params physicalVolumeProtectionGroupParams \
    --physical-params-file-protection-type-params physicalFileProtectionGroupParams \
    --mssql-params-file-protection-type-params mssqlFileProtectionGroupParams \
    --mssql-params-native-protection-type-params mssqlNativeProtectionGroupParams \
    --mssql-params-protection-type kFile \
    --mssql-params-volume-protection-type-params mssqlVolumeProtectionGroupParams
```
{: pre}

### `ibmcloud backup-recovery protection-group delete`
{: #backup-recovery-cli-protection-group-delete-command}

Returns Success if the Protection Group is deleted.

```sh
ibmcloud backup-recovery protection-group delete --id ID --xibm-tenant-id XIBM-TENANT-ID [--delete-snapshots=DELETE-SNAPSHOTS] [--force]
```


#### Command options
{: #backup-recovery-protection-group-delete-cli-options}

`--id` (string)
:   Specifies a unique id of the Protection Group. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--delete-snapshots` (bool)
:   Specifies whether Snapshots that are generated by the Protection Group should also be deleted when the Protection Group is deleted.

`-f`, `--force` (bool)
:   Force the command to execute without confirmation.

#### Example
{: #backup-recovery-protection-group-delete-examples}

```sh
ibmcloud backup-recovery protection-group delete \
    --id exampleString \
    --xibm-tenant-id tenantId \
    --delete-snapshots=true
```
{: pre}

## protection-group-run
{: #backup-recovery-protection-group-run-cli}

Commands for ProtectionGroupRun resource.

```sh
ibmcloud backup-recovery protection-group-run --help
```


### `ibmcloud backup-recovery protection-group-run list`
{: #backup-recovery-cli-protection-group-run-list-command}

Get the runs for a particular Protection Group.

```sh
ibmcloud backup-recovery protection-group-run list --id ID --xibm-tenant-id XIBM-TENANT-ID [--request-initiator-type REQUEST-INITIATOR-TYPE] [--run-id RUN-ID] [--start-time-usecs START-TIME-USECS] [--end-time-usecs END-TIME-USECS] [--run-types RUN-TYPES] [--include-object-details=INCLUDE-OBJECT-DETAILS] [--local-backup-run-status LOCAL-BACKUP-RUN-STATUS] [--replication-run-status REPLICATION-RUN-STATUS] [--archival-run-status ARCHIVAL-RUN-STATUS] [--cloud-spin-run-status CLOUD-SPIN-RUN-STATUS] [--num-runs NUM-RUNS] [--exclude-non-restorable-runs=EXCLUDE-NON-RESTORABLE-RUNS] [--run-tags RUN-TAGS] [--use-cached-data=USE-CACHED-DATA] [--filter-by-end-time=FILTER-BY-END-TIME] [--snapshot-target-types SNAPSHOT-TARGET-TYPES] [--only-return-successful-copy-run=ONLY-RETURN-SUCCESSFUL-COPY-RUN] [--filter-by-copy-task-end-time=FILTER-BY-COPY-TASK-END-TIME]
```


#### Command options
{: #backup-recovery-protection-group-run-list-cli-options}

`--id` (string)
:   Specifies a unique id of the Protection Group. Required.

    The value must match regular expression `/^\\d+:\\d+:\\d+$/`.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--request-initiator-type` (string)
:   Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests.

    Allowable values are: `UIUser`, `UIAuto`, `Helios`.

`--run-id` (string)
:   Specifies the protection run id.

    The value must match regular expression `/^\\d+:\\d+$/`.

`--start-time-usecs` (int64)
:   Start time for time range filter. Specify the start time as a Unix epoch Timestamp (in microseconds), only runs executing after this time will be returned. By default it is endTimeUsecs minus an hour.

`--end-time-usecs` (int64)
:   End time for time range filter. Specify the end time as a Unix epoch Timestamp (in microseconds), only runs executing before this time will be returned. By default it is current time.

`--run-types` ([]string)
:   Filter by run type. Only protection run matching the specified types will be returned.

    Allowable list items are: `kAll`, `kHydrateCDP`, `kSystem`, `kStorageArraySnapshot`, `kIncremental`, `kFull`, `kLog`.

`--include-object-details` (bool)
:   Specifies whether the result includes the object details for each protection run. If set to true, details of the protected object will be returned. If set to false or not specified, details will not be returned.

`--local-backup-run-status` ([]string)
:   Specifies a list of local backup status, runs matching the status is returned.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `Paused`.

`--replication-run-status` ([]string)
:   Specifies a list of replication status, runs matching the status is returned.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `Paused`.

`--archival-run-status` ([]string)
:   Specifies a list of archival status, runs matching the status is returned.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `Paused`.

`--cloud-spin-run-status` ([]string)
:   Specifies a list of cloud spin status, runs matching the status is returned.<br> 'Running' indicates that the run is still running.<br> 'Canceled' indicates that the run has been canceled.<br> 'Canceling' indicates that the run is in the process of being canceled.<br> 'Failed' indicates that the run has failed.<br> 'Missed' indicates that the run was unable to take place at the scheduled time because the previous run was still happening.<br> 'Succeeded' indicates that the run has finished successfully.<br> 'SucceededWithWarning' indicates that the run finished successfully, but there were some warning messages.<br> 'Paused' indicates that the ongoing run has been paused.<br> 'Skipped' indicates that the run was skipped.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `Paused`.

`--num-runs` (int64)
:   Specifies the max number of runs. If not specified, at most 100 runs will be returned.

`--exclude-non-restorable-runs` (bool)
:   Specifies whether to exclude non restorable runs. Run is treated restorable only if there is at least one object snapshot (which might be either a local or an archival snapshot) which is not deleted or expired. Default value is false.

    The default value is `false`.

`--run-tags` ([]string)
:   Specifies a list of tags for protection runs. If this is specified, only the runs that match these tags will be returned.

`--use-cached-data` (bool)
:   Specifies whether we can serve the GET request from the read replica cache. There is a lag of 15 seconds between the read replica and the primary data source.

`--filter-by-end-time` (bool)
:   If true, the runs with backup end time within the specified time range will be returned. Otherwise, the runs with start time in the time range are returned. Defaults to filtering by start time unless this flag is set. Only one of filterByEndTime and filterByCopyTaskEndTime can be set.

`--snapshot-target-types` ([]string)
:   Specifies the snapshot's target type that should be filtered.

    Allowable list items are: `Local`, `Archival`, `RpaasArchival`, `StorageArraySnapshot`, `Remote`.

`--only-return-successful-copy-run` (bool)
:   only successful copy runs are returned.

`--filter-by-copy-task-end-time` (bool)
:   If true, then the details of the runs for which any copyTask was completed in the given time range will be returned. Only one of filterByEndTime and filterByCopyTaskEndTime can be set.

#### Example
{: #backup-recovery-protection-group-run-list-examples}

```sh
ibmcloud backup-recovery protection-group-run list \
    --id exampleString \
    --xibm-tenant-id tenantId \
    --request-initiator-type UIUser \
    --run-id 11:111 \
    --start-time-usecs 26 \
    --end-time-usecs 26 \
    --run-types kAll,kHydrateCDP,kSystem,kStorageArraySnapshot,kIncremental,kFull,kLog \
    --include-object-details=true \
    --local-backup-run-status Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,Paused \
    --replication-run-status Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,Paused \
    --archival-run-status Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,Paused \
    --cloud-spin-run-status Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,Paused \
    --num-runs 26 \
    --exclude-non-restorable-runs=false \
    --run-tags tag1 \
    --use-cached-data=true \
    --filter-by-end-time=true \
    --snapshot-target-types Local,Archival,RpaasArchival,StorageArraySnapshot,Remote \
    --only-return-successful-copy-run=true \
    --filter-by-copy-task-end-time=true
```
{: pre}

### `ibmcloud backup-recovery protection-group-run update`
{: #backup-recovery-cli-protection-group-run-update-command}

Update runs for a particular Protection Group. A user can perform the following actions: 1. Extend or reduce retention of a local, replication, and archival snapshots. 2. Can perform resync operation on failed copy snapshots attempts in this Run. 3. Add new replication and archival snapshot targets to the Run. 4. Add or remove legal hold on the snapshots. Only a user with DSO role can perform this operation. 5. Delete the snapshots that were created as a part of this Run. 6. Apply datalock on existing snapshots where a user cannot manually delete snapshots before the expiry time.

```sh
ibmcloud backup-recovery protection-group-run update --id ID --xibm-tenant-id XIBM-TENANT-ID --update-protection-group-run-params UPDATE-PROTECTION-GROUP-RUN-PARAMS | @UPDATE-PROTECTION-GROUP-RUN-PARAMS-FILE
```


#### Command options
{: #backup-recovery-protection-group-run-update-cli-options}

`--id` (string)
:   Specifies a unique id of the Protection Group. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--update-protection-group-run-params` (string)
:   &nbsp; Required.

    It can also be a path to a JSON file. The minimum length is `1` item.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--update-protection-group-run-params=@path/to/file.json`.

#### Example
{: #backup-recovery-protection-group-run-update-examples}

```sh
ibmcloud backup-recovery protection-group-run update \
    --id exampleString \
    --xibm-tenant-id tenantId \
    --update-protection-group-run-params '[{"runId": "11:111", "localSnapshotConfig": {"enableLegalHold": true, "deleteSnapshot": true, "dataLock": "Compliance", "daysToKeep": 26}, "replicationSnapshotConfig": {"newSnapshotConfig": [{"id": 26, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}], "updateExistingSnapshotConfig": [{"id": 4, "name": "update-snapshot-config", "enableLegalHold": true, "deleteSnapshot": true, "resync": true, "dataLock": "Compliance", "daysToKeep": 26}]}, "archivalSnapshotConfig": {"newSnapshotConfig": [{"id": 2, "archivalTargetType": "Tape", "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnlyFullySuccessful": true}], "updateExistingSnapshotConfig": [{"id": 3, "name": "update-snapshot-config", "archivalTargetType": "Tape", "enableLegalHold": true, "deleteSnapshot": true, "resync": true, "dataLock": "Compliance", "daysToKeep": 26}]}}]'
```
{: pre}

### `ibmcloud backup-recovery protection-group-run create`
{: #backup-recovery-cli-protection-group-run-create-command}

Create a new protection run. This can be used to start a run for a Protection Group on demand, ignoring the schedule and retention specified in the protection policy.

```sh
ibmcloud backup-recovery protection-group-run create --id ID --xibm-tenant-id XIBM-TENANT-ID --run-type RUN-TYPE [--objects OBJECTS | @OBJECTS-FILE] [--targets-config (TARGETS-CONFIG | @TARGETS-CONFIG-FILE) | --targets-config-use-policy-defaults=TARGETS-CONFIG-USE-POLICY-DEFAULTS --targets-config-replications TARGETS-CONFIG-REPLICATIONS --targets-config-archivals TARGETS-CONFIG-ARCHIVALS --targets-config-cloud-replications TARGETS-CONFIG-CLOUD-REPLICATIONS]
```


#### Command options
{: #backup-recovery-protection-group-run-create-cli-options}

`--id` (string)
:   Specifies a unique id of the Protection Group. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--run-type` (string)
:   Type of protection run. 'kRegular' indicates an incremental (CBT) backup. Incremental backups utilizing CBT (if supported) are captured of the target protection objects. The first run of a kRegular schedule captures all the blocks. 'kFull' indicates a full (no CBT) backup. A complete backup (all blocks) of the target protection objects are always captured and Change Block Tracking (CBT) is not utilized. 'kLog' indicates a Database Log backup. Capture the database transaction logs to allow rolling back to a specific point in time. 'kSystem' indicates system volume backup. It produces an image for bare metal recovery. Required.

    Allowable values are: `kRegular`, `kFull`, `kLog`, `kSystem`, `kHydrateCDP`, `kStorageArraySnapshot`.

`--objects` (string)
:   Specifies the list of objects to be protected by this Protection Group run. These can be leaf objects or nonleaf objects in the protection hierarchy. This must be specified only if a subset of objects from the Protection Groups needs to be protected. It can also be a path to a JSON file.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--objects=@path/to/file.json`.

`--targets-config` (string)
:   Specifies the replication and archival targets. It can also be a path to a JSON file. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--targets-config=@path/to/file.json`.

`--targets-config-use-policy-defaults` (bool)
:   Specifies whether to use default policy settings or not. If specified as true then 'replications' and 'archivals' should not be specified. In case of true value, replication targets that are configured in the policy will be added internally. This option provides a value for a subfield of the JSON option 'targets-config'. It is mutually exclusive with that option.

    The default value is `false`.

`--targets-config-replications` (string)
:   Specifies a list of replication targets configurations. It can also be a path to a JSON file. This option provides a value for a subfield of the JSON option 'targets-config'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--targets-config-replications=@path/to/file.json`.

`--targets-config-archivals` (string)
:   Specifies a list of archival targets configurations. It can also be a path to a JSON file. This option provides a value for a subfield of the JSON option 'targets-config'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--targets-config-archivals=@path/to/file.json`.

`--targets-config-cloud-replications` (string)
:   Specifies a list of cloud replication targets configurations. It can also be a path to a JSON file. This option provides a value for a subfield of the JSON option 'targets-config'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--targets-config-cloud-replications=@path/to/file.json`.

#### Examples
{: #backup-recovery-protection-group-run-create-examples}

```sh
ibmcloud backup-recovery protection-group-run create \
    --id runId \
    --xibm-tenant-id tenantId \
    --run-type kRegular \
    --objects '[{"id": 4, "appIds": [26,27], "physicalParams": {"metadataFilePath": "~/metadata"}}]' \
    --targets-config '{"usePolicyDefaults": false, "replications": [{"id": 26, "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}], "archivals": [{"id": 26, "archivalTargetType": "Tape", "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}, "copyOnlyFullySuccessful": true}], "cloudReplications": [{"awsTarget": {"region": 26, "sourceId": 26}, "azureTarget": {"resourceGroup": 26, "sourceId": 26}, "targetType": "AWS", "retention": {"unit": "Days", "duration": 1, "dataLockConfig": {"mode": "Compliance", "unit": "Days", "duration": 1, "enableWormOnExternalTarget": true}}}]}'
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery protection-group-run create \
    --id runId \
    --xibm-tenant-id tenantId \
    --run-type kRegular \
    --objects '[runObject]' \
    --targets-config-use-policy-defaults=false \
    --targets-config-replications '[runReplicationConfig]' \
    --targets-config-archivals '[runArchivalConfig]' \
    --targets-config-cloud-replications '[runCloudReplicationConfig]'
```
{: pre}

### `ibmcloud backup-recovery protection-group-run perform-action`
{: #backup-recovery-cli-protection-group-run-perform-action-command}

Perform various actions on a Protection Group run.

```sh
ibmcloud backup-recovery protection-group-run perform-action --id ID --xibm-tenant-id XIBM-TENANT-ID --action ACTION [--pause-params PAUSE-PARAMS | @PAUSE-PARAMS-FILE] [--resume-params RESUME-PARAMS | @RESUME-PARAMS-FILE] [--cancel-params CANCEL-PARAMS | @CANCEL-PARAMS-FILE]
```


#### Command options
{: #backup-recovery-protection-group-run-perform-action-cli-options}

`--id` (string)
:   Specifies a unique id of the Protection Group. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--action` (string)
:   Specifies the type of the action that will be performed on protection runs. Required.

    Allowable values are: `Pause`, `Resume`, `Cancel`.

`--pause-params`
:   Specifies the pause action params for a protection run. It can also be a path to a JSON file.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--pause-params=@path/to/file.json`.

`--resume-params`
:   Specifies the resume action params for a protection run. It can also be a path to a JSON file.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--resume-params=@path/to/file.json`.

`--cancel-params`
:   Specifies the cancel action params for a protection run. It can also be a path to a JSON file.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--cancel-params=@path/to/file.json`.

#### Example
{: #backup-recovery-protection-group-run-perform-action-examples}

```sh
ibmcloud backup-recovery protection-group-run perform-action \
    --id runId \
    --xibm-tenant-id tenantId \
    --action Pause \
    --pause-params '[{"runId": "11:111"}]' \
    --resume-params '[{"runId": "11:111"}]' \
    --cancel-params '[{"runId": "11:111", "localTaskId": "123:456:789", "objectIds": [26,27], "replicationTaskId": ["123:456:789"], "archivalTaskId": ["123:456:789"], "cloudSpinTaskId": ["123:456:789"]}]'
```
{: pre}

## Recovery
{: #backup-recovery-recovery-cli}

Commands for Recovery resource.

```sh
ibmcloud backup-recovery recovery --help
```


### `ibmcloud backup-recovery recovery list`
{: #backup-recovery-cli-recovery-list-command}

Lists the Recoveries.

```sh
ibmcloud backup-recovery recovery list --xibm-tenant-id XIBM-TENANT-ID [--ids IDS] [--return-only-child-recoveries=RETURN-ONLY-CHILD-RECOVERIES] [--start-time-usecs START-TIME-USECS] [--end-time-usecs END-TIME-USECS] [--snapshot-target-type SNAPSHOT-TARGET-TYPE] [--archival-target-type ARCHIVAL-TARGET-TYPE] [--snapshot-environments SNAPSHOT-ENVIRONMENTS] [--status STATUS] [--recovery-actions RECOVERY-ACTIONS]
```


#### Command options
{: #backup-recovery-recovery-list-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--ids` ([]string)
:   Filter Recoveries for given ids.

    The list items must match regular expression `/^\\d+:\\d+:\\d+$/`.

`--return-only-child-recoveries` (bool)
:   Returns only child recoveries if passed as true. This filter should always be used along with 'ids' filter.

`--start-time-usecs` (int64)
:   Returns the recoveries, which are started after the specific time. This value should be in the Unix timestamp epoch in microseconds.

`--end-time-usecs` (int64)
:   Returns the recoveries, which are started before the specific time. This value should be in the Unix timestamp epoch in microseconds.

`--snapshot-target-type` ([]string)
:   Specifies the snapshot's target type from which recovery has been performed.

    Allowable list items are: `Local`, `Archival`, `RpaasArchival`, `StorageArraySnapshot`, `Remote`.

`--archival-target-type` ([]string)
:   Specifies the snapshot's archival target type from which recovery has been performed. This parameter applies only if 'snapshotTargetType' is 'Archival'.

    Allowable list items are: `Tape`, `Cloud`, `Nas`.

`--snapshot-environments` ([]string)
:   Specifies the list of snapshot environment types to filter Recoveries. If empty, Recoveries that are related to all environments will be returned.

    Allowable list items are: `kPhysical`, `kSQL`, `kKubernetes`.

`--status` ([]string)
:   Specifies the list of run status to filter Recoveries. If empty, Recoveries with all run status will be returned.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `LegalHold`.

`--recovery-actions` ([]string)
:   Specifies the list of recovery actions to filter Recoveries. If empty, Recoveries that are related to all actions will be returned.

    Allowable list items are: `RecoverVMs`, `RecoverFiles`, `InstantVolumeMount`, `RecoverVmDisks`, `RecoverVApps`, `RecoverVAppTemplates`, `UptierSnapshot`, `RecoverApps`, `CloneApps`, `RecoverNasVolume`, `RecoverPhysicalVolumes`, `RecoverSystem`, `RecoverExchangeDbs`, `CloneAppView`, `RecoverSanVolumes`, `RecoverSanGroup`, `RecoverMailbox`, `RecoverOneDrive`, `RecoverSharePoint`, `RecoverPublicFolders`, `RecoverMsGroup`, `RecoverMsTeam`, `ConvertToPst`, `DownloadChats`, `RecoverMailboxCSM`, `RecoverOneDriveCSM`, `RecoverSharePointCSM`, `RecoverNamespaces`, `RecoverObjects`, `RecoverSfdcObjects`, `RecoverSfdcOrg`, `RecoverSfdcRecords`, `DownloadFilesAndFolders`, `CloneVMs`, `CloneView`, `CloneRefreshApp`, `CloneVMsToView`, `ConvertAndDeployVMs`, `DeployVMs`.

#### Example
{: #backup-recovery-recovery-list-examples}

```sh
ibmcloud backup-recovery recovery list \
    --xibm-tenant-id tenantId \
    --ids 11:111:11 \
    --return-only-child-recoveries=true \
    --start-time-usecs 26 \
    --end-time-usecs 26 \
    --snapshot-target-type Local,Archival,RpaasArchival,StorageArraySnapshot,Remote \
    --archival-target-type Tape,Cloud,Nas \
    --snapshot-environments kPhysical,kSQL \
    --status Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,LegalHold \
    --recovery-actions RecoverVMs,RecoverFiles,InstantVolumeMount,RecoverVmDisks,RecoverVApps,RecoverVAppTemplates,UptierSnapshot,RecoverApps,CloneApps,RecoverNasVolume,RecoverPhysicalVolumes,RecoverSystem,RecoverExchangeDbs,CloneAppView,RecoverSanVolumes,RecoverSanGroup,RecoverMailbox,RecoverOneDrive,RecoverSharePoint,RecoverPublicFolders,RecoverMsGroup,RecoverMsTeam,ConvertToPst,DownloadChats,RecoverMailboxCSM,RecoverOneDriveCSM,RecoverSharePointCSM,RecoverNamespaces,RecoverObjects,RecoverSfdcObjects,RecoverSfdcOrg,RecoverSfdcRecords,DownloadFilesAndFolders,CloneVMs,CloneView,CloneRefreshApp,CloneVMsToView,ConvertAndDeployVMs,DeployVMs
```
{: pre}

### `ibmcloud backup-recovery recovery create`
{: #backup-recovery-cli-recovery-create-command}

Performs a Recovery.

```sh
ibmcloud backup-recovery recovery create --xibm-tenant-id XIBM-TENANT-ID --name NAME --snapshot-environment SNAPSHOT-ENVIRONMENT [--physical-params PHYSICAL-PARAMS | --physical-params-objects PHYSICAL-PARAMS-OBJECTS --physical-params-recovery-action PHYSICAL-PARAMS-RECOVERY-ACTION --physical-params-recover-volume-params PHYSICAL-PARAMS-RECOVER-VOLUME-PARAMS --physical-params-mount-volume-params PHYSICAL-PARAMS-MOUNT-VOLUME-PARAMS --physical-params-recover-file-and-folder-params PHYSICAL-PARAMS-RECOVER-FILE-AND-FOLDER-PARAMS --physical-params-download-file-and-folder-params PHYSICAL-PARAMS-DOWNLOAD-FILE-AND-FOLDER-PARAMS --physical-params-system-recovery-params PHYSICAL-PARAMS-SYSTEM-RECOVERY-PARAMS] [--kubernetes-params KUBERNETES-PARAMS | --kubernetes-params-download-file-and-folder-params KUBERNETES-PARAMS-DOWNLOAD-FILE-AND-FOLDER-PARAMS --kubernetes-params-objects KUBERNETES-PARAMS-OBJECTS --kubernetes-params-recover-file-and-folder-params KUBERNETES-PARAMS-RECOVER-FILE-AND-FOLDER-PARAMS --kubernetes-params-recover-namespace-params KUBERNETES-PARAMS-RECOVER-NAMESPACE-PARAMS --kubernetes-params-recovery-action KUBERNETES-PARAMS-RECOVERY-ACTION] [--mssql-params MSSQL-PARAMS | --mssql-params-recover-app-params MSSQL-PARAMS-RECOVER-APP-PARAMS --mssql-params-recovery-action MSSQL-PARAMS-RECOVERY-ACTION --mssql-params-vlan-config MSSQL-PARAMS-VLAN-CONFIG] [--request-initiator-type REQUEST-INITIATOR-TYPE]
```


#### Command options
{: #backup-recovery-recovery-create-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--name` (string)
:   Specifies the name of the Recovery. Required.

`--snapshot-environment` (string)
:   Specifies the type of environment of snapshots for which the Recovery has to be performed. Required.

    Allowable values are: `kPhysical`, `kSQL`.

`--kubernetes-params` (string)
:   Specifies the recovery options specific to the Kubernetes environment. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-download-file-and-folder-params` (string)
:   Specifies the parameters to download files and folders. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-objects` (string)
:   Specifies the list of objects that need to be recovered. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-recover-file-and-folder-params` (string)
:   Specifies the parameters to perform a file and folder recovery. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-recover-namespace-params` (string)
:   Specifies the parameters to recover Kubernetes Namespaces. It should be a JSON string or a path to a JSON file.

`--kubernetes-params-recovery-action` (string)
:   Specifies the type of recover action to be performed. Allowable values are: RecoverNamespaces, RecoverFiles, DownloadFilesAndFolders.

`--physical-params`
:   Specifies the recovery options specific to the Physical environment. It can also be a path to a JSON file. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params=@path/to/file.json`.

`--mssql-params`
:   Specifies the recovery options specific to the Sql environment. It can also be a path to a JSON file. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params=@path/to/file.json`.

`--request-initiator-type` (string)
:   Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests. Allowable values are: UIUser, UIAuto, Helios.

`--physical-params-objects` (string)
:   Specifies the list of Recover Object parameters. For recovering files, specifies the object contains the file to recover. It can also be a path to a JSON file. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params-objects=@path/to/file.json`.

`--physical-params-recovery-action` (string)
:   Specifies the type of recover action to be performed. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Allowable values are: `RecoverPhysicalVolumes`, `InstantVolumeMount`, `RecoverFiles`, `RecoverSystem`.

`--physical-params-recover-volume-params` (string)
:   Specifies the parameters to recover Physical Volumes. It can also be a path to a JSON file. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params-recover-volume-params=@path/to/file.json`.

`--physical-params-mount-volume-params` (string)
:   Specifies the parameters to mount Physical Volumes. It can also be a path to a JSON file. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params-mount-volume-params=@path/to/file.json`.

`--physical-params-recover-file-and-folder-params` (string)
:   Specifies the parameters to perform a file and folder recovery. It can also be a path to a JSON file. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params-recover-file-and-folder-params=@path/to/file.json`.

`--physical-params-download-file-and-folder-params` (string)
:   Specifies the parameters to download files and folders. It can also be a path to a JSON file. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params-download-file-and-folder-params=@path/to/file.json`.

`--physical-params-system-recovery-params` (string)
:   Specifies the parameters to perform a system recovery. It can also be a path to a JSON file. This option provides a value for a subfield of the JSON option 'physical-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--physical-params-system-recovery-params=@path/to/file.json`.

`--mssql-params-recover-app-params` (string)
:   Specifies the parameters to recover Sql databases. It can also be a path to a JSON file. The minimum length is 1 item. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params-recover-app-params=@path/to/file.json`.

`--mssql-params-recovery-action` (string)
:   Specifies the type of recover action to be performed. Allowable values are: RecoverApps, CloneApps. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

`--mssql-params-vlan-config` (string)
:   Specifies VLAN Params associated with the recovered. If this is not specified, then the VLAN settings are automatically selected from one of the below options: a. If VLANs are configured on IBM, then the VLAN host/VIP will be automatically based on the client's (for example ESXI host) IP address. b. If VLANs are not configured on IBM, then the partition hostname or VIPs are used for Recovery. It can also be a path to a JSON file. This option provides a value for a subfield of the JSON option 'mssql-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mssql-params-vlan-config=@path/to/file.json`.

#### Examples
{: #backup-recovery-recovery-create-examples}

```sh
ibmcloud backup-recovery recovery create \
    --xibm-tenant-id tenantId \
    --name create-recovery \
    --snapshot-environment kPhysical \
    --physical-params '{"objects": [{"snapshotId": "snapshotID", "pointInTimeUsecs": 26, "protectionGroupId": "protectionGroupID", "protectionGroupName": "protectionGroupName", "recoverFromStandby": true}], "recoveryAction": "RecoverPhysicalVolumes", "recoverVolumeParams": {"targetEnvironment": "kPhysical", "physicalTargetParams": {"mountTarget": {"id": 26}, "volumeMapping": [{"sourceVolumeGuid": "sourceVolumeGuid", "destinationVolumeGuid": "destinationVolumeGuid"}], "forceUnmountVolume": true, "vlanConfig": {"id": 38, "disableVlan": true}}}, "mountVolumeParams": {"targetEnvironment": "kPhysical", "physicalTargetParams": {"mountToOriginalTarget": true, "originalTargetConfig": {"serverCredentials": {"username": "Username", "password": "Password"}}, "newTargetConfig": {"mountTarget": {"id": 26}, "serverCredentials": {"username": "Username", "password": "Password"}}, "readOnlyMount": true, "volumeNames": ["volume1"], "vlanConfig": {"id": 38, "disableVlan": true}}}, "recoverFileAndFolderParams": {"filesAndFolders": [{"absolutePath": "~/folder1", "isDirectory": true, "isViewFileRecovery": true}], "targetEnvironment": "kPhysical", "physicalTargetParams": {"recoverTarget": {"id": 26}, "restoreToOriginalPaths": true, "overwriteExisting": true, "alternateRestoreDirectory": "~/dirAlt", "preserveAttributes": true, "preserveTimestamps": true, "preserveAcls": true, "continueOnError": true, "saveSuccessFiles": true, "vlanConfig": {"id": 38, "disableVlan": true}, "restoreEntityType": "kRegular"}}, "downloadFileAndFolderParams": {"expiryTimeUsecs": 26, "filesAndFolders": [{"absolutePath": "~/folder1", "isDirectory": true, "isViewFileRecovery": true}], "downloadFilePath": "~/downloadFile"}, "systemRecoveryParams": {"fullNasPath": "~/nas"}}' \
    --kubernetes-params '{"downloadFileAndFolderParams": {"expiryTimeUsecs": 26, "filesAndFolders": [{"absolutePath": "~/folder1", "isDirectory": true, "isViewFileRecovery": true}], "downloadFilePath": "exampleString"}, "objects": [{"snapshotId": "snapshotID", "pointInTimeUsecs": 26, "protectionGroupId": "protectionGroupID", "protectionGroupName": "protectionGroupName", "recoverFromStandby": true}], "recoverFileAndFolderParams": {"filesAndFolders": [{"absolutePath": "~/folder1", "isDirectory": true, "isViewFileRecovery": true}], "kubernetesTargetParams": {"continueOnError": true, "newTargetConfig": {"absolutePath": "exampleString", "targetNamespace": {"id": 26}, "targetPvc": {"id": 26}, "targetSource": {"id": 26}}, "originalTargetConfig": {"alternatePath": "exampleString", "recoverToOriginalPath": true}, "overwriteExisting": true, "preserveAttributes": true, "recoverToOriginalTarget": true, "vlanConfig": {"id": 38, "disableVlan": true}}, "targetEnvironment": "kKubernetes"}, "recoverNamespaceParams": {"kubernetesTargetParams": {"excludeParams": {"labelCombinationMethod": "AND", "labelVector": [{}], "objects": [26,27]}, "excludedPvcs": [{}], "includeParams": {"labelCombinationMethod": "AND", "labelVector": [{}], "objects": [26,27]}, "objects": [{"snapshotId": "snapshotID", "pointInTimeUsecs": 26, "protectionGroupId": "protectionGroupID", "protectionGroupName": "protectionGroupName", "recoverFromStandby": true}], "recoverProtectionGroupRunsParams": [{"archivalTargetId": 26, "protectionGroupId": "exampleString", "protectionGroupInstanceId": 26, "protectionGroupRunId": "exampleString"}], "recoverPvcsOnly": true, "recoveryTargetConfig": {"newSourceConfig": {"source": {"id": 26}}, "recoverToNewSource": true}, "renameRecoveredNamespacesParams": {"prefix": "exampleString", "suffix": "exampleString"}, "skipClusterCompatibilityCheck": true, "storageClass": {"storageClassMapping": [{}], "useStorageClassMapping": true}}, "targetEnvironment": "kKubernetes", "vlanConfig": {"id": 38, "disableVlan": true}}, "recoveryAction": "RecoverNamespaces"}' \
    --mssql-params '{"recoverAppParams": [{"snapshotId": "snapshotId", "pointInTimeUsecs": 26, "protectionGroupId": "protectionGroupId", "protectionGroupName": "protectionGroupName", "recoverFromStandby": true, "aagInfo": {"name": "aagInfoName", "objectId": 26}, "hostInfo": {"id": "hostInfoId", "name": "hostInfoName", "environment": "kPhysical"}, "isEncrypted": true, "sqlTargetParams": {"newSourceConfig": {"keepCdc": true, "multiStageRestoreOptions": {"enableAutoSync": true, "enableMultiStageRestore": true}, "nativeLogRecoveryWithClause": "LogRecoveryWithClause", "nativeRecoveryWithClause": "RecoveryWithClause", "overwritingPolicy": "FailIfExists", "replayEntireLastLog": true, "restoreTimeUsecs": 26, "secondaryDataFilesDirList": [{"directory": "~/dir1", "filenamePattern": ".sql"}], "withNoRecovery": true, "dataFileDirectoryLocation": "~/dir1", "databaseName": "recovery-database-sql", "host": {"id": 26}, "instanceName": "database-instance-1", "logFileDirectoryLocation": "~/dir2"}, "originalSourceConfig": {"keepCdc": true, "multiStageRestoreOptions": {"enableAutoSync": true, "enableMultiStageRestore": true}, "nativeLogRecoveryWithClause": "LogRecoveryWithClause", "nativeRecoveryWithClause": "RecoveryWithClause", "overwritingPolicy": "FailIfExists", "replayEntireLastLog": true, "restoreTimeUsecs": 26, "secondaryDataFilesDirList": [{"directory": "~/dir1", "filenamePattern": ".sql"}], "withNoRecovery": true, "captureTailLogs": true, "dataFileDirectoryLocation": "~/dir1", "logFileDirectoryLocation": "~/dir2", "newDatabaseName": "recovery-database-sql-new"}, "recoverToNewSource": true}, "targetEnvironment": "kSQL"}], "recoveryAction": "RecoverApps", "vlanConfig": {"id": 38, "disableVlan": true}}' \
    --request-initiator-type UIUser
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery recovery create \
    --xibm-tenant-id tenantId \
    --name create-recovery \
    --snapshot-environment kPhysical \
    --request-initiator-type UIUser \
    --physical-params-objects '[commonRecoverObjectSnapshotParams]' \
    --physical-params-recovery-action RecoverPhysicalVolumes \
    --physical-params-recover-volume-params recoverPhysicalParamsRecoverVolumeParams \
    --physical-params-mount-volume-params recoverPhysicalParamsMountVolumeParams \
    --physical-params-recover-file-and-folder-params recoverPhysicalParamsRecoverFileAndFolderParams \
    --physical-params-download-file-and-folder-params recoverPhysicalParamsDownloadFileAndFolderParams \
    --physical-params-system-recovery-params recoverPhysicalParamsSystemRecoveryParams \
    --mssql-params-recover-app-params '[recoverSqlAppParams]' \
    --mssql-params-recovery-action RecoverApps \
    --mssql-params-vlan-config recoveryVlanConfig
```
{: pre}

### `ibmcloud backup-recovery recovery get`
{: #backup-recovery-cli-recovery-get-command}

Get Recovery for a given id.

```sh
ibmcloud backup-recovery recovery get --id ID --xibm-tenant-id XIBM-TENANT-ID
```


#### Command options
{: #backup-recovery-recovery-get-cli-options}

`--id` (string)
:   Specifies the id of a Recovery. Required.

    The value must match regular expression `/^\\d+:\\d+:\\d+$/`.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

#### Example
{: #backup-recovery-recovery-get-examples}

```sh
ibmcloud backup-recovery recovery get \
    --id exampleString \
    --xibm-tenant-id tenantId
```
{: pre}

### `ibmcloud backup-recovery recovery files-download`
{: #backup-recovery-cli-recovery-files-download-command}

Download files from the given download file recovery.

```sh
ibmcloud backup-recovery recovery files-download --id ID --xibm-tenant-id XIBM-TENANT-ID [--start-offset START-OFFSET] [--length LENGTH] [--file-type FILE-TYPE] [--source-name SOURCE-NAME] [--start-time START-TIME] [--include-tenants=INCLUDE-TENANTS]
```


#### Command options
{: #backup-recovery-recovery-files-download-cli-options}

`--id` (string)
:   Specifies the id of a Recovery. Required.

    The value must match regular expression `/^\\d+:\\d+:\\d+$/`.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--start-offset` (int64)
:   Specifies the start offset of the file chunk to be downloaded.

`--length` (int64)
:   Specifies the length of bytes to download. This can’t be greater than 8MB (8388608 byets).

`--file-type` (string)
:   Specifies the downloaded type, that is: error, success_files_list.

`--source-name` (string)
:   Specifies the name of the source on which restore is done.

`--start-time` (string)
:   Specifies the start time of the restore task.

`--include-tenants` (bool)
:   Specifies whether objects of all the organizations under the hierarchy of the logged in user's organization should be returned.

#### Example
{: #backup-recovery-recovery-files-download-examples}

```sh
ibmcloud backup-recovery recovery files-download \
    --id exampleString \
    --xibm-tenant-id tenantId \
    --start-offset 26 \
    --length 26 \
    --file-type fileType \
    --source-name sourceName \
    --start-time startTime \
    --include-tenants=true
```
{: pre}

## Data-source-connection
{: #backup-recovery-data-source-connection-cli}

Commands for DataSourceConnection resource.

```sh
ibmcloud backup-recovery data-source-connection --help
```


### `ibmcloud backup-recovery data-source-connection list`
{: #backup-recovery-cli-data-source-connection-list-command}

Gets all specified data-source connections.

```sh
ibmcloud backup-recovery data-source-connection list --xibm-tenant-id XIBM-TENANT-ID [--connection-ids CONNECTION-IDS] [--connection-names CONNECTION-NAMES]
```


#### Command options
{: #backup-recovery-data-source-connection-list-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--connection-ids` ([]string)
:   Specifies the unique IDs of the connections, which are to be fetched.

`--connection-names` ([]string)
:   Specifies the names of the connections that are to be fetched.

#### Example
{: #backup-recovery-data-source-connection-list-examples}

```sh
ibmcloud backup-recovery data-source-connection list \
    --xibm-tenant-id tenantId \
    --connection-ids connectionId1,connectionId2 \
    --connection-names connectionName1,connectionName2
```
{: pre}

### `ibmcloud backup-recovery data-source-connection create`
{: #backup-recovery-cli-data-source-connection-create-command}

Creates a data-source connection that can be used to register and protect sources, to access filer services, and so on.

```sh
ibmcloud backup-recovery data-source-connection create --connection-name CONNECTION-NAME [--xibm-tenant-id XIBM-TENANT-ID]
```


#### Command options
{: #backup-recovery-data-source-connection-create-cli-options}

`--connection-name` (string)
:   Specifies the name of the connection being created. For a given tenant, different connections can't have the same name. However, two (or more) different tenants can each have a connection with the same name. Required.

`--xibm-tenant-id` (string)
:   Id of the tenant accessing the cluster.

#### Example
{: #backup-recovery-data-source-connection-create-examples}

```sh
ibmcloud backup-recovery data-source-connection create \
    --connection-name data-source-connection \
    --xibm-tenant-id tenantId
```
{: pre}

### `ibmcloud backup-recovery data-source-connection delete`
{: #backup-recovery-cli-data-source-connection-delete-command}

Delete a data-source connection by using its ID. After deleting a connection, any connectors within it won't be able to connect to the cluster. A connection should only be deleted after ensuring that no sources are using it.

```sh
ibmcloud backup-recovery data-source-connection delete --connection-id CONNECTION-ID --xibm-tenant-id XIBM-TENANT-ID [--force]
```


#### Command options
{: #backup-recovery-data-source-connection-delete-cli-options}

`--connection-id` (string)
:   Specifies the ID of the connection, connectors belonging to which are to be fetched. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`-f`, `--force` (bool)
:   Force the command to execute.

#### Example
{: #backup-recovery-data-source-connection-delete-examples}

```sh
ibmcloud backup-recovery data-source-connection delete \
    --connection-id exampleString \
    --xibm-tenant-id tenantId
```
{: pre}

### `ibmcloud backup-recovery data-source-connection patch`
{: #backup-recovery-cli-data-source-connection-patch-command}

Patch the data-source connection specified by the ID in the request path.

```sh
ibmcloud backup-recovery data-source-connection patch --connection-id CONNECTION-ID --xibm-tenant-id XIBM-TENANT-ID --connection-name CONNECTION-NAME
```


#### Command options
{: #backup-recovery-data-source-connection-patch-cli-options}

`--connection-id` (string)
:   Specifies the ID of the connection, connectors belonging to which are to be fetched. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--connection-name` (string)
:   A new name for the connection being patched. Required.

#### Example
{: #backup-recovery-data-source-connection-patch-examples}

```sh
ibmcloud backup-recovery data-source-connection patch \
    --connection-id connectionId \
    --xibm-tenant-id tenantId \
    --connection-name connectionName
```
{: pre}

### `ibmcloud backup-recovery data-source-connection registration-token-generate`
{: #backup-recovery-cli-data-source-connection-registration-token-generate-command}

Generate a token to register connectors against the data-source connection specified by the ID in the request path. The same token can be used to register multiple connectors as long as the token is valid. Once the token expires, typically in a day, this API can be hit again to generate another token.

```sh
ibmcloud backup-recovery data-source-connection registration-token-generate --connection-id CONNECTION-ID --xibm-tenant-id XIBM-TENANT-ID
```


#### Command options
{: #backup-recovery-data-source-connection-registration-token-generate-cli-options}

`--connection-id` (string)
:   Specifies the ID of the connection, connectors belonging to which are to be fetched. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

#### Example
{: #backup-recovery-data-source-connection-registration-token-generate-examples}

```sh
ibmcloud backup-recovery data-source-connection registration-token-generate \
    --connection-id exampleString \
    --xibm-tenant-id tenantId
```
{: pre}

## Data-source-connector
{: #backup-recovery-data-source-connector-cli}

Commands for DataSourceConnector resource.

```sh
ibmcloud backup-recovery data-source-connector --help
```


### `ibmcloud backup-recovery data-source-connector list`
{: #backup-recovery-cli-data-source-connector-list-command}

Gets all specified data-source connectors.

```sh
ibmcloud backup-recovery data-source-connector list --xibm-tenant-id XIBM-TENANT-ID [--connector-ids CONNECTOR-IDS] [--connector-names CONNECTOR-NAMES] [--connection-id CONNECTION-ID]
```


#### Command options
{: #backup-recovery-data-source-connector-list-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--connector-ids` ([]string)
:   Specifies the unique IDs of the connectors, which are to be fetched.

`--connector-names` ([]string)
:   Specifies the names of the connectors, which are to be fetched.

`--connection-id` (string)
:   Specifies the ID of the connection, connectors belonging to which are to be fetched.

#### Example
{: #backup-recovery-data-source-connector-list-examples}

```sh
ibmcloud backup-recovery data-source-connector list \
    --xibm-tenant-id tenantId \
    --connector-ids connectorId1,connectorId2 \
    --connector-names connectionName1,connectionName2 \
    --connection-id exampleString
```
{: pre}

### `ibmcloud backup-recovery data-source-connector delete`
{: #backup-recovery-cli-data-source-connector-delete-command}

Delete the data-source connector specified by the ID in the request path.

```sh
ibmcloud backup-recovery data-source-connector delete --connector-id CONNECTOR-ID --xibm-tenant-id XIBM-TENANT-ID [--force]
```


#### Command options
{: #backup-recovery-data-source-connector-delete-cli-options}

`--connector-id` (string)
:   Specifies the unique ID of the connector that is to be deleted. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`-f`, `--force` (bool)
:   Force the command to execute without confirmation.

#### Example
{: #backup-recovery-data-source-connector-delete-examples}

```sh
ibmcloud backup-recovery data-source-connector delete \
    --connector-id connectorId \
    --xibm-tenant-id tenantId
```
{: pre}

### `ibmcloud backup-recovery data-source-connector patch`
{: #backup-recovery-cli-data-source-connector-patch-command}

Patch the data-source connector specified by the ID in the request path.

```sh
ibmcloud backup-recovery data-source-connector patch --connector-id CONNECTOR-ID --xibm-tenant-id XIBM-TENANT-ID [--connector-name CONNECTOR-NAME]
```


#### Command options
{: #backup-recovery-data-source-connector-patch-cli-options}

`--connector-id` (string)
:   Specifies the unique ID of the connector that is to be patched. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--connector-name` (string)
:   Name of the connector.

#### Example
{: #backup-recovery-data-source-connector-patch-examples}

```sh
ibmcloud backup-recovery data-source-connector patch \
    --connector-id connectorID \
    --xibm-tenant-id tenantId \
    --connector-name connectorName
```
{: pre}
### `ibmcloud backup-recovery data-source-connector-logs`
{: #backup-recovery-cli-data-source-connector-logs-command}

Lists the logs corresponding to the data-source connector creation and registration.

```sh
ibmcloud backup-recovery data-source-connector-logs --access-token ACCESS-TOKEN
```


#### Command options
{: #backup-recovery-data-source-connector-logs-cli-options}

`--access-token` (string)
:   Access-token that is received from the connector. Required.

#### Example
{: #backup-recovery-data-source-connector-logs-examples}

```sh
ibmcloud backup-recovery data-source-connector-logs \
    --access-token eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdXRoLXR5cGUiOiIxIiwiZG9tYWluIjoiTE9DQUwiLCJleHBpcmF0aW9uLXRpbWUiOiIxNzM4NjY0NTI2IiwiaW4tY2x1c3RlciI6dHJ1ZSwibG9jYWxlIjoiZW4tdXMiLCJyb2xlcyI6IkNPSEVTSVRZX0FETUlOIiwic2lkcy1oYXNoIjoidG9CT3FhSllHUVhOTEF6ZWN5TTh1S05vbXNMT1VnbFVQUjYwNTJkdmJIYyIsInVzZXItc2lkIjoiUy0xLTEwMC0yMS0xMDY3Mjc0Mi0zOTcxMDE1MS0xIiwidXNlcm5hbWUiOiJhZG1pbiJ9.CiW0yedyrx7GQeI9GxloKU-zcuHUDCt0jvcz6H2bGd0ABNUWxryX22xNzEzAYIjoU3qdaS1hF7ch9WzKU-Jnk3eh2wmn_Ezb8Qe_gmxmeXRxKqPGSnVYZdMREXQsPJYfbncyftr-iiOluPZ52UgzkcBS2MeRIur5UEYNCumZqoYDVXAxLbuEyBhIrWMPQMUAe2gym4QRd6U-zmtMoLwa6DlKyzJsV_75mR-B9Vg9Aq78_DjXTgX6Lbq_IJJHplL83Sd1vJhlMO92C1Zm8AF2n_PeyDeFbUtU6TfCS6BlqFAfFv1sxDjKLAoPqmNagvEB-w_HeW_dfjGLfNUzqc3JUQ
```
{: pre}
### `ibmcloud backup-recovery data-source-connector-register`
{: #backup-recovery-cli-data-source-connector-register-command}

Register a data source connector with a cluster by using the supplied registration token. The registration token for the data-source connection with which this connector is to be registered has to be obtained by the user by invoking the relevant '/data-source-connections' APIs.

```sh
ibmcloud backup-recovery data-source-connector-register --registration-token REGISTRATION-TOKEN [--access-token ACCESS-TOKEN] [--connector-id CONNECTOR-ID]
```


#### Command options
{: #backup-recovery-data-source-connector-register-cli-options}

`--registration-token` (string)
:   The registration token. Required.

`--access-token` (string)
:   Access token for authentication.

`--connector-id` (int64)
:   The connector's ID to be used for registration. Two connectors belonging to the same tenant are guaranteed to have different IDs.

#### Example
{: #backup-recovery-data-source-connector-register-examples}

```sh
ibmcloud backup-recovery data-source-connector-register \
    --access-token eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdXRoLXR5cGUiOiIxIiwiZG9tYWluIjoiTE9DQUwiLCJleHBpcmF0aW9uLXRpbWUiOiIxNzM4NjY0NTI2IiwiaW4tY2x1c3RlciI6dHJ1ZSwibG9jYWxlIjoiZW4tdXMiLCJyb2xlcyI6IkNPSEVTSVRZX0FETUlOIiwic2lkcy1oYXNoIjoidG9CT3FhSllHUVhOTEF6ZWN5TTh1S05vbXNMT1VnbFVQUjYwNTJkdmJIYyIsInVzZXItc2lkIjoiUy0xLTEwMC0yMS0xMDY3Mjc0Mi0zOTcxMDE1MS0xIiwidXNlcm5hbWUiOiJhZG1pbiJ9.CiW0yedyrx7GQeI9GxloKU-zcuHUDCt0jvcz6H2bGd0ABNUWxryX22xNzEzAYIjoU3qdaS1hF7ch9WzKU-Jnk3eh2wmn_Ezb8Qe_gmxmeXRxKqPGSnVYZdMREXQsPJYfbncyftr-iiOluPZ52UgzkcBS2MeRIur5UEYNCumZqoYDVXAxLbuEyBhIrWMPQMUAe2gym4QRd6U-zmtMoLwa6DlKyzJsV_75mR-B9Vg9Aq78_DjXTgX6Lbq_IJJHplL83Sd1vJhlMO92C1Zm8AF2n_PeyDeFbUtU6TfCS6BlqFAfFv1sxDjKLAoPqmNagvEB-w_HeW_dfjGLfNUzqc3JUQ \
    --registration-token exampleString \
    --connector-id 26
```
### `ibmcloud backup-recovery data-source-connector-status`
{: #backup-recovery-cli-data-source-connector-status-command}

Lists the data source connector status, which includes registration as well as cluster-connectivity status.

```sh
ibmcloud backup-recovery data-source-connector-status --access-token ACCESS-TOKEN
```


#### Command options
{: #backup-recovery-data-source-connector-status-cli-options}

`--access-token` (string)
:   Access-token that is received from the connector. Required.

#### Example
{: #backup-recovery-data-source-connector-status-examples}

```sh
ibmcloud backup-recovery data-source-connector-status --access-token eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdXRoLXR5cGUiOiIxIiwiZG9tYWluIjoiTE9DQUwiLCJleHBpcmF0aW9uLXRpbWUiOiIxNzM4NjY0NTI2IiwiaW4tY2x1c3RlciI6dHJ1ZSwibG9jYWxlIjoiZW4tdXMiLCJyb2xlcyI6IkNPSEVTSVRZX0FETUlOIiwic2lkcy1oYXNoIjoidG9CT3FhSllHUVhOTEF6ZWN5TTh1S05vbXNMT1VnbFVQUjYwNTJkdmJIYyIsInVzZXItc2lkIjoiUy0xLTEwMC0yMS0xMDY3Mjc0Mi0zOTcxMDE1MS0xIiwidXNlcm5hbWUiOiJhZG1pbiJ9.CiW0yedyrx7GQeI9GxloKU-zcuHUDCt0jvcz6H2bGd0ABNUWxryX22xNzEzAYIjoU3qdaS1hF7ch9WzKU-Jnk3eh2wmn_Ezb8Qe_gmxmeXRxKqPGSnVYZdMREXQsPJYfbncyftr-iiOluPZ52UgzkcBS2MeRIur5UEYNCumZqoYDVXAxLbuEyBhIrWMPQMUAe2gym4QRd6U-zmtMoLwa6DlKyzJsV_75mR-B9Vg9Aq78_DjXTgX6Lbq_IJJHplL83Sd1vJhlMO92C1Zm8AF2n_PeyDeFbUtU6TfCS6BlqFAfFv1sxDjKLAoPqmNagvEB-w_HeW_dfjGLfNUzqc3JUQ
```
{: pre}



## Agent commands
{: #backup-recovery-agent-cli}

### `ibmcloud backup-recovery agent-download`
{: #backup-recovery-cli-agent-download-command}

Download agent for different hosts.

```sh
ibmcloud backup-recovery agent-download --xibm-tenant-id XIBM-TENANT-ID --platform PLATFORM [--linux-params (LINUX-PARAMS | @LINUX-PARAMS-FILE) | --linux-params-package-type LINUX-PARAMS-PACKAGE-TYPE]
```


#### Command options
{: #backup-recovery-agent-download-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--platform` (string)
:   Specifies the platform for which the agent needs to be downloaded. Required.

    Allowable values are: `kWindows`, `kLinux`.

`--linux-params` (string)
:   Linux agent parameters. It can also be a path to a JSON file.

`--linux-params-package-type` (string)
:   Specifies the type of installer.

    Allowable values are: `kScript`, `kRPM`, `kSuseRPM`, `kDEB`, `kPowerPCRPM`.

`--output-file` (string)
:   Filename/path to write the resulting output to.

#### Examples
{: #backup-recovery-agent-download-examples}

```sh
ibmcloud backup-recovery agent-download \
    --xibm-tenant-id tenantId \
    --platform kWindows \
    --linux-params '{"packageType": "kScript"}' \
    --output-file tempdir/example-output.txt
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery agent-download \
    --xibm-tenant-id tenantId \
    --platform kWindows \
    --linux-params-package-type kScript \
    --output-file tempdir/example-output.txt
```
{: pre}

## Data source connector commands
{: #backup-recovery-data-source-connector-cli}

### `ibmcloud backup-recovery connector-metadata-get`
{: #backup-recovery-cli-connector-metadata-get-command}

Get information about the available connectors.

```sh
ibmcloud backup-recovery connector-metadata-get --xibm-tenant-id XIBM-TENANT-ID
```


#### Command options
{: #backup-recovery-connector-metadata-get-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

#### Example
{: #backup-recovery-connector-metadata-get-examples}

```sh
ibmcloud backup-recovery connector-metadata-get \
    --xibm-tenant-id tenantId
```
{: pre}

## Object commands
{: #backup-recovery-object-cli}

### `ibmcloud backup-recovery object-snapshots-list`
{: #backup-recovery-cli-object-snapshots-list-command}

List the snapshots for a given object.

```sh
ibmcloud backup-recovery object-snapshots-list --id ID --xibm-tenant-id XIBM-TENANT-ID [--from-time-usecs FROM-TIME-USECS] [--to-time-usecs TO-TIME-USECS] [--run-start-from-time-usecs RUN-START-FROM-TIME-USECS] [--run-start-to-time-usecs RUN-START-TO-TIME-USECS] [--snapshot-actions SNAPSHOT-ACTIONS] [--run-types RUN-TYPES] [--protection-group-ids PROTECTION-GROUP-IDS] [--run-instance-ids RUN-INSTANCE-IDS] [--region-ids REGION-IDS] [--object-action-keys OBJECT-ACTION-KEYS]
```


#### Command options
{: #backup-recovery-object-snapshots-list-cli-options}

`--id` (int64)
:   Specifies the id of the Object. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--from-time-usecs` (int64)
:   Specifies the timestamp in the Unix time epoch in microseconds to filter Object's snapshots that were taken after this value.

`--to-time-usecs` (int64)
:   Specifies the timestamp in the Unix time epoch in microseconds to filter Object's snapshots that were taken before this value.

`--run-start-from-time-usecs` (int64)
:   Specifies the timestamp in Unix time epoch in microseconds to filter Object's snapshots that were run after this value.

`--run-start-to-time-usecs` (int64)
:   Specifies the timestamp in the Unix time epoch in microseconds to filter Object's snapshots, which were run before this value.

`--snapshot-actions` ([]string)
:   Specifies a list of recovery actions. Only snapshots that apply to these actions will be returned.

    Allowable list items are: `RecoverVMs`, `RecoverFiles`, `InstantVolumeMount`, `RecoverVmDisks`, `MountVolumes`, `RecoverVApps`, `RecoverApps`, `RecoverNasVolume`, `RecoverPhysicalVolumes`, `RecoverSystem`, `RecoverSanVolumes`, `RecoverNamespaces`, `RecoverObjects`, `DownloadFilesAndFolders`, `RecoverPublicFolders`, `RecoverVAppTemplates`, `RecoverMailbox`, `RecoverOneDrive`, `RecoverMsTeam`, `RecoverMsGroup`, `RecoverSharePoint`, `ConvertToPst`, `RecoverSfdcRecords`, `DownloadChats`, `RecoverMailboxCSM`, `RecoverOneDriveCSM`, `RecoverSharePointCSM`.

`--run-types` ([]string)
:   Filter by run type. Only protection runs matching the specified types will be returned. By default, CDP hydration snapshots are not included unless explicitly queried by using this field.

    Allowable list items are: `kRegular`, `kFull`, `kLog`, `kSystem`, `kHydrateCDP`, `kStorageArraySnapshot`.

`--protection-group-ids` ([]string)
:   If specified, this returns only the snapshots of the specified object ID, which belong to the provided protection group IDs.

`--run-instance-ids` ([]int64)
:   Filter by a list of run instance IDs. If specified, only snapshots that are created by these protection runs will be returned.

`--region-ids` ([]string)
:   Filter by a list of region IDs.

`--object-action-keys` ([]string)
:   Filter by ObjectActionKey, which uniquely represents the protection of an object. An object can be protected in multiple ways but at most once for a given combination of ObjectActionKey. When specified, only snapshots matching the given action keys are returned for the corresponding object.

    Allowable list items are: `kVMware`, `kHyperV`, `kKVM`, `kAcropolis`, `kPhysical`, `kPhysicalFiles`, `kGPFS`, `kElastifile`, `kNetapp`, `kGenericNas`, `kIsilon`, `kFlashBlade`, `kPure`, `kIbmFlashSystem`, `kSQL`, `kExchange`, `kAD`, `kOracle`, `kView`, `kRemoteAdapter`, `kO365`, `kO365PublicFolders`, `kO365Teams`, `kO365Group`, `kO365Exchange`, `kO365OneDrive`, `kO365Sharepoint`, `kKubernetes`, `kCassandra`, `kMongoDB`, `kCouchbase`, `kHdfs`, `kHive`, `kHBase`, `kSAPHANA`, `kUDA`, `kSfdc`, `kO365ExchangeCSM`, `kO365OneDriveCSM`, `kO365SharepointCSM`.

#### Example
{: #backup-recovery-object-snapshots-list-examples}

```sh
ibmcloud backup-recovery object-snapshots-list \
    --id 26 \
    --xibm-tenant-id tenantId \
    --from-time-usecs 26 \
    --to-time-usecs 26 \
    --run-start-from-time-usecs 26 \
    --run-start-to-time-usecs 26 \
    --snapshot-actions RecoverVMs,RecoverFiles,InstantVolumeMount,RecoverVmDisks,MountVolumes,RecoverVApps,RecoverApps,RecoverNasVolume,RecoverPhysicalVolumes,RecoverSystem,RecoverSanVolumes,RecoverNamespaces,RecoverObjects,DownloadFilesAndFolders,RecoverPublicFolders,RecoverVAppTemplates,RecoverMailbox,RecoverOneDrive,RecoverMsTeam,RecoverMsGroup,RecoverSharePoint,ConvertToPst,RecoverSfdcRecords,DownloadChats,RecoverMailboxCSM,RecoverOneDriveCSM,RecoverSharePointCSM \
    --run-types kRegular,kFull,kLog,kSystem,kHydrateCDP,kStorageArraySnapshot \
    --protection-group-ids protectionGroupId1 \
    --run-instance-ids 26,27 \
    --region-ids regionId1 \
    --object-action-keys kVMware,kHyperV,kKVM,kAcropolis,kPhysical,kPhysicalFiles,kGPFS,kElastifile,kNetapp,kGenericNas,kIsilon,kFlashBlade,kPure,kIbmFlashSystem,kSQL,kExchange,kAD,kOracle,kView,kRemoteAdapter,kO365,kO365PublicFolders,kO365Teams,kO365Group,kO365Exchange,kO365OneDrive,kO365Sharepoint,kKubernetes,kCassandra,kMongoDB,kCouchbase,kHdfs,kHive,kHBase,kSAPHANA,kUDA,kSfdc,kO365ExchangeCSM,kO365OneDriveCSM,kO365SharepointCSM
```
{: pre}

## Recovery commands
{: #backup-recovery-recovery-cli}

### `ibmcloud backup-recovery download-recovery-create`
{: #backup-recovery-cli-download-recovery-create-command}

Creates a download files and folders recovery.

```sh
ibmcloud backup-recovery download-recovery-create --xibm-tenant-id XIBM-TENANT-ID --name NAME --files-and-folders FILES-AND-FOLDERS | @FILES-AND-FOLDERS-FILE [--object (OBJECT | @OBJECT-FILE) | --object-snapshot-id OBJECT-SNAPSHOT-ID --object-point-in-time-usecs OBJECT-POINT-IN-TIME-USECS --object-protection-group-id OBJECT-PROTECTION-GROUP-ID --object-protection-group-name OBJECT-PROTECTION-GROUP-NAME --object-recover-from-standby=OBJECT-RECOVER-FROM-STANDBY] [--documents DOCUMENTS | @DOCUMENTS-FILE] [--parent-recovery-id PARENT-RECOVERY-ID] [--glacier-retrieval-type GLACIER-RETRIEVAL-TYPE]
```


#### Command options
{: #backup-recovery-download-recovery-create-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--name` (string)
:   Specifies the name of the recovery task. This field must be set and must be a unique name. Required.

`--object`
:   Specifies the common snapshot parameters for a protected object. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--object=@path/to/file.json`.

`--files-and-folders`
:   Specifies the list of files and folders to download. Required.

    The minimum length is `1` item.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--files-and-folders=@path/to/file.json`.

`--documents`
:   Specifies the list of documents to download by using item ids. Only one of filesAndFolders or documents should be used. Currently, only files are supported by documents.

    The minimum length is `1` item.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--documents=@path/to/file.json`.

`--parent-recovery-id` (string)
:   If current recovery is a child task that is triggered through another parent recovery operation, then this field specifies the id of the parent recovery.

    The value must match regular expression `/^\\d+:\\d+:\\d+$/`.

`--glacier-retrieval-type` (string)
:   Specifies the glacier retrieval type when restoring or downloading files or folders from a Glacier-based cloud snapshot.

    Allowable values are: `kStandard`, `kExpeditedNoPCU`, `kExpeditedWithPCU`.

`--object-snapshot-id` (string)
:   Specifies the snapshot id. This option provides a value for a subfield of the JSON option 'object'. It is mutually exclusive with that option.

`--object-point-in-time-usecs` (int64)
:   Specifies the timestamp (in microseconds from epoch) for recovering to a point-in-time in the past. This option provides a value for a subfield of the JSON option 'object'. It is mutually exclusive with that option.

`--object-protection-group-id` (string)
:   Specifies the protection group ID of the object snapshot. This option provides a value for a subfield of the JSON option 'object'. It is mutually exclusive with that option.

`--object-protection-group-name` (string)
:   Specifies the protection group name of the object snapshot. This option provides a value for a subfield of the JSON option 'object'. It is mutually exclusive with that option.

`--object-recover-from-standby` (bool)
:   Specifies that the user wants to perform standby restore if it is enabled for this object. This option provides a value for a subfield of the JSON option 'object'. It is mutually exclusive with that option.

#### Examples
{: #backup-recovery-download-recovery-create-examples}

```sh
ibmcloud backup-recovery download-recovery-create \
    --xibm-tenant-id tenantId \
    --name create-download-files-and-folders-recovery \
    --object '{"snapshotId": "snapshotId", "pointInTimeUsecs": 26, "protectionGroupId": "protectionGroupId", "protectionGroupName": "protectionGroupName", "recoverFromStandby": true}' \
    --files-and-folders '[{"absolutePath": "~/home/dir1", "isDirectory": true}]' \
    --documents '[{"isDirectory": true, "itemId": "item1"}]' \
    --parent-recovery-id parentRecoveryId \
    --glacier-retrieval-type kStandard
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery download-recovery-create \
    --xibm-tenant-id tenantId \
    --name create-download-files-and-folders-recovery \
    --files-and-folders '[filesAndFoldersObject]' \
    --documents '[documentObject]' \
    --parent-recovery-id parentRecoveryId \
    --glacier-retrieval-type kStandard \
    --object-snapshot-id exampleString \
    --object-point-in-time-usecs 26 \
    --object-protection-group-id exampleString \
    --object-protection-group-name exampleString \
    --object-recover-from-standby=true
```
{: pre}

### `ibmcloud backup-recovery restore-points`
{: #backup-recovery-cli-restore-points-command}

List Restore Points, that is, returns the snapshots in a given time range.

```sh
ibmcloud backup-recovery restore-points --xibm-tenant-id XIBM-TENANT-ID --end-time-usecs END-TIME-USECS --environment ENVIRONMENT --protection-group-ids PROTECTION-GROUP-IDS --start-time-usecs START-TIME-USECS [--source-id SOURCE-ID]
```


#### Command options
{: #backup-recovery-restore-points-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--end-time-usecs` (int64)
:   Specifies the end time specified as a Unix epoch Timestamp in microseconds. Required.

`--environment` (string)
:   Specifies the protection source environment type. Required.

    Allowable values are: `kVMware`, `kHyperV`, `kKVM`, `kAcropolis`, `kPhysical`, `kGPFS`, `kElastifile`, `kNetapp`, `kGenericNas`, `kIsilon`, `kFlashBlade`, `kPure`, `kIbmFlashSystem`, `kSQL`, `kExchange`, `kAD`, `kOracle`, `kView`, `kRemoteAdapter`, `kO365`, `kKubernetes`, `kCassandra`, `kMongoDB`, `kCouchbase`, `kHdfs`, `kHive`, `kSAPHANA`, `kHBase`, `kUDA`, `kSfdc`.

`--protection-group-ids` ([]string)
:   Specifies the jobs for which to get the full snapshot information. Required.

`--start-time-usecs` (int64)
:   Specifies the start time specified as a Unix epoch Timestamp in microseconds. Required.

`--source-id` (int64)
:   Specifies the id of the Protection Source that is to be restored.

#### Example
{: #backup-recovery-restore-points-examples}

```sh
ibmcloud backup-recovery restore-points \
    --xibm-tenant-id tenantId \
    --end-time-usecs 45 \
    --environment kVMware \
    --protection-group-ids protectionGroupId1 \
    --start-time-usecs 15 \
    --source-id 26
```
{: pre}

### `ibmcloud backup-recovery indexed-file-download`
{: #backup-recovery-cli-indexed-file-download-command}

Download an indexed file from a snapshot.

```sh
ibmcloud backup-recovery indexed-file-download --snapshots-id SNAPSHOTS-ID --xibm-tenant-id XIBM-TENANT-ID [--file-path FILE-PATH] [--nvram-file=NVRAM-FILE] [--retry-attempt RETRY-ATTEMPT] [--start-offset START-OFFSET] [--length LENGTH]
```


#### Command options
{: #backup-recovery-indexed-file-download-cli-options}

`--snapshots-id` (string)
:   Specifies the snapshot id to download from. Required.

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--file-path` (string)
:   Specifies the path to the file to download. If no path is specified and the snapshot environment is kVMWare, a VMX file for VMware will be downloaded. For other snapshot environments, this field must be specified.

`--nvram-file` (bool)
:   Specifies whether NVRAM file for VMware should be downloaded.

`--retry-attempt` (int64)
:   Specifies the number of attempts the protection run took to create this file.

`--start-offset` (int64)
:   Specifies the start offset of the file chunk to be downloaded.

`--length` (int64)
:   Specifies the length of bytes to download. This can’t be greater than 8MB (8388608 byets).

#### Example
{: #backup-recovery-indexed-file-download-examples}

```sh
ibmcloud backup-recovery indexed-file-download \
    --snapshots-id snapshotId1 \
    --xibm-tenant-id tenantId \
    --file-path ~/home/downloadFile \
    --nvram-file=true \
    --retry-attempt 26 \
    --start-offset 26 \
    --length 26
```
{: pre}

## Search commands
{: #backup-recovery-search-cli}

### `ibmcloud backup-recovery indexed-objects-search`
{: #backup-recovery-cli-indexed-objects-search-command}

List all the indexed objects like files and folders, emails, mailboxes etc., that match the specified search and filter criteria from protected objects.

```sh
ibmcloud backup-recovery indexed-objects-search --xibm-tenant-id XIBM-TENANT-ID --object-type OBJECT-TYPE [--protection-group-ids PROTECTION-GROUP-IDS] [--storage-domain-ids STORAGE-DOMAIN-IDS] [--tenant-id TENANT-ID] [--include-tenants=INCLUDE-TENANTS] [--tags TAGS] [--snapshot-tags SNAPSHOT-TAGS] [--must-have-tag-ids MUST-HAVE-TAG-IDS] [--might-have-tag-ids MIGHT-HAVE-TAG-IDS] [--must-have-snapshot-tag-ids MUST-HAVE-SNAPSHOT-TAG-IDS] [--might-have-snapshot-tag-ids MIGHT-HAVE-SNAPSHOT-TAG-IDS] [--pagination-cookie PAGINATION-COOKIE] [--count COUNT] [--use-cached-data=USE-CACHED-DATA] [--cassandra-params CASSANDRA-PARAMS | --cassandra-params-cassandra-object-types CASSANDRA-PARAMS-CASSANDRA-OBJECT-TYPES --cassandra-params-search-string CASSANDRA-PARAMS-SEARCH-STRING --cassandra-params-source-ids CASSANDRA-PARAMS-SOURCE-IDS] [--couchbase-params COUCHBASE-PARAMS | --couchbase-params-couchbase-object-types COUCHBASE-PARAMS-COUCHBASE-OBJECT-TYPES --couchbase-params-search-string COUCHBASE-PARAMS-SEARCH-STRING --couchbase-params-source-ids COUCHBASE-PARAMS-SOURCE-IDS] [--email-params EMAIL-PARAMS | --email-params-attendees-addresses EMAIL-PARAMS-ATTENDEES-ADDRESSES --email-params-bcc-recipient-addresses EMAIL-PARAMS-BCC-RECIPIENT-ADDRESSES --email-params-cc-recipient-addresses EMAIL-PARAMS-CC-RECIPIENT-ADDRESSES --email-params-created-end-time-secs EMAIL-PARAMS-CREATED-END-TIME-SECS --email-params-created-start-time-secs EMAIL-PARAMS-CREATED-START-TIME-SECS --email-params-due-date-end-time-secs EMAIL-PARAMS-DUE-DATE-END-TIME-SECS --email-params-due-date-start-time-secs EMAIL-PARAMS-DUE-DATE-START-TIME-SECS --email-params-email-address EMAIL-PARAMS-EMAIL-ADDRESS --email-params-email-subject EMAIL-PARAMS-EMAIL-SUBJECT --email-params-first-name EMAIL-PARAMS-FIRST-NAME --email-params-folder-names EMAIL-PARAMS-FOLDER-NAMES --email-params-has-attachment=EMAIL-PARAMS-HAS-ATTACHMENT --email-params-last-modified-end-time-secs EMAIL-PARAMS-LAST-MODIFIED-END-TIME-SECS --email-params-last-modified-start-time-secs EMAIL-PARAMS-LAST-MODIFIED-START-TIME-SECS --email-params-last-name EMAIL-PARAMS-LAST-NAME --email-params-middle-name EMAIL-PARAMS-MIDDLE-NAME --email-params-organizer-address EMAIL-PARAMS-ORGANIZER-ADDRESS --email-params-received-end-time-secs EMAIL-PARAMS-RECEIVED-END-TIME-SECS --email-params-received-start-time-secs EMAIL-PARAMS-RECEIVED-START-TIME-SECS --email-params-recipient-addresses EMAIL-PARAMS-RECIPIENT-ADDRESSES --email-params-sender-address EMAIL-PARAMS-SENDER-ADDRESS --email-params-source-environment EMAIL-PARAMS-SOURCE-ENVIRONMENT --email-params-task-status-types EMAIL-PARAMS-TASK-STATUS-TYPES --email-params-types EMAIL-PARAMS-TYPES --email-params-o365-params EMAIL-PARAMS-O365-PARAMS] [--exchange-params EXCHANGE-PARAMS | --exchange-params-search-string EXCHANGE-PARAMS-SEARCH-STRING] [--file-params FILE-PARAMS | --file-params-search-string FILE-PARAMS-SEARCH-STRING --file-params-types FILE-PARAMS-TYPES --file-params-source-environments FILE-PARAMS-SOURCE-ENVIRONMENTS --file-params-source-ids FILE-PARAMS-SOURCE-IDS --file-params-object-ids FILE-PARAMS-OBJECT-IDS] [--hbase-params HBASE-PARAMS | --hbase-params-hbase-object-types HBASE-PARAMS-HBASE-OBJECT-TYPES --hbase-params-search-string HBASE-PARAMS-SEARCH-STRING --hbase-params-source-ids HBASE-PARAMS-SOURCE-IDS] [--hdfs-params HDFS-PARAMS | --hdfs-params-hdfs-types HDFS-PARAMS-HDFS-TYPES --hdfs-params-search-string HDFS-PARAMS-SEARCH-STRING --hdfs-params-source-ids HDFS-PARAMS-SOURCE-IDS] [--hive-params HIVE-PARAMS | --hive-params-hive-object-types HIVE-PARAMS-HIVE-OBJECT-TYPES --hive-params-search-string HIVE-PARAMS-SEARCH-STRING --hive-params-source-ids HIVE-PARAMS-SOURCE-IDS] [--mongodb-params MONGODB-PARAMS | --mongodb-params-mongo-db-object-types MONGODB-PARAMS-MONGO-DB-OBJECT-TYPES --mongodb-params-search-string MONGODB-PARAMS-SEARCH-STRING --mongodb-params-source-ids MONGODB-PARAMS-SOURCE-IDS] [--ms-groups-params MS-GROUPS-PARAMS | --ms-groups-params-mailbox-params MS-GROUPS-PARAMS-MAILBOX-PARAMS --ms-groups-params-o365-params MS-GROUPS-PARAMS-O365-PARAMS --ms-groups-params-site-params MS-GROUPS-PARAMS-SITE-PARAMS] [--ms-teams-params MS-TEAMS-PARAMS | --ms-teams-params-category-types MS-TEAMS-PARAMS-CATEGORY-TYPES --ms-teams-params-channel-names MS-TEAMS-PARAMS-CHANNEL-NAMES --ms-teams-params-channel-params MS-TEAMS-PARAMS-CHANNEL-PARAMS --ms-teams-params-creation-end-time-secs MS-TEAMS-PARAMS-CREATION-END-TIME-SECS --ms-teams-params-creation-start-time-secs MS-TEAMS-PARAMS-CREATION-START-TIME-SECS --ms-teams-params-o365-params MS-TEAMS-PARAMS-O365-PARAMS --ms-teams-params-owner-names MS-TEAMS-PARAMS-OWNER-NAMES --ms-teams-params-search-string MS-TEAMS-PARAMS-SEARCH-STRING --ms-teams-params-size-bytes-lower-limit MS-TEAMS-PARAMS-SIZE-BYTES-LOWER-LIMIT --ms-teams-params-size-bytes-upper-limit MS-TEAMS-PARAMS-SIZE-BYTES-UPPER-LIMIT --ms-teams-params-types MS-TEAMS-PARAMS-TYPES] [--one-drive-params ONE-DRIVE-PARAMS | --one-drive-params-category-types ONE-DRIVE-PARAMS-CATEGORY-TYPES --one-drive-params-creation-end-time-secs ONE-DRIVE-PARAMS-CREATION-END-TIME-SECS --one-drive-params-creation-start-time-secs ONE-DRIVE-PARAMS-CREATION-START-TIME-SECS --one-drive-params-include-files=ONE-DRIVE-PARAMS-INCLUDE-FILES --one-drive-params-include-folders=ONE-DRIVE-PARAMS-INCLUDE-FOLDERS --one-drive-params-o365-params ONE-DRIVE-PARAMS-O365-PARAMS --one-drive-params-owner-names ONE-DRIVE-PARAMS-OWNER-NAMES --one-drive-params-search-string ONE-DRIVE-PARAMS-SEARCH-STRING --one-drive-params-size-bytes-lower-limit ONE-DRIVE-PARAMS-SIZE-BYTES-LOWER-LIMIT --one-drive-params-size-bytes-upper-limit ONE-DRIVE-PARAMS-SIZE-BYTES-UPPER-LIMIT] [--public-folder-params PUBLIC-FOLDER-PARAMS | --public-folder-params-search-string PUBLIC-FOLDER-PARAMS-SEARCH-STRING --public-folder-params-types PUBLIC-FOLDER-PARAMS-TYPES --public-folder-params-has-attachment=PUBLIC-FOLDER-PARAMS-HAS-ATTACHMENT --public-folder-params-sender-address PUBLIC-FOLDER-PARAMS-SENDER-ADDRESS --public-folder-params-recipient-addresses PUBLIC-FOLDER-PARAMS-RECIPIENT-ADDRESSES --public-folder-params-cc-recipient-addresses PUBLIC-FOLDER-PARAMS-CC-RECIPIENT-ADDRESSES --public-folder-params-bcc-recipient-addresses PUBLIC-FOLDER-PARAMS-BCC-RECIPIENT-ADDRESSES --public-folder-params-received-start-time-secs PUBLIC-FOLDER-PARAMS-RECEIVED-START-TIME-SECS --public-folder-params-received-end-time-secs PUBLIC-FOLDER-PARAMS-RECEIVED-END-TIME-SECS] [--sfdc-params SFDC-PARAMS | --sfdc-params-mutation-types SFDC-PARAMS-MUTATION-TYPES --sfdc-params-object-name SFDC-PARAMS-OBJECT-NAME --sfdc-params-query-string SFDC-PARAMS-QUERY-STRING --sfdc-params-snapshot-id SFDC-PARAMS-SNAPSHOT-ID] [--sharepoint-params SHAREPOINT-PARAMS | --sharepoint-params-category-types SHAREPOINT-PARAMS-CATEGORY-TYPES --sharepoint-params-creation-end-time-secs SHAREPOINT-PARAMS-CREATION-END-TIME-SECS --sharepoint-params-creation-start-time-secs SHAREPOINT-PARAMS-CREATION-START-TIME-SECS --sharepoint-params-include-files=SHAREPOINT-PARAMS-INCLUDE-FILES --sharepoint-params-include-folders=SHAREPOINT-PARAMS-INCLUDE-FOLDERS --sharepoint-params-o365-params SHAREPOINT-PARAMS-O365-PARAMS --sharepoint-params-owner-names SHAREPOINT-PARAMS-OWNER-NAMES --sharepoint-params-search-string SHAREPOINT-PARAMS-SEARCH-STRING --sharepoint-params-size-bytes-lower-limit SHAREPOINT-PARAMS-SIZE-BYTES-LOWER-LIMIT --sharepoint-params-size-bytes-upper-limit SHAREPOINT-PARAMS-SIZE-BYTES-UPPER-LIMIT] [--uda-params UDA-PARAMS | --uda-params-search-string UDA-PARAMS-SEARCH-STRING --uda-params-source-ids UDA-PARAMS-SOURCE-IDS]
```


#### Command options
{: #backup-recovery-indexed-objects-search-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--object-type` (string)
:   Specifies the object type to be searched for. Required.

    Allowable values are: `Emails`, `Files`, `CassandraObjects`, `CouchbaseObjects`, `HbaseObjects`, `HiveObjects`, `MongoObjects`, `HDFSObjects`, `ExchangeObjects`, `PublicFolders`, `GroupsObjects`, `TeamsObjects`, `SharepointObjects`, `OneDriveObjects`, `UdaObjects`, `SfdcRecords`.

`--protection-group-ids` ([]string)
:   Specifies a list of Protection Group ids to filter the indexed objects. If specified, the objects indexed by specified Protection Group ids will be returned.

`--storage-domain-ids` ([]int64)
:   Specifies the Storage Domain ids to filter indexed objects for which Protection Groups are writing data to {{site.data.keyword.baas_full_notm}} Views on the specified Storage Domains.

`--tenant-id` (string)
:   TenantId contains the id of the tenant for which objects are to be returned.

`--include-tenants` (bool)
:   If true, the response includes objects, which belong to all tenants that the current user has permission to see. The default value is false.

    The default value is `false`.

`--tags` ([]string)
:   This field is deprecated. Use --might-have-tag-ids instead.

`--snapshot-tags` ([]string)
:   This field is deprecated. Use --might-have-snapshot-tag-ids instead.

`--must-have-tag-ids` ([]string)
:   Specifies tags that must be all present in the document.

    The list items must match regular expression `/^\\d+:\\d+:[A-Z0-9-]+$/`.

`--might-have-tag-ids` ([]string)
:   Specifies a list of tags, one or more of which might be present in the document. These are OR'ed together and the resulting criteria AND'ed with the rest of the query.

    The list items must match regular expression `/^\\d+:\\d+:[A-Z0-9-]+$/`.

`--must-have-snapshot-tag-ids` ([]string)
:   Specifies snapshot tags, which must be all present in the document.

    The list items must match regular expression `/^\\d+:\\d+:[A-Z0-9-]+$/`.

`--might-have-snapshot-tag-ids` ([]string)
:   Specifies a list of snapshot tags, one or more of which might be present in the document. These are OR'ed together and the resulting criteria AND'ed with the rest of the query.

    The list items must match regular expression `/^\\d+:\\d+:[A-Z0-9-]+$/`.

`--pagination-cookie` (string)
:   Specifies the pagination cookie with which subsequent parts of the response can be fetched.

`--count` (int64)
:   Specifies the number of indexed objects to be fetched for the specified pagination cookie.

`--use-cached-data` (bool)
:   Specifies whether we can serve the GET request from the read replica cache. There is a lag of 15 seconds between the read replica and the primary data source.

`--cassandra-params`
:   Parameters required to search Cassandra on a cluster. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--cassandra-params=@path/to/file.json`.

`--couchbase-params`
:   Parameters required to search CouchBase on a cluster. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--couchbase-params=@path/to/file.json`.

`--email-params`
:   Specifies the request parameters to search for emails and email folders. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--email-params=@path/to/file.json`.

`--exchange-params`
:   Specifies the parameters, which are specific for searching Exchange mailboxes. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--exchange-params=@path/to/file.json`.

`--file-params`
:   Specifies the request parameters to search for files and file folders. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--file-params=@path/to/file.json`.

`--hbase-params`
:   Parameters required to search Hbase on a cluster. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--hbase-params=@path/to/file.json`.

`--hdfs-params`
:   Parameters required to search HDFS on a cluster. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--hdfs-params=@path/to/file.json`.

`--hive-params`
:   Parameters required to search Hive on a cluster. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--hive-params=@path/to/file.json`.

`--mongodb-params`
:   Parameters required to search MongoDB on a cluster. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--mongodb-params=@path/to/file.json`.

`--ms-groups-params`
:   Specifies the request params to search for Groups items. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--ms-groups-params=@path/to/file.json`.

`--ms-teams-params`
:   Specifies the request params to search for Teams items. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--ms-teams-params=@path/to/file.json`.

`--one-drive-params`
:   Specifies the request parameters to search for files/folders in document libraries. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--one-drive-params=@path/to/file.json`.

`--public-folder-params`
:   Specifies the request parameters to search for Public Folder items. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--public-folder-params=@path/to/file.json`.

`--sfdc-params`
:   Specifies the parameters, which are specific for searching Salesforce records. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--sfdc-params=@path/to/file.json`.

`--sharepoint-params`
:   Specifies the request parameters to search for files/folders in document libraries. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--sharepoint-params=@path/to/file.json`.

`--uda-params`
:   Parameters required to search Universal Data Adapter objects. This JSON option can instead be provided by setting individual fields with other options. It is mutually exclusive with those options.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--uda-params=@path/to/file.json`.

`--cassandra-params-cassandra-object-types` ([]string)
:   Specifies one or more Cassandra object types to be searched. This option provides a value for a subfield of the JSON option 'cassandra-params'. It is mutually exclusive with that option.

    Allowable list items are: `CassandraKeyspaces`, `CassandraTables`.

`--cassandra-params-search-string` (string)
:   Specifies the search string to search the Cassandra Objects. This option provides a value for a subfield of the JSON option 'cassandra-params'. It is mutually exclusive with that option.

`--cassandra-params-source-ids` ([]int64)
:   Specifies a list of source ids. Only files that are found in these sources will be returned. This option provides a value for a subfield of the JSON option 'cassandra-params'. It is mutually exclusive with that option.

`--couchbase-params-couchbase-object-types` ([]string)
:   Specifies Couchbase object types be searched. For Couchbase it can only be set to 'CouchbaseBuckets'. This option provides a value for a subfield of the JSON option 'couchbase-params'. It is mutually exclusive with that option.

    Allowable list items are: `CouchbaseBuckets`.

`--couchbase-params-search-string` (string)
:   Specifies the search string to search the Couchbase Objects. This option provides a value for a subfield of the JSON option 'couchbase-params'. It is mutually exclusive with that option.

`--couchbase-params-source-ids` ([]int64)
:   Specifies a list of source ids. Only files that are found in these sources will be returned. This option provides a value for a subfield of the JSON option 'couchbase-params'. It is mutually exclusive with that option.

`--email-params-attendees-addresses` ([]string)
:   Filters the calendar items, which have specified email addresses as attendees. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-bcc-recipient-addresses` ([]string)
:   Filters the emails, which are sent to specified email addresses in BCC. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-cc-recipient-addresses` ([]string)
:   Filters the emails, which are sent to specified email addresses in CC. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-created-end-time-secs` (int64)
:   Specifies the end time in the Unix timestamp epoch in seconds where the created time of the email/item is less than the specified value. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-created-start-time-secs` (int64)
:   Specifies the start time in the Unix timestamp epoch in seconds where the created time of the email/item is more than the specified value. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-due-date-end-time-secs` (int64)
:   Specifies the end time in the Unix timestamp epoch in seconds where the last modification time of the email/item is less than the specified value. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-due-date-start-time-secs` (int64)
:   Specifies the start time in the Unix timestamp epoch in seconds where the last modification time of the email/item is more than the specified value. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-email-address` (string)
:   Filters the contact items, which have specified text in email address. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-email-subject` (string)
:   Filters the emails, which have the specified text in its subject. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-first-name` (string)
:   Filters the contacts with specified text in first name. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-folder-names` ([]string)
:   Filters the emails that are categorized to specified folders. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-has-attachment` (bool)
:   Filters the emails, which have an attachment. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-last-modified-end-time-secs` (int64)
:   Specifies the end time in the Unix timestamp epoch in seconds where the last modification time of the email/item is less than the specified value. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-last-modified-start-time-secs` (int64)
:   Specifies the start time in the Unix timestamp epoch in seconds where the last modification time of the email/item is more than the specified value. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-last-name` (string)
:   Filters the contacts with specified text in last name. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-middle-name` (string)
:   Filters the contacts with specified text in the middle name. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-organizer-address` (string)
:   Filters the calendar items, which are organized by the specified User's email address. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-received-end-time-secs` (int64)
:   Specifies the end time in the Unix timestamp epoch in seconds where the received time of the email is less than the specified value. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-received-start-time-secs` (int64)
:   Specifies the start time in the Unix timestamp epoch in seconds where the received time of the email is more than the specified value. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-recipient-addresses` ([]string)
:   Filters the emails, which are sent to specified email addresses. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-sender-address` (string)
:   Filters the emails, which are received from specified User's email address. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

`--email-params-source-environment` (string)
:   Specifies the source environment. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

    Allowable values are: `kO365`.

`--email-params-task-status-types` ([]string)
:   Specifies a list of task item status types. Task items having status within the given types will be returned. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

    Allowable list items are: `NotStarted`, `InProgress`, `Completed`, `WaitingOnOthers`, `Deferred`.

`--email-params-types` ([]string)
:   Specifies a list of mailbox item types. Only items within the given types will be returned. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

    Allowable list items are: `Email`, `Folder`, `Calendar`, `Contact`, `Task`, `Note`.

`--email-params-o365-params`
:   Specifies email search request params specific to the O365 environment. This option provides a value for a subfield of the JSON option 'email-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--email-params-o365-params=@path/to/file.json`.

`--exchange-params-search-string` (string)
:   Specifies the search string to search the Exchange Objects. This option provides a value for a subfield of the JSON option 'exchange-params'. It is mutually exclusive with that option.

`--file-params-search-string` (string)
:   Specifies the search string to filter the files. User can specify a wildcard character '*' as a suffix to a string where all files name are matched with the prefix string. This option provides a value for a subfield of the JSON option 'file-params'. It is mutually exclusive with that option.

`--file-params-types` ([]string)
:   Specifies a list of file types. Only files within the given types will be returned. This option provides a value for a subfield of the JSON option 'file-params'. It is mutually exclusive with that option.

    Allowable list items are: `File`, `Directory`, `Symlink`.

`--file-params-source-environments` ([]string)
:   Specifies a list of the source environments. Only files from these types of source will be returned. This option provides a value for a subfield of the JSON option 'file-params'. It is mutually exclusive with that option.

    Allowable list items are: `kVMware`, `kHyperV`, `kSQL`, `kView`, `kRemoteAdapter`, `kPhysical`, `kPhysicalFiles`, `kPure`, `kIbmFlashSystem`, `kNetapp`, `kGenericNas`, `kAcropolis`, `kIsilon`, `kGPFS`, `kKVM`, `kExchange`, `kOracle`, `kFlashBlade`, `kO365`, `kHyperFlex`, `kKubernetes`, `kElastifile`, `kSAPHANA`, `kUDA`, `kSfdc`.

`--file-params-source-ids` ([]int64)
:   Specifies a list of source ids. Only files that are found in these sources will be returned. This option provides a value for a subfield of the JSON option 'file-params'. It is mutually exclusive with that option.

`--file-params-object-ids` ([]int64)
:   Specifies a list of object ids. Only files that are found in these objects will be returned. This option provides a value for a subfield of the JSON option 'file-params'. It is mutually exclusive with that option.

`--hbase-params-hbase-object-types` ([]string)
:   Specifies one or more Hbase object types be searched. This option provides a value for a subfield of the JSON option 'hbase-params'. It is mutually exclusive with that option.

    Allowable list items are: `HbaseNamespaces`, `HbaseTables`.

`--hbase-params-search-string` (string)
:   Specifies the search string to search the Hbase Objects. This option provides a value for a subfield of the JSON option 'hbase-params'. It is mutually exclusive with that option.

`--hbase-params-source-ids` ([]int64)
:   Specifies a list of source ids. Only files that are found in these sources will be returned. This option provides a value for a subfield of the JSON option 'hbase-params'. It is mutually exclusive with that option.

`--hdfs-params-hdfs-types` ([]string)
:   Specifies types such as Folders or Files or both to be searched. This option provides a value for a subfield of the JSON option 'hdfs-params'. It is mutually exclusive with that option.

    Allowable list items are: `HDFSFolders`, `HDFSFiles`.

`--hdfs-params-search-string` (string)
:   Specifies the search string to search the HDFS Folders and Files. This option provides a value for a subfield of the JSON option 'hdfs-params'. It is mutually exclusive with that option.

`--hdfs-params-source-ids` ([]int64)
:   Specifies a list of source ids. Only files that are found in these sources will be returned. This option provides a value for a subfield of the JSON option 'hdfs-params'. It is mutually exclusive with that option.

`--hive-params-hive-object-types` ([]string)
:   Specifies one or more Hive object types be searched. This option provides a value for a subfield of the JSON option 'hive-params'. It is mutually exclusive with that option.

    Allowable list items are: `HiveDatabases`, `HiveTables`, `HivePartitions`.

`--hive-params-search-string` (string)
:   Specifies the search string to search the Hive Objects. This option provides a value for a subfield of the JSON option 'hive-params'. It is mutually exclusive with that option.

`--hive-params-source-ids` ([]int64)
:   Specifies a list of source ids. Only files that are found in these sources will be returned. This option provides a value for a subfield of the JSON option 'hive-params'. It is mutually exclusive with that option.

`--mongodb-params-mongo-db-object-types` ([]string)
:   Specifies one or more MongoDB object types be searched. This option provides a value for a subfield of the JSON option 'mongodb-params'. It is mutually exclusive with that option.

    Allowable list items are: `MongoDatabases`, `MongoCollections`.

`--mongodb-params-search-string` (string)
:   Specifies the search string to search the MongoDB Objects. This option provides a value for a subfield of the JSON option 'mongodb-params'. It is mutually exclusive with that option.

`--mongodb-params-source-ids` ([]int64)
:   Specifies a list of source ids. Only files that are found in these sources will be returned. This option provides a value for a subfield of the JSON option 'mongodb-params'. It is mutually exclusive with that option.

`--ms-groups-params-mailbox-params`
:   Specifies the request parameters to search for mailbox items and folders. This option provides a value for a subfield of the JSON option 'ms-groups-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--ms-groups-params-mailbox-params=@path/to/file.json`.

`--ms-groups-params-o365-params`
:   Specifies O365 specific params search request params to search for indexed items. This option provides a value for a subfield of the JSON option 'ms-groups-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--ms-groups-params-o365-params=@path/to/file.json`.

`--ms-groups-params-site-params`
:   Specifies the request parameters to search for files/folders in document libraries. This option provides a value for a subfield of the JSON option 'ms-groups-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--ms-groups-params-site-params=@path/to/file.json`.

`--ms-teams-params-category-types` ([]string)
:   Specifies a list of teams files types. Only items within the given types will be returned. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

    Allowable list items are: `Document`, `Excel`, `Powerpoint`, `Image`, `OneNote`.

`--ms-teams-params-channel-names` ([]string)
:   Specifies the list of channel names to filter while doing a search for files. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

`--ms-teams-params-channel-params`
:   Specifies the request parameters that are related to channels for Microsoft365 teams. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--ms-teams-params-channel-params=@path/to/file.json`.

`--ms-teams-params-creation-end-time-secs` (int64)
:   Specifies the end time in the Unix timestamp epoch in seconds when the item is created. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

`--ms-teams-params-creation-start-time-secs` (int64)
:   Specifies the start time in the Unix timestamp epoch in seconds when the item is created. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

`--ms-teams-params-o365-params`
:   Specifies O365 specific params search request params to search for indexed items. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--ms-teams-params-o365-params=@path/to/file.json`.

`--ms-teams-params-owner-names` ([]string)
:   Specifies the list of owner email ids to filter on owner of the item. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

`--ms-teams-params-search-string` (string)
:   Specifies the search string to filter the items. User can specify a wildcard character '*' as a suffix to a string where all item names are matched with the prefix string. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

`--ms-teams-params-size-bytes-lower-limit` (int64)
:   Specifies the minimum size of the item in bytes. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

`--ms-teams-params-size-bytes-upper-limit` (int64)
:   Specifies the maximum size of the item in bytes. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

`--ms-teams-params-types` ([]string)
:   Specifies a list of Teams item types. Only items within the given types will be returned. This option provides a value for a subfield of the JSON option 'ms-teams-params'. It is mutually exclusive with that option.

    Allowable list items are: `Channel`, `Chat`, `Conversation`, `File`, `Folder`.

`--one-drive-params-category-types` ([]string)
:   Specifies a list of document library types. Only items within the given types will be returned. This option provides a value for a subfield of the JSON option 'one-drive-params'. It is mutually exclusive with that option.

    Allowable list items are: `Document`, `Excel`, `Powerpoint`, `Image`, `OneNote`.

`--one-drive-params-creation-end-time-secs` (int64)
:   Specifies the end time in the Unix timestamp epoch in seconds when the file/folder is created. This option provides a value for a subfield of the JSON option 'one-drive-params'. It is mutually exclusive with that option.

`--one-drive-params-creation-start-time-secs` (int64)
:   Specifies the start time in the Unix timestamp epoch in seconds when the file/folder is created. This option provides a value for a subfield of the JSON option 'one-drive-params'. It is mutually exclusive with that option.

`--one-drive-params-include-files` (bool)
:   Specifies whether to include files in the response. Default is true. This option provides a value for a subfield of the JSON option 'one-drive-params'. It is mutually exclusive with that option.

    The default value is `true`.

`--one-drive-params-include-folders` (bool)
:   Specifies whether to include folders in the response. Default is true. This option provides a value for a subfield of the JSON option 'one-drive-params'. It is mutually exclusive with that option.

    The default value is `true`.

`--one-drive-params-o365-params`
:   Specifies O365 specific params search request params to search for indexed items. This option provides a value for a subfield of the JSON option 'one-drive-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--one-drive-params-o365-params=@path/to/file.json`.

`--one-drive-params-owner-names` ([]string)
:   Specifies the list of owner names to filter on owner of the file/folder. This option provides a value for a subfield of the JSON option 'one-drive-params'. It is mutually exclusive with that option.

`--one-drive-params-search-string` (string)
:   Specifies the search string to filter the files/folders. User can specify a wildcard character '*' as a suffix to a string where all item names are matched with the prefix string. This option provides a value for a subfield of the JSON option 'one-drive-params'. It is mutually exclusive with that option.

`--one-drive-params-size-bytes-lower-limit` (int64)
:   Specifies the minimum size of the file in bytes. This option provides a value for a subfield of the JSON option 'one-drive-params'. It is mutually exclusive with that option.

`--one-drive-params-size-bytes-upper-limit` (int64)
:   Specifies the maximum size of the file in bytes. This option provides a value for a subfield of the JSON option 'one-drive-params'. It is mutually exclusive with that option.

`--public-folder-params-search-string` (string)
:   Specifies the search string to filter the items. User can specify a wildcard character '*' as a suffix to a string where all item names are matched with the prefix string. This option provides a value for a subfield of the JSON option 'public-folder-params'. It is mutually exclusive with that option.

`--public-folder-params-types` ([]string)
:   Specifies a list of public folder item types. Only items within the given types will be returned. This option provides a value for a subfield of the JSON option 'public-folder-params'. It is mutually exclusive with that option.

    Allowable list items are: `Calendar`, `Contact`, `Post`, `Folder`, `Task`, `Journal`, `Note`.

`--public-folder-params-has-attachment` (bool)
:   Filters the public folder items, which have an attachment. This option provides a value for a subfield of the JSON option 'public-folder-params'. It is mutually exclusive with that option.

`--public-folder-params-sender-address` (string)
:   Filters the public folder items, which are received from specified user's email address. This option provides a value for a subfield of the JSON option 'public-folder-params'. It is mutually exclusive with that option.

    The value must match regular expression `/^\\S+@\\S+.\\S+$/`.

`--public-folder-params-recipient-addresses` ([]string)
:   Filters the public folder items, which are sent to specified email addresses. This option provides a value for a subfield of the JSON option 'public-folder-params'. It is mutually exclusive with that option.

    The list items must match regular expression `/^\\S+@\\S+.\\S+$/`.

`--public-folder-params-cc-recipient-addresses` ([]string)
:   Filters the public folder items, which are sent to specified email addresses in CC. This option provides a value for a subfield of the JSON option 'public-folder-params'. It is mutually exclusive with that option.

    The list items must match regular expression `/^\\S+@\\S+.\\S+$/`.

`--public-folder-params-bcc-recipient-addresses` ([]string)
:   Filters the public folder items, which are sent to specified email addresses in BCC. This option provides a value for a subfield of the JSON option 'public-folder-params'. It is mutually exclusive with that option.

    The list items must match regular expression `/^\\S+@\\S+.\\S+$/`.

`--public-folder-params-received-start-time-secs` (int64)
:   Specifies the start time in Unix timestamp epoch in seconds where the received time of the public folder item is more than the specified value. This option provides a value for a subfield of the JSON option 'public-folder-params'. It is mutually exclusive with that option.

`--public-folder-params-received-end-time-secs` (int64)
:   Specifies the end time in the Unix timestamp epoch in seconds where the received time of the public folder items is less than the specified value. This option provides a value for a subfield of the JSON option 'public-folder-params'. It is mutually exclusive with that option.

`--sfdc-params-mutation-types` ([]string)
:   Specifies a list of mutations types for an object. This option provides a value for a subfield of the JSON option 'sfdc-params'. It is mutually exclusive with that option.

    Allowable list items are: `All`, `Added`, `Removed`, `Changed`.

`--sfdc-params-object-name` (string)
:   Specifies the name of the object. This option provides a value for a subfield of the JSON option 'sfdc-params'. It is mutually exclusive with that option.

`--sfdc-params-query-string` (string)
:   Specifies the query string to search records. The query string can be one or multiple clauses joined by 'AND' or 'OR' claused. This option provides a value for a subfield of the JSON option 'sfdc-params'. It is mutually exclusive with that option.

`--sfdc-params-snapshot-id` (string)
:   Specifies the id of the snapshot for the object. This option provides a value for a subfield of the JSON option 'sfdc-params'. It is mutually exclusive with that option.

`--sharepoint-params-category-types` ([]string)
:   Specifies a list of document library types. Only items within the given types will be returned. This option provides a value for a subfield of the JSON option 'sharepoint-params'. It is mutually exclusive with that option.

    Allowable list items are: `Document`, `Excel`, `Powerpoint`, `Image`, `OneNote`.

`--sharepoint-params-creation-end-time-secs` (int64)
:   Specifies the end time in the Unix timestamp epoch in seconds when the file/folder is created. This option provides a value for a subfield of the JSON option 'sharepoint-params'. It is mutually exclusive with that option.

`--sharepoint-params-creation-start-time-secs` (int64)
:   Specifies the start time in the Unix timestamp epoch in seconds when the file/folder is created. This option provides a value for a subfield of the JSON option 'sharepoint-params'. It is mutually exclusive with that option.

`--sharepoint-params-include-files` (bool)
:   Specifies whether to include files in the response. Default is true. This option provides a value for a subfield of the JSON option 'sharepoint-params'. It is mutually exclusive with that option.

    The default value is `true`.

`--sharepoint-params-include-folders` (bool)
:   Specifies whether to include folders in the response. Default is true. This option provides a value for a subfield of the JSON option 'sharepoint-params'. It is mutually exclusive with that option.

    The default value is `true`.

`--sharepoint-params-o365-params`
:   Specifies O365 specific params search request params to search for indexed items. This option provides a value for a subfield of the JSON option 'sharepoint-params'. It is mutually exclusive with that option.

    Provide a JSON string option or specify a JSON file to read from by providing a filepath option that begins with a `@`, for example `--sharepoint-params-o365-params=@path/to/file.json`.

`--sharepoint-params-owner-names` ([]string)
:   Specifies the list of owner names to filter on owner of the file/folder. This option provides a value for a subfield of the JSON option 'sharepoint-params'. It is mutually exclusive with that option.

`--sharepoint-params-search-string` (string)
:   Specifies the search string to filter the files/folders. User can specify a wildcard character '*' as a suffix to a string where all item names are matched with the prefix string. This option provides a value for a subfield of the JSON option 'sharepoint-params'. It is mutually exclusive with that option.

`--sharepoint-params-size-bytes-lower-limit` (int64)
:   Specifies the minimum size of the file in bytes. This option provides a value for a subfield of the JSON option 'sharepoint-params'. It is mutually exclusive with that option.

`--sharepoint-params-size-bytes-upper-limit` (int64)
:   Specifies the maximum size of the file in bytes. This option provides a value for a subfield of the JSON option 'sharepoint-params'. It is mutually exclusive with that option.

`--uda-params-search-string` (string)
:   Specifies the search string to search the Universal Data Adapter Objects. This option provides a value for a subfield of the JSON option 'uda-params'. It is mutually exclusive with that option.

`--uda-params-source-ids` ([]int64)
:   Specifies a list of source ids. Only files that are found in these sources will be returned. This option provides a value for a subfield of the JSON option 'uda-params'. It is mutually exclusive with that option.

#### Examples
{: #backup-recovery-indexed-objects-search-examples}

```sh
ibmcloud backup-recovery indexed-objects-search \
    --xibm-tenant-id tenantId \
    --object-type Emails \
    --protection-group-ids protectionGroupId1 \
    --storage-domain-ids 26,27 \
    --tenant-id tenantId \
    --include-tenants=false \
    --tags 123:456:ABC-123,123:456:ABC-456 \
    --snapshot-tags 123:456:DEF-123,123:456:DEF-456 \
    --must-have-tag-ids 123:456:ABC-123 \
    --might-have-tag-ids 123:456:ABC-456 \
    --must-have-snapshot-tag-ids 123:456:DEF-123 \
    --might-have-snapshot-tag-ids 123:456:DEF-456 \
    --pagination-cookie paginationCookie \
    --count 38 \
    --use-cached-data=true \
    --cassandra-params '{"cassandraObjectTypes": ["CassandraKeyspaces","CassandraTables"], "searchString": "searchString", "sourceIds": [26,27]}' \
    --couchbase-params '{"couchbaseObjectTypes": ["CouchbaseBuckets"], "searchString": "searchString", "sourceIds": [26,27]}' \
    --email-params '{"attendeesAddresses": ["attendee1@domain.com"], "bccRecipientAddresses": ["bccrecipient@domain.com"], "ccRecipientAddresses": ["ccrecipient@domain.com"], "createdEndTimeSecs": 26, "createdStartTimeSecs": 26, "dueDateEndTimeSecs": 26, "dueDateStartTimeSecs": 26, "emailAddress": "email@domain.com", "emailSubject": "Email Subject", "firstName": "First Name", "folderNames": ["folder1"], "hasAttachment": true, "lastModifiedEndTimeSecs": 26, "lastModifiedStartTimeSecs": 26, "lastName": "Last Name", "middleName": "Middle Name", "organizerAddress": "organizer@domain.com", "receivedEndTimeSecs": 26, "receivedStartTimeSecs": 26, "recipientAddresses": ["recipient@domain.com"], "senderAddress": "sender@domain.com", "sourceEnvironment": "kO365", "taskStatusTypes": ["NotStarted","InProgress","Completed","WaitingOnOthers","Deferred"], "types": ["Email","Folder","Calendar","Contact","Task","Note"], "o365Params": {"domainIds": [26,27], "mailboxIds": [26,27]}}' \
    --exchange-params '{"searchString": "searchString"}' \
    --file-params '{"searchString": "searchString", "types": ["File","Directory","Symlink"], "sourceEnvironments": ["kVMware","kHyperV","kSQL","kView","kRemoteAdapter","kPhysical","kPhysicalFiles","kPure","kIbmFlashSystem","kNetapp","kGenericNas","kAcropolis","kIsilon","kGPFS","kKVM","kExchange","kOracle","kFlashBlade","kO365","kHyperFlex","kKubernetes","kElastifile","kSAPHANA","kUDA","kSfdc"], "sourceIds": [26,27], "objectIds": [26,27]}' \
    --hbase-params '{"hbaseObjectTypes": ["HbaseNamespaces","HbaseTables"], "searchString": "searchString", "sourceIds": [26,27]}' \
    --hdfs-params '{"hdfsTypes": ["HDFSFolders","HDFSFiles"], "searchString": "searchString", "sourceIds": [26,27]}' \
    --hive-params '{"hiveObjectTypes": ["HiveDatabases","HiveTables","HivePartitions"], "searchString": "searchString", "sourceIds": [26,27]}' \
    --mongodb-params '{"mongoDBObjectTypes": ["MongoDatabases","MongoCollections"], "searchString": "searchString", "sourceIds": [26,27]}' \
    --ms-groups-params '{"mailboxParams": {"attendeesAddresses": ["attendee1@domain.com"], "bccRecipientAddresses": ["bccrecipient@domain.com"], "ccRecipientAddresses": ["ccrecipient@domain.com"], "createdEndTimeSecs": 26, "createdStartTimeSecs": 26, "dueDateEndTimeSecs": 26, "dueDateStartTimeSecs": 26, "emailAddress": "email@domain.com", "emailSubject": "Email Subject", "firstName": "First Name", "folderNames": ["folder1"], "hasAttachment": true, "lastModifiedEndTimeSecs": 26, "lastModifiedStartTimeSecs": 26, "lastName": "Last Name", "middleName": "Middle Name", "organizerAddress": "organizer@domain.com", "receivedEndTimeSecs": 26, "receivedStartTimeSecs": 26, "recipientAddresses": ["recipient@domain.com"], "senderAddress": "sender@domain.com", "sourceEnvironment": "kO365", "taskStatusTypes": ["NotStarted","InProgress","Completed","WaitingOnOthers","Deferred"], "types": ["Email","Folder","Calendar","Contact","Task","Note"]}, "o365Params": {"domainIds": [26,27], "groupIds": [26,27], "siteIds": [26,27], "teamsIds": [26,27], "userIds": [26,27]}, "siteParams": {"categoryTypes": ["Document","Excel","Powerpoint","Image","OneNote"], "creationEndTimeSecs": 26, "creationStartTimeSecs": 26, "includeFiles": true, "includeFolders": true, "o365Params": {"domainIds": [26,27], "groupIds": [26,27], "siteIds": [26,27], "teamsIds": [26,27], "userIds": [26,27]}, "ownerNames": ["ownerName1"], "searchString": "searchString", "sizeBytesLowerLimit": 26, "sizeBytesUpperLimit": 26}}' \
    --ms-teams-params '{"categoryTypes": ["Document","Excel","Powerpoint","Image","OneNote"], "channelNames": ["channelName1"], "channelParams": {"channelEmail": "channel@domain.com", "channelId": "channelId", "channelName": "channelName", "includePrivateChannels": true, "includePublicChannels": true}, "creationEndTimeSecs": 26, "creationStartTimeSecs": 26, "o365Params": {"domainIds": [26,27], "groupIds": [26,27], "siteIds": [26,27], "teamsIds": [26,27], "userIds": [26,27]}, "ownerNames": ["ownerName1"], "searchString": "searchString", "sizeBytesLowerLimit": 26, "sizeBytesUpperLimit": 26, "types": ["Channel","Chat","Conversation","File","Folder"]}' \
    --one-drive-params '{"categoryTypes": ["Document","Excel","Powerpoint","Image","OneNote"], "creationEndTimeSecs": 26, "creationStartTimeSecs": 26, "includeFiles": true, "includeFolders": true, "o365Params": {"domainIds": [26,27], "groupIds": [26,27], "siteIds": [26,27], "teamsIds": [26,27], "userIds": [26,27]}, "ownerNames": ["ownerName1"], "searchString": "searchString", "sizeBytesLowerLimit": 26, "sizeBytesUpperLimit": 26}' \
    --public-folder-params '{"searchString": "searchString", "types": ["Calendar","Contact","Post","Folder","Task","Journal","Note"], "hasAttachment": true, "senderAddress": "sender@domain.com", "recipientAddresses": ["recipient@domain.com"], "ccRecipientAddresses": ["ccrecipient@domain.com"], "bccRecipientAddresses": ["bccrecipient@domain.com"], "receivedStartTimeSecs": 26, "receivedEndTimeSecs": 26}' \
    --sfdc-params '{"mutationTypes": ["All","Added","Removed","Changed"], "objectName": "objectName", "queryString": "queryString", "snapshotId": "snapshotId"}' \
    --sharepoint-params '{"categoryTypes": ["Document","Excel","Powerpoint","Image","OneNote"], "creationEndTimeSecs": 26, "creationStartTimeSecs": 26, "includeFiles": true, "includeFolders": true, "o365Params": {"domainIds": [26,27], "groupIds": [26,27], "siteIds": [26,27], "teamsIds": [26,27], "userIds": [26,27]}, "ownerNames": ["ownerName1"], "searchString": "searchString", "sizeBytesLowerLimit": 26, "sizeBytesUpperLimit": 26}' \
    --uda-params '{"searchString": "searchString", "sourceIds": [26,27]}'
```
{: pre}

Alternatively, granular options are available for the subfields of JSON string options:
```sh
ibmcloud backup-recovery indexed-objects-search \
    --xibm-tenant-id tenantId \
    --object-type Emails \
    --protection-group-ids protectionGroupId1 \
    --storage-domain-ids 26,27 \
    --tenant-id tenantId \
    --include-tenants=false \
    --tags 123:456:ABC-123,123:456:ABC-456 \
    --snapshot-tags 123:456:DEF-123,123:456:DEF-456 \
    --must-have-tag-ids 123:456:ABC-123 \
    --might-have-tag-ids 123:456:ABC-456 \
    --must-have-snapshot-tag-ids 123:456:DEF-123 \
    --might-have-snapshot-tag-ids 123:456:DEF-456 \
    --pagination-cookie paginationCookie \
    --count 38 \
    --use-cached-data=true \
    --cassandra-params-cassandra-object-types CassandraKeyspaces,CassandraTables \
    --cassandra-params-search-string exampleString \
    --cassandra-params-source-ids 26,27 \
    --couchbase-params-couchbase-object-types CouchbaseBuckets \
    --couchbase-params-search-string exampleString \
    --couchbase-params-source-ids 26,27 \
    --email-params-attendees-addresses exampleString,anotherTestString \
    --email-params-bcc-recipient-addresses exampleString,anotherTestString \
    --email-params-cc-recipient-addresses exampleString,anotherTestString \
    --email-params-created-end-time-secs 26 \
    --email-params-created-start-time-secs 26 \
    --email-params-due-date-end-time-secs 26 \
    --email-params-due-date-start-time-secs 26 \
    --email-params-email-address exampleString \
    --email-params-email-subject exampleString \
    --email-params-first-name exampleString \
    --email-params-folder-names exampleString,anotherTestString \
    --email-params-has-attachment=true \
    --email-params-last-modified-end-time-secs 26 \
    --email-params-last-modified-start-time-secs 26 \
    --email-params-last-name exampleString \
    --email-params-middle-name exampleString \
    --email-params-organizer-address exampleString \
    --email-params-received-end-time-secs 26 \
    --email-params-received-start-time-secs 26 \
    --email-params-recipient-addresses exampleString,anotherTestString \
    --email-params-sender-address exampleString \
    --email-params-source-environment kO365 \
    --email-params-task-status-types NotStarted,InProgress,Completed,WaitingOnOthers,Deferred \
    --email-params-types Email,Folder,Calendar,Contact,Task,Note \
    --email-params-o365-params o365SearchEmailsRequestParams \
    --exchange-params-search-string exampleString \
    --file-params-search-string exampleString \
    --file-params-types File,Directory,Symlink \
    --file-params-source-environments kVMware,kHyperV,kSQL,kView,kRemoteAdapter,kPhysical,kPhysicalFiles,kPure,kIbmFlashSystem,kNetapp,kGenericNas,kAcropolis,kIsilon,kGPFS,kKVM,kExchange,kOracle,kFlashBlade,kO365,kHyperFlex,kKubernetes,kElastifile,kSAPHANA,kUDA,kSfdc \
    --file-params-source-ids 26,27 \
    --file-params-object-ids 26,27 \
    --hbase-params-hbase-object-types HbaseNamespaces,HbaseTables \
    --hbase-params-search-string exampleString \
    --hbase-params-source-ids 26,27 \
    --hdfs-params-hdfs-types HDFSFolders,HDFSFiles \
    --hdfs-params-search-string exampleString \
    --hdfs-params-source-ids 26,27 \
    --hive-params-hive-object-types HiveDatabases,HiveTables,HivePartitions \
    --hive-params-search-string exampleString \
    --hive-params-source-ids 26,27 \
    --mongodb-params-mongo-db-object-types MongoDatabases,MongoCollections \
    --mongodb-params-search-string exampleString \
    --mongodb-params-source-ids 26,27 \
    --ms-groups-params-mailbox-params searchEmailRequestParamsBase \
    --ms-groups-params-o365-params o365SearchRequestParams \
    --ms-groups-params-site-params searchDocumentLibraryRequestParams \
    --ms-teams-params-category-types Document,Excel,Powerpoint,Image,OneNote \
    --ms-teams-params-channel-names exampleString,anotherTestString \
    --ms-teams-params-channel-params o365TeamsChannelsSearchRequestParams \
    --ms-teams-params-creation-end-time-secs 26 \
    --ms-teams-params-creation-start-time-secs 26 \
    --ms-teams-params-o365-params o365SearchRequestParams \
    --ms-teams-params-owner-names exampleString,anotherTestString \
    --ms-teams-params-search-string exampleString \
    --ms-teams-params-size-bytes-lower-limit 26 \
    --ms-teams-params-size-bytes-upper-limit 26 \
    --ms-teams-params-types Channel,Chat,Conversation,File,Folder \
    --one-drive-params-category-types Document,Excel,Powerpoint,Image,OneNote \
    --one-drive-params-creation-end-time-secs 26 \
    --one-drive-params-creation-start-time-secs 26 \
    --one-drive-params-include-files=true \
    --one-drive-params-include-folders=true \
    --one-drive-params-o365-params o365SearchRequestParams \
    --one-drive-params-owner-names exampleString,anotherTestString \
    --one-drive-params-search-string exampleString \
    --one-drive-params-size-bytes-lower-limit 26 \
    --one-drive-params-size-bytes-upper-limit 26 \
    --public-folder-params-search-string exampleString \
    --public-folder-params-types Calendar,Contact,Post,Folder,Task,Journal,Note \
    --public-folder-params-has-attachment=true \
    --public-folder-params-sender-address exampleString \
    --public-folder-params-recipient-addresses exampleString,anotherTestString \
    --public-folder-params-cc-recipient-addresses exampleString,anotherTestString \
    --public-folder-params-bcc-recipient-addresses exampleString,anotherTestString \
    --public-folder-params-received-start-time-secs 26 \
    --public-folder-params-received-end-time-secs 26 \
    --sfdc-params-mutation-types All,Added,Removed,Changed \
    --sfdc-params-object-name exampleString \
    --sfdc-params-query-string exampleString \
    --sfdc-params-snapshot-id exampleString \
    --sharepoint-params-category-types Document,Excel,Powerpoint,Image,OneNote \
    --sharepoint-params-creation-end-time-secs 26 \
    --sharepoint-params-creation-start-time-secs 26 \
    --sharepoint-params-include-files=true \
    --sharepoint-params-include-folders=true \
    --sharepoint-params-o365-params o365SearchRequestParams \
    --sharepoint-params-owner-names exampleString,anotherTestString \
    --sharepoint-params-search-string exampleString \
    --sharepoint-params-size-bytes-lower-limit 26 \
    --sharepoint-params-size-bytes-upper-limit 26 \
    --uda-params-search-string exampleString \
    --uda-params-source-ids 26,27
```
{: pre}

### `ibmcloud backup-recovery objects-search`
{: #backup-recovery-cli-objects-search-command}

List objects.

```sh
ibmcloud backup-recovery objects-search --xibm-tenant-id XIBM-TENANT-ID [--request-initiator-type REQUEST-INITIATOR-TYPE] [--search-string SEARCH-STRING] [--environments ENVIRONMENTS] [--protection-types PROTECTION-TYPES] [--protection-group-ids PROTECTION-GROUP-IDS] [--object-ids OBJECT-IDS] [--os-types OS-TYPES] [--source-ids SOURCE-IDS] [--source-uuids SOURCE-UUIDS] [--is-protected=IS-PROTECTED] [--is-deleted=IS-DELETED] [--last-run-status-list LAST-RUN-STATUS-LIST] [--cluster-identifiers CLUSTER-IDENTIFIERS] [--include-deleted-objects=INCLUDE-DELETED-OBJECTS] [--pagination-cookie PAGINATION-COOKIE] [--count COUNT] [--must-have-tag-ids MUST-HAVE-TAG-IDS] [--might-have-tag-ids MIGHT-HAVE-TAG-IDS] [--must-have-snapshot-tag-ids MUST-HAVE-SNAPSHOT-TAG-IDS] [--might-have-snapshot-tag-ids MIGHT-HAVE-SNAPSHOT-TAG-IDS] [--tag-search-name TAG-SEARCH-NAME] [--tag-names TAG-NAMES] [--tag-types TAG-TYPES] [--tag-categories TAG-CATEGORIES] [--tag-sub-categories TAG-SUB-CATEGORIES] [--include-helios-tag-info-for-objects=INCLUDE-HELIOS-TAG-INFO-FOR-OBJECTS] [--external-filters EXTERNAL-FILTERS]
```


#### Command options
{: #backup-recovery-objects-search-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--request-initiator-type` (string)
:   Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests.

    Allowable values are: `UIUser`, `UIAuto`, `Helios`.

`--search-string` (string)
:   Specifies the search string to filter the objects. This search string will be applicable for objectnames. User can specify a wildcard character '*' as a suffix to a string where all object names are matched with the prefix string. For example, if vm1 and vm2 are the names of objects, the user can specify vm* to list the objects. If not specified, then all the objects are returned which will match other filtering criteria.

`--environments` ([]string)
:   Specifies the environment type to filter objects.

    Allowable list items are: `kPhysical`, `kSQL`.

`--protection-types` ([]string)
:   Specifies the protection type to filter objects.

    Allowable list items are: `kAgent`, `kNative`, `kSnapshotManager`, `kFile`, `kVolume`.

`--protection-group-ids` ([]string)
:   Specifies a list of Protection Group ids to filter the objects. If specified, the objects that are protected by specified Protection Group ids will be returned.

`--object-ids` ([]int64)
:   Specifies a list of Object ids to filter.

`--os-types` ([]string)
:   Specifies the operating system types to filter objects on.

    Allowable list items are: `kLinux`, `kWindows`.

`--source-ids` ([]int64)
:   Specifies a list of Protection Source object ids to filter the objects. If specified, the object which are present in those Sources will be returned.

`--source-uuids` ([]string)
:   Specifies a list of Protection Source object uuids to filter the objects. If specified, the object, which are present in those Sources will be returned.

`--is-protected` (bool)
:   Specifies the protection status of objects. If set to true, only protected objects are returned. If set to false, only unprotected objects are returned. If not specified, all objects are returned.

`--is-deleted` (bool)
:   If set to true, then objects that are deleted on at least one cluster are returned. If not set or set to false then objects that are registered on at least one cluster are returned.

`--last-run-status-list` ([]string)
:   Specifies a list of the status of the object's last protection run. Only objects with last run status of these will be returned.

    Allowable list items are: `Accepted`, `Running`, `Canceled`, `Canceling`, `Failed`, `Missed`, `Succeeded`, `SucceededWithWarning`, `OnHold`, `Finalizing`, `Skipped`, `LegalHold`.

`--cluster-identifiers` ([]string)
:   Specifies the list of cluster identifiers. The format is clusterId:clusterIncarnationId. Only records from clusters having these identifiers will be returned.

`--include-deleted-objects` (bool)
:   Specifies whether to include deleted objects in response. These objects can't be protected but can be recovered. This field is deprecated.

`--pagination-cookie` (string)
:   Specifies the pagination cookie with which subsequent parts of the response can be fetched.

`--count` (int64)
:   Specifies the number of objects to be fetched for the specified pagination cookie.

`--must-have-tag-ids` ([]string)
:   Specifies tags, which must be all present in the document.

    The list items must match regular expression `/^\\d+:\\d+:[A-Z0-9-]+$/`.

`--might-have-tag-ids` ([]string)
:   Specifies a list of tags, one or more of which might be present in the document. These are OR'ed together and the resulting criteria AND'ed with the rest of the query.

    The list items must match regular expression `/^\\d+:\\d+:[A-Z0-9-]+$/`.

`--must-have-snapshot-tag-ids` ([]string)
:   Specifies snapshot tags, which must be all present in the document.

    The list items must match regular expression `/^\\d+:\\d+:[A-Z0-9-]+$/`.

`--might-have-snapshot-tag-ids` ([]string)
:   Specifies a list of snapshot tags, one or more of which might be present in the document. These are OR'ed together and the resulting criteria AND'ed with the rest of the query.

    The list items must match regular expression `/^\\d+:\\d+:[A-Z0-9-]+$/`.

`--tag-search-name` (string)
:   Specifies the tag name to filter the tagged objects and snapshots. User can specify a wildcard character '*' as a suffix to a string where all object's tag names are matched with the prefix string.

`--tag-names` ([]string)
:   Specifies the tag names to filter the tagged objects and snapshots.

`--tag-types` ([]string)
:   Specifies the tag names to filter the tagged objects and snapshots.

    Allowable list items are: `System`, `Custom`, `ThirdParty`.

`--tag-categories` ([]string)
:   Specifies the tag category to filter the objects and snapshots.

    Allowable list items are: `Security`.

`--tag-sub-categories` ([]string)
:   Specifies the tag subcategory to filter the objects and snapshots.

    Allowable list items are: `Classification`, `Threats`, `Anomalies`, `Dspm`.

`--include-helios-tag-info-for-objects` (bool)
:   Specifies whether to include helios tags information for objects in response. The default value is false.

`--external-filters` ([]string)
:   Specifies the key-value pairs to filter the results for the search. Each filter is of the form 'key:value'. The filter 'externalFilters:k1:v1&externalFilters:k2:v2&externalFilters:k2:v3' returns the documents where each document matches the query (k1=v1) AND (k2=v2 OR k2 = v3). Allowed keys: - vmBiosUuid - graphUuid - arn - instanceId - bucketName - azureId.

#### Example
{: #backup-recovery-objects-search-examples}

```sh
ibmcloud backup-recovery objects-search \
    --xibm-tenant-id tenantId \
    --request-initiator-type UIUser \
    --search-string searchString \
    --environments kPhysical,kSQL \
    --protection-types kAgent,kNative,kSnapshotManager,kFile,kVolume \
    --protection-group-ids protectionGroupId1 \
    --object-ids 26,27 \
    --os-types kLinux,kWindows \
    --source-ids 26,27 \
    --source-uuids sourceUuid1 \
    --is-protected=true \
    --is-deleted=true \
    --last-run-status-list Accepted,Running,Canceled,Canceling,Failed,Missed,Succeeded,SucceededWithWarning,OnHold,Finalizing,Skipped,LegalHold \
    --cluster-identifiers clusterIdentifier1 \
    --include-deleted-objects=true \
    --pagination-cookie paginationCookie \
    --count 38 \
    --must-have-tag-ids 123:456:ABC-123 \
    --might-have-tag-ids 123:456:ABC-456 \
    --must-have-snapshot-tag-ids 123:456:DEF-123 \
    --might-have-snapshot-tag-ids 123:456:DEF-456 \
    --tag-search-name tagName \
    --tag-names tag1 \
    --tag-types System,Custom,ThirdParty \
    --tag-categories Security \
    --tag-sub-categories Classification,Threats,Anomalies,Dspm \
    --include-helios-tag-info-for-objects=true \
    --external-filters filter1
```
{: pre}

### `ibmcloud backup-recovery protected-objects-search`
{: #backup-recovery-cli-protected-objects-search-command}

List protected objects and corresponding detail information from registered sources that are filtered by specified query parameters. If no search pattern or filter parameters are specified, all protected objects currently found are returned.

```sh
ibmcloud backup-recovery protected-objects-search --xibm-tenant-id XIBM-TENANT-ID [--request-initiator-type REQUEST-INITIATOR-TYPE] [--search-string SEARCH-STRING] [--environments ENVIRONMENTS] [--snapshot-actions SNAPSHOT-ACTIONS] [--object-action-key OBJECT-ACTION-KEY] [--protection-group-ids PROTECTION-GROUP-IDS] [--object-ids OBJECT-IDS] [--sub-result-size SUB-RESULT-SIZE] [--filter-snapshot-from-usecs FILTER-SNAPSHOT-FROM-USECS] [--filter-snapshot-to-usecs FILTER-SNAPSHOT-TO-USECS] [--os-types OS-TYPES] [--source-ids SOURCE-IDS] [--run-instance-ids RUN-INSTANCE-IDS] [--cdp-protected-only=CDP-PROTECTED-ONLY] [--use-cached-data=USE-CACHED-DATA]
```


#### Command options
{: #backup-recovery-protected-objects-search-cli-options}

`--xibm-tenant-id` (string)
:   Specifies the tenant ID for the backup recovery service instance. Required.

`--request-initiator-type` (string)
:   Specifies the type of request from UI, which is used for services like magneto to determine the priority of requests.

    Allowable values are: `UIUser`, `UIAuto`, `Helios`.

`--search-string` (string)
:   Specifies the search string to filter the objects. This search string will be applicable for object names and Protection Group names. User can specify a wildcard character '*' as a suffix to a string where all object and their Protection Group names are matched with the prefix string. For example, if vm1 and vm2 are the names of objects, the user can specify vm* to list the objects. If not specified, then all the objects with Protection Groups will be returned which will match other filtering criteria.

`--environments` ([]string)
:   Specifies the environment type to filter objects.

    Allowable list items are: `kPhysical`, `kSQL`.

`--snapshot-actions` ([]string)
:   Specifies a list of recovery actions. Only snapshots that apply to these actions will be returned.

    Allowable list items are: `RecoverVMs`, `RecoverFiles`, `InstantVolumeMount`, `RecoverVmDisks`, `MountVolumes`, `RecoverVApps`, `RecoverApps`, `RecoverNasVolume`, `RecoverPhysicalVolumes`, `RecoverSystem`, `RecoverSanVolumes`, `RecoverNamespaces`, `RecoverObjects`, `DownloadFilesAndFolders`, `RecoverPublicFolders`, `RecoverVAppTemplates`, `RecoverMailbox`, `RecoverOneDrive`, `RecoverMsTeam`, `RecoverMsGroup`, `RecoverSharePoint`, `ConvertToPst`, `RecoverSfdcRecords`, `DownloadChats`, `RecoverMailboxCSM`, `RecoverOneDriveCSM`, `RecoverSharePointCSM`.

`--object-action-key` (string)
:   Filter by ObjectActionKey, which uniquely represents the protection of an object. An object can be protected in multiple ways but at most once for a given combination of ObjectActionKey. When specified, the latest snapshot information matching the objectActionKey is for corresponding object.

    Allowable values are: `kPhysical`, `kSQL`.

`--protection-group-ids` ([]string)
:   Specifies a list of Protection Group ids to filter the objects. If specified, the objects that are protected by specified Protection Group ids will be returned.

`--object-ids` ([]int64)
:   Specifies a list of Object ids to filter.

`--sub-result-size` (int64)
:   Specifies the size of objects to be fetched for a single subresult.

`--filter-snapshot-from-usecs` (int64)
:   Specifies the timestamp in the Unix time epoch in microseconds to filter the objects if the Object has a successful snapshot after this value.

`--filter-snapshot-to-usecs` (int64)
:   Specifies the timestamp in the Unix time epoch in microseconds to filter the objects if the Object has a successful snapshot before this value.

`--os-types` ([]string)
:   Specifies the operating system types to filter objects on.

    Allowable list items are: `kLinux`, `kWindows`.

`--source-ids` ([]int64)
:   Specifies a list of Protection Source object ids to filter the objects. If specified, the object that are present in those Sources will be returned.

`--run-instance-ids` ([]int64)
:   Specifies a list of run instance ids. If specified only objects belonging to the provided run id will be returned.

`--cdp-protected-only` (bool)
:   Specifies whether to only return the CDP protected objects.

`--use-cached-data` (bool)
:   Specifies whether we can serve the GET request to the read replica cache. There is a lag of 15 seconds between the read replica and the primary data source.

#### Example
{: #backup-recovery-protected-objects-search-examples}

```sh
ibmcloud backup-recovery protected-objects-search \
    --xibm-tenant-id tenantId \
    --request-initiator-type UIUser \
    --search-string searchString \
    --environments kPhysical,kSQL \
    --snapshot-actions RecoverVMs,RecoverFiles,InstantVolumeMount,RecoverVmDisks,MountVolumes,RecoverVApps,RecoverApps,RecoverNasVolume,RecoverPhysicalVolumes,RecoverSystem,RecoverSanVolumes,RecoverNamespaces,RecoverObjects,DownloadFilesAndFolders,RecoverPublicFolders,RecoverVAppTemplates,RecoverMailbox,RecoverOneDrive,RecoverMsTeam,RecoverMsGroup,RecoverSharePoint,ConvertToPst,RecoverSfdcRecords,DownloadChats,RecoverMailboxCSM,RecoverOneDriveCSM,RecoverSharePointCSM \
    --object-action-key kPhysical \
    --protection-group-ids protectionGroupId1 \
    --object-ids 26,27 \
    --sub-result-size 38 \
    --filter-snapshot-from-usecs 26 \
    --filter-snapshot-to-usecs 26 \
    --os-types kLinux,kWindows \
    --source-ids 26,27 \
    --run-instance-ids 26,27 \
    --cdp-protected-only=true \
    --use-cached-data=true
```
{: pre}

## Provider-instances
{: #backup-recovery-provider-instances-cli}

List all the Provider Instances with details. The API returns the detailed listing of provider instances. The providers are responsible for managing the life cycle of instances and this API only intends to provide metadata information about the instances.

```sh
ibmcloud backup-recovery provider-instances [--instance-ids INSTANCE-IDS] [--regions REGIONS] [--include-service-instance-status=INCLUDE-SERVICE-INSTANCE-STATUS]
```


#### Command options
{: #backup-recovery-provider-instances-cli-options}

`--instance-ids` ([]string)
:   List of Provider Instance IDs to filter on.

`--regions` ([]string)
:   List of regions to filter Provider Instances on.

`--include-service-instance-status` (bool)
:   Bool flag to check whether to include other details like cluster details and software version along with status for the service instances. The default value is false.

#### Example
{: #backup-recovery-provider-instances-examples}

```sh
ibmcloud backup-recovery provider-instances \
    --instance-ids exampleString,anotherTestString \
    --regions exampleString,anotherTestString \
    --include-service-instance-status=false
```
{: pre}

## Ibmcloud backup-recovery management-console
{: #backup-recovery-console-resources-cli}

Commands for Management Console resources.

```sh
ibmcloud backup-recovery management-console --help
```

### `ibmcloud backup-recovery management-console resources-list`
{: #backup-recovery-cli-console-resources-list-command}

Get different kinds of resources available that are discovered on Management Console. These values can be used for filtering options.

```sh
ibmcloud backup-recovery management-console resources-list --resource-type RESOURCE-TYPE
```

#### Command options
{: #backup-recovery-console-resources-list-cli-options}

`--resource-type` (string)
:   Required. Specifies the type of the resource. Allowable values are: Policies, ProtectionGroups, RegisteredSources, MessageCodeMappings.

#### Example
{: #backup-recovery-console-resources-list-examples}

```sh
 ibmcloud backup-recovery management-console resources-list \
    --resource-type Policies
```
{: pre}

### `ibmcloud backup-recovery management-console components-get`
{: #backup-recovery-cli-console-components-get-command}

Get information about a Report component.

```sh
ibmcloud backup-recovery management-console components-get --id ID
```

#### Command options
{: #backup-recovery-console-components-get-cli-options}

`--id` (string)
:   Specifies the id of the report component. Required.

#### Example
{: #backup-recovery-console-components-get-examples}

```sh
ibmcloud backup-recovery management-console components-get \
    --id exampleString
```
{: pre}

### `ibmcloud backup-recovery management-console components-list`
{: #backup-recovery-cli-console-components-list-command}

Fetches a list of all report components accessible by the logged in user.

```sh
ibmcloud backup-recovery management-console components-list [--ids IDS]
```

#### Command options
{: #backup-recovery-console-components-list-cli-options}

`--ids` ([]string)
:   Specifies the ids of the report component to fetch.

#### Example
{: #backup-recovery-console-components-list-examples}

```sh
ibmcloud backup-recovery management-console components-list \
    --ids exampleString,anotherTestString
```
{: pre}

### `ibmcloud backup-recovery management-console components-preview`
{: #backup-recovery-cli-console-components-get-preview-command}

Get preview for a component specified by Id.

```sh
ibmcloud backup-recovery management-console components-preview --id ID [--filters FILTERS | @FILTERS-FILE] [--limit (LIMIT | @LIMIT-FILE) | --limit-from LIMIT-FROM --limit-size LIMIT-SIZE] [--sort SORT | @SORT-FILE] [--timezone TIMEZONE]
```

#### Command options
{: #backup-recovery-console-components-get-preview-cli-options}

`--id` (string)
:   Specifies the id of the component. Required.

`--filters` (string)
:   Specifies list of global filters that are applicable to given components in the report. It should be a JSON string or a path to a JSON file.

`--limit` (string)
:   Specifies the parameters to limit the resulting dataset. It should be a JSON string or a path to a JSON file.

`--limit-from` (int)
:   Specifies the offset to which the resulting data will be skipped before applying the size parameter. For example, if dataset size is 10 objects, from=2 and size=5, then from 10 objects only 5 objects are returned starting from offset 2 i.e., 2 to 7. If not specified, then none of the objects are skipped.

`--limit-size` (int)
:   Specifies the number of objects to be returned from the offset specified. The minimum value is 1.

`--sort` (string)
:   Specifies the sorting (ordering) parameters to be applied to the resulting data. It should be a JSON string or a path to a JSON file.

`--timezone` (string)
:   Specifies the time zone of the user. If nil, defaults to UTC. The time that is specified should be a location name in the IANA time zone database, for example, 'America/Los_Angeles'.

#### Example
{: #backup-recovery-console-components-get-preview-examples}

```sh
ibmcloud backup-recovery management-console components-preview \
    --id exampleString \
    --filters '[{"attribute": "exampleString", "filterType": "In", "inFilterParams": {"attributeDataType": "Bool", "attributeLabels": ["exampleString","anotherTestString"], "boolFilterValues": [true,false], "int32FilterValues": [38,39], "int64FilterValues": [26,27], "stringFilterValues": ["exampleString","anotherTestString"]}, "rangeFilterParams": {"lowerBound": 26, "upperBound": 26}, "systemsFilterParams": {"systemIds": ["exampleString","anotherTestString"], "systemNames": ["exampleString","anotherTestString"]}, "timeRangeFilterParams": {"dateRange": "Last1Hour", "durationHours": 26, "lowerBound": 26, "upperBound": 26}}]' \
    --limit '{"from": 38, "size": 1}' \
    --sort '[{"attribute": "exampleString", "desc": true}]' \
    --timezone exampleString
```
{: pre}

### `ibmcloud backup-recovery management-console report-type`
{: #backup-recovery-cli-management-console-report-type-command}

Fetches list of properties of a report type.

```sh
  ibmcloud backup-recovery management-console report-type --report-type REPORT-TYPE
```

#### Command options
{: #backup-recovery-management-console-report-type-cli-options}

`--report-type` (string)
:   Required. Specifies the report type. Allowable values are: Failures, ProtectedUnprotectedObjects, ProtectedObjects, ProtectionActivity, ProtectionGroupSummary, ProtectionRuns, ProtectionRunsTrend, Recovery.

#### Example
{: #backup-recovery-management-console-report-type-examples}

```sh
  ibmcloud backup-recovery management-console report-type \
    --report-type Failures
```
{: pre}

### `ibmcloud backup-recovery management-console export-report`
{: #backup-recovery-cli-management-console-export-report-command}

Export a configured report.

```sh
  ibmcloud backup-recovery management-console export-report --id ID [--async=ASYNC] [--filters FILTERS | @FILTERS-FILE] [--layout LAYOUT] [--report-format REPORT-FORMAT] [--timezone TIMEZONE]
```

#### Command options
{: #backup-recovery-management-console-export-report-cli-options}

`--async` (boolean)
:   Specifies whether the report should be generated asynchronously.

`--filters` (string)
:   Specifies a list of global filters that are applicable to given components in the report. It should be a JSON string or a path to a JSON file.

`--id` (string)
:   Required. Specifies the id of the report.

`--layout` (string)
:   The layout of the report that needs to be exported.

`--output-file` (string)
:   Filename/path to write the resulting output to.

`--report-format` (string)
:   The format in which the report needs to be exported. Allowable values are: XLS, CSV.

`--timezone` (string)
:   Specifies the time zone of the user. If nil, defaults to UTC. The time that is specified should be a location name in the IANA time zone database, for example, 'America/Los_Angeles'.

#### Example
{: #backup-recovery-management-console-export-report-examples}

```sh
  ibmcloud backup-recovery management-console export-report \
    --id exampleString \
    --async=true \
    --filters '[{"attribute": "exampleString", "filterType": "In", "inFilterParams": {"attributeDataType": "Bool", "attributeLabels": ["exampleString","anotherTestString"], "boolFilterValues": [true,false], "int32FilterValues": [38,39], "int64FilterValues": [26,27], "stringFilterValues": ["exampleString","anotherTestString"]}, "rangeFilterParams": {"lowerBound": 26, "upperBound": 26}, "systemsFilterParams": {"systemIds": ["exampleString","anotherTestString"], "systemNames": ["exampleString","anotherTestString"]}, "timeRangeFilterParams": {"dateRange": "Last1Hour", "durationHours": 26, "lowerBound": 26, "upperBound": 26}}]' \
    --layout exampleString \
    --report-format XLS \
    --timezone exampleString \
    --output-file tempdir/example-output.txt
```
{: pre}

### `ibmcloud backup-recovery management-console reports-preview`
{: #backup-recovery-cli-management-console-reports-preview-command}

Get preview of a configured report.

```sh
  ibmcloud backup-recovery management-console reports-preview --id ID [--component-ids COMPONENT-IDS] [--filters FILTERS | @FILTERS-FILE] [--timezone TIMEZONE]
```

#### Command options
{: #backup-recovery-management-console-reports-preview-cli-options}

`--component-ids` (string)
:   Specifies a list of components ids to be evaluated for the given report. If not specified, then all the components are evaluated.

`--filters` (string)
:   Specifies a list of global filters that are applicable to given components in the report. It should be a JSON string or a path to a JSON file.

`--id` (string)
:   Required. Specifies the id of the report.

`--timezone` (string)
:   Specifies the time zone of the user. If nil, defaults to UTC. The time specified should be a location name in the IANA time zone database, for example, 'America/Los_Angeles'.

#### Example
{: #backup-recovery-management-console-reports-preview-examples}

```sh
  ibmcloud backup-recovery management-console reports-preview \
    --id exampleString \
    --component-ids exampleString,anotherTestString \
    --filters '[{"attribute": "exampleString", "filterType": "In", "inFilterParams": {"attributeDataType": "Bool", "attributeLabels": ["exampleString","anotherTestString"], "boolFilterValues": [true,false], "int32FilterValues": [38,39], "int64FilterValues": [26,27], "stringFilterValues": ["exampleString","anotherTestString"]}, "rangeFilterParams": {"lowerBound": 26, "upperBound": 26}, "systemsFilterParams": {"systemIds": ["exampleString","anotherTestString"], "systemNames": ["exampleString","anotherTestString"]}, "timeRangeFilterParams": {"dateRange": "Last1Hour", "durationHours": 26, "lowerBound": 26, "upperBound": 26}}]' \
    --timezone exampleString
```
{: pre}

### `ibmcloud backup-recovery management-console reports-get`
{: #backup-recovery-cli-management-console-reports-get-command}

Get a report for a given id.

```sh
  ibmcloud backup-recovery management-console reports-get --id ID
```

#### Command options
{: #backup-recovery-management-console-reports-get-cli-options}

`--id` (string)
:   Required. Specifies the id of the report.

#### Example
{: #backup-recovery-management-console-reports-get-examples}

```sh
  ibmcloud backup-recovery management-console reports-get \
    --id exampleString
```
{: pre}

### `ibmcloud backup-recovery management-console reports-list`
{: #backup-recovery-cli-management-console-reports-list-command}

Fetches list of all reports accessible by logged in user.

```sh
  ibmcloud backup-recovery management-console reports-list [--ids IDS] [--user-context USER-CONTEXT]
```

#### Command options
{: #backup-recovery-management-console-reports-list-cli-options}

`--ids` (string)
:   Specifies the ids of reports to fetch.

`--user-context` (string)
:   Specifies the user context to filter reports. Allowable values are: IBMBaaS.

#### Example
{: #backup-recovery-management-console-reports-list-examples}

```sh
  ibmcloud backup-recovery management-console reports-list \
    --ids exampleString,anotherTestString \
    --user-context IBMBaaS
```
{: pre}


## End-to-end example: Kubernetes backup and recovery
{: #backup-recovery-k8s-end-to-end-example}

This section demonstrates a complete, end-to-end workflow for protecting a Kubernetes (IKS/ROKS) cluster by using the {{site.data.keyword.baas_full_notm}} CLI.
The example covers cluster registration, policy creation, backup execution, recovery, and clean up.
{: .shortdesc}

### Prerequisites
{: #backup-recovery-k8s-example-prereqs}

Before you begin, ensure that you have the following:

* {{site.data.keyword.cloud_notm}} CLI installed - verify with:
  ```sh
  ibmcloud --version
  ```
  {: pre}

* `backup-recovery` CLI plug-in installed - verify with:
  ```sh
  ibmcloud plugin list | grep backup-recovery
  ```
  {: pre}

  If not installed, run:
  ```sh
  ibmcloud plugin install backup-recovery
  ```
  {: pre}

* `kubectl` configured for the source/target Kubernetes cluster - verify with:
  ```sh
  kubectl version --client
  kubectl get nodes
  ```
  {: pre}

* Access to {{site.data.keyword.cloud_notm}} instance.
* IAM API key for authentication.

### Step 1: Log in to IBM Cloud
{: #backup-recovery-k8s-example-step1}

Log in to {{site.data.keyword.cloud_notm}} by using Single Sign-On (SSO).

```sh
ibmcloud login --sso
```
{: pre}

### Step 2: Configure the service URL
{: #backup-recovery-k8s-example-step2}

Set the service URL for your Backup & Recovery instance. For more information about service URLs and how to retrieve them, see [Understanding service-url](#backup-recovery-service-url-explained).

```sh
ibmcloud backup-recovery config set service-url 'https://<instance-id>.<region>.backup-recovery.cloud.ibm.com/v2'
```
{: pre}

Replace `<instance-id>` with your Backup & Recovery instance ID and `<region>` with your IBM Cloud region (for example, `us-east`, `eu-de`, `eu-gb`).

### Step 3: Set the IAM API key
{: #backup-recovery-k8s-example-step3}

Export your IAM API key as an environment variable for authentication.

```sh
export BACKUP_RECOVERY_APIKEY=<iam-api-key>
```
{: pre}

Replace `<iam-api-key>` with your actual IAM API key.

### Step 4: Create a data source connection
{: #backup-recovery-k8s-example-step4}

Create a data source connection for your Kubernetes cluster deployment platform. This connection is required to access your Kubernetes cluster and its resources.

**Using CLI:**

```sh
ibmcloud backup-recovery data-source-connection create \
  --connection-name "data-source-connection-cli" \
  --xibm-tenant-id "8x7y9z2a3b/"
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step4-output}

```text
connectionId        4521789036542187520
connectionName      data-source-connection-cli
registrationToken   eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJjbHVzdGVyX2VuZHBv....
tenantId            8x7y9z2a3b/
```
{: codeblock}

**Important:** Save the `registrationToken` from the output. This token is used in Step 5 when installing the data source connector with the helm command.

**Note:** The `--connection-env-type` parameter is currently not supported to create connection for Kubernetes or OpenShift clusters. We recommend using the console (see "Using Console (Alternative)" below) for connection creation for now.

Data source connections for Kubernetes and OpenShift clusters must be created from the {{site.data.keyword.baas_full_notm}} instance dashboard. The Deployment Platform options (**ROKS VPC**, **IKS VPC**, **ROKS classic**, **IKS classic**) are not available when creating data source connections from the Backup Recovery Manager.
{: important}

**Using Console (Alternative):**

For detailed instructions, see [Creating a data source connection](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#data-source-connector-iks-roks-create-data-source-connection).

1. Access the {{site.data.keyword.baas_full_notm}} instance dashboard
2. Go to **Dashboard** > **System** > **Data Source Connections**
3. Check whether an existing connection is available for your deployment platform. If yes, you can use it and skip to Step 5
4. To create a new connection, click **New Connection**
5. In the **Create data source connection** wizard:
   - Select the **Deployment Platform** that matches your cluster (for example, ROKS VPC, IKS VPC, ROKS classic, IKS classic)
   - Click **Create**

After creating the connection, record the `connectionId` for use in Step 7 and the `registrationToken` for use in Step 5.

### Step 5: Install the data source connector
{: #backup-recovery-k8s-example-step5}

Install the data source connector on your Kubernetes cluster. For detailed instructions, see [Creating a data source connection](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#data-source-connector-iks-roks-create-data-source-connection).

### Step 6: Create a bearer token for Kubernetes cluster
{: #backup-recovery-k8s-example-step6}

Create a bearer token that will be used as the `clientPrivateKey` value during cluster registration. For detailed instructions, see [How to create a bearer token for a Kubernetes or OpenShift cluster](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#data-source-connector-iks-roks-create-bearer-token-cluster).

Save the output token value. This is used as the `clientPrivateKey` parameter in Step 8 when registering the Kubernetes cluster.

### Step 7: Identify the data source connection
{: #backup-recovery-k8s-example-step7}

Retrieve the data source connection details by filtering by connection name. This connection is used for Kubernetes cluster registration.

```sh
ibmcloud backup-recovery data-source-connection list \
  --connection-names "<your_connection_name>" \
  --xibm-tenant-id '<your_tenant_id>' \
  --output json
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step7-output}

```json
{
  "connections": [
    {
      "connectionId": "9876543210987654321",
      "connectionName": "roks-vpc-connection",
      "connectionEnvType": "kRoksVpc",
      "connectorIds": ["5432109876543210"],
      "tenantId": "abc123xyz/",
      "connectivityStatus": {
        "isConnected": true
      }
    }
  ]
}
```
{: codeblock}

From the output, locate and record the `connectionId` value. This ID is used in Step 8 when registering the Kubernetes cluster. In this example, the connection ID is:

* **Connection ID:** `9876543210987654321`

### Step 8: Register the Kubernetes cluster
{: #backup-recovery-k8s-example-step8}

Register the Kubernetes (ROKS) cluster as a protection source. This step deploys the Velero components and {{site.data.keyword.baas_full}} data mover into the cluster.

Before running the registration command, retrieve your cluster's API endpoint. For detailed instructions, see [How to get the Kubernetes or OpenShift cluster endpoint](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#how-to-get-iks-roks-endpoint).

Now register the cluster by using the connection ID from Step 7, the bearer token from Step 6, and the cluster endpoint:

```sh
ibmcloud backup-recovery protection-source register \
  --xibm-tenant-id "<your_tenant_id>" \
  --environment "kKubernetes" \
  --name "test-registration-new" \
  --data-source-connection-id "connectionId_from_step_5" \
  --kubernetes-params '{
    "endpoint": "cluster_endpoint_from_ibm_cloud_console",
    "clientPrivateKey": "<bearer_token_from_step_4>",
    "kubernetesDistribution": "kROKS",
    "datamoverServiceType": "kHostPort"
  }' \
  --output json
```
{: pre}

Replace `<bearer_token_from_step_6>` with the actual bearer token value you retrieved in Step 6.

#### Sample output
{: #backup-recovery-k8s-example-step8-output}

```json
{
  "id": 67565,
  "environment": "kKubernetes",
  "authenticationStatus": "Pending",
  "registrationTimeMsecs": 1775647251544,
  "sourceInfo": {
    "name": "https://c102.private.eu-es.containers.cloud.ibm.com:30339",
    "objectType": "kCluster",
    "uuid": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```
{: codeblock}

### Step 9: (Optional) Verify agent deployment in the cluster
{: #backup-recovery-k8s-example-step9}

Confirm that the backup agents are running.

```sh
kubectl get ns | grep brs
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step9-output1}

```text
brs-backup-agent-6b094462-26aa-4346-95bf-09d2409520d5   Active   2m
ibm-brs-dsc                                           Active   6d
```
{: codeblock}

```sh
kubectl get all -n brs-backup-agent-6b094462-26aa-4346-95bf-09d2409520d5
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step9-output2}

```text
pod/cohesity-dm-26mlb       1/1 Running
pod/cohesity-dm-5c9r8       1/1 Running
pod/velero-7b5b677f7c-qfphn 1/1 Running
daemonset.apps/cohesity-dm  3/3 Ready
deployment.apps/velero     1/1 Ready
```
{: codeblock}

Expected components include:

* DaemonSet pods
* Deployment

### Step 10: Create a protection policy
{: #backup-recovery-k8s-example-step10}

Create a backup policy with a daily incremental schedule and a two-week retention period.

```sh
ibmcloud backup-recovery protection-policy create \
  --xibm-tenant-id "<your_tenant_id>" \
  --name "test-backup-policy-2" \
  --backup-policy \
'{
  "regular": {
    "incremental": {
      "schedule": {
        "daySchedule": {
          "frequency": 1
        },
        "unit": "Days"
      }
    },
    "retention": {
      "duration": 2,
      "unit": "Weeks"
    },
    "primaryBackupTarget": {
      "useDefaultBackupTarget": true
    }
  }
}' \
  --retry-options \
'{
  "retries": 3,
  "retryIntervalMins": 5
}' \
  --output json
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step10-output}

```json
{
  "id": "1234567890123456:9876543210987:7654321",
  "name": "test-backup-policy-2",
  "backupPolicy": {
    "regular": {
      "incremental": {
        "schedule": {
          "unit": "Days",
          "daySchedule": { "frequency": 1 }
        }
      },
      "retention": {
        "unit": "Weeks",
        "duration": 2
      }
    }
  }
}
```
{: codeblock}

From the output, locate and record the policy `id` value. This ID is used in Step 13 when creating the protection group. In this example, the policy ID is:

* **Policy ID:** `1234567890123456:9876543210987:7654321`

### Step 11: Verify protection policies
{: #backup-recovery-k8s-example-step11}

```sh
ibmcloud backup-recovery protection-policy list \
  --xibm-tenant-id "<your_tenant_id>" \
  --output json
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step11-output}

```json
{
  "policies": [
    {
      "id": "1234567890123456:9876543210987:7654321",
      "name": "test-backup-policy-2"
    },
    {
      "id": "1234567890123456:9876543210987:1111111",
      "name": "Basic"
    }
  ]
}
```
{: codeblock}

### Step 12: List protection sources
{: #backup-recovery-k8s-example-step12}

List Kubernetes objects (namespaces, PVCs, labels) available for protection.

```sh
ibmcloud backup-recovery protection-source list \
  --xibm-tenant-id "<your_tenant_id>" \
  --output json
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step12-output}

```json
{
  "protectionSources": [
    {
      "protectionSource": {
        "id": 67598,
        "name": "roks-419-reg",
        "environment": "kKubernetes",
        "type": "kNamespace"
      }
    }
  ]
}
```
{: codeblock}

From the output, identify the Kubernetes objects (namespaces, PVCs, and so on) you want to protect and record their `id` values. These IDs are used in Step 13 when creating the protection group. In this example, the namespace object ID is:

* **Object ID:** `67598`

### Step 13: Create a protection group
{: #backup-recovery-k8s-example-step13}

Associate Kubernetes objects with the protection policy.

```sh
ibmcloud backup-recovery protection-group create \
  --xibm-tenant-id "<your_tenant_id>" \
  --name "test2-protection-group" \
  --description "Protection group for test-registration cluster" \
  --policy-id "1234567890123456:9876543210987:7654321" \
  --environment "kKubernetes" \
  --priority "kMedium" \
  --qos-policy "kBackupHDD" \
  --kubernetes-params '{
    "objects": [
      { "id": 67598, "backupOnlyPvc": false }
    ],
    "leverageCSISnapshot": false,
    "nonSnapshotBackup": true
  }' \
  --output json
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step13-output}

```json
{
  "id": "5555666677778888:1122334455667:8899000",
  "name": "test2-protection-group",
  "policyId": "1234567890123456:9876543210987:7654321",
  "environment": "kKubernetes",
  "priority": "kMedium"
}
```
{: codeblock}

From the output, locate and record the protection group `id` value. This ID is used in Step 14 to trigger backup runs and in Step 15 to monitor them. In this example, the protection group ID is:

* **Protection Group ID:** `5555666677778888:1122334455667:8899000`

### Step 14: Trigger a backup run
{: #backup-recovery-k8s-example-step14}

Start an on-demand full backup.

```sh
ibmcloud backup-recovery protection-group-run create \
  --id "5555666677778888:1122334455667:8899000" \
  --xibm-tenant-id "<your_tenant_id>" \
  --run-type "kFull" \
  --output json
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step14-output}

```json
{
  "protectionGroupId": "5555666677778888:1122334455667:8899000",
  "runType": "kFull",
  "status": "Accepted"
}
```
{: codeblock}

### Step 15: Monitor backup runs
{: #backup-recovery-k8s-example-step15}

```sh
ibmcloud backup-recovery protection-group-run list \
  --id "5555666677778888:1122334455667:8899000" \
  --xibm-tenant-id "<your_tenant_id>" \
  --output json
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step15-output}

```json
{
  "runs": [
    {
      "runType": "kFull",
      "status": "kSuccess",
      "startTimeUsecs": 1775642489148017
    }
  ]
}
```
{: codeblock}

Detailed view including object-level status:

```sh
ibmcloud backup-recovery protection-group-run list \
  --id "5555666677778888:1122334455667:8899000" \
  --xibm-tenant-id "<your_tenant_id>" \
  --include-object-details=true \
  --exclude-non-restorable-runs=true \
  --num-runs 10 \
  --output json
```
{: pre}

### Step 16: Recover namespaces to the same cluster
{: #backup-recovery-k8s-example-step16}

```sh
ibmcloud backup-recovery recovery create \
  --xibm-tenant-id "<your_tenant_id>" \
  --name "cli-test-recovery" \
  --snapshot-environment "kKubernetes" \
  --kubernetes-params '{
    "recoveryAction": "RecoverNamespaces",
    "recoverNamespaceParams": {
      "targetEnvironment": "kKubernetes",
      "kubernetesTargetParams": {
        "objects": [
          { "snapshotId": "<snapshotId>" }
        ],
        "renameRecoveredNamespacesParams": {
          "prefix": "copy2"
        },
        "recoveryTargetConfig": {
          "recoverToNewSource": false
        }
      }
    }
  }' \
  --output json
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step16-output}

```json
{
  "id": 2238894,
  "name": "cli-test-recovery",
  "status": "Accepted",
  "recoveryAction": "RecoverNamespaces"
}
```
{: codeblock}

### Step 17: Recover namespaces to a different cluster
{: #backup-recovery-k8s-example-step17}

```sh
ibmcloud backup-recovery recovery create \
  --xibm-tenant-id "<your_tenant_id>" \
  --name "cli-test-recovery-new-source" \
  --snapshot-environment "kKubernetes" \
  --kubernetes-params '{
    "recoveryAction": "RecoverNamespaces",
    "recoverNamespaceParams": {
      "targetEnvironment": "kKubernetes",
      "kubernetesTargetParams": {
        "objects": [
          { "snapshotId": "<snapshotId>" }
        ],
        "skipClusterCompatibilityCheck": true,
        "renameRecoveredNamespacesParams": {
          "prefix": "copy2"
        },
        "recoveryTargetConfig": {
          "recoverToNewSource": true,
          "newSourceConfig": {
            "source": {
              "id": 66974,
              "name": "https://c106.private.useast.containers.cloud.ibm.com:31561"
            }
          }
        }
      }
    }
  }' \
  --output json
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step17-output}

```json
{
  "id": 2238942,
  "name": "cli-test-recovery-new-source",
  "status": "Accepted"
}
```
{: codeblock}

### Step 18: Cleanup (unregister the protection source) - Optional
{: #backup-recovery-k8s-example-step18}

If you want to clean up and remove the protection source, follow these steps in order:

**1. Delete the protection group:**

```sh
ibmcloud backup-recovery protection-group delete \
  --id "5555666677778888:1122334455667:8899000" \
  --xibm-tenant-id "<your_tenant_id>"
```
{: pre}

**2. Delete the protection policy (optional):**

```sh
ibmcloud backup-recovery protection-policy delete \
  --id "1234567890123456:9876543210987:7654321" \
  --xibm-tenant-id "<your_tenant_id>"
```
{: pre}

**3. Unregister the cluster:**

```sh
ibmcloud backup-recovery protection-source registration-delete \
  --id 67565 \
  --xibm-tenant-id "<your_tenant_id>"
```
{: pre}

#### Sample output
{: #backup-recovery-k8s-example-step18-output}

```text
Are you sure you want to delete this registration? [y/N]: y
Registration deleted successfully
```
{: codeblock}

**4. Delete the data source connection:**

```sh
ibmcloud backup-recovery data-source-connection delete \
  --connection-id "4521789036542187520" \
  --xibm-tenant-id "<your_tenant_id>"
  --force
```
{: pre}

**Alternative - Using Console:**

If you prefer to use the console:
1. Navigate to your Backup & Recovery instance
2. Go to **Data Protection** > **Data Source Connections**
3. Select the connection and delete it

This completes the cleanup process. All backup data and snapshots that are associated with the protection group will be retained according to the retention policy unless explicitly deleted.
