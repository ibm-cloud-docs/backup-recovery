---

copyright:
  years:  2024, 2025
lastupdated: "2025-07-30"

keywords:

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Service endpoints
{: #endpoints}

Access {{site.data.keyword.baas_full_notm}} by using the listed endpoints for each supported {{site.data.keyword.cloud_notm}} location.
{: shortdesc}


## Private endpoints
{: #private-endpoints}

Private endpoints restrict access to the {{site.data.keyword.cloud_notm}} private network.  Both the API and service backup support private endpoints.

| Geography | Region                           | API Endpoint | Backup Endpoint |
|-----------|----------------------------------|---------------------|--------------------|
| United States  | Washington MZR (`us-east`) | `<instance_ID>.private.us-east.backup-recovery.cloud.ibm.com` | `<instance_ID>.private.us-east.backup-recovery.cloud.ibm.com` |
{: caption="List of {{site.data.keyword.baas_full_notm}} private endpoints" caption-side="top"}


When you connect by using a [VPE](/docs/vpc?topic=vpc-about-vpe) or [CSE](/docs/account?topic=account-service-endpoints-overview), use port 443 with API and and port 29991 with the backup endpoints.
{: important}

## Querying the service endpoints with the CLI
{: #endpoints-cli}
{: cli}

To retrieve the service endpoints of a {{site.data.keyword.baas_full_notm}} instance using IBM Cloud CLI, use the command illustrated below:

`ibmcloud resource service-instance (NAME|ID) --output json`

The endpoint information for a service is available as part of the `extensions` blob in the response json.

```json
{
    "extensions": {
        "endpoints": {
           "public": "https://<instance_ID>.us-east.backup-recovery.cloud.ibm.com",
           "private": "https://<instance_ID>.private.us-east.backup-recovery.cloud.ibm.com"
        }
    }
}
```

## Querying the service endpoints with the API
{: #endpoints-api}
{: api}

The service endpoint information can be queried using the [Global Search API](/apidocs/search#search) as illustrated below:

```shell
# Search using service instance name
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: bearer <your IAM token>' -d '{"query": "type:resource-instance AND name:ABC","fields": ["*"]}' 'https://api.global-search-tagging.cloud.ibm.com/v3/resources/search?limit=1'

# Search using service instance guid
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' --header 'Authorization: bearer <your IAM token>' -d '{"query": "type:resource-instance AND service_instance:9613ab9d-9749-4ef7-82b9-56cd023492e5","fields": ["doc.extensions.endpoints.private"]}' 'https://api.global-search-tagging.cloud.ibm.com/v3/resources/search?limit=1'
```
