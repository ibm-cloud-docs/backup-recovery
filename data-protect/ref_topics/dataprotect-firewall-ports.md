# Firewall Ports for User-Deployed SaaS Connectors

> A typical SaaS Connector connects with the Cohesity DataProtect as a Service and the Data Sources. The following diagram shows the source, destination, ports, and protocols for traffic flow between the user-deployed SaaS Connector and the Data Sources and the user-deployed SaaS Connector and Cohesity DataProtect as a Service.
> 
> More information is provided in the sections that follow the diagram.
> 
> ![](../Resources/Images/data-protect/user-deployed-saas.png)
> 
> Legend
> 
> ![](../Resources/Images/data-protect/dataprotect-firewall-ports-legend.png)

# SaaS Connector Management

Ensure that the following ports are open to allow communication between the Cohesity SaaS Connector(s) and Cohesity Cloud Services:

    
| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| SaaS Connector | helios.cohesity.com | 443 | TCP | Connection used for control path |
| SaaS Connector | \*.awsglobalaccelerator.com | 443 | TCP | Connection used for control path |
| SaaS Connector | \*.s3.<region>.amazonaws.com<br><br>\*.dmaas.helios.cohesity.com | 443 | TCP | Connection used for data path |
| SaaS Connector | helios-data.cohesity.com | 443 | TCP | Used to send telemetry data |
| SaaS Connector | \*.cloudfront.net | 443 | TCP | To download upgrade packages |
| SaaS Connector | production-rigelcheck-us.dmaas.helios.cohesity.com<br><br>helios-production-rigelcheck-1.s3.us-east-2.amazonaws.com | 443 | TCP | Required to perform connectivity checks with the Cohesity Cloud Services. |
| SaaS Connector | 8.8.8.8 or internal DNS | 53  | TCP, UDP | Host resolution. |
| SaaS Connector | time.google.com or internal NTP | 123, 323 | UDP | Incoming NTP requests are detected by port 123.<br><br>Chrony is the default implementation of NTP used by recent versions of CentOS and RHEL. Open port 323 if you want to use the Chronyc tool to monitor the synchronization status of Chrony and make changes if necessary. |
| SaaS Connector | rt.cohesity.com | 22 or 443 | TCP | The Cohesity Support Channel uses Secure Shell (SSH) and listens through port 22 or 443. Port 22 is used by default and can be updated to 443 using the Cohesity CLI. For more information, see [Manage the Support Channel.](https://docs.cohesity.com/7_2/Web/UserGuide/Content/Dashboard/Admin/SupportChannel.htm) |

# Virtual Machines

## VMware

Ensure that the following ports are open to allow communication between the Cohesity SaaS Connector(s) and VMware environment:

    
| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| SaaS Connector | VMware vCenter | 443 | TCP | Required for making VMware API calls for backup and recovery over HTTPS/HTTPS (TLS). |
| SaaS Connector | ESXi Host(s) | 443 | TCP | Required for VMware Tools-based file and folder recoveries. Allow communication to each ESXi host over port 443 for VMware tools-based file and folder recovery, irrespective of whether the vCenter or Standalone ESXi host is registered with the Cohesity DataProtect as a Service. |
| SaaS Connector | ESXi Host(s) | 902 | TCP | Needs to be open on each ESXi host for VADP (vSphere Storage APIs for Data Protection), a vSphere API, that enables backup and restore operations via port 902. |

## Microsoft SCVMM and Hyper-V Servers

Ensure that the following ports are open to allow communication between the Cohesity SaaS Connector(s) and Hyper-V environment:

    
| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| Cohesity Agent running on Standalone Hyper-V and SCVMM server | Guest VM (local host) running on Standalone Hyper-V and SCVMM Server | 5986 | TCP | Required for file and folder recovery operations. |
| SaaS Connector | Standalone Hyper-V and SCVMM Server | 50051 | TCP | Required for backup and recovery operations.. |

## VMC on AWS

Ensure that the following ports are open to allow communication between the Cohesity SaaS Connector(s) and the VMC in the AWS environment:

    
| Source | Destination | Destination Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| SaaS Connector | VMware vCenter | 443 | TCP | Required for making VMware API calls for backup and recovery over HTTPS/HTTPS (TLS).<br><br>Needs to be configured as a Management Gateway firewall rule in the VMC UI. |
| SaaS Connector | ESXi Hosts | 443 | TCP | Required for VMware Tools-based file and folder recoveries. Allow communication to each ESXi host over port 443 for VMware tools-based file and folder recovery, irrespective of whether the vCenter or Standalone ESXi host is registered with the Cohesity cluster.<br><br>Needs to be configured as a Management Gateway firewall rule in the VMC UI. |
| SaaS Connector | Any | Any | TCP | Required for backup and recovery operations.<br><br>Cohesity recommends selecting “Any” in the Service column when configuring this Compute Gateway firewall rule in the VMC UI. |

## Azure VMware Solution

Ensure that the following ports are open to allow communication between the Cohesity SaaS Connector(s) and the AVS environment:

    
| Source | Destination | Destination Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| SaaS Connector | VMware vCenter | 443 | TCP | Required for making VMware API calls for backup and recovery over HTTPS/HTTPS (TLS).<br><br>Refer to the VMware cloud provider's documentation for updating the gateway firewall rules. |
| SaaS Connector | ESXi Hosts | 443 | TCP | Required for VMware Tools-based file and folder recoveries. Allow communication to each ESXi host over port 443 for VMware tools-based file and folder recovery.<br><br>Refer to the VMware cloud provider's documentation for updating the gateway firewall rules. |
| SaaS Connector | Any | Any | TCP | Required for backup and recovery operations.<br><br>Refer to the VMware cloud provider's documentation for updating the gateway firewall rules. |
| SaaS Connector | ESXi Host(s) hosted in AVS environment | 902 | TCP | Each ESXi host must have port 902 open for VADP (vSphere Storage APIs for Data Protection), a vSphere API, allowing backup and restoring operations through port 902.<br><br>Refer to the VMware cloud provider's documentation for updating the gateway firewall rules. |

## Physical Servers

Ensure that the following ports are open to allow communication between the Cohesity SaaS Connector(s) and Physical Servers:

    
| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| SaaS Connector | Physical Windows or Linux Server | 50051 | TCP | Required for Backup and Recovery operations. |
| Local Host (Physical Windows or Linux Server) | Local Host (Physical Windows or Linux Server) | 59999 | TCP | Required for local-to-local communication for self-monitoring and debugging purposes. |

### Agent Upgrade

#### Incoming Traffic

    
| Source | Destination | Destination Port | Protocol | Usage Notes |
| --- | --- | --- | --- | --- |
| Client | Cohesity SaaS Connector | 80,443 | TCP | For Agent upgrade from UI. |

# Databases

## Oracle Servers

Ensure that the following ports are open to allow communication between the Cohesity SaaS Connector(s) and Oracle Server:

    
| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| SaaS Connector | Oracle Server | 50051 | TCP | Required for Backup and Recovery operations. |
| Oracle Server | SaaS Connector | 111, 2049 | TCP | Required for Backup and Recovery operations in Linux servers. |
| Oracle Server | SaaS Connector | 11113, 11117 | TCP | Required for Backup and Recovery operations in Windows servers. |
| Local Host (Physical Windows or Linux Server) | Local Host (Physical Windows or Linux Server) | 59999 | TCP | Required for local-to-local communication for self-monitoring and debugging purposes. |

## Microsoft SQL Servers

Ensure that the following ports are open to allow communication between the Cohesity SaaS Connector(s) and Microsoft SQL Server:

    
| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| SaaS Connector | MS SQL Host | 50051 | TCP | Required for Backup and Recovery operations. |
| MS SQL Host | SaaS Connector | 11113, 11117 | TCP | Required for Backup and Recovery operations. |
| MS SQL Host | Cohesity agent running on the MS SQL Host | 1433 | TCP | Default TCP port for MS SQL instances. Ensure port is open to allow communication between the the MS SQL instance and the Cohesity Agent. |

## Network Attached Storage (NAS)

Ensure that the following ports are open to allow communication between the Cohesity SaaS Connector(s) and NAS Server:

    
| Source | Destination | Port | Protocol | Purpose |
| --- | --- | --- | --- | --- |
| SaaS Connector | NAS Server | 2049 in NFS server & 111 in portmapper | NFS | To establish connection with the NAS source and carry out the Backup and Recovery operations. |
| 445 | SMB | To establish connection with the NAS source and carry out the Backup and Recovery operations. |
| 443 | HTTPS | Required for snapshot-based backups of Netapp, Isilon, Pure Storage, and so on. |
