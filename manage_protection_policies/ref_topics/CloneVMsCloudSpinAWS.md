---

copyright:
  years: 2025
lastupdated: "2025-08-28"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Clone vms to aws cloud using cloudspin
{: #clone_vms_to_aws_cloud_using_cloudspin}


On a {{site.data.keyword.baas_full_notm}} with a Protection Group that captures Snapshots of VMs with a CloudSpin schedule, the cluster clones VMs to an AWS cloud service. You can then launch the VMs to a running state.

What happens during the CloudSpin process is dependent on the cloud service type:


| Full | Incremental |
| --- | --- |
| When you clone to an AWS Cloud using CloudSpin, the {{site.data.keyword.baas_full_notm}} copies the Snapshot to an S3 bucket in the specified AWS account and then converts the snapshot from VMDK format to an AMI format using the AWS import service. | On the first CloudSpin run, the {{site.data.keyword.baas_full_notm}} copies the full VM snapshot data to EBS volumes. The number of EBS volumes corresponds to the number of backed up VM disks. Once copying is done, {{site.data.keyword.baas_full_notm}} uses AWS import service to generate an AMI from the EBS volume corresponding to the root volume and generates the EBS snapshots for all other volumes, if any.<br><br>For subsequent runs, only incremental changes are copied between the snapshots.<br><br>Incremental CloudSpin to AWS requires a Fleet Instance. The cluster communicates with the Fleet Instance using private IP. Therefore, the on-prem cluster must be on a VPN to AWS VPC. |
{: caption="" caption-side="bottom"}

IBM Cloud Supports the CloudSpin of VMware VMs with UEFI boot mode enabled. AWS supports the following UEFI boot mode enabled operating systems:

*   Windows Server 2012, 2016, 2019

*   Ubuntu 16, 18


IBM Cloud Supports CloudSpin on both on-premises {{site.data.keyword.baas_full_notm}} and Cloud Edition {{site.data.keyword.baas_full_notm}}s:

![](../../Resources/Images/CloudSpinBasic.png)

![](../../Resources/Images/CloudSpinFromCloud.png)

**To use CloudSpin to clone Snapshots to AWS:** 

1. Verify the following requirements prior to backing up the VMs: 

    *   All the VMware vSphere versions supported by {{site.data.keyword.baas_full_notm}} for backup is supported for cloning to AWS. For the list of supported VMware vSphere versions, see [Supported Software](../../ReleaseNotes/SupportedVersions.htm).


    *   Cloning VMware VMs to an AWS Cloud has the following requirements:

        *   Cloning Snapshots to an AWS Cloud is supported for VMware VMs with the following Linux operating systems: 

            *   CentOS 7.x
            *   Red Hat Enterprise Linux (RHEL) 7.x
            *   Red Hat Enterprise Linux (RHEL) 6.7
            *   Red Hat Enterprise Linux (RHEL) 8.x
            *   SUSE Linux Enterprise Server 11

            *   Ubuntu 14

            *   Ubuntu 16
        *   Cloning Snapshots to an AWS Cloud is supported for VMware VMs with the Windows operating system.

        *   The disk size of the VMware VMs must be a multiple of 1 MB.

        *   VMs that have one to ten disks can be cloned.

        *   Due to an AWS limitation, performing a CloudSpin to an instance type that has an ENA is not supported. For more information, see [Enhanced Networking on Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/enhanced-networking.html){: external}.

        *   When converting and cloning or copying Snapshots of a Linux VM with multiple disks, the disks must be mounted by UUID prior to being backed up by the Protection Group, as shown by the following example entry in a `/etc/fstab`: 

            **UUID**\=e12e623a-c6ee-4718-9b1a-802456e72258 /userdata ext4 defaults,nofail 0 0

            For detailed instructions, see [Edit /etc/fstab to use UUID](../../CloudEdition/EditFstabToUseUUID.htm).

            For AWS, be aware of the following requirements:

            *   In addition, the fstab entry for **CentOS 7.4**, **RHEL 7.4**, **RHEL 6.7**, **SUSE 11** and **Ubuntu 16** operating systems must specify the `nofail` option as shown in the following example:

                UUID=e12e623a-c6ee-4718-9b1a-802456e72258 /userdata ext4 defaults,**nofail** 0 0

            *   In addition, the fstab entry for the **Ubuntu 14** operating system must specify the `nobootwait` option as shown in the following example:

                UUID=2b262696-41a6-47c9-b173-d02717ee0e10 /mnt/expvol ext4 defaults,**nobootwait** 0 0

        *   When backing up Windows VMs with multiple disks, before a Protection Group captures the Snapshot, the Backup agent and VMware tools must be installed on the VM and the VM must be running. For more information, see [Install and Manage the Agent on Windows Servers](/docs/backup-recovery?topic=backup-recovery-install_and_manage_the_agent_on_windows_servers). In addition, port 50051 must be open in your Firewall. For more information, see [Manage Firewall Ports](/docs/backup-recovery?topic=backup-recovery-manage_firewall_ports).

        *   For a Windows VM, if you want to access the newly created VM in the Cloud service using Remote Desktop, enable the Windows Remote Desktop on the VM prior to capturing the Snapshots of a Windows VM.

2. Register a Cloud Source. Select **Data Protection > Sources** and from the **Register Source** drop-down, select **AWS**. A cloud source is a cloud service instance where the snapshots are converted and copied **to**. For details, see the appropriate Source topic:

*   [Register or Edit an Azure Cloud Source](/docs/backup-recovery?topic=backup-recovery-source-registration-tutorial)
*   [Register an AWS Cloud Source](/docs/backup-recovery?topic=backup-recovery-source-registration-tutorial)

4. For a Windows VM, if you want to access the newly created VM in the Cloud service using Remote Desktop, enable the Windows Remote Desktop on the VM prior to capturing the Snapshots of a Windows VM.

5. Create a Policy with a CloudSpin schedule that converts and copies Snapshots to the Cloud Source registered in the previous step. For detailed instructions, see [Create or Edit a Standard Policy](/docs/backup-recovery?topic=backup-recovery-create_or_edit_a_standard_policy). Protection Groups that use this policy will automatically convert and copy Snapshots backed up by the Protection Group to a Cloud Source using the specified CloudSpin schedule.

    For AWS:


    | Full | Incremental |
    | --- | --- |
    | In Full CloudSpin, the AMI and snapshots are created in the specified Region. | Specify the Region, VPC, and Subnet for an incremental CloudSpin. The AMI and snapshots are created in the specified Region. VPC and Subnet are required for the following reason:<br><br>Incremental CloudSpin requires connectivity between the {{site.data.keyword.baas_full_notm}} and AWS through VPC and Subnet - During incremental CloudSpin, {{site.data.keyword.baas_full_notm}} launches a temporary EC2 instance in the provided VPC and Subnet. The temporary instance is used to copy data from {{site.data.keyword.baas_full_notm}} to AWS. An instance launched in this subnet must be reachable from the {{site.data.keyword.baas_full_notm}}. |

6. Create a Protection Group that uses the Policy created in the previous step.

    After the Protection Group run and CloudSpin Copy Task successfully completes, the VM can be launched.

7. Launch the VM created in the Cloud Service:

    Launch VM from the {{site.data.keyword.baas_full_notm}} Console:

    1. Select **Data Protection > Protection**.
    2. Click the Job that backed up the VM to launch in the Cloud.
    3. Browse for a protection run that contains a Snapshot to convert to a VM. The protection run must have the Status of **Success** and the associated CloudSpin Copy task must be successful.
    4. Click the date link of the protection run in the Start Time column to view the protection run details.
    5. Click the **ClouldSpin Task** tab.
    6. To launch, complete one of the following actions:
        *   Click the actions menu and select **Launch Instance**. The following fields are displayed:
            *   **Source**—Displays the registered source.

            *   **Resource Group**—Displays the resource pool that stores the cloned VMs.

            *   **Storage Account**—Displays the VM size used when creating the cloned VMs.

            *   **Storage Container**—Displays the storage container used to store the cloned VMs.

            *   **Compute**—Select the Microsoft Azure VM size type to use when creating the cloned VMs.

            *   **Availability Set**—Optionally, select the availability set to which the VM has to be cloned.

            *   **Virtual Network**—Select the Microsoft Azure Virtual Network for the cloned VMs.

            *   **Subnet**—Select the Microsoft Azure Virtual subnet for the cloned VMs.

            *   **Use Azure Managed Disks**—To use Azure managed disks, enable the toggle and select the OS Disk Type and Data Disk Type.

                Click **Submit** to save the changes.

        *   Click **Launch All Instances**.

    To tear down a launched VM, see [Tear Down a Launched VM from {{site.data.keyword.baas_full_notm}} Console](#Tear).

    Launch VM from AWS Console:

    1. Log in to the [Amazon AWS console](https://aws.amazon.com/){: external} and select **EC2**. The **EC2 Dashboard** displays.
    2. In the left frame, select **IMAGES > AMIs**.
    3. In the search field, enter part of the VM name. The generated VM name consists of the Job name, the original name and the Snapshot time.
    4. Select a VM and click **Launch**.
    5. Enter the required settings and click **Review and Launch**.


To prevent a tear down of a cloned CloudSpin VM, see the [How to prevent a tear down of a cloned CloudSpin VM](/docs/backup-recovery?topic=How-to-prevent-tear-down-of-cloudspin-VM){: external} KB article.

Tear Down a Launched VM from {{site.data.keyword.baas_full_notm}} Console

*   In the {{site.data.keyword.baas_full_notm}} Console, select **Test & Dev**.
*   For the clone task to tear down, click the actions menu (![](../../Resources/Images/i/icn/more-h.svg)) and select **Tear Down**.

## Consideration
{: #consideration}

*   AWS Security Token Service (AWS STS) must be enabled in the region where a VM is being Cloud spun.

*   {{site.data.keyword.baas_full_notm}} does not support CloudSpin of VMware VMs from VMware Cloud (VMC) to AWS.

*   IBM Cloud Supports both on-demand and policy-based CloudSpin for snapshots stored on the {{site.data.keyword.baas_full_notm}} (primary copy).

*   {{site.data.keyword.baas_full_notm}} does not support on-demand and policy-based CloudSpin for archived or CloudRetrieved snapshots.

*   IBM Cloud Supports on-demand CloudSpin for snapshots that are replicated to another {{site.data.keyword.baas_full_notm}}.
