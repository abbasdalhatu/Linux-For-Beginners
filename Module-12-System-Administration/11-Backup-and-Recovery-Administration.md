---
---


# Module 12 – Lesson 11: Backup and Recovery Administration

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain backups and recovery.
2. Understand why backups are important.
3. Identify different backup types.
4. Understand backup strategies.
5. Use Linux commands to create backups.
6. Compress and archive files.
7. Restore files from backups.
8. Verify backup integrity.
9. Understand backup security.
10. Create a practical backup and recovery report.

---

## 2. What Is a Backup?

A backup is a separate copy of important data that can be used to recover information if the original data is lost, damaged, corrupted, or deleted.

### Example

Imagine you have a university project containing:

- Source code.
- Research documents.
- Database files.
- Project reports.

One day, your laptop storage becomes corrupted.

Without a backup, you may lose your entire project.

With a backup, you can restore the files.

### Why are backups important?

Backups protect against:

1. Accidental deletion.
2. Hardware failure.
3. Malware and ransomware.
4. Filesystem corruption.
5. Software errors.
6. Theft or physical damage.
7. Human mistakes.

**Important:** A backup is useful only if it can actually be restored.

---

## 3. Understanding Backup Types

There are three important backup types.

### A. Full Backup

A full backup copies all selected data.

Example:

You have 10 GB of important files.

A full backup copies all 10 GB.

Advantages:
- Simple restoration.
- Complete copy of selected data.
- Easy to understand.

Disadvantages:
- Requires more storage.
- May take longer.

### B. Incremental Backup

An incremental backup copies data that has changed since the previous backup.

Example:

| Backup | Data Copied |
|---|---|
| Sunday | Full backup |
| Monday | Monday's changes |
| Tuesday | Tuesday's changes |
| Wednesday | Wednesday's changes |

Advantages:
- Uses less storage.
- Often faster.

Disadvantages:
- Restoration may require the full backup and multiple incremental backups.

### C. Differential Backup

A differential backup copies changes made since the last full backup.

Example:

| Backup | Data Copied |
|---|---|
| Sunday | Full backup |
| Monday | Changes since Sunday |
| Tuesday | All changes since Sunday |
| Wednesday | All changes since Sunday |

Advantages:
- Restoration is simpler than a long incremental chain.
- Often faster to restore.

Disadvantages:
- Backup size increases until the next full backup.

### Comparison

| Type | Copies | Storage Requirement |
|---|---|---|
| Full | All selected data | High |
| Incremental | Changes since previous backup | Usually low |
| Differential | Changes since last full backup | Usually increases |

---

## 4. Understanding the 3-2-1 Backup Rule

The 3-2-1 rule is a common backup strategy.

It recommends:

- **3 copies:** Keep three copies of important data, including the original.
- **2 media types:** Use at least two different storage types.
- **1 off-site copy:** Keep one copy in a separate location.

### Example

A university student might use:

1. Original project on a laptop.
2. Backup on an external hard drive.
3. Another backup in secure cloud storage.

This helps protect against a single device failure or physical incident.

### Modern improvement: 3-2-1-1-0

Some organisations extend the rule:

- 3 copies of data.
- 2 different storage types.
- 1 off-site copy.
- 1 offline or immutable copy.
- 0 unverified backup errors.

This emphasises protection against ransomware and backup corruption.

---

## 5. Linux Backup Tools

Linux provides several tools for backup administration.

### A. cp

Copies files and directories.

Example:

```bash
cp report.txt report-backup.txt
```

Copy a directory:

```bash
cp -r project project-backup
```

### B. rsync

A powerful file synchronisation and backup tool.

Example:

```bash
rsync -av source/ destination/
```

Important options:

| Option | Meaning |
|---|---|
| `-a` | Archive mode |
| `-v` | Verbose output |
| `-r` | Recursive copying |
| `-n` | Dry run |
| `--delete` | Deletes destination files absent from source |

**Warning:** `--delete` can remove important destination files. Do not use it without careful review.

### C. tar

Creates archive files.

Example:

```bash
tar -cvf backup.tar project/
```

Options:

- `-c`: Create archive.
- `-v`: Show files being processed.
- `-f`: Specify archive filename.

### D. gzip

Compresses files.

Example:

```bash
gzip backup.tar
```

This creates:

```text
backup.tar.gz
```

### E. zip

Creates ZIP archives.

Example:

```bash
zip -r backup.zip project/
```

---

## 6. Creating a Full Backup with tar

The `tar` command is widely used for Linux backups.

### Example

```bash
tar -czvf project-backup.tar.gz project/
```

Meaning:

- `-c`: Create archive.
- `-z`: Compress using gzip.
- `-v`: Display progress.
- `-f`: Specify archive filename.

### Inspect an archive

```bash
tar -tzf project-backup.tar.gz
```

This lists the archive contents without extracting them.

### Extract an archive

```bash
tar -xzvf project-backup.tar.gz
```

This extracts the archive into the current directory.

Always inspect archive contents and choose a safe extraction directory before restoring files.

---

## 7. Using rsync for Backups

`rsync` is useful for copying files efficiently.

Example:

```bash
rsync -av ~/Documents/ ~/backup/Documents/
```

This synchronises the contents of the source directory into the destination.

### Dry-run mode

Before performing a synchronisation, preview the operation:

```bash
rsync -avn ~/Documents/ ~/backup/Documents/
```

The `-n` option performs a dry run.

This helps you understand what would happen without making changes.

### Important difference: trailing slash

Consider:

```bash
rsync -av source/ destination/
```

This copies the contents of `source` into `destination`.

Compare:

```bash
rsync -av source destination/
```

This copies the `source` directory itself into `destination`.

Understanding this difference helps prevent backup mistakes.

---

## 8. Understanding Backup Verification

A backup should be verified after creation.

Verification helps establish that the backup file exists, can be read, and contains the expected data.

### A. Check file size

```bash
ls -lh backup.tar.gz
```

### B. List archive contents

```bash
tar -tzf backup.tar.gz
```

### C. Calculate a SHA-256 checksum

```bash
sha256sum backup.tar.gz
```

Example output:

```text
a1b2c3... backup.tar.gz
```

The displayed hash is an illustrative example.

### What is a checksum?

A checksum is a value calculated from data.

If the archive changes, its SHA-256 hash will normally change too.

You can compare the checksum calculated immediately after backup creation with a later checksum.

Matching checksums provide evidence that the file has not changed between those checks.

However, a checksum alone does not prove that the backup is complete or restorable.

---

## 9. Understanding Backup Recovery

Recovery is the process of restoring data from a backup.

Example:

Original file:

```text
project/report.txt
```

Backup archive:

```text
project-backup.tar.gz
```

If the original file is lost, the backup can be extracted to recover it.

### Recovery steps

1. Identify the correct backup.
2. Verify its integrity.
3. Inspect its contents.
4. Choose a safe recovery location.
5. Extract the required files.
6. Verify the restored data.
7. Document the recovery operation.

### Important rule

Do not extract an untrusted archive directly into important system directories.

Always inspect its contents first.

---

## 10. Practical Lab: Creating and Restoring a Backup

### Objective

Create sample files, archive them, verify the backup, simulate accidental deletion, and recover the files.

Everything will happen inside your personal course lab.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson11
cd ~/module12-lab/lesson11
```

### Step 2: Create a sample project

```bash
mkdir -p project/source project/documents
```

### Step 3: Create sample files

```bash
echo "Linux Backup Administration" > project/documents/notes.txt
```

```bash
echo "print('Hello from my backup project')" > project/source/main.py
```

```bash
echo "Backup and recovery practical exercise" > project/README.txt
```

### Step 4: Inspect the project

```bash
find project -type f
```

Display the contents:

```bash
cat project/documents/notes.txt
cat project/source/main.py
cat project/README.txt
```

### Step 5: Create a compressed backup

```bash
tar -czvf project-backup.tar.gz project/
```

Your archive should be:

```text
project-backup.tar.gz
```

### Step 6: Inspect the archive

```bash
tar -tzf project-backup.tar.gz
```

Confirm that the three sample files appear.

### Step 7: Calculate the SHA-256 checksum

```bash
sha256sum project-backup.tar.gz > backup-checksum.txt
```

Display it:

```bash
cat backup-checksum.txt
```

### Step 8: Verify the checksum

```bash
sha256sum -c backup-checksum.txt
```

You should receive:

```text
project-backup.tar.gz: OK
```

### Step 9: Simulate accidental deletion

Remove only the sample project directory:

```bash
rm -r project
```

This is safe because the directory was created specifically for this exercise.

Verify:

```bash
ls
```

The original project directory should be gone, while the backup archive remains.

### Step 10: Create a recovery directory

```bash
mkdir recovered
```

### Step 11: Restore the backup

```bash
tar -xzvf project-backup.tar.gz -C recovered/
```

The restored directory should be:

```text
recovered/project/
```

### Step 12: Verify recovered files

```bash
find recovered -type f
```

Read the restored notes:

```bash
cat recovered/project/documents/notes.txt
```

Read the restored Python file:

```bash
cat recovered/project/source/main.py
```

### Step 13: Compare original and restored data

The original directory was deleted, so use the recovered project to confirm that all three files are present.

```bash
find recovered/project -type f | sort
```

---

## 11. Practical Lab: Using rsync

### Step 1: Create source and destination directories

```bash
mkdir -p rsync-lab/source
mkdir -p rsync-lab/backup
```

### Step 2: Create sample files

```bash
echo "First backup file" > rsync-lab/source/file1.txt
echo "Second backup file" > rsync-lab/source/file2.txt
```

### Step 3: Preview synchronisation

```bash
rsync -avn rsync-lab/source/ rsync-lab/backup/
```

Because this is a dry run, no files should be copied.

### Step 4: Perform synchronisation

```bash
rsync -av rsync-lab/source/ rsync-lab/backup/
```

### Step 5: Verify the destination

```bash
ls -l rsync-lab/backup/
```

You should see both files.

### Step 6: Compare directories

```bash
diff -r rsync-lab/source rsync-lab/backup
```

No output normally means the directories contain matching file contents.

---

## 12. Backup Security

Backups often contain sensitive information.

For example:

- Student records.
- Password databases.
- Business documents.
- Source code.
- Database exports.
- Configuration files.

If attackers gain access to backups, they may obtain information that the original system protects.

### Important security practices

1. Restrict backup file permissions.
2. Encrypt sensitive backups.
3. Store copies separately from the original system.
4. Use offline or immutable backups where appropriate.
5. Test restoration regularly.
6. Protect backup credentials.
7. Monitor backup activity.
8. Maintain a recovery plan.

### Example: Restrict backup permissions

```bash
chmod 600 project-backup.tar.gz
```

This gives the file owner read and write permissions while removing access for group and other users.

For shared backup environments, access permissions should follow the organisation's security policy.

---

## 13. Lesson Summary

You learned:

1. A backup is a separate copy of important data.
2. Full backups copy all selected data.
3. Incremental backups copy changes since the previous backup.
4. Differential backups copy changes since the last full backup.
5. The 3-2-1 rule helps protect important data.
6. `tar` creates archives.
7. `gzip` compresses files.
8. `rsync` synchronises files and directories.
9. `sha256sum` calculates and verifies checksums.
10. Recovery restores information from a backup.
11. Backups must be protected against unauthorised access.
12. Restoration tests are essential.

---

## 14. Knowledge Check

1. What is a backup?
2. Why are backups important?
3. What is the difference between full and incremental backups?
4. How does a differential backup work?
5. Explain the 3-2-1 backup rule.
6. What is the purpose of `tar`?
7. What does the `-z` option do in a tar command?
8. What is the purpose of `rsync`?
9. What is a dry run?
10. What is a SHA-256 checksum?
11. Why should backups be tested?
12. Why should sensitive backup files be encrypted?

---

## 15. Practical Completion Checklist

- [ ] I understand backup and recovery.
- [ ] I understand full, incremental, and differential backups.
- [ ] I understand the 3-2-1 backup rule.
- [ ] I created a sample project.
- [ ] I created a compressed tar archive.
- [ ] I inspected the archive.
- [ ] I calculated and verified a SHA-256 checksum.
- [ ] I simulated accidental deletion.
- [ ] I restored my project successfully.
- [ ] I used rsync in dry-run mode.
- [ ] I synchronised files with rsync.
- [ ] I understand backup security.

**Lesson 11 is complete when you successfully create, verify, and restore your backup.**
