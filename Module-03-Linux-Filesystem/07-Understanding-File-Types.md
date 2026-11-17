---
---

# LESSON 07: UNDERSTANDING LINUX FILE TYPES

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

Linux treats many different types of resources as files.

Unlike operating systems where files are mainly documents, images, and applications, Linux also represents devices, communication channels, and other system resources using special file types.

Understanding file types helps Linux administrators manage files, investigate systems, and understand how the operating system works.

## 2. The Main Linux File Types

Linux commonly identifies seven file types:

1. Regular files
2. Directories
3. Symbolic links
4. Character device files
5. Block device files
6. Named pipes (FIFOs)
7. Sockets

Each type has a different purpose.

## 3. Regular Files

A regular file stores information or data.

Examples include:

- Text documents
- Images
- Videos
- Scripts
- Executable programs
- Configuration files

Examples:

```text
notes.txt
script.sh
image.png
config.conf
```

Create a regular file:

```bash
touch notes.txt
```

Inspect it:

```bash
ls -l notes.txt
```

Identify its type:

```bash
file notes.txt
```

In a long listing, a regular file begins with:

```text
-
```

Example:

```text
-rw-r--r-- 1 kali kali 0 Nov 14 12:00 notes.txt
```

The first character `-` indicates a regular file.

## 4. Directories

A directory stores references to files and other directories.

Example:

```text
/home/kali/Documents
```

Create a directory:

```bash
mkdir myfolder
```

Inspect it:

```bash
ls -ld myfolder
```

In a long listing, a directory begins with:

```text
d
```

Example:

```text
drwxr-xr-x 2 kali kali 4096 Nov 14 12:00 myfolder
```

The first character `d` indicates a directory.

## 5. Symbolic Links

A symbolic link, also called a symlink, is a special file that points to another file or directory.

Think of it as a shortcut.

For example:

```text
original.txt
shortcut.txt
```

The shortcut can point to the original file.

Create an example:

```bash
touch original.txt
ln -s original.txt shortcut.txt
```

Inspect the link:

```bash
ls -l shortcut.txt
```

Possible output:

```text
shortcut.txt -> original.txt
```

The arrow shows the target of the symbolic link.

In a long listing, a symbolic link begins with:

```text
l
```

## 6. Character Device Files

Character device files provide access to devices that transfer data as a stream of characters or bytes.

Examples:

```text
/dev/null
/dev/zero
/dev/tty
```

These are special interfaces provided by the operating system.

Inspect one:

```bash
ls -l /dev/null
```

In a long listing, a character device begins with:

```text
c
```

For example, `/dev/null` discards data written to it.

A simple demonstration:

```bash
echo "Hello Linux" > /dev/null
```

The text is discarded instead of being saved.

## 7. Block Device Files

Block device files represent devices that provide block-based data access, such as storage devices.

Examples may include:

```text
/dev/sda
/dev/sda1
/dev/nvme0n1
```

The exact names depend on the hardware and virtual machine configuration.

Inspect available block devices:

```bash
lsblk
```

Inspect a device entry if one exists:

```bash
ls -l /dev/sda
```

If `/dev/sda` does not exist, do not worry. Your VM may use a different device name.

In a long listing, block devices begin with:

```text
b
```

Do not write to or format storage devices during this exercise.

## 8. Named Pipes (FIFOs)

A named pipe is a special file used for communication between processes.

FIFO means **First In, First Out**.

Data written into the pipe can be read by another process in the order it was received.

Create a named pipe:

```bash
mkfifo mypipe
```

Inspect it:

```bash
ls -l mypipe
```

In a long listing, a named pipe begins with:

```text
p
```

A named pipe can connect programs without requiring them to communicate through an ordinary disk file.

## 9. Sockets

A socket is a communication endpoint used by processes to exchange information.

Sockets can support communication between processes on the same computer or across a network.

Examples of Unix domain sockets may be found under:

```text
/run
/var/run
```

Inspect sockets using:

```bash
ss -lx
```

This displays listening Unix domain sockets.

In a long listing, a socket file begins with:

```text
s
```

Sockets are important in networking, system services, and cybersecurity investigations.

## 10. Understanding the First Character in `ls -l`

The first character in a detailed directory listing identifies the file type.

| Character | File Type |
|---|---|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `c` | Character device |
| `b` | Block device |
| `p` | Named pipe |
| `s` | Socket |

Example:

```text
-rw-r--r--  regular file
drwxr-xr-x  directory
lrwxrwxrwx  symbolic link
crw-rw-rw-  character device
brw-rw----  block device
prw-r--r--  named pipe
srwxr-xr-x  socket
```

The remaining characters represent permissions and other metadata.

We will study permissions in detail in Module 07.

## 11. Using `stat` to Inspect Files

The `stat` command displays detailed information about a file or directory.

Example:

```bash
stat notes.txt
```

It can display:

- File type
- File size
- Permissions
- Inode number
- Ownership
- Access time
- Modification time
- Change time

This is useful when investigating files and understanding their metadata.

## 12. Why File Types Matter in Cybersecurity

Understanding file types helps security professionals:

1. Identify suspicious files.
2. Understand symbolic links and their targets.
3. Investigate device interfaces.
4. Understand communication between processes.
5. Inspect storage devices.
6. Analyze file metadata.

For example, a file named `image.jpg` may not actually contain image data.

You can investigate its actual format with:

```bash
file image.jpg
```

Never assume that a file is safe simply because its name or extension looks harmless.

## 13. Practical Laboratory

Perform these exercises inside Kali Linux.

### Exercise 1: Create a Regular File

```bash
mkdir -p ~/filesystem-lab
cd ~/filesystem-lab
touch notes.txt
ls -l notes.txt
file notes.txt
```

Identify the file type.

### Exercise 2: Create a Directory

```bash
mkdir documents
ls -ld documents
```

Identify the first character in the output.

### Exercise 3: Create a Symbolic Link

```bash
ln -s notes.txt notes-link
ls -l notes-link
```

Identify the link target.

### Exercise 4: Inspect a Character Device

```bash
ls -l /dev/null
```

Identify its file type.

### Exercise 5: Inspect Block Devices

```bash
lsblk
```

Identify the storage devices available to your Kali VM.

### Exercise 6: Create a Named Pipe

```bash
mkfifo mypipe
ls -l mypipe
```

Identify the first character in the output.

### Exercise 7: Inspect Unix Sockets

```bash
ss -lx
```

Observe the available Unix domain sockets.

### Exercise 8: Inspect File Metadata

```bash
stat notes.txt
```

Identify the file size, permissions, inode, and timestamps.

### Exercise 9: Clean Up Your Laboratory

After completing the exercises, remove only the files and directories created for this lab:

```bash
cd ~
rm -r filesystem-lab
```

Make sure you are using the exact laboratory directory before executing the removal command.

## 14. Knowledge Check

1. How many common Linux file types did we study?
2. What character represents a regular file?
3. What character represents a directory?
4. What is a symbolic link?
5. What is the purpose of `/dev/null`?
6. What are block device files used for?
7. What does FIFO mean?
8. What is a Unix socket?
9. Which command identifies the actual type of a file?
10. What information does the `stat` command display?

## 15. Lesson Summary

Linux supports several file types, including regular files, directories, symbolic links, character devices, block devices, named pipes, and sockets.

We learned how to identify these types using `ls -l`, `file`, `stat`, `lsblk`, and `ss`.

Understanding file types is important for system administration, file investigation, and cybersecurity.

**Next Lesson:** Hidden Files and Directories.