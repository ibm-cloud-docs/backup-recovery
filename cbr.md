---

copyright:
  years:  2025
lastupdated: "2025-12-12"

keywords: cbr, backup-recovery

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protecting backup-recovery resources with context-based restrictions
{: #cbr}


Context-based restrictions give account owners and administrators the ability to define and enforce access restrictions for {{site.data.keyword.cloud}} resources based on the context of access requests. Access to **backup-recovery** resources can be controlled with context-based restrictions and identity and access management (IAM) policies.
{: shortdesc}

These restrictions work with traditional IAM policies, which are based on identity, to provide an extra layer of protection. Unlike IAM policies, context-based restrictions don’t assign access. Context-based restrictions check that an access request comes from an allowed context that you configure. Since both IAM access and context-based restrictions enforce access, context-based restrictions offer protection even in the face of compromised or mismanaged credentials. For more information, see [What are context-based restrictions](/docs/account?topic=account-context-restrictions-whatis).

A user must have the Administrator role on the backup-recovery service to create, update, or delete rules. A user must also have either the Editor or Administrator role on the Context-based restrictions service to create, update, or delete network zones. A user with the Viewer role on the Context-based restrictions service can add only network zones to a rule.
{: note}

Any {{site.data.keyword.cloudaccesstraillong_notm}} or audit log events generated come from the Context-based restrictions service, not backup-recovery. For more information, see [Monitoring context-based restrictions](/docs/account?topic=account-cbr-monitor).

To start protecting your backup-recovery resources with context-based restrictions, see the tutorial for [Leveraging context-based restrictions to secure your resources](/docs/account?topic=account-context-restrictions-tutorial).

## How backup-recovery integrates with context-based restrictions

{: #cbr-overview}

Backup & Recovery exposes management and data-plane operations through the console and APIs. You can protect the **entire service** (all actions) or **scope protection** to specific resources and, where available, specific API groups (for finer-grained control). The exact set of resource attributes that you can use to scope a rule is visible in the rule builder UI; documenting these attributes helps customers that use the CLI or API because there currently is no programmatic way to retrieve the available resource attributes for a given service.

### Protecting specific APIs

{: #cbr-api-protection}

If you need to allow some **backup-recovery** API categories and restrict others, you can scope rules to API groupings (for example, “instance management” vs “job execution”). Ask your development team for the **CBR service registration JSON** for **backup-recovery** to identify the API groupings customers can select when scoping a rule, including the actions and action descriptions that map to each grouping.
{: tip}

**Examples (API):**

- **Protect all backup-recovery actions** in the account:

  ```json
  {
    "resources": [
      {
        "attributes": [
          { "name": "accountId",    "value": "YOUR_ACCOUNT_ID" },
          { "name": "serviceName",  "value": "backup-recovery" }
        ]
      }
    ],
    "description": "Allow BRS only from approved contexts",
    "contexts": [],
    "enforcement_mode": "enabled"
  }
  ```

## Limitations

{: #cbr-limitations}

Context-based restrictions protect only the actions associated with the Backup Recovery API. Actions that are associated with the following platform APIs are not protected by context-based restrictions. Reference the API docs for the specific action IDs.

- [Resource Instance APIs](/apidocs/resource-controller/resource-controller#list-resource-instances)
- [Resource Keys APIs](/apidocs/resource-controller/resource-controller#list-resource-keys)
- [Resource Bindings APIs](/apidocs/resource-controller/resource-controller#list-resource-bindings)
- [Resource Aliases APIs](/apidocs/resource-controller/resource-controller#list-resource-aliases)
- [IAM Policy APIs](/apidocs/iam-policy-management#list-policies)
- [Global Search APIs](/apidocs/search)
- Global Tagging [Attach](/apidocs/tagging#attach-tag) and [Detach](/apidocs/tagging#detach-tag) APIs
- [Context-based Restriction Rule APIs](/apidocs/context-based-restrictions#create-rule)
- [Secrets Manager APIs](/apidocs/secrets-manager)

## Creating network zones

{: #network-zone}

A network zone represents an allowlist of IP addresses where an access request is created. It defines a set of one or more network locations that are specified by the following attributes:

- IP addresses, which include individual addresses, ranges, or subnets.
- VPCs
- Service references, which allow access from other {{site.data.keyword.cloud_notm}} services.

Make sure to add backup-recovery to network zones for rules that target other {{site.data.keyword.cloud_notm}} resources, or some operations in your workflow might fail.
{: important}

### Creating network zones in the console

{: #network-zone-ui}
{: ui}

To create network zones from the console, complete the following steps:

1. Go to **Manage > Context-based restrictions** in the {{site.data.keyword.cloud_notm}} console.
1. Click **Network zones > Create**.
1. Provide a name and description for your network zone.
1. Add network locations, VPCs, or service references to the zone.
1. Click **Create**.

### Creating network zones by using the API

{: #network-zone-api}
{: api}

You can create network zones by using the `create-zone` command. For more information, see the [API docs](/apidocs/context-based-restrictions#create-zone). You can add backup-recovery to network zones as a service reference to allow backup-recovery to access resources and services in your account that are the subject of a rule.

The `serviceRef` attribute for backup-recovery is `backup-recovery`.
{: tip}

### Creating network zones by using the CLI

{: #network-zone-cli}
{: cli}

You can use the `cbr-zone-create` command to add network locations, VPCs, and service references to network zones. For more information, see the CBR [CLI reference](/docs/account?topic=account-cbr-plugin#cbr-zones-cli). Add backup-recovery to network zones as a service reference to allow backup-recovery to access resources and services in your account that are the subject of a rule.

To find a list of available service refs, run the `ibmcloud cbr service-ref-targets` [command](/docs/account?topic=account-cbr-plugin#cbr-cli-service-ref-targets-command). The `service_name` for backup-recovery is `backup-recovery`.
{: tip}

## Creating rules

{: #create-rules}

Define restrictions to backup-recovery resources by creating rules.

### Creating rules in the console

{: #rules-ui}
{: ui}

To create rules from the console, complete the following steps:

1. Go to **Manage > Context-based restrictions** in the {{site.data.keyword.cloud_notm}} console.
1. Click **Rules > Create**.
1. Select the **backup-recovery** service.
1. Scope the rule to specific resources or API types as needed.
1. Add network zones to the rule.
1. Review and create the rule.

### Creating rules by using the API

{: #rules-api}
{: api}

Review the following example to learn how to create rules for backup-recovery. For more information, see the [API docs](/apidocs/context-based-restrictions#create-rule).

```json
{
  "resources": [
    {
      "attributes": [
        { "name": "accountId",    "value": "YOUR_ACCOUNT_ID" },
        { "name": "serviceName",  "value": "backup-recovery" }
      ]
    }
  ],
  "description": "Allow backup-recovery only from approved contexts",
  "contexts": [
    {
      "attributes": [
        {
          "name": "networkZoneId",
          "value": "YOUR_ZONE_ID"
        }
      ]
    }
  ],
  "enforcement_mode": "enabled"
}
```

### Creating rules by using the CLI

{: #rules-cli}
{: cli}

Review the following example to learn how to create rules for backup-recovery. For more information, see the CBR [CLI reference](/docs/account?topic=account-cbr-plugin).

```bash
ibmcloud cbr rule-create --service-name backup-recovery --zone-id YOUR_ZONE_ID --description "Allow backup-recovery only from approved contexts"
```
