
# MODULE 14 – DIGITAL FORENSICS

# LESSON 10: MODULE 14 FINAL PRACTICAL ASSESSMENT

## 1. Assessment Overview

Welcome to the final practical assessment for Module 14 – Digital Forensics.

This assessment evaluates your understanding of:

1. Digital forensics fundamentals.
2. Digital evidence and chain of custody.
3. File systems and forensic analysis.
4. File hashing and integrity.
5. Disk imaging and acquisition.
6. Metadata and file analysis.
7. Deleted file recovery.
8. Memory forensics.
9. Forensic investigation and reporting.

### Assessment Objectives

By completing this assessment, you should demonstrate that you can:

- Prepare a forensic investigation workspace.
- Create and preserve digital evidence.
- Calculate and verify SHA-256 hashes.
- Examine file metadata.
- Identify file types.
- Document evidence.
- Maintain chain-of-custody records.
- Prepare a forensic investigation report.

### Safety Rules

- Use only self-created training evidence.
- Do not examine another person's device without authorization.
- Do not perform experiments on your Windows host's physical disk.
- Do not alter original evidence.
- Preserve original files and work on copies.
- Document every important action.

---

# PART A – FORENSIC WORKSPACE PREPARATION

## 2. Create the Final Assessment Directory

Run:

```bash
mkdir -p ~/practical-labs/module14/final-assessment/{evidence,working-copies,hashes,metadata,reports,documentation}
```

Navigate into it:

```bash
cd ~/practical-labs/module14/final-assessment
```

Verify:

```bash
find . -maxdepth 2 -type d
```

Expected structure:

```text
final-assessment/
├── evidence/
├── working-copies/
├── hashes/
├── metadata/
├── reports/
└── documentation/
```

---

# PART B – EVIDENCE CREATION AND PRESERVATION

## 3. Create Training Evidence

Create three sample files:

```bash
echo "Digital forensic investigation training document." > evidence/document.txt
```

```bash
echo "Authentication event: successful login at 09:15." > evidence/authentication-log.txt
```

```bash
echo "System event: USB device connected at 09:20." > evidence/system-log.txt
```

Create an additional file:

```bash
echo "Training evidence for file integrity examination." > evidence/integrity-test.txt
```

List your evidence:

```bash
ls -lh evidence/
```

## 4. Preserve Original Evidence

Change the permissions of the evidence files to read-only for the owner:

```bash
chmod 400 evidence/*
```

Verify:

```bash
ls -l evidence/
```

This is a basic practice demonstration, not a forensic write blocker.

The permissions can be changed again by an appropriately privileged owner. Proper forensic preservation requires more than file permissions.

## 5. Create Working Copies

```bash
cp evidence/* working-copies/
```

Verify:

```bash
ls -lh working-copies/
```

Your original evidence should remain separate from the working copies.

---

# PART C – HASHING AND INTEGRITY VERIFICATION

## 6. Calculate SHA-256 Hashes

```bash
sha256sum evidence/* > hashes/original-evidence-sha256.txt
```

View the records:

```bash
cat hashes/original-evidence-sha256.txt
```

## 7. Verify Evidence Integrity

Run:

```bash
sha256sum -c hashes/original-evidence-sha256.txt
```

Expected result:

```text
evidence/authentication-log.txt: OK
evidence/document.txt: OK
evidence/integrity-test.txt: OK
evidence/system-log.txt: OK
```

## 8. Compare Original and Working Copies

Run:

```bash
for f in evidence/*; do
    name=$(basename "$f")
    echo "Checking $name"
    sha256sum "$f" "working-copies/$name"
done
```

Confirm that each original and its corresponding working copy have matching hashes.

## 9. Integrity Tampering Simulation

Modify only the working copy:

```bash
echo "Additional training information." >> working-copies/integrity-test.txt
```

Now compare its hash with the original:

```bash
sha256sum evidence/integrity-test.txt working-copies/integrity-test.txt
```

The hashes should differ.

This demonstrates how a change in file content can be detected through hashing.

Do not modify the original evidence.

---

# PART D – FILE SYSTEM AND METADATA ANALYSIS

## 10. Examine File Types

Run:

```bash
file evidence/*
```

Record the results.

## 11. Examine File Metadata

```bash
stat evidence/document.txt
```

Also examine all files:

```bash
for f in evidence/*; do
    echo "===== $f ====="
    stat "$f"
done
```

Observe:

- File size.
- File permissions.
- Owner.
- Inode.
- Access time.
- Modification time.
- Status change time.

Remember: Linux ctime is inode status change time, not necessarily file creation time.

## 12. Examine File Signatures

Use:

```bash
xxd -l 32 evidence/document.txt
```

Observe the first bytes of the file.

Compare this with the output from:

```bash
file evidence/document.txt
```

Explain why file signatures can be useful during forensic investigations.

---

# PART E – DELETED FILE RECOVERY CONCEPT

## 13. Simulate Deleted File Recovery

Create a new training file:

```bash
echo "Deleted file recovery assessment." > evidence/recovery-test.txt
```

Create a preserved copy:

```bash
cp evidence/recovery-test.txt working-copies/recovery-test.txt
```

Calculate its hash:

```bash
sha256sum evidence/recovery-test.txt working-copies/recovery-test.txt
```

Now simulate deletion:

```bash
chmod u+w evidence/recovery-test.txt
rm evidence/recovery-test.txt
```

Restore the content from the preserved copy:

```bash
cp working-copies/recovery-test.txt recovered-recovery-test.txt
```

Verify:

```bash
cat recovered-recovery-test.txt
```

Calculate hashes:

```bash
sha256sum working-copies/recovery-test.txt recovered-recovery-test.txt
```

The hashes should match.

**Assessment note:** This is backup-based recovery, not raw disk recovery or file carving.

---

# PART F – MEMORY FORENSICS PREPARATION

## 14. Prepare Memory Forensics Records

Create a training memory investigation record:

```bash
cat > documentation/memory-forensics-record.txt <<'EOF'
MEMORY FORENSICS RECORD

Investigation ID: M14-001

Memory evidence type:
Training exercise only.

Analysis tool:
Volatility 3

Acquisition:
No physical memory acquisition performed.

Analysis:
No actual memory image analyzed.

Purpose:
Document preparation for memory forensic analysis.

Limitations:
No genuine RAM image was supplied.
EOF
```

If Volatility 3 is installed, verify:

```bash
vol -h
```

You do not need to analyze an actual memory image for this assessment.

---

# PART G – EVIDENCE DOCUMENTATION

## 15. Create an Evidence Register

```bash
cat > documentation/evidence-register.csv <<'EOF'
Evidence ID,Description,Location,Status
EV-001,Training document,evidence/document.txt,Preserved
EV-002,Authentication log,evidence/authentication-log.txt,Preserved
EV-003,System log,evidence/system-log.txt,Preserved
EV-004,Integrity test file,evidence/integrity-test.txt,Preserved
EOF
```

## 16. Create a Chain-of-Custody Record

```bash
cat > documentation/chain-of-custody.csv <<'EOF'
Evidence ID,Action,Responsible Party,Date,Notes
EV-001,Created,Training Investigator,2026-11-15,Self-created evidence
EV-002,Created,Training Investigator,2026-11-15,Self-created evidence
EV-003,Created,Training Investigator,2026-11-15,Self-created evidence
EV-004,Created,Training Investigator,2026-11-15,Self-created evidence
EOF
```

These are simulated records for this assessment.

---

# PART H – FINAL FORENSIC REPORT

## 17. Generate Your Final Report

Create the report:

```bash
cat > reports/final-forensic-report.md <<'EOF'
# MODULE 14 FINAL FORENSIC REPORT

## 1. Investigation Details

Investigation ID: M14-001

Investigation Type: Digital Forensics Training

Investigator: Training Investigator

Date: 15 November 2026

## 2. Executive Summary

This assessment demonstrates digital evidence creation,
preservation, hashing, metadata examination, backup-based
recovery, and forensic documentation.

## 3. Scope

The investigation was limited to self-created training files.

No external devices, individuals, or organizations were investigated.

## 4. Evidence Examined

- EV-001: Training document
- EV-002: Authentication log
- EV-003: System log
- EV-004: Integrity test file

## 5. Tools Used

- Linux terminal
- sha256sum
- stat
- file
- xxd
- cp
- chmod

## 6. Methods

Evidence was created in a dedicated directory.

SHA-256 hashes were calculated.

Working copies were created.

File metadata and file types were examined.

A controlled deletion and backup-based recovery simulation
was performed.

## 7. Findings

The original training files were successfully created.

SHA-256 integrity records were generated.

Working copies initially matched their originals.

A modified working copy produced a different hash.

File metadata and file types were examined.

A deleted training file was restored from a preserved copy.

## 8. Memory Forensics

Memory forensic preparation was documented.

No physical memory acquisition or actual memory image
analysis was performed.

## 9. Evidence Handling

An evidence register and simulated chain-of-custody record
were created.

Original evidence was separated from working copies.

## 10. Limitations

This was a controlled training exercise.

No physical disk imaging was performed.

No raw-device file carving was performed.

No actual memory image was analyzed.

The training evidence does not establish any real-world incident.

## 11. Conclusion

The assessment demonstrates the fundamental stages of a
digital forensic investigation.

Evidence integrity, preservation, documentation, and
objective reporting were practiced.

## 12. Recommendations

- Preserve original evidence.
- Verify hashes after acquisition and transfer.
- Maintain complete chain-of-custody records.
- Analyze working copies.
- Document limitations.
- Support conclusions with evidence.
EOF
```

---

# PART I – FINAL VERIFICATION

## 18. Verify Evidence Hashes Again

```bash
sha256sum -c hashes/original-evidence-sha256.txt
```

The original evidence should still pass verification.

Remember that the integrity-test working copy was modified intentionally. Its original evidence file should remain unchanged.

## 19. Inspect Your Final Workspace

```bash
find . -maxdepth 3 -type f
```

Expected structure:

```text
final-assessment/
│
├── evidence/
│   ├── authentication-log.txt
│   ├── document.txt
│   ├── integrity-test.txt
│   └── system-log.txt
│
├── working-copies/
│   ├── authentication-log.txt
│   ├── document.txt
│   ├── integrity-test.txt
│   ├── recovery-test.txt
│   └── system-log.txt
│
├── hashes/
│   └── original-evidence-sha256.txt
│
├── documentation/
│   ├── evidence-register.csv
│   ├── chain-of-custody.csv
│   └── memory-forensics-record.txt
│
└── reports/
    └── final-forensic-report.md
```

You should also have:

```text
recovered-recovery-test.txt
```

in the assessment root directory.

---

# PART J – THEORY EXAMINATION

Answer these questions to test your understanding.

### Section 1: Fundamentals

1. Define digital forensics.
2. Identify five branches of digital forensics.
3. Explain the importance of evidence preservation.
4. What is chain of custody?
5. Differentiate volatile and non-volatile evidence.

### Section 2: File Systems and Integrity

6. What is an inode?
7. Explain the difference between allocated and unallocated space.
8. What is a cryptographic hash?
9. Why is SHA-256 useful in forensic investigations?
10. Explain the difference between hashing and encryption.

### Section 3: Acquisition and Recovery

11. What is disk imaging?
12. Explain physical and logical acquisition.
13. What is a write blocker?
14. What is file carving?
15. How can TRIM affect SSD recovery?

### Section 4: Metadata and Memory

16. What is file metadata?
17. What information can EXIF metadata contain?
18. What is a file signature?
19. Define memory forensics.
20. Name four common Volatility 3 plugins or plugin families.

### Section 5: Investigation and Reporting

21. Identify the major stages of a forensic investigation.
22. What is evidence correlation?
23. Differentiate a fact, inference, and assumption.
24. List eight sections of a professional forensic report.
25. Why must investigators document limitations?

---

# PART K – PRACTICAL SCENARIOS

## Scenario 1: Evidence Integrity

An investigator calculates a SHA-256 hash before transferring a file.

After transfer, the hash is different.

Questions:

1. What does this indicate?
2. What should the investigator do?
3. Can the investigator immediately conclude that the file was deliberately altered?

## Scenario 2: Deleted Evidence

An organization reports that an important document was deleted.

Questions:

1. What should the investigator do first?
2. Why should new writes be avoided?
3. How might SSD TRIM affect recovery?
4. Why is a forensic image useful?

## Scenario 3: Suspicious Memory Activity

A memory examination reveals an unexpected process and a network connection.

Questions:

1. What should the investigator examine next?
2. Why is the process name alone insufficient?
3. What additional evidence could help validate the finding?

## Scenario 4: Conflicting Timestamps

A file metadata record shows 09:20, while a system log shows 08:20.

Questions:

1. What could explain the difference?
2. What role might time zones play?
3. How should the investigator document the discrepancy?

---

# PART L – MODULE COMPLETION CHECKLIST

- [ ] Lesson 01: Digital Forensics Fundamentals
- [ ] Lesson 02: Digital Evidence and Chain of Custody
- [ ] Lesson 03: File Systems and Forensic Analysis
- [ ] Lesson 04: File Hashing and Integrity
- [ ] Lesson 05: Disk Imaging and Acquisition
- [ ] Lesson 06: Metadata and File Analysis
- [ ] Lesson 07: Deleted File Recovery
- [ ] Lesson 08: Memory Forensics Fundamentals
- [ ] Lesson 09: Forensic Investigation and Reporting
- [ ] Lesson 10: Final Practical Assessment

## Final Skills Checklist

- [ ] I can create a forensic workspace.
- [ ] I can organize evidence.
- [ ] I can calculate SHA-256 hashes.
- [ ] I can verify file integrity.
- [ ] I can examine file metadata.
- [ ] I understand disk imaging.
- [ ] I understand deleted-file recovery.
- [ ] I understand memory forensics.
- [ ] I can document chain of custody.
- [ ] I can create a forensic report.

---

# MODULE 14 – COMPLETION STATEMENT

Congratulations!

You have completed the Digital Forensics training module.

You have studied the principles and practiced essential activities involving:

- Digital evidence.
- File-system analysis.
- Hash verification.
- Disk acquisition concepts.
- Metadata examination.
- Deleted-file recovery concepts.
- Memory forensic preparation.
- Evidence handling.
- Investigation reporting.

**Professional reminder:** Digital forensics is about preserving the truth. Every conclusion must be supported by reliable evidence, documented methods, and transparent limitations.
EOF
```

---

## 20. Final Assessment Completion

When your practical assessment is complete, review your report and verify that the original evidence hashes still match.

You may keep the theory questions for later revision; you do not need to submit all 25 answers before proceeding.

**Next milestone:** Module 14 completion, followed by Module 15 – Capstone Project.
