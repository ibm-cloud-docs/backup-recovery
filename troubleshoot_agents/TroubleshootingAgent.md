---

copyright:
  years: 2024
lastupdated: "2024-10-13"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshoot Backup agent
{: #troubleshoot_cohesity_agent}

This topic provides the troubleshooting information for Backup agents.

## Collect diagnostic information for Backup agents
{: #collect_diagnostic_information_for_cohesity_agents}

For Microsoft Windows servers, you can find the logs for Backup agents at `C:\ProgramData\Cohesity\`.

If the **ProgramData** folder is hidden or not visible, then from the top-menu of the **C** directory, click **View**, and then check the **Hidden items** check box.

The hidden folders and files will be displayed.

## Find the Backup agent version
{: #find_the_cohesity_agent_version}

To find the version of the Backup agent installed on Windows, Linux, and AIX servers, perform the following steps:

1. Navigate to **Data Protection > Sources**
2. Click **Register > Physical Server**.
3. Select a server. If the installed Agent's version is lower than the {{site.data.keyword.baas_full_notm}} version, it will display **Upgrade available**.

## Prerequisites
{: #prerequisites}

Ports 80 and 443 are open from the client to the cluster so that Backup agent upgrade from UI can occur.

## Troubleshooting agent issues
{: #troubleshooting_agent_issues}

The following table provides details about Agent issues that you might encounter:

|     |     |     |     |
| --- | --- | --- | --- |
| Issue | Cause | Workaround |     |
| Registering a Backup agent on a new {{site.data.keyword.baas_full_notm}} fails with the following error:<br><br>`Host is already registered with another cluster: <old cluster-id>, <new cluster-id>` | Agent is already registered to a different {{site.data.keyword.baas_full_notm}} ID. | 1. Navigate to **Data Protection > Sources****\> Register > Physical Server**.<br>2. cluster\_fqdn/protection/sources/new/physical`?forceRegister=true`" to the end of the URL and press the **Enter** key. <br>    You will be redirected to a new registration page.<br>3. Enter the Agent information and ensure that the Force Agent Registration check box is selected. |     |
| Installing or upgrading the Backup agent either manually or automatically fails with the following error: `WinHttpSendRequest() failed : 0x2ee7` | This error indicates a DNS issue in Windows. While installing the Backup agent, the Agent confirms that it can resolve the host name of the {{site.data.keyword.baas_full_notm}}. If this check fails, this error is returned. | Ensure DNS forward and reverse lookups are working on the server or VM. |     |
{: caption="" caption-side="bottom"}

Login to the IBM Cloud Support site to see more Knowledge Base Articles.

## Upgrading the Backup agent on a windows physical server may become stuck
{: #upgrading_the_cohesity_agent_on_a_windows_physical_server_may_become_stuck}

Under specific conditions, upgrading the Backup agent on a Windows physical server may become stuck. To identify if a server is affected by this issue, read the following:

Check if the agent logs (under %programdata%\\Cohesity) contain "Cannot start the agent because the port 50051 is in use," indicating that the agent is in a crash loop.

The upgrade issue can occur if all of the following are true:

*   The Backup agent is running under a specific user account (not the default Local System account).
*   The Backup agent service is the only service in the running state at the time of upgrade using the specified account.
    *   The specified user account has not been logged into through other means (such as RDP) at the time of the upgrade.
*   The "Do not forcefully unload the user registry at user logoff" policy is "Not Configured" (the default) or set to "No."

To get the agent out of this state:

1. Open Windows Task Manager and click the Details tab.
2. Locate the "installer.exe" and "installer.tmp" processes (with the Cohesity icon).
3. Right-click "installer.tmp" and select "End Task". This stops both processes ("installer.exe" and "installer.tmp").

To avoid future occurrences:

Use the Group Policy Editor (gpedit.msc) to enable the "Do not forcefully unload the user registry at user logoff" policy. The policy is located under Computer Configuration > Administrative Templates > System > User Profiles.
