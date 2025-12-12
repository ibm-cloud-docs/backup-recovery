---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Source-side deduplication and cache optimization for physical server
{: #source-side_deduplication_and_cache_optimization_for_physical_server}


The {{site.data.keyword.baas_full}} source-side deduplication provides storage efficient deduplication by eliminating redundant data at the server level before transferring the data to the {{site.data.keyword.baas_full_notm}}. You can enable source-side deduplication for a physical server while [creating a Protection Group](/docs/backup-recovery?topic=backup-recovery-add_or_edit_a_protection_group_for_physical_servers). By enabling source-side deduplication, the data that has been partially transferred, or entirely transferred, is not transferred over the network again. This reduces the utilization of network bandwidth.

Additionally, the source-side deduplication plugin integrates with the {{site.data.keyword.baas_full_notm}}’s built-in deduplication and minimizes data traffic by moving only the new data blocks.

Advantages of the source-side deduplication:

*   Reduce network traffic between physical servers and {{site.data.keyword.baas_full_notm}}.
*   Faster backups and lower SLA if data is backed up over slower Networks/WAN.
*   Offloading deduplication and enables the {{site.data.keyword.baas_full_notm}} to run more backup workload sessions.

## Cache optimization
{: #cache_optimization}

Cache Optimization is a modified source dedup functionality that offers optimized backup performance on the {{site.data.keyword.baas_full_notm}}. It is targeted for large files(>64GiB). When enabled, only the changed blocks of the target files are backed up during the subsequent incremental backup runs instead of an entire file.

To use the Cache Optimization functionality, both the Backup agent and cluster should have version 7.0 and later.

Advantages of the Cache optimization:

*   Reduce network traffic between physical servers and {{site.data.keyword.baas_full_notm}}.

*   Less CPU intensive on the source compared to source-side deduplication.
