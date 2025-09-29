---

copyright:
  years: 2024, 2025
lastupdated: "2025-08-27"

keywords: wildcard

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Use wildcard \* in search for recovery
{: #use_wildcard_*_in_search_for_recovery}


You can optionally use the Wildcard `*` in any location in the search string. The following table shows some example searches.

| **Example Search** | **Finds** | **Description** |
| --- | --- | --- |
| `*` | All the files, Protection Group Names, or Server names |     |
| `test*` | `vol1/usr/bin/test`<br><br>`vol1/usr/share/setup/test.ps` | Note that this search does not find: <br><br>`vol1/usr/memtest.ext` |
| `*test*` | `vol1/usr/bin/test`<br><br>`vol1/usr/share/setup/test.ps`<br><br>`vol1/usr/memtest.ext` |     |
| `*xml` | `usr/local/firefox/searchplugin/yahoo.xml` | When searching for file extensions, do not enter the period in the search string. For example if searching for all files that end in the `.txt` extension, enter `*txt` and not `*.txt`. |
| `search* *xml` | `usr/searchplugins/dsl.xml` | Enter `search* *xml` with a space and not `search*xml`. However, when you add spaces between search terms, only files that match all the search terms are found, for example searching with the string: `search* *xml` does not find: <br><br>`usr/searchplugins`<br><br>Entering just `search` does not find `usr/searchplugins/dsl.xml`. |
| `*plug* *xml` | `usr/searchplugins/dsl.xml` | Only files that match all the search terms are found. |
{: caption="" caption-side="bottom"}

Ensure to follow the standard regular expression rules. Specify a dot '.' before asterisk '\*' to match all characters, for example, L:.\*.ndf.
