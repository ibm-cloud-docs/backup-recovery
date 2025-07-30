---

copyright:
  years: 2024
lastupdated: "2024-10-7"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Register an aws cloud source
{: #register_an_aws_cloud_source}

12 September 2024

Register an AWS Cloud Source to support the following functionality:

*   **Backing up AWS infrastructure** — For this functionality, the registered AWS Cloud source contains the Amazon EC2 and/or S3 to back up.
    
    The backup support for AWS S3 is an Early Access feature. Contact your Cohesity account team to enable the feature.
    
*   **Converting and cloning VMware VMs** to AWS — For this functionality, the registered AWS Cloud source is where the VMs are cloned to. Converting and cloning VMs occur during the following use cases: 
    
    *   Clone VMs from On-Premises to a Cloud Service (On-Demand CloudSpin)
        
    *   Clone VMs to Cloud Using CloudSpin
        
    *   Replicate and Clone VMs in Cloud Edition
        
    
    For registering an AWS source to an on-premises {{site.data.keyword.baas_full_notm}}, you must use IAM: User. 
    For registering an AWS source to a Cloud Edition {{site.data.keyword.baas_full_notm}}, you can use IAM: User or IAM: Role. If you use Cloud Edition, you can [Automate IAM Role Permission Assignment for Cohesity AWS Cloud Edition](CloudFormationTemplate.htm).
    

### **to register a aws�cloud source using iam user**
{: #**to_register_a_aws�cloud_source_using_iam_user**}

​AWS Security Token Service (STS) must be enabled in all AWS regions that are activated in your AWS account.

1. Create a IAM User that has the required permissions for either backing up Amazon EC2 or converting and cloning Amazon EC2. For more details, see [AWS and AWS GovCloud Permissions](../../CloudPermissions/AWSPermissions.htm).
2. In the Cohesity UI, select **Data Protection > Sources**.
3. Select **Register > Virtual Machines**.
4. From the **Select Hypervisor Source Type** drop-down, select **AWS : IAM User/Role**.
5. Choose Category as **Standard** or **Gov**.
    
6. Choose the **Authentication Method** as **IAM User**.
    
    This step is not applicable if category is chosen as Gov and is available only for Cloud Edition.
    
    ![](../../Resources/Images/AWS/Register_AWS.gif)
    
7. Enter the information described below and click **Register**.
    
     
    | Field Name | Description |
    | --- | --- |
    | **Access Key ID** | Specify the Access Key ID for the new IAM user that was created to back up a AWS Cloud source or convert and clone Amazon EC2 to an AWS Cloud Source. For general information about AWS keys, see [Access Keys (Access Key ID and Secret Access Key)](http://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys){: external}. |
    | **Secret Access Key** | Specify the Secret Access Key for the new IAM user that was created to back up a AWS Cloud Source or convert and clone Amazon EC2 to an AWS Cloud Source. For general information about AWS keys, see [Access Keys (Access Key ID and Secret Access Key)](http://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys){: external}. |
    | **ARN** | Specify an Amazon Resource Name (ARN) of the IAM user that is used to identify the resources in AWS. For more information, see [Amazon Resource Names (ARNs)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html){: external} . |
    | **Specify Fleet Setting**s | To understand fleet instances, see Fleet Instances.<br><br>Enable this option to:<br><br>*   Specify the VPC and subnet in which the EC2 fleet instance should be launched.<br>*   Add tags to the EC2 fleet instance.<br><br>By default, this option is disabled and the EC2 fleet instance is launched in the same VPC and subnet of the Amazon EC2 instances that are to be protected.<br><br>This option is applicable only for the fleet instance created for backing up Amazon EC2.<br><br>Specify the VPC and Subnet<br><br>From the **Fleet Instance location** drop-down list select one of the following options to specify the VPC and subnet in which the fleet instance should be launched.<br><br>*   **Same Subnet as protected Amazon EC2**: The fleet instance will be launched in the same VPC and subnet of the Amazon EC2 instances that are to be protected.<br>*   **Select New**: The fleet instance will be launched in the specified VPC and subnet of the specified region. If the AWS source is not registered with the {{site.data.keyword.baas_full_notm}}, the cluster cannot detect the AWS region, and the associated VPC and subnets. To configure a new VPC and subnet for the fleet instance, you must first register the source and then update the registered source to specify the VPC and subnet. For more information, see [Configure a New VPC and Subnet for Fleet Instance](#top).<br><br>If you have not selected the **Fleet Instance location** after enabling the **Specify Fleet Settings** option, then the EC2 fleet instance is launched in the same VPC and subnet of the Amazon EC2 instances that are to be protected.<br><br>**Add a Tag**<br><br>Tags enable you to categorize and identify your fleet instances. A tag consists of a key and an optional value, both of which you define.<br><br>To add a tag to the fleet instances, click **Add Tags** and enter a tag key and value. You can add up to 47 tags to a fleet instance.<br><br>Delete a Tag<br><br>To delete an existing tag, click the **cross** icon next to the tag. |
    
    ## Configure a New VPC and Subnet for Fleet Instance
    
    To launch EC2 fleet instance will be launched in the specified subnet when you backup the Amazon EC2 instance to the {{site.data.keyword.baas_full_notm}}. To configure a new VPC and subnet for the fleet instance:
    
    1. In the {{site.data.keyword.baas_full_notm}} Console select **Data Protection** > **Sources**.
    2. Navigate the hierarchy to the registered AWS source for which you want to configure a new VPC and subnet for the fleet instance.
    3. Click **Edit** next to the **Specify Subnet for Fleet Instance** text , or click the actions menu next to the AWS source and select **Edit**.
        
        The Edit Source page appears.
        
    4. Enable the **Specify Fleet Settings** option if you have not enabled when registering the AWS source.
    5. From the **Fleet Instance Location** drop-down list, select **Select New**.
    6. Select a region, and its VPC and subnet in which the fleet instance should be launched.
        
        As the Amazon EC2 source are mostly located in one region, you can select the VPC and subnet of only one region.
        
    7. Click **Save**.
        

### To register a aws cloud source using iam role
{: #to_register_a_aws_cloud_source_using_iam_role}

For information on required AWS permissions, see [AWS and AWS GovCloud Permissions](../../CloudPermissions/AWSPermissions.htm).

*   This option is available only for Cloud Edition and not for Physical or VE.
*   For AWS Standard, IAM role is not supported for Runbook and CE deployment in Helios.
*   IAM role is not supported for workflows in AWS Gov.
*   For AWS C2S, IAM role is not supported for archive/recovery and cloud tier workflows.

1. Select **Data Protection** > **Sources**.
2. Select **Register** > **Virtual Machines**.
3. From the **Select Hypervisor Source Type** drop-down, select **AWS : IAM User/Role**.
4. Choose **Category** as **Standard** or **Gov**. 
    
    For C2S registration, see To register an AWS C2S source using IAM Role.
    
5. Choose the **Authentication Method** as **IAM Role**.
6. Click the **CloudFormation Template** link to download the CFT file.
    
    See [Automate IAM Role Permission Assignment for Cohesity AWS Cloud Edition](CloudFormationTemplate.htm) on AWS for details.
    
7. Enter the **Account ID** where the CE is running and select the **IAM Role** as **CohesitySourceRegistrationRole**.
8. Click **Register**.
    

## To register an aws c2s source using iam role
{: #to_register_an_aws_c2s_source_using_iam_role}

IBM Cloud Supports Commercial Cloud Services (C2S) region in AWS backup and recovery workflows. C2S is AWS's secret region offered by AWS to the US Intelligence community. You can register an AWS C2S source only using an IAM role. Contact [IBM Cloud Support](../../Support/ContactSupport.htm) to enable the feature.

1. Select **Data Protection > Sources**.
    
2. Select **Register** > **Virtual Machines**.
    
3. From the **Select Hypervisor Source Type** drop-down, select **AWS : IAM User/Role**.
    
4. Choose **Category** as **C2S**.
    
5. Configure the following parameters:  
    
     
    | Field Name | Description |
    | --- | --- |
    | Base URL | URL of the Commercial Cloud Services (C2S) server that contains the Amazon storage provided by the C2S agency. |
    | Agency | The name of the agency that is providing the Commercial Cloud Services (C2S) server. |
    | Mission | The account name for accessing the Commercial Cloud Services (C2S) server which is provided by the C2S agency. |
    | Role | The IAM role to use when accessing the Amazon storage which is provided by the C2S agency. |
    | Password | The password to read the Client Private Key when accessing the Commercial Cloud Services (C2S) server which is provided by the C2S agency. |
    | Server CA Trusted Certificate | Upload the Certificate Authority Trusted Certificate file to access the Commercial Cloud Services (C2S) server which is provided by the C2S agency. |
    | Client Certificate | Upload the Client Certificate file to access the Commercial Cloud Services (C2S) server which is provided by the C2S agency. |
    | Client Private Key | Upload the Client Private Key file to use when accessing the Commercial Cloud Services (C2S) server which is provided by the C2S agency. |
    | Specify Fleet Settings<br><br>(Optional) | To understand fleet instances, see Fleet Instances.<br><br>Enable this option to:<br><br>*   Specify the VPC and subnet in which the EC2 fleet instance should be launched.<br>*   Add tags to the EC2 fleet instance.<br><br>By default, this option is disabled and the EC2 fleet instance is launched in the same VPC and subnet of the Amazon EC2 instances that are to be protected. |
    
6. Click **Register**.
    

The following are the considerations of AWS C2S:

*   Amazon EC2 Simple Systems Manager (SSM) is not available in C2S. You must install the Backup agent on the target machine for FLR.
    
*   AWS diff APIs are not available in C2S.
    
*   All VM restores are fleet-based.
    

### Support for http proxy server
{: #support_for_http_proxy_server}

HTTP Proxy is used if the cluster using the proxy server is behind a private network and cannot reach the internet to go through the proxy. {{site.data.keyword.baas_full_notm}} supports HTTP proxy for AWS sources for performing the following workflows:

*   On-demand CloudSpin
*   Policy-based CloudSpin
*   Cloud-native backup and recovery
*   Disaster recovery failback workflows

To enable HTTP proxy, contact [IBM Cloud Support](../../Support/ContactSupport.htm).

## Unregister aws source
{: #unregister_aws_source}

If you plan to stop taking backup of the AWS services, you can unregister the AWS source from the {{site.data.keyword.baas_full_notm}}. Before you unregister, you must delete all the Protection Groups associated with the AWS source.

To unregister an AWS source on the {{site.data.keyword.baas_full_notm}}:

1. In the {{site.data.keyword.baas_full_notm}} Console, select **Data Protection** > **Sources**.
    
2. Click the actions menu (![](../../Resources/Images/i/icn/more-h.svg)) next to the AWS source and select **Unregister**.
    
3. Click **Unregister**.
    

When you unregister a source, all the objects in the source will be unprotected. Any new protection after the re-registration of the source will trigger a full backup and not an incremental backup.
