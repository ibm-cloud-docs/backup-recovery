---

copyright:
  years: 2026
lastupdated: "2026-09-24"

keywords: source registration, protection group, 404 error, data source connection, SAP HANA recovery, known issues

subcollection: backup-recovery

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why does registering a source not work from the **Protection Group Creation and Recovery** page?
{: #troubleshoot-source-registration}
{: troubleshoot}

Currently, you have to register a source form the sources page located on the left navigation pane of the Backup and Recovery instances dashboard.
{: shortdesc}

# What happens if I get a 404 error for data source connections and the page shows an error that says "data source connection does not exist" and source registration also shows the same error?
{: #troubleshoot-datasource-404}
{: troubleshoot}

A 404 error appears for data source connections, and both the data source connection page and source registration show a "data source connection does not exist" message.
{: shortdesc}


# Why does the SAP Hana backup and recovery UI not allow a click to submit the job when searching for * protection groups?
{: #troubleshoot-saphana-wildcard}
{: troubleshoot}

You have to search for the database you are trying to restore by name, for example *dbname,* instead of * if you have both SAP hana and DB2 sources in the same instance.
{: shortdesc}


