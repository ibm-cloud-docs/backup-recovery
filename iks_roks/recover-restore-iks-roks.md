---

copyright:
  years: 2025
lastupdated: "2026-02-13"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Recover Kubernetes Namespaces
{: #recovering-restoring-backup}

After you protect your Kubernetes namespaces, you can recover them from IBM Backup and Recovery Service to either:
- The original (same) location
- A different Kubernetes cluster registered with BRS

## Recovering Namespaces to the same Location
{: #recovering-same-location}

When you are recovering the namespaces to the same or original location, then only the resources, PVCs, or the metadata of the Kubernetes namespaces that are missing or deleted from the original location are restored from the namespace snapshot you selected for recovery. BRS does not overwrite or restore the resources, PVCs, or the metadata of the Kubernetes namespace that exists in the original location.

For example, a Kubernetes namespace you want to restore to the same location contains a deployment resource and a service account. When BRS initiates the restore to the original location and if the service account is missing from the original location, then BRS restores only the service account. The restore of the deployment resource is skipped.


## Restoring the namespaces of a Kubernetes cluster to the same location
{: #recovering-restoring-same-location}

1. In the {{site.data.keyword.baas_full_notm}} UI, navigate to `Dashboard` \> `Data Protection` \> `Recoveries`.
2. Click `Recovery `on the upper-right of the page and then select `Kubernetes Cluster` \> `Namespace` as the recovery source.<br> The Kubernetes Namespaces page appears.
3. Search for a namespace or Protection Group that contains the snapshots to recover from by entering characters of the namespace name, Protection Group name, or by specifying the wildcard character `*`.
   You can optionally filter the search results by source, Protection Group, Storage Domain, or date.

4. From the list of search results, select one or more objects (namespaces) or Protection Groups that contain the snapshots to recover. By default, the latest snapshot is selected for recovery.

   If you have selected a Protection Group, then all the namespaces that are backed up in the latest protection that is run of this Protection Group are selected for recovery. The option to select these namespaces individually are grayed out in the search result list. You can select Protection Groups and namespaces with snapshots that are stored in the same Storage Domain, and belonging to the same source. Once you select a namespace or Protection Group, the **Storage Domain** and **Source** filters are automatically locked.
   {: note}

5. When recovering from a different snapshot, perform these steps depending on the object selected:

   In this workflow, deleted snapshots may be displayed as valid recover points for a brief period until they are removed from the search index by a background process.
   {: note}

   To choose a different snapshot for a Protection Group:
   - From the selected lists, mouse over the snapshot, and then click the `edit` icon.
   - From the `Protection Run` drop-down menu, select the snapshot from which you want to recover the namespaces. By default the snapshot is recovered from the local cluster.
   - Optionally, you can also select the snapshot that is archived to the cloud by clicking the cloud icon if available in the `Location` option.
   - Click `Select Recovery Point` and then go to step 6.

   To choose a different snapshot for a Namespace:
   - From the selected lists, mouse over the snapshot, and then click the `edit` icon.<br>
   The Edit recovery point for <namespace_name> page appears. By default, the page displays the snapshots available in the last one month.
   - Optionally, you can also select a snapshot from a different date range or by selecting the date range from the `Date` drop-down list. Once you select the date range and then click `Apply`, the Edit recovery point page displays the available snapshots in the specified range.
   - Select the snapshots to be recovered.
   - Optionally, you can also select snapshots that are archived to the cloud by clicking the cloud icon if available in the `Location` option.
   - Click Select Recovery Point and then go to step 6.

6. Click Next: Recover Options.
7. From the `Recover To` options, select `Original Location` to recover the namespace to the original location.
8. Edit the default settings of the following Recovery Options:
     - **Rename**: Add Prefix and/or Suffix strings to the names of the new namespaces created by this task.<br> Example: Prefix `Test_` and Suffix `_QA` applied to namespace `App` becomes `Test_App_QA`.
     - **Task Name**: Change the default name of the recovery task.
     - **Namespace Resources**: Allow you to select the namespace resources you want to recover. Enable the following options to customize these settings: Click the edit icon on **Namespace Resources**.
        - **Resources**: In the Resources you can select the following options:
           - **Persistent Volume Claim (PVC) Inclusion/Exclusion**: Enable this option to choose which PVCs to include or exclude in the recovery task. You also have an option to recover only PVCs and their dependent resources. The dependent resources include: ConfigMaps and PersistentVolumes.
           - **Resource Inclusion/Exclusion**: Enable this toggle to include or exclude resource classes, and then select the protected resource from the drop-down.
        - **Storage Class**: Enable the following options to customize these settings:
           - **Unbind PVCs from original PV mapping**: Before using this recovery option, you must specify the location of the BRS DataProtect plug-in image in the **BRS DataProtect Plugin Image Path** field during registration of a source.<br> When enabled, the DataProtect Plugin Image clears certain fields from all PVCs within the namespace during the recovery process. This action removes references to the original PersistentVolume (PV), allowing the recovered PVCs to dynamically provision a new PV or bind to any available PV.<br> This option is useful if the snapshot contains PVCs with a Retain reclaimPolicy.
           - To map recovered PVCs to a different storage class:
              - Click `+ Add` to create a new mapping. Each mapping includes two drop-down lists:
                 - Left drop-down: Select a storage class from the source (backed-up) Kubernetes cluster.
                 - Right drop-down: Select a storage class from the target Kubernetes cluster.
              - This mapping helps ensure that any PVCs using the selected source storage class are re-created that uses the selected target storage class during recovery.
              - To create more mappings, click `+ Add`.
        - Recover PVCs Only: Enable the Recover only PVC and related resources under the PVC Inclusion/Exclusion from the Namespace selected option if you want to recover only PVCs and not the metadata from the namespace.
        - **Include or Exclude Labels**: Enable the **Enable PVC Inclusion/Exclusion** option and provide the following information:
          - In the `Logical Rule` drop-down, select:
              - `Match Any of the following labels`
              - `Match All of the following labels`
          - Select `Include`, click `+ Add`, and provide the key and value of the label that is assigned to the PVCs you want to include.<br> Or <br> Select Exclude, click + Add, and provide the key and value of the label that is tagged to the PVCs you want to exclude.
9. Click `Recovery`.<br> The recovery task is initiated. You can monitor the recovery task from the Recoveries page.


## Recovering Namespaces to a different Location
{: #recovering-different-location}

You can recover the namespaces of the Kubernetes to an alternative Kubernetes cluster that is registered with the BRS instance. If you are recovering the Kubernetes namespace to an alternative location, then the entire namespace is restored. However, if you are restoring to this location again, then BRS will not restore the resources, PVCs, or the metadata of the Kubernetes namespace that already exists in the location.

To restore the namespaces of a Kubernetes cluster to a new location:

1. In the {{site.data.keyword.baas_full_notm}} UI, navigate to: `Dashboard` \> `Data Protection` \> `Recoveries`.
2. Click `Recovery` on the upper-right of the page and then select `Kubernetes Cluster` \> `Namespace` as the recovery source.<br> The Kubernetes Namespaces page appears.
3. Search for a namespace or Protection Group that contains the snapshots to recover from by entering characters of the namespace name, Protection Group name, or by specifying the wildcard character `*`.<br> You can optionally filter the search results by source, Protection Group, Storage Domain, or date.
4. From the list of search results, select one or more objects (namespaces) or Protection Groups that contain the snapshots to recover. By default, the latest snapshot is selected for recovery.

   If you have selected a Protection Group, then all the namespaces that are backed up in the latest protection that is run of this Protection Group will be selected for recovery. The option to select these namespaces individually will be grayed out in the search result list.
   You can select Protection Groups and namespaces with snapshots that are stored in the same Storage Domain and belonging to the same source. Once you select a namespace or Protection Group, the `Storage Domain` and `Source` filters will automatically get locked.
   {: note}

5. To recover from a different snapshot, perform the following steps depending on the object selected. In this workflow, deleted snapshots may be displayed as valid recover points for a brief period until they are removed from the search index by a background process.
   - To choose a different snapshot for a Protection Group:
      - From the selected lists, mouse over the snapshot, and then click the `edit` icon.
      - From the `Protection Run` drop-down menu, select the snapshot from which you want to recover the namespaces. By default the snapshot is recovered from the local cluster.
      - Optionally, you can also select the snapshot that is archived to the cloud by clicking the cloud icon if available in the Location option.
      - Click Select Recovery Point and then go to step 6.
   - To choose a different snapshot for a Namespace:
      - From the selected lists, mouse over the snapshot, and then click the `edit` icon.<br> The Edit recovery point for <namespace_name> page appears. By default, the page displays the snapshots available in the last one month.
      - Optionally, you can also select a snapshot from a different date range or by selecting the date range from the `Date` drop-down list. Once you select the date range and then click `Apply`, the Edit recovery point page displays the available snapshots in the specified range.
      - Select the snapshots to be recovered.
      - Optionally, you can also select snapshots that are archived to the cloud by clicking the cloud icon if available in the `Location` option.
      - Click `Select Recovery Point` and then go to step 6.
6. Click `Next: Recover Options`.
7. From `Recover To`, select `New Location`.
8. From `Registered Source`, select the Kubernetes source or click `Register Source`.
9. Optional. Edit the default Recovery Options:
   - **Rename**: Add Prefix and/or Suffix strings to the names of the new namespaces created by this task. For example, you might specify Prefix `Test_` and Suffix `_QA` to the namespace’s name `App` and when the namespace is recovered, the new namespace that is created is called `Test_App_QA`.
   - **Task Name**: Change the default name of the recovery task.
   - **Namespace Resources**: Allows you to select the namespace resources you want to recover. Enable the following options to customize these settings: Click the edit icon on **Namespace Resources**.
      - **Resources**: In the Resources you can select the following options:
         - **Persistent Volume Claim (PVC) Inclusion/Exclusion**: Enable this option to choose which PVCs to include or exclude in the recovery task. You also have an option to recover only PVCs and their dependent resources. The dependent resources include: ConfigMaps and PersistentVolumes.
         - **Resource Inclusion/Exclusion**: Enable this toggle to include or exclude resource classes, and then select the protected resource from the drop-down.
      - **Storage Class**: Enable the following options to customize these settings:
         - **Unbind PVCs from original PV mapping**: Before using this recovery option, you must specify the location of the BRS DataProtect plug-in image in the **BRS DataProtect plug-in Image Path** field during registration of a source.<br> When enabled, the DataProtect plug-in Image clears certain fields from all PVCs within the namespace during the recovery process. This action removes references to the original PersistentVolume (PV), allowing the recovered PVCs to dynamically provision a new PV or bind to any available PV.<br> This option is useful if the snapshot contains PVCs with a Retain reclaimPolicy.
         - To map recovered PVCs to a different storage class:
            - Click `+ Add` to create a new mapping.<br> Each mapping includes two drop-down lists:
               - Left drop-down: Select a storage class from the source (backed-up) Kubernetes cluster.
               - Right drop-down: Select a storage class from the target Kubernetes cluster.
            - This mapping helps ensure that any PVCs using the selected source storage class are re-created that uses the selected target storage class during recovery.
            - To create more mappings, click `+ Add`.
   - **Additional capability — alternative region/zone recovery**: BRS also supports recovering a Kubernetes cluster namespace to an alternative region/zone. To use this feature:
      - Specify up to 1 region mapping and one or more zone mappings.
      - BRS inspects the following resource types and modify region/zone information:
         - Storage Classes:
            - Edits region/zone in parameters.
            - Edits allowedTopologies if present.
         - Secrets:
            - Edits storage class secrets that customize PVCs.
            - Only secrets with the same name as the PVC will be modified.
            - Only secrets with type:
               - vpc.block.csi.ibm.io
               - openshift-storage.rbd.csi.ceph.com
               - openshift-storage.cephfs.csi.ceph.com
               - pxd.portworx.com
         - Persistent Volumes: If a PV is being restored, BRS modifies:
            - If a PV is being restored, BRS modifies:
               - zone/region volumeAttributes
               - nodeAffinity zone/region information
   - **Recover PVCs Only**: Enable the Recover only PVC and related resources under the PVC Inclusion/Exclusion from the Namespace selected. option if you want to recover only PVCs and not the metadata from the namespace.
      - **Include or Exclude Labels**: Enable the `Enable PVC Inclusion/Exclusion` option and provide the following information:
         - In the `Logical Rule` drop-down, select:
            - “Match Any of the following labels”
            - “Match All of the following labels”
         - Select `Include`, click `+ Add`, and provide the key and value of the label that is assigned to the PVCs you want to include.<br> Or <br> Select `Exclude`, click `+ Add`, and provide the key and value of the label that is tagged to the PVCs you want to exclude.
10. Click `Recovery`.<br> The recovery task is initiated. You can monitor the recovery task from the Recoveries page.

## Features on the UI
{: #recovery-ui-features}

| Feature | Description |
|--------|-------------|
| Rename | Allows you to choose the name of the namespace that will be created or restored in the destination cluster. This lets you restore the source namespace under a new or custom name. |
| Task Name | Specifies the name that is assigned to the migration task within the workflow. This helps identify and track the task during execution and troubleshooting. |
| Cluster Resources | Defines which cluster-scoped resources should be included in the migration. These resources are restored at the cluster level on the destination environment. |
| Namespace Resources | Displays only the resources that exist in the source namespace and allows you to select which ones to migrate from a drop-down list. This helps us restore only relevant objects from the source namespace. We can also select either PVCs and other related resources present in the namespace for Inclusion/Exclusion. |
| Skip cluster compatibility check | Bypasses version and compatibility validation between the source and destination clusters. This option allows us to restore the Kubernetes resources and objects between incompatible cluster types.(ROKS -> IKS) |
| Region Mapping | Used to modify the region parameters of custom StorageClasses during restore. When applied, the StorageClass is re-created in the destination cluster with updated region values. |
| Zone Mapping | Used to modify the zone parameters of custom StorageClasses during restore. The StorageClass is restored on the destination cluster with the new zone values applied. |
| Include/Exclude Labels | Applies label-based filtering specifically for PersistentVolumeClaims (PVCs). You can choose to include or exclude PVCs based on their labels to fine-tune what gets migrated. |
