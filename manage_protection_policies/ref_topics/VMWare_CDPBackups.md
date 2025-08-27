---

copyright:
  years: 2025
lastupdated: "2025-8-4"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.baas_full_notm}} cdp for vmware
{: #cohesity_cdp_for_vmware}

17 September 2024

To protect your VMware VMs, you can optionally choose to leverage the Continuous Data Protection (CDP) option available on the {{site.data.keyword.baas_full_notm}} Data Cloud. {{site.data.keyword.baas_full_notm}} CDP provides near-zero RPOs for your applications.

Every change to the VM (IOs) is captured and replicated to the {{site.data.keyword.baas_full_notm}} cluster. This provides a continuous window of protection allowing you to restore a VM to a selected point-in-time within the configured CDP window.

{{site.data.keyword.baas_full_notm}} uses the [VMware API for I/O Filtering (VAIO)](https://code.vmware.com/programs/vsphere-apis-for-io-filtering){: external} based solution for providing Continuous Data Protection. In this process, {{site.data.keyword.baas_full_notm}} deploys an I/O Filter in a vSphere environment. The scope of the filter installation is at a cluster level (also known as ClusterComputeResouce) within the vCenter. Once the filter installation is complete, an I/O Filter is attached to the VM marked for protection. This filter intercepts all the real-time IOs of the VM and sends the IOs to the Filter Daemon running on the ESXi host. The Filter Daemon continuously replicates the IOs to the {{site.data.keyword.baas_full_notm}} cluster (IO Journal) based on the VM Storage Policy attached to the VM.

In releases earlier than 7.0, the {{site.data.keyword.baas_full_notm}} cluster automatically deploys the VAIO filter on the ESXi host clusters. From release 7.0 onwards, {{site.data.keyword.baas_full_notm}} provides an additional option to download the VAIO from the {{site.data.keyword.baas_full_notm}} cluster and upload it to an HTTP server to deploy it manually on the ESXi host clusters using the vSphere IO filter installation APIs. For details, see [Deploy the VAIO Filter Manually](#Deploy).

You can either manually create or update an existing VM Storage Policy and then attach it to the VMs or {{site.data.keyword.baas_full_notm}} will automatically create and attach the VM Storage Policy to the VMs you want to perform continuous data protection.

If you are manually attaching the VM Storage Policy, then ensure to:

*   Attach a Storage Policy to the VMs before a CDP-enabled Protection Group is created for these VMs.

*   While creating the VM Storage Policy, select the **Enable host based rules** checkbox in the **Policy structure** tab and create rules for Replication data service under **Host based services** with the following settings:

    *   **Provider**: cohfilter

    *   **vCenterUUID**: <vCenterUUID>

    *   **CohesityID**: <ClusterId:IncarnationId>

    *   **CohesityClusterEndpoint**: <endpoint>:20004

    *   **HeartbeatInterval**: 30


You can view the default Storage Policy created by {{site.data.keyword.baas_full_notm}} on the VMware vCenter as a reference for the above settings.

During the backup process, all the IOs of a workload will be saved in a write-ahead-log (WAL), and is used to recover the applications to any point-in-time as long as all the previous IOs are available. If any IOs are missed during the CDP, then a VADP backup is triggered, and the CDP backup process is re-initialized. For better RTO, the hydrated snapshots of the VMs are also captured to the {{site.data.keyword.baas_full_notm}} cluster every 15 minutes.

## Core components
{: #core_components}

This section lists the core components of the {{site.data.keyword.baas_full_notm}} CDP.

*   **I/O Filter**: An I/O Filter is run per VM on the ESXi host as part of the Virtual Machine Executable (VMX) process. It intercepts the IOs of a VM synchronously in real-time and replicates those IOs to the {{site.data.keyword.baas_full_notm}} cluster.
*   **I/O Filter Daemon**: An I/O Filter Daemon, also implemented using VAIO, provides more resources to perform replication, caching, batching, and other operations. It runs in the ESXi host environment and communicates with individual VM’s I/O Filters over a Unix socket. An I/O Filter Daemon is ideal for replicating collected IOs from VMs over the network asynchronously.
*   **{{site.data.keyword.baas_full_notm}} Data Cloud**: The CDP service starts on all the {{site.data.keyword.baas_full_notm}} cluster nodes and receives the Filter Daemon requests.

![](../../Resources/Images/VMWareCDP_Architecture1_661x333.png)

## Cdp requirements for vmware vcenter
{: #cdp_requirements_for_vmware_vcenter}

If you are enabling CDP for near-zero RPO, ensure that the VMware vCenter meets these additional requirements:


| Requirements | Details |
| --- | --- |
| ESXi version | VMware ESXi 7.0 U3, 7.0 U2, 7.0 U1, 7.0, 6.7 U3, 6.7 U2, 6.7 U1, 6.7, 6.5 U3, 6.5 U2 |
| Memory required per VM disk | 1 GB (from the heap) |
| Memory required by Daemon | 4 GB |
| ESXi Hosts | All the ESXi hosts within the Cluster Compute Resource should be on the same VLAN. |
| Upgrade filter | For automatic upgrades or uninstallation of filters, a VMware cluster should have Distributed Resource Scheduler (DRS) enabled with shared storage. |
{: caption="Table 1. " caption-side="bottom"}

### Cdp related permissions
{: #cdp_related_permissions}

For CDP, in addition to the required [VMware vCenter Related Permissions](../../Setup/SourcePrivileges.htm#Role), you must also require the following VMware privileges:


| Privilege Level |     | Permissions Required | Description |
| --- | --- | --- | --- |
| Host | Configuration | Maintenance | To move hosts automatically to maintenance mode to uninstall CDP filter. |
| Query patch | To deploy CDP Filter to the host. |
| Profile-driven Storage |     | Profile-driven storage update | To create and update Storage profiles to enable CDP for virtual machines. |
| Profile-driven storage view | To view defined storage capabilities and storage profiles in order to manage CDP. |
{: caption="Table 2. " caption-side="bottom"}

## Enable cohesity cdp on vms
{: #enable_cohesity_cdp_on_vms}

To enable CDP on VMs, create a new protection policy and attach the Continuous Data Protection option to the policy. Within the CDP option specify the retain point in time configuration, and associate this policy with the VM’s Protection Group.

After associating the protection policy with the Protection Group, the protection run will first initiate a full VADP based backup and, in the process, attach the I/O Filter to the ESXi host. An incremental protection run follows the initial full backup, and the VM's continuous protection will begin from this point on. You can view all the continuously protected VMs from the same Dashboard page.

### Steps
{: #steps}

Perform these tasks to enable CDP for VMs:

1. [Create a Protection Policy](#Create) with the **Continuous Data Protection** option.

2. Associate this policy with the VMs’ Protection group that you plan to protect using CDP. See [Add or Edit a Protection Group for Virtual Servers](JobServerVirtual.htm).


## Create a protection policy
{: #create_a_protection_policy}

A Protection Policy is a group of settings that define the type of backup, backup schedule, and backup snapshot retention. Also, for Disaster Recovery, you can add CDP and replication for the Protection Group using the Protection Policy.

To create a Protection Policy with CDP & Replication:

1. Navigate to **Data Protection** > **Policies**.
2. Click **Create Policy** located at the top right of the page.

    The Create Protection Policy page appears.

3. In the **Policy Name** field, specify a protection policy name. The name can contain the alphanumerics, underscores, hyphens, periods, and spaces. This field is required and can be changed later.
4. In the **Incremental Backup** field, specify when backups are captured, how long they are retained.
5. Click **More Options**.
6. Click the **Continuous Data Protection** option from the floating menu. In the **Retain Point in Time for** field, enter a number and select Minute, Hour, or Day from the drop-down. This value defines the period the {{site.data.keyword.baas_full_notm}} cluster will retain the captured CDP logs. You can use this option to restore the VM applications to a point-in-time as opposed to periodic snapshots available with regular protection runs.

    When CDP is enabled for a protection policy, VM IOs are captured continuously and streamed in real time to the local cluster. As a result, data protected by CDP-enabled policies are available on the local cluster with near-zero RPO.

7. Click the **Replication** icon to replicate snapshots with CDP to another {{site.data.keyword.baas_full_notm}} cluster. The copied snapshots are stored in the storage domain of the remote cluster. (The storage domain, however, is specified in the Protection Group.) The replication schedule for copying Snapshots cannot be more often than the protection schedule for capturing Snapshots. For example if Snapshots are captured hourly, the {{site.data.keyword.baas_full_notm}} cluster cannot replicate them every 30 minutes but it can replicate them hourly or any value over one hour.

    When **Continuous Data Protection** and **Replication** are enabled, VM IOs are captured real-time and replicated periodically (every 15 minutes by default) on the remote cluster.

    1. **Replicate to**: Select either a Remote Cluster or AWS.

        From the drop-down, select either a cluster source or an AWS S3 storage source to replicate snapshots. If you select an S3 storage object, you also need to select a region.

        You can also register a new cluster to replicate to by selecting **Register Remote Cluster**.

        From {{site.data.keyword.baas_full_notm}} release 7.0 onwards, you can enable **CDP Synchronous Replication** for a Protection Group. Once enabled, VM IOs of every VM in that Protection Group are streamed directly to the remote cluster (continuous streaming, not periodic).

        As a result, Synch Replication makes data available for immediate (near-zero RPO) recovery. For details on enabling **CDP Synchronous Replication**, see [Add or Edit a Protection Group for Virtual Servers](JobServerVirtual.htm) section.

        Irrespective of whether you use an existing remote cluster or register a new one, you must enable the **Encryption** toggle in the remote cluster if you have enabled the **CDP Synchronous Replication** field in the Protection job to replicate the real-time IOs and logs to the remote cluster synchronously. For details on the **CDP Synchronous Replication** field, see [Add or Edit a Protection Group for Virtual Servers](JobServerVirtual.htm). For detailed instructions on registering a remote cluster and enabling encryption, see [Create a Connection to the Remote Cohesity Cluster](../Platform/ReplicationRegisterRemoteCluster.htm).

    2. **Retain for xx days**: Specify how long the snapshots are retained on the remote cluster.

    To add additional replication schedules, repeat these steps. To replicate to multiple {{site.data.keyword.baas_full_notm}} clusters, create multiple replication schedules with different replication targets.

    If you specify multiple replication schedules with the same replication target that replicate the same set of snapshots but with different retention periods, the snapshots are retained for the longest time period. For example if you define two replication schedules with the same replication target that retain the same set of snapshots but one schedule specifies a 90 day retention period and other schedule specifies a 180 day retention period, the set of snapshots are retained for 180 days.

8. Click **Save**.

The policy is immediately available to use in Protection Groups. If you want to configure additional policy settings, see [Create or Edit a Standard Policy](PolicyCreateEdit.htm).

If replication is enabled for CDP, then blackout window option in the policy would not be honored.

## Recovery
{: #recovery}

You can perform a point-in-time recovery of VMs, files and folders, Virtual Disks, and IVM of a CDP based Protection Group s by selecting the points in the timeline bar on the recovery page. This timeline will also show when the VADP backups were taken. So, the user can optionally recover from the VADP snapshots as well. For detailed instructions on recovering VMs, files and folder, Virtual Disks, and IVM, see [Recover Virtual Machines](../../LandingPage/vmr-recovery.htm).

## Deploy the vaio filter manually
{: #deploy_the_vaio_filter_manually}

From release 7.0 onwards, you can deploy the VAIO filter manually on the ESXi host cluster. To deploy the VAIO filter, use the VMware vSphere IOFilterManager APIs that can be accessed through the vCenter MOB interface.

The vibURL required to deploy the VAIO filter is generated by [downloading the VAIO installer from the Cohesity cluster](#Download) and [directly hosting the VAIO installer on an HTTP server](#Generate) accessible from the vCenter server and the ESXi hosts where the VAIO filter needs to be installed.

### Prerequisites
{: #prerequisites}

*   Ensure the VMware vCenter meets the [CDP requirements](#CDP).

*   Ensure the VMware vCenter meets the [conditions and permissions](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.storage.doc/GUID-1021600A-AFE0-45FA-9AAC-CE70A383AA95.html){: external} required to install the I/O filers in its ESXi host clusters.

*   Uninstall existing I/O Filters from the ESXi hosts and the ESXi host clusters.


The VAIO filter can only be installed on the ESXi host cluster and not on the ESXi host.

### Download the vaio filter from the {{site.data.keyword.baas_full_notm}} cluster
{: #download_the_vaio_filter_from_the_cohesity_cluster}

To download the VAIO filter from the {{site.data.keyword.baas_full_notm}} cluster:

1. Navigate to **Data Protection** > **Sources**.

2. Click **Cohesity Agent Download**.

3. In the **CDP VMware IO Filter** field of the **Download Agents** dialog, perform the following:

    1. Use the drop-down that displays the default ESXi version, **6.7**, to pick the ESXi version of the host in which you want VAIO deployed. Clicking on the drop-down displays both **6.7** and **7.0**. Select **6.7** or **7.0** based on the following:

        *   If all the ESXi hosts in the cluster are of versions earlier than 7.0, choose **6.7**.

        *   If all the ESXi hosts in the cluster are of versions later than 7.0, choose **7.0**.


        If your cluster has 6.7 and 7.0 ESXi hosts, contact your {{site.data.keyword.baas_full_notm}} account team to enable this option.

    2. Click the **Download** icon (![](../../Resources/Images/virtualization/download-VAIO_33x33.png)).


The VAIO filter’s installer (the VIB-vSphere Installation Bundle) is downloaded to your local system. [Generate the vibURL required to deploy the VAIO filter](#Generate) in the vCenter MOB interface by directly hosting the downloaded installer on an HTTP server accessible from the vCenter server and the ESXi hosts where the VAIO filter needs to be installed. Your {{site.data.keyword.baas_full_notm}} cluster can also act as the HTTP web server.

### Generate the url from the {{site.data.keyword.baas_full_notm}} cluster
{: #generate_the_url_from_the_cohesity_cluster}

To generate the URL for the VAIO filter:

1. Construct the URL as follows:

    **Format**

    <URL of the HTTP server or the Cohesity cluster>/files/bin/installers/

    **Example**

    http://10.2.111.11/files/bin/installers/

    When you enter the constructed URL in the address bar of the browser, all the I/O filters available on the {{site.data.keyword.baas_full_notm}} cluster are displayed.

    ![](../../Resources/Images/virtualization/download-io-filter.png)

2. From the displayed list, decide which I/O filter to download based on the ESXi version of the host in which you want VAIO deployed:

    *   If all the ESXi hosts in the cluster are of versions earlier than 7.0, choose _cohfilter-1.0.4-ee96825b-certified.zip_

    *   If all the ESXi hosts in the cluster are of versions 7.0 or later, choose _VMW-esx-7.0.0-coh-cohfilter-1.0.3-44d0c2a2-certified.zip_

3. To copy the link to the clipboard, right-click the I/O filter and select **Copy Link Address**.

    You use the copied link address to install the VAIO filter in VMware MOB interface.


For details on how to install, uninstall, and upgrade the VAIO filter in the ESXi cluster, see [VMware MOB API documentation for IoFilterManager](https://developer.vmware.com/apis/196/vsphere/doc/vim.IoFilterManager.html#installIoFilter){: external}.

## Upgrade cdp i/o filter
{: #upgrade_cdp_io_filter}

To upgrade the {{site.data.keyword.baas_full_notm}} I/O Filter from the {{site.data.keyword.baas_full_notm}} cluster:

1. Navigate to **Data Protection** > **Sources**.

    The Sources page displays a list of sources registered with the {{site.data.keyword.baas_full_notm}} cluster.

2. Select the VMware vCenter Server that has CDP enabled for its protected objects.

    The Protected Objects tab displays the list of protected objects.

3. Select the All Objects tab and select the Physical Hierarchy View.
4. Navigate to a CDP protected ClusterComputeResource. Optionally you can use the filter option available on the page to view the protected ClusterComputeResource.

    An upgrade icon (![](../../Resources/Images/CDPUpgrade_24x22.png)) is displayed next to the CDP protected object if an I/O Filter upgrade is available.

5. Click the upgrade icon (![](../../Resources/Images/CDPUpgrade_24x22.png)) next to the protected object, and then select **Upgrade CDP I/O Filter**.

    The Upgrade CDP I/O Filter page appears.

6. Click **Continue**.

*   {{site.data.keyword.baas_full_notm}} will upgrade the I/O Filters on all the ESXi hosts and VMs belonging to the same VMware ESXi cluster of the selected object.

*   {{site.data.keyword.baas_full_notm}} will perform the upgrade of the I/O Filter on the ESXi hosts only if the ESXi hosts in the cluster are in maintenance mode. {{site.data.keyword.baas_full_notm}} will withhold the upgrade till the ESXi hosts go to maintenance mode.


## Uninstall cdp i/o filter
{: #uninstall_cdp_io_filter}

Ensure these prerequisites are met before you uninstall the {{site.data.keyword.baas_full_notm}} I/O Filters attached to VMs on the ESXi hosts:

*   If the VMs are not migrated to another cluster that has a VM Storage Policy with {{site.data.keyword.baas_full_notm}} I/O Filter, then ensure that the {{site.data.keyword.baas_full_notm}} I/O Filter is not attached to any VMs.
*   This procedure requires the ESXi hosts to be in maintenance mode. You should shut down the VMs to put the ESXi hosts in maintenance mode.
*   If the ESXi hosts are set up with DRS mode, then moving an ESXi host in maintenance mode migrates the VMs to another ClusterComputeResource which has the same filter installed. You can perform a rolling uninstallation to remove the I/O Filter associated with all the hosts to remove the filter definition at the cluster level.

After detaching the I/O Filter from all the VMs, it is recommended to remove the VM Storage Policy from the vSphere created by the {{site.data.keyword.baas_full_notm}} cluster.

To uninstall the {{site.data.keyword.baas_full_notm}} I/O Filter from the {{site.data.keyword.baas_full_notm}} cluster:

1. Navigate to **Data Protection > Sources**.

    The Sources page displays a list of sources registered with the {{site.data.keyword.baas_full_notm}} cluster.

2. Select the VMware Cluster that has CDP enabled for its protected objects.

    The Protected Objects tab displays the list of protected objects.

3. Select the **All Objects** tab, and select the Physical Hierarchy View.
4. Navigate to a CDP protected object. Optionally you can use the filter option available on the page to view the protected object.

    A filter icon (![](../../Resources/Images/CDPUninstall_24x21.png)) with the following message displayed next to the CDP protected object if an I/O Filter upgrade is available.

    Uninstall CDP IO Filter

5. Click the icon (![](../../Resources/Images/CDPUninstall_24x21.png)) next to the protected object, and then select **Uninstall CDP IO Filter**.

    The uninstall CDP IO Filter page appears.

6. Click **Continue**.

Once initiated, the {{site.data.keyword.baas_full_notm}} VMware I/O filter installation will continue to completion until CDP is removed from a protection policy.

## Considerations
{: #considerations}

*   If a new host is added to the Cluster Compute Resource of the VMware vCenter, CDP will be auto-deployed on the newly added host and the I/O Filter Daemon will start running on it.

*   The {{site.data.keyword.baas_full_notm}} cluster will continue to protect the CDP-enabled VMs that are moved (or migrated) to another ESXi host using VMware Storage vMotion.

*   You can perform a point-in-time recovery of VMs, files and folders, Virtual Disks, and IVM of a CDP-based Protection Groups by selecting the points in the timeline bar on the recovery page. This timeline will also display when the VADP backups were taken and you can optionally recover from the VADP snapshots. For detailed instructions on recovering VMs, files and folder, Virtual Disks, and IVM, see Recovery.

*   If you are manually attaching the VM Storage Policy, then ensure to:

    *   Attach a Storage Policy to the VMs before a CDP-enabled Protection Group is created for these VMs.

    *   While creating the VM Storage Policy, select the **Enable host based rules** checkbox in the **Policy structure** tab and create rules for Replication data service under **Host based services** with the following settings:

        *   **Provider**: cohfilter

        *   **vCenterUUID**: <vCenterUUID>

        *   **CohesityID**: <ClusterId:IncarnationId>

        *   **CohesityClusterEndpoint**: <endpoint>:20004

        *   **HeartbeatInterval**: 30


        You can view the default Storage Policy created by {{site.data.keyword.baas_full_notm}} on the VMware vCenter as a reference for the above settings.

*   If there is no VM Storage Policy attached to the VMs, then when the Protection Group is run, {{site.data.keyword.baas_full_notm}} will automatically create and attach the VM Storage Policy required for CDP.

*   A CDP-enabled Protection Group supports a maximum of 25 VMs per node and 40 VMs per ESXi host, in each {{site.data.keyword.baas_full_notm}} cluster. Attempts to create Protection Groups that exceed this limit fail with the following error messages:

    The limit on the number of CDP-protected VMs from an ESXi host is being exceeded on the host.

    The maximum number of VMware VMs supported on this cluster for CDP is <number of nodes in the {{site.data.keyword.baas_full_notm}} cluster times 25>

*   If the Storage Policy attached to the VM does not have the correct CDP attributes configured, then a corresponding error message is displayed for that VM on the CDP status page (**Protection****\> CDP** > **Protected Objects**).

*   Do not make any updates to the {{site.data.keyword.baas_full_notm}} managed I/O Filter or the VM Storage Policy directly from vCenter as this will cause the CDP to fail.

*   CDP will stop if the FQDN of the {{site.data.keyword.baas_full_notm}} cluster is changed after the CDP protection runs are configured for a VMware VM. To reenable CDP, update the new FQDN in the CDP Storage policy and reapply the policy on all the VMs associated with that policy.

*   If a Protection Group is deleted or paused, then you must either manually detach the VM Storage Policies that were manually attached or update the Storage Policies to remove the CDP attributes. Once you resume the Protection Group, you can reattach the VM Storage Policies with CDP attributes.

*   If the time stamp on the {{site.data.keyword.baas_full_notm}} cluster and the workload VM are not in sync, then the CDP protection window displayed in the {{site.data.keyword.baas_full_notm}} cluster will be inaccurate.

*   You can select point-in-time restore only for individual VMs and not for a Protection Group.

*   The performance of your vCenter server might be impacted if many VMs are added to a CDP policy at once.

*   {{site.data.keyword.baas_full_notm}} clusters monitor the I/O throughput of CDP-enabled objects continuously. The guardrails help you track the resource utilization, performance, and stability of the {{site.data.keyword.baas_full_notm}} cluster.

    When the I/O throughputs exceed the pre-defined thresholds, appropriate alert messages are displayed under the Message column of the Protection Run details page.

    *   **CDP disabled for a set duration**. If the I/O throughput of an object sporadically increases above the threshold, causing a resource scarcity on the {{site.data.keyword.baas_full_notm}} cluster, CDP is disabled for a duration determined by {{site.data.keyword.baas_full_notm}}.

        **Alert Message**

        _CDP was temporarily disabled at <hh:mm> because of High IO Throughput. It will be enabled automatically after <default period set by Cohesity>._

        CDP is automatically re-enabled once the time duration set by {{site.data.keyword.baas_full_notm}} expires. To modify the time duration set by {{site.data.keyword.baas_full_notm}}, contact your {{site.data.keyword.baas_full_notm}} account team.

    *   **CDP disabled temporarily**: If the I/O throughput of a CDP-enabled object exceeds the threshold for a prolonged period, CDP is disabled until you enable it again.

        **Alert Message**

        CDP disabled at <hh:mm> PM because of continuous high I/O throughput. Please review the I/O throughput of an entity.

        Disabling CDP ensures the sustainability of the {{site.data.keyword.baas_full_notm}} cluster and prevents the objects that exceed the thresholds from affecting the other objects in the {{site.data.keyword.baas_full_notm}} cluster.

        When CDP is disabled with the preceding message, it is not enabled unless you manually enable it. You can review the I/O throughput of such entities and re-enable CDP if you like. You may also contact your {{site.data.keyword.baas_full_notm}} account team for more details.

*   CDP protection is not supported for the VMware VMs residing in the vVol datastores.

*   For CDP-enabled VMware VMs that use vSAN-based datastores, you must attach the storage policy manually. For details, see [CPD Offline Storage Attach for vSAN Datastores](https://support.cohesity.com/s/article/CDP-Offline-Policy-Attach-for-Vsan-datastores?internal=true){: external}.
