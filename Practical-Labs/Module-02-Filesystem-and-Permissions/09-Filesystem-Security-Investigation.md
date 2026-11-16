
# PRACTICAL LABS
# MODULE 02 – FILESYSTEM AND PERMISSION LABS

## LAB 09: FILESYSTEM SECURITY INVESTIGATION

### 1. Introduction

A Linux filesystem contains important information about users, applications, permissions, ownership, and system activity.

A security investigator must know how to examine files and directories to identify suspicious configurations and potential security weaknesses.

In this lab, you will perform a controlled filesystem security investigation inside your Kali Linux virtual machine.

You will investigate:

- World-writable files and directories.
- Unexpected ownership.
- SUID and SGID permissions.
- Extended ACLs.
- Symbolic links.
- Sensitive configuration files.
- File integrity using cryptographic hashes.
- Suspicious filesystem permissions.

All activities will take place on your own Kali VM and dedicated practice directory.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Inspect filesystem permissions.
2. Identify world-writable files and directories.
3. Identify SUID and SGID files.
4. Investigate unusual ownership.
5. Examine ACL configurations.
6. Identify symbolic links and broken links.
7. Calculate file hashes.
8. Create a filesystem security audit report.
9. Explain why filesystem misconfigurations matter.

### 3. Prepare Your Investigation Workspace

Create the workspace:

```bash
mkdir -p ~/practical-labs/module02/lab09
cd ~/practical-labs/module02/lab09
```

Create investigation folders:

```bash
mkdir -p evidence
mkdir -p reports
mkdir -p suspicious
mkdir -p normal
```

Create sample files:

```bash
echo "Normal university document" > normal/document.txt
echo "Confidential practice data" > suspicious/confidential.txt
echo "System configuration example" > evidence/config.txt
```

Inspect:

```bash
find . -maxdepth 2 -type f -ls
```

### 4. Investigation 1: Examine File Permissions

Inspect all files and directories:

```bash
ls -laR .
```

Use `stat` to examine an individual file:

```bash
stat suspicious/confidential.txt
```

Look for:

- File type.
- Owner.
- Group.
- Permission mode.
- File size.
- Modification time.
- Inode number.

Create a detailed permission listing:

```bash
find . -printf '%M %u %g %p\n'
```

Explanation:

- `%M` displays permissions.
- `%u` displays the owner.
- `%g` displays the group.
- `%p` displays the pathname.

### 5. Investigation 2: Identify World-Writable Files

A world-writable file allows users with access to its parent directory to modify its contents, subject to other security controls.

Create a controlled example:

```bash
touch suspicious/public-write.txt
chmod 666 suspicious/public-write.txt
```

Find world-writable files in your lab:

```bash
find . -type f -perm -0002 -ls
```

Find world-writable directories:

```bash
find . -type d -perm -0002 -ls
```

Understand the difference:

- World-writable file: Others have write permission on the file.
- World-writable directory: Others have write permission on the directory.

A world-writable directory may allow users to create or remove entries, depending on additional permissions and sticky-bit protection.

### 6. Investigation 3: Identify SUID and SGID Files

Create controlled examples:

```bash
touch suspicious/suid-example
touch suspicious/sgid-example
chmod 4755 suspicious/suid-example
chmod 2755 suspicious/sgid-example
```

Search for SUID files:

```bash
find . -type f -perm -4000 -ls
```

Search for SGID files:

```bash
find . -type f -perm -2000 -ls
```

These are deliberately configured practice files, not executable programs.

Inspect:

```bash
ls -l suspicious/
```

Security lesson:

Unexpected SUID or SGID permissions deserve investigation because executable files with these permissions can operate with additional privileges.

### 7. Investigation 4: Examine File Ownership

Display ownership:

```bash
find . -printf '%u %g %p\n'
```

Inspect selected files:

```bash
stat suspicious/confidential.txt
stat evidence/config.txt
```

Search for files not owned by your current user:

```bash
find . -type f ! -user "$(whoami)" -ls
```

Interpret the results carefully.

A file owned by another user is not automatically malicious. It may be legitimate.

Investigators should compare ownership against expected system or application requirements.

### 8. Investigation 5: Examine ACL Configurations

Create an ACL practice file:

```bash
touch suspicious/acl-example.txt
```

Set a restrictive traditional permission:

```bash
chmod 600 suspicious/acl-example.txt
```

Grant an additional read permission to a named user if your dedicated test user from earlier labs still exists:

```bash
getent passwd acluser
```

If the user exists:

```bash
setfacl -m u:acluser:r-- suspicious/acl-example.txt
```

Inspect:

```bash
getfacl suspicious/acl-example.txt
```

Look for:

- Named user entries.
- Named group entries.
- Default ACLs.
- ACL masks.
- Effective permissions.

Remove the practice ACL afterward:

```bash
setfacl -b suspicious/acl-example.txt
```

### 9. Investigation 6: Examine Symbolic Links

Create a normal target:

```bash
echo "Expected target file" > normal/target.txt
```

Create a symbolic link:

```bash
ln -s ../normal/target.txt suspicious/target-link
```

Inspect:

```bash
ls -l suspicious/target-link
```

Resolve the link:

```bash
readlink -f suspicious/target-link
```

Find symbolic links:

```bash
find . -type l -ls
```

Find broken symbolic links:

```bash
find . -xtype l -print
```

A symbolic link should be investigated when it points somewhere unexpected, especially if a program or administrator assumes the target is within a trusted directory.

### 10. Investigation 7: Examine Sensitive System Files

Linux stores important system configuration and account information in `/etc`.

Inspect selected metadata:

```bash
ls -l /etc/passwd
ls -l /etc/group
ls -l /etc/shadow
```

Inspect the password database format:

```bash
head -n 5 /etc/passwd
```

Do not display or copy the contents of `/etc/shadow`.

Inspect its metadata only:

```bash
stat /etc/shadow
```

Understand the distinction:

| File | Purpose |
|---|---|
| `/etc/passwd` | Account information |
| `/etc/group` | Group information |
| `/etc/shadow` | Protected password authentication data |
| `/etc/sudoers` | Sudo authorization configuration |

Do not modify these files during this investigation.

### 11. Investigation 8: File Integrity with SHA-256

A cryptographic hash can help identify whether file contents have changed.

Create a sample:

```bash
echo "Original evidence content" > evidence/integrity-test.txt
```

Calculate its SHA-256 hash:

```bash
sha256sum evidence/integrity-test.txt
```

Save the hash:

```bash
sha256sum evidence/integrity-test.txt > evidence/integrity-hash.txt
```

Verify:

```bash
sha256sum -c evidence/integrity-hash.txt
```

Expected result:

```text
evidence/integrity-test.txt: OK
```

Modify the file:

```bash
echo "Modified content" >> evidence/integrity-test.txt
```

Verify again:

```bash
sha256sum -c evidence/integrity-hash.txt
```

The verification should now fail because the file content changed.

Important:

A hash helps detect changes relative to a trusted earlier hash. It does not prove who created a file or whether its original contents were trustworthy.

### 12. Investigation 9: Examine Recent File Modifications

Find files modified within the last day in your lab:

```bash
find . -type f -mtime -1 -ls
```

Find files modified within the last hour:

```bash
find . -type f -mmin -60 -ls
```

Inspect timestamps:

```bash
stat suspicious/confidential.txt
```

Understand these common timestamps:

- Access time (atime): Last access.
- Modification time (mtime): Last content modification.
- Change time (ctime): Last inode metadata change.

Important: ctime is not the file creation time.

### 13. Investigation 10: Search for Suspicious Filenames

Search your lab for common sensitive filename patterns:

```bash
find . -type f \( -iname "*.key" -o -iname "*.pem" -o -iname "*.conf" -o -iname "*.bak" \) -print
```

Search for hidden files:

```bash
find . -type f -name ".*" -print
```

Hidden files are not automatically malicious.

Their contents, ownership, permissions, and purpose must be examined before drawing conclusions.

### 14. Build an Automated Filesystem Audit

Create a simple Bash script:

```bash
nano filesystem-audit.sh
```

Enter:

```bash
#!/bin/bash

TARGET="${1:-.}"

if [ ! -d "$TARGET" ]; then
    echo "Error: Target directory does not exist."
    exit 1
fi

echo "FILESYSTEM SECURITY AUDIT"
echo "========================="
echo "Date: $(date)"
echo "User: $(whoami)"
echo "Target: $TARGET"
echo

echo "[1] WORLD-WRITABLE FILES"
find "$TARGET" -type f -perm -0002 -ls

echo
echo "[2] WORLD-WRITABLE DIRECTORIES"
find "$TARGET" -type d -perm -0002 -ls

echo
echo "[3] SUID FILES"
find "$TARGET" -type f -perm -4000 -ls

echo
echo "[4] SGID FILES"
find "$TARGET" -type f -perm -2000 -ls

echo
echo "[5] SYMBOLIC LINKS"
find "$TARGET" -type l -ls

echo
echo "[6] BROKEN SYMBOLIC LINKS"
find "$TARGET" -xtype l -print

echo
echo "[7] FILE OWNERSHIP"
find "$TARGET" -printf '%u %g %p\n'

echo
echo "AUDIT COMPLETED"
```

Save and exit.

Make it executable:

```bash
chmod 700 filesystem-audit.sh
```

Run:

```bash
./filesystem-audit.sh .
```

Run it against the suspicious directory:

```bash
./filesystem-audit.sh suspicious
```

The script is intentionally limited to the supplied target directory. It does not modify the files it examines.

### 15. Generate the Final Investigation Report

Run:

```bash
./filesystem-audit.sh . > reports/filesystem-security-report.txt
```

Add a summary:

```bash
{
    echo
    echo "INVESTIGATION SUMMARY"
    echo "===================="
    echo "Investigation directory: $PWD"
    echo "Report generated: $(date)"
    echo
    echo "SHA-256 EVIDENCE HASHES:"
    find evidence -type f -exec sha256sum {} \;
    echo
    echo "FILESYSTEM CAPACITY:"
    df -h .
} >> reports/filesystem-security-report.txt
```

Read the report:

```bash
cat reports/filesystem-security-report.txt
```

### 16. Professional Investigation Methodology

A professional filesystem investigation follows a structured process:

1. Define the scope.
2. Identify the systems and directories being examined.
3. Record the date and time.
4. Inspect ownership and permissions.
5. Identify unusual configurations.
6. Collect relevant metadata.
7. Calculate hashes when appropriate.
8. Preserve original evidence.
9. Record observations.
10. Document findings and recommendations.

Do not immediately delete suspicious files.

Preserve evidence first, document the finding, and investigate its purpose.

### 17. Lab Cleanup

Remove the special permissions from the practice files:

```bash
chmod u-s suspicious/suid-example
chmod g-s suspicious/sgid-example
```

Remove world-writable permissions:

```bash
chmod 644 suspicious/public-write.txt
```

Keep the audit report and evidence files for your assessment.

Do not modify files outside your dedicated lab workspace.

### 18. Knowledge Check

1. What is a world-writable file?
2. Why can world-writable directories create security risks?
3. What is the security significance of SUID?
4. What does SGID do?
5. Why should unexpected file ownership be investigated?
6. What does `getfacl` reveal?
7. How can you identify broken symbolic links?
8. What is the purpose of SHA-256?
9. What is the difference between mtime and ctime?
10. Why should `/etc/shadow` be protected?
11. Why should suspicious files be preserved before investigation?
12. What information should a filesystem audit report contain?

### 19. Lab Completion Checklist

- [ ] Created the investigation workspace.
- [ ] Inspected filesystem permissions.
- [ ] Identified world-writable files.
- [ ] Identified world-writable directories.
- [ ] Investigated SUID and SGID files.
- [ ] Examined ownership.
- [ ] Inspected ACL configurations.
- [ ] Investigated symbolic links.
- [ ] Inspected sensitive system-file metadata.
- [ ] Calculated SHA-256 hashes.
- [ ] Verified file integrity.
- [ ] Examined modification timestamps.
- [ ] Searched for suspicious filenames.
- [ ] Created the Bash audit script.
- [ ] Generated the investigation report.
- [ ] Completed the cleanup.

### 20. Expected Workspace

```text
~/practical-labs/module02/lab09/
├── evidence/
│   ├── config.txt
│   ├── integrity-test.txt
│   └── integrity-hash.txt
├── reports/
│   └── filesystem-security-report.txt
├── suspicious/
│   ├── confidential.txt
│   ├── public-write.txt
│   ├── suid-example
│   ├── sgid-example
│   ├── acl-example.txt
│   └── target-link
├── normal/
│   ├── document.txt
│   └── target.txt
└── filesystem-audit.sh
```

**Lab 09 Status:** Complete when the practical exercises and checklist are finished.

**Next Lab:** Lab 10 – Module 02 Practical Assessment.
