---

copyright:
  years: 2024, 2025
lastupdated: "2025-07-30"

keywords: <KEYWORDS>

subcollection: backup-recovery

---

{{site.data.keyword.attribute-definition-list}}

# Add exclusions
{: #add_exclusions}

12 September 2024

During File-based Physical Server Backups, by default, all files and directories under the parent directory specified in the Include path of a Protection Group are protected by the {{site.data.keyword.baas_full_notm}}. However, to exclude specific sub-directories and files in the Include path, the Exclude path can be specified in the Protection Group.

Furthermore, Wildcards can be used in the Exclude paths to advance and maximize the exclusion behavior. {{site.data.keyword.baas_full_notm}} by default supports only one wild character per Exclude path and usage of both characters in a single exclude path is not supported. IBM Cloud Supports the below wildcards in the Exclude path only. Wildcards are not supported for the Include paths.


| Character | Description and Usage |
| --- | --- |
| Asterisk (\*) | *   Represents any number of characters<br>    <br>*   Used for both directories and file names<br>    <br>*   Can be used anywhere in the string<br>    <br>*   The scope is across directories and their associated sub-directories and files in the Include path |
| Question mark (?) | *   Used for excluding files with suffix<br>    <br>*   Represents any number of characters in the filename<br>    <br>*   Must be preceded by a valid directory path<br>    <br>*   Must be followed by a suffix<br>    <br>*   The scope is limited to the specified directory and the contents of sub-directories are not considered. |
{: caption="" caption-side="bottom"}

Apart from the above characters, all other special symbols in the directory or file name are matched as exact text. And if any of the above characters \[i.e. Asterisk (\*) or Question mark (?)\] are in the directory or file name, then the backslash ( \\ ) escape character can be used to match the characters \[i.e Asterisk (\*) or Question mark (?)\].

**Examples**

Some examples of wildcard usage in the Exclude path on the **Windows** server:

Windows excludes are not case sensitive.


| Requirement | Configuration | Included | Excluded |
| --- | --- | --- | --- |
| Windows excludes are not case-sensitive | **Include Path**: C:\\temp<br><br>**Exclude Path**:<br><br>C:\\temp\\\*abc | C:\\temp\\ABCFOLDER<br><br>C:\\temp\\aBCFile<br><br>C:\\temp\\dir1\\ABCFOLDER<br><br>C:\\temp\\dir1\\ABCFile | C:\\temp\\FOLDERABC<br><br>C:\\temp\\FILEabc<br><br>C:\\temp\\dir1\\FOLDERABC<br><br>C:\\temp\\dir1\\FILEabc |
| Exclude Subfolder:<br><br>Backup C:\\temp but exclude C:\\temp\\folder2 | **Include Path**: C:\\temp<br><br>**Exclude Path**: C:\\temp\\folder2 | C:\\temp\\folder1<br><br>C:\\temp\\file1.txt<br><br>C:\\temp\\file1.rtf | C:\\temp\\folder2 |
| Exclude a specific file:<br><br>C:\\temp\\folder2\\file1.txt | **Include Path**: C:\\temp<br><br>**Exclude Path**: C:\\temp\\folder2\\file1.txt | C:\\temp<br><br>C:\\temp\\folder1<br><br>C:\\temp\\folder2\\file2.txt<br><br>C:\\temp\\file1.txt<br><br>C:\\temp\\file2.txt | C:\\temp\\folder2\\file1.txt |
| Exclude a specific file anywhere in child's path | **Include Path**: C:\\temp<br><br>**Exclude Path**: \*file\_exclude.txt | C:\\temp<br><br>(& all its child files/folder) | C:\\temp\\file\_exclude.txt<br><br>C:\\temp\\folder1\\file\_exclude.txt<br><br>C:\\temp\\folder2\\file\_exclude.txt<br><br>(anywhere in C:\\temp) |
| Exclude a file type anywhere in the child path | **Include Path**: C:\\temp<br><br>**Exclude Path**: \*.txt | C:\\temp<br><br>(& all its child files/folders) | C:\\temp\\file.txt<br><br>C:\\temp\\folder1\\file.txt<br><br>C:\\temp\\folder2\\file.txt |
| Exclude all directories starting with excdir at the specified path | **Include Path**: C:\\temp<br><br>**Exclude Path**: C:\\temp\\folder1\\excdir\* | C:\\temp\\excdirectory<br><br>C:\\temp\\excdir1<br><br>C:\\temp\\folder2\\excdirectory | C:\\temp\\folder1\\excdirectory<br><br>C:\\temp\\folder1\\excdir1 |
| Exclude all files/directories ending with a specified string anywhere in the child path | **Include Path** : C:\\temp<br><br>**Exclude Path**: C:\\temp\\\*win | C:\\temp\\windows<br><br>C:\\temp\\winner<br><br>C:\\temp\\win.png | C:\\temp\\folderwin<br><br>C:\\temp\\win<br><br>C:\\temp\\folder1\\folderwin<br><br>C:\\temp\\folder1\\win<br><br>C:\\temp\\folder2\\folderwin<br><br>C:\\temp\\folder2\\win |
| Exclude paths with specific prefixes and suffix in filenames | **Include Path** : C:\\temp<br><br>**Exclude Path**: <br><br>C:\\temp\\debug\*.gz<br><br>C:\\temp\\log\*2022 | C:\\temp\\log\_2020<br><br>C:\\temp\\debug.gzip<br><br>C:\\temp\\log2022a | C:\\temp\\log\_collection\_2022<br><br>C:\\temp\\log\\files\_2022<br><br>C:\\temp\\debuglogs.gz<br><br>C:\\temp\\debuglogs.tar.gz<br><br>C:\\temp\\debug\\log.tar.gz |
| Exclude all files/directories with either prefix or suffix | **Include Path**: C:\\temp<br><br>**Exclude Path**: C:\\temp\\\*exclude<br><br>C:\\temp\\exclude\* | C:\\temp\\excdiretory<br><br>C:\\temp\\excdri1<br><br>C:\\temp\\imageexcludes<br><br>C:\\temp\\file\_exclude.txt<br><br>C:\\temp\\2022\_excludefiles | C:\\temp\\directoryexclud<br><br>C:\\temp\\excludedirectory<br><br>C:\\temp\\excludefile.txt<br><br>C:\\temp\\folder1\\direxclude |
| Exclude all .txt files only in C:\\temp directory and not child directory | **Include Path**: C:\\temp<br><br>**Exclude Path**: C:\\temp\\?.txt | C:\\temp\\file.exe<br><br>C:\\temp\\folder1\\file.txt<br><br>C:\\temp\\folder1\\abc.txt | C:\\temp\\file1.txt<br><br>C:\\temp\\file2.txtC:\\temp\\file12.txt |
| Exclude a file with specified file name anywhere in the path. | **Include Path**: C:\\temp<br><br>**Exclude Path**: C:\\temp\\\*file1.txt | C:\\temp\\file01.txt<br><br>C:\\temp\\file01.dat<br><br>C:\\temp\\myfile1.txt<br><br>C:\\temp\\folder1\\file1.dat | C:\\temp\\file1.txt<br><br>C:\\temp\\folder1\\file1.txt<br><br>C:\\temp\\folder2\\myfile1.txt |
{: caption="" caption-side="bottom"}

Some examples of wildcard usage in the Exclude path on the **Linux** server:

Linux excludes are case-sensitive.


| Requirement | Configuration | Included | Excluded |
| --- | --- | --- | --- |
| Linux excludes are case-sensitive | **Include Path**: /home/cohesity/temp<br><br>**Exclude Path**:<br><br>/home/cohesity/temp/\*abc | /home/cohesity/temp/123ABC<br><br>/home/cohesity/temp/123abC<br><br>/home/cohesity/temp/dri1/123ABC<br><br>/home/cohesity/temp/dir1/123abC<br><br>/home/cohesity/temp/abc.bin<br><br>/home/cohesity/temp/dir1/abc.bin<br><br>/home/cohesity/temp/dir1/abc.Bin | /home/cohesity/temp/abc<br><br>/home/cohesity/temp/123abc<br><br>/home/cohesity/temp/dir1/123abc<br><br>/home/cohesity/temp/dir1/dir2/abc |
| Backup /var and but exclude /var/log | **Include Path**: /var<br><br>**Exclude Path**: /var/log | /var/cache<br><br>/var/db<br><br>/var/lib<br><br>/var/mail<br><br>/var/spool<br><br>etc. | /var/log |
| Exclude a specific file: /var/log/cohesity/debug.log | **Include Path**: /<br><br>**Exclude Path**: /var/log/cohesity/debug.log | / <br><br>(& all its child files/directories) | /var/log/cohesity/debug.log |
| Exclude a specific file anywhere in child's path | **Include Path**: /var<br><br>**Exclude Path**: debug.log | /var<br><br>(& all its child files/directories) | /var/log/cohesity/debug.log <br><br>/var/mail/debug.log<br><br>/var/opt/debug.log<br><br>(anywhere in /var) |
| Exclude a file type anywhere in the child path | **Include Path**: /var<br><br>**Exclude Path**: \*.log | /var<br><br>(& all its child files/directories) | All files with extension .log anywhere inside /var directory |
| Exclude all files/directories starting with foo at the specified path | **Include Path**: /usr<br><br>**Exclude Path**: /usr/foo\* | /usr/bar\_foo<br><br>/usr/bin<br><br>/usr/etc<br><br>/usr/lib<br><br>/usr/local/foo<br><br>/usr/share/foo<br><br>etc. | /usr/foo<br><br>/usr/foo1<br><br>/usr/foo\_bin<br><br>/usr/foobar |
| Exclude all files/directories ending with a specified string anywhere in the child path | **Include Path**: /usr<br><br>**Exclude Path**: /usr/\*foo | /usr/bin<br><br>/usr/etc<br><br>/usr/foo1<br><br>/usr/foobar<br><br>/usr/foo\_bin | /usr/bar\_foo<br><br>/usr/foo<br><br>/usr/local/foo<br><br>/usr/share/foo |
| Exclude paths with specific prefix and suffix in filenames | **Include Path**: /var/<br><br>**Exclude Path**: /var/log/debug\*.gz<br><br>/var/log\*2022 | /var/cache<br><br>/var/db<br><br>/var/lib<br><br>/var/log\_012021<br><br>/var/mail<br><br>/var/spool<br><br>etc. | /var/log/debug.gz<br><br>/var/log/debug.tar.gz<br><br>/var/log/debug-agent-logs.tar.gz<br><br>/var/log/debug-agent-08-2021.tar.gz<br><br>/var/log\_012022/<br><br>/var/log\_app\_2022/ |
| Exclude all files/directories with either prefix or suffix match | **Include Path**: /usr<br><br>**Exclude Path**:<br><br>/usr/\*foo<br><br>/usr/foo\* | /usr/bin<br><br>/usr/etc<br><br>/usr/barfoobar<br><br>/usr/lib<br><br>etc. | /usr/bar\_foo<br><br>/usr/foo<br><br>/usr/foo\_bin<br><br>/usr/foo1<br><br>/usr/foobar<br><br>/usr/local/foo<br><br>/usr/share/foo |
| Exclude all .txt files only in /data directory and not child directories | **Include Path**: /data<br><br>**Exclude Path**: /data/?.txt | /data/archive.tar.gz<br><br>/data/bar/list.txt<br><br>/data/db<br><br>/data/etc<br><br>/data/foo/mydata.txt<br><br>/data/script.sh | /data/a.txt<br><br>/data/one.txt<br><br>/data/foo.txt<br><br>/data/bar.txt |
{: caption="" caption-side="bottom"}
