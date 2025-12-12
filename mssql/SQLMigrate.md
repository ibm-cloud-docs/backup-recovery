---

copyright:
  years: 2024, 2025
lastupdated: "2025-12-12"

keywords: microsoft sql, snapshot, migration

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Migrate microsoft sql server databases
{: #migrate_microsoft_sql_server_databases}

Before you migrate an MS SQL database, ensure that a file-based snapshot exists on the cluster.

## Migrate ms�sql�database
{: #migrate_ms�sql�database}

This section lists the steps to migrate an MS SQL database.

1. Select **Dashboard** and from the drop-down, select **MS SQL**.
2. Select **Migrate** from the database's actions menu.

    You can also start a restore task by navigating to **Data Protection > Recoveries > Recover > Migrate Database (MS SQL only)**.

3. Search for an MS SQL server or database. Enter characters of the name and a list of items displays. You can optionally [specify the wildcard character \*](/docs/backup-recovery?topic=backup-recovery-use_wildcard_*_in_search_for_recovery&interface=ui). You can also optionally narrow the search results by specifying filter criteria, for example, you can filter the search results by a specific Protection Group. Click the **Add Filters** icon, specify the filter and click **Add**.
4. Select a database and set the migration options for the MS SQL database you want to migrate.

    1. **Task Name:** Optionally change the default name for the task.
    2. **Objects Being Recovered**: This displays the database to be migrated.

    3. **Sync Options**: Specify the sync options for the database to be migrated.

        Transaction log backup is not synced. Only the incremental and full backup is synced.

        *   **Auto Sync Databases**: This option automatically keeps the database in sync with the latest available snapshot until you finalize the database and bring it online.

        *   **Manually Sync Databases**: Once the initial database migration is completed, you can sync the migrated database files with newer snapshots available until you finalize the database and bring it online.

    4. **Recover Point:** This displays the selected recover point for the migration.
    5. **Settings:** Specify the **SQL Host** and **SQL Instance** to migrate the database to.

        Provide the full path location to restore all database files.

        You can optionally restore data files and log files to different locations. Click **Add Alternative Log File Locations and Custom Rules** to provide locations for data and log files and any custom rules. For data files, you can provide a full path or pattern for the original files and a location to restore to. When providing a location, use the standard path format. For example, use C:\\path for Windows instead of the extended path format \\\\?\\c:\\path.

        *   Wildcard character used for file matching such as \*
            *   _\*.ndf_ - All ndf files. Useful if you want every ndf file to go to the same folder
            *   _m:\\\*.ndf_ - Every ndf file on drive m:
            *   _c:\\\*1.ndf_ - Every ndf file on drive c: with a suffix of 1.ndf
        *   Regex pattern


        *   _.\*\\.ndf_ - All files with the ndf extension
        *   _^.\*foo1.\*\\.ndf$_ - All files with the word foo1 and the extension ndf

        *   Full path to a source database file

        Database files that have non-default file extensions or that do not match a specified filename or pattern are restored to the data file restore location.

        Click **Add Another Custom Rule** to add multiple custom rules.

        Ensure to follow the standard regular expression rules. Specify a dot '.' before asterisk '\*' to match all characters, for example, L:.\*.ndf.

5. By default, an MS SQL restore WITH RECOVERY is performed. You can optionally toggle this off to perform a restore WITH NO RECOVERY.
6. Click **Start Migration**.

    The initial file copy task starts. This is the first step in the migration process.

7. After the file copy task completes, the options available for the next migration step are displayed.

    *   **Finalize** - Available if the target database is synchronized to the latest snapshot.
    *   **Sync** - Available if there is a snapshot that is more recent than the target database.

    If you select the **Auto Sync Databases** option in Step 4, the **Auto Sync to latest** toggle is enabled by default. You can manually disable this option if required.


Database migration is not supported for AG databases.

## Cancel migration
{: #cancel_migration}

You can cancel an in-progress migration task. Canceling the migration prevents syncing or finalizing the database and may leave it in an unusable state. Delete the database files manually and then start another migration task as needed.

1. In the {{site.data.keyword.baas_full}} Console, select **Data Protection > Recoveries**.
2. Find the migration task in the list and click the task name to display its detail page.
3. Click the **Cancel** button located at the top right of the page.
4. Respond to the confirmation prompt.
