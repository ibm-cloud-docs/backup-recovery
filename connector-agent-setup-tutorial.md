---

copyright:
  years: 2026
lastupdated: "2026-06-26"

keywords: connector agent, backup agent, direct connection, agent registration, backup and recovery

subcollection: backup-recovery

content-type: tutorial
services: backup-recovery
account-plan: paid
completion-time: 45m

---

{{site.data.keyword.attribute-definition-list}}

# Set up and register a Connector Agent
{: #connector-agent-setup-tutorial}
{: toc-content-type="tutorial"}
{: toc-services="backup-recovery"}
{: toc-completion-time="45m"}

In this tutorial, you learn how to set up and register a Connector Agent on {{site.data.keyword.baas_full}}. The Connector Agent enables direct connectivity between your data sources and the backup cluster, providing an alternative to the Data Source Connector for certain workloads.
{: shortdesc}

The Connector Agent is a lightweight software component that runs on your source host and establishes a direct connection to the {{site.data.keyword.baas_full_notm}} cluster. This architecture is useful for Oracle RAC environments and other scenarios where direct agent-to-cluster communication is preferred.

## Objectives
{: #connector-agent-objectives}

- Deploy and run the Connector Agent on Linux or Windows hosts
- Obtain a registration token from the {{site.data.keyword.baas_full_notm}} instance dashboard
- Claim and register the agent with your {{site.data.keyword.baas_full_notm}} instance
- Verify successful agent registration
- Register a data source by using the Connector Agent

## Before you begin
{: #connector-agent-prereqs}

Before you start this tutorial, ensure that you have the following:

- An active {{site.data.keyword.baas_full_notm}} instance that is active
- Access to the Backup and Recovery service  instance dashboard
- A Windows or Linux host where you want to install the connector agent
- The backup agent is already installed on your target host
- Network connectivity from the agent host to the cluster on the following ports:
    - Port 443 (HTTPS)
    - Port 29991 (broker)
    - Port 11117 (bridge)
- The Connector Agent binary is downloaded to your target host:
    - Linux installer
    - Windows installer
- Administrator or root access on the host where you install the agent
- Basic knowledge of REST APIs and command-line operations
- Recommended host sizing for a single Connector:
   - 1vCPU and 512 MB of RAM should be allocated when transfer throughput is expected to average ~100 MB/s, with occasional peaks up to ~150 MB/s (megabytes per second).

## Obtain the connector agent
{: #connector-agent-obtain}
{: step}

You can download the connector agent installer from the Backup and Recovery service  instance dashboard.

1. Log in to the IBM Cloud console.
2. Navigate to your Backup Recovery Service instance.
3. Click **Launch dashboard**.
4. In the dashboard, navigate to **Sources**.
5. Click **Register source**.
6. Select your source type:
   - For Microsoft SQL server: Select **Microsoft SQL**
   - For Oracle server: Select **Oracle**
   - For physical Windows servers: Select **Physical**
7. Click **Start registration**.
8. Click **Download connector agent**.
9. Select your operating system (Windows or Linux) from the dropdown menu.
10. Click **Download**.

If you download the connector agent by using this method, you need to transfer the installer to your target host before proceeding with the installation.
{: note}

## Deploy the Connector Agent
{: #connector-agent-deploy}
{: step}

Deploy the Connector Agent on your source host by downloading the installer directly onto your source or copy the installer binary onto your source.

The deployment process differs slightly between Linux and Windows systems.
{: note}

### Deploy on Linux
{: #connector-agent-deploy-linux}

1. Make the installer executable:

   ```curl
   chmod +x connector_agent_7.3.12_linux_x64_installer
   ```
   {: codeblock}

2. Install Connector Agent as root:

   ```curl
   sudo ./connector_agent_7.3.12_linux_x64_installer -- --install

   Verifying archive integrity... All good.
   Uncompressing Connector Agent  100%
   Installer log: /tmp/connector_agent_installer_L1JZZ5.log
   ------------------------------------------------------------------------------
   ==> Starting Connector Agent installation...
   ------------------------------------------------------------------------------
   ==> This is x86_64 system
   kernel_version is: 4.18.0
   ==> Service user: connector_agent
   ==> Service group: connector_agent
   ==> Creating system group connector_agent...
   + groupadd --system connector_agent
   ==> Creating system user connector_agent...
   + useradd --system --no-create-home --shell /usr/sbin/nologin --gid connector_agent connector_agent

   Do you want to install Connector Agent? [y/N] y
   ==> Installation directory: /opt/connector_agent
   ==> Config directory: /etc/connector_agent
   ==> Log directory: /var/log/connector_agent
   ==> Creating directories...
   + mkdir -p /opt/connector_agent/bin
   + mkdir -p /opt/connector_agent/lib
   + mkdir -p /var/log/connector_agent
   + mkdir -p /etc/connector_agent
   + install -o root -g root -m 0600 /dev/null /opt/connector_agent/.connector_agent
   + install -o root -g root -m 0600 /dev/null /etc/connector_agent/.connector_agent
   + install -o root -g root -m 0600 /dev/null /var/log/connector_agent/.connector_agent
   ==> Copying connector_agent_exec to /opt/connector_agent/bin...
   + cp /tmp/selfgz3528422832/connector_agent_exec /opt/connector_agent/bin/
   + chmod 755 /opt/connector_agent/bin/connector_agent_exec
   ==> Copying bundled libraries to /opt/connector_agent/lib...
   + cp -r /tmp/selfgz3528422832/dlls_for_linking/. /opt/connector_agent/lib/
   + chmod -R 755 /opt/connector_agent/lib
   ==> Running patchelf on connector_agent_exec...
   Setting r-path for connector_agent_exec to /opt/connector_agent/lib
   + /opt/connector_agent/lib/patchelf --remove-rpath /opt/connector_agent/bin/connector_agent_exec
   + /opt/connector_agent/lib/patchelf --force-rpath --set-rpath /opt/connector_agent/lib /opt/connector_agent/bin/connector_agent_exec
   Setting r-path for all libs in /opt/connector_agent/lib
   + /opt/connector_agent/lib/patchelf --set-rpath /opt/connector_agent/lib /opt/connector_agent/lib/libc.so.6
   + /opt/connector_agent/lib/patchelf --set-rpath /opt/connector_agent/lib /opt/connector_agent/lib/libcrypto.so.3
   + /opt/connector_agent/lib/patchelf --set-rpath /opt/connector_agent/lib /opt/connector_agent/lib/libnss_compat.so.2
   + /opt/connector_agent/lib/patchelf --set-rpath /opt/connector_agent/lib /opt/connector_agent/lib/libnss_db.so.2
   + /opt/connector_agent/lib/patchelf --set-rpath /opt/connector_agent/lib /opt/connector_agent/lib/libnss_dns.so.2
   + /opt/connector_agent/lib/patchelf --set-rpath /opt/connector_agent/lib /opt/connector_agent/lib/libnss_files.so.2
   + /opt/connector_agent/lib/patchelf --set-rpath /opt/connector_agent/lib /opt/connector_agent/lib/libnss_hesiod.so.2
   + /opt/connector_agent/lib/patchelf --set-rpath /opt/connector_agent/lib /opt/connector_agent/lib/libresolv.so.2
   + /opt/connector_agent/lib/patchelf --set-rpath /opt/connector_agent/lib /opt/connector_agent/lib/libssl.so.3
   Setting interpreter-path for connector_agent_exec to /opt/connector_agent/lib/ld-linux-x86-64.so.2
   + /opt/connector_agent/lib/patchelf --set-interpreter /opt/connector_agent/lib/ld-linux-x86-64.so.2 /opt/connector_agent/bin/connector_agent_exec
   ==> Generating set_env.sh...
   + cp /tmp/selfgz3528422832/set_env_template.sh /etc/connector_agent/set_env.sh
   + chmod 640 /etc/connector_agent/set_env.sh
   ==> Installing management script...
   + cp /tmp/selfgz3528422832/connector_agent.sh /opt/connector_agent/bin/
   + chmod 755 /opt/connector_agent/bin/connector_agent.sh
   ==> Applying ownership and permissions for service user connector_agent...
   + chown -R root:root /opt/connector_agent
   + chmod 755 /opt/connector_agent /opt/connector_agent/bin
   + chgrp -R connector_agent /opt/connector_agent/lib
   + chmod 750 /opt/connector_agent/lib
   + chown -R root:connector_agent /etc/connector_agent
   + chmod 770 /etc/connector_agent
   + chown -R connector_agent:connector_agent /var/log/connector_agent
   + chmod 750 /var/log/connector_agent
   ==> Starting connector agent...
   Starting connector_agent_exec...
   connector_agent_exec started (PID: 35831).
   ------------------------------------------------------------------------------

   Connector Agent installed at /opt/connector_agent

   Management:
   sudo /opt/connector_agent/bin/connector_agent.sh {start|stop|status|restart|version}
   ```
   {: codeblock}

### Deploy on Windows
{: #connector-agent-deploy-windows}

1. Open a powerShell terminal.

   ```powershell
   C:\>powershell
   Windows PowerShell
   Copyright (C) Microsoft Corporation. All rights reserved.
   ```
   {: codeblock}

2. Run the connector agent installer with the "-install" option.

   ```powershell
   PS C:\> .\connector_agent_7.3.12_win_x64_installer.exe -install
   Installer log: C:\Users\ADMINI~1.ARE\AppData\Local\Temp\2\connector_agent_installer_1163485291.log

   Do you want to install Connector Agent? [y/N] y
   Extracting to C:\Program Files\ConnectorAgent ...
   Service 'ConnectorAgent' started.
   Install complete. Service 'ConnectorAgent' is registered and started.
   Install dir: C:\Program Files\ConnectorAgent
   Config dir:  C:\ProgramData\ConnectorAgent
   Log dir:     C:\ProgramData\ConnectorAgent\logs
   ```
   {: codeblock}

## Fetch the registration token
{: #connector-agent-registration-token}
{: step}

Obtain a time-limited registration token (JWT) that is used to claim the agent.

**Option 1**: Fetch the registration token that uses API

1. Call the connector agents config API. First, obtain an IAM bearer token by using your API key.

   ```curl
   curl -X POST 'https://iam.cloud.ibm.com/identity/token' \
   -H 'Content-Type: application/x-www-form-urlencoded' \
   -d 'grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=<apikey-with-access-to-your-brs-instance>'
   ```
   {: codeblock}

   Replace `<apikey-with-access-to-your-brs-instance>` with your actual API key. The response includes an `access_token` field. Copy this token value to use in the next step.

2. Copy the `registrationToken` value from the response JSON. This token is time-limited and must be used promptly. Replace `<access-token>` with the token that is obtained in the previous step and `<tenant-id>` with your tenant ID.

   ```curl
   curl -k -X GET "https://<your-instance-url>/v2/connector-agents/configs" \
   -H "Content-Type: application/json" \
   -H "Accept: application/json" \
   -H "X-IBM-Tenant-ID: <tenant-id>" \
   -H "Authorization: Bearer <iam-token>" \
   -v
   ```
   {: codeblock}

   Replace the following values:

   `<your-instance-url>`: Your BRS instance URL from the IBM Cloud console

   `<tenant-id>`: Your tenant ID from the IBM Cloud console

   `<iam-token>`: The access token from the above

**Option 2**: Fetch the registration token by using the Backup and Recovery service instance dashboard


1. In the Backup and Recovery service  instance dashboard, click **Sources** in the left navigation menu.
2. Click **Register source** in the upper right corner.
3. Select your source type:
   - For Microsoft SQL server: Select **Microsoft SQL**
   - For Oracle server: Select **Oracle**
   - For physical Windows servers: Select **Physical**
4. Click **Start registration**.
5. The registration workflow displays the **Registration token** section.
6. The registration token displays in a text field. This token is a unique identifier that links your connector agent to your Backup and Recovery service  instance.
7. Click the **Copy** icon next to the token to copy it to your clipboard.

Keep the {{site.data.keyword.baas_full_notm}} instance dashboard open with the registration workflow displayed. You will return to this screen after running the registration command.
{: tip}

## Claim the agent
{: #connector-agent-claim}
{: step}

With the agent running and the registration token that is obtained, claim the agent by calling its local registration endpoint. Run the following steps on your source host:

1. Call the agent's registration endpoint. The agent listens on port 8080 by default:

   ```curl
   curl -s -X POST \
     -H "Content-Type: application/json" \
     -d '{
       "registrationToken": "<paste-registration-token>",
       "connectionName": "<unique-connection-name>",
       "joinExistingConnection": false
     }' \
     "http://localhost:8080/v1/connector-agents/registration"
   ```
   {: codeblock}

2. Verify the response:
   - A successful claim returns HTTP 204 No Content
   - If the agent is already claimed, the API returns 204 with X-Registration-Status: already-registered

    ```http
    HTTP/1.1 204 No Content
    X-Registration-Status: already-registered
    ```
    {: codeblock}

Replace `<paste-registration-token>` with the registration token from the previous step and `<unique-connection-name>` with a unique name for this connection within your tenant.

### Key parameters
{: #connector-agent-claim-parameters}

`connectionName`
:   Must be unique within the tenant. This name identifies the connection in the {{site.data.keyword.baas_full_notm}} UI.

`joinExistingConnection`
:   Set to `true` if this agent is joining an already-claimed connection, such as in Oracle RAC clustered agent scenarios. For stand-alone agents, use `false`.

## Verify agent registration
{: #connector-agent-verify}
{: step}

Confirm that the agent appears in the cluster's list of registered Connector Agents.

### Option 1: Verify agent registration that uses API
{: #verify-registration-api}

1. List all Connector Agents on the cluster:

    Use the following curl command to retrieve a list of connector agents for your tenant:

    ```curl
    curl -k -X GET "https://<your-instance-url>/v2/connector-agents?tenantId=<tenant-id>" \
    -H "Authorization: Bearer <iam-token>" \
    -H "X-IBM-Tenant-ID: <tenant-id>" \
    -H "Content-Type: application/json" \
    -H "Accept: application/json"
    ```
    {: codeblock}

    Where:

    `<your-instance-url>`: The URL of your Backup Recovery Service instance. You can find this value on your BRS instance's page in the IBM Cloud console.

    `<tenant-id>`: Your tenant ID. You can find this value on your BRS instance's page in the IBM Cloud console. This value is used in both the query parameter and the `X-IBM-Tenant-ID` header.

    `<iam-token>`: Your IBM Cloud IAM access token.

    Before you can call the Backup Recovery Service API, you need to generate an IAM access token. Use the following curl command to generate a token:

    ```curl
    curl -X POST 'https://iam.cloud.ibm.com/identity/token' \
    -H 'Content-Type: application/x-www-form-urlencoded' \
    -d 'grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=<apikey>'
    ```
    {: codeblock}

    Where:

    `<apikey>`: Your IBM Cloud API key with access to your Backup Recovery Service instance.

    The response includes an `access_token` field that contains your IAM token. Use this token value as the `<iam-token>` in the connector agents request.

2. Review the JSON array response to confirm your agent and connection name appear in the list.

3. Optionally, filter the results:
   - By connection ID: append `&connectionIds=<id>` to the URL
   - By connection name: append `&connectionNames=<name>` to the URL

### Option 2: Verify agent registration by using the Backup and Recovery service instance dashboard
{: #verify-registration-brs}

1. In the dashboard, navigate to **Sources** in the left navigation menu.
2. If you just completed the registration, you might need to refresh the page to see the newly registered source.
3. Use the search bar at the top of the Sources page to locate your data source by:
   - Hostname (for example, `WIN-SERVER-01`)
   - IP address (for example, `10.241.0.4`)
   - Partial name match
4. Alternatively, use the filter dropdown to narrow the list by source type.

## Register a data source
{: #connector-agent-register-source}
{: step}

After the agent is successfully registered, you can use it to register data sources for backup and recovery operations.

### Option 1: Register a data source that uses API
{: #register-data-source-api}

1. Register the Physical box (Linux or Windows) using the provided curl command:

   ```curl
   curl -X POST \
      --url 'https://<your-instance-url>/v2/data-protect/sources/registrations' \
      -H 'Authorization: Bearer <iam-token>'\
      -H 'X-IBM-Tenant-ID: <tenant_id>'\
      -H 'Accept: application/json'\
      -H 'Content-type: application/json' \
      --data-raw '{
      "environment": "kPhysical",
      "dataSourceConnectionId": "<connector_agent_id>",
      "physicalParams": {
         "endpoint": "<IP address of host>",
         "hostType": "<kLinux, or kWindows>",
         "physicalType": "kHost",
         "name": "<name_for_this_source>"
      }
      }'
   ```
   {: codeblock}

2. Register the SQL or Oracle application by using the provided curl command:

   ```curl
   curl -X POST \
      --url 'https://<cluster_endpoint>/v2/data-protect/sources/registrations' \
      -H 'Authorization: Bearer <access_token>'\
      -H 'X-IBM-Tenant-ID: <tenant_id>'\
      -H 'Accept: application/json'\
      -H 'Content-type: application/json' \
      --data-raw '{
      "environment": "kPhysical",
      "dataSourceConnectionId": "<connector_agent_id>",
      "physicalParams": {
         "endpoint": "<IP address of host>",
         "hostType": "<kWindows or kLinux>",
         "physicalType": "kHost",
         "applications": ["<kSQL or kOracle>"],
         "name": "<name_for_this_source>"
      }
      }'
   ```
   {: codeblock}

### Option 2: Register a data source by using Backup and Recovery service instance dashboard
{: #register-data-source-brs}

1. Log in to the {{site.data.keyword.baas_full_notm}} UI.

2. Navigate to the **Data Protection** tab.

3. Select the **Sources** tab.

4. Click **Register Source**.

5. Select the type of source that you want to protect (for example, Oracle, MS SQL, or file system).

6. On the source registration page, select the **Direct** radio button to use a Connector Agent.

7. From the dropdown menu, select the connection name of the Connector Agent you registered.

8. Complete the remaining source registration fields according to your source type.

9. Click **Register** to complete the source registration.

The data source is now registered and ready for backup operations by using the Connector Agent.

10. Verify the following information:
    - **Source name**: Confirms that the correct host is registered
    - **IP address**: Matches your target host's IP address
    - **Status**: Should show as "Active" or "Connected"
    - **Source type**: Displays the correct type (Physical, SQL, and more.)
11. For Microsoft SQL Server sources, verify that the source appears in two locations:
    - Under **SQL** category (when filtered by Microsoft SQL)
    - Under **Physical** category (when filtered by Physical)

This dual listing occurs because SQL Server is both a database application and runs on a physical server. You can manage backups for the SQL databases separately from the physical server backups.
{: tip}

## Next steps
{: #connector-agent-next-steps}

Now that you have successfully set up and registered a Connector Agent, you can:

- [Create protection groups](/docs/backup-recovery?topic=backup-recovery-create-protection-groups) to organize your data sources
- [Configure backup policies](/docs/backup-recovery?topic=backup-recovery-create-policy) to define backup schedules and retention
- [Perform backup operations](/docs/backup-recovery?topic=backup-recovery-backup-linux) on your registered sources
- [Monitor backup jobs](/docs/backup-recovery?topic=backup-recovery-view-jobs) through the {{site.data.keyword.baas_full_notm}} UI

## Troubleshooting
{: #connector-agent-troubleshooting}

### Config file errors on startup
{: #connector-agent-troubleshoot-config}

If you see config file errors when starting the agent, this is expected behavior for an unclaimed agent. The config file is created automatically after a successful claim operation.

### Permission denied errors
{: #connector-agent-troubleshoot-permissions}

If you encounter permission denied errors when running the binary or creating the config file:

- On Linux: Make certain the binary has run permissions by using `chmod +x`
- On Windows: Run the command prompt or PowerShell as administrator

### Cannot reach the cluster
{: #connector-agent-troubleshoot-connectivity}

If the agent cannot connect to the cluster, verify network connectivity:

1. Test HTTPS connectivity:

   ```http
   nping <ip-or-fqdn> -p 443
   ```
   {: codeblock}

2. Test broker connectivity:

   ```http
   nping <ip-or-fqdn> -p 29991
   ```
   {: codeblock}

3. Try an unauthenticated API call:

   ```curl
   curl -sk "https://<ip-or-fqdn>/v2/data-protect/policies"
   ```
   {: codeblock}

4. Check broker (gRPC) connectivity:

   ```curl
   openssl s_client -connect <ip-or-fqdn>:29991
   ```
   {: codeblock}

   You should receive a valid TLS response.

### Agent not appearing after claim
{: #connector-agent-troubleshoot-not-appearing}

If the agent does not appear in the cluster after claiming:

- Contact IBM Support
- Confirm that you are using the correct registration token for the agent you are registering
- Check that the tenant ID in the API calls matches the tenant where the agent should be registered
- Review the agent logs for any error messages
