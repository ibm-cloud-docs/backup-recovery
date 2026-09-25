---

copyright:
  years: 2026

lastupdated: "2026-09-25"

keywords: Oracle adapter prerequisites, Oracle database requirements, Oracle RAC, Oracle single instance, Oracle ASM, archive log mode, DBID, SCAN VIP, SBT library, VCS deployment

subcollection: backup-recovery

content-type: prereq

---

{{site.data.keyword.attribute-definition-list}}

# Prerequisites for Oracle adapter
{: #prerequisites-oracle-adapter}

Make sure the following prerequisites are met before using the {{site.data.keyword.baas_full_notm}} Oracle Adapter.
{: shortdesc}

- **DBIDs**: All Oracle databases that are protected by the {{site.data.keyword.baas_full_notm}} cluster must have a unique DBID on the Oracle source where the databases reside.

- **SCAN VIP (Linux and AIX only)**: To register Oracle RAC, you must use SCAN-NAME or SCAN-VIP. Otherwise, registration, backup, and recovery of Oracle databases will fail.

- **Authentication**: For database-level authentication, the TNS listener service name must be the same as the database name.

- **Archive Log Mode**: Archive Log mode must be enabled for any database opened in Read-Write mode. Otherwise, the backup task will fail.

- **Version**: The recovery source and target database must be the same version of Oracle Database. For example, snapshots of an 11g Oracle Database cannot be recovered to a 12c Oracle Database.

- **Full backup**: {{site.data.keyword.baas_full_notm}} recommends taking a full database backup after any activity that changes the control file, such as adding or removing a datafile, restoring to the original location with overwrite, or changing the log file location.

- **Oracle single instance deployment**:
   - For an Oracle single instance database, the database must be entered in the `/etc/oratab` file on Linux and AIX, and in the `/var/opt/oracle/oratab` file on Solaris. Otherwise, {{site.data.keyword.baas_full_notm}} cannot discover the database.
   - For an Oracle single instance database running on Windows, the `OracleService${ORACLE_SID}` service must be running.

- **VCS deployment (Linux and AIX only)**: For Veritas Cluster Server (VCS) deployments, the Oracle database environment must be configured in `/etc/oratab` on all VCS nodes.

- **Register Oracle RAC**: To register an Oracle RAC or a RAC node as a physical server, the `host` command must be executed on each of the nodes of that RAC.

   The `noac` flag for NFS mounts must be set for Oracle RAC versions 11gR2 and 12cR1. If not set, Oracle RAC backup fails.
   {: important}

- **Oracle RAC database (Linux and AIX only)**:
   - The {{site.data.keyword.baas_full_notm}} Linux Agent must be installed on all Oracle servers that are part of an Oracle RAC.
   - The `GRID_HOME` must be specified in the `/etc/oratab` or `/etc/oraInst.loc` file.

- **DB authentication**: If you use database authentication, all databases on the system must share the same username and password.

- **SBT library (Windows only)**: On the Windows Oracle server, install the SBT library in the same directory where the Windows Agent is installed. By default, the agent is installed in the `c:\Program Files\IBM\oracle` directory. Contact IBM Support to obtain the SBT library installer.

## Oracle alternate restore to ASM disk group
{: #prerequisites-oracle-adapter-asm}

For Oracle alternate restore to an ASM disk group, verify the following prerequisites before starting restores on the {{site.data.keyword.baas_full_notm}} cluster.

### ASM compatibility (COMPATIBLE.RDBMS)
{: #prerequisites-oracle-adapter-asm-compatibility}

The disk group attributes that determine compatibility are `COMPATIBLE.ASM`, `COMPATIBLE.RDBMS`, and `COMPATIBLE.ADVM`. The `COMPATIBLE.ASM` and `COMPATIBLE.RDBMS` attribute settings determine the minimum Oracle Database software version numbers that a system can use for Oracle ASM and the database instance types respectively.

The value of the `COMPATIBLE.RDBMS` attribute determines the minimum `COMPATIBLE` database initialization parameter setting for any database instance that is allowed to use the disk group. Before advancing the `COMPATIBLE.RDBMS` attribute, ensure that the values for the `COMPATIBLE` initialization parameter for all databases that access the disk group are set to at least the value of the new setting for `COMPATIBLE.RDBMS`.

For example, if the `COMPATIBLE` initialization parameter of the databases is set to `12.2`, then `COMPATIBLE.RDBMS` can be set to any value between `10.1` and `12.2` inclusively. If the `COMPATIBLE` initialization parameter is set to `18.0`, then `COMPATIBLE.RDBMS` can be set to any value between `10.1` and `18.0` inclusively.

To check the existing compatibility for an ASM disk group, run the following query:

```sql
select name, DATABASE_COMPATIBILITY, COMPATIBILITY from v$asm_diskgroup;
```
{: codeblock}

The following table shows examples of disk group compatibility attribute settings:

| COMPATIBLE.ASM | COMPATIBLE.RDBMS | COMPATIBLE.ADVM |
|----------------|-----------------|----------------|
| 11.2.0.2 | 10.1 | 11.2.0.2 |
| 12.2 | 12.2 | 12.2 |
| 18.0 | 12.2 | 18.0 |
| 19.0 | 18.0 | 19.0 |
{: caption="Examples of disk group compatibility attribute settings" caption-side="bottom"}

### Oracle software owner and ASM management owner
{: #prerequisites-oracle-adapter-asm-owner}

If the Oracle Grid or Oracle Restart owner is different from the Oracle RDBMS owner, make sure that the Oracle Grid OS group is added as a secondary group for the Oracle RDBMS owner.

For example:

| Component | OS user | OS group |
|-----------|---------|----------|
| Oracle Grid / Oracle Restart | `oraasm` | `asmdba`, `asmadmin` |
| Oracle RDBMS | `oracle` | `oinstall`, `dba` |
{: caption="Example Oracle software owner and ASM management owner" caption-side="bottom"}

In this case, add `asmdba` as a secondary group to the `oracle` user.
