---

copyright:
  years: 2024
lastupdated: "2025-07-30"

keywords: authorization, iam, basics, credentials

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Service credentials
{: #service-credentials}

A [service credential](/docs/account?topic=account-service_credentials&interface=ui) provides the necessary information to connect an application to {{site.data.keyword.baas_full_notm}} packaged in a JSON document.
{: shortdesc}

Service credentials are always associated with a Service ID, and new Service IDs can be created along with a new credential.

To view a credential you must be granted the Administrator platform role or a custom role that has the `resource-controller.credential.retrieve_all` action. For more information about [this update](/docs/overview?overview-whatsnew#may2022), see [the documentation](/docs/account?topic=account-service_credentials&interface=ui#viewing-credentials-ui).
{: important}

Use the following steps to create a service credential:

1. Log in to the {{site.data.keyword.cloud_notm}} console and navigate to your instance of {{site.data.keyword.baas_full_notm}}.
2. In the side navigation, click **Service Credentials**.
3. Click **New credential** and provide the necessary information.
4. Click **Add** to generate service credential.

When creating a service credential, it is possible to provide a value of `None` for the role.  This will prevent the creation of unintended or unnecessary IAM access policies. Any access policies for the associated service ID will need to be managed using the IAM console or APIs.
{: tip}

The credential has the following values:

| Field name               | Value                                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| `apikey`                 | New API key that is created for the Service ID                                                                                                      |
| `iam_apikey_description` | API key description - initially generated but editable                                                                                          |
| `iam_apikey_name`        | API key name - initially generated but editable                                                                                                 |
| `iam_role_crn`           | Unique identifier for the assigned role                                                                                                             |
| `iam_serviceid_crn`      | Unique identifier for the Service ID                                                                                                                |
| `resource_instance_id`   | Unique identifier for the instance of {{site.data.keyword.baas_full_notm}} the credential accesses. This is also referred to as a service credential. |
{: caption="Credential values" caption-side="top"}

This is an example of a service credential:

```json
{
  "apikey": "0viPHOY7LbLNa9eLftrtHPpTjoGv6hbLD1QalRXikliJ",
  "iam_apikey_description": "Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:backup-recovery:global:a/3ag0e9402tyfd5d29761c3e97696b71n:d6f74k03-6k4f-4a82-b165-697354o63903::",
  "iam_apikey_name": "auto-generated-apikey-f9274b63-ef0b-4b4e-a00b-b3bf9023f9dd",
  "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
  "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/3ag0e9402tyfd5d29761c3e97696b71n::serviceid:ServiceId-540a4a41-7322-4fdd-a9e7-e0cb7ab760f9",
  "resource_instance_id": "crn:v1:bluemix:public:backup-recovery:global:a/3ag0e9402tyfd5d29761c3e97696b71n:d6f74k03-6k4f-4a82-b165-697354o63903::"
}
```

You can also use the IBM Cloud CLI to create a new service credential (which is a subset of something called a *service key*). This example extracts the credential and writes it to a file where the IBM COS SDKs can automatically source the API key and Service Instance ID. First, create the Service Key (called `config-example` and associated with a COS instance called `backup-dev-enablement` in this example):

```sh
ic resource service-key-create config-example --instance-name backup-dev-enablement
```

For more information about IAM visit - [Getting started with IAM](/docs/allowlist/backup-recovery?topic=backup-recovery-iam&interface=ui)
