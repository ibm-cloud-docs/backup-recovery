---

copyright:
  years: 2026, 2026
lastupdated: "2026-06-26"

keywords: ocp virtualization, openshift virtualization, kubevirt, virtual machine, vm backup, roks, protection

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Protecting VMs running on OpenShift Virtualization
{: #protecting-namespace-ocpvirt-vm}

Use {{site.data.keyword.baas_full_notm}} for protecting VMs running on OpenShift Virtualization. You can protect all VMs in a namespace by protecting the namespace, or you can protect selected VMs within a namespace by using VM inclusion, exclusion, or label-based selection.

This support is available only for Red Hat OpenShift on IBM Cloud that is configured for OpenShift Virtualization.

OpenShift Virtualization for IBM Backup and Recovery is not generally available. This feature is intended for testing and proof-of-concept scenarios in the AU-SYD region, and is not recommended for production workloads.
{: important}

For OpenShift Virtualization cluster requirements and setup guidance, see [Set-up OpenShift Virtualization on IBM Cloud](/docs/backup-recovery?topic=backup-recovery-ocp-virt-setup).

When no VM selection is configured, protection continues to work like standard Kubernetes namespace protection and backs up the selected namespace with all the resources including the VMs.

## Prerequisites for scheduling VM backups
{: #ocpvirt-prereq-schedule-backup}

Before you configure protection for OpenShift Virtualization VMs, make sure that the following requirements are met:

1. An [{{site.data.keyword.baas_full_notm}} instance](#data-source-connector-iks-roks-access-instance) is created.
2. The Red Hat OpenShift on IBM Cloud is [registered as a source](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#data-source-connector-iks-roks-register).
3. A data source connector is created and deployed on the data source cluster. For more information, see [Create or configure a data source connector](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#data-source-connector-iks-roks-create-configure).

## Level of VM Protection
{: #ocpvirt-how-vm-backup-works}

{{site.data.keyword.baas_full_notm}} protection in {{site.data.keyword.baas_full_notm}} supports the following behaviors:

- **Namespace-level protection**: If you do not select specific VMs, the backup includes all VMs and container workload in the namespace.
- **Granular VM-level protection**: You can select one or more VMs inside a namespace.
- **Label-based VM protection**: You can include or exclude VMs by labels.

## How to protect VMs running on OpenShift Virtualization
{: #protecting-ocpvirt-vms-schedule-backup-basic}

Follow these steps to protect VMs that run on OpenShift Virtualization :

1. [Access your {{site.data.keyword.baas_full_notm}} instance](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks#data-source-connector-iks-roks-access-instance).
2. Go to `Dashboard` > `Data Protection` > `Sources`.
3. Locate your registered OpenShift cluster under the source list.
4. Click the cluster endpoint to view the available namespaces.
5. Select the namespace where the VMs are running that you want to protect.
6. Click the edit icon for the namespace.
7. In the namespace options, review the **Include/Exclude VMs** section.
8. Use the available VM inclusion or exclusion option to select the VM names that you want to protect.

   | Option | Description |
   |------|-------------|
   | **VM Inclusion/Exclusion** | Select the VM names that you want to include or exclude. Only the selected VMs are backed up, based on the option that you choose. |
   {: caption="VM selection options" caption-side="bottom"}

9. Save the namespace configuration.
10. Select the protection policy. Available options are:

    - **Use an Existing Protection Policy**: Select an existing protection policy to reuse the current configuration.
    - **Create a New Protection Policy**: Create a new protection policy and configure settings such as the schedule, retention, and retries. For more information, see [Creating a protection policy](/docs/backup-recovery?topic=backup-recovery-baas-policy-creation).

11. Click `Protect` to save the configuration and start scheduled protection.
12. To monitor the run, go to `Data Protection` > `Protection`, click the protection group name, and then click a specific run to review its details.

When only selected VMs are included, the run details show the included VMs and list all other VMs in the namespace as excluded from that backup.
{: note}

## Configuration options for VM protection
{: #ocpvirt-vm-protection-advanced}

After a protection group is created, you can edit it from `Data Protection` > `Protection` > `Protection Group Name` > `⋮` > `Edit`.

For advanced settings, see [Configuration options](/docs/backup-recovery?topic=backup-recovery-protecting-namespace-iks-roks#protecting-namespace-iks-roks-advanced).

## Running an on-demand VM backup
{: #ocpvirt-run-protection-now}

You can run a protection group immediately without waiting for the next scheduled time. For instructions, see the [Protection group Run Now](/docs/backup-recovery?topic=backup-recovery-protection-group-run-now).

## Monitoring a VM backup protection run
{: #ocpvirt-monitor-backup-run}

To monitor a backup run:

1. Go to `Data Protection` > `Protection`.
2. Click the relevant protection group.
3. Click the active or completed run.
4. Review the run summary, included VMs, excluded VMs, protected PVCs, duration, and throughput.

In one feature walkthrough, a single VM with one PVC completed backup successfully in about six minutes, and the run details displayed the VM name, the associated PVC, data written, and duration.

## Managing the protection lifecycle
{: #ocpvirt-managing-protection-lifecycle}

For ongoing operations, see [Managing Protection Group Runs](/docs/backup-recovery?topic=backup-recovery-protection-group-run-now). You can:

- Run a backup immediately
- Pause or resume a protection group
- Delete protection for an object
- Review historical runs

## Known issues and limitations
{: #known-issues-ocpvirt}

The following items apply to OpenShift Virtualization VM protection :

- **Tenant Pulse log visibility**: Tenant-scoped users cannot view VM backup summary or Pulse logs for IBM-backed VM protection runs. Cluster-admin or service provider admin users are not affected.
- **Namespace-level lock for backups**: Only one protection group can back up VMs in a specific namespace at a time. If another job targets the same namespace, it is queued until the current backup finishes.
- **Namespace-level lock for restores**: Only one restore job can target a specific namespace at a time.
- **SELinux guest freeze issue**: Some VM backups might fail with `unable to execute QEMU agent command 'guest-fsfreeze-freeze'`. This is a known Red Hat issue where SELinux prevents the QEMU Guest Agent from accessing certain files that are required during the filesystem freeze operation. As a workaround, enable the `virt_qemu_ga_read_nonsecurity_files` SELinux boolean on the affected worker nodes.

## Related information
{: #ocpvirt-related-information}

- [Creating a data source connection and registering a Kubernetes source](/docs/backup-recovery?topic=backup-recovery-data-source-connector-iks-roks)
- [Creating a protection policy](/docs/backup-recovery?topic=backup-recovery-baas-policy-creation)
- [Running protection on demand](/docs/backup-recovery?topic=backup-recovery-protection-group-run-now)
