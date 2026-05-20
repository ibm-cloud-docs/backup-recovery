---

copyright:
  years: 2025, 2026
lastupdated: "2026-05-20"

keywords: cross-account backup, vpeg, virtual private endpoint gateway, iks, roks, kubernetes

subcollection: backup-recovery

content-type: tutorial

completion-time: 1h

draft: true

---

{{site.data.keyword.attribute-definition-list}}

# Cross-Account Backup and Recovery with Virtual Private Endpoint Gateway
{: #cross-account-backup-vpeg}

This guide explains how to configure cross-account backup and recovery between IBM Cloud accounts using Virtual Private Endpoint Gateway (VPEG) for secure private connectivity.

## Overview
{: #cross-account-backup-vpeg-overview}

Cross-account backup enables you to protect workloads running in one IBM Cloud account by backing them up to a Backup and Recovery Service (BRS) instance located in a different account. This configuration uses VPEG to establish secure, private network connectivity between accounts.

## Architecture
{: #cross-account-backup-vpeg-architecture}

In this setup:
- **Source Account**: Contains the IKS/ROKS cluster with workloads to be protected
- **Target Account**: Contains the BRS instance that stores the backups
- **Connectivity**: VPEG provides private network connectivity between the VPC in the source account and the BRS instance in the target account

## Prerequisites
{: #cross-account-backup-vpeg-prerequisites}

Before you begin, ensure you have:

- Administrative access to both IBM Cloud accounts
- A BRS instance provisioned in the target account
- An IKS or ROKS cluster deployed in a VPC in the source account
- IBM Cloud CLI installed with the VPC infrastructure plugin
- Appropriate IAM permissions in both accounts

## Configuration Steps
{: #cross-account-backup-vpeg-configuration}

### Step 1: Create Service-to-Service Authorization
{: #cross-account-backup-vpeg-s2s-auth}

Create an authorization policy in the target account (where the BRS instance resides) to allow the VPC Infrastructure Service from the source account to access the BRS instance.

1. Log in to the **target account** (BRS instance account)
2. Navigate to **IAM** > **Authorizations**
3. Click **Create**
4. Configure the authorization:
   - **Source account**: Enter the source account ID (where the IKS/ROKS cluster is located)
   - **Source service**: Select **VPC Infrastructure Services**
   - **Resources**: Select **Virtual Private Endpoint for VPC**
   - **Target service**: Select **Backup and Recovery**
   - **Resources**: Select the specific BRS instance or leave as "All resources"
   - **Service access**: Select **Viewer** role
5. Click **Authorize**

### Step 2: Create Virtual Private Endpoint Gateway with Reserved IPs
{: #cross-account-backup-vpeg-create-vpeg}

**Recommended Approach:** Create the VPEG with reserved IPs and security group in a single command for the most efficient setup.

Since the BRS instance is in a different account, you must create the VPEG using the IBM Cloud CLI rather than the UI.

1. Log in to the **source account** (IKS/ROKS cluster account):
   ```bash
   ibmcloud login
   ```

2. Target the appropriate region:
   ```bash
   ibmcloud target -r <region> -g <resource-group>
   ```

3. Identify the Kubernetes VPEG security group:
   ```bash
   ibmcloud is security-groups --vpc <vpc-id-or-name>
   ```
   
   Look for the security group with name pattern: `kube-vpegw-<vpc-id>`

4. List the subnets in your VPC to get subnet IDs:
   ```bash
   ibmcloud is subnets --vpc <vpc-id-or-name> --output JSON
   ```

5. Create the VPEG with all configurations:
   ```bash
   ibmcloud is endpoint-gateway-create \
     --vpc <vpc-id> \
     --target <brs-instance-crn> \
     --name <vpeg-name> \
     --sg <security-group-id> \
     --new-reserved-ip '{"subnet": {"id": "<subnet-id-1>"}, "name": "<reserved-ip-name-1>", "auto_delete": true}' \
     --new-reserved-ip '{"subnet": {"id": "<subnet-id-2>"}, "name": "<reserved-ip-name-2>", "auto_delete": true}' \
     --new-reserved-ip '{"subnet": {"id": "<subnet-id-3>"}, "name": "<reserved-ip-name-3>", "auto_delete": true}'
   ```

   **Example:**
   ```bash
   ibmcloud is endpoint-gateway-create \
     --vpc r006-5683bb27-26f7-47e2-93d2-29044622713e \
     --target crn:v1:bluemix:public:backup-recovery:us-south:a/<account-id>:<instance-id>:: \
     --name brs-cross-account-vpeg \
     --sg kube-vpegw-r006-5683bb27-26f7-47e2-93d2-29044622713e \
     --new-reserved-ip '{"subnet": {"id": "0717-a529e1b9-d4cf-48a0-a1bb-e9a1d32cb6e7"}, "name": "cross-account-rip-1", "auto_delete": true}' \
     --new-reserved-ip '{"subnet": {"id": "0727-b639f2c0-e5d8-59b1-b2cc-f0b2e43dc7f8"}, "name": "cross-account-rip-2", "auto_delete": true}' \
     --new-reserved-ip '{"subnet": {"id": "0737-c749g3d1-f6e9-60c2-c3dd-g1c3f54ed8g9"}, "name": "cross-account-rip-3", "auto_delete": true}'
   ```

   This single command:
   - Creates the VPEG
   - Attaches the Kubernetes VPEG security group
   - Binds reserved IPs to all three subnets
   - Sets auto-delete for automatic cleanup

6. Note the VPEG ID from the command output for use in subsequent steps.

### Alternative: Step-by-Step Approach
{: #cross-account-backup-vpeg-alternative-approach}

If you prefer to configure components separately or need to add reserved IPs to an existing VPEG:

1. Create a basic VPEG with security group:
   ```bash
   ibmcloud is endpoint-gateway-create \
     --vpc <vpc-id> \
     --target <brs-instance-crn> \
     --name <vpeg-name> \
     --sg <security-group-id>
   ```

   Note the VPEG ID from the output.

2. List the subnets in your VPC:
   ```bash
   ibmcloud is subnets --vpc <vpc-id-or-name>
   ```

   Or to see all subnets with JSON output for easier parsing:
   ```bash
   ibmcloud is subnets --vpc <vpc-id-or-name> --output JSON
   ```

3a. Create and bind a reserved IP in each subnet directly to the VPEG:
   ```bash
   ibmcloud is subnet-reserved-ip-create <subnet-id> \
     --name <reserved-ip-name> \
     --target <vpeg-id> \
     --auto-delete true
   ```

   **Example:**
   ```bash
   ibmcloud is subnet-reserved-ip-create 0717-a529e1b9-d4cf-48a0-a1bb-e9a1d32cb6e7 \
     --name cross-account-rip-1 \
     --target r006-abc123-vpeg \
     --auto-delete true
   ```

3b. Alternatively, create the reserved IP first, then bind it:
   ```bash
   # Create reserved IP
   ibmcloud is subnet-reserved-ip-create <subnet-id> \
     --name <reserved-ip-name>
   
   # Bind to VPEG
   ibmcloud is endpoint-gateway-reserved-ip-bind <vpeg-id> \
     --rip <reserved-ip-name> \
     --subnet <subnet-id>
   ```

4. Repeat the chosen approach (step 3a or step 3b) for all subnets in the VPC (typically 3 subnets for high availability across zones).

### Step 3: Verify VPEG Configuration
{: #cross-account-backup-vpeg-verify}

Verify that the VPEG is properly configured with reserved IPs and security group.

```bash
ibmcloud is endpoint-gateway <vpeg-id>
```

Check that the output shows:
- Status: `stable`
- Reserved IPs bound to each subnet
- Security group attached (should show `kube-vpegw-<vpc-id>`)

You can also use JSON output for detailed information:
```bash
ibmcloud is endpoint-gateway <vpeg-id> --output JSON
```

### Step 4: Create Data Source Connection and Register Kubernetes Source
{: #cross-account-backup-vpeg-register-source}

Now that network connectivity is established, create a data source connection and register your Kubernetes cluster.

Follow the standard procedures to create a data source connection and register your Kubernetes cluster:

1. [Create and configure data source connector](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#data-source-connector-iks-roks-create-configure)
2. [Register Kubernetes cluster as a source](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks)

**Important for cross-account setup:** When registering the Kubernetes source, use the **Private** or **VPE** endpoint URL of your cluster (not the public endpoint) to ensure traffic flows through the VPEG connection established in the previous steps.

### Step 5: Create Protection Group and Run Backups
{: #cross-account-backup-vpeg-create-protection}

Follow the standard procedures to create protection groups and run backups:

1. [Create or schedule a backup](./protecting-namespace-cluster-iks-roks.md)
2. [Run backup on demand](./run-now-iks-roks.md)

## Troubleshooting
{: #cross-account-backup-vpeg-troubleshooting}

### VPEG Connection Issues
{: #cross-account-backup-vpeg-vpeg-issues}

- Verify the service-to-service authorization is correctly configured
- Ensure reserved IPs are bound to all subnets
- Confirm the correct security group is attached to the VPEG
- Check that the BRS instance CRN is correct

### Agent Installation Failures
{: #cross-account-backup-vpeg-agent-issues}

- Verify network connectivity from the cluster to the BRS instance
- Check that the Helm chart version is compatible with your cluster
- Review agent pod logs for specific error messages
