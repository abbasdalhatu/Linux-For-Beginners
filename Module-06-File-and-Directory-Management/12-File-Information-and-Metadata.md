# Module 06 – File and Directory Management

## Lesson 12: File Information and Metadata

### 1. Learning Objectives

By the end of this lesson, you should be able to:

- Understand what file metadata means.
- Display file information using `stat`.
- Identify file types using `file`.
- Understand file sizes and timestamps.
- View file permissions and ownership.
- Interpret important metadata fields.
- Explain how metadata supports cybersecurity investigations.

### 2. Introduction to File Metadata

Every file in Linux has information associated with it.

This information is called **metadata**.

Metadata describes a file rather than the actual contents of the file.

For example, imagine you have a file called:

```text
security-report.txt
```

Its metadata may include:

- Filename.
- File type.
- File size.
- Owner.
- Group.
- Permissions.
- Last access time.
- Last modification time.
- Last metadata change time.

Metadata helps Linux manage files and helps administrators understand their properties.

### 3. Understanding the `stat` Command

The `stat` command displays detailed information about a file or directory.

Syntax:

```bash
stat filename
```

Example:

```bash
stat security-report.txt
```

Possible output:

```text
  File: security-report.txt
  Size: 1024       Blocks: 8
  IO Block: 4096   regular file
Device: ...        Inode: 123456
Access: (0644/-rw-r--r--)  Uid: (1000/user) Gid: (1000/user)
Access: 2026-11-14 10:00:00
Modify: 2026-11-14 10:05:00
Change: 2026-11-14 10:05:00
 Birth: 2026-11-14 09:55:00
```

The actual values will depend on your system.

### 4. Understanding Important `stat` Fields

| Field | Meaning |
|---|---|
| File | Name of the file. |
| Size | File size in bytes. |
| Blocks | Storage blocks allocated to the file. |
| IO Block | Preferred filesystem I/O block size. |
| Inode | Filesystem identifier for the file object. |
| Access | Last access timestamp. |
| Modify | Last content modification timestamp. |
| Change | Last metadata/status change timestamp. |
| Birth | File creation time, when supported. |

### 5. Understanding File Timestamps

Linux commonly tracks three important timestamps.

#### A. Access Time (atime)

Records when file contents were last accessed.

Example:

```bash
stat security-report.txt
```

Look for the `Access` timestamp.

#### B. Modification Time (mtime)

Records when the file's contents were last modified.

For example, editing a text file normally updates its modification time.

#### C. Change Time (ctime)

Records when the file's metadata or status last changed.

Examples include:

- Changing file permissions.
- Changing ownership.
- Modifying file contents.

**Important:** `ctime` does not mean creation time.

Some Linux filesystems also expose a birth or creation timestamp, but support varies.

### 6. Understanding File Types with `file`

The `file` command identifies the type of a file by examining its contents and structure.

Syntax:

```bash
file filename
```

Example:

```bash
file security-report.txt
```

Possible output:

```text
security-report.txt: ASCII text
```

Other examples:

```bash
file image.png
file document.pdf
file script.sh
```

The output may identify images, PDF documents, shell scripts, executables, and other formats.

**Important:** A filename extension does not guarantee the actual file type.

For example, a malicious executable could be renamed to `photo.jpg`. The `file` command can help reveal its actual format.

### 7. Understanding File Sizes with `du`

The `du` command means disk usage.

It displays how much storage a file or directory occupies.

Example:

```bash
du security-report.txt
```

For human-readable output:

```bash
du -h security-report.txt
```

To inspect a directory:

```bash
du -sh Documents/
```

Where:

- `-h` displays human-readable units.
- `-s` displays a summary.

### 8. Understanding File Sizes with `ls`

You can also use `ls` to display file sizes.

Example:

```bash
ls -lh
```

This displays:

- File permissions.
- Number of hard links.
- Owner.
- Group.
- File size.
- Modification time.
- Filename.

Example output:

```text
-rw-r--r-- 1 user user 2.0K Nov 14 10:00 security-report.txt
```

### 9. Understanding Inodes

An inode is a filesystem data structure that stores information about a file.

It contains metadata such as:

- File permissions.
- Ownership.
- File size.
- Timestamps.
- References to the file's data blocks.

A filename points to an inode through a directory entry.

To display an inode number:

```bash
ls -i security-report.txt
```

To display detailed inode information:

```bash
stat security-report.txt
```

**Important:** The inode number identifies a filesystem object within a filesystem. It is not necessarily a globally unique identifier.

### 10. Useful Metadata Commands

| Command | Purpose |
|---|---|
| `stat` | Displays detailed metadata. |
| `file` | Identifies file type. |
| `ls -l` | Displays file information. |
| `ls -lh` | Displays human-readable sizes. |
| `ls -i` | Displays inode numbers. |
| `du -h` | Displays human-readable disk usage. |
| `du -sh` | Displays total directory usage. |

### 11. Practical Linux Lab

We will create sample files and inspect their metadata.

#### Step 1: Create the practice directory

```bash
mkdir -p ~/module06-lab/metadata
cd ~/module06-lab/metadata
```

#### Step 2: Create a sample text file

```bash
echo "Linux metadata investigation practice." > report.txt
```

#### Step 3: Display detailed metadata

```bash
stat report.txt
```

Identify:

- File size.
- Inode number.
- Access time.
- Modification time.
- Change time.

#### Step 4: Identify the file type

```bash
file report.txt
```

You should see that it is a text file.

#### Step 5: Inspect file information

```bash
ls -l report.txt
```

Then:

```bash
ls -lh report.txt
```

Compare the outputs.

#### Step 6: Display the inode number

```bash
ls -i report.txt
```

Record the inode number.

#### Step 7: Inspect disk usage

```bash
du -h report.txt
```

Then:

```bash
du -sh .
```

The second command displays the total disk usage of your practice directory.

#### Step 8: Create another file type

Create a shell script:

```bash
echo '#!/bin/bash' > sample.sh
echo 'echo "Metadata practice"' >> sample.sh
```

Identify its type:

```bash
file sample.sh
```

#### Step 9: Change file permissions

```bash
chmod 600 report.txt
```

Inspect the metadata again:

```bash
stat report.txt
```

Notice that the permissions have changed.

#### Step 10: Compare timestamps

Inspect the file:

```bash
stat report.txt
```

Then append new content:

```bash
echo "New investigation note." >> report.txt
```

Inspect it again:

```bash
stat report.txt
```

Observe how the modification and change timestamps update.

### 12. Cybersecurity Applications

File metadata is important in cybersecurity and digital forensics.

**A. Digital Forensics**

Investigators examine timestamps to help reconstruct file activity.

**B. Malware Analysis**

The `file` command can help identify suspicious files whose extensions do not match their actual formats.

**C. Incident Response**

Administrators inspect file ownership, permissions, and modification times when investigating suspicious changes.

**D. Storage Investigation**

The `du` command helps identify directories consuming large amounts of storage.

**E. Evidence Handling**

Investigators record metadata to document the condition and properties of collected files.

**Important forensic warning:** Accessing files can sometimes update access timestamps, depending on filesystem mount options and system behaviour. Metadata can also be altered. Never treat timestamps alone as conclusive proof of an event.

### 13. Common Mistakes

| Mistake | Explanation |
|---|---|
| Confusing ctime with creation time | ctime means metadata/status change time. |
| Trusting file extensions | Extensions can be changed without changing file contents. |
| Confusing file size with disk usage | Filesystem allocation and metadata affect actual storage usage. |
| Assuming all filesystems support birth time | Creation-time reporting depends on filesystem support. |
| Treating timestamps as perfect evidence | Timestamps can be modified or affected by system behaviour. |

### 14. Knowledge Check

Answer these questions:

1. What is file metadata?
2. Which command displays detailed file metadata?
3. What does the `file` command do?
4. What does inode mean?
5. What is the difference between atime, mtime, and ctime?
6. Does ctime mean creation time?
7. Which command displays human-readable file sizes using `ls`?
8. What does `du -sh` do?
9. Why should cybersecurity analysts not rely only on file extensions?
10. How can metadata help during a digital forensic investigation?

### 15. Lesson Summary

In this lesson, you learned:

- Metadata describes a file.
- `stat` displays detailed metadata.
- `file` identifies the actual file type.
- `ls -lh` displays file sizes in readable units.
- `du -sh` displays directory disk usage.
- Inodes identify filesystem objects.
- atime, mtime, and ctime represent different timestamps.
- Metadata is useful in digital forensics and security investigations.

**Next Lesson:** Lesson 13 – Hard Links and Symbolic Links.