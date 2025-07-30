---

copyright:
  years: 2024
lastupdated: "2025-07-30"

keywords: metadata, reference, api

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}


# Common headers and error codes
{: #compatibility-common}

Data transfers use many standard protocols and have unique requirements. Keep up-to-date with the reference to common headers and some error codes.
{: shortdesc}

## Common Request Headers
{: #compatibility-request-headers}

The following table describes supported common request headers. {{site.data.keyword.cos_full}} ignores any common headers that are not listed below if sent as part of a request, although some requests might support extra headers as defined in this document.

| Header | Note |
|------|-------------|
| Authorization | Required for all requests (OAuth2 bearer token). |
| X-IBM-Tenant-ID | Tenant ID |
| Content-Type|  application/json |
{: caption="Common Request Headers" caption-side="top"}

## Error Codes
{: #compatibility-errors}

| Error Code | Description | HTTP Status Code |
|------|-------------|-------------|
| KValidationError | request body has an error | 400 |
| KValidationError | If either of incremental or full schedule is specified then retention must be set | 400 |
| BXNIM0415E | Provided API key could not be found | 400 |
| KStatusUnauthorized | Unauthorized | 401 |
| KAccessDenied | Access Denied | 403 |
| KEntityNotExistsError | Resource not found | 404 |
| KDuplicateError | already exists in organization | 409 |
| KInternalError | An internal critical error has occurred | 500 |
| KInternalError | Resource already exists for tenant | 500 |
| KInternalError | In NextGen CE, only CAD policies are allowed | 500 |
{: caption="Error Codes" caption-side="top"}
