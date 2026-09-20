# NTFS Partition Analysis

## Investigation Overview

A forensic examination of a Windows 10 EWF disk image was conducted using Kali Linux and The Sleuth Kit. The examination included analysis of the NTFS file system associated with partition 3.

The fsstat utility was used to identify file system characteristics, metadata structures, and allocation information.

## Forensic Tool

The Sleuth Kit: fsstat

## File System Information

| Attribute | Finding |
|---|---|
| File System | NTFS |
| Volume Serial Number | 46B0E0F2B0E0E8FF |
| Sector Size | 512 bytes |
| Cluster Size | 4096 bytes |
| MFT Entry Size | 1024 bytes |
| Index Record Size | 4096 bytes |
| First MFT Cluster | 786432 |
| MFT Mirror Cluster | 2 |
| Root Directory Entry | 5 |
| Total Cluster Range | 0–3772173 |

## Master File Table Analysis

The NTFS Master File Table ($MFT) begins at cluster 786432. The MFT stores metadata records describing files and directories within the NTFS volume.

Each MFT entry is 1024 bytes. These records provide information that may support file identification, timestamp analysis, and file system reconstruction.

## File System Allocation

The examined volume uses 512-byte sectors and 4096-byte clusters. Each cluster therefore contains eight sectors.

Understanding the file system allocation structure supports interpretation of file locations and metadata during forensic examination.

## Root Directory

The NTFS root directory is identified by MFT entry 5. This record represents the root of the volume's directory hierarchy.

## Forensic Significance

The examination established the NTFS volume's structural characteristics and identified key metadata locations. These findings provide a foundation for subsequent file system enumeration, artifact recovery, and user activity reconstruction.

## Evidence Handling

The analysis was conducted against the mounted forensic image. Original and post-analysis hash records were compared as part of the evidence integrity verification process.

A separate evidence integrity report documents the verification results and a transcription discrepancy identified in the original custody log.
