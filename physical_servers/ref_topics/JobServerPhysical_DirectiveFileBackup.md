# Directive File Backup

12 September 2024

The directive file defines the locations of the files and folders that you can back up in a protection group. Specify the file path of the directive file (meta-file) on the source while adding it to a protection group.

Directive files must use ASCII encoding (as opposed to UTF or Unicode). Many text editors default to UTF or Unicode, so select ASCII as the text document encoding manually.

The directive file is a text file in which every entry should start on a new line. It should be of one of the following types:

1.  **Path without any wildcards**: The fully qualified paths of the files and folders from the root directory in the case of a Linux or UNIX host, or a Windows directory path in the case of Windows that you can back up in a protection group.

2.  **Path with supported wildcards**: It is a combination of a fully qualified [File Path](#Defining) and supported wildcards (?,\*) in the [Directory Path](#Defining).


A path with wildcards will only match files and not folders (or directories).

You should create this file, place it on the source machine, and then grant read-write access to the {{site.data.keyword.baas_full_notm}} will parse it during the protection group run. Each physical server should have its own directive file.

While creating the protection group, select the ‘**Use directive file for backup**’ checkbox and specify the file path of the directive file in the text box. {{site.data.keyword.baas_full_notm}} will treat this as a directive file and parse every entry within the file to back up data from all the defined locations as specified in it. For any reason, if nothing is found in any of the specified paths, the protection group skips that entry and proceeds with the remaining entries within the file.

### Success file list

After the protection group is complete, the system generates a success file list (an <input metafile>-out.txt log file) that contains the entries of all the successfully backed-up files. You can find the location of this file in the pulse logs after the backup run.

For each entry in the directive file, the corresponding entries in the success file list will be as follows:

Path without wildcards

If we are able to successfully backup the entry (file or folder) then this entry will be added to the success file as is.

Path with wildcards

We will populate the dirname of the wildcard path (even if there are no matching files) followed by the files that match the wildcard path and have been successfully backed up.

Example

For the wildcard path `/home/user/documents/*.txt` in the directive file and the directory structure.

/home/user/downloads/image.jpg
/home/user/documents/report.txt
/home/user/documents/file1.txt
/home/user/documents/project.doc
/home/user/documents/index.html
/home/user/documents/directory/file1.txt

Corresponding success file list entries will be

/home/user/documents
/home/user/documents/report.txt
/home/user/documents/file1.txt

Local exclusions are not supported for directive file-based backups.

### Wildcard Support for Directive File Backups

Managing entries for directive file backups can be made easier by utilizing specific wildcards. These wildcards are outlined below, and it should be noted that they will only match files within the last directory listed in a single path entry. Any sub-directories beneath that level will not be processed. However, they can be specified explicitly, if needed.


| Character | Description and Usage |
| --- | --- |
| Asterisk (\*) | *   Represents any number of characters<br>    <br><br>*   Only applicable to files |
| Question mark (?) | *   Represents one character<br>    <br><br>*   Only applicable to files |

Apart from the above characters, all other special symbols in the directory or file name are matched as exact text. And if either of the above characters \[that is, asterisk (\*) or question mark (?)\] are in the directory or file name, then the backslash ( \\ ) escape character can be used to match the characters \[asterisk (\*) or question mark (?)\].

Examples

The following is an example of a directive file. Every path in the file should start on a new line.

These examples contain fully qualified paths for folders and files on the source. Relative paths are not supported in a directive file.

Entries in Windows directive file

*   C:\\Users


*   D:\\Departments\\Finance


*   D:\\Departments\\HR?


*   G:\\Misc\\department\_list.txt


*   G:\\Misc\_2\\\*.txt


Entries in Linux or UNIX Directive file

*   /home/cohesity/meta\_file.txt

*   /home/cohesity/\*.txt

*   /home/cohesity/file?

*   /home/cohesity/backup\_folder


This table lists the examples of directive file backup (with and without wildcard support).


| Index | Directive file contents | Directory Structure | What is included for backup |
| --- | --- | --- | --- |
| 1   | /A/B/file1<br><br>/A/B/dir1<br><br>/A/B/info.log | /A/B/file1/A/B/file2/A/B/file10<br><br>/A/B/dir1<br><br>/A/B/dir1/dfile1<br><br>/A/B/dir1/dfile2/A/B/info.log/A/B/error.log | /A/B/file1<br><br>/A/B/dir1<br><br>/A/B/dir1/dfile1<br><br>/A/B/dir1/dfile2<br><br>/A/B/info.log |
| 2   | /A/B/file?/A/B/\*.log | /A/B/file1/A/B/file2/A/B/file10<br><br>/A/B/dir1<br><br>/A/B/dir1/dfile1<br><br>/A/B/dir1/dfile2/A/B/info.log/A/B/error.log | /A/B/file1 (matches /A/B/file?)/A/B/file2 (matches /A/B/file?)/A/B/info.log (matches /A/B/\*.log)/A/B/error.log (matches /A/B/\*.log) |
| 3   | /A/B/????1/A/B/\*f\* | /A/B/file1/A/B/file2/A/B/file10<br><br>/A/B/dir1<br><br>/A/B/dir1/dfile1<br><br>/A/B/dir1/dfile2/A/B/info.log/A/B/error.log | /A/B/file1 (matches both /A/B/????1 and /A/B/\*f\*)/A/B/file2 (matches /A/B/\*f\*)/A/B/file10 (matches /A/B/\*f\*)/A/B/info.log (matches /A/B/\*f\*) |
| 4   | /A/\*/dir1<br><br>/A/B/file1\*/A/B/error.log | /A/B/file1/A/B/file2/A/B/file10<br><br>/A/B/dir1<br><br>/A/B/dir1/dfile1<br><br>/A/B/dir1/dfile2/A/B/info.log/A/B/error.log | /A/B/file1 (matches /A/B/file1\*)/A/B/file10 (matches /A/B/file1\*)<br><br>/A/B/error.log (matches /A/B/error.log)The entry /A/\*/dir1 will be ignored as this is not a valid wildcard\_path |
| 5   | /A/B/\* | /A/B/C/file1<br><br>/A/B/C/D/file2<br><br>/A/B/file1<br><br>/A/B/file2 | /A/B/file1( matches /A/B/\*)<br><br>/A/B/file2( matches /A/B/\*)we will backup only the files inside and not directories. <br><br>So /A/B/C is excluded even though it matches the entry /A/B/\* |

### Defining the path for Wildcard Inclusions for Directive File-based Backups (File Path and Directory Path)

In a file path, directories are separated by directory separators (such as "/" or "\\"). The dirname forms the path leading from the root directory or the base of the drive to the file or directory identified by the basename.

For example, in the file path `/home/user/documents/report.txt`

*   The dirname is `/home/user/documents`


*   The basename is `report.txt`


In the file path `C:\Users\John\Documents\project\file.doc`

*   The dirname is `C:\Users\John\Documents\project`


*   The basename is `file.doc`


In the file path `/var/www/html/index.html`

*   The dirname is `/var/www/html`


*   The basename is `index.html`
