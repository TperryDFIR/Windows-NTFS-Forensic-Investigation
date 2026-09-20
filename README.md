# Windows NTFS Forensic Investigation

## Project Overview

This case study documents a forensic examination of a Windows 10 Expert Witness Format (EWF/E01) disk image using Kali Linux, The Sleuth Kit, EWF utilities, RegRipper, and FTK Imager.

The investigation demonstrates evidence integrity verification, GPT partition analysis, NTFS file-system examination, file recovery, Windows Registry analysis, and correlation of forensic artifacts.

## Investigation Objectives

* Preserve and verify the integrity of digital evidence.
* Identify disk partitions and examine NTFS structures.
* Locate and recover a file using NTFS metadata.
* Analyze Windows Registry artifacts associated with recent file activity.
* Correlate registry findings with file-system evidence.
* Document findings, limitations, and evidence-handling procedures.

## Tools and Technologies

| Tool                              | Investigative purpose                                   |
| --------------------------------- | ------------------------------------------------------- |
| Kali Linux                        | Forensic examination environment                        |
| EWF utilities                     | Access to the forensic disk image                       |
| The Sleuth Kit                    | Partition analysis, NTFS examination, and file recovery |
| RegRipper                         | Windows Registry artifact analysis                      |
| FTK Imager                        | Forensic image and file-system examination              |
| SHA-256, SHA-1, and MD5 utilities | Evidence integrity verification                         |

## Investigation Documentation

| Section                                                                             | Description                                         |
| ----------------------------------------------------------------------------------- | --------------------------------------------------- |
| [Evidence Acquisition and Integrity](02-Evidence-Acquisition/Evidence-Integrity.md) | Original and post-analysis image hash verification  |
| [NTFS Partition Analysis](03-Partition-Analysis/NTFS-Partition-Analysis.md)         | GPT partition structure and NTFS examination        |
| [Artifact Recovery](05-Artifact-Recovery/Artifact-Recovery-Analysis.md)             | File identification, recovery, and verification     |
| [Windows Registry Analysis](06-Registry-Analysis/Registry-Artifact-Analysis.md)     | RecentDocs examination and user-profile attribution |
| [Findings and Conclusions](07-Findings-and-Conclusions/Investigation-Summary.md)    | Consolidated investigative findings and limitations |

## Investigation Reports

1. [Evidence Acquisition and Integrity](02-Evidence-Acquisition/Evidence-Integrity.md)
2. [GPT and NTFS Partition Analysis](03-Partition-Analysis/NTFS-Partition-Analysis.md)
3. [Artifact Recovery](05-Artifact-Recovery/Artifact-Recovery-Analysis.md)
4. [Windows Registry Analysis](06-Registry-Analysis/Registry-Artifact-Analysis.md)
5. [Findings and Conclusions](07-Findings-and-Conclusions/Investigation-Summary.md)

## Key Findings

The examination identified a GPT-partitioned Windows disk image containing an NTFS volume beginning at sector `239616`.

A document referenced in the `mortysmith` Windows user profile's RecentDocs registry artifact was correlated with NTFS MFT record `91143`. An independent extraction using The Sleuth Kit reproduced the previously recovered file, with matching SHA-256 hashes and no reported byte-level differences.

Original and post-analysis logical image hash records matched. A separate documentation correction preserved the audit trail for a SHA-1 transcription discrepancy in the evidence custody log.

## Evidence Handling and Privacy

The forensic image, registry hives, and recovered document contents are not included in this public repository.

This repository presents investigative methodology, documented findings, and supporting analysis while protecting potentially sensitive personal information.

## Skills Demonstrated

Digital forensics • Windows forensics • NTFS analysis • Registry analysis • Evidence integrity • File recovery • Forensic documentation • Evidence correlation
