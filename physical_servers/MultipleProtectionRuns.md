---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-05"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Start on-demand multiple protection runs for physical server
{: #start_on-demand_multiple_protection_runs_for_physical_server}

After creating a directive file-based Protection Group for a physical server, IBM Cloud Supports to simultaneously trigger more than one protection runs of the Protection Group to backup only a specific set of files and folders in each run. This functionality can be achieved using the {{site.data.keyword.baas_full_notm}} REST API, `/public/protectionJobs/run/{id}`.

To trigger a protection run, add the `metadata file path` parameter in the JSON body of the API and provide the directive file path having the file or folder you plan to backup as the value.

Example to trigger a protection run from using the REST API:

### Uri
{: #uri}

https://<cluster\_name>/irisservices/api/v1/public/protectionJobs/run/<protectionGroupID>

### Method
{: #method}

POST

### Json body
{: #json_body}

{
 "copyRunTargets": \[
 \],
 "runNowParameters": \[
   {
     "physicalParams": {
       "metadataFilePath": "/home/cohesity/dirfile2"
     },
     "sourceId": 5
   }
 \],
 "runType": "kFull"
}

Where **/home/cohesity/dirfile2** is the directive file path that contains a list of files and folders folder that will be backed up using this API.

Similarly, you can simultaneously trigger multiple protection runs of a Protection Group.

## Limitation
{: #limitation}

You cannot simultaneously trigger multiple protection runs to back up the same file or folder.

For example, you cannot start a protection run with the directive file path as **/home/cohesity/dirfile2** if there is already another active protection run of the same Protection Group, which is using the same directive file path.
