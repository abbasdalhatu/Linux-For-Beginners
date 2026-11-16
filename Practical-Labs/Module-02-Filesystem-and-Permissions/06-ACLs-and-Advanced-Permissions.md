
# PRACTICAL LABS
# MODULE 02 – FILESYSTEM AND PERMISSION LABS

## LAB 06: ACLs AND ADVANCED PERMISSIONS

### 1. Introduction

In Linux, traditional permissions provide access control for three categories:

- Owner
- Group
- Others

However, sometimes these three categories are not enough.

Imagine a university department with a shared project file.

You want:
- The file owner to have full access.
- The project group to have read and write access.
- One additional student to have read-only access.
- Everyone else to have no access.

Traditional permissions alone cannot conveniently express all these requirements.

Linux solves this problem using **Access Control Lists (ACLs)**.

ACLs allow administrators to assign permissions to specific users and groups beyond the traditional owner-group-others model.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain Access Control Lists.
2. Understand the difference between traditional permissions and ACLs.
3. Inspect ACL information.
4. Grant permissions to specific users.
5. Grant permissions to specific groups.
6. Modify and remove ACL entries.
7. Understand ACL masks.
8. Configure default ACLs on directories.
9. Audit ACL configurations.

### 3. Understanding Access Control Lists

An ACL is an additional access-control mechanism attached to a filesystem object.

Traditional permission example:

```text
-rw-r-----
```

This gives:
- Owner: Read and write.
- Group: Read.
- Others: No access.

An ACL can provide additional entries:

```text
user:student1:r--
user:student2:rw-
group:developers:r--
```

This means specific users and groups can receive permissions individually.

### 4. Check ACL Support

First inspect whether the ACL commands are available.

```bash
which getfacl
which setfacl
```

If they are missing, install the standard ACL utilities:

```bash
sudo apt update
sudo apt install acl
```

Verify:

```bash
getfacl --version
setfacl --version
```

Check your filesystem mount information:

```bash
findmnt -T ~
```

Modern Linux filesystems commonly support POSIX ACLs, but support depends on the filesystem and mount configuration.

### 5. Create Your Practical Workspace

```bash
mkdir -p ~/practical-labs/module02/lab06
cd ~/practical-labs/module02/lab06
```

Create sample files and directories:

```bash
touch project-report.txt
touch confidential-data.txt
mkdir shared-project
```

Add practice content:

```bash
echo "University project report" > project-report.txt
echo "Confidential practice information" > confidential-data.txt
```

Inspect:

```bash
ls -l
```

### 6. Understanding getfacl

The `getfacl` command displays ACL information.

Syntax:

```bash
getfacl filename
```

Inspect your files:

```bash
getfacl project-report.txt
getfacl confidential-data.txt
```

Example output:

```text
# file: project-report.txt
# owner: kali
# group: kali
user::rw-
group::r--
other::r--
```

Explanation:

| Entry | Meaning |
|---|---|
| `user::` | Owner permissions |
| `group::` | Owning group permissions |
| `other::` | Other users' permissions |

An extended ACL may contain additional named-user or named-group entries.

### 7. Grant Permissions to a Specific User

Create a dedicated test user if one does not already exist:

```bash
getent passwd acluser
```

If no result appears:

```bash
sudo useradd -m -s /bin/bash acluser
```

Set a temporary practice password:

```bash
sudo passwd acluser
```

Grant the user read-only access:

```bash
setfacl -m u:acluser:r-- project-report.txt
```

Inspect:

```bash
getfacl project-report.txt
```

You should see an entry similar to:

```text
user:acluser:r--
```

The test user can now receive read permission through the ACL, subject to directory traversal permissions and other access controls.

### 8. Grant Permissions to a Specific Group

Create a dedicated test group if it does not already exist:

```bash
getent group aclteam
```

If it does not exist:

```bash
sudo groupadd aclteam
```

Grant the group read and write access:

```bash
setfacl -m g:aclteam:rw- project-report.txt
```

Inspect:

```bash
getfacl project-report.txt
```

You should see:

```text
group:aclteam:rw-
```

### 9. Modify Existing ACL Entries

You can change an existing ACL entry using `setfacl -m`.

For example, change the user's permission from read-only to read and write:

```bash
setfacl -m u:acluser:rw- project-report.txt
```

Change it back:

```bash
setfacl -m u:acluser:r-- project-report.txt
```

Change the group's permission:

```bash
setfacl -m g:aclteam:r-- project-report.txt
```

Verify:

```bash
getfacl project-report.txt
```

### 10. Remove an ACL Entry

Use `setfacl -x` to remove a specific ACL entry.

Remove the test user's entry:

```bash
setfacl -x u:acluser project-report.txt
```

Remove the test group's entry:

```bash
setfacl -x g:aclteam project-report.txt
```

Inspect:

```bash
getfacl project-report.txt
```

To remove all extended ACL entries:

```bash
setfacl -b project-report.txt
```

Be careful: this removes extended ACL entries from the selected object.

### 11. Understanding ACL Masks

The ACL mask determines the maximum effective permissions for:

- The owning group entry.
- Named user entries.
- Named group entries.

It does not limit the file owner entry or the `other` entry.

Example:

```text
user::rw-
user:acluser:rwx
group::r--
mask::r--
other::---
```

Although `acluser` has an ACL entry of `rwx`, the mask limits the effective permissions to `r--`.

Inspect the ACL:

```bash
getfacl project-report.txt
```

Display effective permissions:

```bash
getfacl -e project-report.txt
```

Set the ACL mask:

```bash
setfacl -m m::r-- project-report.txt
```

Inspect again:

```bash
getfacl -e project-report.txt
```

Restore the mask to allow read and write:

```bash
setfacl -m m::rw- project-report.txt
```

Important: The mask is not an additional permission granted to users. It is a limit on the effective permissions of relevant ACL entries.

### 12. Understanding Default ACLs

Default ACLs are applied to newly created objects inside a directory.

They are useful when multiple users need consistent access to a shared workspace.

Create a directory:

```bash
mkdir team-space
```

Assign a default ACL for the test group:

```bash
setfacl -m d:g:aclteam:rwx team-space
```

Inspect:

```bash
getfacl team-space
```

You should see a default entry similar to:

```text
default:group:aclteam:rwx
```

Create a file inside:

```bash
touch team-space/new-report.txt
```

Inspect the new file:

```bash
getfacl team-space/new-report.txt
```

The new object should inherit an ACL entry for the group, with effective permissions affected by the requested creation mode and ACL mask.

### 13. Practical Exercise: Shared Project Access

You are managing a university project directory.

Requirements:

- Owner retains normal file access.
- A specific user receives read-only access.
- A project group receives read and write access.
- Other users receive no access.

Create a directory:

```bash
mkdir acl-project
```

Create a report:

```bash
echo "Shared university project information" > acl-project/report.txt
```

Set the traditional permissions:

```bash
chmod 640 acl-project/report.txt
```

Grant the specific user read access:

```bash
setfacl -m u:acluser:r-- acl-project/report.txt
```

Grant the project group read and write access:

```bash
setfacl -m g:aclteam:rw- acl-project/report.txt
```

Inspect:

```bash
getfacl acl-project/report.txt
```

Review the effective permissions carefully, including the ACL mask.

### 14. Understanding ACLs and Traditional Permissions

When an extended ACL exists, the traditional group permission bits shown by `ls -l` correspond to the ACL mask rather than necessarily representing only the owning group's entry.

Example:

```text
-rw-rw----+
```

The `+` indicates additional ACL information.

Inspect with:

```bash
ls -l project-report.txt
getfacl project-report.txt
```

Remember:

- `ls -l` provides a quick overview.
- `getfacl` provides detailed ACL information.
- The ACL mask can restrict effective permissions.

### 15. Security Investigation: Find Files with Extended ACLs

Extended ACLs can grant access that is not immediately obvious from a simple permission listing.

Search your lab directory:

```bash
getfacl -R .
```

Look for:

- Named user entries.
- Named group entries.
- Default ACL entries.
- Unexpected permissions.
- ACL masks that restrict effective access.

For a quick inventory:

```bash
find . -type f -exec sh -c '
    for file do
        if getfacl -cp "$file" | grep -qE "^(user:[^:]|group:[^:]|default:)"; then
            echo "Extended ACL: $file"
        fi
    done
' sh {} +
```

This is a basic inventory exercise, not a complete filesystem security scanner.

### 16. Generate an ACL Audit Report

Run:

```bash
cd ~/practical-labs/module02/lab06

{
    echo "LINUX ACL AUDIT REPORT"
    echo "======================"
    echo "Date: $(date)"
    echo "User: $(whoami)"
    echo
    echo "FILES AND DIRECTORIES:"
    ls -lah
    echo
    echo "ACL DETAILS:"
    getfacl -R .
    echo
    echo "FILES WITH EXTENDED ACLS:"
    find . -type f -exec sh -c '
        for file do
            if getfacl -cp "$file" | grep -qE "^(user:[^:]|group:[^:]|default:)"; then
                echo "$file"
            fi
        done
    ' sh {} +
} > acl-audit-report.txt
```

Read the report:

```bash
cat acl-audit-report.txt
```

### 17. Lab Cleanup

After saving your report, remove the dedicated test user and group if you created them for this lab.

Remove ACL entries from the practice objects:

```bash
setfacl -Rb ~/practical-labs/module02/lab06
```

This removes ACL entries recursively within your dedicated lab directory.

Check the result:

```bash
getfacl project-report.txt
```

If you created the dedicated test user:

```bash
sudo userdel -r acluser
```

If you created the dedicated group:

```bash
sudo groupdel aclteam
```

Only remove accounts and groups created specifically for this exercise. Do not remove accounts or groups used by other projects.

### 18. Knowledge Check

1. What does ACL stand for?
2. Why are ACLs useful?
3. Which command displays ACL information?
4. Which command modifies ACL entries?
5. What does `setfacl -m` do?
6. What does `setfacl -x` do?
7. What is an ACL mask?
8. What are default ACLs?
9. What does the `+` symbol in `ls -l` often indicate?
10. Why should security administrators audit extended ACLs?

### 19. Lab Completion Checklist

- [ ] Checked ACL utilities.
- [ ] Created the Lab 06 workspace.
- [ ] Inspected ACLs using `getfacl`.
- [ ] Created a dedicated test user if needed.
- [ ] Created a dedicated test group if needed.
- [ ] Granted permissions to a specific user.
- [ ] Granted permissions to a specific group.
- [ ] Modified ACL entries.
- [ ] Removed ACL entries.
- [ ] Practised ACL masks.
- [ ] Configured default ACLs.
- [ ] Investigated extended ACLs.
- [ ] Generated `acl-audit-report.txt`.
- [ ] Cleaned up temporary ACL entries and accounts safely.

### 20. Expected Workspace

```text
~/practical-labs/module02/lab06/
├── project-report.txt
├── confidential-data.txt
├── shared-project/
├── team-space/
│   └── new-report.txt
├── acl-project/
│   └── report.txt
└── acl-audit-report.txt
```

**Lab 06 Status:** Complete when the practical exercises and checklist are finished.

**Next Lab:** Lab 07 – Special Permissions.
