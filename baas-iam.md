---

copyright:
  years: 2025
lastupdated: "2025-07-30"

keywords: authorization, iam, basics, backup and recovery

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Getting Started with IAM
{: #iam}

Access to {{site.data.keyword.baas_full}} service instances for users in your account is controlled by {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM).
{: shortdesc}

## Identity and Access Management roles
{: #iam-roles}

Every user that accesses the {{site.data.keyword.baas_full_notm}} service in your account must be assigned an access policy with an IAM user role defined. That policy determines what actions the user can perform within the context of the service or instance you select. The allowable actions are customized and defined by the {{site.data.keyword.cloud_notm}} service as operations that are allowed to be performed on the service. The actions are then mapped to IAM user roles.

Policies enable access to be granted at different levels. Some of the options include the following:

* Access across all instances of the service in your account
* Access to an individual service instance in your account
* Access to all IAM-enabled services in your account

After you define the scope of the access policy, you assign a role. Review the following tables which outline what actions each role allows within the {{site.data.keyword.baas_full_notm}} service.

The following table details actions that are mapped to platform management roles. Platform management roles enable users to perform tasks on service resources at the platform level, for example assign user access for the service, create or delete service IDs, create instances, and bind instances to applications.

| Platform management role | Description of actions                                                                                                             | Example actions                                                                                                         |
| :----------------------- | :--------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------- |
| Viewer                   | View service instances but not modify them                                                                                         | <ul><li>List available BRS service instances</li><li>View BRS service plan details</li><li>View usage details</li></ul> |
| Editor                   | Perform all platform actions except for managing the accounts and assigning access policies                                        | <ul><li>Create and delete BRS service instances</li></ul>                                                               |
| Operator                 | Not used by Backup and Recovery                                                                                                                    | None                                                                                                                    |
| Administrator            | Perform all platform actions based on the resource this role is being assigned, including assigning access policies to other users, as well as setting PublicAccess policy on buckets. | <ul><li>Update user policies</li><li>Update pricing plans</li></ul>                                                              |
{: caption="IAM user roles and actions"}


The following table details actions that are mapped to service access roles. Service access roles enable users access to {{site.data.keyword.baas_full_notm}} as well as the ability to call the {{site.data.keyword.baas_full_notm}} API.

| Service access role | Description of actions                                                                                                | Example actions                                                                                             |
| :------------------ | :-------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------- |
| Reader              | Readers will be able to view the sources, protection groups and recoveries.                                           |  View Protection Groups, View Protection Policies, View Protection Sources                                                                            |
| Writer              | In addition to Reader actions,Writers will be able to Modify protection groups, policies,  add sources and perform restore .  | Modify Protection Groups, Modify Protection Policies, Modify Protection Sources                  |
| Manager             | In addition to Writer actions, Managers can complete privileged actions including modifying data lock, deleting snapshots etc,. | Delete Resources, Modify Data Lock                                                                                                  |
{: caption="IAM service access roles and actions"}

For information about assigning user roles in the UI, see [Managing IAM access](/docs/account?topic=account-assign-access-resources).

## Identity and Access Management actions
{: #iam-actions}

| Action id                                                        | Description                                                                         |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| `backup-recovery.dashboard.view`                                 | View Protection, Sources.                                                           |
| `backup-recovery.dashboard.edit`                                 | Edit Protection, Sources, Recoveries                                                |
| `backup-recovery.dashboard.manage`                               | Manage Datalock, Deletion                                                           |
{: caption="Granular IAM action descriptions"}

