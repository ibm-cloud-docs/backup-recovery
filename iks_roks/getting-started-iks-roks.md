---

copyright:
  years: 2025
lastupdated: "2026-02-16"

keywords: data source connector, iks, roks, cluster

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Register Kubernetes/OpenShift as a data source
{: #data-source-connector-iks-roks}

This information is provided for beta use only and is subject to change. Only the regions us-east, us-south, eu-fr2, and eu-es are available now for this feature.
{: beta}

You must create a data source connector on the same VPC where the Kubernetes/OpenShift cluster is deployed.
{: important}

Located to the right of this page is a summary of key topics that are found on this page.
{: note}

## Quick reference to key sections for new users
{: #iks-roks-tutorial-quick-reference}

A. [Before you begin](#baas-getting-started-iks-roks)

B. Prerequisites for backup and restore:
   - You must have a [{{site.data.keyword.baas_full_notm}} instance that is created or create a new one](#data-source-connector-iks-roks-access-instance)
   - [Create or use existing data source connector](#data-source-connector-iks-roks-create-configure)
   - [Kubernetes/OpenShift cluster should be registered](#data-source-connector-iks-roks-register)

C. Take a backup of the Kubernetes/OpenShift cluster:
   - [Access {{site.data.keyword.baas_full_notm}} instance](#data-source-connector-iks-roks-access-instance)
   - [Create and configure data source connector](#data-source-connector-iks-roks-create-configure)
   - [Register source kubernetes/OpenShift cluster](#data-source-connector-iks-roks-register)
   - [Create or schedule a backup](/docs/backup-recovery?topic=backup-recovery-protecting-namespace-iks-roks)

D. Restore backup to Kubernetes/OpenShift cluster:
   - [Access {{site.data.keyword.baas_full_notm}} instance](#data-source-connector-iks-roks-access-instance)
   - [Create and configure data source connector](#data-source-connector-iks-roks-create-configure)
   - [Register source kubernetes/OpenShift cluster](#data-source-connector-iks-roks-register)
   - [Restore backup](/docs/backup-recovery?topic=backup-recovery-recovering-restoring-backup)

E. [Troubleshooting](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks-troubleshooting)


## Before you begin
{: #baas-getting-started-iks-roks}

You need the following to get started with {{site.data.keyword.baas_full_notm}} with Kubernetes/OpenShift:
- An [{{site.data.keyword.cloud}} Platform account](https://cloud.ibm.com)
- An active instance of [{{site.data.keyword.baas_full_notm}} service](https://cloud.ibm.com/catalog/services/backup-and-recovery?catalog_query=aHR0cHM6Ly9jbG91ZC5pYm0uY29tL2NhdGFsb2cjaGlnaGxpZ2h0cw%3D%3D)

## Accessing your instances
{: #data-source-connector-iks-roks-access-instance}

1. Verify that your [IBM Cloud Platform account](https://cloud.ibm.com/){: external} has access to the required {{site.data.keyword.baas_full_notm}} service.
    1. Go to `Navigation Menu` \> `Backup and Recovery`.
    2. Search for and select your {{site.data.keyword.baas_full_notm}} instance.
    3. On the instance details page, click `Launch dashboard`.
    4. Enter your SSO or account credentials to authenticate.
2. Alternatively, obtain the instance details and credentials to log in.


## Backup requirements for Kubernetes/OpenShift clusters
{: #data-source-connector-iks-roks-backup-requirements}

Your Kubernetes/OpenShift cluster must be registered with the instance to:

- Take backups
- Restore backups

## Backup and Restore compatibility
{: #data-source-connector-iks-roks-brs-comp}

You can back up and restore data on:

- The same Kubernetes/OpenShift cluster
- A different Kubernetes/OpenShift cluster within the same region
- An application running on a Kubernetes/OpenShift cluster
- Kubernetes/OpenShift namespace, PVCs, and more

Clusters must be compatible, especially in terms of storage configuration.
{: note}

## Create or configure a data source connector
{: #data-source-connector-iks-roks-create-configure}

Ensure the node has sufficient CPU and memory to run the Containerized Data Source Connector and Datamover pods. The following table lists their resource requirements.

| Pod Name                                   | CPU Requests | Memory Requests |
|--------------------------------------------|--------------|-----------------|
| Containerized Data Source Connector        | 2            | 5Gi             |
| Datamover                                  | 500m         | 128M            |


### Create a data source connection
{: #data-source-connector-iks-roks-create-data-source-connection}

1. Open the instance dashboard that was discussed in an earlier step on [Accessing your instances](#data-source-connector-iks-roks-access-instance).
2. Go to: `Dashboard` \> `System` \> `Data Source Connections`.
3. Click `New Connection`.
4. Under the Deployment Platform, select one of the following options that matches your Kubernetes/OpenShift cluster type:

   - `ROKS CLASSIC`
   - `ROKS VPC`
   - `IKS CLASSIC`
   - `IKS VPC`

5. Click `Create`. You are presented with a helm install command that you need to run on the cluster you want to protect. Copy the helm install command and save it securely, as you need it in a later step.
6. Click `Done`, then open the menu for the newly created connection. Click `Rename Connection` to rename it for easier future identification.



### Configure a data source connector
{: #data-source-connector-iks-roks-create-configure}







1. In the Cloud Shell terminal, list the available Kubernetes/OpenShift clusters to identify the cluster name:

   ```sh
   ibmcloud ks cluster ls
   ```
   {: codeblock}

   From the output, note the cluster name where the Data Source Connector should be deployed to protect the cluster.

2. Download and configure the `KUBECONFIG` for the selected cluster with admin privileges:

   ```sh
   ibmcloud ks cluster config --cluster <cluster-name> --admin
   ```
   {: codeblock}

4. The Helm chart is hosted in the IBM Container Registry (ICR). Log in to the Helm/OCI registry by using the following command:

   ```sh
   helm registry login icr.io --username iamapikey --password "${API_KEY}"
   ```
   {: codeblock}

   If successful, you see a login confirmation.
5. Update the previously generated helm install command with `--namespace`, `--create-namespace`, `fullnameOverride` and the release name. In the following example, `registrationToken` is masked:

      ```sh
      helm install dsc oci://icr.io/ext/brs/brs-ds-connector-chart --version 7.2.17-release-20260108-ed857f1c --set secrets.registrationToken=xxxxxxx --set fullnameOverride=dsc  --namespace ibm-brs-data-source-connector --create-namespace
      ```
      {: codeblock}
6. Run the updated helm install command in the IBM Cloud Shell.

7. Check that the Helm release is installed:

   ```sh
   helm list -n ibm-brs-data-source-connector
   ```
   {: codeblock}



   

   

   









## How to get the Kubernetes/OpenShift cluster endpoint
{: #how-to-get-iks-roks-endpoint}

1. Go to `Menu` \> `Containers` \> `Clusters`.
2. Filter by location. For example, Washington DC(us-east).
3. The endpoint can be found in the **Overview page** in the Networking section where you can find the information for your private and public endpoints.








## How to register an Kubernetes/OpenShift cluster with {{site.data.keyword.baas_full_notm}} service
{: #data-source-connector-iks-roks-register}

1. Open your {{site.data.keyword.baas_full_notm}} instance.
2. Go to: `Dashboard` \> `Data Protection` \> `Sources` \> `Register Source`.
3. Choose the Kubernetes cluster type.
4. Select the previously created [data source connection](#data-source-connector-iks-roks-create-data-source-connection), or create a new connection.
5. Click **Continue**.
6. Provide these details in the form:

   |  Cluster Endpoint  |  Bearer Token |  Kubernetes Distribution  |
   |----|----|----|
   |[Private or Public](#how-to-get-iks-roks-endpoint)|[See How to create a Bearer Token](#data-source-connector-iks-roks-create-bearer-token-cluster)|Kubernetes/OpenShift|

7. Click **Complete** to finish the registration.
8. You are redirected to the list of data sources where you can see the status of your data source registration.



## How to create a bearer token for a Kubernetes/OpenShift cluster
{: #data-source-connector-iks-roks-create-bearer-token-cluster}

1. Configure kubectl or oc cli by getting kube config `ibmcloud ks cluster config --cluster <cluster> --admin`
2. Create service account and cluster role binding

    ```sh
    kubectl create serviceaccount brs-sa -n default
    ```
    {: pre}

    ```sh
    kubectl create clusterrolebinding brs-cl-role --clusterrole=cluster-admin --serviceaccount=default:brs-sa
    ```
    {: pre}

3. Create secret:
   1. Create a file: secret.yaml.
   2. Add the following contents to the file.

      ```yaml
      apiVersion: v1
      kind: Secret
      metadata:
         name: ibm-token
         namespace: default
         annotations:
         kubernetes.io/service-account.name: ibm
      type: kubernetes.io/service-account-token
      ```
      {: pre}

   3. Run the command:

      ```sh
      kubectl apply -f secret.yaml
      ```
      {: pre}

   4. Get the bearer token and use for the registration

      ```sh
      kubectl describe secrets -n default ibm-token | grep token: | awk '{print $2}' | head -1
      ```
      {: pre}
