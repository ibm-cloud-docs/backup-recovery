---

copyright:
   years: 2025
lastupdated: "2025-12-12"

keywords: tutorials, cbr, firewall, rules, resources, restrictions
subcollection: backup-recovery
content-type: tutorial
account-plan: lite
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}

# Securing Backup and Recovery resources using context-based restrictions
{: #brs-tutorial-cbr}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

In this tutorial, you establish [context-based restrictions](/docs-draft/backup-recovery?topic=backup-recovery-cbr) that prevent any access to a Backup and Recovery service instance (resources) unless the request originates from a trusted network zone.
{: shortdesc}

## Before you begin
{: #brs-tutorial-cbr-prereqs}

Before you plan on using [context-based restrictions](/docs-draft/backup-recovery?topic=backup-recovery-cbr) with Backup and Recovery resources, you need:

- An [IBM Cloud™ Platform account](http://cloud.ibm.com/)
- A [service instance of IBM Cloud Backup and Recovery](/catalog/services/backup-and-recovery)
- A role of `Administrator` for context-based restrictions

## Navigate to the context-based restrictions console
{: #brs-tutorial-cbr-console}
{: step}

- From the **Manage** menu, select **Context-based restrictions**.



## Create a new rule
{: #brs-tutorial-cbr-new-rule}
{: step}

During the creation of the rule, in the **Select your resources** wizard step, you can specify the Backup and Recovery resources to which you would like to apply the context-based restrictions. This rule creation can become as specific or generic as you want - you could apply the rule to all Backup and Recovery service instances or to a specific service group or service instance only.

In this example, we choose a service instance.

1. Click **Rules**, then click **Create**.
2. On the **Service** pane, choose **Backup and Recovery** from the drop-down menu, then click **Next**.
3. On the **APIs** pane, select what API actions to protect (**All** by default), then click **Next**.
4. On the **Resources** pane, select the **Specific resources** radio button.
5. Choose the **Service Instance** from the drop-down menu.
6. Select the service instance that you want the rule to affect, then click **Review**.
7. Review the Rule details thus far, then click **Continue**.




## Add context to the rule
{: #brs-tutorial-cbr-context}
{: step}

Now in the **Add a context** wizard step, you can optionally enforce your rule by specific endpoint types (Public, Private, or Direct, All by default).

1. Turn on the **Endpoints** toggle button to decide which endpoint types to use.
2. Check one or more endpoint type boxes.



## Create a network zone
{: #brs-tutorial-cbr-network}
{: step}

Still, in the **Add a context** wizard step, now that we know what the rule affects, we need to decide what the rule allows. To do that, create a new _network zone_ and apply it to the new rule.

1. In the **Network zones** section after the **Endpoints** pane, click **Create +**.
2. Give the network zone a helpful name and description.
3. Add some IP ranges to the **Allowed IP addresses** text box.
4. Click **Next** to create a new network zone.



If you want to optionally Allow by VPCs (besides that by IP addresses), you can select 1 or multiple VPCs by name or by CRNs as well. You can also Allow Service by referencing it, which would associate its IP addresses with the Network Zone. {: tip}

5. Review the Network Zone details, then click **Create**.
6. Once created, Check the new Network Zone from the table and then click **Add >**.
7. Click **Continue**.



## Finish the rule and verify that it works
{: #brs-tutorial-cbr-finish-rule}
{: step}

Now in the final **Describe your rule** wizard step, you describe your rule, select the desired Enforcement mode, review the details, and finally create it.

1. Choose a name for the rule. While it's optional, this helps keep things organized if you end up with many different rules across all of your cloud services.
2. On the **Enforcement** section, select the desired radio button (**Enabled** by default).
3. Review the final Rule details and then click **Create**.



An easy way to check that if it works is to [send a simple CLI command] from outside of the allowed network zone, such as a Backup and Recovery service instance describe command (`ic resource service-instance <service_instance_name>`). It fails with a `403` error code.

## Next steps
{: #brs-tutorial-cbr-next}

Learn more about [context-based restrictions for Backup and Recovery resources](/docs-draft/backup-recovery?topic=backup-recovery-cbr).
