---
---

# Module 03: Linux Filesystem – Final Assessment

## Course: Linux for Ethical Hackers

**Module:** 03 – Linux Filesystem  
**Assessment Type:** Theory and Practical  
**Total Marks:** 100  
**Pass Mark:** 50%

---

## Section A: Multiple Choice Questions (20 Marks)

Choose the correct answer. Each question carries 2 marks.

**1. What is the root directory in Linux?**

A. `/home`  
B. `/root`  
C. `/`  
D. `/etc`

**2. Which command displays the current working directory?**

A. `ls`  
B. `pwd`  
C. `cd`  
D. `whoami`

**3. Which directory stores system configuration files?**

A. `/etc`  
B. `/tmp`  
C. `/var`  
D. `/dev`

**4. Which command displays human-readable disk usage?**

A. `ls -h`  
B. `df -h`  
C. `pwd -h`  
D. `cd -h`

**5. What does the `~` symbol represent?**

A. Root directory  
B. Current directory  
C. User's home directory  
D. Parent directory

**6. Which command displays block devices and partitions?**

A. `lsblk`  
B. `chmod`  
C. `grep`  
D. `whoami`

**7. What does a filename beginning with a dot usually indicate?**

A. Executable file  
B. Hidden file  
C. System error  
D. Empty file

**8. Which directory contains process and kernel information through a virtual filesystem?**

A. `/boot`  
B. `/home`  
C. `/proc`  
D. `/opt`

**9. Which command estimates directory disk usage?**

A. `du`  
B. `pwd`  
C. `touch`  
D. `who`

**10. Which command checks filesystem consistency and can perform repairs?**

A. `find`  
B. `fsck`  
C. `mkdir`  
D. `cat`

---

## Section B: Short Answer Questions (20 Marks)

Answer each question clearly. Each question carries 4 marks.

1. Define a Linux filesystem.
2. Explain the difference between `/` and `/root`.
3. Differentiate between absolute and relative paths.
4. Explain the difference between partitions and filesystems.
5. What are `/proc` and `/sys` used for?

---

## Section C: Linux Commands (20 Marks)

Write the correct Linux command for each task. Each question carries 2 marks.

1. Display your current working directory.
2. List all files, including hidden files.
3. Navigate to `/etc`.
4. Display available disk space in human-readable format.
5. Display the size of your home directory.
6. Display mounted filesystems.
7. Display block devices and filesystem types.
8. Identify the type of `/etc/passwd`.
9. Display information about system memory.
10. Find regular files larger than 100 MB in your home directory.

---

## Section D: Practical Laboratory (30 Marks)

Perform the following tasks in your Kali Linux virtual machine.

### Task 1: Filesystem Navigation (5 Marks)

Run:

```bash
pwd
cd /
ls -lah
cd /etc
pwd
```

Explain what each command does.

### Task 2: Hidden Files (5 Marks)

Run:

```bash
ls -la ~
```

Identify three hidden files or directories, if present.

Explain why Linux uses hidden files.

### Task 3: Storage Inspection (5 Marks)

Run:

```bash
df -h
lsblk -f
```

Identify your root filesystem and report its available storage.

### Task 4: Directory Analysis (5 Marks)

Run:

```bash
du -sh ~
du -sh /var/log
```

Explain what the two commands measure.

### Task 5: Kernel Information (5 Marks)

Run:

```bash
cat /proc/cpuinfo
cat /proc/meminfo
uname -r
```

Explain what information each command displays.

### Task 6: File Metadata (5 Marks)

Run:

```bash
stat /etc/passwd
file /etc/passwd
ls -l /etc/passwd
```

Explain the difference between the information provided by these commands.

---

## Section E: Cybersecurity Application (10 Marks)

Answer both questions. Each question carries 5 marks.

**1.** You are investigating a Linux computer and discover a hidden directory inside a user's home directory. Explain five steps you would take to inspect it safely.

**2.** A Linux server reports that its disk is almost full. Mention five commands you could use to investigate the problem and explain their purposes.

---

## Assessment Submission Checklist

- [ ] Section A completed.
- [ ] Section B completed.
- [ ] Section C completed.
- [ ] Section D practical tasks completed.
- [ ] Section E completed.
- [ ] Answers reviewed.

## Final Assessment Result

| Score | Grade |
|---|---|
| 80–100 | Excellent |
| 70–79 | Very Good |
| 60–69 | Good |
| 50–59 | Pass |
| 0–49 | Fail |

**Pass Requirement:** 50/100.

---

## Module Completion

After completing this assessment, you should be able to:

1. Understand the Linux filesystem hierarchy.
2. Navigate directories using Linux paths.
3. Identify Linux file types.
4. Inspect hidden files and directories.
5. Understand storage devices and partitions.
6. Understand mounting and unmounting.
7. Inspect `/proc` and `/sys`.
8. Investigate common filesystem problems.
9. Use filesystem inspection commands confidently.

**Congratulations on completing Module 03 – Linux Filesystem!**