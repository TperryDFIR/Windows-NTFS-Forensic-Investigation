# Windows Registry Artifact Analysis

## Objective

Examine Windows Registry artifacts to identify references to recently accessed files and correlate user activity with findings from the NTFS file system examination.

## Evidence Source and Tool

* **Windows user profile:** `mortysmith`
* **Registry hive:** `/Users/mortysmith/NTUSER.DAT`
* **Registry artifact:** `Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`
* **Analysis tool:** RegRipper, `recentdocs` plugin v.20200427
* **Saved analysis output:** `mortysmith_recentdocs.txt`

The registry hive was examined from the read-only mounted forensic image at `/mnt/lab8_ntfs/Users/mortysmith/NTUSER.DAT`.

A separate examination of `/Users/ricksanchez/NTUSER.DAT` did not return a matching reference to `My Social Security Number.txt` in its RecentDocs plugin output.


## RecentDocs Findings

The parent `RecentDocs` key contained references to the following items:

| Recorded item                   | Type          |
| ------------------------------- | ------------- |
| `Portal_gun.png`                | PNG image     |
| `Pictures`                      | Folder        |
| `Jessica.jpg`                   | JPEG image    |
| `My Social Security Number.txt` | Text document |
| `Plans.txt`                     | Text document |
| `Thoughts.txt`                  | Text document |

The parent key had a LastWrite timestamp of **2020-09-18 23:07:44 UTC**.

### Text Document Subkey

The `.txt` subkey contained the following most-recently-used order:

```text
MRUListEx = 2,1,0
  2 = My Social Security Number.txt
  1 = Plans.txt
  0 = Thoughts.txt
```

The `.txt` subkey had a LastWrite timestamp of **2020-09-18 22:50:36 UTC**.

The ordering places `My Social Security Number.txt` first among the text-document references recorded in this subkey. The LastWrite timestamp records modification of the registry key and should not be interpreted as the exact time the document was opened.

### Other Recent Item References

| Subkey   | LastWrite (UTC)     | Recorded item    |
| -------- | ------------------- | ---------------- |
| `.jpg`   | 2020-09-18 23:01:11 | `Jessica.jpg`    |
| `.png`   | 2020-09-18 23:07:44 | `Portal_gun.png` |
| `Folder` | 2020-09-18 23:01:11 | `Pictures`       |

## Correlation with NTFS Evidence

The filename `My Social Security Number.txt` was also identified in the NTFS file listing and correlated with **MFT record `91143`**.

The file was independently recovered from the NTFS partition using The Sleuth Kit `icat` utility with a partition starting sector of `239616`. A SHA-256 comparison and byte-for-byte comparison supported that the independent recovery matched the previously extracted file.

This correlation links a Windows Registry reference to a file-system artifact in the examined image.

## Interpretation and Limitations

The RecentDocs findings support that the examined Windows user profile retained references to these files and folders. They do not, by themselves, prove who physically accessed the workstation, the exact time a particular document was opened, or whether its contents were transmitted elsewhere.

The recovered text document contains potentially sensitive personal information. Its contents and extracted copies were retained outside the public GitHub repository.

## Conclusion

Analysis of the `RecentDocs` registry artifact identified recent-item references that could be correlated with NTFS metadata and file recovery results. Combining registry and file-system evidence provided a more complete account of activity represented in the acquired Windows image than either artifact source alone.
The relevant RecentDocs references were attributed to the mortysmith Windows user profile by examining its NTUSER.DAT hive and comparing the resulting RegRipper output with the previously saved analysis.
