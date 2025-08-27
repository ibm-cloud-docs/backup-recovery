---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Manage protection policy
{: #manage_protection_policy}

28 May 2024

A protection policy is a reusable set of settings that define how and when objects are protected, replicated and archived. You can select a standard policy to use when configuring a Protection Group. A Protection Group uses the schedules and settings defined in the policy to determine when and how backups are captured, archived or replicated.

For instructions on creating and managing policies, see [Create or Edit a Standard Policy](../Dashboard/Protection/PolicyCreateEdit.htm).

Some settings are defined in the Protection Group and some settings are defined in the policy. For example, for a Protection Group that protects servers, the schedule is defined at the policy level but indexing is defined at the Protection Group level as shown in the following figure.

Policies are reusable. You can define a single policy that is used by different Protection Groups as shown in the following figure.

![](../Resources/Images/PolicyJobReuse.png "Click to Enlarge")
