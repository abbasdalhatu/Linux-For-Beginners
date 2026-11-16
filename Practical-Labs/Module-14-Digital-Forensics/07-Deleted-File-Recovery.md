
# MODULE 14 – DIGITAL FORENSICS

# LESSON 07: DELETED FILE RECOVERY

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what happens when a file is deleted.
2. Understand allocated and unallocated storage.
3. Explain file recovery and data carving.
4. Identify factors that affect recovery success.
5. Understand the difference between HDD and SSD deletion behavior.
6. Identify common forensic recovery tools.
7. Perform a safe file-recovery simulation.
8. Document recovery findings.

---

## 2. What Happens When a File Is Deleted?

Deleting a file does not always immediately erase its contents from a storage device.

In many file systems, deletion initially removes or changes the references that allow the operating system to locate the file.

The storage space may then be marked as available for reuse.

### Simple Example

Imagine a library catalogue containing:

```text
Book A → Shelf 1
Book B → Shelf 2
Book C → Shelf 3
```

If Book B is removed from the catalogue, its physical contents may remain until the shelf is reused or the book is destroyed.

File deletion can work in a similar way.

The operating system may remove the file's directory entry or mark its storage as available, while some underlying data remains.

However, recovery is not guaranteed.

---

## 3. Allocated vs Unallocated Space

### 3.1 Allocated Space

Allocated space refers to storage currently assigned to files or file-system structures.

Example:

```text
document.txt
photo.jpg
report.pdf
```

These files occupy allocated storage.

### 3.2 Unallocated Space

Unallocated space refers to storage that the file system considers available for reuse.

It may contain remnants of previously deleted files.

A forensic investigator may examine unallocated space for recoverable information.

### Important

Unallocated space does not necessarily contain recoverable files. Data may have been overwritten, trimmed, encrypted, or otherwise made unavailable.

---

## 4. How File Recovery Works

File recovery attempts to retrieve data that is no longer normally accessible through the file system.

There are several approaches.

### 4.1 File-System-Based Recovery

Uses file-system structures and metadata to locate deleted files.

It may recover:
- Original filenames.
- Directory information.
- File timestamps.
- File contents.

Success depends on whether the relevant metadata remains available.

### 4.2 File Carving

File carving searches storage data for recognizable file signatures.

For example:

- JPEG signatures.
- PNG signatures.
- PDF signatures.
- ZIP signatures.

It may recover files without relying on the original directory structure.

However, carved files may lose:
- Original filenames.
- Directory paths.
- Original timestamps.
- Some file metadata.

Fragmented files may also be difficult to reconstruct accurately.

### 4.3 Backup-Based Recovery

Files may be restored from:
- Backups.
- Snapshots.
- Cloud version history.
- System restore mechanisms.

Backup recovery is often more reliable than attempting to recover overwritten data.

---

## 5. HDD vs SSD Recovery

The type of storage device affects recovery possibilities.

### Hard Disk Drive (HDD)

Traditional HDDs use magnetic storage.

After deletion, data may remain physically present until overwritten.

Recovery may therefore be possible under suitable conditions.

### Solid State Drive (SSD)

SSDs use flash memory and may support TRIM.

TRIM allows an operating system to notify an SSD that certain storage blocks are no longer needed.

The SSD may later erase or reclaim those blocks.

This can make deleted-file recovery more difficult or impossible.

### Comparison

| Feature | HDD | SSD |
|---|---|---|
| Storage technology | Magnetic | Flash memory |
| Deleted data | May remain until overwritten | May be affected by TRIM |
| Recovery possibility | Sometimes possible | Often more difficult |
| Performance | Generally slower | Generally faster |

**Important:** Do not continue using a device containing important deleted evidence. New activity may overwrite data or trigger storage cleanup. Preserve the device and follow an approved forensic acquisition procedure.

---

## 6. Common File-Recovery Tools

| Tool | Purpose |
|---|---|
| TestDisk | Partition and file-system recovery |
| PhotoRec | File carving and recovery |
| Sleuth Kit | File-system forensic analysis |
| Autopsy | Digital forensic examination |
| Scalpel | File carving |
| Foremost | File carving |

These tools should be used on authorized evidence or copies, never against unknown devices.

---

## 7. What Is PhotoRec?

PhotoRec is a data recovery utility designed to recover files using file signatures.

It can recognize formats such as:
- JPEG.
- PNG.
- PDF.
- ZIP.
- Other supported file types.

It does not depend entirely on the original file-system metadata.

This means recovered files may have generic filenames and may not retain their original folder structure.

---

## 8. What Is TestDisk?

TestDisk is a recovery utility that can help with:

- Lost partition recovery.
- Partition table repair.
- Certain file-system recovery operations.
- Recovering deleted files on supported file systems.

TestDisk and PhotoRec serve different purposes.

TestDisk focuses on partition and file-system structures, while PhotoRec focuses on recovering file contents through carving.

---

## 9. Important Recovery Limitations

File recovery may fail because of:

1. Data overwritten by new files.
2. SSD TRIM operations.
3. Storage-device damage.
4. Encryption.
5. File fragmentation.
6. Corrupted file-system structures.
7. Incomplete acquisition.
8. Unsupported file formats.

A forensic investigator must report recovery limitations rather than claiming that every deleted file can be restored.

---

## 10. Practical Lab: Safe Deleted-File Recovery Simulation

### Objective

Simulate deletion, understand file-system behavior, and practice recovery from a controlled copy.

This exercise uses a self-created file and a preserved backup copy. It does not attempt raw-device recovery.

### Step 1: Create a Lab Directory

```bash
mkdir -p ~/practical-labs/module14/lab07/evidence
cd ~/practical-labs/module14/lab07
```

### Step 2: Create a Sample File

```bash
echo "This is a digital forensics recovery exercise." > evidence/original.txt
```

### Step 3: Create a Preserved Backup

```bash
cp evidence/original.txt preserved-copy.txt
```

### Step 4: Calculate the Original Hash

```bash
sha256sum evidence/original.txt preserved-copy.txt
```

The hashes should match.

### Step 5: Record the File Metadata

```bash
stat evidence/original.txt
```

### Step 6: Simulate Deletion

```bash
rm evidence/original.txt
```

Confirm that the file is no longer in the evidence directory:

```bash
ls -la evidence/
```

### Step 7: Recover From the Preserved Copy

```bash
cp preserved-copy.txt recovered-file.txt
```

This is a backup-based recovery demonstration, not recovery from unallocated disk space.

### Step 8: Verify the Recovered File

```bash
cat recovered-file.txt
```

### Step 9: Compare Hashes

```bash
sha256sum preserved-copy.txt recovered-file.txt
```

The hashes should match.

### Step 10: Create a Recovery Report

```bash
cat > recovery-report.txt <<'EOF'
DIGITAL FILE RECOVERY REPORT

Investigation ID: REC-001

Original file:
evidence/original.txt

Recovery source:
preserved-copy.txt

Recovered file:
recovered-file.txt

Recovery method:
Preserved backup copy

Integrity verification:
SHA-256 hashes compared.

Result:
The recovered file matches the preserved copy.

Limitations:
This was a backup-based recovery simulation.
No raw-device recovery or file carving was performed.

Scope:
Self-created training files only.
EOF
```

### Step 11: Review Your Lab

```bash
find . -maxdepth 2 -type f
```

Expected:

```text
./preserved-copy.txt
./recovered-file.txt
./recovery-report.txt
```

---

## 11. Optional Tool Familiarization

Check whether PhotoRec is installed:

```bash
photorec --version
```

If unavailable, install it through Kali's official repositories:

```bash
sudo apt update
sudo apt install testdisk
```

Check again:

```bash
photorec --version
```

You do not need to run PhotoRec against a physical device for this lesson.

---

## 12. Practical Investigation Challenge

Answer these questions:

1. What happens when a file is deleted?
2. What is allocated space?
3. What is unallocated space?
4. Explain file carving.
5. Why can PhotoRec recover files without original filenames?
6. How can TRIM affect SSD recovery?
7. What is the difference between TestDisk and PhotoRec?
8. Why should investigators avoid writing new data to a device containing deleted evidence?

---

## 13. Knowledge Check

**Question 1:** Where might remnants of deleted files remain?

A. Unallocated space  
B. CPU cache only  
C. Network gateway  
D. DNS server

**Question 2:** Which tool is commonly used for file carving?

A. PhotoRec  
B. UFW  
C. Wireshark  
D. Nano

**Question 3:** What can TRIM do on an SSD?

A. Notify the device that blocks are no longer needed  
B. Encrypt every file  
C. Recover every deleted file  
D. Create a partition table

**Question 4:** Which recovery approach searches for recognizable file signatures?

A. File carving  
B. Password spraying  
C. Port scanning  
D. Packet routing

**Question 5:** What should an investigator do when important deleted evidence is discovered?

A. Continue using the device normally  
B. Preserve it and follow forensic acquisition procedures  
C. Install unnecessary applications  
D. Format the device

---

## 14. Lesson Completion Checklist

- [ ] I understand file deletion.
- [ ] I understand allocated and unallocated space.
- [ ] I understand file carving.
- [ ] I understand HDD and SSD recovery differences.
- [ ] I know the purposes of PhotoRec and TestDisk.
- [ ] I completed the recovery simulation.
- [ ] I verified the recovered file with SHA-256.
- [ ] I created a recovery report.
- [ ] I completed the knowledge check.

---

## 15. Key Takeaway

Deleted does not always mean permanently erased. Depending on the file system, storage technology, and subsequent activity, some data may remain recoverable.

**Remember: Preserve the storage device first. Recovery attempts should be performed on an appropriate forensic copy whenever possible.**
