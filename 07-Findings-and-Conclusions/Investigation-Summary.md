# Windows NTFS Forensic Investigation: Findings and Conclusions

## Investigation Overview

This case study documents a forensic examination of a Windows 10 disk image using Kali Linux, The Sleuth Kit, EWF utilities, and RegRipper. The examination focused on evidence integrity, disk partition structure, NTFS metadata, file recovery, and Windows Registry artifacts.

The analysis was conducted against a reconstructed EWF image exposed at `/mnt/ewf/ewf1`. The Windows NTFS volume was also examined through a read-only mount at `/mnt/lab8_ntfs`.

## Evidence Integrity

The examiner recorded original and post-analysis MD5, SHA-1, and SHA-256 hashes for the reconstructed image. Comparison of the two saved hash records using `diff -u` reported no differences.

A separate documentation review identified a one-character SHA-1 transcription discrepancy in `evidence_custody_log.txt`. The original custody log was retained, and the discrepancy was documented in `hash_verification_correction.txt`, dated September 19, 2026.

**Finding:** The saved original and post-analysis hash records match. The custody log’s SHA-1 transcription discrepancy was separately documented without altering the original record.

## Disk and Partition Analysis

The Sleuth Kit `mmls` utility identified a GUID Partition Table (GPT) with four allocated partitions:

| Partition | Starting sector | Description                                              |
| --------- | --------------: | -------------------------------------------------------- |
| 1         |           2,048 | EFI System partition                                     |
| 2         |         206,848 | Microsoft Reserved partition                             |
| 3         |         239,616 | Basic Data partition containing the examined NTFS volume |
| 4         |      30,418,944 | Unlabeled partition                                      |

The examined NTFS partition began at sector `239616`, using 512-byte sectors.

**Finding:** Partition analysis established the correct file-system offset for subsequent NTFS examination and file recovery.

## NTFS Artifact Identification and Recovery

An NTFS file listing identified `My Social Security Number.txt` at MFT record `91143`.

The Sleuth Kit `icat` utility was used to independently extract the file from the reconstructed EWF image:

```bash
sudo icat -o 239616 \
/mnt/ewf/ewf1 91143 \
> /home/forensics/Cases/Week3/Analysis/recovered_file_verification.txt
```

The independently recovered file was identified as ASCII text with CRLF line endings.

SHA-256 calculations for the independently recovered file and the previously saved extraction produced matching results. A byte-for-byte comparison using `cmp` reported no differences.

**Finding:** The independent recovery reproduced the previously extracted artifact and supported the repeatability of the recovery procedure.

## Windows Registry Analysis

RegRipper’s `recentdocs` plugin was used to examine the `NTUSER.DAT` hives associated with the Windows user profiles `mortysmith` and `ricksanchez`.

The `mortysmith` profile contained a RecentDocs reference to `My Social Security Number.txt`. The relevant `.txt` subkey recorded the following order:

```text
MRUListEx = 2,1,0
  2 = My Social Security Number.txt
  1 = Plans.txt
  0 = Thoughts.txt
```

The `.txt` subkey had a LastWrite timestamp of **2020-09-18 22:50:36 UTC**.

The examined `ricksanchez` RecentDocs output did not contain a matching reference to that filename.

**Finding:** A recent-item reference in the `mortysmith` Windows user profile was correlated with the file-system artifact identified at MFT record `91143`.

The registry evidence does not establish who physically used the workstation or the exact time the document was opened. The LastWrite timestamp reflects modification of the registry key, not necessarily the time of a specific file-access event.

## Evidence Correlation

| Evidence source             | Relevant result                                        |
| --------------------------- | ------------------------------------------------------ |
| GPT partition analysis      | NTFS partition begins at sector `239616`               |
| NTFS file listing           | Target document identified at MFT record `91143`       |
| File recovery               | `icat` extracted the identified file                   |
| File verification           | Saved extraction and independent recovery matched      |
| Morty Smith’s `NTUSER.DAT`  | RecentDocs reference to the same filename              |
| Rick Sanchez’s `NTUSER.DAT` | No matching filename in the examined RecentDocs output |
| Image integrity records     | Saved original and post-analysis hashes matched        |

Together, these findings demonstrate how file-system metadata, recovered content, and Windows Registry artifacts can be examined and correlated within a single forensic investigation.

## Scope and Limitations

This case study documents findings supported by the examined artifacts. It does not establish that a particular person created the recovered document, that its contents belong to a particular user, or that the document was transmitted outside the workstation.

The recovered document may contain sensitive personal information. Its contents, extracted copies, and the forensic disk image were excluded from the public GitHub repository.

## Conclusion

The examination documented a repeatable workflow for verifying evidence integrity, identifying a GPT/NTFS partition, locating and recovering an NTFS file, and correlating that file with a Windows user-profile registry artifact.

The investigation also demonstrated the importance of preserving original documentation and recording corrections transparently when a transcription discrepancy is identified.
