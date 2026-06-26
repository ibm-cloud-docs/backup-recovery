---

copyright:
  years: 2026
lastupdated: "2026-06-26"

keywords: connector agent, api, registration, configuration, backup recovery

subcollection: cloud-object-storage

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.baas_full_notm}} Connector Agent API operations
{: #connector-agent-operations}

The connector agent capabilities of {{site.data.keyword.baas_full}} are available via a RESTful API. Operations and methods for listing connector agents, retrieving configuration details, and registering connector agents are documented here.
{: shortdesc}

It uses {{site.data.keyword.iamlong}} for authentication and authorization. For more information about endpoints and authentication, see [Endpoints and storage locations](/docs/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints).
{: tip}

## Authentication
{: #connector-agent-auth}

All API requests require an API key for authentication. To create an API key, see the getting started documentation.

The API key does not expire until explicitly deleted.

---

## List connector agents
{: #list-connector-agents}

A `GET` request to `/connector-agents` returns a list of connector agents for a given tenant based on filters on connection names or connection IDs. If no filters are provided, it lists all agents for that tenant.

**Syntax**

```http
GET /connector-agents
```
{: codeblock}

### Required header parameters

| Name | Type | Description |
|------|------|-------------|
| `X-IBM-Tenant-Id` | String | Specifies the unique ID of the tenant. |
{: caption="Required header parameters for listing connector agents" caption-side="bottom"}

### Required query parameters

| Name | Type | Description |
|------|------|-------------|
| `tenantId` | String | Specifies the ID of the tenant for which the connector agents are to be fetched. |
{: caption="Required query parameters for listing connector agents" caption-side="bottom"}

### Optional query parameters

| Name | Type | Description |
|------|------|-------------|
| `connectionNames` | Array of strings | Specifies the connection names whose connector agents are to be fetched. |
| `connectionIds` | Array of integers | Specifies the connection IDs whose connector agents are to be fetched. |
{: caption="Optional query parameters for listing connector agents" caption-side="bottom"}

**Example request**

```http
GET /connector-agents?tenantId=tenant123&connectionNames=connection1,connection2 HTTP/1.1
Authorization: Bearer {apiKey}
X-IBM-Tenant-Id: tenant123
Host: <BACKUP_RECOVERY_URL>
```
{: codeblock}

**Example response**

```json
{
  "connectorAgents": [
    {
      "connectorAgentId": "agent-001",
      "connectorAgentName": "Production Agent 1",
      "connectionId": "conn-123",
      "connectionName": "connection1",
      "softwareVersion": "1.0.0",
      "connectivityStatus": {
        "isConnected": true,
        "connectedSinceTimestampSecs": 1704067200,
        "lastKnownHealthOkTimestampSecs": 1704153600
      }
    },
    {
      "connectorAgentId": "agent-002",
      "connectorAgentName": "Production Agent 2",
      "connectionId": "conn-124",
      "connectionName": "connection2",
      "softwareVersion": "1.0.0",
      "connectivityStatus": {
        "isConnected": false,
        "message": "Connection timeout",
        "lastKnownHealthOkTimestampSecs": 1704067200
      }
    }
  ]
}
```
{: codeblock}

### Response fields




| Property | Type | Description |
| -------- | ---- | ----------- |
| `connectorAgentId` | String | The unique ID of the connector agent. |
| `connectorAgentName` | String | The name of the connector agent. |
| `connectionId` | String | The ID of the connection to which this connector agent belongs. |
| `connectionName` | String | The name of the connection to which this connector agent belongs. |
| `softwareVersion` | String | The connector agent's software version. |
| `connectivityStatus` | Object | Connector agent connectivity status information including current connectivity status to cluster, when it last connected to the cluster successfully, and from when it has been continuously connected to the cluster without any interruptions. |
| `connectivityStatus.isConnected` | Boolean | Whether the connector agent is currently connected to the cluster. |
| `connectivityStatus.message` | String | Error message when the connector agent is unable to connect to the cluster. |
| `connectivityStatus.connectedSinceTimestampSecs` | Integer | The timestamp in UNIX seconds since when this connector agent has been connected to its cluster without any interruptions. This property is not present if the connector agent is not currently connected to its cluster. |
| `connectivityStatus.lastKnownHealthOkTimestampSecs` | Integer | The most recent known timestamp in UNIX seconds at which this connector agent passed the health checks. This property can be present even if the connector agent is not currently connected to its cluster. |
{: caption="Connector agent properties" caption-side="bottom"}

---

## Get connector agent configuration
{: #get-connector-agent-config}

A `GET` request to `/connector-agents/config` retrieves the configuration, containing JWT registration token, for claiming a connector agent to the DataProtect cluster.

**Syntax**

```http
GET /connector-agents/config
```
{: codeblock}

### Required header parameters

| Name | Type | Description |
|------|------|-------------|
| `X-IBM-Tenant-Id` | String | Specifies the unique ID of the tenant. |
{: caption="Required header parameters for getting connector agent configuration" caption-side="bottom"}

**Example request**

```http
GET /connector-agents/config HTTP/1.1
Authorization: Bearer {apiKey}
X-IBM-Tenant-Id: tenant123
Host: <BACKUP_RECOVERY_URL>
```
{: codeblock}

**Example response**

```json
{
  "registrationToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}
```
{: codeblock}

### Response fields



| Property | Type | Description |
| -------- | ---- | ----------- |
| `registrationToken` | String | Token that is used for authenticating the connector agent with the DataProtect cluster. By default, the token is valid for 24 hours. |
{: caption="Registration token property" caption-side="bottom"}

---

## Register a connector agent
{: #register-connector-agent}

A `POST` request to `/v1/connector-agents/registration` registers a connector agent with a backup instance using the supplied registration token. The registration token for the connector agent must be obtained by invoking the `/connector-agents/config` API on the backup instance. When a duplicate registration is attempted, this API returns success with a header indicating that it was already registered.

**Syntax**

```http
POST /v1/connector-agents/registration
```
{: codeblock}

### Request body

The request body must contain the following parameters:

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `registrationToken` | String | Yes | The JWT registration token. A single token can be used to register multiple connector agents in that tenant. By default, the token is valid for 24 hours. |
| `connectionName` | String | Yes | Specifies the name to be associated with the connector agent. This must be unique within the tenant to which this connector agent is registered. |
| `joinExistingConnection` | Boolean | No | Whether this agent is joining a connection that was already claimed by a previous registration (for example, another agent in the same cluster for clustered sources). When true, the server adds this agent to the existing connection instead of rejecting the request as a duplicate. If the connection does not yet exist, a new one is created regardless of this flag. Default is false. |
{: caption="Request body parameters for registering a connector agent" caption-side="bottom"}

**Example request**

```http
POST /v1/connector-agents/registration HTTP/1.1
Authorization: Bearer {apiKey}
Host: <BACKUP_RECOVERY_URL>
Content-Type: application/json

{
  "registrationToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
  "connectionName": "production-agent-01",
  "joinExistingConnection": false
}
```
{: codeblock}

**Example response**

The API returns a `204 No Content` response on success. The response includes a header that indicates the registration status:

```http
HTTP/1.1 204 No Content
X-Registration-Status: registered
```
{: codeblock}

### Response headers



| Header | Type | Description |
| ------ | ---- | ----------- |
| `X-Registration-Status` | String | Indicates if a duplicate registration was attempted. Possible values are `registered` (new registration) or `already-registered` (duplicate registration attempt). |
{: caption="X-Registration-Status header" caption-side="bottom"}

### Error responses
{: #connector-agent-error-codes}

If the registration fails, the API returns an error response with details about the failure:

```json
{
  "errorCode": "InvalidToken",
  "message": "The provided registration token is invalid or has expired"
}
```
{: codeblock}



The following table lists common error codes that you might encounter when using the Connector Agent APIs.

| Error code | Description |
| ---------- | ----------- |
| `InvalidToken` | The registration token is invalid or has expired. |
| `DuplicateConnection` | A connection with the specified name already exists and `joinExistingConnection` is false. |
| `UnauthorizedAccess` | The request lacks valid authentication credentials. |
{: caption="Common error codes" caption-side="bottom"}
