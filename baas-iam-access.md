---

copyright:
  years:  2017, 2022
lastupdated: "2025-07-30"

keywords: authorization, iam, basics

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}


# Managing IAM access for _{{site.data.keyword.baas_full_notm}}_
{: #iam-docs-template}

Access to {{site.data.keyword.baas_full_notm}} service instances for users in your account is controlled by {{site.data.keyword.cloud}} Identity and Access Management (IAM). Every user that accesses the {{site.data.keyword.baas_full_notm}} service in your account must be assigned an access policy with an IAM role. Review the following roles, actions, and more to help determine the best way to assign access to {{site.data.keyword.baas_full_notm}}.
{: shortdesc}

The access policy that you assign users in your account determines what actions a user can perform within the context of the service or specific instance that you select. The allowable actions are customized and defined by the {{site.data.keyword.baas_full_notm}} as operations that are allowed to be performed on the service. Each action is mapped to an IAM platform or service role that you can assign to a user.

If a specific role and its actions don't fit the use case that you're looking to address, you can [create a custom role](/docs/account?topic=account-custom-roles#custom-access-roles) and pick the actions to include.
{: tip}

IAM access policies enable access to be granted at different levels. Some of the options include the following:

* Access across all instances of the service in your account
* Access to an individual service instance in your account 

Review the following tables that outline what types of tasks each role allows for when you're working with the {{site.data.keyword.baas_full_notm}} service. Platform management roles enable users to perform tasks on service resources at the platform level, for example, assign user access to the service, create or delete instances, and bind instances to applications. Service access roles enable users access to {{site.data.keyword.baas_full_notm}} and the ability to call the {{site.data.keyword.baas_full_notm}}'s API. For information about the exact actions that are mapped to each role, see [{{site.data.keyword.baas_full_notm}}](_YourSubHeadingLink_).






| Platform role |  Description of actions |
|---------------|-------------------------|
| Viewer                 |  As a viewer, you can view service instances, but you can't modify them. |
| Operator               |  As an operator, you can perform platform actions required to configure and operate service instances, such as viewing a service's dashboard.            |
| Editor                 |  As an editor, you can perform all platform actions except for managing the account and assigning access policies.            |
| Administrator          |  As an administrator, you can perform all platform actions based on the resource this role is being assigned, including assigning access policies to other users.            |
{: row-headers}
{: class="simple-tab-table"}
{: caption="Table 1. IAM platform roles" caption-side="bottom"}
{: #iamrolesplatform}
{: tab-title="Platform roles"}
{: tab-group="IAM"}

| Service role |  Description of actions |
|--------------|------------------------|
| Reader         | As a reader, you can view backup jobs, policies, sources.   |
| Writer         | As a writer, you have permissions beyond the reader role, including creating and editing backup connectors, agent upgrades, backup jobs and restores.            |
| Manager        | As a manager, you have permissions beyond the writer role to complete privileged actions such as managing Protection sources, policies and groups.            |
{: row-headers}
{: class="simple-tab-table"}
{: caption="Table 1. IAM service access roles" caption-side="bottom"}
{: #iamrolesservice}
{: tab-title="Service roles"}
{: tab-group="IAM"}

## Assigning access to {{site.data.keyword.baas_full_notm}} in the console
{: #assign-access-console}
{: ui}

There are two common ways to assign access in the console:

* Access policies per user. You can manage access policies per user from the **Manage** > **Access (IAM)** > **Users** page in the console. For information about the steps to assign IAM access, see [Managing access to resources in the console](/docs/account?topic=account-assign-access-resources&interface=ui#access-resources-console).
* Access groups. Access groups are used to streamline access management by assigning access to a group once, then you can add or remove users as needed from the group to control their access. You manage access groups and their access from the **Manage** > **Access (IAM)** > **Access groups** page in the console. For more information, see [Assigning access to a group in the console](/docs/account?topic=account-groups&interface=ui#access_ag).


## Assigning access to {{site.data.keyword.baas_full_notm}} in the CLI
{: #assign-access-cli}
{: cli}

For step-by-step instructions for assigning, removing, and reviewing access, see [Assigning access to resources by using the CLI](/docs/account?topic=account-assign-access-resources&interface=cli#access-resources-cli). The following example shows a command for assigning the `<Reader>` role for `<{{site.data.keyword.baas_full_notm}}>`:

Use `backup-recovery` for the service name. Also, use quotations around role names that are more than one word like the example here.
{: tip}




```bash
ibmcloud iam user-policy-create USER@EXAMPLE.COM --service-name backup-recovery --roles "Reader"
```
{: pre}

## Assigning access to {{site.data.keyword.baas_full_notm}} by using the API
{: #assign-access-api}
{: api}

For step-by-step instructions for assigning, removing, and reviewing access, see [Assigning access to resources by using the API](/docs/account?topic=account-assign-access-resources&interface=api) or the [Create a policy API docs](/apidocs/iam-policy-management#create-policy). Role cloud resource names (CRN) in the following table are used to assign access with the API.


| Role name | Role CRN |
|---------------|-----------------|
| Viewer                 | `crn:v1:bluemix:public:backup-recovery::::serviceRole:Viewer`        |
| Operator               | `crn:v1:bluemix:public:backup-recovery::::serviceRole:Operator`      |
| Editor                 | `crn:v1:bluemix:public:backup-recovery::::serviceRole:Editor`        |
| Administrator          | `crn:v1:bluemix:public:backup-recovery::::serviceRole:Administrator` |
| Reader         | `crn:v1:bluemix:public:backup-recovery::::serviceRole:Reader`        |
| Writer         | `crn:v1:bluemix:public:backup-recovery::::serviceRole:Writer`        |
| Manager        | `crn:v1:bluemix:public:backup-recovery::::serviceRole:Manager`       |
{: caption="Table 2. Role ID values for API use" caption-side="bottom"}



The following example is for assigning the `<Reader>` role for `<{{site.data.keyword.baas_full_notm}}>`:

Use `backup-recovery` for the service name, and refer to the Role ID values table to ensure that you're using the correct value for the CRN.
{: tip}


```curl
curl -X POST 'https://iam.cloud.ibm.com/v1/policies' -H 'Authorization: Bearer $TOKEN' -H 'Content-Type: application/json' -d '{
  "type": "access",
  "description": "Reader role for {{site.data.keyword.baas_full_notm}}",
  "subjects": [
    {
      "attributes": [
        {
          "name": "iam_id",
          "value": "IBMid-123453user"
        }
      ]
    }'
  ],
  "roles":[
    {
      "role_id": "crn:v1:bluemix:public:backup-recovery::::serviceRole:Reader"
    }
  ],
  "resources":[
    {
      "attributes": [
        {
          "name": "accountId",
          "value": "$ACCOUNT_ID"
        },
        {
          "name": "serviceName",
          "value": "backup-recovery"
        }
      ]
    }
  ]
}
```
{: curl}
{: codeblock}

```java
SubjectAttribute subjectAttribute = new SubjectAttribute.Builder()
      .name("iam_id")
      .value("IBMid-123453user")
      .build();

PolicySubject policySubjects = new PolicySubject.Builder()
      .addAttributes(subjectAttribute)
      .build();

PolicyRole policyRoles = new PolicyRole.Builder()
      .roleId("crn:v1:bluemix:public:backup-recovery::::serviceRole:Reader")
      .build();

ResourceAttribute accountIdResourceAttribute = new ResourceAttribute.Builder()
      .name("accountId")
      .value("ACCOUNT_ID")
      .operator("stringEquals")
      .build();

ResourceAttribute serviceNameResourceAttribute = new ResourceAttribute.Builder()
      .name("serviceName")
      .value("backup-recovery")
      .operator("stringEquals")
      .build();

PolicyResource policyResources = new PolicyResource.Builder()
      .addAttributes(accountIdResourceAttribute)
      .addAttributes(serviceNameResourceAttribute)
      .build();

CreatePolicyOptions options = new CreatePolicyOptions.Builder()
      .type("access")
      .subjects(Arrays.asList(policySubjects))
      .roles(Arrays.asList(policyRoles))
      .resources(Arrays.asList(policyResources))
      .build();

Response<Policy> response = service.createPolicy(options).execute();
Policy policy = response.getResult();

System.out.println(policy);
```
{: java}
{: codeblock}

```javascript
const policySubjects = [
  {
    attributes: [
      {
        name: 'iam_id',
        value: 'IBMid-123453user',
      },
    ],
  },
];
const policyRoles = [
  {
    role_id: 'crn:v1:bluemix:public:backup-recovery::::serviceRole:Reader',
  },
];
const accountIdResourceAttribute = {
  name: 'accountId',
  value: 'ACCOUNT_ID',
  operator: 'stringEquals',
};
const serviceNameResourceAttribute = {
  name: 'serviceName',
  value: 'backup-recovery',
  operator: 'stringEquals',
};
const policyResources = [
  {
    attributes: [accountIdResourceAttribute, serviceNameResourceAttribute]
  },
];
const params = {
  type: 'access',
  subjects: policySubjects,
  roles: policyRoles,
  resources: policyResources,
};

iamPolicyManagementService.createPolicy(params)
  .then(res => {
    examplePolicyId = res.result.id;
    console.log(JSON.stringify(res.result, null, 2));
  })
  .catch(err => {
    console.warn(err)
  });
```
{: javascript}
{: codeblock}

```python
policy_subjects = PolicySubject(
  attributes=[SubjectAttribute(name='iam_id', value='IBMid-123453user')])
policy_roles = PolicyRole(
  role_id='crn:v1:bluemix:public:backup-recovery::::serviceRole:Reader')
account_id_resource_attribute = ResourceAttribute(
  name='accountId', value='ACCOUNT_ID')
service_name_resource_attribute = ResourceAttribute(
  name='serviceName', value='backup-recovery')
policy_resources = PolicyResource(
  attributes=[account_id_resource_attribute,
        service_name_resource_attribute])

policy = iam_policy_management_service.create_policy(
  type='access',
  subjects=[policy_subjects],
  roles=[policy_roles],
  resources=[policy_resources]
).get_result()

print(json.dumps(policy, indent=2))
```
{: python}
{: codeblock}

```go
subjectAttribute := &iampolicymanagementv1.SubjectAttribute{
  Name:  core.StringPtr("iam_id"),
  Value: core.StringPtr("IBMid-123453user"),
}
policySubjects := &iampolicymanagementv1.PolicySubject{
  Attributes: []iampolicymanagementv1.SubjectAttribute{*subjectAttribute},
}
policyRoles := &iampolicymanagementv1.PolicyRole{
  RoleID: core.StringPtr("crn:v1:bluemix:public:backup-recovery::::serviceRole:Reader"),
}
accountIDResourceAttribute := &iampolicymanagementv1.ResourceAttribute{
  Name:     core.StringPtr("accountId"),
  Value:    core.StringPtr("ACCOUNT_ID"),
  Operator: core.StringPtr("stringEquals"),
}
serviceNameResourceAttribute := &iampolicymanagementv1.ResourceAttribute{
  Name:     core.StringPtr("serviceName"),
  Value:    core.StringPtr("backup-recovery"),
  Operator: core.StringPtr("stringEquals"),
}
policyResources := &iampolicymanagementv1.PolicyResource{
  Attributes: []iampolicymanagementv1.ResourceAttribute{
    *accountIDResourceAttribute, *serviceNameResourceAttribute}
}

options := iamPolicyManagementService.NewCreatePolicyOptions(
  "access",
  []iampolicymanagementv1.PolicySubject{*policySubjects},
  []iampolicymanagementv1.PolicyRole{*policyRoles},
  []iampolicymanagementv1.PolicyResource{*policyResources},
)

policy, response, err := iamPolicyManagementService.CreatePolicy(options)
if err != nil {
  panic(err)
}
b, _ := json.MarshalIndent(policy, "", "  ")
fmt.Println(string(b))
```
{: go}
{: codeblock}

## Assigning access to {{site.data.keyword.baas_full_notm}} by using Terraform
{: #assign-access-terraform}
{: terraform}



The following example is for assigning the `<Reader>` role for `<{{site.data.keyword.baas_full_notm}}>`:

Use `backup-recovery` for the service name.
{: tip}


```terraform
resource "ibm_iam_user_policy" "policy" {
  ibm_id = "test@example.com"
  roles  = ["Reader"]

  resources {
    service = "backup-recovery"
  }
}
```
{: codeblock}

For more information, see [ibm_iam_user_policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_user_policy){: external}.
