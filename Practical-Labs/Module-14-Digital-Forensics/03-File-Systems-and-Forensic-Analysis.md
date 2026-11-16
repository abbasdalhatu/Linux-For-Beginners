
# MODULE 14 – DIGITAL FORENSICS

# LESSON 03: FILE SYSTEMS AND FORENSIC ANALYSIS

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a file system is.
2. Identify common Linux and Windows file systems.
3. Understand files, directories, metadata, and inodes.
4. Explain how file systems organize stored information.
5. Understand file-system artifacts used in investigations.
6. Use Linux commands to inspect file systems.
7. Identify suspicious file-system activity in a controlled lab.

---

## 2. What Is a File System?

A file system is the method an operating system uses to organize, store, retrieve, and manage data on a storage device.

In simple terms:

A file system is like a library organization system.

- Books represent files.
- Shelves represent directories.
- Book labels represent filenames and metadata.
- The library catalogue represents file-system structures.

Without a file system, an operating system would have difficulty locating and organizing stored information.

### Example

When you save a document named:

`assignment.docx`

The file system maintains information that helps the operating system locate and access its stored data.

---

## 3. Common File Systems

Different operating systems use different file systems.

### 3.1 FAT32

FAT32 is an older file system commonly used on removable storage devices.

Characteristics:

- Broad compatibility.
- Common on USB drives.
- Maximum individual file size of approximately 4 GB.
- Does not provide the same permission features as modern Linux file systems.

### 3.2 exFAT

exFAT is designed for flash storage and removable devices.

Characteristics:

- Supports large files.
- Common on external storage.
- Compatible with many operating systems.

### 3.3 NTFS

NTFS is a major Windows file system.

Characteristics:

- Supports file permissions.
- Supports journaling.
- Supports large files and volumes.
- Maintains file-system metadata.
- Supports alternate data streams.

Important NTFS structures include:

- Master File Table (MFT).
- File records.
- Security descriptors.
- Journaling information.

### 3.4 ext4

ext4 is a common Linux file system.

Characteristics:

- Supports journaling.
- Uses inodes.
- Supports file permissions.
- Supports large storage volumes.
- Maintains file-system metadata.

### 3.5 XFS

XFS is a high-performance journaling file system used in Linux environments.

It is commonly used for large storage systems and server workloads.

---

## 4. Understanding Files and Directories

A file is a collection of stored information.

Examples:

- `report.txt`
- `photo.jpg`
- `system.log`
- `backup.tar`

A directory is a structure used to organize files and other directories.

Example:

```text
forensics/
├── evidence/
│   ├── report.txt
│   └── system.log
├── images/
│   └── photo.jpg
└── reports/
    └── final-report.txt
```

During an investigation, directory structures may help investigators understand how information was organized.

---

## 5. Understanding File Metadata

Metadata is information that describes a file.

Examples include:

- File name.
- File size.
- File type.
- Creation timestamp, where supported.
- Modification timestamp.
- Access timestamp.
- Ownership.
- Permissions.

### Example

Consider:

`report.txt`

Its metadata might indicate:

- Name: report.txt
- Size: 2 KB
- Owner: student
- Permissions: 644
- Last modification: Recorded timestamp

Metadata can provide useful investigative context.

However, timestamps can be changed, copied, affected by system configuration, or interpreted differently across file systems. Never treat a timestamp alone as proof of who performed an action.

---

## 6. What Is an Inode?

An inode is a data structure used by many Unix-like file systems, including ext4.

It stores information about a file, such as:

- File type.
- Permissions.
- Ownership.
- File size.
- Timestamps.
- Pointers to data blocks.

An inode generally does not store the filename itself.

The filename is maintained through directory entries that associate names with inode numbers.

### Example

Run:

```bash
ls -i
```

You may see:

```text
123456 report.txt
```

The number is the inode number associated with the file.

Inspect more information:

```bash
stat report.txt
```

---

## 7. Important File-System Forensic Artifacts

Investigators examine file-system structures to understand system activity.

### 7.1 File Timestamps

Timestamps may help establish a timeline.

On Linux, common timestamp categories include:

- atime: Last access time.
- mtime: Last content modification time.
- ctime: Last inode status change time.

Important: Linux ctime is not the file creation time.

Some Linux file systems also support a birth time, but availability depends on the file system and tools.

### 7.2 Deleted Files

Deleting a file usually removes or changes the file-system reference to it.

The underlying data may remain temporarily until overwritten or otherwise reclaimed.

Recovery is not guaranteed.

### 7.3 File Permissions

Permissions indicate which users can read, write, or execute a file.

Example:

```text
-rw-r--r--
```

This means:

- Owner: Read and write.
- Group: Read.
- Others: Read.

Unexpected permission changes may be relevant during a security investigation.

### 7.4 Hidden Files

Linux filenames beginning with a dot are normally hidden from ordinary directory listings.

Example:

`.secret.txt`

Display hidden files:

```bash
ls -la
```

Hidden files are not automatically malicious. They are simply files that ordinary listings may omit.

### 7.5 NTFS Master File Table

The NTFS Master File Table (MFT) stores records describing files and directories on an NTFS volume.

MFT records can contain information such as:

- File names.
- File attributes.
- File timestamps.
- Data attributes.
- File-system relationships.

Forensic tools can examine MFT information when analyzing an NTFS image.

---

## 8. Linux Commands for File-System Investigation

These commands help inspect files and storage.

### Display mounted file systems

```bash
findmnt
```

### Display disk usage

```bash
df -h
```

### Display storage devices

```bash
lsblk -f
```

### Display file metadata

```bash
stat filename.txt
```

### Display inode numbers

```bash
ls -i
```

### Display hidden files

```bash
ls -la
```

### Identify file types

```bash
file filename.txt
```

### Find files by name

```bash
find . -type f -name "*.txt"
```

### Find files modified in the last 24 hours

```bash
find . -type f -mtime -1
```

Use these commands within your own lab directory or an authorized forensic workspace.

---

## 9. Practical Lab: File-System Investigation

### Objective

Create a small file-system investigation environment and inspect files, metadata, timestamps, permissions, and inode numbers.

### Step 1: Navigate to Your Module 14 Lab

```bash
cd ~/practical-labs/module14
```

### Step 2: Create the Lab Directory

```bash
mkdir -p lab03/evidence
cd lab03
```

### Step 3: Create Three Sample Files

```bash
echo "Confidential practice document" > evidence/document.txt

echo "System activity sample" > evidence/activity.log

echo "Forensic investigation notes" > evidence/notes.txt
```

### Step 4: List the Files

```bash
ls -lh evidence/
```

### Step 5: Display Hidden Files

Create a hidden file:

```bash
echo "Hidden practice artifact" > evidence/.hidden.txt
```

List all files:

```bash
ls -la evidence/
```

### Step 6: Inspect File Metadata

```bash
stat evidence/document.txt
```

Observe:

- File size.
- Permissions.
- Owner.
- Inode.
- Access time.
- Modification time.
- Change time.

### Step 7: Inspect Inode Numbers

```bash
ls -li evidence/
```

Record the inode numbers of your files.

### Step 8: Identify File Types

```bash
file evidence/*
```

Note: The ordinary `*` wildcard does not include hidden filenames. Inspect the hidden file separately:

```bash
file evidence/.hidden.txt
```

### Step 9: Inspect Permissions

```bash
ls -l evidence/
```

Change the permissions of one practice file:

```bash
chmod 600 evidence/document.txt
```

Inspect it again:

```bash
ls -l evidence/document.txt
```

The permissions should now show:

```text
-rw-------
```

### Step 10: Inspect File-System Storage

```bash
df -h .
```

This displays information about the file system containing your current directory.

### Step 11: Find Your Sample Files

```bash
find evidence/ -type f
```

Find text files:

```bash
find evidence/ -type f -name "*.txt"
```

### Step 12: Create an Investigation Notes File

```bash
cat > investigation-notes.txt <<'EOF'
FILE-SYSTEM INVESTIGATION

Investigation ID: FS-001
Environment: Personal Kali practice lab

Files examined:
1. document.txt
2. activity.log
3. notes.txt
4. .hidden.txt

Activities:
- Listed directory contents.
- Examined file metadata.
- Inspected inode numbers.
- Identified file types.
- Examined permissions.
- Inspected mounted file-system usage.

Scope:
Self-created training files only.

Conclusion:
The exercise demonstrates how Linux file-system
metadata and directory structures can support
a digital forensic investigation.
EOF
```

### Step 13: Review Your Results

```bash
find . -maxdepth 2 -type f -print
```

---

## 10. Practical Investigation Challenge

Answer these questions using your lab results.

1. Which command displays inode numbers?
2. Which command displays detailed file metadata?
3. What is the difference between atime and mtime?
4. Why does a hidden Linux file begin with a dot?
5. What is the purpose of the NTFS MFT?
6. What information does an inode contain?
7. Why should investigators avoid relying on a single timestamp?
8. Can a deleted file always be recovered? Explain.

---

## 11. Knowledge Check

**Question 1:** Which file system is commonly used by modern Windows installations?

A. ext4  
B. NTFS  
C. XFS  
D. ISO9660

**Question 2:** What does an inode primarily store?

A. File metadata and data-block references  
B. Only the filename  
C. Internet addresses  
D. Passwords

**Question 3:** Which command displays detailed file metadata?

A. ping  
B. stat  
C. whoami  
D. uptime

**Question 4:** What does Linux ctime represent?

A. File creation time in every case  
B. Last inode status change time  
C. Last network connection  
D. System boot time

**Question 5:** What is the NTFS MFT?

A. A network protocol  
B. A Windows firewall  
C. A structure containing file and directory records  
D. A Linux package manager

---

## 12. Lesson Completion Checklist

- [ ] I understand file systems.
- [ ] I can identify NTFS, FAT32, exFAT, ext4, and XFS.
- [ ] I understand files and directories.
- [ ] I understand file metadata.
- [ ] I understand inodes.
- [ ] I understand basic file-system forensic artifacts.
- [ ] I completed the Linux investigation lab.
- [ ] I inspected file permissions and timestamps.
- [ ] I completed the knowledge check.

---

## 13. Key Takeaway

File systems contain valuable information that can help investigators reconstruct digital activity.

Understanding metadata, timestamps, permissions, inodes, and file-system structures is essential for effective digital forensic analysis.

**Remember: A file contains information, but its file-system metadata can also provide important investigative context.**
