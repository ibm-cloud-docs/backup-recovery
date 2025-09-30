---

copyright:
  years: 2024, 2025
lastupdated: "2025-09-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Manage firewall ports
{: #manage_firewall_ports}

You must open certain ports in the firewall to allow the {{site.data.keyword.baas_full_notm}} to transmit and receive data. The cluster sends the following types of traffic over the network. You can isolate traffic on physical or logical networks to improve performance and security.

*   IPMI traffic for admins to access nodes, typically for pre-boot BIOS access and post-boot console access.
*   Management traffic:

    *   from Nodes to external services such as Active Directory, DNS, NTP and SNMP
    *   for admins to access the {{site.data.keyword.baas_full_notm}}
*   Backup and restore traffic for moving data between protected sources and the cluster.
*   Replication traffic for replicating data from one cluster to another cluster.
*   Data access (NAS) traffic for accessing data on the cluster by using protocols such as NFS, SMB or S3.
*   Archive and tiering traffic for moving data to external targets such as a cloud or tape system.
*   Support Channel traffic between nodes and the IBM Cloud Support servers.
*   Internal traffic between nodes on a cluster.

Using the {{site.data.keyword.baas_full_notm}} UI or CLI, you can configure application-based firewall rules and manage incoming traffic on a {{site.data.keyword.baas_full_notm}}. For more details, see [Manage Application-Based Firewall Rules](ManageRules.htm#top).

### Identify source ip of outgoing traffic
{: #identify_source_ip_of_outgoing_traffic}

The source IP of outgoing traffic depends on the matching static route’s outgoing interface. If there is no static route, the source IP depends on the default route's outgoing interface. Use the route list command to view the list of routes configured on an interface in a {{site.data.keyword.baas_full_notm}} and the IP address of the destination network.

In the following example, the route list command output shows the VLAN interface (intf\_group1.100) of a cluster configured with the default route.

iris\_cli route list

DEST NETWORK                  : 0.0.0.0/0

NEXT HOP                      : 10.1.100.1

INTERFACE GROUP NAME          : intf\_group1.100

For more details, see [Add a Static Route](../CLI/StaticRoute.htm).

## Cluster management
{: #cluster_management}

### Cluster upgrade
{: #cluster_upgrade}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | downloads.cohesity.com/oauth2/login<br><br>s3-us-west-2.amazonaws.com<br><br>CDN<br><br>[CloudFront](http://dipowd173xxgy.cloudfront.net/){: external} | 443 | TCP | For cluster upgrade packages. |
{: caption="" caption-side="bottom"}

### Agent upgrade
{: #agent_upgrade}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| Client | {{site.data.keyword.baas_full_notm}} | 80,443 | TCP | For Agent upgrade from UI. |
{: caption="" caption-side="bottom"}

### Troubleshooting
{: #troubleshooting}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| SSH client | {{site.data.keyword.baas_full_notm}} | 22  | TCP | Support Channel used by IBM Cloud Support for troubleshooting |
{: caption="" caption-side="bottom"}

## Networking
{: #networking}

### Agent communication
{: #agent_communication}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Backup agent | 50051 | TCP | Used for Backup agent and Agent less data transfer to the cluster. Needs to be open on all nodes on the cluster and any backup target using the Backup agent, such as SQL servers, physical servers and VMs running the {{site.data.keyword.baas_full_notm}} installed Agent.<br><br>{{site.data.keyword.baas_full_notm}}s pick any available ephemeral port to initiate communication to the agent service via port 50051. The firewall needs to allow the connection request from the {{site.data.keyword.baas_full_notm}} nodes if the destination port is 50051. | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Network services
{: #network_services}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | DNS | 53  | TCP/UDP | DNS, if an external DNS server is configured. | Management |
| {{site.data.keyword.baas_full_notm}} | DNS server | 5353 | UDP | mDNS multicasting. |
{: caption="" caption-side="bottom"}

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | External NTP Server (time.google.com ) | 123 | UDP | Requests to external NTP servers are sent via port 123. | Management |
| 323 | *   Required for cluster setup.<br>    <br>*   Chrony is the default implementation of NTP used by recent versions of CentOS, RHEL, and AlmaLinux. Open port 323 if you want to use the chronyc tool to monitor the synchronization status of Chrony and make changes if necessary. |
{: caption="" caption-side="bottom"}

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 123 | UDP | Time synchronization among {{site.data.keyword.baas_full_notm}} nodes during node-to-node communication is implemented via port 123. | Management |
| {{site.data.keyword.baas_full_notm}} | DNS | 53  | TCP/UDP | Required if an internal DNS server is configured. |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 5353 | UDP | mDNS multicasting. |
{: caption="" caption-side="bottom"}

### {{site.data.keyword.baas_full_notm}} ipmi network
{: #{{site.data.keyword.baas_full_notm}}_ipmi_network}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Admin (Browser) | {{site.data.keyword.baas_full_notm}} IPMI address on each node | 80, 443 | TCP | Required for HTTP and HTTPS | Management and IPMI |
| 3520, 5900 | Required for IKVM web browser access |
{: caption="" caption-side="bottom"}

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} (bond0 on each node) | {{site.data.keyword.baas_full_notm}} IPMI address on each node | 623 | UDP | IPMI, Intel standard manageability port. | IPMI |
| 7578 | TCP |
| Admin | 5120 | UDP/TCP | Required for virtual media mount. |
{: caption="" caption-side="bottom"}

### Smb and nfs media servers
{: #smb_and_nfs_media_servers}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| NFS Clients | {{site.data.keyword.baas_full_notm}} NFS target | 2049 | TCP | Required for NFS | Data Protection |
| SMB Clients | {{site.data.keyword.baas_full_notm}} SMB target | 445 | SMB filer functionality. SMB and SMB2 restore. | Data Access |
{: caption="" caption-side="bottom"}

### Nfs for {{site.data.keyword.baas_full_notm}} views
{: #nfs_for_{{site.data.keyword.baas_full_notm}}_views}

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| NFS Client | {{site.data.keyword.baas_full_notm}} (with NFS server) | 2049 (NFS server), 111 (port mapper) | NFS | Port 111 needs to be open on the NFS client and server.<br><br>NFS Linux clients do not need any additional configuration on the {{site.data.keyword.baas_full_notm}} | Required for mounting the Backup and recovery views. |
{: caption="" caption-side="bottom"}

### Dhcp server
{: #dhcp_server}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | DHCP Server | 67, 68 | TCP | Required for DHCP.<br><br>Required for SMB and ICMP. | Management |
{: caption="" caption-side="bottom"}

### Snmp
{: #snmp}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination  Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| SNMP | {{site.data.keyword.baas_full_notm}} | 161 | UDP | SNMP, if the cluster is configured to use SNMP. | Management |
{: caption="" caption-side="bottom"}

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination  Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Network Management System (NMS) | 162 | UDP | SNMP, if the cluster is configured to use SNMP traps. | Management |
{: caption="" caption-side="bottom"}

### Smtp
{: #smtp}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | SMTP server | Frequently opened ports on the SMTP server are either 25 or 465 or 587 | TCP | Depends on the port setting on the SMTP server. If SMTP opens a random port, the same port must be opened on the {{site.data.keyword.baas_full_notm}}. | Management |
{: caption="" caption-side="bottom"}

## Virtualization
{: #virtualization}

### Vmware
{: #vmware}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| ESXi host | VIPs and VLAN IPs on {{site.data.keyword.baas_full_notm}} | 111<br><br>2049 | TCP | Required for VMDK restore with Instant Recovery<br><br>Required for VMDK Restore<br><br>Required for Test &Dev clone<br><br>Required for File Recovery and Instant Volume Mount for Windows VMs when using agent-based recovery. | Recovery |
{: caption="" caption-side="bottom"}

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | ESXi Host | 902 | TCP | Ensure that SSL communication for TCP port 902 to the ESXi host is enabled, otherwise backups will fail. | Backup and Recovery |
{: caption="" caption-side="bottom"}

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | VMware vCenter<br><br>ESXi Host<br><br>VMware vCloud Director | 443 | TCP | The {{site.data.keyword.baas_full_notm}} uses HTTPS by default. Required for VMware API.<br><br>Required for VMware Tools-based file and folder recoveries.<br><br>Required for HTTPS/HTTPS(TLS) | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Microsoft scvmm and hyper-v
{: #microsoft_scvmm_and_hyper-v}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}

 
| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Windows machine with Backup agent installed | SCVMM | 5986 | TCP | SCVMM machines can be LOCALHOST or remote targets. | Management |
| Hyper-V | Hyper-V host machine is LOCALHOST. | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Host | 50051 | TCP | Required for both SCVMM machines and Hyper-V host machines. | Backup and Recovery |
{: caption="" caption-side="bottom"}

Incoming Traffic


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Destination or target Windows machine | 445 (SMB),<br><br>135 (WMI) | TCP | Required for agentless file recovery | Backup and Recovery |
{: caption="" caption-side="bottom"}

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Host | {{site.data.keyword.baas_full_notm}} | 445 | TCP | Required by Hyper-V host to access SMB data. | Recovery |
{: caption="" caption-side="bottom"}

### Nutanix acropolis
{: #nutanix_acropolis}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Data Services IP and IP of all CVMs on the Nutanix cluster. | 9440 | TCP | Used for REST API calls | Backup and Recovery |
| 3205, 3260 | iSCSI port for AHV backups. |
{: caption="" caption-side="bottom"}

### Rhv
{: #rhv}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Host | {{site.data.keyword.baas_full_notm}} | 50051 | TCP | Required for RHV host machines. | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Instant volume mount
{: #instant_volume_mount}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| ESXi host | VIPs and VLAN IPs on {{site.data.keyword.baas_full_notm}} | 111, 2049 | TCP | Instant Volume Mount | Recovery |
{: caption="" caption-side="bottom"}

### File restore to windows vm
{: #file_restore_to_windows_vm}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| ESXi host | VIPs and VLAN IPs on {{site.data.keyword.baas_full_notm}} | 111, 2049 | TCP | File restore to Windows VM | Recovery |
{: caption="" caption-side="bottom"}

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Host | {{site.data.keyword.baas_full_notm}} | 50051 | TCP | Required for persistent agent workflow for File level recovery and IVM. | Recovery |
{: caption="" caption-side="bottom"}

### File restore to linux vm
{: #file_restore_to_linux_vm}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Linux VM | VIPs and VLAN IPs on {{site.data.keyword.baas_full_notm}} | 111, 2049 | TCP | Required for file restore to Linux VM. | Recovery |
{: caption="" caption-side="bottom"}

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Host | {{site.data.keyword.baas_full_notm}} | 50051 | TCP | Required for Linux VM file restores. | Backup and Recovery |
{: caption="" caption-side="bottom"}

## Nas
{: #nas}

### Isilon
{: #isilon}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Isilon | {{site.data.keyword.baas_full_notm}} | 111 | TCP/UDP | Required for RPC connection. | Backup and Recovery |
| 300, 302, 304, 2049 | Required for NFS. |
| 8080 | Required for HTTP connection. |
| 443 | TCP | Required for HTTPS connection with Isilon. |
| 445 | Required for SMB. |
{: caption="" caption-side="bottom"}

> ### NetApp

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| NetApp | {{site.data.keyword.baas_full_notm}} | 111 | TCP/UDP | Required for RPC connection. | Backup and Recovery |
| 443 | Required for HTTPS connection with NetApp. |
| 635, 2049, 4045, 4046 | Required for NFS. |
| 445 | TCP | Required for SMB. |
| 1052, 10569 | TCP | Required for SnapDiff-based backup. |
| 3000 | TCP | Required by NetApp clusters to write to {{site.data.keyword.baas_full_notm}} S3 storage during SnapDiff backups. |
{: caption="" caption-side="bottom"}

### Generic nas
{: #generic_nas}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Generic NAS | {{site.data.keyword.baas_full_notm}} | 111 | TCP/UDP | Required for RPC connection. | Backup and Recovery |
| 635 2049 | Required for NFS. |
| 445 | TCP | Required for SMB. |
{: caption="" caption-side="bottom"}

### Pure flashblade and ibm�storage flashsystem
{: #pure_flashblade_and_ibm�storage_flashsystem}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Pure Storage | 3260, 3205 | TCP | iSCSI, and FC port for Pure Storage. | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | IBM Storage FlashSystem | 3260, 7443 | TCP | 3260 port for iSCSI<br><br>7443 port for API | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Elastifile
{: #elastifile}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Target | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Elastifile | {{site.data.keyword.baas_full_notm}} | 443 | TCP | Required for EMS/API operations. | Backup and Recovery |
| 111, 2049, 644, 4040, 4045 | TCP/UDP | Required for NFS Operations. |
{: caption="" caption-side="bottom"}

## Databases
{: #databases}

### Ms sql servers
{: #ms_sql_servers}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Server | {{site.data.keyword.baas_full_notm}} | 445 | TCP | Required for SQL servers. | Backup and Recovery |
| MS SQL server (any port) | {{site.data.keyword.baas_full_notm}} | 11113, 11117 | Required for VDI-based backup and restore of MS SQL database (using RemoteWANSnapFS). |
{: caption="" caption-side="bottom"}

### Oracle
{: #oracle}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Oracle Source (Backup agent) | {{site.data.keyword.baas_full_notm}} | 11113 | TCP | Source side deduplication for Oracle when using the Remote Adapter RMAN scriptx.<br><br>Secure gRPC used by Oracle SBT. | Backup and Recovery |
| Oracle Source | {{site.data.keyword.baas_full_notm}} | 11117 | TCP | Required from 6.8.1\_u3 onwards with the introduction of secure gRPC for Oracle SBT. | Backup and Recovery |
{: caption="" caption-side="bottom"}

## Postgresql
{: #postgresql}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| PostgreSQL | {{site.data.keyword.baas_full_notm}} | 11113 | TCP/IP | Required for physical backups and recovery at the cluster level. | Backup and Recovery |
{: caption="" caption-side="bottom"}

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | PostgreSQL | 50051, 59999 | TCP/IP | Required for physical backups and recovery at the cluster level. | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Sap hana
{: #sap_hana}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | DestinatiAon Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| SAP HANA | {{site.data.keyword.baas_full_notm}} | 11113 | TCP | Access to {{site.data.keyword.baas_full_notm}} views. | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Cassandra
{: #cassandra}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Cassandra nodes | 22  | TCP | Required for Cassandra backup.<br><br>Required for Cassandra optimized restore/DSE 6.x restore. | Backup and restore |
| 7000 | Required for Cassandra bulk restore. | Restore |
| 7001 | Required for Cassandra bulk restore incase of ssl enabled cluster. |
| 7199 | Required for Cassandra repository registration.<br><br>Required for Cassandra backup (snapshot operation).<br><br>Required for Fetching Cassandra configurations (list of nodes, token map etc). | Management |
| 8983 | Required for Cassandra solr index management. |
| 9042 | Required for Cassandra backup.<br><br>Required for Cassandra restore.<br><br>Required for Cassandra source registration. |
| 9160 | Required for Cassandra bulk restore (DSE 4.x / Cassandra 2.x). |
{: caption="" caption-side="bottom"}

### Mongodb
{: #mongodb}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | MongoDB Nodes | 27017, 27018, 27019 | TCP | Required for MongoDB backups and recovery. | Backup and Recovery |
{: caption="" caption-side="bottom"}

## Open-source software platform
{: #open-source_software_platform}

### Hadoop - hdfs
{: #hadoop_-_hdfs}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}

The following ports must be open to enable communication between {{site.data.keyword.baas_full_notm}} and **Cloudera Data Platform 7.1.x**. These are the default ports used. Refer the configuration in Cloudera Manager if you have overridden these ports.


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 8020 | TCP | Required to connect to Namenode through native protocol | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 9870 | TCP | Required to connect to Namenode through REST APIs (Non-SSL) | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 9871 | TCP | Required to connect to Namenode through REST APIs (SSL) | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 9864 | TCP | Required to connect to datanode through REST APIs (Non-SSL) | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 9865 | TCP | Required to connect to datanode through REST APIs (SSL) | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 9866 | TCP | Required to connect to datanode through native protocol | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 9292 | TCP | HTTP port required to connect to KMS | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 9294 | TCP | HTTPS port required to connect to KMS (SSL) | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Hadoop - hive
{: #hadoop_-_hive}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}

The following ports must be open to enable communication between {{site.data.keyword.baas_full_notm}} and **Cloudera Data Platform 7.1.x**.


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 9083 | TCP | Required to connect to Hive Metastore through the native protocol (Thrift) | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Hadoop - hbase
{: #hadoop_-_hbase}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}

The following ports are required for **Cloudera Data Platform 7.1.x**.


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 22001 | TCP | Required to connect to HMaster through native protocol | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 22002 | TCP | Required to connect to HMaster through HTTP protocol | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 22101 | TCP | Required to connect to Region server through native protocol | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 22102 | TCP | Required to connect to Region server through HTTP protocol | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | Hadoop Primary node | 2181 | TCP | Required to connect to Zookeeper service through native protocol | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Grpc
{: #grpc}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Backup Target (any port) | {{site.data.keyword.baas_full_notm}} | 11117 | TCP | Required for secure gRPC connections on cluster from backup target. | Backup |
{: caption="" caption-side="bottom"}

### Kubernetes
{: #kubernetes}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Kubernetes cluster | VIPs on {{site.data.keyword.baas_full_notm}} | 3000 | TCP | Velero S3 communication for metadata backup and restore. | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Cloud vms
{: #cloud_vms}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | *   **AWS**<br>    <br>    *   \*amazonaws.com<br>        <br>*   **Azure**<br>    <br>    *   Login.windows.net<br>        <br>    *   management.azure.com<br>        <br>    *   \*.blob.core.windows.net<br>        <br>*   **GCP**<br>    <br>    *   google.apis.com | 443 | TCP | Required for CloudSpin, CloudRetrieve, and Native Snapshot<br><br>XL CE and NGCE deployment.<br><br>Required for HTTPS connections. | Backup and Recovery |
| Fleet instances | 50051 | Required for CloudSpin to AWS EC2.<br><br>Required for Native Snapshot backup for AWS/GCP. |
| Cloud Source VMs | Required for File Level Recovery from Native Snapshot backup.<br><br>Required for Agent Protection and Restore for AWS EC2.<br><br>Required for Agent Protection and Restore for Azure VM. |
{: caption="" caption-side="bottom"}

## External targets
{: #external_targets}

### Cloud workflow
{: #cloud_workflow}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 29999 | TCP | Required for Archive Services. | Backup and Recover |
{: caption="" caption-side="bottom"}

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | *   **AWS**<br>    *   \*amazonaws.com<br>        <br>*   **Azure**<br>    <br>    *   \*.blob.core.windows.net<br>        <br>    *   \*vault.azure.net<br>        <br>*   **GCP**<br>    <br>    *   google.apis.com | 443 | TCP | Required for CloudArchive, CloudTier. | Backup and Recovery |
{: caption="" caption-side="bottom"}

### Amazon s3 clients
{: #amazon_s3_clients}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| S3 Clients | {{site.data.keyword.baas_full_notm}} | 3000 | TCP | Access to {{site.data.keyword.baas_full_notm}} views using S3 protocol. | Data Access |
{: caption="" caption-side="bottom"}

### Qstar
{: #qstar}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | QStar ASM host | 18082 | TCP | QStar External Target - Port number 18082 is set on the QStar host by default. If your QStar host is not using the default Web Services Port, update the port number. SOAP over HTTPS. | Backup and Recovery |
{: caption="" caption-side="bottom"}

## Storage arrays
{: #storage_arrays}

### Nimble storage
{: #nimble_storage}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Nimble Storage | 5392 | TCP | REST API | Backup and Recovery |
| 3260, 3205 | iSCSI port for Nimble Storage. |
{: caption="" caption-side="bottom"}

### Pure storage
{: #pure_storage}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Pure Storage | 3260, 3205 | TCP | iSCSI port for Pure Storage. | Backup and Recovery |
| {{site.data.keyword.baas_full_notm}} | IBM Storage FlashSystem | 3260, 7443 | TCP | 3260 port for iSCSI<br><br>7443 port for API | Backup and Recovery |
{: caption="" caption-side="bottom"}

## Active directory
{: #active_directory}

### Microsoft active directory (rbac)
{: #microsoft_active_directory_rbac}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Client | {{site.data.keyword.baas_full_notm}} | 53  | TCP/UDP | Serve DNS requests from an external source. | Management |
| {{site.data.keyword.baas_full_notm}} | Active Directory | 445 | TCP | Required only when initially joining the Cluster to Active Directory. |
{: caption="" caption-side="bottom"}

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Kerberos Key Distribution Center (AD) | 88  | TCP/UDP | Required for Keberos if the cluster is configured to use Active Directory. | Management |
| LDAP | 389 | Required if the cluster is configured to use Active Directory or LDAP. | Management |
| Active Directory | 137 | TCP | Required only when initially joining the Cluster to Active Directory. | Management |
| 139 | Required only when initially joining the Cluster to Active Directory (for the NetBIOS session service). | Management |
|     | 636 | TCP/UDP | Required to make secure connections between LDAP clients and servers. | Management |
{: caption="" caption-side="bottom"}

## Microsoft exchange servers
{: #microsoft_exchange_servers}

No specific ports need to be opened to protect Microsoft Exchange Servers, provided the following requirements have been met as part of your cluster configuration:

*   The ports listed in the [Physical Servers](#Physical) section are open to allow communication between your Physical Servers and {{site.data.keyword.baas_full_notm}} for backup operations.

*   The ports listed in the [SMB and NFS Media Servers](#SMB) section are open to allow communication between the {{site.data.keyword.baas_full_notm}} and SMB clients for recovery operations.

*   The ports listed in [Active Directory](#Active)section are open to allow communication between the {{site.data.keyword.baas_full_notm}} and the Active Directory for recovery operations.


## Physical servers
{: #physical_servers}

### Physical server backup
{: #physical_server_backup}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| Mount Target | {{site.data.keyword.baas_full_notm}} | 111 | TCP/UDP | NFS restores on Linux physical servers. |
| Local Host (Physical Windows or Linux Server) | Local Host (Physical Windows or Linux Server) | 59999 | TCP | Required only if local-to-local communication (used for self-monitoring and debugging purposes) is blocked by your firewall. |
{: caption="" caption-side="bottom"}

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Physical Server with Backup agent installed | 50051 | TCP | Linux and Windows physical server backups. |
{: caption="" caption-side="bottom"}

## Bare machine recovery
{: #bare_machine_recovery}

### Cbmr
{: #cbmr}


| Port | Protocol | Source | Target | Usage Notes |
| --- | --- | --- | --- | --- |
| 50051 | TCP | {{site.data.keyword.baas_full_notm}} | Physical server | Backup operations. |
| 111, 2049 | TCP | {{site.data.keyword.baas_full_notm}} | Physical server | NFS restore operations on a Linux server. |
| 445 | TCP | {{site.data.keyword.baas_full_notm}} | Physical server | SMB restore operations on a Windows server. |
| 443 | TCP | Recovery environment | {{site.data.keyword.baas_full_notm}} | To connect the CBMR recovery environment with the {{site.data.keyword.baas_full_notm}}. |
| 59999, 50051 | TCP | {{site.data.keyword.baas_full_notm}} | Recovery environment | To connect the CBMR recovery environment with the {{site.data.keyword.baas_full_notm}}. |
{: caption="" caption-side="bottom"}

### Cobmr
{: #cobmr}


| Port | Protocol | Source | Target | Usage Notes |
| --- | --- | --- | --- | --- |
| 50051 | TCP | {{site.data.keyword.baas_full_notm}} | Physical server | Backup operations. |
| 111, 2049 | TCP | {{site.data.keyword.baas_full_notm}} | Physical server | NFS restore operations on a Linux server. |
| 445 | TCP | {{site.data.keyword.baas_full_notm}} | Physical server | SMB restore operations on a Windows server. |
| 443 | TCP | Recovery environment | {{site.data.keyword.baas_full_notm}} | Port 443 is only required to perform a CoBMR recovery. It is not required for backup.<br><br>You need this port open on the target system to which you are performing the recovery. |
| 59999, 50051 | TCP | {{site.data.keyword.baas_full_notm}} | Recovery environment | To connect the CBMR recovery environment with the {{site.data.keyword.baas_full_notm}}. |
{: caption="" caption-side="bottom"}

## Key management service
{: #key_management_service}

### Kms (key management service)
{: #kms_key_management_service}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | SafeNet KeySecure server | 5696 | TCP | Required if the cluster is configured to use an external Key Management Service (KMS) such as SafeNet KeySecure. 5696 is the default KMIP port and can be changed when configuring the external KMS. For more information, see [External Key Management Service](../KMS/External KMS.htm). | Management |
{: caption="" caption-side="bottom"}

## Openstack integration
{: #openstack_integration}

### Swift protocol
{: #swift_protocol}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Swift Clients | {{site.data.keyword.baas_full_notm}} | 3001 | TCP | Access to {{site.data.keyword.baas_full_notm}} views using swift protocol. | Data Access |
{: caption="" caption-side="bottom"}



## Multitenancy
{: #multitenancy}

### Hybrid extender
{: #hybrid_extender}

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Hybrid Extender | DNS | 53  | TCP/UDP | DNS, if an external DNS server is configured. | Management |
| Client | Hybrid Extender | 53  | Serve DNS requests from an external source. | Management |
| SSH client | Hybrid Extender | 22  | TCP | Required for SSH server on Bifrost VM for Bifrost-RT | Management |
| Server | Hybrid Extender | 445 | Required for SQL servers. | Backup and Recovery |
| MS SQL server (any port) | Hybrid Extender | 11113, 11117 | VDI-based backup and restore of MS SQL database (using RemoteWANSnapFS) | Backup and Recovery |
| Hybrid Extender | Organization (Tenant) | 29994 | The internal port used on the organization's network for debugging and uploading the configuration file on the Hybrid Extender VM. | Management |
| NFS Clients | Hybrid Extender  NFS target | 2049 | Required for NFS | Data Protection |
| SMB Clients | Hybrid Extender  SMB target | 445 | SMB filer functionality. SMB and SMB2 restore. | Data Access |
| Linux VM | Hybrid Extender | 111, 2049 | File restore to Linux VM | Recovery |
| Hybrid Extender | {{site.data.keyword.baas_full_notm}} | 11117 | Required for Hybrid Extender with Oracle SBT | Backup and Recovery |
| ESXi host | Hybrid Extender | 111, 2049 | File restore to Windows VM | Recovery |
{: caption="" caption-side="bottom"}

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Hybrid Extender | NTP Server | 123 | UDP | NTP, if an external NTP server is configured. | Management |
| HTTP client | Hybrid Extender | 80  | TCP | HTTP | Management |
| Hybrid Extender | ESXi Host | 902 | Ensure that SSL communication for TCP port 902 to the ESXi host is enabled, otherwise backups will fail. | Backup and Recovery |
| Hybrid Extender | Active Directory | 137 | Required only when initially joining the Cluster to Active Directory. | Management |
| Hybrid Extender | Active Directory | 139 | Required only when initially joining the Cluster to Active Directory (for the NetBIOS session service). | Management |
| Hybrid Extender | Active Directory | 445 | Required only when initially joining the Cluster to Active Directory. | Management |
| Hybrid Extender | Kerberos Key Distribution Center (AD) | 88  | TCP/UDP | Required for Keberos if the cluster is configured to use Active Directory. | Management |
| Hybrid Extender | LDAP | 389 | Required if the cluster is configured to use Active Directory or LDAP. | Management |
| LDAPS | 636 |
{: caption="" caption-side="bottom"}

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| Hybrid Extender | DHCP Server | 67, 68 | TCP | DHCP.<br><br>SMB/ICMP | Management |
| Hybrid Extender | VMware vCenter<br><br>VMware Standalone ESXi Host<br><br>VMware vCloud Director | 443 | TCP | The {{site.data.keyword.baas_full_notm}} uses HTTPS by default. <br><br>Required for Pure FlashBlade<br><br>Required for HTTPS/HTTPS(TLS). | Backup and Recovery |
| Hybrid Extender | {{site.data.keyword.baas_full_notm}} | 29991 | TCP | Used in a multi-tenant environment for interaction between the Hybrid Extender and {{site.data.keyword.baas_full_notm}}. | Management |
| Hybrid Extender | Isilon | 111 | TCP/UDP | Required for RPC connection. | Backup and Recovery |
| 300, 302, 304, 2049 | Required for NFS. |
| 443 | TCP | Required for HTTPS connection with Isilon. |
| 445 | TCP | Required for SMB. |
| 8080 | TCP/UDP | Required for HTTP connection. |
| Generic NAS | Hybrid Extender | 111 | TCP/UDP | Required for RPC connection. | Backup and Recovery |
| 445 | TCP | Required for SMB. |
| 635, 2049 | TCP/UDP | Required for NFS. |
| Mount Target | Hybrid Extender | 111 | TCP/UDP | NFS restores on Linux physical servers. | Backup and Recovery |
| Host | Hybrid Extender | 50051 | TCP | Linux and Windows physical server backups.<br><br>Use Node IP. If VLANs are configured, use VLAN VIPs. | Backup and Recovery |
| Hybrid Extender | {{site.data.keyword.baas_full_notm}} | 29991 | TCP | For Oracle Adapter Need this port for communication, And NFS ports for mounting the Backup/Recovery Views. | Backup and Recovery. |
| Host | Hybrid Extender | 50051 | TCP | Required for persistent agent workflow for File level recovery and IVM. | Recovery |
{: caption="" caption-side="bottom"}

## Cohesity node detection
{: #cohesity_node_detection}

### Cohesity node detection
{: #cohesity_node_detection}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes | Type of Traffic |
| --- | --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} node or free node | {{site.data.keyword.baas_full_notm}} | 5353 | UDP | Used for node discovery during cluster creation and code addition. | Management |
{: caption="" caption-side="bottom"}

## IBM Cloud Support
{: #cohesity_support}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Outgoing traffic
{: #outgoing_traffic}

Make sure your network team has unblocked and is not monitoring the IBM Cloud Support channel sites or ports for seamless connectivity and effective communication.


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | support-connect.cohesity.com | 443 | TCP | For establishing an SSH reverse tunnel to the Support Channel server.<br><br>Ensure to disable the SSL/TLS certificate inspection on the firewall. |
| support-connect-trust.cohesity.com | 443 | TCP | For establishing a secure tunnel for connecting to the Support Channel server.<br><br>Ensure to disable the SSL/TLS certificate inspection on the firewall. |
| {{site.data.keyword.baas_full_notm}} | 3022, 3023, 3024, 3025, 3080 | TCP | The primary node of the {{site.data.keyword.baas_full_notm}} connects to the Support Channel server and the other nodes use these ports to communicate with the primary node. For more information, see [Manage the Support Channel](../Dashboard/Admin/SupportChannel.htm). |
{: caption="" caption-side="bottom"}

## Internal traffic
{: #internal_traffic}

### Internal traffic between nodes
{: #internal_traffic_between_nodes}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Incoming traffic
{: #incoming_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 2222 | TCP | As the support user, access the Host Shell via port 2222. Enabling Host Shell access requires generating and sharing the Support Token with IBM Cloud Support. For details, see [Using the Secure Shell](../CLI/secureshell.htm). |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 9111 | TCP | Serviceability Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 11112 | TCP | I/O Operations Service (Low Latency) |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 11115 | TCP | Archive Helper Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 11116 | TCP | I/O Operations Helper Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 12222 | TCP | Distributed Key-Value Store Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 12321 | TCP | Distributed Key-Value Store Helper Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 20001 | TCP | NAS Backup Helper Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 20002 | TCP | SMB Backup Helper Service v1 |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 21111 | TCP | Alert Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 22222 | TCP | Cluster Configuration Manager Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 23458 | TCP | Cluster Health Monitor Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 23460 | TCP | Support Automation & Proactive Wellness Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 24567 | TCP | UI and REST API Helper Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 24680 | TCP | Healer and Space Reclamation Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 25566 | TCP | Cluster Metrics Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 25678 | TCP | Hardware and Health Monitor Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 25679 | TCP | Hardware and Health Monitor Helper Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 25680 | TCP | Cluster Metrics Store Service |
| Client | {{site.data.keyword.baas_full_notm}} | 25700 | TCP | Indexing Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 25800 | TCP | Indexing Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 25801 | TCP/UDP | Analytics & Indexing Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 25998 | TCP | Marketplace Apps Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 25999 | TCP | Indexing Helper Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 26000 | TCP | Index Catalog Service v2 |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 26001 | TCP | Index Catalog Service v1 |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 26440 | TCP | Kubernetes Infra Management API Server |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 26443 | TCP | Kubernetes API Server Load Balancer |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 26444 | TCP | Kube Lifecycle Manager Service Peer Communication |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 26445 | TCP | Kube Lifecycle Manager Service Client Communication |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 26447 | TCP | Kubernetes API Server |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 26448 | TCP | Kubelet API |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 26999 | TCP | Reporting Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 27999 | TCP | Reporting Service PostgreSQL |
| {{site.data.keyword.baas_full_notm}} | Third-Party tools (for example Tableau, SSRS etc.) | TCP | Used for connections to Custom Reporting Database. |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 30000 | TCP | Required for applying a patch. |
{: caption="" caption-side="bottom"}

Outgoing Traffic


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 3022, 3023, 3024, 3025, 3080 | TCP | For {{site.data.keyword.baas_full_notm}} to communicate with the Teleport support server.<br><br>Make sure the Support Channel is enabled for the cluster (**Settings** > **Summary**). |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 22222 | TCP | For {{site.data.keyword.baas_full_notm}} Cloud Edition operations. |
{: caption="" caption-side="bottom"}

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 22  | TCP | Support Channel used by IBM Cloud Support for troubleshooting. |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 11113 | TCP | Source-side Deduplication Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 22000 | TCP | Key Management Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 23456 | TCP | Cluster configuration and cluster management traffic |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 23464 | TCP |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 28999 | TCP | Reporting Service |
| {{site.data.keyword.baas_full_notm}} | {{site.data.keyword.baas_full_notm}} | 29990 | TCP | Hybrid Extender Service |
{: caption="" caption-side="bottom"}

## Cluster-to-cluster traffic
{: #cluster-to-cluster_traffic}

If the {{site.data.keyword.baas_full_notm}} uses an interface with the node IP address to reach the external server or client, configure the firewall settings to allow the node IP addresses. If the {{site.data.keyword.baas_full_notm}} uses an interface with only virtual IP addresses, configure the firewall settings to allow the virtual IP addresses. To identify whether the source IP is Static IP or Virtual IP for outgoing traffic, see [Identify Source IP of Outgoing Traffic](#Identify).

#### Bidirectional traffic
{: #bidirectional_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Remote {{site.data.keyword.baas_full_notm}} (target or receiving) | 443 | TCP | Required for replication pairing and ongoing secure communications between replication pairs. |
| {{site.data.keyword.baas_full_notm}} | Remote {{site.data.keyword.baas_full_notm}} (target or receiving) | 11111 | TCP | Required for I/O Service between all nodes and replication pairs when replication is enabled. |
| {{site.data.keyword.baas_full_notm}} | Remote {{site.data.keyword.baas_full_notm}} (target or receiving) | 23335 | TCP | If both the local and remote {{site.data.keyword.baas_full_notm}}s support TLS, port number 23335 is utilized for TLS communication during replication. |
| {{site.data.keyword.baas_full_notm}} | Remote {{site.data.keyword.baas_full_notm}} (target or receiving) | 20000 | TCP | *   Required for replication.<br>    <br>*   Required for interaction handling between {{site.data.keyword.baas_full_notm}} and target backup systems. |
{: caption="" caption-side="bottom"}

#### Outgoing traffic
{: #outgoing_traffic}


| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| {{site.data.keyword.baas_full_notm}} | Remote {{site.data.keyword.baas_full_notm}} (target or receiving) | 24444 | TCP | Required for Health Check when replication is enabled on the {{site.data.keyword.baas_full_notm}} |
{: caption="" caption-side="bottom"}

## Cohesity cli commands used to manage firewall ports
{: #cohesity_cli_commands_used_to_manage_firewall_ports}

For on-premises clusters, you can use the following {{site.data.keyword.baas_full_notm}} CLI commands to manage the cluster firewall configuration. For more information, see [Using the Cohesity CLI](https://docs.cohesity.com/7_2/Web/PDFs/72_PlatformCLI.pdf){: external} and [List of Cohesity CLI Commands](../CLI_Reference/Objects.htm).

The firewall commands use the following terminology: Active {{site.data.keyword.baas_full_notm}} rules are called an _attachment_. An attachment includes a _profile_ and multiple _ipsets_. A profile is a group of TCP or UDP ports. An ipset is subnet with the format of <IP/prefix>. An attachment can be on one or more specific network interfaces or it can apply to all interfaces when no interface is specified.


| CLI Command | Description |
| --- | --- |
| firewall-profile activate | Activates a profile on a list of interfaces with a list of ipsets. |
| firewall-profile add | Adds a collection of service ports with the protocol of each port for the firewall. |
| firewall-profile deactivate | Deactivates a list of active firewall profiles. |
| firewall-profile delete | Removes a list of firewall profiles. |
| firewall-profile list | Lists the groups of firewall profiles. |
| firewall-profile list-active | Lists all the active firewall profiles. Each attachment activates a profile on specific network interfaces and ipsets. |
| firewall-ipset list | Lists the IP sets for a firewall. |
| firewall-profile activate revert-to-default | Reverts all the firewall rules to the default settings. |
{: caption="" caption-side="bottom"}

To list the parameters for each command, type `help` after the `firewall-profile <option>` or `firewall-ipset <option>` for example:

admin@node-1> firewall-profile activate help
