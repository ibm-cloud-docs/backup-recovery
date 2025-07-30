---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Ensure Adequate Privileges for {{site.data.keyword.baas_full_notm}} on the Source
{: #ensure-adequate-privileges-overview}

The {{site.data.keyword.baas_full_notm}} must perform various actions on the source. For the {{site.data.keyword.baas_full_notm}} to perform these actions, the user specified to connect to the source (the one used to register the source) must have adequate privileges. For information about each source type, refer to the appropriate section:

*   [VMware vCenter](#vCenter)
*   [Standalone ESXi](#ESXi)

*   [vCloud Director](#vCloud)

*   [NetApp](#NetApp)
*   [Dell EMC Isilon](#Isilon)
*   [Storage Snapshot Integration](#Storage_Snapshot)
*   [Active Directory](#Active)
*   [Linux Servers](#Linux)
*   [Windows Servers](#Windows)

## Create a Role with Necessary vCenter Server Privileges
{: #ensure-adequate-privileges-role-necessary}

The {{site.data.keyword.baas_full_notm}} performs various VMware actions such as creating VMware snapshots, restoring VMware snapshots to ESXi hosts using vSphere Storage vMotion and creating new VMs by mounting the new VMDKs. For the {{site.data.keyword.baas_full_notm}} to have adequate vCenter server privileges, the VirtualCenter user specified to connect to the source (vCenter Server ) must have the role privileges listed in [Role Privileges for vCenter Server](#Role) and that VirtualCenter user must be associated with vCenter objects that the {{site.data.keyword.baas_full_notm}} interfaces with.

By default, the **VMware Consolidated Backup user (sample)** role does not have adequate privileges for the VMware activities performed by the {{site.data.keyword.baas_full_notm}}.

**Create a Role with adequate vCenter Server Privileges**
{: #ensure-adequate-privileges-role-adequate}

1.  Using the vSphere Web Client, log in to the vCenter server that contains the VM to protect.
2.  Create a new role.
3.  Select the appropriate privileges for the new role as listed below.
4.  Assign this role to the vCenter users that will access the {{site.data.keyword.baas_full_notm}}. Assign this role to users at the top vCenter object level and enable **Propagate to children** check box.
5.  When registering a source on the {{site.data.keyword.baas_full_notm}}, specify a VirtualCenter user assigned to a role with the required {{site.data.keyword.baas_full_notm}} privileges (defined below).

For more information on VMware vCenter permissions, refer the knowledge base article, [Required {{site.data.keyword.baas_full_notm}} Permissions for VMware vCenter Environments](/docs/backup-recovery?topic=Required-Cohesity-permissions-for-VMware-vCenter-environments).

### Role Privileges for vCenter Server
{: #ensure-adequate-privileges-role-vcenter}

The following table lists all the privileges that you need to use a {{site.data.keyword.baas_full_notm}} with VMware vCenter.

*   The [Session view and stop sessions privilege](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.security.doc/GUID-84BDB0C4-AAB8-41AA-BB52-66C5E2A5B5A1.html?hWord=N4IghgNiBcIG4EsCmB3ABGAdgEzQZwBcB7AB3yTzwSMxAF8g) is applied to all the tasks to close any sessions after a process finishes. The [Cryptographic privileges](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.security.doc/GUID-660CCB35-847F-46B3-81CA-10DDDB9D7AA9.html) for Add Disk and Direct Access are only needed for encrypted VMs. The [Configure Datastore privilege](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.security.doc/GUID-B2426ACC-D73F-4732-8BBC-DE9B1B2263D9.html) is only needed for throttling or storage snapshot integration.
*   Starting with VMware vSphere 8.0 version, the **Profile-driven Storage** privilege level is replaced with **VM Storage Policies**.



| Privilege Level |     | Permissions Required When Using |     |
| --- | --- | --- | --- |
| VMware Snapshot Only | Storage Snapshot Integration<br><br>(Pure, Nimble, NetApp) |
| --- | --- | --- | --- |
| Cryptographic operations |     | *   Add Disk<br>*   Direct Access<br>*   Encrypt new<br><br>Cryptographic privileges are only needed for encrypted VMs. | *   Add Disk<br>*   Direct Access<br>*   Encrypt new<br><br>Cryptographic privileges are only needed for encrypted VMs. |
| Datastore |     | *   Allocate space<br>*   Browse datastore<br>*   Configure datastore<br>*   Low level file operations<br>*   Move datastore<br>*   Remove file<br><br>Configure Datastore privilege is only needed for throttling. | *   Allocate space<br>*   Browse datastore<br>*   Configure datastore<br>*   Low level file operations<br>*   Move datastore<br>*   Rename datastore<br>    <br>*   Remove file<br><br>Configure Datastore privilege is only needed for throttling. |
| Folder |     | *   Create folder<br>*   Delete folder | *   Create folder<br>*   Delete folder |
| Global |     | *   Disable Methods<br>*   Enable Methods<br>*   Licenses<br>*   Log event<br>*   Manage custom attributes<br>*   Set custom attribute | *   Disable Methods<br>*   Enable Methods<br>*   Licenses<br>*   Log event<br>*   Manage custom attributes<br>*   Set custom attribute |
| Host | Configuration | *   Maintenance<br>    <br>*   Query Patch<br>    <br>*   Storage partition configuration | Storage partition configuration |
| Local operations | N/A | Delete virtual machine |
| Network |     | Assign network | Assign network |
| Profile-driven Storage |     | *   Profile-driven storage update<br>    <br>*   Profile-driven storage view |     |
| Resource |     | *   Assign virtual machine to resource pool<br>*   Migrate powered off virtual machine<br>*   Migrate powered on virtual machine | *   Assign virtual machine to the resource pool<br>*   Migrate powered off virtual machine<br>*   Migrate powered on virtual machine |
| Session |     | View and stop sessions | View and stop sessions |
| Virtual Machine | Change Configuration | *   Acquire disk lease<br>*   Add existing disk<br>*   Add new disk<br>*   Add or remove device<br>*   Advanced configuration<br>*   Change Settings<br>*   Change Swapfile placement<br>*   Configure Raw device<br>*   Display connection settings<br>    <br>*   Remove disk<br>*   Rename\*<br>*   Toggle disk change tracking<br>*   Modify device settings<br>*   UpgradeVirtualHardware | *   Acquire disk lease<br>*   Add existing disk<br>*   Add new disk<br>*   Add or remove device<br>*   Advanced configuration<br>*   Change Settings<br>*   Change Swapfile placement<br>*   Configure Raw device<br>*   Remove disk<br>*   Rename\*<br>    <br>*   Toggle disk change tracking<br>*   Modify device settings<br>*   UpgradeVirtualHardware |
| Change Configuration (For Runbook) | *   Change CPU count<br>*   Change Memory<br>*   Change Settings<br>*   Change resource<br>*   Modify device settings<br>*   Rename | *   Change CPU count<br>*   Change Memory<br>*   Change Settings<br>*   Change resource<br>*   Modify device settings<br>*   Rename |
| Edit Inventory | *   Create new<br>*   Register<br>*   Remove<br>    <br>*   Unregister | *   Create new<br>*   Register<br>*   Remove<br>*   Unregister |
| Guest Operations | *   Guest operation modifications<br>*   Guest operation program execution<br>*   Guest operation queries | *   Guest operation modifications<br>*   Guest operation program execution<br>*   Guest operation queries |
| Interaction | *   Connect devices<br>*   Guest operating system management by VIX API<br>*   Power off<br>*   Power on | *   Connect devices<br>*   Guest operating system management by VIX API<br>*   Power off<br>*   Power on |
| Provisioning | *   Allow disk access<br>    <br>*   Allow read-only disk access<br>*   Allow virtual machine download<br>*   Customize guest (For Runbook)<br>    <br>*   Mark as template | *   Allow disk access<br>*   Allow read-only disk access<br>*   Allow virtual machine download<br>*   Customize guest(For Runbook)<br>*   Mark as template |
| Snapshot management | *   Create snapshot<br>*   Remove snapshot<br>*   Revert snapshot | *   Create snapshot<br>*   Remove snapshot<br>*   Revert snapshot |
| vApp |     | *   Add virtual machine<br>*   Assign resource pool<br>*   Unregister | *   Add virtual machine<br>*   Assign resource pool<br>*   Unregister |
| vSphere Tagging |     | *   Assign or Unassign vSphere Tag<br>    <br>*   Assign or Unassign vSphere Tag on Object | Assign or unassign tag |

\*Rename permission is required for a copy recovery.

## Role Privileges for Standalone ESXi
{: #ensure-adequate-privileges-role-standalone}

The role privileges required by the {{site.data.keyword.baas_full_notm}} for standalone ESXi are listed below.

| Privilege Level |     | Required Permissions |
| --- | --- | --- |
| Datastore |     | AllocateSpace  <br>Browse  <br>Config—Required if Source Datastore throttling is enabled  <br>Delete—Required if Source Datastore throttling is enabled  <br>DeleteFile  <br>FileManagement  <br>Move—Required if Source Datastore throttling is enabled  <br>Rename—Required if Source Datastore throttling is enabled  <br>UpdateVirtualMachineFiles—Required if Source Datastore throttling is enabled  <br>UpdateVirtualMachineMetadata—Required if Source Datastore throttling is enabled |
| Folder |     | Create  <br>Delete |
| Global |     | DisableMethods  <br>EnableMethods  <br>Licenses  <br>LogEvent<br><br>Manage custom attributes<br><br>Set custom attribute |
| Host | Configuration | Storage |
| Local operations | Delete virtual machine |
| Network |     | Assign |
| Resource |     | AssignVMToPool  <br>ColdMigrate  <br>HotMigrate |
| System |     | Anonymous  <br>Read  <br>View |
| vApp |     | AssignResourcePool  <br>AssignVM  <br>Unregister |
| **Session** |     | View and stop sessions |
| Virtual machine | Configuration | AddExistingDisk  <br>AddNewDisk  <br>AddRemoveDevice  <br>AdvancedConfig  <br>CPUCount  <br>ChangeTracking  <br>DiskLease  <br>EditDevice  <br>HostUSBDevice  <br>Memory  <br>RawDevice  <br>ReloadFromPath  <br>RemoveDisk  <br>Rename  <br>ResetGuestInfo  <br>Resource  <br>Settings  <br>SwapPlacement  <br>UpgradeVirtualHardware |
| GuestOperations | Execute  <br>Modify  <br>Query |
| Interact | GuestControl  <br>PowerOff  <br>PowerOn |
| Inventory | Create  <br>Delete  <br>Register  <br>Unregister |
| Provisioning | DiskRandomRead  <br>GetVmFiles |
| State | Create snapshot  <br>Remove snapshot  <br>Revert to snapshot |
| Cryptographic Operations |     | Add Disk<br><br>Direct Access<br><br>Encrypt<br><br>Migrate |



## vCloud Director
{: #ensure-adequate-privileges-vcloud}

The following table lists all the privileges that you need to use a {{site.data.keyword.baas_full_notm}} with vCD.


| Permission | Application | Privilege |     |
| --- | --- | --- | --- |
| View | Manage |
| --- | --- | --- | --- |
| Access Control | Organization | Organization | Edit Organization Properties |
|     |     | View Organizations |     |
|     |     | View vApp ACL |     |
|     | User | View Group/ User |     |
| Administration | General | View Error Details |     |
|     |     | Perform Administrator queries |     |
|     |     | View SSL Settings |     |
|     |     | Test Connection |     |
|     |     | View Trusted Certificates |     |
| Compute | Organization Virtual Data Center (VDC) | View Alternate Administrator version of Compute Policies for an Organization VDC | Open Organization VDC Network in vSphere |
|     |     | View Compute Policies for an Organization VDC | Create a Disk |
|     |     | View Disk Encryption Status | Delete a Disk |
|     |     | View Disk Properties | Edit Disk Properties |
|     |     | View Organization VDC Resource Pool | Open Organization VDC Resource Pool in vSphere |
|     |     | View Organization VDC Storage Policy Capabilities | Create a Shared Disk |
|     |     | View Organization VDC Extended Properties | Edit Organization VDC Storage Policy |
|     |     | View Organization VDCs | Open Organization VDC Storage Policy in vSphere |
|     |     | View stranded items | Remove Organization VDC Storage Policy |
|     |     |     | Set Default Storage Policy |
|     |     |     | Create Organization VDC |
|     |     |     | Delete Organization VDCs |
|     |     |     | Edit Organization VDC Name and Description |
|     |     |     | Enable / Disable Organization VDC |
|     |     |     | Edit Organization VDC Extended Properties |
|     |     |     | Edit VM-VM Affinity Rule |
|     |     |     | Open Organization VDC Storage Policy in vSphere |
|     |     |     | Remove Organization VDC Storage Policy |
|     | Provider VDC | View Compute Policies for a Provider VDC | Manage Compute Policies for a Provider VDC |
|     |     | View Provider VDC Resource Pool | Migrate Provider VDC Resource Pool VMs |
|     |     | View Provider VDC Storage Policy | Open Provider VDC Resource Pool in vSphere |
|     |     | View Provider VDC | Edit Provider VDC Storage Policy |
|     |     |     | Enable / Disable Provider VDC Storage Policy |
|     |     |     | Open Provider VDC Storage Policy in vSphere |
|     |     |     | Remove Provider VDC Storage Policy |
|     |     |     | Add Provider VDC Resource Pool |
|     |     |     | Create / Delete Provider VDC |
|     |     |     | Delete Provider VDC Resource Pool |
|     |     |     | Edit Provider VDC |
|     |     |     | Enable / Disable Provider VDC |
|     |     |     | Enable / Disable Provider VDC Resource Pool |
|     |     |     | Enable Provider VDC VXLAN |
|     |     |     | Merge Provider VDC |
|     |     |     | Manage System Storage Policy Supported Entity Types |
|     | vApp | View vApp Shadow VMs | Change vApp Template Owner |
|     |     | View Encryption Status of VMs and VM's disks | Force storage lease expiration for vApp templates |
|     |     | View VM metrics | Import vApp Template |
|     |     |     | Open vApp Template in vSphere |
|     |     |     | Preserve All ExtraConfig Elements During OVF Import and Export |
|     |     |     | Preserve Ethernet-Coalescing ExtraConfig Elements during OVF Import and Export |
|     |     |     | Preserve Latency ExtraConfig Elements during OVF Import and Export |
|     |     |     | Preserve ExtraConfig Elements During OVF Import and Export if they match patterns specified by the system administrator in the 'vapp.allowed.extra.config' configuration property |
|     |     |     | Preserve NUMA Node Affinity ExtraConfig Elements during OVF Import and Export |
|     |     |     | Change Owner |
|     |     |     | Copy a vApp |
|     |     |     | Create / Reconfigure a vApp |
|     |     |     | Delete a vApp |
|     |     |     | Download a vApp |
|     |     |     | Edit vApp Properties |
|     |     |     | Edit VM Compute Policy |
|     |     |     | Edit VM CPU |
|     |     |     | Edit VM CPU and Memory Reservation / Limit / Shares in all VDC types |
|     |     |     | Edit VM Hard Disk |
|     |     |     | Edit VM Memory |
|     |     |     | Edit VM Network |
|     |     |     | Edit VM Properties |
|     |     |     | Enter / Exit vApp Maintenance Mode |
|     |     |     | Force runtime lease expiration for vApps |
|     |     |     | Force storage lease expiration for vApps |
|     |     |     | Import vApp |
|     |     |     | Manage maintenance mode for vApps |
|     |     |     | Manage VM Password Settings |
|     |     |     | Open vApp in vSphere |
|     |     |     | Start / Stop / Suspend / Reset a vApp |
|     |     |     | Share a vApp |
|     |     |     | Create / Revert / Remove a Snapshot |
|     |     |     | Upload a vApp |
|     |     |     | Access to VM Console |
|     |     |     | Edit / View VM Boot Options |
|     |     |     | View Compliance of vApp VMs |
|     |     |     | Migrate / Force Undeploy / Relocate / Consolidate vApp VMs |
|     |     |     | Allow metadata mapping domain to vCenter |
| Extensions | VMware vCD Extension | View Tenant Portal Plugin Information |     |
| Infrastructure | Datastore | View Datastore | Delete Datastore |
|     |     |     | Edit Datastore |
|     |     |     | Enable/Disable Datastore |
|     |     |     | Open Datastore in vSphere |
|     | Host | View Host | Enable / Disable a Host |
|     |     |     | Open a Host in vSphere |
|     |     |     | Prepare / Unprepare a Host |
|     | Resource Pool | Open Resource Pool |     |
|     |     | View Resource Pool |     |
|     | vCenter | View vCenter | Reload a VM from vSphere |
|     |     | View vCenter server | Attach / Detach vCenter |
|     |     |     | Enable / Disable vCenter |
|     |     |     | Open vCenter Server in vSphere |
|     |     |     | Refresh vCenter |
| Libraries | Catalog | View Private and Shared Catalogs within Current Organization |     |
| Networking | Organization VDC Network | View Properties | Manage Direct Organization VDC Network |
|     |     |     | Edit Properties |
|     |     |     | Import Network |
|     |     |     | Create Shared Organization VDC Network |
|     | Provider Network | View Provider Network | Create Provider Network |
|     |     |     | Edit Provider Network |
|     |     |     | Delete Provider Network |
|     |     |     | Open Provider Network in vSphere |



## Dell EMC Isilon
{: #ensure-adequate-privileges-dell-emc}

### User Permissions to Register Isilon
{: #ensure-adequate-privileges-user-permissions}

To register an Isilon NAS with {{site.data.keyword.baas_full_notm}} Isilon Adapter, ensure that the following permissions are configured in the Isilon cluster:


| Name | Description | Access |
| --- | --- | --- |
| Platform API | To have access to Isilon’s APIs | Read-Only |
| Auth | To verify users and passwords | Read-Only |
| Cluster | To obtain cluster identity and settings | Read-Only |
| Job Engine | To read and write ChangeList jobs | Read/Write |
| Network | To obtain the network interfaces | Read-Only |
| SMB | To read the settings in SMB server | Read-Only |
| Snapshot | To fetch, create, and delete snapshots for shares and exports | Read/Write |
| NFS | To read and write settings to and from the NFS server.<br><br>This setting modifies the NFS export used to mount, such as /ifs. | Read/Write<br><br>If NFS backup is not required, the user must have **Read-Only** permission. |

### Snapshot IQ License
{: #ensure-adequate-privileges-snapshot}

You must have the SnapshotIQ license to take a snapshot on Isilon.

## Storage Snapshot Integration
{: #ensure-adequate-privileges-storage-snapshot}

The {{site.data.keyword.baas_full_notm}} integrates natively with Pure Storage or Nimble Storage to improve snapshot-based data protection. To leverage storage snapshot integration, {{site.data.keyword.baas_full_notm}} needs to perform the following tasks:

1.  Discover, configure, and register iSCSI LUN.
2.  Perform VMFS datastore updates and configure storage I/O control on datastore.
3.  Rename, mount / unmount, and delete the datastore.
4.  Remove iSCSI LUN.
5.  Delete VM and folder.

**User Permission**

For storage snapshot integration, the following additional privileges are required along with the permissions described in [Role Privileges for vCenter Server](#Role) .

| Privilege Level |     | Required Permissions |
| --- | --- | --- |
| **Virtual Machine** | Change Configuration | Acquire disk lease<br><br>Add or remove device<br><br>Change Settings |
| Provisioning | Allow disk access |

## Active Directory
{: #ensure-adequate-privileges-active-dir}

For more information, see [Active Directory Protection Requirements](https://docs.cohesity.com/7_1_1/Web/UserGuide/Content/ActiveDirectory/ADRequirements.htm).

## Linux Servers
{: #ensure-adequate-privileges-linux-servers}

For more information, see [Install and Manage the Agent on Linux Servers](/docs/backup-recovery?topic=backup-recovery-install-and-manage-the-agent-on-linux-servers).

## Windows Servers
{: #ensure-adequate-privileges-windows-servers}

Specify the local or AD user with administrative privileges on the server. For more information, see [Install and Manage the Agent on Windows Servers](/docs/backup-recovery?topic=backup-recovery-install-and-manage-the-agent-on-windows-servers).
