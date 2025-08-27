---

copyright:
  years: 2017, 2024
lastupdated: "2025-08-27"

keywords: object storage, sdk, overview

subcollection: cloud-object-storage


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

# About {{site.data.keyword.baas_full}} SDKs
{: #sdk-about}

{{site.data.keyword.baas_full}} provides SDKs for Go featuring capabilities to make the most of {{site.data.keyword.baas_full_notm}}.
{: shortdesc}

These SDKs are based on the official AWS S3 API SDKs, but are modified to use {{site.data.keyword.cloud}} features like Data Protect and SAAS Connector.

| Feature                                             | Java                                              | Python                                            | NodeJS                                            | GO                                                | CLI                                               |  Terraform                                              |
|-----------------------------------------------------|---------------------------------------------------|---------------------------------------------------|---------------------------------------------------|---------------------------------------------------|---------------------------------------------------|---------------------------------------------------|
| [Data protect](#sdk-data-protect)               |  |  |  | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |![Checkmark icon](../../icons/checkmark-icon.svg)|
| [SAAS connector](#sdk-saas-connector)        |  |  |  | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
{: caption="Features supported per SDK"}

## Data protect
{: #sdk-data-protect}

DataProtect leverage a comprehensive set of APIs to enable programmatic access and automation of data protection and management operations.

## SAAS connector
{: #sdk-saas-connector}

Allows connector configuration and registration along with logs and status retrieval.
