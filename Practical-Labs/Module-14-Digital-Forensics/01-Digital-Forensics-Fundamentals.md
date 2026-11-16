
# MODULE 14 – DIGITAL FORENSICS

# LESSON 01: DIGITAL FORENSICS FUNDAMENTALS

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define digital forensics.
2. Explain why digital forensics is important.
3. Identify the major branches of digital forensics.
4. Understand the digital forensic investigation process.
5. Explain the difference between evidence collection and evidence analysis.
6. Understand evidence integrity and preservation.
7. Identify common digital forensic tools.

---

## 2. What Is Digital Forensics?

Digital forensics is the scientific process of identifying, collecting, preserving, examining, and presenting digital evidence from electronic devices.

In simple terms:

**Digital forensics is the investigation of digital devices to discover what happened, how it happened, and who may have been responsible.**

Digital evidence can come from:

- Computers
- Smartphones
- Hard drives
- USB flash drives
- Memory cards
- Network traffic
- Servers
- Cloud storage
- Digital cameras

### Real-World Example

Imagine a company discovers that an employee has copied confidential documents onto a USB flash drive.

A digital forensic investigator may:

1. Identify the employee's computer.
2. Secure the relevant evidence.
3. Create a forensic image of the storage device.
4. Examine file activity and timestamps.
5. Search for USB connection records.
6. Analyze relevant logs.
7. Prepare a forensic report.

The goal is to establish facts using reliable digital evidence rather than assumptions.

---

## 3. Why Is Digital Forensics Important?

Digital forensics helps organizations and investigators understand digital incidents.

Its major applications include:

### A. Cybercrime Investigation

Investigating activities such as:
- Unauthorized system access.
- Online fraud.
- Identity theft.
- Malware infections.
- Data theft.

### B. Incident Response

Helping security teams determine:
- What happened during an attack.
- Which systems were affected.
- What information may have been exposed.
- How the attacker gained access.

### C. Employee Misconduct Investigations

Investigating unauthorized copying, deletion, or disclosure of company information, subject to applicable policies and legal authority.

### D. Legal Investigations

Providing properly preserved digital evidence for legal proceedings.

### E. Data Recovery

Recovering or examining deleted, damaged, or inaccessible files where technically possible.

---

## 4. Major Branches of Digital Forensics

### 4.1 Computer Forensics

Examines computers and their storage devices.

Examples:
- Windows computers.
- Linux systems.
- Desktop computers.
- Laptop computers.

Investigators may examine files, logs, browser artifacts, and user activity.

### 4.2 Mobile Device Forensics

Examines smartphones and tablets.

Potential evidence includes:
- Call records.
- Messages.
- Application data.
- Photos and videos.
- Device logs.
- Location-related records, where lawfully available.

### 4.3 Network Forensics

Examines network traffic and communication records.

Examples:
- Packet captures.
- DNS requests.
- Connection records.
- Network logs.

Tools may include Wireshark and Zeek.

### 4.4 Memory Forensics

Examines volatile memory (RAM).

Potential findings include:
- Running processes.
- Active network connections.
- Loaded modules.
- Certain malware artifacts.
- Data that may not be present on disk.

### 4.5 Cloud Forensics

Examines digital evidence associated with cloud services.

Examples:
- Cloud access logs.
- File-sharing records.
- Authentication events.
- Cloud storage activity.

Investigators must consider provider access restrictions, retention policies, and legal requirements.

---

## 5. The Digital Forensic Investigation Process

A forensic investigation follows a structured methodology.

### Stage 1: Identification

Determine which devices, systems, accounts, or data sources may contain relevant evidence.

Example: Identifying a workstation involved in a suspected data leak.

### Stage 2: Preservation

Protect evidence from accidental modification, deletion, or contamination.

Examples:
- Restrict unnecessary access.
- Document the device's condition.
- Maintain proper evidence handling.
- Follow approved procedures before interacting with a device.

### Stage 3: Acquisition

Create a forensic copy or collect evidence using an appropriate, documented method.

For storage devices, investigators may create a bit-for-bit forensic image.

### Stage 4: Examination

Extract relevant information from the acquired evidence.

Examples:
- File listings.
- Metadata.
- Logs.
- Browser artifacts.
- Timestamps.

### Stage 5: Analysis

Interpret the extracted information to answer investigation questions.

Example: Determining whether a file was accessed, modified, or copied during a particular period.

### Stage 6: Documentation and Reporting

Record:
- Investigation scope.
- Tools and versions.
- Procedures performed.
- Findings.
- Limitations.
- Evidence integrity checks.
- Conclusions supported by evidence.

### Investigation Flow

Identification → Preservation → Acquisition → Examination → Analysis → Reporting

---

## 6. What Is Digital Evidence?

Digital evidence is information stored or transmitted in digital form that may help establish facts during an investigation.

Examples:

| Evidence | Possible investigative value |
|---|---|
| Documents | Information and file history |
| Browser history | Web activity |
| System logs | System and security events |
| Emails | Communication records |
| Images | Visual information and metadata |
| Network captures | Communication patterns |
| RAM data | Running processes and volatile artifacts |
| File timestamps | File activity timelines |

### Important Principle

Not every digital artifact proves wrongdoing.

For example, a file's creation timestamp alone does not prove who created it. Evidence must be interpreted in context and corroborated where possible.

---

## 7. Evidence Integrity

Evidence integrity means maintaining evidence in a condition that allows investigators to demonstrate that it has not been improperly altered.

A common method is cryptographic hashing.

### What Is Hashing?

Hashing converts data into a fixed-length value using a mathematical algorithm.

Common algorithms include:

- SHA-256
- SHA-512
- SHA-1 (older; not recommended for modern integrity-sensitive use)
- MD5 (older; not recommended for modern integrity-sensitive use)

For forensic integrity verification, SHA-256 is a common choice.

### Example

Original file:

`evidence.txt`

SHA-256 hash:

`A1B2C3...`

After copying the file, calculate its hash again.

If the hashes match, that supports the conclusion that the two files contain identical data.

A matching hash does not independently prove where the evidence came from or who handled it. That requires documentation and chain-of-custody records.

---

## 8. Chain of Custody

Chain of custody is the documented history of evidence handling.

It records:
- Who collected the evidence.
- When it was collected.
- Where it was collected.
- Who accessed it.
- When it changed hands.
- Where it was stored.

Example:

| Date and time | Person | Action |
|---|---|---|
| 09:00 | Investigator A | Collected USB drive |
| 09:15 | Investigator A | Recorded evidence ID |
| 09:30 | Investigator A | Stored evidence securely |
| 11:00 | Investigator B | Received evidence for examination |

A complete chain of custody helps demonstrate that evidence was properly controlled.

---

## 9. Common Digital Forensic Tools

| Tool | Purpose |
|---|---|
| Autopsy | Digital forensic examination |
| Sleuth Kit | File-system forensic analysis |
| FTK Imager | Forensic imaging and evidence preview |
| Wireshark | Network traffic analysis |
| Volatility | Memory forensics |
| ExifTool | Metadata examination |
| sha256sum | File integrity verification on Linux |

These tools should be used only on devices and evidence you own or are authorized to examine.

---

## 10. Practical Lab: Create a Digital Evidence Workspace

### Objective

Create a small practice evidence workspace and generate SHA-256 hashes for sample files.

This exercise uses only files you create yourself.

### Step 1: Create a Lab Directory

Open Kali Linux and run:

```bash
mkdir -p ~/practical-labs/module14/lab01/evidence
cd ~/practical-labs/module14/lab01
```

### Step 2: Create a Sample Evidence File

```bash
echo "Digital forensics practice evidence" > evidence/sample.txt
```

### Step 3: View the File

```bash
cat evidence/sample.txt
```

### Step 4: Calculate Its SHA-256 Hash

```bash
sha256sum evidence/sample.txt
```

Record the hash in your notes.

### Step 5: Create a Second Copy

```bash
cp evidence/sample.txt evidence/sample-copy.txt
```

### Step 6: Compare Both Hashes

```bash
sha256sum evidence/sample.txt evidence/sample-copy.txt
```

Observe whether the two hashes match.

### Step 7: Test Evidence Integrity

Append a new line to the copy:

```bash
echo "This file has been modified" >> evidence/sample-copy.txt
```

Calculate the hashes again:

```bash
sha256sum evidence/sample.txt evidence/sample-copy.txt
```

The hashes should now be different.

### Step 8: Create an Evidence Inventory

```bash
cat > evidence-inventory.txt <<'EOF'
Evidence ID: E-001
Description: Self-created practice text file
Original file: evidence/sample.txt
Copy: evidence/sample-copy.txt
Collection date: Record current date
Purpose: SHA-256 integrity demonstration
EOF
```

View the inventory:

```bash
cat evidence-inventory.txt
```

### Step 9: List Your Lab Files

```bash
find . -type f
```

You should see:

```text
./evidence/sample.txt
./evidence/sample-copy.txt
./evidence-inventory.txt
```

---

## 11. Practical Lab Questions

Answer these in your notebook:

1. What is digital forensics?
2. Why is evidence preservation important?
3. What is the difference between examination and analysis?
4. What does a SHA-256 hash help an investigator verify?
5. What is chain of custody?
6. Why should investigators avoid modifying original evidence?
7. Name four branches of digital forensics.
8. What is the purpose of an evidence inventory?

---

## 12. Knowledge Check

**Question 1:** Which stage protects digital evidence from accidental modification?

A. Analysis  
B. Preservation  
C. Reporting  
D. Examination

**Question 2:** Which tool is commonly used for memory forensics?

A. Volatility  
B. LibreOffice  
C. UFW  
D. Nano

**Question 3:** What is the purpose of chain of custody?

A. To improve network speed  
B. To document evidence handling  
C. To delete unnecessary files  
D. To encrypt a hard drive

**Question 4:** Which algorithm is recommended here for generating a modern forensic integrity hash?

A. MD5  
B. SHA-1  
C. SHA-256  
D. Base64

**Question 5:** Which branch examines captured network packets?

A. Mobile forensics  
B. Network forensics  
C. Computer forensics  
D. Cloud administration

---

## 13. Lesson Completion Checklist

- [ ] I understand digital forensics.
- [ ] I can identify the major branches.
- [ ] I understand the forensic investigation process.
- [ ] I understand digital evidence and chain of custody.
- [ ] I created my evidence workspace.
- [ ] I generated SHA-256 hashes.
- [ ] I verified that modifying a file changes its hash.
- [ ] I answered the knowledge-check questions.

## 14. Key Takeaway

Digital forensics is not simply about finding files or recovering deleted information. It is a disciplined investigative process that requires evidence preservation, integrity verification, accurate analysis, proper documentation, and defensible reporting.

**Remember: Evidence must be collected and handled carefully before it can be trusted.**
