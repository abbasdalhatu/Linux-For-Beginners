
# MODULE 14 – DIGITAL FORENSICS

# LESSON 05: DISK IMAGING AND ACQUISITION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define disk imaging.
2. Explain forensic acquisition.
3. Differentiate physical and logical acquisition.
4. Understand bit-for-bit disk imaging.
5. Identify common forensic image formats.
6. Understand write blockers.
7. Generate and verify forensic image hashes.
8. Create a basic forensic acquisition report.

---

## 2. What Is Disk Imaging?

Disk imaging is the process of creating a copy of data stored on a storage device.

In digital forensics, a forensic disk image is an acquired copy created using a documented method to preserve and examine digital evidence.

In simple terms:

Imagine a computer hard drive contains important evidence. Instead of performing the investigation directly on the original drive, an investigator creates a forensic image and examines the acquired copy.

### Example

Original storage device:

```text
Physical hard drive
       |
       v
Forensic acquisition
       |
       v
Forensic image
       |
       v
Forensic examination
```

The original evidence should be preserved according to the investigation procedure.

---

## 3. Why Is Disk Imaging Important?

Disk imaging helps investigators:

- Preserve a source device.
- Examine data without repeatedly handling the original.
- Maintain an evidence trail.
- Perform repeatable analysis.
- Recover some deleted-file information where possible.
- Examine file-system structures.
- Verify acquired data using cryptographic hashes.

A forensic image may contain more than ordinary visible files, depending on the acquisition method and source device.

For example, a physical image may include:

- Partition information.
- File-system structures.
- Allocated data.
- Unallocated space.
- Some deleted-file remnants.
- Other accessible sectors.

---

## 4. Types of Forensic Acquisition

### 4.1 Physical Acquisition

Physical acquisition attempts to capture the accessible addressable contents of a storage device at the physical-device level.

It may include:

- Partition tables.
- File-system structures.
- Allocated sectors.
- Unallocated sectors.

A physical acquisition is useful when investigators need a broader view of the storage device.

However, some modern devices use encryption, controller-level translation, inaccessible areas, or hardware-specific behavior that can limit acquisition.

### 4.2 Logical Acquisition

Logical acquisition collects selected files, directories, or logical data accessible through the operating system or acquisition interface.

Examples:

- Documents.
- Pictures.
- Log files.
- User directories.
- Selected application data.

Logical acquisition may not capture deleted data or unallocated storage areas.

### Comparison

| Physical acquisition | Logical acquisition |
|---|---|
| Device-level capture | Selected logical data |
| May include unallocated space | Usually focuses on accessible files |
| Can capture file-system structures | Preserves selected file information |
| Often larger | Often smaller |

---

## 5. What Is a Bit-for-Bit Image?

A bit-for-bit image is a copy that attempts to reproduce the source device's accessible data at the sector level.

It aims to preserve the source's contents, including areas that may not appear in ordinary file listings.

For example, a storage device may contain:

- 100 GB of visible files.
- File-system metadata.
- Unallocated space.
- Other storage structures.

A suitable physical image may preserve these areas rather than copying only the visible files.

Important: An image file is not automatically a perfect representation of every aspect of a physical device. Acquisition limitations must be documented.

---

## 6. Common Forensic Image Formats

### 6.1 RAW Image

RAW is a straightforward byte-for-byte image format.

Common extension:

```text
disk-image.raw
```

Advantages:
- Simple format.
- Broad compatibility.
- Useful for forensic analysis.

### 6.2 E01 Format

E01 is associated with the Expert Witness Format (EWF).

Common extension:

```text
evidence.E01
```

Features may include:
- Metadata.
- Compression.
- Segmentation.
- Integrity information.

### 6.3 AFF Format

AFF stands for Advanced Forensic Format.

It is designed for storing forensic data and associated metadata.

### Comparison

| Format | Description |
|---|---|
| RAW | Simple image of acquired data |
| E01 | Forensic evidence container |
| AFF | Advanced forensic image format |

---

## 7. What Is a Write Blocker?

A write blocker is a hardware or software mechanism designed to prevent write operations to a storage device during forensic access.

Its purpose is to reduce the risk of modifying the original evidence.

### Hardware Write Blocker

A physical device placed between the source storage device and the forensic workstation.

### Software Write Blocking

Software controls designed to prevent writes to a storage device.

The protection offered depends on the operating system, configuration, device, and acquisition environment.

### Why Is It Important?

Without suitable write protection, normal system activity may modify the source device.

Forensic procedures therefore use validated write-blocking methods when appropriate.

---

## 8. Forensic Imaging Tools

Common tools include:

| Tool | Purpose |
|---|---|
| FTK Imager | Forensic acquisition and image examination |
| Guymager | Disk imaging on Linux |
| `dd` | Low-level data copying |
| `dc3dd` | Forensic-oriented acquisition and hashing |
| Autopsy | Digital forensic examination |
| Sleuth Kit | File-system forensic analysis |

### Important Note About `dd`

The Linux `dd` command can copy storage-device contents, but it is not automatically a complete forensic acquisition solution.

It does not independently provide all the documentation, validation, error handling, or write protection required for a professional investigation.

Always verify the source, destination, acquisition method, and authorization before working with real storage devices.

---

## 9. Disk Imaging Safety

Before acquiring a real storage device:

1. Confirm written authorization.
2. Identify the correct source device.
3. Record device details.
4. Use appropriate write protection.
5. Confirm that the destination has enough storage.
6. Document the acquisition tool and version.
7. Record start and end times.
8. Calculate and record hashes.
9. Verify the acquired image.
10. Store the evidence securely.

**Critical warning:** Selecting the wrong source or destination can destroy data. Never experiment with disk imaging commands against your Windows system disk or an unknown device.

For this lesson, we will create a small image from a self-created practice file instead of imaging a physical disk.

---

## 10. Practical Lab: Create a Safe Forensic Image

### Objective

Create a small RAW image from a practice file, calculate its SHA-256 hash, verify it, and document the acquisition.

This exercise does not access or modify a physical disk.

### Step 1: Create the Lab Directory

```bash
mkdir -p ~/practical-labs/module14/lab05/source
cd ~/practical-labs/module14/lab05
```

### Step 2: Create a Sample Evidence File

```bash
echo "Forensic acquisition training sample" > source/evidence.txt
```

### Step 3: Inspect the Source

```bash
ls -lh source/evidence.txt
```

```bash
file source/evidence.txt
```

### Step 4: Calculate the Source Hash

```bash
sha256sum source/evidence.txt
```

Save the source hash:

```bash
sha256sum source/evidence.txt > source-hash.txt
```

### Step 5: Create a RAW-Style Copy

For this training exercise, create a byte-for-byte copy of the sample file:

```bash
cp source/evidence.txt evidence-image.raw
```

This is a file-copy demonstration, not a physical-disk acquisition.

### Step 6: Verify the Image

```bash
sha256sum evidence-image.raw
```

Compare the source and image hashes:

```bash
sha256sum source/evidence.txt evidence-image.raw
```

They should match.

### Step 7: Verify Using `cmp`

```bash
cmp source/evidence.txt evidence-image.raw
```

If the files are identical, `cmp` produces no output and exits successfully.

Check its exit status:

```bash
echo $?
```

Expected:

```text
0
```

### Step 8: Create an Acquisition Record

```bash
cat > acquisition-record.txt <<'EOF'
FORENSIC ACQUISITION RECORD

Investigation ID: ACQ-001
Evidence ID: E-005

Source:
source/evidence.txt

Acquired image:
evidence-image.raw

Acquisition method:
Local byte-for-byte file copy

Image format:
RAW-style training file

Integrity algorithm:
SHA-256

Verification:
Source and acquired image hashes compared.
Files were also compared using cmp.

Scope:
Self-created training evidence only.

Limitations:
This is a file-copy demonstration.
No physical storage device was acquired.
No hardware write blocker was used.
EOF
```

### Step 9: Create an Image Hash Record

```bash
sha256sum evidence-image.raw > image-hash.txt
```

Verify it:

```bash
sha256sum -c image-hash.txt
```

Expected:

```text
evidence-image.raw: OK
```

### Step 10: Review the Lab

```bash
find . -maxdepth 2 -type f
```

Your workspace should contain:

```text
./source/evidence.txt
./source-hash.txt
./evidence-image.raw
./acquisition-record.txt
./image-hash.txt
```

---

## 11. Practical Investigation Challenge

Answer these questions:

1. What is disk imaging?
2. What is the difference between physical and logical acquisition?
3. What is a bit-for-bit image?
4. Why is a write blocker useful?
5. Name three forensic image formats or acquisition formats.
6. Why should investigators calculate hashes before and after acquisition?
7. What is the purpose of an acquisition record?
8. Why is it dangerous to experiment with `dd` on an unknown disk?

---

## 12. Knowledge Check

**Question 1:** What is the main purpose of forensic disk imaging?

A. To install an operating system  
B. To create an acquired copy of storage data  
C. To improve network speed  
D. To delete temporary files

**Question 2:** Which acquisition type attempts to capture device-level storage contents?

A. Logical acquisition  
B. Physical acquisition  
C. Browser acquisition  
D. User authentication

**Question 3:** What is a write blocker designed to prevent?

A. Reading data  
B. Writing to the original evidence device  
C. Calculating hashes  
D. Creating reports

**Question 4:** Which is a common forensic image format?

A. E01  
B. DOCX  
C. HTML  
D. MP3

**Question 5:** What can a matching source and image SHA-256 hash support?

A. Verification of matching contents  
B. Identification of the device owner  
C. Proof of criminal activity  
D. Proof of legal authorization

---

## 13. Lesson Completion Checklist

- [ ] I understand disk imaging.
- [ ] I understand physical acquisition.
- [ ] I understand logical acquisition.
- [ ] I understand bit-for-bit imaging.
- [ ] I can identify RAW, E01, and AFF formats.
- [ ] I understand write blockers.
- [ ] I created a safe practice image.
- [ ] I calculated and verified SHA-256 hashes.
- [ ] I created an acquisition record.
- [ ] I completed the knowledge check.

---

## 14. Key Takeaway

Disk imaging allows investigators to acquire data for examination while preserving the original evidence according to an appropriate forensic procedure.

A reliable acquisition requires authorization, correct device identification, suitable write protection, documentation, and integrity verification.

**Remember: Preserve the source, examine the acquired copy, and document every important action.**
