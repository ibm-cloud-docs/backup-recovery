---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: microsoft sql, protection group, snapshot, clone database, restore database, migrate database

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Microsoft sql server dashboard
{: #microsoft_sql_server_dashboard}

Access the MS SQL Dashboard by going to **Dashboard** and from the drop-down, select **MS SQL**.

The page's upper portion displays information such as metrics, status and recovery history. In the page's lower portion, you can start the different actions from the Hosts tab and the Databases tab. The MS SQL Dashboard displays local MS SQL databases only. To restore or clone replicated or CloudRetrieved databases, use their respective restore or clone workflows.

From the Hosts tab, mouse over the actions menu for the following actions (some actions are available for physical servers only):

*   **Protect Databases** — Create a Protection Group using this server.
*   **Unregister MS SQL** — Unregister the MS SQL application. The server remains registered with the {{site.data.keyword.baas_full}}.
*   **Edit** — Configure the settings of a registered MS SQL server.
*   **Unregister** — Unregister this server from the {{site.data.keyword.baas_full_notm}}.
*   **Refresh** — Manually force a refresh of the object hierarchy.
*   **Upgrade Agents** — Upgrade the Backup agent on the server if an upgrade is available.

From the Databases tab, mouse over the actions menu for the following actions (some actions are not available for system databases):

*   **Protect** — Create a Protection Group using this database.
*   **Run Now** — Start a run of this Protection Group. You have the option to back up one or more objects.

    If the policy associated with the Protection Group is configured with replication and archive targets, you can select those targets. If the policy supports multiple backup types, you can select an incremental, full or log backup.

    VM MS SQL Protection Groups do not support selecting individual databases for run now; only the entire VM is selectable.

*   **Pause Future Runs/Resume Future Runs**
    *   Select **Pause Future Runs** to stop new runs of this Protection Group from executing. However, if the job is currently executing a run, the current run continues to execute and only future runs of this job are paused.
    *   Select **Resume Future Runs** to restore the Protection Group to a running state. The runs resume as specified in the schedule defined in the policy associated with this job.
*   **Cancel this Protection Group Run** — Cancel the current run of this Protection Group. (This option is only available when a Protection Group run is executing.) This cancel only affects the current run of the Protection Group and not future runs.
*   **Clone** — Make a copy of the database. The clone database is a unique copy of the original database taken from the backup.
*   Follow the instructions provided in [Clone Microsoft SQL Server Databases](/docs/allowlist/backup-recovery?topic=backup-recovery-clone_microsoft_sql_server_databases).
*   **Recover** — Restore a database. Follow the instructions provided in [Recover Microsoft SQL Server](/docs/allowlist/backup-recovery?topic=backup-recovery-recover_microsoft_sql_server).
*   **Migrate** — Migrate a database. Follow the instructions provided in [Migrate Microsoft SQL Server Databases](/docs/allowlist/backup-recovery?topic=backup-recovery-migrate_microsoft_sql_server_databases).

## Database recover and clone actions
{: #database_recover_and_clone_actions}

In the Databases tab click a protected database to display available recover and clone actions. To recover or clone the latest snapshot, click the arrow next to the **Protect** button in the top right corner and select an action. By default, the latest snapshot is selected. You can optionally choose to restore from a different snapshot by clicking **Edit**.

You can also provide a specific time to restore to if transaction logs are available. Click a date on the calendar to select a snapshot from that date. The default point selected will be the latest log transaction or the latest full or incremental, whichever is the latest. The slider also displays available points in time and displays snapshots as circles. Manually select a point on the slider if you want to restore to a specific point in time. Selecting an invalid time from the slider automatically changes to the closest available snapshot. If your manual selection is within 10 minutes of a snapshot, your selection automatically snaps to it. You can also select a specific point in time by entering it in the time fields.

After selecting the desired recover point, click **Recover**. You can choose to clone the selected recover point by clicking the arrow adjacent to recover and selecting **Clone**.

In this workflow, deleted snapshots may be displayed as valid recover points for a brief period until they are removed from the search index by a background process.
