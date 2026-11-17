---
---

# Module 07 – Users and Permissions

# Lesson 08: Managing Groups in Linux

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what Linux groups are.
2. Understand primary and supplementary groups.
3. Create groups using `groupadd`.
4. Modify groups using `groupmod`.
5. Delete groups using `groupdel`.
6. Add and remove users from groups.
7. Inspect group information.
8. Apply group management to real-world security scenarios.

## 2. What Are Linux Groups?

A Linux group is a collection of user accounts.

Groups allow administrators to manage permissions for multiple users at once.

For example, a university server might have:

- Computer Science group
- Cybersecurity group
- Information Technology group

Instead of granting permissions to every user individually, an administrator can assign permissions to a group.

## 3. Understanding Primary and Supplementary Groups

### A. Primary Group

Every normal Linux user has a primary group.

It is the default group associated with files created by that user, subject to directory settings and explicit group changes.

Check a user's groups:

```bash
id username
```

Example output:

```text
uid=1000(abbas) gid=1000(abbas) groups=1000(abbas),27(sudo)
```

### B. Supplementary Groups

Supplementary groups provide additional access to resources.

A user can belong to several supplementary groups.

Add a user to a supplementary group:

```bash
sudo usermod -aG securitylab abbas
```

**Important:** Use `-aG` when appending a supplementary group. Using `-G` without `-a` replaces the existing supplementary group list.

## 4. Understanding Group Configuration Files

### /etc/group

This file stores group information.

View it:

```bash
cat /etc/group
```

Example:

```text
securitylab:x:1005:abbas,ahmad
```

| Field | Meaning |
|---|---|
| securitylab | Group name |
| x | Group password placeholder |
| 1005 | Group ID (GID) |
| abbas,ahmad | Supplementary members |

### /etc/gshadow

This file stores protected group information.

Inspect it:

```bash
sudo cat /etc/gshadow
```

Do not modify it manually during this lesson.

## 5. Creating Groups with groupadd

Syntax:

```bash
sudo groupadd groupname
```

Example:

```bash
sudo groupadd securitylab
```

Verify:

```bash
getent group securitylab
```

Create a group with a specific GID:

```bash
sudo groupadd -g 1500 securityteam
```

The GID must be available.

Check it:

```bash
getent group 1500
```

## 6. Adding Users to Groups

Syntax:

```bash
sudo usermod -aG groupname username
```

Example:

```bash
sudo usermod -aG securitylab abbas
```

Verify:

```bash
id abbas
```

You can also check:

```bash
getent group securitylab
```

Existing login sessions may not immediately recognise new memberships. Log out and back in, or start a new login session before testing access.

## 7. Removing Users from Groups

Use `gpasswd -d`:

```bash
sudo gpasswd -d username groupname
```

Example:

```bash
sudo gpasswd -d abbas securitylab
```

Verify:

```bash
id abbas
```

The supplementary group should no longer appear.

## 8. Changing Group Names with groupmod

Syntax:

```bash
sudo groupmod -n newname oldname
```

Example:

```bash
sudo groupmod -n cyberteam securitylab
```

Verify:

```bash
getent group cyberteam
```

### Changing a Group ID

```bash
sudo groupmod -g 1600 cyberteam
```

Before changing a GID, inspect files owned by the old GID. Those files may retain numeric ownership and require an ownership update.

## 9. Deleting Groups with groupdel

Syntax:

```bash
sudo groupdel groupname
```

Example:

```bash
sudo groupdel securitylab
```

Verify:

```bash
getent group securitylab
```

Linux normally refuses to delete a group that is the primary group of an existing user.

Deleting a group does not automatically change file ownership associated with its GID.

Inspect ownership before deleting groups.

## 10. Understanding the groups Command

Display your groups:

```bash
groups
```

Display another user's groups:

```bash
groups username
```

For more detailed information:

```bash
id username
```

## 11. Practical Laboratory

### Lab Objective

Create a group, add a practice user, inspect memberships, and remove the group safely.

Practice resources:

- Group: `securitylab`
- User: `labanalyst`

If you deleted `labanalyst` in Lesson 07, recreate it first:

```bash
sudo adduser labanalyst
```

### Step 1: Check Whether the Group Exists

```bash
getent group securitylab
```

If no result appears, continue.

### Step 2: Create the Group if Necessary

```bash
sudo groupadd securitylab
```

### Step 3: Verify the Group

```bash
getent group securitylab
```

### Step 4: Add the Practice User

```bash
sudo usermod -aG securitylab labanalyst
```

### Step 5: Verify Membership

```bash
id labanalyst
```

### Step 6: Inspect Group Membership

```bash
getent group securitylab
```

### Step 7: Remove the Practice User from the Group

```bash
sudo gpasswd -d labanalyst securitylab
```

### Step 8: Verify Removal

```bash
id labanalyst
```

### Step 9: Delete the Practice Group

Only if it is no longer needed and is not the primary group of an existing user:

```bash
sudo groupdel securitylab
```

### Step 10: Verify Group Deletion

```bash
getent group securitylab
```

## 12. Real-World Cybersecurity Applications

### A. Role-Based Access Control

Organisations can create groups such as:

- developers
- securityteam
- systemadmins
- auditors

### B. Shared Directories

A group can receive access to a shared directory without granting access to every system user.

### C. Least Privilege

Users should only belong to groups necessary for their responsibilities.

### D. Security Auditing

Administrators can inspect group membership using:

```bash
getent group
id username
groups username
```

## 13. Knowledge Check

1. What is a Linux group?
2. Why do administrators create groups?
3. What is the difference between primary and supplementary groups?
4. Which file stores Linux group information?
5. What is the purpose of `/etc/gshadow`?
6. Which command creates a group?
7. Which command adds a user to a supplementary group?
8. Why should you use `-aG` instead of `-G` when appending a group?
9. Which command removes a user from a supplementary group?
10. Which command renames a group?
11. Which command deletes a group?
12. What does GID mean?
13. Why should administrators audit group memberships?
14. What can happen to file ownership when a group is deleted?
15. What is the relationship between Linux groups and access control?

## 14. Lesson Summary

In this lesson, we learned:

- Linux groups organise users.
- Every normal user has a primary group.
- Users can have supplementary groups.
- `groupadd` creates groups.
- `groupmod` modifies groups.
- `groupdel` deletes groups.
- `usermod -aG` adds supplementary memberships.
- `gpasswd -d` removes supplementary membership.
- `/etc/group` stores group information.
- `/etc/gshadow` stores protected group information.

**Key Security Principle:** Give users only the group memberships and permissions they need to perform their responsibilities.

---

**End of Lesson 08**

**Next Lesson:** Sudo and Administrative Privileges.