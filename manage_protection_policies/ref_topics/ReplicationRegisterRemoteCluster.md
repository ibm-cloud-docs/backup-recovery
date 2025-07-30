---

copyright:
  years: 2024
lastupdated: "2024-10-7"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Create a connection to the remote {{site.data.keyword.baas_full_notm}}
{: #create_a_connection_to_the_remote_cohesity_cluster}

9 September 2024

If you are setting up replication on the {{site.data.keyword.baas_full_notm}} that contains the original snapshots to replicate, you must register the connection to the remote {{site.data.keyword.baas_full_notm}} that will store the replicated snapshots. The capturing (local) {{site.data.keyword.baas_full_notm}} captures the snapshots and replicates them to the remote {{site.data.keyword.baas_full_notm}}.

A remote cluster is reachable by the source cluster only after the registration is complete because the firewall ports on the remote cluster are open after the registration.

To register a connection to a remote {{site.data.keyword.baas_full_notm}} for replication and remote access:

1. Log in to the {{site.data.keyword.baas_full_notm}} on which the original snapshots are stored.
    
2. Navigate to **Infrastructure > Remote Clusters**.
    
3. On the **Remote Clusters** page, click **Register Remote Cluster**.
    
    The **Register Remote Cluster** dialog is displayed.
    
4. **VIP or Node IP Addresses**—Specify one of the following:

*   One or more Virtual IP (VIP) addresses of the remote {{site.data.keyword.baas_full_notm}}. Cohesity recommends specifying VIPs because of their ability to failover to other nodes in the {{site.data.keyword.baas_full_notm}} if a node is down or unreachable.
    
*   One or more node IP addresses of the remote {{site.data.keyword.baas_full_notm}}.
    
    If **Distribute Load** is enabled for replication, a single node is specified in this field and the primary {{site.data.keyword.baas_full_notm}} can establish an initial connection to the specified remote node, and the {{site.data.keyword.baas_full_notm}} distributes the network traffic to other nodes in the remote {{site.data.keyword.baas_full_notm}}. Cohesity recommends specifying VIPs. However, if you specify node IP addresses, Cohesity recommends specifying multiple node IP addresses. By specifying multiple node IP addresses, if the network traffic cannot reach a node, the {{site.data.keyword.baas_full_notm}} can try a different node to establish the initial connection.
    
    If network traffic is restricted in your environment, specify the VIP or node IP addresses of the {{site.data.keyword.baas_full_notm}} that are configured to receive network traffic in your environment and turn off the **Distribute Load** toggle.
    
    To add more IP addresses after the remote connection is configured, edit the remote connection, click **Change**, enter the addresses in the **VIP or Node IP Addresses** field, and click **Connect** to verify connectivity to the addresses.
    

6. **Username**—Specify a local Cohesity user.
    
    *   For remote access, Cohesity recommends you specify an admin user. Ensure you disable the MFA for this user.
        
    *   For replication, Cohesity recommends you specify a local user with only the **Replication** role on the remote {{site.data.keyword.baas_full_notm}}. Ensure you disable the MFA for this user. For more information see [Disable MFA for Local Users](../Admin/mfa-local-user.htm#Disable).
        
        *   The local user with only the **Replication** role on the remote {{site.data.keyword.baas_full_notm}} does not have access to the Cohesity UI and can only perform replication.
            
        
        *   The audit log records the user specified while setting up the remote {{site.data.keyword.baas_full_notm}} and not the user logged in to the remote {{site.data.keyword.baas_full_notm}}. All the actions done on a remote {{site.data.keyword.baas_full_notm}} are done on behalf of the remote user configured while setting up the remote {{site.data.keyword.baas_full_notm}}.
            
        
7. **Password**—Specify the password of the user specified.
8. Select **Interface Group**—Select one of the following:
    
    *   Select **Automatically** if you want the {{site.data.keyword.baas_full_notm}} to select the interface group.
        
    *   Select **Manually** to select the associated interface group from the dropdown list.
        
9. Click **Continue** to specify the cluster options.
    
    The cluster connection status is displayed.
    
10. Turn on the **Remote Access** toggle to manage the remote {{site.data.keyword.baas_full_notm}} from the {{site.data.keyword.baas_full_notm}} you are currently using. Remote access is not required for replication. After remote access is enabled, you can select the remote {{site.data.keyword.baas_full_notm}} name from the dropdown in the top left of the {{site.data.keyword.baas_full_notm}}.
    
11. **Replication**—Starting from the 6.8 release, replication is enabled by default. Configure replication settings to the remote {{site.data.keyword.baas_full_notm}}.
12. **Pair Storage Domains for Replication**—Select the storage domains:
    
    1. Select a local storage domain. This storage domain contains the original snapshots that will be replicated.
        
        You cannot pair multiple storage domains on one {{site.data.keyword.baas_full_notm}} to the same storage domain on the other {{site.data.keyword.baas_full_notm}}. You cannot pair a storage domain to two different storage domains on the same remote cluster. You can pair a storage domain to two different storage domains if they are in different remote clusters. For more information, see [Pair Storage Domains for Replication](../../Concepts/ReplicationSetup.htm#Pair).
        
    2. Select a remote storage domain. This storage domain is where the copies of the replicated snapshots are stored.
13. Outbound compression is enabled by default. Cohesity recommends keeping this option enabled so that this {{site.data.keyword.baas_full_notm}} can compress the replication data before sending it to another {{site.data.keyword.baas_full_notm}}.
    
14. **Distribute Load**—Turn on the toggle if traffic can be distributed to all nodes. Turn off if your network environment requires restricting network traffic to the subset of IP addresses listed in the **Node IP Address** field. For example, if your environment has a firewall that is limiting the network traffic to the remote {{site.data.keyword.baas_full_notm}} and if **Distribute Load** is enabled, the {{site.data.keyword.baas_full_notm}} automatically distributes the network traffic to other nodes in the remote {{site.data.keyword.baas_full_notm}}.
15. Turn on the **Encryption** toggle to encrypt the replication data generated by this {{site.data.keyword.baas_full_notm}}. Click **Generate New Key**. The encryption key is displayed. The {{site.data.keyword.baas_full_notm}} that is receiving encrypted data must enable encryption and specify the same encryption key as the {{site.data.keyword.baas_full_notm}} that sent the replication data. Copy the encryption key to the clipboard, so you can paste it while creating the connection from the capturing {{site.data.keyword.baas_full_notm}} back to the remote {{site.data.keyword.baas_full_notm}} as described in [Create Connection Back to the Capturing {{site.data.keyword.baas_full_notm}}](ReplicationRegisterOtherCluster.htm).
16. Turn on the **Throttle** toggle to set a replication data transfer rate limit from this {{site.data.keyword.baas_full_notm}} to another {{site.data.keyword.baas_full_notm}}. Specify the data rate in Megabits per second. For this connection, this value limits the data transfer rate when copying replication data such as snapshots from this {{site.data.keyword.baas_full_notm}} to another {{site.data.keyword.baas_full_notm}}.
    
17. Turn on the **Quiet Times and Throttle Overrides** toggle to specify one or more replication override windows. These windows override the setting in the **Data Transfer Rate Limit** field during the time period specified for the window.
    *   You can optionally change the time zone that applies to all the override windows specified for the connection between the local {{site.data.keyword.baas_full_notm}} and the remote {{site.data.keyword.baas_full_notm}}.
    *   Select the days of the week when the override window must be applied.
    *   Specify a time period in a day when the override window must be applied.
    *   Specify a type for the window:
        
        *   **Quiet Time**—A window when data is not replicated from the local {{site.data.keyword.baas_full_notm}} to a remote cluster. This value overrides the **Data Transfer Rate Limit** value specified.
        *   **Throttle**—A window when this data transfer rate overrides the **Data Transfer Rate Limit** value specified. This window sets the maximum data transfer rate between the local {{site.data.keyword.baas_full_notm}} and the remote {{site.data.keyword.baas_full_notm}} during the specified time period.
    *   If you selected **Throttle**, enter the replication maximum data transfer rate for this window in Megabits per second. For this connection, this value limits the rate that replication data such as snapshots are copied from the local {{site.data.keyword.baas_full_notm}} to the remote {{site.data.keyword.baas_full_notm}} during this window.
        
        If overlapping windows are specified, the latest one that was created takes precedence. For example, if you first create a quiet time window from Monday 9 AM to 10 PM and then a throttle window from 5 PM to 12 PM with a data rate of 100 megabits per second, from 5 PM to 12 PM the data rate is limited to 100 megabits per second.
        
18. Click **Continue** to view the summary.
    
19. Verify the details and click **Register** to register the remote {{site.data.keyword.baas_full_notm}}.
    

If you configured replication, ensure that you set up replication on the remote {{site.data.keyword.baas_full_notm}}. For more information, see [Create Connection Back to the Capturing {{site.data.keyword.baas_full_notm}}](ReplicationRegisterOtherCluster.htm).

## Edit remote {{site.data.keyword.baas_full_notm}} details
{: #edit_remote_cohesity_cluster_details}

To edit a remote {{site.data.keyword.baas_full_notm}} connection:

1. Navigate to **Infrastructure > Remote Clusters**.
    
2. On the **Remote Clusters** page, hover over the remote cluster that you want to edit and click the edit icon.
    
    The **Edit Remote Cluster** dialog is displayed.
    
3. Modify the remote cluster details as needed and click **Save**.
    

## Delete the remote {{site.data.keyword.baas_full_notm}} connection
{: #delete_the_remote_cohesity_cluster_connection}

To delete a remote {{site.data.keyword.baas_full_notm}} connection:

1. Navigate to **Infrastructure > Remote Clusters**.
    
2. On the **Remote Clusters** page, hover over the remote cluster that you want to delete and click the delete icon.
    
3. Click **Delete** to confirm.
    
    After you delete, all storage domain pairings in this connection are deleted and any replication task using this connection fails.
