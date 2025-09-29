---

copyright:
  years:  2025
lastupdated: "2025-08-27"

keywords:

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Deleting a service instance
{: #instance-remove}

You can remove an {{site.data.keyword.logs_full_notm}} instance by using the {{site.data.keyword.cloud_notm}} UI, by using the command line, or by using a Terraform script.
{: shortdesc}


## Before you remove your instance
{: #prereq_remove}

Before you remove an {{site.data.keyword.logs_full_notm}} instance from the {{site.data.keyword.cloud_notm}}, clean up resources associated with the instance:

1. Make note of the sources that forwarded logs to the {{site.data.keyword.logs_full_notm}} instance that you removed and, if appropriate, remove the logging agent from each source.
2. Remove permissions that are granted to users to work with the instane being deleted.

    If you manage access by using dedicated access groups to work with a specific instance, you must remove these access groups.

    If you manage access to multiple logging instances by using access groups, you must remove the policies that grant permissions to the instance you are deleting.

    If you grant individual policies to users, you must gather the list of users that had  permissions to work with the deleted instance. Then, for each user, you must remove the policies that relate to the instance that you are deleting.


## Removing an instance through the {{site.data.keyword.cloud_notm}} UI
{: #remove_ui}
{: ui}

To remove an instance of {{site.data.keyword.logs_full_notm}} by using the {{site.data.keyword.cloud_notm}} UI, complete the following steps:

1. [Log in to your {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/login){: external}.

	After you log in, the {{site.data.keyword.cloud_notm}} UI opens.

2. Go to the menu icon ![menu icon](/icons/icon_hamburger.svg) &gt; **Observability** to access the *Observability* Dashboard.

3. Click **Logging**. The list of logging instances is displayed. You might need to click the **Cloud Logs** tab to see your {{site.data.keyword.logs_full_notm}} instances.

4. Click the **Actions** icon ![Actions icon](/icons/action-menu-icon.svg) next to the instance that you want to delete.

5. Type the name of instance to confirm.

6. Click **Delete**.


## Removing an instance through the CLI
{: #remove_cli}
{: cli}

To remove an instance of {{site.data.keyword.logs_full_notm}} through the command line, complete the following steps:

1. [Pre-requisite] Install the {{site.data.keyword.cloud_notm}} CLI.

   For more information, see [Installing the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).

2. Log in to {{site.data.keyword.cloud_notm}}. Run the following command: [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login)

3. Get information about the instance that you plan to delete.

    ```text
    ibmcloud resource service-instance INSTANCE-NAME --output JSON
    ```
    {: codeblock}

4. Remove any service key associated with the instance.

    Find the service keys that are associated with an instance:

    ```text
    ibmcloud resource service-keys --instance-id INSTANCE-GUID
    ```
    {: codeblock}

    Run the following command to delete 1 service key. You must delete all keys associated with the instance to be deleted.

    ```text
    ibmcloud resource service-key-delete SERVICE-KEY-NAME
    ```
    {: codeblock}

5. Remove the instance. Run the [ibmcloud resource service-instance-delete](/docs/cli?topic=cli-ibmcloud_commands_resource#ibmcloud_resource_service_instance_delete) command:

    ```text
    ibmcloud resource service-instance-delete NAME
    ```
    {: codeblock}

    Where NAME is the name of the instance.

    For example, to remove the `logging-instance-01` instance, run the following command:

    ```text
    ibmcloud resource service-instance-delete logging-instance-01
    ```
    {: codeblock}

## Removing an instance using Terraform
{: #remove_terraform}
{: terraform}

See [Setting up Terraform](/docs/cloud-logs?topic=cloud-logs-terraform-setup) for information on removing instances using Terraform.

## Instance cleanup
{: #instance_cleanup}

The process of removing a instance from a service entails the elimination of both data and metadata associated with the instance. This involves the following key steps:

1.	Removal of instance snapshots data:
- All snapshots of the instance's data must be deleted from the service to ensure no residual information remains within the system.
- The snapshots must also be permanently removed from the associated COS (Cloud Object Storage) bucket to free up external storage resources.

1.	Unregistration of instance sources: All sources registered under the instance must be unregistered from the service. This step ensures that the instance is fully detached from the service's resources and operations.

1.	Deletion of instance-specific policies: Any policies configured specifically for the instance must be identified and deleted. This includes access controls, operational policies, or any other settings unique to the instance.

1.	External resource cleanup: After the in-service cleanup is complete, IBM must delete external resources associated with the instance. This includes the COS bucket, SaaS connectors, and any other external infrastructure tied to the instance.

This method serves as a temporary solution and does not delete all the metadata of the instance from the service. Once the instance deactivation and suspension functionalities are fully implemented, IBM should discontinue using this process. The new functionality will provide a more streamlined and robust approach for managing instance lifecycles.
{: important}

### Instance cleanup script
{: #instance_cleanup_script}

#### Overview
{: #instance_cleanup_script_overview}

Python script provided below automates the cleanup process for a specified instance by deleting the following data:

1.	Protection Jobs

2.	Policies

3.	Sources

It also involves adjusting certain configurations (GFlags) to facilitate the cleanup and restore them afterward.

#### Requirements
{: #instance_cleanup_script_requirements}

- Python Version: Python 3.x
- Libraries: Standard Python libraries (os, re, sys, time, logging, json, urllib, ssl, subprocess).
- Dependencies: iris_cli binary for updating GFlags.

#### Usage
{: #instance_cleanup_script_usage}

Command-Line Example
```sh
python3 instance_cleanup.py --instanceId "<instance_id>" --username "<username>" --password "<password>"
```
Arguments
- instanceId: The unique identifier of the instance to clean up.
- username: The username of admin.
- password: The password of admin.

If required arguments are missing, the script logs an error and exits.

Example command:
```sh
python3 instance_cleanup.py --instanceId "a05f1ea9dd/" --username "admin" --password "P@ssword"
```
Script Execution Flow
1. Set Up Logging: Configures a logger to output detailed logs to stdout.
2. Initialize instance Object: Parses arguments, sets up class variables.
3. Authentication: Fetches an access token using the provided credentials.
4. Instance info retrieval:
   - Fetches detailed instance data.
   - Retrieves registration and environment details.
   - If the instance is not active or sources don’t exist, performs partial cleanup and exits gracefully.
5. Pre-Setup (GFlag Configuration):
   - Captures current GFlag values for backup.
   - Updates GFlags to enable easier cleanup operations.
   - Sets application modes, if applicable.
6. Cleanup Operations:
   - Deletes backup jobs.
   - Unregisters all associated sources.
   - Deletes policies tied to the instance.
7. Final GFlag Restorations (Post Cleanup):
   - Restores original GFlag values.
   - Resets application modes.
8. Completion:
   - Logs total runtime.
   - Exits with appropriate status.

#### List of APIs called during execution
{: #instance_cleanup_script_list_of_apis}

Below is a list of all the API endpoints it calls, along with their general purposes:

1.  Access token retrieval

    Endpoint: POST https://{{service}}/irisservices/api/v1/public/accessTokens
    Purpose: Authenticates using provided credentials and obtains a Bearer token.
    In all the following APIs we call with the following headers:
    ```sh
    {
    "Authorization": "Bearer "+ <access_token>,
        "X-IMPERSONATE-INSTANCE-ID": <instance_id>
    }
    ```

2.  Instance information

    Endpoint: GET https://{{service}}/irisservices/api/v1/public/instances?ids=<instanceId>&properties=ProtectionPolicy
    Purpose: Retrieves details about the specified instance, including name, description, and instance ID validation.
3.  Registration Information

    Endpoint: GET https://{{service}}/irisservices/api/v1/public/protectionSources/registrationInfo?includeEntityPermissionInfo=true&includeApplicationsTreeInfo=false
    Purpose: Fetches the registration info for all sources under the instance. To curate list of sources that needs to be unregistered.
4.  Backup Jobs

    - List Backup Jobs:

    Endpoint: GET https://{{service}}/irisservices/api/v1/public/protectionJobs
    Purpose: Lists all backup jobs associated with the instance.
    - Delete Backup Jobs:

    Endpoint: DELETE https://{{service}}/irisservices/api/v1/public/protectionJobs/<jobId>
      - We call it with { "deleteSnapshots": True } to delete all the snapshots associated with the job.
      - Purpose: Deletes specific backup jobs based on their job ID.
5.  Protection Sources (Unregistration)

    Endpoint: DELETE https://{{service}}/irisservices/api/v1/public/protectionSources/<source_id>
    Purpose: Unregisters individual sources associated with the instance.

6.  Policies

    - List Policies:

    Endpoint: GET https://{{service}}/irisservices/api/v1/public/protectionPolicies?instanceIds=<instanceId>
    Purpose: Retrieves a list of all policies associated with the instance.
    - Delete Policies:

    Endpoint: DELETE https://{{service}}/irisservices/api/v1/public/protectionPolicies/<policyId>
    Purpose: Deletes specified policies linked to the instance.



