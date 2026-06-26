---

copyright:
  years: 2026, 2026
lastupdated: "2026-06-26"

keywords: ocp virtualization, openshift virtualization, kubevirt, virtual machine, vm recovery, vm restore, roks, recover

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Recover VMs for OpenShift Virtualization
{: #recover-restore-ocpvirt-vm}

After protecting your virtual machines (VMs) running on OpenShift Virtualization, you can use {{site.data.keyword.baas_full_notm}} to restore them to:
- The original Red Hat OpenShift cluster on IBM Cloud
- A different Red Hat OpenShift cluster on IBM Cloud that is registered with {{site.data.keyword.baas_full_notm}} with openshift virtualization operator pre-deployed.

This support is available only for Red Hat OpenShift on IBM Cloud that is configured for OpenShift Virtualization.

OpenShift Virtualization for IBM Backup and Recovery is not generally available. This feature is intended for testing and proof-of-concept scenarios in the AU-SYD region, and is not recommended for production workloads.
{: important}

For OpenShift Virtualization cluster requirements and setup guidance, see [Set-up OpenShift Virtualization on IBM Cloud](/docs/backup-recovery?topic=backup-recovery-ocp-virt-setup).

Recovery supports full VM restore. You can recover one or more selected VMs from a backed-up namespace. The selected VMs are restored together with their associated DataVolumes and PVCs.

## Recover VMs to the original or a different Red Hat OpenShift cluster on IBM Cloud
{: #recover-ocpvirt-same-or-new-location}

When you recover backed-up VMs to their original cluster or to a different registered Red Hat OpenShift cluster on IBM Cloud, {{site.data.keyword.baas_full_notm}} does not overwrite existing VMs or namespace resources. If the target namespace already contains the same VM, the existing VM is skipped.

If no individual VM selection is made during recovery, all backed-up VMs in the selected namespace backup are restored.

## Steps to recover backed-up VMs
{: #recover-restore-ocpvirt-steps}

1. [Access your {{site.data.keyword.baas_full_notm}} instance](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#data-source-connector-iks-roks-access-instance).
2. Go to `Dashboard` > `Data Protection` > `Recoveries`.
3. Click `Recover`, and then select `Kubernetes Cluster`.
4. In the **New Recovery** view, search for the namespace backup or protection group that contains the VM that you want to restore.
5. Select the recovery point:
   - To recover the latest snapshot, select the item.
   - To recover a specific snapshot, click the edit icon and choose the required recovery point.
6. Click `Next: Recover Options`.
7. Under **Recover To**, choose one of the following:
   - **Original Location** to recover to the same Red Hat OpenShift cluster on IBM Cloud
   - **New Location** to recover to a different registered Red Hat OpenShift cluster on IBM Cloud
8. If you are recovering to a different cluster, select the destination registered source.
9. In **Rename**, optionally keep the default prefix `copy-` or enter a different prefix or suffix for the restored namespace.
10. In **Task Name**, review or update the recovery task name.
11. In **Namespace Resources**, click the edit icon for the namespace that contains the backed-up VMs.
12. In the namespace recovery options, use the available VM inclusion or exclusion option to select the VMs that you want to restore.

    | Option | Description |
    |------|-------------|
    | **VM Inclusion/Exclusion** | Select the VM names that you want to include or exclude. Only the selected VMs are restored, based on the option that you choose. |
    {: caption="VM recovery selection" caption-side="bottom"}

13. Review the remaining namespace resource options, such as PVC inclusion or exclusion, storage class mapping, and related resource settings, if they are needed for your environment.
14. Click `Apply` to save the namespace recovery settings.
15. Click `Recover` to start the recovery.
16. Monitor the task on the `Recoveries` page.

Recovery selection for OpenShift Virtualization VMs is explicit. Label-based VM filtering is supported at backup time, but not in the recovery dialog.
{: note}

## Recovery options for OpenShift Virtualization VMs
{: #ocpvirt-recovery-options}

The recovery workflow includes the following options.

### Recover To
{: #ocpvirt-recover-to}

- **Original Location**: Restores the VM to the same registered Red Hat OpenShift cluster on IBM Cloud.
- **New Location**: Restores the VM to a different registered Red Hat OpenShift cluster on IBM Cloud.

### Rename
{: #ocpvirt-recovery-rename}

You can add a prefix or suffix to the recovered namespace name. By default, the recovery workflow prefixes the namespace with `copy-`.

In the product walkthrough, a prefix such as `demo-` was used to restore the VM into a new namespace, for example `demo-<source-namespace>`.

### Task Name
{: #ocpvirt-recovery-task-name}

Use this field to assign a meaningful name to the recovery task so that it is easier to identify on the `Recoveries` page.

### Namespace resources
{: #ocpvirt-recovery-namespace-resources}

From the namespace edit dialog, you can control what is restored:

- Select specific backed-up VMs for granular VM recovery
- Review included PVCs that belong to the selected VMs
- Configure PVC inclusion or exclusion if required
- Configure storage class mappings if the destination cluster uses different storage classes

When a VM is selected for restore, its associated PVCs and DataVolumes are restored together with the VM.
{: note}

### Storage class mapping
{: #ocpvirt-storage-class-mapping}

If the destination cluster uses different storage classes, map the original storage class to a new storage class in the **Storage Class** tab of the namespace recovery options.





## Monitoring VM recoveries
{: #monitoring-ocpvirt-recoveries}

You can monitor recovery tasks from `Dashboard` > `Data Protection` > `Recoveries`.

The page displays:
- Recovery task name
- Start time
- Status
- Duration

Recovery states include `Succeeded`, `Warning`, `Failed`, `Running`, and `Canceled`.

In one feature demonstration, restoring a single VM to a new namespace completed in about seven minutes. The workflow created the new namespace, restored the VM, and preserved the test file that existed in the source VM before backup.

## Known issues and limitations
{: #known-issues-ocpvirt-recovery}

The following recovery limitations apply to OpenShift Virtualization VMs:

- **No overwrite of existing namespaces or VMs**: If the target namespace already contains the same VM, the restore skips the existing VM backup.
- **Namespace-level restore lock**: Only one restore job can target the same namespace at a time. Additional restore jobs are queued.

## Related information
{: #ocpvirt-recovery-related-information}

- [Protecting OpenShift Virtualization VMs and scheduling a backup](/docs/backup-recovery?topic=backup-recovery-protecting-namespace-ocpvirt-vm)
- [Creating a data source connection and registering a Kubernetes source](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks)
- [Managing Protection Group Runs](/docs/backup-recovery?topic=backup-recovery-protection-group-run-now)
