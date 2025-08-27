---

copyright:
  years: 2025
lastupdated: "2025-08-27"

keywords: performance, backup and recovery, saas connectors, source load, source drive configuration, structure of data, size of files

subcollection: backup-recovery


---

{{site.data.keyword.attribute-definition-list}}

# Factors affecting the Performance of {{site.data.keyword.baas_full}} service
{: #performance-factors}

Many factors will impact the performance an end user sees with the {{site.data.keyword.baas_full_notm}} service. The following lists the major factors for both backups and recoveries, roughly in order of importance to each.
{: shortdesc}

## Backups
{: #performance-factors-backups}

Listed in order of importance:

1. **Number of SaaS Connectors** - In general, increasing the number of SaaS connectors should increase performance of backups. Once the number of entities to be recovered is less than or equal to the number of SaaS connectors, additional SaaS connectors will provide little increase in performance. See below for guidance on determining the initial number of SaaS connectors and for monitoring for SaaS connectors becoming bottlenecks.
2. **Size of Files** - For file backups, larger files (> ~100KB) tend to be bound by the throughput of the SaaS connector; smaller files (< ~100KB) tend to be bound by the number of files per second that can be processed. For MS SQL databases, processing is dependent on the size of the records of the database and how rapidly the MS SQL server can deliver them for backup.
3. **Structure of Data** - In general, data that is highly compressible (i.e., data that has a high dedup rate) will be backed up faster. At the extreme, completely random data will have the lowest backup performance.
4. **Source Drive Configuration** - Performance of file backup can be dependent on the performance characteristics of the drive from which the data is being read. For drives with specified guaranteed IOPS, testing has shown that this impact is small. However, for VMs using VSAN drives, performance may be impacted heavily by the disk activity of other VMs in the VDC sharing the VSAN block (all VMs in the VDC share the same VSAN block, and there is no guaranteed IOPS rates).
5. **Source Load** - The amount of non-backup load on sources will impact the performance of backup; high loads on the source will negatively impact the performance of backup, since the source will have less bandwidth to deliver information for backup. In particular, it has been found that some anti-virus software will perform real-time scanning of data being restored, slowing down backup.
6. **Source Configuration** - A source with higher processing power and memory will provide better backup performance. This especially is true for very small sources (i.e., 1-2 CPU with small amounts of memory).

## Recovery
{: #performance-factors-recovery}

Listed in order of importance:

1. **Source Drive Configuration** - Performance of file recovery is heavily dependent on the performance characteristics of the drive to which the data is to be written. A high IOPS drive will provide better write performance, increasing the performance of the recovery. Conversely, a low IOPS drive will provide worse recovery performance. In the case of VMs using VSAN drives, performance may be impacted heavily by the disk activity of other VMs in the VDC sharing the VSAN block (all VMs in the VDC share the same VSAN block, and there is no guaranteed IOPS rates).
2. **Size of Files** - For file recoveries, larger files (> ~100KB) tend to be bound by the throughput of the SaaS connector; smaller files (< ~100KB) tend to be bound by the number of files per second that can be processed. For MS SQL databases, processing is dependent on the size of the records of the database and how rapidly the MS SQL server can process them for recovery.
3. **Structure of Data** - In general, data that is highly compressible (i.e., data that has a high dedup rate) will be recovered faster. At the extreme, completely random data will have the lowest recovery performance.
4. **Number of SaaS Connectors** - In general, increasing the number of SaaS connectors will increase the performance of recovery, although this increase will be smaller. However, this can be nuanced. Once the number of entities to be recovered is less than or equal to the number of SaaS connectors, additional SaaS connectors will provide little increase in performance. See below for guidance on determining the initial number of SaaS connectors and for monitoring for SaaS connectors becoming bottlenecks.
5. **Source Load** - The amount of non-backup load on sources will impact the performance of recovery: high loads on the source will negatively impact the performance of recovery, since the source will have less bandwidth to receive and process information for recovery. In particular, it has been found that some anti-virus software will perform real-time scanning of data being restored, slowing down recovery.
6. **Source Configuration** - A source with higher processing power and memory will provide better recovery performance. This especially is true for very small sources (i.e., 1-2 CPU with small amounts of memory).

## Warning for MS SQL Backup and Recovery
{: #performance-factors-warning}

During creation of a protection group to back up MS SQL databases, it is possible to configure the number of streams used in the backup (see picture). Currently, IBM recommends that this value NOT be changed from the default value of 3 without first contacting IBM support. Changing this to a higher value may cause a SaaS Connector to crash, while changing it to a lower value will degrade backup performance.
{: important}

## Planning Initial Number of SaaS Connectors in a Connection
{: #performance-factors-planning-saas-connect}

Please work with IBM support to determine the initial number of SaaS connectors. However, a good rule of thumb is to start with 1 SaaS connector for every 5-10 sources. For reliability reasons and to ensure upgrades without service interruptions, a minimum of 2 SaaS connectors should be deployed. For very large databases (greater than 1TB), a single pair of SaaS connectors should suffice.

### Triggers for Increasing the Number of SaaS Connectors in a Connection
{: #performance-factors-planning-triggers}

The load on a SaaS connector can be found in the following manner:

1. Log into the backup instance.
2. Go to System.
3. Go to Data Source Connections.
4. Click on the name of the connection.
5. Click on the IP address of the desired SaaS connector.
6. Examine the Utilization (%) graph.

If the SaaS connectors in a connection are operating at 100% CPU for most of or the whole of a backup window, it indicates that the SaaS connectors are operating at maximum capacity. In this case, it probably would be valuable to add one or more additional SaaS connector to the connection. In general, it would be best to add a single SaaS connector and again review the CPU utilization and continue to add additional SaaS connectors one at a time until the CPU utilization falls below 100%. In general, it is best if all SaaS connectors in a connection operate at less than or equal to 75% CPU utilization.

If adding an additional SaaS connector does not improve performance, it indicates that some other bottleneck has been reached, and it is not useful to any more to the connection.  (Add SaaS Connectors 1 at a time until performance doesn’t improve).




