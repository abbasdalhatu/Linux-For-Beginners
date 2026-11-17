---
---

# MODULE 06 – FILE AND DIRECTORY MANAGEMENT

## LESSON 02: UNDERSTANDING LINUX FILE TYPES

### 1. Introduction

Linux treats many resources as files. This is one of the fundamental ideas behind the Linux operating system.

Unlike Windows, where users mainly interact with regular files, folders, and device interfaces, Linux represents different types of resources through a common filesystem interface.

Understanding file types helps you:

- Identify what a file represents.
- Understand how Linux organises system resources.
- Recognise executable files and symbolic links.
- Investigate suspicious files.
- Understand how devices interact with the operating system.

Linux has several important file types.

### 2. The Main Linux File Types

Linux commonly uses seven file types:

| Symbol | File Type | Purpose |
|---|---|---|
| `-` | Regular file | Stores ordinary data |
| `d` | Directory | Organises files and directories |
| `l` | Symbolic link | Points to another file or directory |
| `c` | Character device | Handles data as a stream of characters |
| `b` | Block device | Handles data in blocks |
| `p` | Named pipe (FIFO) | Allows communication between processes |
| `s` | Socket | Enables communication between processes |

The first character displayed by `ls -l` identifies the file type.

### 3. Regular Files (`-`)

A regular file stores ordinary information.

Examples include:

```text
notes.txt
report.pdf
script.sh
image.png
access.log
```

Regular files can contain:

- Text
- Images
- Videos
- Programs
- Configuration data
- Logs

Example:

```bash
touch notes.txt
ls -l notes.txt
```

You may see output similar to:

```text
-rw-r--r-- 1 kali kali 0 Nov 14 10:00 notes.txt
```

The first character `-` identifies a regular file.

### 4. Directories (`d`)

A directory is a special file type used to organise files and other directories.

Example:

```bash
mkdir documents
ls -ld documents
```

Possible output:

```text
drwxr-xr-x 2 kali kali 4096 Nov 14 10:00 documents
```

The first character `d` identifies a directory.

Remember:

A directory is not simply a regular file containing documents. It is a filesystem structure that associates names with filesystem objects.

### 5. Symbolic Links (`l`)

A symbolic link, also called a symlink, is a special file that points to another file or directory.

Think of it as a shortcut.

Example:

```bash
touch original.txt
ln -s original.txt shortcut.txt
```

Display the result:

```bash
ls -l
```

Possible output:

```text
lrwxrwxrwx 1 kali kali 12 Nov 14 10:00 shortcut.txt -> original.txt
```

The first character `l` identifies a symbolic link.

The arrow `->` shows the target.

Important points:

- A symbolic link has its own filename.
- It points to another filesystem path.
- It can point to a directory.
- It can become broken if its target is removed or moved.

### 6. Character Device Files (`c`)

Character devices handle data as a stream of characters or bytes.

Examples include certain terminal and serial interfaces.

Linux device files are commonly found inside:

```text
/dev
```

For example:

```bash
ls -l /dev/tty
```

The first character may be `c`, identifying a character device.

Character devices are commonly used for:

- Terminal communication
- Serial devices
- Input and output interfaces

### 7. Block Device Files (`b`)

Block devices transfer data in blocks and are commonly associated with storage devices.

Examples include:

- Hard disk drives
- Solid-state drives
- USB storage devices
- Virtual disks

Inspect block devices with:

```bash
ls -l /dev
```

A more focused command is:

```bash
lsblk
```

The `lsblk` command displays information about block devices.

Example:

```text
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda      8:0    0   50G  0 disk
├─sda1   8:1    0   49G  0 part /
└─sda2   8:2    0    1G  0 part [SWAP]
```

The exact output depends on your virtual machine.

**Safety:** Do not format, partition, or modify storage devices just to practise identifying block devices.

### 8. Named Pipes (`p`)

A named pipe, also called FIFO, allows processes to communicate through a filesystem object.

FIFO means First In, First Out.

One process can write data into the pipe while another process reads it.

Create a named pipe:

```bash
mkfifo mypipe
```

Inspect it:

```bash
ls -l mypipe
```

The first character should be:

```text
p
```

Named pipes are useful for communication between programs.

They differ from ordinary files because they provide a communication channel rather than permanent storage of their contents.

### 9. Sockets (`s`)

A socket is a communication endpoint used by processes to exchange data.

Sockets can support communication:

- Between processes on the same computer.
- Between computers over a network.

Linux uses sockets for many services and applications.

You can inspect Unix domain sockets with:

```bash
ss -xl
```

You may also encounter socket files inside directories such as:

```text
/run
/var/run
```

A socket file is identified by the symbol `s` in long-format directory listings.

### 10. How to Identify File Types

Linux provides several commands for identifying file types.

**A. Using `ls -l`**

```bash
ls -l
```

The first character identifies the filesystem object type.

**B. Using `file`**

```bash
file notes.txt
```

This command examines a file and reports its detected format or type.

Example:

```text
notes.txt: ASCII text
```

The `file` command is especially useful because a filename extension does not always accurately describe the file's actual contents.

**C. Using `stat`**

```bash
stat notes.txt
```

This displays detailed information about a filesystem object, including its type, size, permissions, and timestamps.

### 11. File Extensions vs File Types

A file extension is the part of a filename that follows the final dot.

Examples:

```text
report.pdf
image.jpg
script.sh
notes.txt
```

However, Linux does not rely entirely on extensions to determine what a file contains.

For example, a file named:

```text
document.txt
```

could contain something other than plain text.

Use:

```bash
file document.txt
```

to inspect its detected content type.

**Cybersecurity connection:** Attackers may disguise malicious files by giving them misleading filenames or extensions. Security analysts should inspect file contents and metadata rather than trusting filenames alone.

### 12. Practical Demonstration

Enter your Module 06 lab:

```bash
cd ~/module06-lab
```

Create sample filesystem objects:

```bash
touch normal.txt
mkdir sample-directory
ln -s normal.txt normal-link
mkfifo sample-pipe
```

Inspect them:

```bash
ls -l
```

You should see:

- `normal.txt` beginning with `-`
- `sample-directory` beginning with `d`
- `normal-link` beginning with `l`
- `sample-pipe` beginning with `p`

Now inspect the regular file:

```bash
file normal.txt
```

Inspect its metadata:

```bash
stat normal.txt
```

You have now created and identified four different filesystem object types.

### 13. Cybersecurity Applications

Understanding file types is useful during security investigations.

**A. Identifying suspicious files**

The `file` command can reveal that a file's contents do not match its extension.

**B. Inspecting symbolic links**

Unexpected symbolic links may redirect users or applications to unusual locations.

**C. Investigating device files**

Device files are part of the operating system's hardware and input/output interfaces.

**D. Understanding process communication**

Named pipes and sockets can be used by applications and services to communicate.

**E. Inspecting system activity**

Commands such as `ls -l`, `stat`, and `ss` help analysts understand filesystem objects and communication endpoints.

### 14. Important Safety Notes

1. Do not experiment with writing directly to device files in `/dev`.
2. Do not modify block devices or partitions without a controlled lab and a clear reason.
3. Do not assume a file is safe because it has a familiar extension.
4. Be careful when following symbolic links, especially in privileged operations.
5. Practise creating and inspecting filesystem objects inside your own lab directory.

### 15. Knowledge Check

Answer these questions in your notebook.

1. How many common Linux file types were introduced in this lesson?
2. What symbol identifies a regular file?
3. What symbol identifies a directory?
4. What is a symbolic link?
5. What is the difference between character devices and block devices?
6. What does FIFO mean?
7. What is the purpose of a socket?
8. What is the purpose of the `file` command?
9. Why should cybersecurity analysts not trust file extensions alone?
10. What does the first character in `ls -l` output tell you?

### 16. Practical Exercises

Complete these exercises inside `~/module06-lab`.

**Exercise 1:** Create a regular file named `evidence.txt`.

**Exercise 2:** Create a directory named `investigation`.

**Exercise 3:** Create a symbolic link named `evidence-link` pointing to `evidence.txt`.

**Exercise 4:** Create a named pipe called `investigation-pipe`.

**Exercise 5:** Use `ls -l` to identify the four filesystem object types.

**Exercise 6:** Use `file` to inspect `evidence.txt`.

**Exercise 7:** Use `stat` to inspect `evidence.txt`.

**Exercise 8:** Use `lsblk` to display the block devices available in your Kali VM.

### 17. Lesson Summary

In this lesson, you learned about:

- Regular files
- Directories
- Symbolic links
- Character devices
- Block devices
- Named pipes
- Sockets
- The `ls -l`, `file`, `stat`, `lsblk`, and `ss` commands
- File extensions versus actual file types
- The cybersecurity importance of file identification

**Next Lesson:** Linux Directory Structure and Paths.

**End of Lesson 02.**