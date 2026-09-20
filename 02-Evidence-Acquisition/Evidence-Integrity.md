## Evidence Integrity Verification

Evidence integrity verification was performed
as part of the Windows 10 NTFS forensic
investigation.

The forensic evidence consisted of a segmented
EWF image containing four evidence segments.

The image was mounted using ewfmount, exposing
the reconstructed disk image at:

/mnt/ewf/ewf1

Cryptographic hashes were recorded before and
after forensic analysis.

### Hash Verification Results

| Algorithm | Verification |
|-----------|--------------|
| MD5       | Match        |
| SHA-1     | Match        |
| SHA-256   | Match        |

The original and post-analysis hash records
were compared using the Linux diff utility.

No differences were identified.

A transcription discrepancy was subsequently
identified in the SHA-1 value recorded in the
original custody log.

The discrepancy was documented separately
while preserving the original record.

### Forensic Significance

Matching pre-analysis and post-analysis hash
values support the conclusion that the
reconstructed evidence image remained
unchanged between verification events.

Maintaining evidence integrity is essential
to ensuring forensic findings are reliable
and reproducible.
