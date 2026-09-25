---

copyright:
  years: 2026

lastupdated: "2026-09-25"

keywords: Oracle adapter troubleshooting, Oracle agent, RMAN error, Oracle database backup, Oracle protection job, ORA error

subcollection: backup-recovery

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting the Oracle adapter
{: #troubleshoot-oracle-adapter}

Use this topic to diagnose and resolve common issues with the {{site.data.keyword.baas_full}} Oracle adapter.
{: shortdesc}

## Oracle agent troubleshooting checklist
{: #troubleshoot-oracle-adapter-checklist}

Before investigating specific errors, verify the following configuration items on the Oracle server.

1. Verify that the agent user (`cohesityagent` by default) is a member of the `dba` and/or `oinstall` groups. Use the `id` command to check:

   ```sh
   id cohesityagent
   uid=1002(cohesityagent) gid=1001(dba) groups=1001(dba)
   ```
   {: codeblock}

1. Verify that the `/etc/sudoers` file has the agent configured. For example:

   ```
   cohesityagent       ALL=(ALL)       NOPASSWD:ALL
   Defaults:cohesityagent !requiretty
   ```
   {: codeblock}

   Use spaces and not tabs, as tabs may lead to syntax errors.
   {: important}

1. Verify that the home directory of the agent user is configured with at least `drwxr-xr-x` permissions. For example:

   ```sh
   ls -ld /home/cohesityagent
   drwxr-xr-x 6 cohesityagent cohesityagent 198 Jan 17 08:58 /home/cohesityagent
   ```
   {: codeblock}

   To change the permissions, run the following command:

   ```sh
   chmod 755 /home/cohesityagent
   ```
   {: codeblock}

1. Verify that `.bashrc` in the agent user's home directory contains `umask 0002`.

1. Verify that the agent is running:

   ```sh
   /etc/init.d/cohesity-agent status
   ```
   {: codeblock}

1. Verify with your DBA that the database to be backed up is listed in `/etc/oratab` with the correct `ORACLE_HOME` path. The third field must contain `Y` or `N`. For example:

   ```
   CohesityOra:/oracle/app/oracle/product/12.2.0/dbhome_1:Y
   ```
   {: codeblock}

1. If database access uses OS authentication, the {{site.data.keyword.baas_full_notm}} agent user must have `sysdba` privileges on the database. If database authentication is used, the database user also requires `sysdba` privileges. Verify using SQL*Plus:

   ```sql
   SQL> column sysdba format a10
   SQL> select username,sysdba from v$pwfile_users;
   USERNAME   SYSDBA
   ---------- ----------
   SYS        TRUE
   COHBACKUP  TRUE
   ```
   {: codeblock}

1. Verify that the Linux physical server has been registered as a source:

   1. Navigate to **Protection** > **Sources**.
   1. Click **Register Source** in the upper right of the page.
   1. Click **Physical Server**.
   1. Enter the Oracle server host address.
   1. Click **Register Server**.

1. Verify that the Oracle database is registered:

   1. Navigate to **Protection** > **Sources**.
   1. Click **Register Source** in the upper right of the page.
   1. Click **Oracle Source**.
   1. Accept the default **Single Instance** as the source type.
   1. Enter the Oracle server host address, or browse the registered sources and select one.
   1. Select the **Authentication Type** to use when connecting to the database.
   1. Click **Register**.

## Known issues and workarounds
{: #troubleshoot-oracle-adapter-issues}

The following table describes known Oracle adapter issues and their resolutions.

| Issue | Cause | Resolution |
|-------|-------|------------|
| Oracle single instance database is not discovered after registering a Linux Oracle server | There is an incorrect `$ORACLE_HOME` directory path in the `/etc/oratab` file for the Oracle SID. | Verify that entries in `/etc/oratab` follow the correct format: `$ORACLE_SID:$ORACLE_HOME:<N\|Y>`. Example: `ORADB:/u01/app/oracle/product/11.2.0.4:N` |
| Oracle RMAN database recovery fails with `ORA-01180` and `ORA-01110` | This error occurs because of known Oracle issues. | In the UI, adjust the **Point in Time** slider to a later date to include more log files. For example, move the slider from August 19 to August 22. |
| Oracle Remote Adapter recovery job to an alternate location fails with `RMAN-05501: aborting duplication of target database` | The RMAN script includes the same `file_path` as the original server when recovering to the destination server. | Contact {{site.data.keyword.baas_full_notm}} support. As a workaround, add `NOFILENAMECHECK` to the RMAN script to ignore this error. |
| Oracle protection job fails with `ORA-09925: Unable to create audit trail file` | {{site.data.keyword.baas_full_notm}} creates a new session (and a new Oracle trace file) when the database backup progress query runs. This is expected Oracle behavior. | Review the Oracle database settings and determine whether `SQL_TRACING` is enabled. In addition, check whether `kcb` or `krb` tracing is enabled. |
| Oracle RMAN protection job configured to use source-side deduplication (SBT) fails in environments without IPv6 enabled | By default, the SBT library uses `sun_rpc` calls that communicate over IPv6. | Either enable IPv6 on the Oracle database servers, or contact {{site.data.keyword.baas_full_notm}} support to download a library that forces communication over IPv4. The IPv4 library requires additional SBT parameters (`gflag-name=sbt_use_grpc` and `gflag-value=true`) in the RMAN script. Example: `ALLOCATE CHANNEL c1 DEVICE TYPE SBT_TAPE PARMS 'SBT_LIBRARY=/home/oracle/libsbt_7_linux-x86_64-3.so, SBT_PARMS=(mount_path=myserver.mydomain.com:/MyView,mount_point=/mnt/backups,vips=$VIP,gflag-name=sbt_use_grpc,gflag-value=true)'` |
| Cloning an Oracle database fails with `ORA-65093: multitenant container database not set up properly` | Cloning Oracle databases with multitenant container database (CDB) enabled is not supported. | To request this capability, see the Cohesity knowledge base article *How to create and view Request for Enhancements*. |
| Oracle database protection job fails with `[kTimeout]: RPC call MountOracleNAS timed out` | The time required to set Oracle server permissions for the {{site.data.keyword.baas_full_notm}} user timed out, or there is a connectivity issue between the Oracle server and the {{site.data.keyword.baas_full_notm}} cluster. For example, bidirectional communication over port 2049 may not be open across network firewalls. | See the related knowledge base article for resolution steps. |
| Oracle protection job fails with `[kAgentError]: ORA-00312: online log 2 thread 1: ORA-19809: limit exceeded for recovery files` | This failure results from a server-side issue. The `db_recovery_file_dest_size` limit was exceeded. | To resolve `ORA-19809` and `ORA-19804`, take one or more of the following actions: (1) Back up the recovery area frequently using RMAN. (2) Change the RMAN retention policy. (3) Change the RMAN archivelog deletion policy. (4) Add disk space and increase `DB_RECOVERY_FILE_DEST_SIZE`. (5) Delete files from the recovery area using RMAN. |
| Oracle Remote Adapter protection job fails with `[kIOError]: [kAppError]: Dedup write failed with error: kQuotaExceeded` | A quota was set on the View used for the RMAN backup, and the View's quota nearly reached the limit. | Increase the quota assigned to the View and re-run the job. |
{: caption="Oracle adapter known issues and resolutions" caption-side="bottom"}
