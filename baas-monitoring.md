---

copyright:
  years: 2025
lastupdated: "2025-07-30"

keywords: backup-recovery observability

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}


# Monitoring metrics for backup-recovery
{: #monitoring}

{{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.baas_full_notm}}, generate platform metrics that you can use to gain operational visibility into the performance and health of the service in your account.
{: shortdesc}

{{site.data.keyword.mon_full_notm}} is a third-party cloud-native, and container-intelligence management system that can be included as part of your {{site.data.keyword.cloud_notm}} architecture. It offers administrators, DevOps teams, and developers full-stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards. For more information, see [Monitoring in IBM Cloud](/docs/monitoring?topic=monitoring-about-monitor).

You can use {{site.data.keyword.metrics_router_full_notm}}, a platform service, to route platform metrics in your account to a destination of your choice by configuring targets and routes that define where platform metrics are sent. For more information, see [About {{site.data.keyword.metrics_router_full_notm}} in {{site.data.keyword.cloud_notm}}](/docs/metrics-router?topic=metrics-router-about).

You can use {{site.data.keyword.mon_full}} to visualize and alert on metrics that are generated in your account and routed by {{site.data.keyword.metrics_router_full_notm}} to an {{site.data.keyword.mon_full_notm}} instance.

## Locations where metrics are generated
{: #mon-locations}



The backup-recovery service sends metrics in the regions that are indicated in the following table.

| Dallas (`us-south`) | Washington (`us-east`)  | Toronto (`ca-tor`) | Sao Paulo (`br-sao`) |
|---------------------|-------------------------|-------------------|----------------------|
| [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} |
{: caption="Regions where metrics are sent in Americas locations" caption-side="top"}
{: #mon-table-1}
{: tab-title="Americas"}
{: tab-group="mon"}
{: class="simple-tab-table"}
{: row-headers}

| Tokyo (`jp-tok`)    | Sydney (`au-syd`) |  Osaka (`jp-osa`) | Chennai (`in-che`) |
|---------------------|------------------|------------------|--------------------|
| [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} |
{: caption="Regions where metrics are sent in Asia Pacific locations" caption-side="top"}
{: #mon-table-2}
{: tab-title="Asia Pacific"}
{: tab-group="mon"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) | Madrid (`eu-es`) |
|---------------------------------------------------------------|---------------------|------------------|
| [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} |
{: caption="Regions where metrics are sent in Europe locations" caption-side="top"}
{: #mon-table-3}
{: tab-title="Europe"}
{: tab-group="mon"}
{: class="simple-tab-table"}
{: row-headers}


## Enabling platform metrics for {{site.data.keyword.baas_full_notm}}
{: #monitoring-enable}



## Viewing metrics
{: #monitoring-view}

To monitor backup-recovery metrics, you must launch the {{site.data.keyword.mon_full_notm}} web UI for the instance that is enabled for platform metrics in the region where your backup-recovery instance is provisioned.
{: important}

### Launching {{site.data.keyword.mon_full}} from the {{site.data.keyword.baas_full_notm}} dashboard
{: #monitoring-view-ui}



### Launching {{site.data.keyword.mon_full}} from the Observability page
{: #monitoring-view-ob}

For more information about launching the {{site.data.keyword.mon_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.mon_full_notm}} documentation.](/docs/monitoring?topic=monitoring-launch)

## Monitoring backup-recovery
{: #monitoring-monitor}



### _Add entries per recommendation to monitor your service_
{: #monitoring-monitor-1}



## serviceName predefined dashboards
{: #monitoring-dashboards}

After you configure your {{site.data.keyword.mon_short}} instance to receive platform metrics, complete the following steps:

1. Go to the [monitoring dashboard](/observe/monitoring){: external} and find your Monitoring instance that is configured to receive platform metrics.
2. In the **View Dashboard** column, click **View {{site.data.keyword.mon_short}}**.
3. Once you are in the {{site.data.keyword.mon_short}} platform, click **Dashboards** on the side menu.
4. Select **IBM** under the **Dashboard Templates** section.
5. Select **{{site.data.keyword.baas_full_notm}} - Overview** to view the dashboard for your {{site.data.keyword.baas_full_notm}} instance.

You are able to see any metrics in your {{site.data.keyword.mon_short}} instance by default once you provision a {{site.data.keyword.baas_full_notm}} instance and make API requests to it.
{: note}

## Metrics available by Service Plan
{: #monitoring-metrics-by-plan}

| Metric Name |
|-----------|
| [Total Backup Jobs Count](#ibm_backup_recovery_backup_jobs_count) | 
| [Total Consumed Storage in Bytes](#ibm_backup_recovery_consumed_storage) | 
| [Total Failed Backup Jobs Count](#ibm_backup_recovery_failed_backup_jobs_count) | 
| [Total Failed Recovery Jobs Count](#ibm_backup_recovery_failed_recovery_jobs_count) | 
| [Total Protected Objects Count](#ibm_backup_recovery_protected_objects_count) | 
| [Total Recovery Jobs Count](#ibm_backup_recovery_recovery_jobs_count) | 
| [Total Registered Objects Count](#ibm_backup_recovery_registered_objects_count) | 
| [Total Running Backup Jobs Count](#ibm_backup_recovery_running_backup_jobs_count) | 
| [Total Running Recovery Jobs Count](#ibm_backup_recovery_running_recovery_jobs_count) | 
| [Total Successful Backup Jobs Count](#ibm_backup_recovery_successful_backup_jobs_count) | 
| [Total Successful Recovery Jobs Count](#ibm_backup_recovery_successful_recovery_jobs_count) | 
{: caption="Table 1: Metrics Available by Plan Names" caption-side="top"}

### Total Backup Jobs Count
{: #ibm_backup_recovery_backup_jobs_count}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_backup_jobs_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 2: Total Backup Jobs Count metric metadata" caption-side="top"}

### Total Consumed Storage in Bytes
{: #ibm_backup_recovery_consumed_storage}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_consumed_storage`|
| `Metric Type` | `gauge` |
| `Value Type`  | `byte` |
| `Segment By` | `Service instance` |
{: caption="Table 3: Total Consumed Storage in Bytes metric metadata" caption-side="top"}


### Total Failed Backup Jobs Count
{: #ibm_backup_recovery_failed_backup_jobs_count}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_failed_backup_jobs_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 5: Total Failed Backup Jobs Count metric metadata" caption-side="top"}

### Total Failed Recovery Jobs Count
{: #ibm_backup_recovery_failed_recovery_jobs_count}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_failed_recovery_jobs_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 6: Total Failed Recovery Jobs Count metric metadata" caption-side="top"}

### Total Protected Objects Count
{: #ibm_backup_recovery_protected_objects_count}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_protected_objects_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 7: Total Protected Objects Count metric metadata" caption-side="top"}

### Total Recovery Jobs Count
{: #ibm_backup_recovery_recovery_jobs_count}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_recovery_jobs_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 8: Total Recovery Jobs Count metric metadata" caption-side="top"}

### Total Registered Objects Count
{: #ibm_backup_recovery_registered_objects_count}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_registered_objects_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 9: Total Registered Objects Count metric metadata" caption-side="top"}

### Total Running Backup Jobs Count
{: #ibm_backup_recovery_running_backup_jobs_count}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_running_backup_jobs_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 10: Total Running Backup Jobs Count metric metadata" caption-side="top"}

### Total Running Recovery Jobs Count
{: #ibm_backup_recovery_running_recovery_jobs_count}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_running_recovery_jobs_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 11: Total Running Recovery Jobs Count metric metadata" caption-side="top"}

### Total Successful Backup Jobs Count
{: #ibm_backup_recovery_successful_backup_jobs_count}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_successful_backup_jobs_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 12: Total Successful Backup Jobs Count metric metadata" caption-side="top"}

### Total Successful Recovery Jobs Count
{: #ibm_backup_recovery_successful_recovery_jobs_count}



| Metadata | Description |
|----------|-------------|
| `Metric Name` | `ibm_backup_recovery_successful_recovery_jobs_count`|
| `Metric Type` | `gauge` |
| `Value Type`  | `none` |
| `Segment By` | `Service instance` |
{: caption="Table 13: Total Successful Recovery Jobs Count metric metadata" caption-side="top"}


## Attributes for segmentation
{: #monitoring-attributes}

### Global attributes
{: #monitoring-attributes-global}

The following attributes are available for segmenting all of the metrics listed above

| Attribute | Attribute Name | Attribute Description |
|-----------|----------------|-----------------------|
| `Cloud Type` | `ibm_ctype` | The cloud type is a value of public, dedicated or local |
| `Location` | `ibm_location` | The location of the monitored resource - this may be a region, data center or global |
| `Resource` | `ibm_resource` | The resource being measured by the service - typically a identifying name or GUID |
| `Resource Type` | `ibm_resource_type` | The type of the resource being measured by the service |
| `Resource group` | `ibm_resource_group_name` | The resource group where the service instance was created |
| `Scope` | `ibm_scope` | The scope is the account, organization or space GUID associated with this metric |
| `Service name` | `ibm_service_name` | Name of the service generating this metric |
{: caption="Table 14: Global attributes" caption-side="top"}
