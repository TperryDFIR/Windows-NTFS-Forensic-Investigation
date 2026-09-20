# Windows Artifact Identification and File Recovery

## Investigation Overview

This examination focused on identifying Windows user activity and correlating Registry artifacts with NTFS file system records.

The investigation was conducted using Kali Linux, RegRipper, and The Sleuth Kit.

## Windows RecentDocs Analysis

The NTUSER.DAT Registry hive was examined using the RegRipper recentdocs plugin.

The following Registry location was analyzed:

`Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`

The analysis identified several recently recorded files, including:

- Portal_gun.png
- Jessica.jpg
- My Social Security Number.txt
- Plans.txt
- Thoughts.txt

### Relevant Registry Timestamps

| Registry Key | LastWrite (UTC) |
|---|---|
| RecentDocs | 2020-09-18 23:07:44 |
| RecentDocs\\.txt | 2020-09-18 22:50:36 |

These timestamps describe modifications to the Registry keys. They should not be interpreted as exact file-opening timestamps.

## NTFS File System Correlation

The NTFS file listing was examined to locate a corresponding file system record.

The examination identified the relevant file with the following metadata:

| Attribute | Finding |
|---|---|
| File | My Social Security Number.txt |
| MFT Record | 91143 |
| Attribute Type | 128 ($DATA) |
| Attribute ID | 1 |
| File Type | Regular file |

The file was identified in the NTFS listing using the following command:

```bash
grep -in -C 3 "My Social Security Number.txt" ntfs_full_listing.txt
```

The matching record provided a file system reference that could be used for further forensic examination.

## File System Enumeration

The saved file analysis summary reported:

| Category | Count |
|---|---:|
| Total files | 100697 |
| Executables | 2748 |

These counts describe the results recorded during the examination of partition 3.

## Forensic Significance

The investigation demonstrated how Windows Registry artifacts can be correlated with NTFS metadata to identify files associated with recorded user activity.

RecentDocs provided evidence that Windows maintained a recent-document reference to the relevant file.

The NTFS listing independently identified a corresponding file system record.

Together, these artifacts support the reconstruction of user activity and help identify files for further forensic examination.

## Evidence Protection

The recovered file's contents have been excluded from this public report to prevent disclosure of potentially sensitive personal information.

Only the investigative methodology and relevant technical metadata are documented.
