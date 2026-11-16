
# MODULE 15 – CAPSTONE PROJECT

# LESSON 07: DIGITAL FORENSICS AND EVIDENCE COLLECTION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain digital forensics.
2. Understand digital evidence.
3. Identify different types of forensic evidence.
4. Apply chain-of-custody principles.
5. Collect system information safely.
6. Examine logs and file metadata.
7. Calculate cryptographic hashes.
8. Create an evidence inventory and forensic report.

---

## 2. What Is Digital Forensics?

Digital forensics is the process of identifying, collecting, preserving, examining, and reporting digital evidence.

It is commonly used during:

- Cybersecurity investigations.
- Data breaches.
- Malware incidents.
- Unauthorized access investigations.
- Insider-threat investigations.
- Computer misuse cases.

### Example

Imagine an organization discovers that an employee account accessed confidential files unexpectedly.

A digital forensic investigator may examine:

- Authentication logs.
- File access records.
- System timestamps.
- Network activity.
- Relevant files.
- User account information.

The objective is to establish what happened using reliable evidence.

---

## 3. The Digital Forensics Process

A typical investigation follows these stages:

1. Identification.
2. Preservation.
3. Collection.
4. Examination.
5. Analysis.
6. Documentation and reporting.

### 3.1 Identification

Determine which systems, files, and records may contain relevant evidence.

### 3.2 Preservation

Protect evidence against accidental modification or deletion.

### 3.3 Collection

Acquire relevant information using appropriate methods.

### 3.4 Examination

Extract useful information from collected evidence.

### 3.5 Analysis

Interpret the evidence and establish relationships between events.

### 3.6 Reporting

Document the methods, findings, limitations, and conclusions.

---

## 4. Types of Digital Evidence

| Evidence Type | Examples |
|---|---|
| File evidence | Documents, images, scripts |
| System logs | Authentication and system events |
| Network evidence | PCAP files |
| Metadata | File timestamps and ownership |
| Memory evidence | Running processes and volatile data |
| Disk evidence | Filesystem and storage artifacts |

Digital evidence must be collected and handled carefully.

---

## 5. Understanding Chain of Custody

Chain of custody is the documented history of evidence from collection to final examination or transfer.

It helps establish:

- Who collected the evidence.
- When it was collected.
- Where it came from.
- Who handled it.
- What actions were performed.
- Whether its integrity was maintained.

### Example Chain-of-Custody Record

| Field | Example |
|---|---|
| Evidence ID | EVD-001 |
| Description | Network packet capture |
| Source | Isolated Kali lab |
| Collected by | Investigator |
| Date/time | Actual collection time |
| SHA-256 | Recorded hash |
| Storage | Protected evidence directory |

Do not invent dates, times, or hashes in an actual investigation.

---

## 6. Practical Lab A: Prepare an Evidence Directory

Create a dedicated forensic workspace:

```bash
mkdir -p ~/practical-labs/module15/forensics/evidence
mkdir -p ~/practical-labs/module15/forensics/reports
mkdir -p ~/practical-labs/module15/forensics/working
```

Inspect the directory:

```bash
tree ~/practical-labs/module15/forensics
```

If `tree` is unavailable:

```bash
find ~/practical-labs/module15/forensics -maxdepth 2 -type d
```

Use:
- `evidence` for preserved original copies.
- `working` for analysis copies.
- `reports` for investigation documentation.

For this training exercise, we will work with newly created sample evidence.

---

## 7. Practical Lab B: Collect System Information

Record the hostname:

```bash
hostname
```

Record the operating system:

```bash
cat /etc/os-release
```

Record the current date and time:

```bash
date -Is
```

Record the current user:

```bash
whoami
```

Record network interfaces:

```bash
ip -br addr
```

Record active processes:

```bash
ps aux
```

Save the information:

```bash
{
  echo "FORENSIC SYSTEM INFORMATION"
  echo "Collection time:"
  date -Is
  echo
  echo "Hostname:"
  hostname
  echo
  echo "Operating system:"
  cat /etc/os-release
  echo
  echo "Current user:"
  whoami
  echo
  echo "Network interfaces:"
  ip -br addr
  echo
  echo "Running processes:"
  ps aux
} > ~/practical-labs/module15/forensics/evidence/system-information.txt
```

This is a live-system information collection exercise, not a complete forensic disk image.

---

## 8. Practical Lab C: Collect System Logs

View recent system events:

```bash
sudo journalctl -n 50
```

Save a limited log extract:

```bash
sudo journalctl -n 50 > ~/practical-labs/module15/forensics/evidence/system-log.txt
```

Check SSH-related events:

```bash
sudo journalctl -u ssh -n 50
```

If available, collect authentication logs:

```bash
if [ -f /var/log/auth.log ]; then
  sudo tail -n 50 /var/log/auth.log
fi
```

### What Should You Examine?

- Event timestamps.
- Authentication successes.
- Authentication failures.
- Service restarts.
- Unexpected errors.
- Unusual system activity.

Record observations without assuming that an event proves malicious behavior.

---

## 9. Practical Lab D: Create Sample Digital Evidence

Create a sample file:

```bash
echo "This is sample forensic evidence for Module 15." > ~/practical-labs/module15/forensics/working/sample-evidence.txt
```

Inspect it:

```bash
file ~/practical-labs/module15/forensics/working/sample-evidence.txt
```

Display its contents:

```bash
cat ~/practical-labs/module15/forensics/working/sample-evidence.txt
```

Inspect file metadata:

```bash
stat ~/practical-labs/module15/forensics/working/sample-evidence.txt
```

Look for:

- File size.
- File type.
- Owner.
- Group.
- Permissions.
- Modification time.
- Access time.
- Change time.

These timestamps have different meanings and should not be treated as interchangeable.

---

## 10. Practical Lab E: Calculate File Hashes

A cryptographic hash produces a fixed-length value from input data.

SHA-256 produces a 256-bit digest.

Calculate the sample file's hash:

```bash
sha256sum ~/practical-labs/module15/forensics/working/sample-evidence.txt
```

Copy the sample into the evidence directory:

```bash
cp ~/practical-labs/module15/forensics/working/sample-evidence.txt ~/practical-labs/module15/forensics/evidence/
```

Calculate and record its hash:

```bash
cd ~/practical-labs/module15/forensics/evidence
sha256sum sample-evidence.txt > sample-evidence.sha256
```

Verify:

```bash
sha256sum -c sample-evidence.sha256
```

Expected result:

```text
sample-evidence.txt: OK
```

### Why Is Hashing Important?

Hashing helps detect whether evidence has changed after a reference hash was calculated.

If even a small part of a file changes, its SHA-256 digest will normally change.

A hash does not prove that evidence is authentic or that it was collected correctly. It supports integrity verification.

---

## 11. Practical Lab F: Create an Evidence Inventory

Create:

```bash
cat > ~/practical-labs/module15/forensics/evidence-inventory.md <<'EOF'
# DIGITAL EVIDENCE INVENTORY

## Investigation
Module 15 - Isolated Cybersecurity Capstone

## Evidence Item 1
Evidence ID: EVD-001
Description: System information collection
Source: Kali Linux VM
File: system-information.txt
Hash: [Record SHA-256]

## Evidence Item 2
Evidence ID: EVD-002
Description: System log extract
Source: Kali Linux VM
File: system-log.txt
Hash: [Record SHA-256]

## Evidence Item 3
Evidence ID: EVD-003
Description: Sample forensic text file
Source: Training exercise
File: sample-evidence.txt
Hash: [Record SHA-256]

## Evidence Handling
- Preserve original evidence copies.
- Analyze separate working copies.
- Record collection times.
- Record all handling activities.
- Verify hashes where appropriate.
EOF
```

Calculate hashes for the collected evidence:

```bash
cd ~/practical-labs/module15/forensics/evidence
sha256sum system-information.txt system-log.txt sample-evidence.txt
```

Update the inventory with the actual values.

---

## 12. Practical Lab G: Create a Chain-of-Custody Record

Create:

```bash
cat > ~/practical-labs/module15/forensics/chain-of-custody.md <<'EOF'
# CHAIN-OF-CUSTODY RECORD

## Investigation
Module 15 - Digital Forensics Exercise

## Evidence Information

Evidence ID:
EVD-003

Description:
Sample forensic text file

Original Source:
Training workspace

Collection Method:
File copy

Collection Date and Time:
[Enter actual date and time]

Collected By:
[Enter investigator identifier]

SHA-256:
[Enter actual hash]

Storage Location:
forensics/evidence/

## Handling History

| Date and Time | Person | Action | Purpose |
|---|---|---|---|
| [Actual time] | [Identifier] | Collected | Initial acquisition |
| [Actual time] | [Identifier] | Hash calculated | Integrity verification |

## Notes

This is a training record. Use accurate collection
details and never fabricate evidence history.
EOF
```

Update the placeholders with your actual information.

---

## 13. Practical Lab H: Analyze the Evidence

Examine the evidence directory:

```bash
ls -lah ~/practical-labs/module15/forensics/evidence
```

Inspect file types:

```bash
file ~/practical-labs/module15/forensics/evidence/*
```

Inspect timestamps:

```bash
stat ~/practical-labs/module15/forensics/evidence/sample-evidence.txt
```

Review the system information:

```bash
less ~/practical-labs/module15/forensics/evidence/system-information.txt
```

Review the log extract:

```bash
less ~/practical-labs/module15/forensics/evidence/system-log.txt
```

Record:

1. What information was collected?
2. What timestamps were observed?
3. Were any unusual events visible?
4. What can the evidence establish?
5. What cannot be concluded from the evidence?

A professional investigator distinguishes observed facts from assumptions.

---

## 14. Practical Lab I: Write a Forensic Investigation Report

Create:

```bash
cat > ~/practical-labs/module15/forensics/reports/forensic-report.md <<'EOF'
# DIGITAL FORENSIC INVESTIGATION REPORT

## 1. Investigation Details

Project:
Isolated Cybersecurity Assessment and Defense Lab

Investigation:
Digital Forensics and Evidence Collection

Environment:
Kali Linux virtual machine

## 2. Objective

Collect and examine selected system information,
logs, file metadata, and sample evidence.

## 3. Collection Methodology

- System information collection
- System log extraction
- File metadata inspection
- SHA-256 hashing
- Evidence inventory
- Chain-of-custody documentation

## 4. Evidence Collected

[List actual evidence files.]

## 5. Evidence Integrity

[Record actual SHA-256 verification results.]

## 6. Findings

[Document observations supported by evidence.]

## 7. Limitations

This exercise used a live training VM and sample files.

It was not a complete forensic acquisition of a disk
or memory image.

## 8. Conclusion

[Summarize what the collected evidence establishes.]

## 9. Recommendations

- Preserve original evidence.
- Maintain accurate custody records.
- Analyze working copies.
- Verify file integrity.
- Document all investigative actions.
EOF
```

Edit the report:

```bash
nano ~/practical-labs/module15/forensics/reports/forensic-report.md
```

---

## 15. Important Forensic Principles

### Principle 1: Preserve Evidence

Avoid changing original evidence.

### Principle 2: Maintain Documentation

Record collection methods, timestamps, and handling activities.

### Principle 3: Verify Integrity

Use cryptographic hashes to identify changes.

### Principle 4: Work on Copies

Where possible, preserve originals and analyze separate working copies.

### Principle 5: Avoid Unsupported Conclusions

Only report conclusions supported by the available evidence.

### Principle 6: Protect Confidentiality

Evidence may contain private or sensitive information. Restrict access appropriately.

---

## 16. Knowledge Check

1. What is digital forensics?
2. What are the major stages of a forensic investigation?
3. What is digital evidence?
4. Explain chain of custody.
5. Why should original evidence be preserved?
6. What is file metadata?
7. What does SHA-256 help verify?
8. What is the difference between evidence and an investigative conclusion?
9. Why should investigators analyze working copies?
10. What information belongs in a forensic report?

---

## 17. Practical Completion Checklist

- [ ] I created a forensic workspace.
- [ ] I collected system information.
- [ ] I collected selected system logs.
- [ ] I created a sample evidence file.
- [ ] I examined file metadata.
- [ ] I calculated SHA-256 hashes.
- [ ] I verified evidence integrity.
- [ ] I created an evidence inventory.
- [ ] I created a chain-of-custody record.
- [ ] I wrote a forensic investigation report.

---

## 18. Key Takeaway

Digital forensics depends on reliable evidence, careful preservation, accurate documentation, and objective analysis.

**Remember: Evidence must be preserved and its integrity supported before conclusions are drawn from it.**
