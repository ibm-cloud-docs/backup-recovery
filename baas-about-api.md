---

copyright:
  years: 2024
lastupdated: "2025-07-30"

keywords: rest, s3, compatibility, api, error

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}


# About the Backup and Recovery API
{: #compatibility-api}

The {{site.data.keyword.cos_full}} API is a REST-based API for reading and writing objects.
{: shortdesc}

It uses {{site.data.keyword.iamlong}} for authentication and authorization, and supports a subset of the S3 API for easy migration of applications to {{site.data.keyword.cloud_notm}}.

This reference documentation is being continuously improved. If you have technical questions about using the API in your application, post them on [StackOverflow](https://stackoverflow.com/). Add both `ibm-cloud-platform` and `object-storage` tags and help improve this documentation thanks to your feedback.

As {{site.data.keyword.iamshort}} tokens are relatively easy to work with, `curl` is a good choice for basic testing and interaction with your storage. More information can be found in [the `curl` reference](/docs/cloud-object-storage?topic=cloud-object-storage-curl).

The following tables describe the complete set of operations of the {{site.data.keyword.baas_full_notm}} API. For more information, see [the API reference page for buckets](/docs/cloud-object-storage?topic=cloud-object-storage-compatibility-api-bucket-operations) or [objects](/docs/cloud-object-storage?topic=cloud-object-storage-object-operations).

## Datasource Registration
{: #compatibility-api-datasource-registration}

These operations create, delete, get information about datasource registrations.

| Method | Note |
|------|-------------|
| `GET` DATASOURCE REGISTRATIONS | Get the list of Protection Source registrations. |
| `GET` DATASOURCE REGISTRATION | Get a Protection Source registration. |
| `POST` DATASOURCE REGISTRATION  | Register a Protection Source. |
| `PUT` DATASOURCE REGISTRATION  |  Update Protection Source registration. |
| `DELETE` DATASOURCE REGISTRATION  | Delete Protection Source Registration. |
| `PATCH` DATASOURCE REGISTRATION  | Patches a Protection Source. |
{: caption="Datasource Registration" caption-side="top"}

## Protection Group
{: #compatibility-protect-group}

These operations create, delete, get information about protection groups.

| Method | Note |
|------|-------------|
| `GET` GROUPS | Get the list of Protection Groups. |
| `GET` GROUP | List details about single Protection Group. |
| `POST` GROUP | Create a Protection Group. |
| `PUT` GROUP |  Update the specified Protection Group. |
| `DELETE` GROUP | Delete unused Protection Group. |
{: caption="Protection Group" caption-side="top"}

## Protection Policy
{: #compatibility-protect-policy}

These operations create, delete, get information about protection policy.

| Method | Note |
|------|-------------|
| `GET` POLICIES | List Protection Policies based on provided filtering parameters. |
| `GET` POLICY | List details about a single Protection Policy. |
| `POST` POLICY | Create the Protection Policy and returns the newly created policy object. |
| `PUT` POLICY |  Update a Protection Policy. |
| `DELETE` POLICY | Delete an unused Protection Policy. |
{: caption="Protection Policy" caption-side="top"}

## Protection Group Runs
{: #compatibility-protect-policy}

These operations create, delete, get information about protection group runs.

| Method | Note |
|------|-------------|
| `GET` PROTECTION GROUP RUNS  |  Get the list of runs for a Protection Group. |
| `POST` PROTECTION GROUP RUN  | Create a new protection run. |
| `PUT` PROTECTION GROUP RUN  | Update runs for a particular Protection Group. |
| `POST` ACTION ON PROTECTION GROUP RUN | Perform various actions on a Protection Group run. |
{: caption="Protection Group Runs" caption-side="top"}

## Recovery
{: #compatibility-recovery}

These operations create, delete, get information about, and control behavior of recoveries.

| Method | Note |
|------|-------------|
| `GET` RECOVERIES | Lists the Recoveries. |
| `GET` RECOVERY | Get Recovery for a given id. |
| `POST` RECOVERY  | Performs a Recovery. |
| `POST` FILES AND FOLDERS RECOVERY  |  Create a download files and folders recovery. |
| `POST` RESTORE POINTS  |  List Restore Points i.e returns the snapshots in a given time range. |
| `GET` FILES FROM FILE RECOVERY  | Download files from the given download file recovery. |
| `GET` FILES FROM SNAPSHOT | Download an indexed file from a snapshot. |
{: caption="Recovery" caption-side="top"}

## Datasource Connections
{: #compatibility-datasource-connections}

These operations create, delete, get information about datasource connections.

| Method | Note |
|------|-------------|
| `GET` CONNECTIONS | Gets all specified data-source connections. |
| `GET` CONNECTION | List details about a single Protection Policy. |
| `POST` CONNECTION | Creates a data-source connection which can be used to register and protect sources, to access filer services, etc. |
| `POST` CONNECTION REGISTRATION TOKEN | Generate a token to register connectors against the data-source connection that can be used to register multiple connectors. |
| `PATCH` CONNECTION |  Patch a data-source connection using its ID. |
| `DELETE` CONNECTION | Delete the data-source connection specified by the ID in the request path. |
{: caption="Datasource Connections" caption-side="top"}

## Agent
{: #compatibility-agent}

These operations create, delete, get information about, and control behavior of agents.

| Method | Note |
|------|-------------|
| `GET` AGENT UPGRADE TASKS | Get the list of agent upgrade tasks. |
| `POST` AGENT | Download agent for different hosts. |
| `POST` AGENT UPGRADE TASK | Create a schedule-based agent upgrade task. |
{: caption="Agent" caption-side="top"}

## Object
{: #compatibility-object}

These operations create, delete, get information about, and control behavior of objects.

| Method | Note |
|------|-------------|
| `GET` OBJECT SNAPSHOTS | List the snapshots for a given object. |
| `GET` OBJECTS | List Objects. |
| `GET` PROTECTED OBJECTS | List Protected Objects. |
| `POST` INDEXED OBJECTS  | List all the indexed objects. |
{: caption="Object" caption-side="top"}

### Datasource Connectors
{: #compatibility-datasource-connectors}

These operations create, delete, get information about datasource connectors.

| Method | Note |
|------|-------------|
| `GET` CONNECTORS | Gets all specified data-source connectors. |
| `GET` CONNECTORS METADATA | Get information about the available connectors. |
| `PATCH` CONNECTOR |  Patch a data-source connector using its ID. |
| `DELETE` CONNECTOR | Delete a data-source connector using its ID. |
{: caption="Datasource Connectors" caption-side="top"}



More information about {{site.data.keyword.baas_full_notm}} features and use-cases can be found at [ibm.com](https://www.ibm.com/products/cloud-object-storage).
