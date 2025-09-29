---

copyright:
  years: 2025
lastupdated: "2025-08-28"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Register or edit an azure cloud source
{: #register_or_edit_an_azure_cloud_source}


A {{site.data.keyword.baas_full_notm}} supports registering an Azure Cloud Source for the following functionality:

*   **Converting, cloning, and copying Snapshots of VMware VMs**—For this functionality, the registered Azure Cloud Source is where the VM Snapshots are cloned or copied to. Converting, cloning or copying VM Snapshots occurs during the following use cases: 
    
    *   [Clone VMs from On-Premises to Azure Cloud Service (On Demand CloudSpin)](../../CloudEdition/CloneOnPermToCloudazure.htm)
    *   [Clone VMs to Azure Cloud Using CloudSpin](../TestDev/CloneVMsCloudSpinAzure.htm)
    *   [Replicate and Clone VMs in Azure Cloud Edition](../../CloudEdition/TestDevCloneAzure.htm)
*   **Backing up Azure infrastructure** — For this functionality, the registered Azure Cloud source contains the Azure VMs to back up.
    

**To register an Azure Cloud Source**:

Before registering the Azure Cloud Source with the {{site.data.keyword.baas_full_notm}}, ensure you have met the [prerequisites](../../CloudEdition/azure-prerequisites-considerations.htm) and reviewed the [considerations](../../CloudEdition/azure-prerequisites-considerations.htm).

1. Select **Data Protection > Sources**.
2. Select **Register > Virtual Machines** and select **Azure: Azure Subscription** from the Source drop-down.
3. Enter the information described below and click **Register Source**.
    
    | Field Name | Description |
    | --- | --- |
    | **Category** | Select one of the following options:<br><br>*   **Standard**—Standard Azure.<br>    <br>*   **Gov**—Azure Government.<br>    <br>*   **Stack**—Azure Stack Hub. You must specify the authentication type, region, and the domain name. For more information, see [Azure Stack Hub](../../Cloud/azure-stack.htm). |
    | **Subscription ID** | The subscription ID for the subscription used to store the resources of the {{site.data.keyword.baas_full_notm}}. Log in to the Azure portal. In the left panel, click **Subscriptions**. Copy the SUBSCRIPTION ID from the table. |
    | **Application ID** | The Application ID assigned by Azure during the service principal creation process. For instructions see [Get application ID and authentication key](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-application-id-and-authentication-key){: external} in the Microsoft Azure documentation. |
    | **Application Key** | The Azure Application key generated using Legacy App Registration during the service principal creation process that is used for authentication. For instructions see [Get application ID and authentication key](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-application-id-and-authentication-key){: external} in the Microsoft Azure documentation. |
    | **Tenant ID** | The unique tenant ID assigned by Azure. For instructions see [Get tenant ID](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id){: external} in the Microsoft Azure documentation.<br><br>The **Tenant ID** field is not displayed for Azure Stack Hub Active Directory Federation Services (AD FS) authentication type. |
    | **Stack Type** | If you select Stack, select the authentication type:<br><br>*   Azure Active Directory<br>    <br>*   Active Directory Federation Services<br>    <br><br>Azure Stack Hub uses either Azure Active Directory or AD FS for authentication. AD FS is mostly used for disconnected deployments of Azure Stack Hub. For more information, see Microsoft Azure Stack Hub [documentation](https://docs.microsoft.com/en-us/azure-stack/operator/azure-stack-integrate-identity?view=azs-2008){: external}. |
    | **Region** | If you select **Stack**, specify the Azure Stack Hub region. For instructions, see [Region management in Azure Stack Hub](https://docs.microsoft.com/en-us/azure-stack/operator/azure-stack-region-management?view=azs-2008){: external} in the Microsoft Azure documentation. |
    | **Domain Name** | If you select **Stack**, specify the Azure Stack Hub domain name. |
    

## Support for http proxy server
{: #support_for_http_proxy_server}

{{site.data.keyword.baas_full_notm}} supports HTTP proxy for Azure connectors for performing the following workflows:

*   On-demand CloudSpin
*   Policy-based CloudSpin
*   Cloud-native backup and recovery
*   Disaster recovery failback workflows

## Unregister azure source
{: #unregister_azure_source}

If you plan to stop taking backup of the Azure services, you can unregister the Azure source from the {{site.data.keyword.baas_full_notm}}. Before you unregister, you must delete all the Protection Groups associated with the Azure source.

To unregister an Azure source on the {{site.data.keyword.baas_full_notm}}:

1. In the {{site.data.keyword.baas_full_notm}} Console, select **Data Protection** > **Sources**.
    
2. Click the actions menu (![](../../Resources/Images/i/icn/more-h.svg)) next to the Azure source and select **Unregister**.
    
3. Click **Unregister**.
    

When you unregister a source, all the objects in the source will be unprotected. Any new protection after the re-registration of the source will trigger a full backup and not an incremental backup.
