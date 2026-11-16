
# PRACTICAL LABS
# MODULE 02 – FILESYSTEM AND PERMISSION LABS

## LAB 10: MODULE 02 PRACTICAL ASSESSMENT

### 1. Assessment Overview

This assessment evaluates your understanding of Linux filesystems, permissions, ownership, ACLs, special permissions, links, mount points, and filesystem security.

You will complete a practical security administration scenario.

**Scenario:**

You have been assigned as a junior Linux security administrator for a university department.

Your responsibility is to create a secure project workspace, configure permissions, manage access, and produce an audit report.

### 2. Assessment Objectives

Demonstrate your ability to:

1. Navigate the Linux filesystem.
2. Identify file types and inodes.
3. Configure file and directory permissions.
4. Manage users and groups.
5. Configure ACLs.
6. Apply special permissions.
7. Create and inspect links.
8. Examine mount points.
9. Investigate filesystem security.
10. Produce a professional security report.

### 3. Part A – Create the Project Workspace

Create:

```bash
mkdir -p ~/practical-labs/module02/lab10
cd ~/practical-labs/module02/lab10
```

Create this structure:

```text
University-Project/
├── documents/
├── shared/
├── private/
├── evidence/
└── reports/
```

Commands:

```bash
mkdir -p University-Project/{documents,shared,private,evidence,reports}
```

Create sample files:

```bash
echo "University Project Documentation" > University-Project/documents/project.txt

echo "Shared Team Information" > University-Project/shared/team.txt

echo "Confidential Department Information" > University-Project/private/confidential.txt

echo "Evidence Collection" > University-Project/evidence/evidence.txt
```

### 4. Part B – Filesystem Investigation

Inspect the directory structure:

```bash
find University-Project -maxdepth 3 -ls
```

Identify file types:

```bash
file University-Project/documents/project.txt
file University-Project/shared/team.txt
```

Inspect inode numbers:

```bash
ls -li University-Project/documents/
```

Inspect metadata:

```bash
stat University-Project/documents/project.txt
```

### 5. Part C – Permission Configuration

Apply these permissions:

| Directory or file | Required permission |
|---|---|
| `documents/` | 750 |
| `shared/` | 2770 |
| `private/` | 700 |
| `evidence/` | 750 |
| `reports/` | 750 |
| `documents/project.txt` | 640 |
| `shared/team.txt` | 660 |
| `private/confidential.txt` | 600 |
| `evidence/evidence.txt` | 640 |

Apply the permissions:

```bash
chmod 750 University-Project/documents
chmod 2770 University-Project/shared
chmod 700 University-Project/private
chmod 750 University-Project/evidence
chmod 750 University-Project/reports

chmod 640 University-Project/documents/project.txt
chmod 660 University-Project/shared/team.txt
chmod 600 University-Project/private/confidential.txt
chmod 640 University-Project/evidence/evidence.txt
```

Verify:

```bash
ls -lR University-Project
```

### 6. Part D – User and Group Management

Check whether the practice group exists:

```bash
getent group universityteam
```

If it does not exist:

```bash
sudo groupadd universityteam
```

Check for a dedicated test user:

```bash
getent passwd universityuser
```

If it does not exist:

```bash
sudo useradd -m -s /bin/bash universityuser
```

Add the user to the group:

```bash
sudo usermod -aG universityteam universityuser
```

Assign group ownership to the shared directory:

```bash
sudo chown :universityteam University-Project/shared
```

Verify:

```bash
id universityuser
ls -ld University-Project/shared
```

Remember that a newly added supplementary group may require a fresh login session before it becomes active for that user.

### 7. Part E – ACL Configuration

Grant `universityuser` read-only access to the project document:

```bash
sudo setfacl -m u:universityuser:r-- University-Project/documents/project.txt
```

Inspect:

```bash
getfacl University-Project/documents/project.txt
```

Configure a default ACL for the shared directory:

```bash
sudo setfacl -m d:g:universityteam:rwx University-Project/shared
```

Inspect:

```bash
getfacl University-Project/shared
```

Remember that default ACLs affect newly created objects and their effective permissions also depend on the requested creation mode.

### 8. Part F – Special Permissions

Verify that the shared directory has SGID:

```bash
ls -ld University-Project/shared
```

Expected permission indicator:

```text
drwxrws---
```

Create a temporary directory:

```bash
mkdir University-Project/shared/temp
```

Apply the sticky bit:

```bash
chmod 1770 University-Project/shared/temp
```

Verify:

```bash
ls -ld University-Project/shared/temp
```

Do not apply SUID to actual system executables during this assessment.

### 9. Part G – Links and Inodes

Create a hard link:

```bash
ln University-Project/documents/project.txt University-Project/documents/project-hardlink.txt
```

Create a symbolic link:

```bash
ln -s project.txt University-Project/documents/project-symlink.txt
```

Inspect:

```bash
ls -li University-Project/documents/
```

Compare the inode numbers of the original and hard link.

Inspect the symbolic link:

```bash
ls -l University-Project/documents/project-symlink.txt
```

### 10. Part H – Mount Point and Storage Investigation

Inspect the root filesystem:

```bash
findmnt /
```

Inspect your home directory:

```bash
findmnt ~
```

Display filesystem usage:

```bash
df -h
```

Display filesystem types:

```bash
df -Th
```

Display block devices:

```bash
lsblk -f
```

Record your observations in the final report.

Do not mount, unmount, or reconfigure devices for this assessment.

### 11. Part I – Security Audit

Search for world-writable files:

```bash
find University-Project -type f -perm -0002 -ls
```

Search for world-writable directories:

```bash
find University-Project -type d -perm -0002 -ls
```

Search for SUID files:

```bash
find University-Project -type f -perm -4000 -ls
```

Search for SGID files:

```bash
find University-Project -type f -perm -2000 -ls
```

Identify symbolic links:

```bash
find University-Project -type l -ls
```

Inspect ACLs:

```bash
getfacl -R University-Project
```

### 12. Part J – File Integrity Verification

Calculate SHA-256 hashes:

```bash
sha256sum University-Project/documents/project.txt
sha256sum University-Project/evidence/evidence.txt
```

Save the hashes:

```bash
sha256sum University-Project/documents/project.txt University-Project/evidence/evidence.txt > University-Project/reports/integrity-hashes.txt
```

Verify:

```bash
sha256sum -c University-Project/reports/integrity-hashes.txt
```

### 13. Part K – Generate the Final Security Report

Create:

```bash
nano University-Project/reports/module02-final-report.txt
```

Use this template:

```text
LINUX FILESYSTEM AND PERMISSION SECURITY REPORT
================================================

STUDENT INFORMATION
-------------------
Student:
Course:
Module: 02
Assessment: Filesystem and Permission Labs

1. FILESYSTEM STRUCTURE
-----------------------
Describe the directory structure created.

2. PERMISSION CONFIGURATION
---------------------------
List the permissions assigned to each directory and file.

3. USER AND GROUP MANAGEMENT
----------------------------
Describe the user and group created.
Explain the purpose of the shared directory.

4. ACCESS CONTROL LISTS
-----------------------
Describe the ACL entries configured.
Explain the purpose of the ACL mask and default ACL.

5. SPECIAL PERMISSIONS
----------------------
Explain the SGID and sticky-bit configurations.

6. LINKS AND INODES
-------------------
Explain the difference between the hard link and symbolic link.

7. STORAGE INVESTIGATION
------------------------
Record observations from findmnt, df, and lsblk.

8. SECURITY AUDIT
-----------------
Summarize the audit results.
Identify any unexpected permissions or ownership.

9. FILE INTEGRITY
-----------------
Explain how SHA-256 verification was performed.

10. CONCLUSION
--------------
Summarize what you learned and identify important
filesystem security practices.
```

Save the report.

### 14. Assessment Verification

Run:

```bash
ls -lR University-Project
```

Run:

```bash
getfacl -R University-Project
```

Run:

```bash
find University-Project -type l -ls
```

Run:

```bash
find University-Project -type f -perm -4000 -ls
```

Run:

```bash
find University-Project -type f -perm -2000 -ls
```

Confirm that the expected files, directories, permissions, ACLs, and reports exist.

### 15. Final Knowledge Review

Review these concepts before moving to Module 03:

1. Linux filesystem hierarchy.
2. File types and inode numbers.
3. Traditional permissions.
4. Numeric and symbolic `chmod`.
5. Users, groups, and ownership.
6. Access Control Lists.
7. ACL masks and default ACLs.
8. SUID, SGID, and sticky bit.
9. Hard links and symbolic links.
10. Mount points and filesystem inspection.
11. World-writable permissions.
12. SHA-256 integrity verification.

### 16. Completion Checklist

- [ ] Created the university project workspace.
- [ ] Identified file types and inode numbers.
- [ ] Configured directory permissions.
- [ ] Configured file permissions.
- [ ] Created a dedicated user and group.
- [ ] Configured shared directory ownership.
- [ ] Configured named-user ACL access.
- [ ] Configured a default ACL.
- [ ] Verified SGID.
- [ ] Configured the sticky bit.
- [ ] Created a hard link.
- [ ] Created a symbolic link.
- [ ] Inspected mounted filesystems.
- [ ] Performed the filesystem security audit.
- [ ] Generated SHA-256 hashes.
- [ ] Verified file integrity.
- [ ] Completed the final report.

### 17. Expected Final Structure

```text
lab10/
└── University-Project/
    ├── documents/
    │   ├── project.txt
    │   ├── project-hardlink.txt
    │   └── project-symlink.txt
    ├── shared/
    │   ├── team.txt
    │   └── temp/
    ├── private/
    │   └── confidential.txt
    ├── evidence/
    │   └── evidence.txt
    └── reports/
        ├── integrity-hashes.txt
        └── module02-final-report.txt
```

**MODULE 02 COMPLETION STATUS:** Complete after all practical tasks and the final report are finished.

**NEXT MODULE:** Module 03 – Bash Scripting Challenges.
