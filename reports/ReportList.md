---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.baas_full}} reports
{: #{{site.data.keyword.baas_full}}_reports}


{{site.data.keyword.baas_full_notm}} provides the following reports:


| Report Name | Description |
| --- | --- |
| Available Local Snapshots Per Job | This report provides Snapshot summary statistics for a leaf Protection Source Object (such as a VM) for all Protection Groups or a specific Protection Group.<br><br>For detailed information about this report, see [Available Local Snapshots](ReportAvailableLocalSnapshotsPerJob.htm). |
| Agent Status Summary | This report provides information about the Backup agents installed on servers registered as Sources on the Cluster.<br><br>For detailed information about this report, see [Agent Status](ReportAgentStatusSummary.htm). |
| Backup Summary | This report shows an overview of the Protection Groups run on the {{site.data.keyword.baas_full_notm}} for the specified filter criteria. For each Protection Group, this report shows how many times the Job has run, total bytes read and statistics about the last time the Job ran.<br><br>For detailed information about this report, see [Backup Summary](ReportBackupSummaryReport.htm#top). |
| Clone and Recover VM Tasks Summary | This report shows an overview of Clone and Recover tasks for the specified date range.<br><br>For detailed information about this report, see [Clone and Recover VM Tasks Summary](ReportCloneTasksSummaryReport.htm). |
| Cluster Wide Storage | This report provides Cluster storage statistics that are useful for administrators to plan out disk space utilization.<br><br>For detailed information about this report, see [Cluster Wide Storage](ReportClusterWideStorage.htm). |
| Data Transferred to External Targets | This report provides statistics about the data transferred to External Targets for archiving Snapshots created by Protection Groups, and moving cold data from the {{site.data.keyword.baas_full_notm}} to Cloud Tier.<br><br>For detailed information about this report, see [Data Transferred to External Targets](ReportDataTransferredToExternalTargets.htm). |
| Failed Objects | This report lists all the Objects for which an error occurred during the last protection runs and prevented the Objects from being backed up.<br><br>For detailed information about this report, see [Failed Objects](ReportFailedObjects.htm). |
| GDPR Object Summary | This report provides information about whether an Object is protected, who has access to it, where Object Snapshots are stored and how long they are retained, and any actions performed on the Object.<br><br>For detailed information about this report, see [GDPR Objects Summary](ReportGDPRObjectSummary.htm). |
| Objects Protected by Multiple Protection Groups | This report lists all the Objects that have been backed up (protected) by more than one Protection Group.<br><br>For detailed information about this report, see [Objects Protected by Multiple Protection Groups](ReportObjectsProtectedbyMultipleBackupJobs.htm). |
| Protected Objects Heatmap | This report provides an interactive grid of cells that indicate whether the Backup Tasks associated with Protection Group runs were successful (green cells) or encountered errors (red cells).<br><br>For detailed information about this report, see [Protected Objects Heatmap](ReportProtectedObjectsHeatmap.htm). |
| Protection Details Per Object | This report provides Protection Run (Snapshot) details for a leaf Protection Source Object (such as a VM).<br><br>For detailed information about this report, see [Protection Details Per Object](ReportProtectionDetailsPerObject.htm). |
| Protection Groups Inventory and Schedule | This report lists all the Protection Groups on the {{site.data.keyword.baas_full_notm}} matching the specified filter criteria. For each Protection Group, this report lists all the Objects that will be backed up by this group and the schedule for when the Objects will be backed up.<br><br>For detailed information about this report, see [Protection Groups Inventory and Schedule](ReportProtectionJobsInventoryandSchedule.htm). |
| Protection Runs Summary | This report provides information about Protection Group runs and related replication and archival tasks.<br><br>For detailed information about this report, see [Protection Runs Summary](ReportProtectionRunsSummary.htm). |
| Protection Summary by Object Type | This report shows the protection status of Objects as of the last protection runs that occurred in the specified Date Range.<br><br>For detailed information about this report, see [Protection Summary By Object Type](ReportProtectionSummaryByObjectType.htm). |
| Source Growth and Variance | This report shows how the size of protected Servers per registered Source are changing over time.<br><br>For detailed information about this report, see [Source Growth & Variance](ReportSourceGrowthandVariance.htm#top). |
| Sources Protected | This report provides overview statistics about the number of Servers protected or unprotected for registered Sources on the {{site.data.keyword.baas_full_notm}}.<br><br>For detailed information about this report, see [Sources Protected](ReportSourcesProtected.htm). |
| Storage Consumed by Backups | This report provides statistics about the Protection Groups that are consuming the most disk space on the {{site.data.keyword.baas_full_notm}}.<br><br>For detailed information about this report, see [Storage Consumed by Protection Groups](ReportStorageConsumedbyBackups.htm#top). |
| Storage Consumed by File Categories | This report provides data statistics about File Categories in the latest Snapshots for all Servers stored on the {{site.data.keyword.baas_full_notm}}. Statistics are only reported for files indexed by the {{site.data.keyword.baas_full_notm}}.<br><br>For detailed information about this report, see [Storage Consumed by File Categories](ReportStorageConsumedbyFileCategories.htm). |
| Storage Consumed by Servers | This report provides statistics about the Servers that are using the most disk space on the {{site.data.keyword.baas_full_notm}} and Primary Storage.<br><br>For detailed information about this report, see [Storage Consumed by Servers](ReportStorageConsumedbyServers.htm). |
| Storage Consumed by Storage Domains | This report provides information about the Storage Domains that are storing the most logical data and raw data data on the {{site.data.keyword.baas_full_notm}}.<br><br>For detailed information about this report, see [Storage Consumed by Storage Domains](ReportStorageConsumedbyViewBoxes.htm). |
| Storage Consumed by Organizations | This report provides information about the organizations consuming the most storage.<br><br>For detailed information about this report, see [Storage Consumed by Organizations](ReportStorageConsumedbyOrganizations.htm). |
| Top Jobs | This report shows the top Protection Groups by the average amount of data transferred, the length of Protection Group run time, the number of Objects protected and the number of SLA violations.<br><br>For detailed information about this report, see [Top Protection Group](ReportTopJobs.htm). |
| Unprotected VMs | This report provides lists of VMs that are protected and not protected by {{site.data.keyword.baas_full_notm}} for the registered Sources on the {{site.data.keyword.baas_full_notm}}. In addition, overview statistics about protected and unprotected VMs are provided.<br><br>For detailed information about this report, see [Unprotected/Protected VMs](ReportUnprotectedVMs.htm). |
| User Quota | This report provides information about the default user quota defined for a View and any overrides defined for individual users.<br><br>For detailed information about this report, see [User Quotas](ReportUserQuotas.htm). |
{: caption="" caption-side="bottom"}
