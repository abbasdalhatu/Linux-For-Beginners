
# PRACTICAL LABS
# MODULE 04 – LINUX SYSTEM ADMINISTRATION

## LAB 09: BACKUP AND RECOVERY

### 1. Lab Overview

Backup and recovery are essential responsibilities of a Linux system administrator.

A backup is a separate copy of important data that can be used when the original data is lost, damaged, corrupted, or accidentally deleted.

Recovery is the process of restoring data or system functionality after a failure.

In cybersecurity, backups are important because they help organizations recover from:

- Accidental file deletion.
- Hardware failure.
- Malware infections.
- Ransomware incidents.
- Software corruption.
- Human mistakes.
- System crashes.

For example, if a company loses important customer records because of a storage failure, a reliable backup can help restore the information.

**Lab environment:** Kali Linux VM.

**Safety:** All practical exercises use files created specifically for this lab. Never test recovery by deleting important system files or modifying your actual operating system.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain the importance of backups.
2. Identify different backup strategies.
3. Understand the Linux `tar` command.
4. Create compressed backups.
5. Restore files from backups.
6. Compare files using checksums.
7. Understand backup verification.
8. Generate a backup and recovery report.

### 3. Understanding Backup Types

There are three major backup types.

**A. Full Backup**

A full backup copies all selected data.

Example:

An organization backs up all its documents, databases, and configuration files.

Advantages:
- Easy to understand.
- Easier to restore.
- Provides a complete backup set.

Disadvantages:
- Requires more storage.
- Can take more time.

**B. Incremental Backup**

An incremental backup copies only data changed since the most recent backup.

Advantages:
- Uses less storage.
- Usually faster.

Disadvantages:
- Restoration may require several backup sets.

**C. Differential Backup**

A differential backup copies data changed since the last full backup.

Advantages:
- Restoration is generally simpler than a long incremental chain.
- Requires fewer backup sets than incremental restoration.

Disadvantages:
- Backup size can increase over time.

### 4. Understand the 3-2-1 Backup Rule

The 3-2-1 backup rule is a widely used backup strategy.

It recommends:

- 3 copies of important data.
- 2 different storage types.
- 1 copy stored off-site.

Example:

| Copy | Location |
|---|---|
| Original | Laptop SSD |
| Backup 1 | External hard drive |
| Backup 2 | Secure off-site storage |

The purpose is to reduce the risk of losing every copy through one incident.

A backup stored on the same disk as the original files is not sufficient protection against disk failure.

### 5. Prepare the Practical Lab Environment

Create the lab directory:

```bash
mkdir -p ~/practical-labs/module04/lab09
```

Create a directory for sample files:

```bash
mkdir -p ~/practical-labs/module04/lab09/source
```

Create three sample files:

```bash
echo "Linux system administration notes" > ~/practical-labs/module04/lab09/source/notes.txt
```

```bash
echo "Backup and recovery practical exercise" > ~/practical-labs/module04/lab09/source/backup-notes.txt
```

```bash
echo "System security and recovery procedures" > ~/practical-labs/module04/lab09/source/security.txt
```

Inspect the files:

```bash
ls -lah ~/practical-labs/module04/lab09/source/
```

Display their contents:

```bash
cat ~/practical-labs/module04/lab09/source/notes.txt
```

You now have sample data to practice backup and recovery.

### 6. Create a Full Backup Using tar

The `tar` command is commonly used to archive files and directories.

Create a compressed backup:

```bash
tar -czvf ~/practical-labs/module04/lab09/full-backup.tar.gz -C ~/practical-labs/module04/lab09 source
```

Understand the options:

| Option | Meaning |
|---|---|
| `c` | Create an archive |
| `z` | Compress using gzip |
| `v` | Display progress |
| `f` | Specify archive filename |

Inspect the backup:

```bash
ls -lh ~/practical-labs/module04/lab09/
```

List the contents of the archive:

```bash
tar -tzvf ~/practical-labs/module04/lab09/full-backup.tar.gz
```

You have now created a compressed full backup of your sample directory.

### 7. Create a Backup Without Compression

Sometimes an administrator wants to create an archive without compression.

Run:

```bash
tar -cvf ~/practical-labs/module04/lab09/plain-backup.tar -C ~/practical-labs/module04/lab09 source
```

Inspect it:

```bash
ls -lh ~/practical-labs/module04/lab09/
```

Compare its size with the compressed archive.

The compressed archive may be smaller, depending on the content.

### 8. Restore Files from a Backup

Restoration means recovering files from an existing backup.

Create a restoration directory:

```bash
mkdir -p ~/practical-labs/module04/lab09/restored
```

Extract the compressed backup:

```bash
tar -xzvf ~/practical-labs/module04/lab09/full-backup.tar.gz -C ~/practical-labs/module04/lab09/restored
```

Inspect the restored directory:

```bash
ls -lah ~/practical-labs/module04/lab09/restored/source/
```

Read a restored file:

```bash
cat ~/practical-labs/module04/lab09/restored/source/notes.txt
```

The original files and restored copies should contain the same information.

### 9. Verify Backup Integrity Using Checksums

Creating a backup is not enough.

An administrator must verify that the backup is readable and that restored files match the originals.

A checksum is a value calculated from file contents.

The SHA-256 algorithm produces a 256-bit hash.

Generate checksums for the original files:

```bash
cd ~/practical-labs/module04/lab09
```

```bash
sha256sum source/*
```

Generate checksums for the restored files:

```bash
sha256sum restored/source/*
```

Compare the matching files.

If the contents are identical, their SHA-256 checksums should match.

For an automated comparison:

```bash
diff -r source restored/source
```

No output from `diff` means it found no content differences.

You can also check the archive itself:

```bash
sha256sum full-backup.tar.gz
```

Remember: a checksum can help detect accidental corruption or unexpected changes. It does not prove that a backup is malware-free or that it is safe to restore.

### 10. Simulate Data Loss Safely

Now you will simulate accidental deletion using only a sample file.

First, confirm that the file exists:

```bash
cat ~/practical-labs/module04/lab09/source/notes.txt
```

Create a separate backup of the sample directory if you have not already done so.

Now remove only the sample file:

```bash
rm ~/practical-labs/module04/lab09/source/notes.txt
```

Verify that it is no longer present:

```bash
ls ~/practical-labs/module04/lab09/source/
```

Restore the backup into the separate restoration directory:

```bash
tar -xzvf ~/practical-labs/module04/lab09/full-backup.tar.gz -C ~/practical-labs/module04/lab09/restored
```

Inspect the recovered file:

```bash
cat ~/practical-labs/module04/lab09/restored/source/notes.txt
```

**Important:** The original sample file remains deleted. You recovered its contents from the backup. This demonstrates why backups are necessary.

### 11. Inspect Available Storage Before Backups

Backups require sufficient storage space.

Check available disk space:

```bash
df -h
```

Check the size of your lab directory:

```bash
du -sh ~/practical-labs/module04/lab09/
```

Check the size of the sample directory:

```bash
du -sh ~/practical-labs/module04/lab09/source/
```

A system administrator should consider:

- Available disk space.
- Backup frequency.
- Backup retention period.
- Data sensitivity.
- Storage reliability.
- Encryption requirements.

### 12. Backup Security

Backups can contain sensitive information.

If an attacker gains access to backups, they may obtain information that is no longer present on the original system.

Important backup security practices include:

**A. Encryption**

Encrypt sensitive backups to prevent unauthorized access.

**B. Access Control**

Restrict backup access to authorized users.

**C. Offline Copies**

Keep at least one protected backup disconnected from normal network access when appropriate.

**D. Retention Policies**

Define how long backups should be retained.

**E. Regular Testing**

Periodically restore sample data to verify that backups actually work.

**F. Monitoring**

Monitor backup jobs and investigate failed backup operations.

### 13. Understand Backup Scheduling

Linux administrators can automate recurring backups using cron or systemd timers.

Inspect the current user's scheduled jobs:

```bash
crontab -l
```

Inspect system-wide cron configuration:

```bash
ls -lah /etc/cron.d/
```

A hypothetical cron schedule might look like:

```text
0 2 * * * /usr/local/bin/backup-script.sh
```

This means the command is scheduled for 2:00 AM every day.

Do not add this schedule to your system during this lab. You are learning to interpret scheduled backup jobs, not configuring production automation.

### 14. Create a Backup and Recovery Report

Create the report script:

```bash
nano ~/practical-labs/module04/lab09/backup-report.sh
```

Enter:

```bash
#!/bin/bash

LAB="$HOME/practical-labs/module04/lab09"
REPORT="$LAB/backup-recovery-report.txt"

{
    echo "LINUX BACKUP AND RECOVERY REPORT"
    echo "================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "SOURCE DIRECTORY"
    ls -lah "$LAB/source"
    echo

    echo "BACKUP ARCHIVES"
    ls -lh "$LAB"/*.tar* 2>/dev/null
    echo

    echo "COMPRESSED BACKUP CONTENTS"
    tar -tzf "$LAB/full-backup.tar.gz" 2>/dev/null
    echo

    echo "BACKUP CHECKSUM"
    sha256sum "$LAB/full-backup.tar.gz"
    echo

    echo "RESTORED FILES"
    find "$LAB/restored" -type f
    echo

    echo "STORAGE INFORMATION"
    df -h "$HOME"
    echo

    echo "LAB DIRECTORY SIZE"
    du -sh "$LAB"

} > "$REPORT" 2>&1

echo "Backup report generated: $REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x ~/practical-labs/module04/lab09/backup-report.sh
```

Execute it:

```bash
~/practical-labs/module04/lab09/backup-report.sh
```

Inspect the report:

```bash
cat ~/practical-labs/module04/lab09/backup-recovery-report.txt
```

### 15. Practical Exercises

**Exercise 1 – Create Sample Data**

Create at least three sample files inside:

```text
~/practical-labs/module04/lab09/source/
```

**Exercise 2 – Create a Full Backup**

Create a compressed archive using `tar`.

**Exercise 3 – Inspect the Archive**

List its contents without extracting it.

**Exercise 4 – Restore the Backup**

Extract the archive into the separate `restored` directory.

**Exercise 5 – Verify Integrity**

Compare the original and restored files using SHA-256 checksums and `diff`.

**Exercise 6 – Simulate Data Loss**

Remove one sample file and recover its contents from the backup.

**Exercise 7 – Check Storage**

Inspect disk space and backup directory size.

**Exercise 8 – Generate the Backup Report**

Run your backup report script and inspect its output.

### 16. Lab Documentation

Create:

```bash
nano ~/practical-labs/module04/lab09/lab09-report.txt
```

Record:

```text
MODULE 04 – LAB 09
BACKUP AND RECOVERY

1. What is a backup?
2. What is the difference between full and incremental backups?
3. Explain the 3-2-1 backup rule.
4. Which command creates a compressed tar archive?
5. Which command extracts a tar.gz archive?
6. What is the purpose of SHA-256?
7. Why should backups be tested?
8. What is the importance of offline backups?
9. What happened during the simulated data loss?
10. What did I learn?
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 17. Knowledge Check

1. What is the difference between backup and recovery?
2. Explain full, incremental, and differential backups.
3. What does the 3-2-1 backup rule mean?
4. What is the purpose of the `tar` command?
5. What does the `-z` option do?
6. Why are checksums important?
7. What is the difference between encryption and hashing?
8. Why should one backup copy be stored offline or separately?
9. How can ransomware affect backups?
10. Why must backup restoration be tested?

### 18. Lab Completion Checklist

- [ ] Created the practical lab directory.
- [ ] Created sample files.
- [ ] Created a compressed backup.
- [ ] Created an uncompressed archive.
- [ ] Inspected archive contents.
- [ ] Restored files from backup.
- [ ] Verified file integrity.
- [ ] Simulated data loss safely.
- [ ] Inspected available storage.
- [ ] Generated the backup report.
- [ ] Completed lab documentation.

**LAB 09 STATUS: READY FOR PRACTICAL COMPLETION**
