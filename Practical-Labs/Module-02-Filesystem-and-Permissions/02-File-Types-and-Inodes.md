
# PRACTICAL LABS
# MODULE 02 – FILESYSTEM AND PERMISSION LABS

## LAB 02: FILE TYPES AND INODES

### 1. Introduction

Linux treats almost everything as a file, but not every file stores ordinary text or documents.

A Linux filesystem contains different file types, including:

- Regular files
- Directories
- Symbolic links
- Character device files
- Block device files
- Named pipes
- Unix domain sockets

Linux also uses a structure called an **inode** to store information about files and directories.

Understanding file types and inodes is important for system administration, digital forensics, and cybersecurity investigations.

### 2. Learning Objectives

By the end of this practical, you should be able to:

1. Identify common Linux file types.
2. Interpret file-type indicators in `ls -l`.
3. Use the `file` command.
4. Understand what an inode represents.
5. Inspect inode numbers using `ls -i` and `stat`.
6. Understand hard links and symbolic links.
7. Identify unusual files during a basic security investigation.

### 3. Understanding Linux File Types

Linux identifies file types using metadata.

Run:

```bash
ls -l /
```

Example output:

```text
drwxr-xr-x  2 root root 4096 bin
drwxr-xr-x  3 root root 4096 boot
-rw-r--r--  1 root root  220 example.txt
lrwxrwxrwx  1 root root    7 lib -> usr/lib
```

Look at the first character in each permission field.

It indicates the file type.

### 4. The Seven Common File Types

#### A. Regular Files (`-`)

Regular files store information such as text, images, scripts, documents, and programs.

Create one:

```bash
mkdir -p ~/practical-labs/module02/lab02
cd ~/practical-labs/module02/lab02

touch normal-file.txt
echo "This is a regular file." > normal-file.txt
```

Inspect it:

```bash
ls -l normal-file.txt
file normal-file.txt
```

The first character in the `ls -l` output should be `-`.

#### B. Directories (`d`)

Directories organize files and other directories.

Create one:

```bash
mkdir documents
```

Inspect it:

```bash
ls -ld documents
file documents
```

The first character should be `d`.

#### C. Symbolic Links (`l`)

A symbolic link is a special file that points to another file or directory by its path.

Create one:

```bash
ln -s normal-file.txt shortcut.txt
```

Inspect it:

```bash
ls -l shortcut.txt
file shortcut.txt
```

Expected pattern:

```text
shortcut.txt -> normal-file.txt
```

Read through the link:

```bash
cat shortcut.txt
```

#### D. Character Device Files (`c`)

Character devices transfer data as a stream of characters or bytes.

Examples include terminal interfaces and certain device interfaces.

Inspect:

```bash
ls -l /dev/null
ls -l /dev/tty
```

The first character is commonly `c`.

Do not write to unfamiliar device files.

#### E. Block Device Files (`b`)

Block devices transfer data in blocks and commonly represent storage devices.

Examples include disks and partitions.

Inspect:

```bash
ls -l /dev/sda
ls -l /dev/loop0
```

Some of these paths may not exist in your VM. That is normal.

Use this to inspect available block devices:

```bash
lsblk
```

The `TYPE` column helps identify disks and partitions.

#### F. Named Pipes (`p`)

A named pipe, also called a FIFO, allows processes to exchange data.

Create one:

```bash
mkfifo communication.pipe
```

Inspect:

```bash
ls -l communication.pipe
file communication.pipe
```

The first character should be `p`.

You can test communication between two terminals.

Terminal 1:

```bash
cat communication.pipe
```

Terminal 2:

```bash
echo "Message from another process" > ~/practical-labs/module02/lab02/communication.pipe
```

Terminal 1 should display the message.

Stop the waiting command with `Ctrl+C` after the message arrives.

#### G. Unix Domain Sockets (`s`)

Sockets allow processes on the same system to communicate.

They are used by many Linux services.

Inspect available socket files:

```bash
find /run -type s 2>/dev/null | head -20
```

The first character in a long listing of a socket is `s`.

### 5. File Type Reference Table

| First Character | File Type | Description |
|---|---|---|
| `-` | Regular file | Stores ordinary file data |
| `d` | Directory | Organizes filesystem entries |
| `l` | Symbolic link | Points to another path |
| `c` | Character device | Character-based device interface |
| `b` | Block device | Block-based storage interface |
| `p` | Named pipe | Inter-process communication |
| `s` | Socket | Communication endpoint |

### 6. Understanding Inodes

An inode is a filesystem data structure that stores metadata about a file.

An inode commonly stores:

- File type
- Permissions
- Owner user ID
- Owner group ID
- File size
- Timestamps
- Link count
- Pointers or references to the file's data blocks

An inode does not normally store the filename itself.

The directory stores the relationship between a filename and its inode number.

Think of it this way:

- Filename: The name you see.
- Inode: The filesystem's record for the file.
- Data blocks: Where the file's contents are stored.

### 7. Inspecting Inode Numbers

Create two files:

```bash
touch first.txt
touch second.txt
```

Display their inode numbers:

```bash
ls -i
```

You may see output similar to:

```text
123456 first.txt
123457 second.txt
```

Your actual numbers will be different.

Inspect one file:

```bash
stat first.txt
```

Look for:

```text
File:
Size:
Inode:
Links:
Access:
Modify:
Change:
```

The inode number identifies the file's inode within its filesystem.

### 8. Understanding Hard Links

A hard link is another directory entry that points to the same inode as an existing file.

Create one:

```bash
ln first.txt first-hardlink.txt
```

Compare inode numbers:

```bash
ls -i first.txt first-hardlink.txt
```

Both should show the same inode number.

Check the link count:

```bash
stat first.txt
```

The link count should increase.

Now add content through the hard link:

```bash
echo "Hard links share the same underlying file." >> first-hardlink.txt
```

Read the original:

```bash
cat first.txt
```

The new content should appear because both names refer to the same underlying file.

Important characteristics:

- Hard links share an inode.
- They refer to the same underlying file data.
- Removing one filename does not automatically remove the data while another hard link remains.
- Hard links generally cannot cross filesystem boundaries.
- Creating hard links to directories is normally restricted.

### 9. Understanding Symbolic Links

A symbolic link points to another pathname.

Create one:

```bash
ln -s first.txt first-symbolic-link.txt
```

Inspect:

```bash
ls -l first-symbolic-link.txt
stat first-symbolic-link.txt
stat -L first-symbolic-link.txt
```

The ordinary `stat` command reports information about the symbolic link itself.

The `-L` option follows the link and reports information about its target.

Compare inode numbers:

```bash
ls -i first.txt first-symbolic-link.txt
```

They should normally be different.

Important characteristics:

- Symbolic links have their own inode.
- They store a target pathname.
- They can point to directories.
- They can cross filesystem boundaries.
- A link can become broken if its target is removed or moved.

### 10. Hard Links vs Symbolic Links

| Feature | Hard Link | Symbolic Link |
|---|---|---|
| Inode | Same as target | Separate inode |
| Points to | Same underlying file | Target pathname |
| Cross filesystem | Generally no | Yes |
| Can point to directory | Normally restricted | Yes |
| Can become broken | Not because another name is removed | Yes |
| Link creation | `ln source link` | `ln -s source link` |

### 11. Practical Investigation

Create a small investigation directory:

```bash
mkdir -p ~/practical-labs/module02/lab02/investigation
cd ~/practical-labs/module02/lab02/investigation
```

Create different file types:

```bash
touch report.txt
mkdir evidence
ln -s report.txt report-link.txt
mkfifo evidence.pipe
```

Inspect them:

```bash
ls -lah
file report.txt evidence report-link.txt evidence.pipe
```

Display inode numbers:

```bash
ls -i
```

Display detailed metadata:

```bash
stat report.txt
stat report-link.txt
stat evidence
```

### 12. Security Investigation: Identifying Unusual File Types

During a Linux security investigation, file-type information can help identify unexpected objects.

Examples:

- Unexpected symbolic links may redirect users or applications to unintended locations.
- Unusual named pipes may be used for inter-process communication.
- Unexpected sockets may indicate unfamiliar local services.
- Unusual executable files may require further investigation.

These observations alone do not prove malicious activity.

For a basic inventory, run:

```bash
find ~/practical-labs/module02/lab02 -type f
find ~/practical-labs/module02/lab02 -type d
find ~/practical-labs/module02/lab02 -type l
find ~/practical-labs/module02/lab02 -type p
find ~/practical-labs/module02/lab02 -type s
```

These commands search your dedicated practice directory for each object type.

### 13. Generate a File and Inode Report

Run:

```bash
cd ~/practical-labs/module02/lab02

{
    echo "FILE TYPES AND INODES REPORT"
    echo "============================"
    echo "Date: $(date)"
    echo "User: $(whoami)"
    echo
    echo "FILESYSTEM OBJECTS:"
    ls -lah
    echo
    echo "INODE NUMBERS:"
    ls -i
    echo
    echo "REGULAR FILES:"
    find . -type f
    echo
    echo "DIRECTORIES:"
    find . -type d
    echo
    echo "SYMBOLIC LINKS:"
    find . -type l
    echo
    echo "NAMED PIPES:"
    find . -type p
    echo
    echo "DETAILED METADATA:"
    stat normal-file.txt
} > file-inode-report.txt
```

Read the report:

```bash
cat file-inode-report.txt
```

### 14. Knowledge Check

1. What are the seven common Linux file types?
2. Which character represents a regular file in `ls -l`?
3. What is an inode?
4. Does an inode normally store the filename?
5. Which command displays inode numbers?
6. What is the difference between a hard link and a symbolic link?
7. Can a symbolic link become broken?
8. What does the link count represent?
9. Which command displays detailed file metadata?
10. Why are file types useful during security investigations?

### 15. Lab Completion Checklist

- [ ] Created a regular file.
- [ ] Created a directory.
- [ ] Created a symbolic link.
- [ ] Inspected character and block device examples.
- [ ] Created and tested a named pipe.
- [ ] Inspected Unix domain sockets.
- [ ] Displayed inode numbers.
- [ ] Inspected metadata using `stat`.
- [ ] Created and compared hard links.
- [ ] Compared hard links and symbolic links.
- [ ] Generated `file-inode-report.txt`.

### 16. Expected Workspace

```text
~/practical-labs/module02/lab02/
├── normal-file.txt
├── documents/
├── shortcut.txt
├── communication.pipe
├── first.txt
├── first-hardlink.txt
├── first-symbolic-link.txt
├── second.txt
├── investigation/
│   ├── report.txt
│   ├── evidence/
│   ├── report-link.txt
│   └── evidence.pipe
└── file-inode-report.txt
```

**Lab 02 Status:** Complete when the exercises and checklist are finished.

**Next Lab:** Lab 03 – File Permissions Fundamentals.
