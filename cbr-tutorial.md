---

copyright:
   years: 2025
lastupdated: "2025-12-02"

keywords: tutorials, cbr, firewall, rules, resources, restrictions
subcollection: backup-recovery
content-type: tutorial
account-plan: lite
completion-time: 10m

---

{{site.data.keyword.attribute-definition-list}}

# Securing Backup and Recovery resources using context-based restrictions
{: #cos-tutorial-cbr}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

In this tutorial, you will establish [context-based restrictions](docs-draft/backup-recovery?topic=backup-recovery-cbr) that prevent any access to a Backup and Recovery service instance (resources) unless the request originates from a trusted network zone.
{: shortdesc}

## Before you begin
{: #cos-tutorial-cbr-prereqs}

Before you plan on using [context-based restrictions](docs-draft/backup-recovery?topic=backup-recovery-cbr) with Backup and Recovery resources, you need:

- An [IBM Cloud™ Platform account](http://cloud.ibm.com/)
- A [service instance of IBM Cloud Backup and Recovery](/catalog/services/backup-and-recovery)
- A role of `Administrator` for context-based restrictions

## Navigate to the context-based restrictions console
{: #cos-tutorial-cbr-console}
{: step}

From the **Manage** menu, select **Context-based restrictions**.

![Navigate to CBR](/images/cbr_1.png){: caption="Navigate to CBR"}

## Create a new rule
{: #cos-tutorial-cbr-new-rule}
{: step}

During the creation of the rule, in the **Select your resources** wizard step, you can specify the Backup and Recovery resources to which you would like to apply the context-based restrictions. This can become as specific or generic as you wish - you could apply the rule to all Backup and Recovery service instances or to an specific service instance only.

In this example, we will choose a service instance.

1. Click on **Rules**, then click on **Create**.
2. On the **Service** pane choose **Backup and Recovery** from the drop down menu, then click on **Next**.

![Create rule](/images/cbr_2.png){: caption="Create rule"}

3. On the **APIs** pane select what API actions to protect (**All** by default), then click on **Next**.
4. On the **Resources** pane select the **Specific resources** radio button.
5. Choose the **Service Instance** from the drop down menu.
6. Select the service instance you want the rule to affect, then click on **Review**.

![Select resources](/images/cbr_3.png){: caption="Select resources"}

7. Review the Rule details thus far, then click **Continue**.

## Add context to the rule
{: #cos-tutorial-cbr-context}
{: step}

Now in the **Add a context** wizard step, you can optionally enforce your rule by specific endpoint types (Public, Private, or Direct, All by default).

1. Turn on the **Endpoints** toggle button to decide which endpoint types to use.
2. Check the endpoint type box(es) accordingly.

![Add context to the rule](/images/cbr_4.png){: caption="Add context to the rule"}

## Create a network zone
{: #cos-tutorial-cbr-network}
{: step}

Still in the **Add a context** wizard step, now that we know what the rule will affect, we need to decide what the rule will allow. To do this, we'll create a new _network zone_ and apply it to the new rule.

1. In the **Network zones** section below the **Endpoints** pane, Click on **Create +**.
2. Give the network zone a helpful name and description.
3. Add some IP ranges to the **Allowed IP addresses** text box.
4. Click **Next**.

![Create Network Zone](/images/cbr_5.png){: caption="Create Network Zone"}

If you want to optionally Allow by VPCs (besides that by IP addresses), you can select 1 or multiple VPCs by name or by CRNs as well. You can also Allow by Service referencing it which would associate its IP addresses with the Network Zone. {: tip}

5. Review the Network Zone details, then Click on **Create**.
6. Once created, Check the new Network Zone from the table and then Click on **Add >**

![Select the created Network Zone](/images/cbr_6.png){: caption="Select the created Network Zone"}

7. Click **Continue**.

## Finish the rule and verify that it works
{: #cos-tutorial-cbr-finish-rule}
{: step}

Now in the final **Describe your rule** wizard step, you will just describe your rule, select the desired Enforcement mode and review the details and finally creating it.

1. Choose a name for the rule. While it's optional, this will help keep things organized if you end up with a lot of different rules across all of your cloud services.
2. On the **Enforcement** section select the desired radio button (**Enabled** by default).
3. Review the final Rule details and then Click on **Create**.

![Finish the rule](/images/cbr_7.png){: caption="Finish the rule"}

An easy way to check that it works is to [send a simple CLI command] from outside of the allowed network zone, such as a Backup and Recovery service instance describe command (`ic resource service-instance <service_instance_name>`).  It will fail with a `403` error code.

## Next steps
{: #cos-tutorial-cbr-next}

Learn more about [context-based restrictions for Backup and Recovery resources](/docs-draft/backup-recovery?topic=backup-recovery-cbr).
