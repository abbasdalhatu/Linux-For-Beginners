
# MODULE 14 – DIGITAL FORENSICS

# LESSON 09: FORENSIC INVESTIGATION AND REPORTING

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain the digital forensic investigation process.
2. Understand evidence collection and preservation.
3. Apply chain-of-custody procedures.
4. Organize forensic evidence.
5. Analyze evidence and correlate findings.
6. Distinguish facts from assumptions.
7. Write a professional forensic investigation report.
8. Present findings clearly and objectively.

---

## 2. What Is a Forensic Investigation?

A digital forensic investigation is a structured process of identifying, collecting, preserving, examining, and reporting digital evidence.

Its purpose is to establish what happened, how it happened, when it happened, and what evidence supports the conclusion.

### Example Scenario

An organization reports that an employee's computer may have been used to access an unauthorized file.

A forensic investigator may need to determine:

- Which user account was involved?
- When did the activity occur?
- Which files were accessed?
- Were files copied or modified?
- Was an external storage device connected?
- What evidence supports the findings?

The investigator must avoid making conclusions without sufficient evidence.

---

## 3. The Digital Forensic Investigation Process

A typical investigation follows these stages:

### Stage 1: Identification

Identify the incident and potential evidence sources.

Examples:
- Desktop computers.
- Laptops.
- Mobile devices.
- External drives.
- Server logs.
- Network records.
- Memory images.

### Stage 2: Preservation

Protect evidence from alteration, loss, or destruction.

Examples:
- Restrict access.
- Document the device condition.
- Use approved evidence-handling procedures.
- Preserve original evidence.
- Maintain chain of custody.

### Stage 3: Collection

Acquire evidence using an appropriate and documented method.

Examples:
- Disk imaging.
- Memory acquisition.
- Log collection.
- File collection.

### Stage 4: Examination

Extract relevant information from the collected evidence.

Examples:
- File-system examination.
- Metadata analysis.
- Hash verification.
- Memory analysis.
- Log analysis.

### Stage 5: Analysis

Interpret the examination results and determine how the evidence relates to the investigation.

### Stage 6: Reporting

Document the investigation, methods, findings, limitations, and conclusions.

---

## 4. Evidence Organization

A professional investigation requires organized evidence storage.

Example:

```text
Investigation-001/
│
├── evidence/
│   ├── disk-image.raw
│   ├── memory-image.raw
│   └── system-logs.txt
│
├── hashes/
│   └── sha256-hashes.txt
│
├── reports/
│   ├── preliminary-report.md
│   └── final-report.md
│
└── documentation/
    ├── evidence-register.csv
    └── chain-of-custody.csv
```

This structure separates evidence from analysis and reporting.

Never edit original evidence files during analysis. Use verified working copies.

---

## 5. Evidence Register

An evidence register records the evidence collected during an investigation.

Example:

| Evidence ID | Description | Source | SHA-256 Recorded |
|---|---|---|---|
| EV-001 | Disk image | Laptop A | Yes |
| EV-002 | Memory image | Laptop A | Yes |
| EV-003 | Authentication logs | Server A | Yes |
| EV-004 | USB device image | External drive | Yes |

The register should also include relevant acquisition dates, responsible personnel, storage locations, and status.

The table above is fictional.

---

## 6. Chain of Custody

Chain of custody documents the history of evidence from collection through examination, transfer, storage, and final disposition.

It helps establish accountability and identify possible evidence-handling issues.

### Example

```text
Evidence ID: EV-001

Collected by: Investigator A
Collection date: 15 November 2026
Collection time: 10:30
Description: Training disk image
Initial SHA-256: Recorded

Transfer:
From: Investigator A
To: Investigator B

Transfer date: 15 November 2026
Transfer time: 11:15

Purpose:
Authorized forensic examination.

Evidence status:
Stored securely.
```

Every transfer should be documented.

A missing or incomplete custody record may affect confidence in the evidence.

---

## 7. Evidence Correlation

Evidence correlation means comparing multiple evidence sources to understand an event.

For example:

| Source | Finding |
|---|---|
| Authentication log | Successful login at 09:15 |
| File metadata | Document modified at 09:22 |
| USB log | USB device connected at 09:20 |
| Network log | External connection recorded at 09:25 |

Together, these records may help establish a timeline.

However, timestamps may use different time zones, clocks, and precision levels.

Investigators should normalize timestamps carefully and document any uncertainty.

A matching timestamp alone does not prove that two events were causally connected.

---

## 8. Building an Investigation Timeline

A timeline arranges relevant events in chronological order.

Fictional example:

| Time | Event | Evidence Source |
|---|---|---|
| 09:15 | User login | Authentication log |
| 09:20 | USB device connected | Device log |
| 09:22 | File modified | File metadata |
| 09:25 | Network connection recorded | Network log |
| 09:40 | User logout | Authentication log |

A good timeline should distinguish:

- Directly observed events.
- Inferred events.
- Uncertain timestamps.
- Missing information.

Do not invent events to fill gaps.

---

## 9. Facts, Inferences, and Assumptions

These three concepts must not be confused.

### Fact

A finding directly supported by evidence.

Example:

"The authentication log records a successful login at 09:15."

### Inference

A reasonable interpretation based on multiple findings.

Example:

"The USB device may have been connected during the same period in which the file was modified."

### Assumption

Something accepted without sufficient supporting evidence.

Example:

"The employee copied the document because a USB device was connected."

An assumption is not a proven finding.

Professional forensic reporting clearly separates these categories.

---

## 10. Professional Forensic Reporting

A forensic report communicates the investigation to technical and non-technical readers.

A good report should be:

- Accurate.
- Objective.
- Clear.
- Reproducible where possible.
- Properly documented.
- Honest about limitations.

### Recommended Report Structure

1. Report title.
2. Investigation reference.
3. Executive summary.
4. Scope and authorization.
5. Investigation objectives.
6. Evidence received.
7. Tools and methods.
8. Examination results.
9. Timeline.
10. Findings.
11. Limitations.
12. Conclusions.
13. Recommendations.
14. Appendices.

---

## 11. Understanding the Executive Summary

The executive summary gives readers a short overview of the investigation.

It should explain:

- Why the investigation was conducted.
- What evidence was examined.
- What was discovered.
- What the findings mean.
- What limitations exist.

Avoid technical detail that belongs in the main report.

### Example

"This investigation examined a set of authorized training files to demonstrate evidence preservation, hash verification, and metadata analysis. The examined copy matched its preserved reference copy using SHA-256. No unauthorized activity was investigated or established."

---

## 12. Tools Used in Forensic Reporting

| Tool | Purpose |
|---|---|
| sha256sum | Evidence integrity verification |
| stat | File metadata examination |
| file | File-type identification |
| ExifTool | Metadata examination |
| Autopsy | Digital forensic analysis |
| Volatility 3 | Memory analysis |
| Sleuth Kit | File-system forensic analysis |
| LibreOffice Writer | Report preparation |

Record tool versions and relevant settings whenever they affect the results.

---

## 13. Practical Lab: Create a Forensic Investigation Report

### Objective

Create a small fictional investigation using your existing training evidence and produce a professional report.

This is a documentation exercise, not an investigation of a real person or system.

### Step 1: Create the Investigation Workspace

```bash
mkdir -p ~/practical-labs/module14/lab09/{evidence,hashes,documentation,reports}
cd ~/practical-labs/module14/lab09
```

### Step 2: Create Sample Evidence

```bash
echo "Authorized digital forensics investigation training." > evidence/sample-document.txt
echo "User login recorded at 09:15." > evidence/authentication-log.txt
echo "USB device connection recorded at 09:20." > evidence/device-log.txt
```

### Step 3: Calculate Evidence Hashes

```bash
sha256sum evidence/* > hashes/sha256-hashes.txt
```

View the hashes:

```bash
cat hashes/sha256-hashes.txt
```

### Step 4: Create an Evidence Register

```bash
cat > documentation/evidence-register.csv <<'EOF'
Evidence ID,Description,Source,Integrity Status
EV-001,Sample document,Training directory,Hash recorded
EV-002,Authentication log,Training directory,Hash recorded
EV-003,Device connection log,Training directory,Hash recorded
EOF
```

### Step 5: Create a Chain-of-Custody Record

```bash
cat > documentation/chain-of-custody.csv <<'EOF'
Evidence ID,Action,Responsible Party,Date,Time,Notes
EV-001,Created,Training investigator,2026-11-15,10:00,Self-created training evidence
EV-002,Created,Training investigator,2026-11-15,10:00,Self-created training evidence
EV-003,Created,Training investigator,2026-11-15,10:00,Self-created training evidence
EOF
```

These are simulated training records, not real acquisition records.

### Step 6: Create an Investigation Timeline

```bash
cat > documentation/timeline.txt <<'EOF'
TRAINING INVESTIGATION TIMELINE

09:15 - Authentication event recorded.
09:20 - USB device connection event recorded.

Note:
These events are fictional training records.
The timestamps do not establish that one event caused another.
EOF
```

### Step 7: Create the Final Report

```bash
cat > reports/final-report.md <<'EOF'
# DIGITAL FORENSIC INVESTIGATION REPORT

## 1. Investigation Details

Investigation ID: INV-001
Investigation Type: Training Investigation
Investigator: Training Investigator
Date: 15 November 2026

## 2. Executive Summary

This exercise demonstrates evidence organization, SHA-256
hashing, chain-of-custody documentation, and timeline creation.

## 3. Scope

The investigation is limited to self-created training files.

No real person, organization, or external computer was investigated.

## 4. Evidence Examined

- EV-001: Sample document
- EV-002: Authentication log
- EV-003: Device connection log

## 5. Tools and Methods

- Linux terminal
- sha256sum
- Text-based evidence documentation

## 6. Findings

Three training evidence files were created.

SHA-256 hashes were calculated and recorded.

A simulated evidence register and chain-of-custody record
were produced.

## 7. Timeline

09:15 - Authentication event recorded.
09:20 - USB device connection event recorded.

## 8. Limitations

All records are fictional training evidence.

No physical device acquisition, memory acquisition,
or real-world incident investigation was performed.

## 9. Conclusion

The training exercise demonstrates how evidence can be
organized, hashed, documented, and presented in a forensic report.

No real-world misconduct or unauthorized activity was established.

## 10. Recommendations

- Preserve original evidence.
- Verify evidence integrity.
- Maintain complete custody records.
- Separate facts from assumptions.
- Document investigative limitations.
EOF
```

### Step 8: Verify Evidence Integrity

Run:

```bash
sha256sum -c hashes/sha256-hashes.txt
```

Expected result:

```text
evidence/authentication-log.txt: OK
evidence/device-log.txt: OK
evidence/sample-document.txt: OK
```

### Step 9: Review Your Investigation

```bash
find . -maxdepth 3 -type f
```

Your workspace should contain:

```text
evidence/
hashes/
documentation/
reports/
```

---

## 14. Practical Investigation Challenge

Answer these questions:

1. What are the major stages of a forensic investigation?
2. Why is evidence preservation important?
3. What information belongs in an evidence register?
4. What is the purpose of chain of custody?
5. Explain evidence correlation.
6. Why should timestamps be normalized?
7. What is the difference between a fact and an inference?
8. What should a forensic report contain?
9. Why should investigators document limitations?
10. Why should original evidence remain unchanged?

---

## 15. Knowledge Check

**Question 1:** What is the first major stage of a forensic investigation?

A. Identification  
B. Reporting  
C. Destruction  
D. Publication

**Question 2:** What records the history of evidence handling?

A. Chain of custody  
B. DNS table  
C. Routing table  
D. Process list

**Question 3:** Which statement is a fact?

A. The employee definitely stole the file.  
B. A log records a successful login at 09:15.  
C. The USB connection proves data theft.  
D. The user must be guilty.

**Question 4:** What is the purpose of evidence correlation?

A. Compare multiple evidence sources  
B. Delete duplicate files  
C. Disable system logging  
D. Change file permissions

**Question 5:** What should a professional forensic report include?

A. Findings and limitations  
B. Unsupported accusations  
C. Unverified assumptions only  
D. Personal opinions

---

## 16. Lesson Completion Checklist

- [ ] I understand the forensic investigation process.
- [ ] I understand evidence preservation.
- [ ] I understand chain of custody.
- [ ] I can create an evidence register.
- [ ] I understand evidence correlation.
- [ ] I can organize an investigation timeline.
- [ ] I understand facts, inferences, and assumptions.
- [ ] I created a forensic investigation report.
- [ ] I verified evidence hashes.
- [ ] I completed the knowledge check.

---

## 17. Key Takeaway

Digital forensic investigation is not just about finding information. It is about collecting, preserving, examining, interpreting, and presenting evidence in a reliable and defensible way.

**Remember: A strong forensic conclusion must be supported by documented evidence, not guesses or assumptions.**
