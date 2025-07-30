---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# About Bare Machine Recovery

9 September 2024

A Bare Machine Recovery (BMR) is the ability to recover a system, including the operating system, its configuration, applications running on the operating system, and its data back to a state it was at a given point.

{{site.data.keyword.baas_full_notm}} integrates with Cristie software to protect your servers for bare machine recovery (BMR). IBM Cloud Supports two types of BMR:

*   **Cristie Bare Machine Recovery (CBMR)**: In this integration, a separate schedule for BMR backup is required to be added to the protection policy.

    First, a block-based protection run is executed followed by a separate BMR run. The BMR run is always Full (no incremental). For more information, see [CBMR](../cbmr/cbmr.htm).

*   **{{site.data.keyword.baas_full_notm}} Bare Machine Recovery (CoBMR)**: In this integration, CoBMR leverages {{site.data.keyword.baas_full_notm}}'s file-based and block-based physical server protection. No separate BMR schedule or protection run is required. For more information, see [CoBMR](../cobmr/cobmr.htm).


*   For BMR-based protection of a physical server, you must first install the Backup agent on the physical server followed by the BMR agent and register the physical server with the {{site.data.keyword.baas_full_notm}}.

*   You can also deploy the Backup agent on a virtual machine and protect the VM as a physical server to perform BMR. For more information, see [Deploy Backup agent on Virtual Servers](../Dashboard/Protection/DeployAgentVirtualServer.htm).


## CBMR and CoBMR Comparison


| Feature | CBMR | CoBMR |
| --- | --- | --- |
| Supported physical servers | *   Windows<br>    <br>*   Linux<br>    <br><br>For more information, see [Supported Software](../cbmr/plan-prepare-cbmr.htm#Software). | *   Windows<br>    <br>*   Linux<br>    <br><br>For more information, see [Supported Software](../cobmr/plan-prepare.htm#Supporte). |
| Protection Policy | You must add a BMR schedule to the policy. | Default Policy - No BMR schedules. |
| Protection Group | Block-based backup | File-based backup<br><br>Block-based backup |
| Protection Run | First performs block-based protection run followed by a BMR run. | Leverages the physical file-based and block-based backup run (No separate BMR schedule) |
| System State Backup | For block-based backup: First Full followed by incremental backup.<br><br>For BMR backup: Always full backup. | First full backup followed by incremental backup. |
| Recovery ISO | For Windows servers, you must install CRISP software and create a separate ISO. For Linux servers, Cristie provides pre-build recovery ISO. | Cristie provides pre-build recovery ISO. |
| Recovery Workflow | Initiate BMR recovery from {{site.data.keyword.baas_full_notm}} to fetch the recovery path (the SMB share has the “system.vtd” file, the Custom Virtual Device Driver file). Then mount the system.vtd file in the CBMR recovery wizard to complete the recovery. | Recovery is performed through CoBMR Recovery Environment. |
