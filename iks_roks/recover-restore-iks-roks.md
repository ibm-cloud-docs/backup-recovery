---

copyright:
  years: 2025
lastupdated: "2026-02-17"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Recover Kubernetes Namespaces
{: #recovering-restoring-backup}

After protecting your Kubernetes namespaces, you can recover them using the {{site.data.keyword.baas_full_notm}} to:
- The original (same) Kubernetes or OpenShift cluster.
- A different Kubernetes or OpenShift cluster registered with {{site.data.keyword.baas_full_notm}}.

## Recovering Namespaces to the original Kubernetes or OpenShift cluster
{: #recovering-same-location}

When recovering namespaces to their original location (the same Kubernetes or OpenShift cluster), only the resources, PVCs, or metadata that are **missing or deleted** are restored from the selected snapshot. {{site.data.keyword.baas_full_notm}} does not overwrite or restore any resources, PVCs, or metadata that already exist in the original location.

For example, suppose a namespace you want to restore contains a deployment resource and a service account. If the service account is missing but the deployment resource still exists, {{site.data.keyword.baas_full_notm}} restores only the service account and skips the deployment resource.

## Restoring namespaces to the same Kubernetes or OpenShift cluster
{: #recovering-restoring-same-location}

1. Log in to the [IBM Cloud Console](https://cloud.ibm.com/){: external}.
2. Go to **Navigation Menu** \> **Backup and Recovery**.
3. Select your {{site.data.keyword.baas_full_notm}} instance.
4. Click **Launch dashboard**.
5. Go to **Dashboard** \> **Data Protection** \> **Recoveries**.
6. Click **Recovery** in the upper-right corner and select **Kubernetes Cluster** \> **Namespace**.
7. Search for the namespace or **Protection Group** containing the snapshots you want to recover.
   - You can enter the namespace name, Protection Group name, or use the wildcard character `*`.
   - Optionally filter results by Source, Protection Group, Storage Domain, or Date.

8. Select one or more namespaces or Protection Groups from the list. By default, the latest snapshot is selected.

   If you select a **Protection Group**, all namespaces backed up in its latest run are selected. You cannot select these namespaces individually. Selecting a namespace or Protection Group locks the **Storage Domain** and **Source** filters.
   {: note}

9. To recover from a specific snapshot (instead of the default latest one):

   **For a Protection Group:**
   - Hover over the Protection Group in the selection list and click the **edit** (pencil) icon.
   - Select the desired snapshot from the **Protection Run** drop-down.
   - (Optional) Click the cloud icon to select a snapshot archived to the cloud.
   - Click **Select Recovery Point**.

   **For a Namespace:**
   - Hover over the namespace and click the **edit** icon.
   - The *Edit recovery point* page appears, showing snapshots from the last month by default.
   - To see other dates, use the **Date** filter and click **Apply**.
   - Select the snapshot you want to allow to recover.
   - (Optional) Click the cloud icon to select a snapshot archived to the cloud.
   - Click **Select Recovery Point**.

   Deleted snapshots may briefly appear as valid recovery points until they are removed from the search index.
   {: note}

10. Click **Next: Recover Options**.
11. Under **Recover To**, select **Original Location** to recover the namespace to the same Kubernetes or OpenShift cluster.
12. Configure the **Recovery Options** as needed:
   - **Rename**: Add a **Prefix** and/or **Suffix** to the names of recovered namespaces.
     - *Example*: Adding prefix `Test_` and suffix `_QA` to namespace `App` creates `Test_App_QA`.
   - **Task Name**: Customize the name of this recovery task.
   - **Namespace Resources**: Choose specific resources to recover. Click the **edit** icon to customize:
      - **Resources**:
         - **Persistent Volume Claim (PVC) Inclusion/Exclusion**: Choose specific PVCs to include or exclude. You can also successfully recover *only* PVCs and their dependent resources (ConfigMaps, PersistentVolumes).
         - **Resource Inclusion/Exclusion**: Include or exclude specific resource classes.
      - **Storage Class**:
         - **Unbind PVCs from original PV mapping**: *Requires the {{site.data.keyword.baas_full_notm}} DataProtect plug-in image to be specified during source registration.* This option clears metadata binding PVCs to their original PersistentVolumes (PVs), allowing them to provision new PVs or bind to available ones. Useful for PVCs with a `Retain` reclaim policy.
         - **Map PVCs to a different storage class**: Click **+ Add** to map a source storage class (left) to a target storage class (right). This ensures recovered PVCs use the correct storage class in the destination.
      - **Recover PVCs Only**: Enable this to recover only PVCs and their dependencies, skipping the namespace metadata.
      - **Include or Exclude Labels**: Filter PVCs by label.
         - Select **Match Any** or **Match All** logical rules.
         - Choose **Include** or **Exclude**, click **+ Add**, and enter the Label Key and Value.

13. Click **Recovery**. You can monitor the progress on the **Recoveries** page.


## Recovering Namespaces to a different Kubernetes or OpenShift cluster
{: #recovering-different-location}

You can recover Kubernetes namespaces to a different Kubernetes or OpenShift cluster registered with the same {{site.data.keyword.baas_full_notm}} instance.

- **New Location**: The entire namespace is restored to a different cluster.
- **Overwrite Behavior**: If the namespace already exists in the destination cluster, {{site.data.keyword.baas_full_notm}} **does not** overwrite existing resources, PVCs, or metadata. It only restores missing items.

To restore to a new location (different cluster):

1. Log in to the [IBM Cloud Console](https://cloud.ibm.com/){: external}.
2. Go to **Navigation Menu** \> **Backup and Recovery**.
3. Select your {{site.data.keyword.baas_full_notm}} instance.
4. Click **Launch dashboard**.
5. Go to **Dashboard** \> **Data Protection** \> **Recoveries**.
6. Click **Recovery** and select **Kubernetes Cluster** \> **Namespace**.
7. Search for the namespace or **Protection Group**. You can filter by Source, Protection Group, Storage Domain, or Date.
8. Select the namespaces or Protection Groups to recover.
   - Selecting a **Protection Group** selects all namespaces from its latest run.
   - Filters lock after your first selection.

9. (Optional) To choose a specific snapshot:
   - **For Protection Groups**: Hover, click **edit**, and select a snapshot from the **Protection Run** menu.
   - **For Namespaces**: Hover, click **edit**, filter by **Date** if needed, and select the desired snapshot.
   - *Cloud snapshots* can be selected by clicking the cloud icon.

10. Click **Next: Recover Options**.
11. Under **Recover To**, select **New Location** to recover the namespace to a different Kubernetes or OpenShift cluster.
12. Under **Registered Source**, select the destination Kubernetes or OpenShift cluster (or click **Register Source** to add a new one).
13. (Optional) Configure **Recovery Options**:
   - **Rename**: Add a prefix or suffix to the new namespace name (e.g., `Test_` + `App` = `Test_App`).
   - **Task Name**: Rename the recovery task for easier tracking.
   - **Namespace Resources**: Click **edit** to granularly select resources:
      - **PVC Inclusion/Exclusion**: Select specific PVCs to recover.
      - **Resource Inclusion/Exclusion**: Include/exclude specific resource types.
      - **Storage Class Mappings**:
         - **Unbind PVCs**: Remove original PV bindings (useful for `Retain` policies).
         - **Mapping**: Click **+ Add** to map source storage classes to destination storage classes.
      - **Recover PVCs Only**: Recover only PVCs and dependencies, ignoring namespace metadata.
      - **Include/Exclude Labels**: Filter PVCs by label key/value pairs.

   - **Alternative Region/Zone Recovery**: {{site.data.keyword.baas_full_notm}} supports recovering to a different region or zone.
      - Specify region and zone mappings.
      - {{site.data.keyword.baas_full_notm}} automatically updates:
         - **Storage Classes**: Updates region/zone parameters and `allowedTopologies`.
         - **Secrets**: Updates storage class secrets (e.g., for `vpc.block.csi.ibm.io`, `openshift-storage`, `portworx`).
         - **Persistent Volumes**: Updates zone/region attributes and node affinity.

14. Click **Recovery**. Monitor the status on the **Recoveries** page.

## Recovery UI Features
{: #recovery-ui-features}

| Feature | Description |
|--------|-------------|
| **Rename** | Allows you to rename the namespace in the destination cluster, creating a restoration with a custom name. |
| **Task Name** | Assigns a custom name to the recovery task for easier identification and tracking in the workflow. |
| **Cluster Resources** | Specifies which cluster-scoped resources to include in the migration. These are restored at the cluster level in the destination. |
| **Namespace Resources** | Limits recovery to specific resources existing in the source namespace. You can also filter PVCs and related resources for inclusion/exclusion. |
| **Skip cluster compatibility check** | Bypasses version validation between source and destination clusters. Useful when checking restoring between different cluster types (e.g., ROKS to IKS). |
| **Region Mapping** | Updates region parameters in custom StorageClasses during restore, creating them with new region values in the destination. |
| **Zone Mapping** | Updates zone parameters in custom StorageClasses during restore, applying new zone values in the destination. |
| **Include/Exclude Labels** | Filters PersistentVolumeClaims (PVCs) by label, allowing you to include or exclude specific PVCs from the migration. |
