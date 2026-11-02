---
copyright:
  years: 2026
lastupdated: "2026-11-02"

keywords: ibm db2, database protection, register source, data protection, universal data adapter

subcollection: backup-recovery

content-type: tutorial
services: backup-recovery
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Register and manage the IBM DB2 source
{: #register-ibm-db2-source}
{: toc-content-type="tutorial"}
{: toc-services="db2"}
{: toc-completion-time="30m"}

To protect your IBM DB2 databases, you need to register your IBM DB2 deployment as a source on the {{site.data.keyword.baas_full_notm}}cluster. After registering your IBM DB2 deployment as a source, you can update the IBM DB2 source configuration or unregister the IBM DB2 source from the {{site.data.keyword.baas_full_notm}}cluster.
{: shortdesc}

## Objectives
{: #register-ibm-db2-objectives}

In this tutorial, you learn how to:

* Register IBM DB2 as a source
* Update the IBM DB2 source configuration
* Unregister the IBM DB2 source

## Before you begin
{: #register-ibm-db2-prereqs}

Before you register IBM DB2 as a source, ensure that you:

* Read and understand the log backup methods provided by {{site.data.keyword.baas_full_notm}}
* Review the considerations
* Perform the prerequisite tasks

For more information, see [Plan and Prepare for IBM DB2 Protection](/docs/db2?topic=db2-plan-prepare-db2-protection).

## Register IBM DB2 as a source
{: #register-ibm-db2}
{: step}

To register IBM DB2 as a source:

1. In the {{site.data.keyword.baas_full_notm}}Dashboard, select **Data Protection** > **Sources**.

2. Click **Register** on the top-right of the page and then select **Universal Data Adapter**.

3. In the **Register Universal Data Adapter** page, perform the following:

   a. From the **Source Type** drop-down menu, select **IBM DB2**.

   b. From the **Host OS Type** drop-down menu, select **Linux**.

   c. In the **Hostname/IP Addresses** field, enter the hostname or IP address of the node(s) that you have identified to run the {{site.data.keyword.baas_full_notm}}DB2 connector. For more information, see [Plan and Prepare for IBM DB2 Protection](/docs/db2?topic=db2-plan-prepare-db2-protection).

   {{site.data.keyword.baas_full_notm}}supports backup and restore of DB2 databases running on dual-stack (IPv4 and IPv6) mode or single-stack (only IPv6) mode.
   {: note}

   d. In the **Datasource Agent Installation Path** field, update the directory path if you have installed the {{site.data.keyword.baas_full_notm}}DB2 connector at the non-default location. For more information, see [Install the {{site.data.keyword.baas_full_notm}}DB2 Connector RPM](/docs/db2?topic=db2-install-db2-connector).

   e. Optional. Enable **Kerberos Authentication** Settings, if you want to connect to the IBM DB2 database using Kerberos-based authentication. Enter the following details:

   | Field | Mandatory? | Description |
   | ----- | ---------- | ----------- |
   | **Kerberos Principal** | No | The Kerberos principal that is required to connect to the database. |
   | **Keytab File Path** | No | The absolute path of the keytab file for the given Kerberos principal. \n \n The path must be accessible by the protection (backup/restore) user. |
   | **Kerberos credential cache path** | No | The absolute path of the Kerberos ticket cache. \n \n The path must be accessible by the protection (backup/restore) user. |
   {: caption="Kerberos authentication settings" caption-side="bottom"}

   At a time, you can use either Keytab OR Kerberos cache-based authentication.
   {: note}

   f. Enable **{{site.data.keyword.baas_full_notm}}SSL Settings** to enable a secure connection to the {{site.data.keyword.baas_full_notm}}cluster while reading or writing data. Enter the path to the SSL certificate used to securely connect to the {{site.data.keyword.baas_full_notm}}cluster.

   * It is **mandatory** to connect to the {{site.data.keyword.baas_full_notm}}cluster securely using secure gRPC when writing or reading data.
   * Ensure that you generate and copy the certificate config file from the {{site.data.keyword.baas_full_notm}}node to all the IBM DB2 nodes. For more information, see [Prerequisites](/docs/db2?topic=db2-prerequisites).
   * Using non-secure gRPC for data transfer between {{site.data.keyword.baas_full_notm}}and database hosts will result in unsecured communication.
   * Enabling secure gRPC will increase data transfer time between {{site.data.keyword.baas_full_notm}}and database hosts compared to non-secure communication.
   {: important}

   g. Under **Source Settings**, enter the details of the IBM DB2 source:

   | Field | Mandatory? | Description |
   | ----- | ---------- | ----------- |
   | **Datasource Name** | Yes | A unique name to identify your IBM DB2 cluster. \n \n After the source registration, you cannot change the Datasource name. |
   | **Protection Username** | Yes | The username of the user account in the IBM DB2 server that has required privileges to perform backup or restore of DB2 databases. \n \n The protection user must have SYSADM, SYSCTRL, or SYSMAINT privileges. |
   | **Instance Name** | Yes | The name of your IBM DB2 instance. |
   | **Environment Variables** | No | Provide the environment variables if you want to override the environment's default value. For example, `PATH=/sbin:/bin:/usr/bin` |
   | **Use {{site.data.keyword.baas_full_notm}}storage for log archival using LOGARCHMETH2** | No | Enable to use {{site.data.keyword.baas_full_notm}}storage for log archival using LOGARCHMETH2. For more information, see Log Backup Methods. \n \n By default the option is disabled and the dump and sweep log backup method is used. For more information, see Log Backup Methods. |
   | **Profile Path** | Yes | The path to the DB2 Profile of the instance. \n \n For example: `/home/db2inst1/sqllib/db2profile` |
   {: caption="IBM DB2 source settings" caption-side="bottom"}

   h. Click **Register**.

## Update the IBM DB2 source configuration
{: #update-ibm-db2-source}
{: step}

After registering your IBM DB2 as a source on the {{site.data.keyword.baas_full_notm}}cluster, you can update the IBM DB2 source configuration.

* Ensure that the {{site.data.keyword.baas_full_notm}}DB2 connector is installed on all the node(s) in the IBM DB2 cluster.
* You cannot modify the source name.
{: important}

To update the IBM DB2 source configuration:

1. In the {{site.data.keyword.baas_full_notm}}Dashboard, select **Data Protection** > **Sources**.

2. Click the actions menu (⋯) next to the IBM DB2 source and select **Edit**.

3. On the **Update Universal Data Adapter** page, update the required configurations.

4. Click **Update**.

## Unregister the IBM DB2 source
{: #unregister-ibm-db2-source}
{: step}

If you plan to stop protecting your IBM DB2 databases, you can unregister the IBM DB2 source from the {{site.data.keyword.baas_full_notm}}cluster.

To unregister the IBM DB2 source:

1. In the {{site.data.keyword.baas_full_notm}}Dashboard, select **Data Protection** > **Sources**.

2. Click the actions menu (⋯) next to the IBM DB2 source and select **Unregister**.

## Next steps
{: #register-ibm-db2-next-steps}

After registering your IBM DB2 source, you can:

* [Create a protection policy](/docs/db2?topic=db2-create-protection-policy)
* [Protect IBM DB2 databases](/docs/db2?topic=db2-protect-db2-databases)
* [Monitor backup jobs](/docs/db2?topic=db2-monitor-backup-jobs)
