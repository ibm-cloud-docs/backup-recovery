---

copyright:
  years: 2025
lastupdated: "2025-12-15"

keywords: rest, helios, reporting, api, components, reports, resources

subcollection: cloud-object-storage

---

{{site.data.keyword.attribute-definition-list}}

#  Backup and Recovery Reporting API operations
{: #helios-reporting-operations}

The modern capabilities of {{site.data.keyword.baas_full_notm}} are conveniently available via a RESTful API. Operations and methods for listing report components, fetching component details, previewing components, listing resources, and managing reports are documented here.
{: shortdesc}

It uses {{site.data.keyword.iamlong}} for authentication and authorization. For more information about endpoints and authentication, see [Endpoints and storage locations](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-endpoints) and [Reporting Service API documentation](https://test.cloud.ibm.com/apidocs/ibm_reporting_api%3C/stage).
{: tip}





## Authentication
{: #helios-reporting-auth}

All API requests require an API key for authentication. To create an API key, see: [Creating your IBM Cloud API key](https://www.ibm.com/docs/en/masv-and-l/cd?topic=cli-creating-your-cloud-api-key){: external}.

The API key does not expire until explicitly deleted.

---

## List Report Components
{: #helios-list-components}

A `GET` request to `/heliosreporting/api/v1/public/components` returns a list of all report components accessible by the logged-in user.

**Syntax**

```http
GET /heliosreporting/api/v1/public/components
```
{: codeblock}

### Optional query parameters

| Name | Type   | Description |
|------|--------|-------------|
| `ids`  | Array  | Specifies the ids of the report components to fetch. |
{: caption="Optional query parameters" caption-side="bottom"}

**Example request**

```http
GET /heliosreporting/api/v1/public/components?ids=component1,component2 HTTP/1.1
Authorization: Bearer {apiKey}
Host: <HELIOS_URL>
```
{: codeblock}

**Example response**

```json
{
  "components": [
    {
      "aggs": {
        "aggregatedAttributes": [
          {
            "aggregationType": "sum",
            "attribute": "string",
            "label": "string"
          }
        ],
        "groupedAttributes": [
          "string"
        ]
      },
      "config": {
        "xlsxParams": {
          "attributeConfig": [
            {
              "attributeName": "string",
              "customLabel": "string",
              "format": "Timestamp"
            }
          ]
        }
      },
      "data": [
        {}
      ],
      "description": "string",
      "filters": [
        {
          "attribute": "string",
          "filterType": "In",
          "inFilterParams": {
            "attributeDataType": "Bool",
            "attributeLabels": [
              "string"
            ],
            "boolFilterValues": [
              true
            ],
            "int32FilterValues": [
              0
            ],
            "int64FilterValues": [
              0
            ],
            "stringFilterValues": [
              "string"
            ]
          },
          "rangeFilterParams": {
            "lowerBound": 0,
            "upperBound": 0
          },
          "systemsFilterParams": {
            "systemIds": [
              "string"
            ],
            "systemNames": [
              "string"
            ]
          },
          "timeRangeFilterParams": {
            "dateRange": "Last1Hour",
            "durationHours": 0,
            "lowerBound": 0,
            "upperBound": 0
          }
        }
      ],
      "id": "string",
      "limit": {
        "from": 0,
        "size": 1
      },
      "name": "string",
      "reportType": "Failures",
      "sort": [
        {
          "attribute": "string",
          "desc": true
        }
      ]
    }
  ]
}
```

---

## Fetch a Report Component
{: #helios-get-component}

A `GET` request to `/heliosreporting/api/v1/public/components/{id}` fetches information about a specific report component.

**Syntax**

```http
GET /heliosreporting/api/v1/public/components/{id}
```
{: codeblock}

### Path parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id`  | String | Yes      | The id of the report component. |
{: caption="Path parameters" caption-side="bottom"}

**Example request**

```http
GET /heliosreporting/api/v1/public/components/component1 HTTP/1.1
Authorization: Bearer {apiKey}
Host: <HELIOS_URL>
```
{: codeblock}

**Example response**

```json
{
  "aggs": {
    "aggregatedAttributes": [
      {
        "aggregationType": "sum",
        "attribute": "string",
        "label": "string"
      }
    ],
    "groupedAttributes": [
      "string"
    ]
  },
  "config": {
    "xlsxParams": {
      "attributeConfig": [
        {
          "attributeName": "string",
          "customLabel": "string",
          "format": "Timestamp"
        }
      ]
    }
  },
  "data": [
    {}
  ],
  "description": "string",
  "filters": [
    {
      "attribute": "string",
      "filterType": "In",
      "inFilterParams": {
        "attributeDataType": "Bool",
        "attributeLabels": [
          "string"
        ],
        "boolFilterValues": [
          true
        ],
        "int32FilterValues": [
          0
        ],
        "int64FilterValues": [
          0
        ],
        "stringFilterValues": [
          "string"
        ]
      },
      "rangeFilterParams": {
        "lowerBound": 0,
        "upperBound": 0
      },
      "systemsFilterParams": {
        "systemIds": [
          "string"
        ],
        "systemNames": [
          "string"
        ]
      },
      "timeRangeFilterParams": {
        "dateRange": "Last1Hour",
        "durationHours": 0,
        "lowerBound": 0,
        "upperBound": 0
      }
    }
  ],
  "id": "string",
  "limit": {
    "from": 0,
    "size": 1
  },
  "name": "string",
  "reportType": "Failures",
  "sort": [
    {
      "attribute": "string",
      "desc": true
    }
  ]
}
```

---

## Preview a Report Component
{: #helios-preview-component}

A `POST` request to `/heliosreporting/api/v1/public/components/{id}/preview` fetches a preview for a component that is specified by id.

**Syntax**

```http
POST /heliosreporting/api/v1/public/components/{id}/preview
```
{: codeblock}

### Path parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id`   | String | Yes      | The id of the component. |
{: caption="Path parameters" caption-side="bottom"}

### Request body

```json
{
  "filters": [
    {
      "attribute": "string",
      "filterType": "In",
      "inFilterParams": {
        "attributeDataType": "Bool",
        "attributeLabels": [
          "string"
        ],
        "boolFilterValues": [
          true
        ],
        "int32FilterValues": [
          0
        ],
        "int64FilterValues": [
          0
        ],
        "stringFilterValues": [
          "string"
        ]
      },
      "rangeFilterParams": {
        "lowerBound": 0,
        "upperBound": 0
      },
      "systemsFilterParams": {
        "systemIds": [
          "string"
        ],
        "systemNames": [
          "string"
        ]
      },
      "timeRangeFilterParams": {
        "dateRange": "Last1Hour",
        "durationHours": 0,
        "lowerBound": 0,
        "upperBound": 0
      }
    }
  ],
  "limit": {
    "from": 0,
    "size": 1
  },
  "sort": [
    {
      "attribute": "string",
      "desc": true
    }
  ],
  "timezone": "string"
}
```

**Example request**

```http
POST /heliosreporting/api/v1/public/components/component1/preview HTTP/1.1
Authorization: Bearer {apiKey}
Content-Type: application/json
Host: <HELIOS_URL>

{
  "filters": [
    {
      "attribute": "string",
      "filterType": "In",
      "inFilterParams": {
        "attributeDataType": "Bool",
        "attributeLabels": [
          "string"
        ],
        "boolFilterValues": [
          true
        ],
        "int32FilterValues": [
          0
        ],
        "int64FilterValues": [
          0
        ],
        "stringFilterValues": [
          "string"
        ]
      },
      "rangeFilterParams": {
        "lowerBound": 0,
        "upperBound": 0
      },
      "systemsFilterParams": {
        "systemIds": [
          "string"
        ],
        "systemNames": [
          "string"
        ]
      },
      "timeRangeFilterParams": {
        "dateRange": "Last1Hour",
        "durationHours": 0,
        "lowerBound": 0,
        "upperBound": 0
      }
    }
  ],
  "limit": {
    "from": 0,
    "size": 1
  },
  "sort": [
    {
      "attribute": "string",
      "desc": true
    }
  ],
  "timezone": "string"
}
```
{: codeblock}

**Example response**

```json
{
  "component": {
    "aggs": {
      "aggregatedAttributes": [
        {
          "aggregationType": "sum",
          "attribute": "string",
          "label": "string"
        }
      ],
      "groupedAttributes": [
        "string"
      ]
    },
    "config": {
      "xlsxParams": {
        "attributeConfig": [
          {
            "attributeName": "string",
            "customLabel": "string",
            "format": "Timestamp"
          }
        ]
      }
    },
    "data": [
      {}
    ],
    "description": "string",
    "filters": [
      {
        "attribute": "string",
        "filterType": "In",
        "inFilterParams": {
          "attributeDataType": "Bool",
          "attributeLabels": [
            "string"
          ],
          "boolFilterValues": [
            true
          ],
          "int32FilterValues": [
            0
          ],
          "int64FilterValues": [
            0
          ],
          "stringFilterValues": [
            "string"
          ]
        },
        "rangeFilterParams": {
          "lowerBound": 0,
          "upperBound": 0
        },
        "systemsFilterParams": {
          "systemIds": [
            "string"
          ],
          "systemNames": [
            "string"
          ]
        },
        "timeRangeFilterParams": {
          "dateRange": "Last1Hour",
          "durationHours": 0,
          "lowerBound": 0,
          "upperBound": 0
        }
      }
    ],
    "id": "string",
    "limit": {
      "from": 0,
      "size": 1
    },
    "name": "string",
    "reportType": "Failures",
    "sort": [
      {
        "attribute": "string",
        "desc": true
      }
    ]
  },
  "filters": [
    {
      "attribute": "string",
      "filterType": "In",
      "inFilterParams": {
        "attributeDataType": "Bool",
        "attributeLabels": [
          "string"
        ],
        "boolFilterValues": [
          true
        ],
        "int32FilterValues": [
          0
        ],
        "int64FilterValues": [
          0
        ],
        "stringFilterValues": [
          "string"
        ]
      },
      "rangeFilterParams": {
        "lowerBound": 0,
        "upperBound": 0
      },
      "systemsFilterParams": {
        "systemIds": [
          "string"
        ],
        "systemNames": [
          "string"
        ]
      },
      "timeRangeFilterParams": {
        "dateRange": "Last1Hour",
        "durationHours": 0,
        "lowerBound": 0,
        "upperBound": 0
      }
    }
  ],
  "generatedTimestampUsecs": 0,
  "lastRefreshTimestampUsecs": 0,
  "timezone": "string"
}
```

---

## List Resources
{: #helios-list-resources}

A `POST` request to `/heliosreporting/api/v1/public/resources` returns different kinds of resources available on {{site.data.keyword.baas_full_notm}}, such as policies, protection groups, registered sources, and message code mappings. These values can be used for filtering options.

**Syntax**

```http
POST /heliosreporting/api/v1/public/resources
```
{: codeblock}

### Request body

```json
{
  "resourceType": "Policies"
}
```

**Example response**

```json
{
  "externalTargets": [
    {
      "id": "string",
      "name": "string",
      "systemId": "string",
      "systemName": "string",
      "targetType": "string"
    }
  ],
  "messageCodeMappings": [
    {
      "messageCode": "string",
      "messageGuid": "string"
    }
  ],
  "policies": [
    {
      "id": "string",
      "isGlobalPolicy": true,
      "name": "string",
      "systemId": "string",
      "systemName": "string"
    }
  ],
  "protectionGroups": [
    {
      "id": "string",
      "name": "string",
      "systemId": "string",
      "systemName": "string"
    }
  ],
  "resourceType": "Policies",
  "sources": [
    {
      "environments": [
        "string"
      ],
      "name": "string",
      "uuid": "string"
    }
  ],
  "tenants": [
    {
      "id": "string",
      "name": "string"
    }
  ]
}
```

---

## List properties of a report type
{: #helios-report-type-properties}

A `GET` request to `/heliosreporting/api/v1/public/reports/report-types/{reportType}` fetches the list of properties for a report type.

**Syntax**

```http
GET /heliosreporting/api/v1/public/reports/report-types/{reportType}
```
{: codeblock}

### Path parameters

| Name       | Type   | Required | Description                  |
|------------|--------|----------|------------------------------|
| `reportType` | String | Yes      | The type of report to query. |
{: caption="Path parameters" caption-side="bottom"}

**Example request**

```http
GET /heliosreporting/api/v1/public/reports/report-types/ProtectionGroupSummary HTTP/1.1
Authorization: Bearer {apiKey}
Host: <HELIOS_URL>
```
{: codeblock}

**Example response**

```json
{
  "attributes": [
    {
      "dataType": "Bool",
      "name": "string"
    }
  ]
}
```

---

## List Reports
{: #helios-list-reports}

A `GET` request to `/heliosreporting/api/v1/public/reports` returns a list of all reports accessible by the logged-in user.

**Syntax**

```http
GET /heliosreporting/api/v1/public/reports
```
{: codeblock}

### Optional query parameters

| Name        | Type   | Description |
|-------------|--------|-------------|
| `ids`         | Array  | Specifies the ids of reports to fetch. |
| `userContext` | String | Specifies the user context to filter reports. |
{: caption="Optional query parameters" caption-side="bottom"}

**Example request**

```http
GET /heliosreporting/api/v1/public/reports HTTP/1.1
Authorization: Bearer {apiKey}
Host: <HELIOS_URL>
```
{: codeblock}

**Example response**

```json
{
  "reports": [
    {
      "category": "Protection",
      "componentIds": [
        "string"
      ],
      "description": "string",
      "id": "string",
      "supportedUserContexts": [
        "IBMBaaS"
      ],
      "title": "string"
    }
  ]
}
```

---

## Fetch a Report
{: #helios-get-report}

A `GET` request to `/heliosreporting/api/v1/public/reports/{id}` fetches a report for a specific id.

**Syntax**

```http
GET /heliosreporting/api/v1/public/reports/{id}
```
{: codeblock}

### Path parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id`   | String | Yes      | The id of the report. |
{: caption="Path parameters" caption-side="bottom"}

**Example request**

```http
GET /heliosreporting/api/v1/public/reports/report1 HTTP/1.1
Authorization: Bearer {apiKey}
Host: <HELIOS_URL>
```
{: codeblock}

**Example response**

```json
{
  "category": "Protection",
  "componentIds": [
    "string"
  ],
  "description": "string",
  "id": "string",
  "supportedUserContexts": [
    "IBMBaaS"
  ],
  "title": "string"
}
```

---

## Preview a Report
{: #helios-preview-report}

A `POST` request to `/heliosreporting/api/v1/public/reports/{id}/preview` fetches a preview of a configured report.

**Syntax**

```http
POST /heliosreporting/api/v1/public/reports/{id}/preview
```
{: codeblock}

### Path parameters

| Name | Type   | Required | Description |
|------|--------|----------|-------------|
| `id`   | String | Yes      | The id of the report. |
{: caption="Path parameters" caption-side="bottom"}

### Request body

```json
{
  "componentIds": [
    "string"
  ],
  "filters": [
    {
      "attribute": "string",
      "filterType": "In",
      "inFilterParams": {
        "attributeDataType": "Bool",
        "attributeLabels": [
          "string"
        ],
        "boolFilterValues": [
          true
        ],
        "int32FilterValues": [
          0
        ],
        "int64FilterValues": [
          0
        ],
        "stringFilterValues": [
          "string"
        ]
      },
      "rangeFilterParams": {
        "lowerBound": 0,
        "upperBound": 0
      },
      "systemsFilterParams": {
        "systemIds": [
          "string"
        ],
        "systemNames": [
          "string"
        ]
      },
      "timeRangeFilterParams": {
        "dateRange": "Last1Hour",
        "durationHours": 0,
        "lowerBound": 0,
        "upperBound": 0
      }
    }
  ],
  "timezone": "string"
}
```

**Example request**

```http
POST /heliosreporting/api/v1/public/reports/report1/preview HTTP/1.1
Authorization: Bearer {apiKey}
Content-Type: application/json
Host: <HELIOS_URL>

{
  "componentIds": [
    "string"
  ],
  "filters": [
    {
      "attribute": "string",
      "filterType": "In",
      "inFilterParams": {
        "attributeDataType": "Bool",
        "attributeLabels": [
          "string"
        ],
        "boolFilterValues": [
          true
        ],
        "int32FilterValues": [
          0
        ],
        "int64FilterValues": [
          0
        ],
        "stringFilterValues": [
          "string"
        ]
      },
      "rangeFilterParams": {
        "lowerBound": 0,
        "upperBound": 0
      },
      "systemsFilterParams": {
        "systemIds": [
          "string"
        ],
        "systemNames": [
          "string"
        ]
      },
      "timeRangeFilterParams": {
        "dateRange": "Last1Hour",
        "durationHours": 0,
        "lowerBound": 0,
        "upperBound": 0
      }
    }
  ],
  "timezone": "string"
}
```
{: codeblock}

**Example response**

```json
{
  "components": [
    {
      "aggs": {
        "aggregatedAttributes": [
          {
            "aggregationType": "sum",
            "attribute": "string",
            "label": "string"
          }
        ],
        "groupedAttributes": [
          "string"
        ]
      },
      "config": {
        "xlsxParams": {
          "attributeConfig": [
            {
              "attributeName": "string",
              "customLabel": "string",
              "format": "Timestamp"
            }
          ]
        }
      },
      "data": [
        {}
      ],
      "description": "string",
      "filters": [
        {
          "attribute": "string",
          "filterType": "In",
          "inFilterParams": {
            "attributeDataType": "Bool",
            "attributeLabels": [
              "string"
            ],
            "boolFilterValues": [
              true
            ],
            "int32FilterValues": [
              0
            ],
            "int64FilterValues": [
              0
            ],
            "stringFilterValues": [
              "string"
            ]
          },
          "rangeFilterParams": {
            "lowerBound": 0,
            "upperBound": 0
          },
          "systemsFilterParams": {
            "systemIds": [
              "string"
            ],
            "systemNames": [
              "string"
            ]
          },
          "timeRangeFilterParams": {
            "dateRange": "Last1Hour",
            "durationHours": 0,
            "lowerBound": 0,
            "upperBound": 0
          }
        }
      ],
      "id": "string",
      "limit": {
        "from": 0,
        "size": 1
      },
      "name": "string",
      "reportType": "Failures",
      "sort": [
        {
          "attribute": "string",
          "desc": true
        }
      ]
    }
  ],
  "filters": [
    {
      "attribute": "string",
      "filterType": "In",
      "inFilterParams": {
        "attributeDataType": "Bool",
        "attributeLabels": [
          "string"
        ],
        "boolFilterValues": [
          true
        ],
        "int32FilterValues": [
          0
        ],
        "int64FilterValues": [
          0
        ],
        "stringFilterValues": [
          "string"
        ]
      },
      "rangeFilterParams": {
        "lowerBound": 0,
        "upperBound": 0
      },
      "systemsFilterParams": {
        "systemIds": [
          "string"
        ],
        "systemNames": [
          "string"
        ]
      },
      "timeRangeFilterParams": {
        "dateRange": "Last1Hour",
        "durationHours": 0,
        "lowerBound": 0,
        "upperBound": 0
      }
    }
  ],
  "generatedTimestampUsecs": 0,
  "id": "string",
  "lastRefreshTimestampUsecs": 0,
  "timezone": "string",
  "title": "string"
}
```

---

## Export a Report
{: #helios-export-report}

A `POST` request to `/heliosreporting/api/v1/public/reports/{id}/export` exports a configured report in PDF, XLS, or CSV format.

**Syntax**

```http
POST /heliosreporting/api/v1/public/reports/{id}/export
```
{: codeblock}

### Path parameters

| Name | Type   | Required | Description                |
|------|--------|----------|----------------------------|
| `id`   | String | Yes      | The id of the report.      |
{: caption="Path parameters" caption-side="bottom"}

### Request body

```json
{
  "async": true,
  "filters": [
    {
      "attribute": "string",
      "filterType": "In",
      "inFilterParams": {
        "attributeDataType": "Bool",
        "attributeLabels": [
          "string"
        ],
        "boolFilterValues": [
          true
        ],
        "int32FilterValues": [
          0
        ],
        "int64FilterValues": [
          0
        ],
        "stringFilterValues": [
          "string"
        ]
      },
      "rangeFilterParams": {
        "lowerBound": 0,
        "upperBound": 0
      },
      "systemsFilterParams": {
        "systemIds": [
          "string"
        ],
        "systemNames": [
          "string"
        ]
      },
      "timeRangeFilterParams": {
        "dateRange": "Last1Hour",
        "durationHours": 0,
        "lowerBound": 0,
        "upperBound": 0
      }
    }
  ],
  "layout": "string",
  "reportFormat": "XLS",
  "timezone": "string"
}
```

**Example request**

```http
POST /heliosreporting/api/v1/public/reports/report1/export HTTP/1.1
Authorization: Bearer {apiKey}
Content-Type: application/json
Host: <HELIOS_URL>

{
  "async": true,
  "filters": [
    {
      "attribute": "string",
      "filterType": "In",
      "inFilterParams": {
        "attributeDataType": "Bool",
        "attributeLabels": [
          "string"
        ],
        "boolFilterValues": [
          true
        ],
        "int32FilterValues": [
          0
        ],
        "int64FilterValues": [
          0
        ],
        "stringFilterValues": [
          "string"
        ]
      },
      "rangeFilterParams": {
        "lowerBound": 0,
        "upperBound": 0
      },
      "systemsFilterParams": {
        "systemIds": [
          "string"
        ],
        "systemNames": [
          "string"
        ]
      },
      "timeRangeFilterParams": {
        "dateRange": "Last1Hour",
        "durationHours": 0,
        "lowerBound": 0,
        "upperBound": 0
      }
    }
  ],
  "layout": "string",
  "reportFormat": "XLS",
  "timezone": "string"
}
```
{: codeblock}

**Example response**

The response is a binary file (PDF, XLS, or CSV) containing the exported report.

---

## List Provider Instances
{: #helios-list-provider-instances}

A `GET` request to `/mcm/provider-instances` lists all provider instances with details. The providers are responsible for managing the lifecycle of instances and this API intends to provide metadata information about the instances.

**Syntax**

```http
GET /mcm/provider-instances
```
{: codeblock}

### Query parameters

| Name                        | Type    | Description                                         |
|-----------------------------|---------|-----------------------------------------------------|
| `instanceIds`                 | Array   | List of Provider Instance IDs to filter on.           |
| `regions`                     | Array   | List of regions to filter Provider Instances on.      |
| `includeServiceInstanceStatus`| Boolean | Indicates whether to include cluster details and software version status.  |
{: caption="Query parameters" caption-side="bottom"}

**Example request**

```http
GET /mcm/provider-instances?regions=us-south&includeServiceInstanceStatus=true HTTP/1.1
Authorization: Bearer {apiKey}
Host: <HELIOS_URL>
```
{: codeblock}

**Example response**

```json
{
  "ibmServiceInstances": [
    {
      "clusterId": 0,
      "clusterIncarnationId": 0,
      "instanceId": "string",
      "lat": 0,
      "lon": 0,
      "name": "string",
      "region": "string",
      "softwareVersion": "string",
      "status": "Active"
    }
  ]
}
```

---

For more information about {{site.data.keyword.baas_full_notm}} Reporting operations, see the [{{site.data.keyword.baas_full_notm}} Reporting documentation](/docs/backup-recovery?topic=backup-recovery-reports_gmc).
