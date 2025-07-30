---

copyright:
  years: 2025
lastupdated: "2025-07-30"

keywords:

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Activity tracking events for {{site.data.keyword.baas_full_notm}}
{: #at_events}


{{site.data.keyword.cloud_notm}} services, such as backup-recovery, generate activity tracking events.
{: shortdesc}

Activity tracking events report on activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use the events to investigate abnormal activity and critical actions and to comply with regulatory audit requirements.

You can use {{site.data.keyword.atracker_full_notm}}, a platform service, to route auditing events in your account to destinations of your choice by configuring targets and routes that define where activity tracking events are sent. For more information, see [About {{site.data.keyword.atracker_full_notm}}](/docs/atracker?topic=atracker-about).

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.


## Locations where activity tracking events are sent by {{site.data.keyword.atracker_full_notm}}
{: #atracker-locations}



The backup-recovery service sends activity tracking events by {{site.data.keyword.atracker_full_notm}} in the regions that are indicated in the following table.

| Dallas (`us-south`) | Washington (`us-east`)  | Toronto (`ca-tor`) | Sao Paulo (`br-sao`) |
|---------------------|-------------------------|-------------------|----------------------|
| [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} |
{: caption="Regions where activity tracking events are sent in Americas locations" caption-side="top"}
{: #atracker-table-1}
{: tab-title="Americas"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}

| Tokyo (`jp-tok`)    | Sydney (`au-syd`) |  Osaka (`jp-osa`) | Chennai (`in-che`) |
|---------------------|------------------|------------------|--------------------|
| [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} |
{: caption="Regions where activity tracking events are sent in Asia Pacific locations" caption-side="top"}
{: #atracker-table-2}
{: tab-title="Asia Pacific"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) | Madrid (`eu-es`) |
|---------------------------------------------------------------|---------------------|------------------|
| [No]{: tag-red} | [No]{: tag-red} | [No]{: tag-red} |
{: caption="Regions where activity tracking events are sent in Europe locations" caption-side="top"}
{: #atracker-table-3}
{: tab-title="Europe"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}


## Viewing activity tracking events for backup-recovery
{: #at-viewing}



You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.


### Launching {{site.data.keyword.logs_full_notm}} from the Observability page
{: #log-launch-standalone}



For information on launching the {{site.data.keyword.logs_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.logs_full_notm}} documentation.](/docs/cloud-logs?topic=cloud-logs-instance-launch)


## List of platform events
{: #at_actions_platform}



The following table lists the activity tracking event actions that the {{site.data.keyword.cloud_notm}} platform generates backup-recovery instances are processed.

| Action                                   | Description |
|------------------------------------------|---------|
| `backup-recovery.instance.create`           | An event is generated when you provision a service instance. |
| `backup-recovery.instance.update`           | An event is generated when you rename a service instance or when you change the service plan. |
| `backup-recovery.instance.delete`           | An event is generated when a service instance is deleted. |
| `backup-recovery.instance.schedule_reclaim` | An event is generated when a service instance is pending_reclamation. |
| `backup-recovery.instance.restore`          | An event is generated when a service instance is restored. |
{: caption="Actions that generate platform events" caption-side="bottom"}

The following table lists the actions that generate an event for managing service credentials that are associated with a service instance.

| Action                         | Description |
|--------------------------------|---------|
| `backup-recovery.key.create` | An event is generated when an API key is created for a service instance through the *Service credentials* section of the service instance UI. |
| `backup-recovery.key.delete` | An event is generated when an API key that is associated with a service instance is deleted from the *Service credentials* section of the service instance UI. |
{: caption="Actions that generate service credentials events" caption-side="bottom"}


## List of management events
{: #at_actions}



| Action                          | API Request              | Description      |
|---------------------------------|--------------------------|------------------|
| `backup-recovery.agent.download` | `POST /v2/data-protect/agents/download` | Download the backup agent |
| `backup-recovery.agent.upgrade` | `POST /v2/data-protect/agents/upgrade-tasks` | Upgrade the backup agent |
| `backup-recovery.agent-upgrade-task.update` | `POST /v2/data-protect/agents/upgrade-tasks/actions` | Retry or Cancel the upgrade agent task |
| `backup-recovery.protection-policy.create` | `POST /v2/data-protect/policies` | Create a new Protection Policy |
| `backup-recovery.protection-policy.delete` | `DELETE /v2/data-protect/policies/.+` | Delete a particular policy |
| `backup-recovery.protection-policy.update` | `PUT /v2/data-protect/policies/.+` | Update a particular policy |
| `backup-recovery.protection-group-run.create` | `POST /v2/data-protect/protection-groups/.+/runs[/]?$` | Create a new run under the protection group |
| `backup-recovery.protection-group-run.update` | `PUT /v2/data-protect/protection-groups/.+/runs[/]?$` | Update runs of a particular protection group |
| `backup-recovery.protection-group-run-action.update` | `POST /v2/data-protect/protection-groups/.+/runs/actions[/]?$` | Perform various actions on a Protection Group run like pause, resume and cancel |
| `backup-recovery.protection-group.create` | `POST /v2/data-protect/protection-groups` | Create a protection group |
| `backup-recovery.protection-group-state.update` | `POST /v2/data-protect/protection-groups/states` | Perform an action like pause, resume, active, deactivate on all specified Protection Groups |
| `backup-recovery.protection-group.delete` | `DELETE /v2/data-protect/protection-groups/.+` | Delete a Protection Group |
| `backup-recovery.protection-group.update` | `PUT /v2/data-protect/protection-groups/.+` | Update a Protection Group |
| `backup-recovery.recover-task.restore` | `POST /v2/data-protect/recoveries` | Perform a Recovery |
| `backup-recovery.recover-task.restore` | `POST /v2/data-protect/recoveries/download-files-folders` | Creates a download files and folders recovery |
| `backup-recovery.recover-task.cancel` | `POST /v2/data-protect/recoveries/.+/cancel[/]?$` | Cancel Recovery for a given id |
| `backup-recovery.recover-task.download` | `GET /v2/data-protect/recoveries/.+/download-files[/]?$` | Download files from the given download file recovery |
| `backup-recovery.recover-task.download` | `GET /v2/data-protect/snapshots/.+/download-file[/]?$` | Download an indexed file from a snapshot |
| `backup-recovery.protection-source.register` | `POST /v2/data-protect/sources/registrations` | Register a source |
| `backup-recovery.protection-source.deregister` | `DELETE /v2/data-protect/sources/registrations/\d+[/]?$` | Unregister a source |
| `backup-recovery.protection-source.update` | `PUT /v2/data-protect/sources/registrations/\d+[/]?$` | Update a registered source |
| `backup-recovery.protection-source.refresh` | `POST /v2/data-protect/sources/\d+/refresh[/]?$` | Refresh a protection source |
| `backup-recovery.data-source-connection.create` | `POST /v2/data-source-connections` | Create a data-source connection |
| `backup-recovery.data-source-connection.delete` | `DELETE /v2/data-source-connections/.+` | Delete a particular data-source connection |
| `backup-recovery.data-source-connection.update` | `PATCH /v2/data-source-connections/.+` | Patch a data-source connection |
| `backup-recovery.data-source-connection.activate` | `POST /v2/data-source-connections/.+/registrationToken[/]?$` | Generate registration token for a data-source connection |
| `backup-recovery.data-source-connector.update` | `PATCH /v2/data-source-connectors/.+` | Patch a data-source connector |
| `backup-recovery.data-source-connector.delete` | `DELETE /v2/data-source-connectors/.+` | Delete a data-source connector |
{: caption="Actions that generate management events" caption-side="bottom"}

