# Module 07 – Users and Permissions

# Lesson 15: Module Assessment

## 1. Assessment Instructions

This assessment evaluates your understanding of Linux users, groups, ownership, permissions, and access control.

### Instructions

1. Answer every question.
2. Use your Kali Linux VM for practical questions.
3. Do not copy answers without understanding them.
4. Show command syntax where required.
5. Complete the practical assessment inside your laboratory directory.

### Total Marks: 100

| Section | Topic | Marks |
|---|---|---:|
| A | Multiple Choice Questions | 20 |
| B | Short Answer Questions | 20 |
| C | Linux Commands and Scenarios | 30 |
| D | Practical Laboratory | 20 |
| E | Cybersecurity Analysis | 10 |
| **Total** | | **100** |

---

# SECTION A: MULTIPLE CHOICE QUESTIONS

**20 Questions – 1 Mark Each**

Choose the correct answer.

### 1. Which user has UID 0 in Linux?

A. Normal user  
B. Root user  
C. Guest user  
D. Nobody user

### 2. Which file stores basic user account information?

A. `/etc/group`  
B. `/etc/shadow`  
C. `/etc/passwd`  
D. `/etc/hosts`

### 3. Which file stores protected password information?

A. `/etc/shadow`  
B. `/etc/passwd`  
C. `/etc/profile`  
D. `/etc/sudoers`

### 4. Which command creates a Linux user with a home directory?

A. `groupadd`  
B. `useradd -m`  
C. `chmod`  
D. `chgrp`

### 5. Which command changes a user's password?

A. `passwd`  
B. `usermod`  
C. `userdel`  
D. `chown`

### 6. Which command locks a user's password authentication?

A. `passwd -u`  
B. `passwd -l`  
C. `useradd`  
D. `groupdel`

### 7. Which command adds a user to a supplementary group without replacing existing supplementary groups?

A. `usermod -G`  
B. `usermod -aG`  
C. `groupdel`  
D. `chmod`

### 8. Which command safely edits the sudoers configuration?

A. `nano /etc/passwd`  
B. `visudo`  
C. `passwd`  
D. `chgrp`

### 9. Which command displays file ownership and permissions?

A. `ls -l`  
B. `pwd`  
C. `whoami`  
D. `clear`

### 10. Which command changes file ownership?

A. `chmod`  
B. `chgrp`  
C. `chown`  
D. `umask`

### 11. What does `r` represent in Linux permissions?

A. Rename  
B. Read  
C. Run  
D. Remove

### 12. What does `x` represent for a directory?

A. Delete the directory  
B. Traverse the directory  
C. Rename the directory  
D. Change ownership

### 13. What does `chmod u+x script.sh` do?

A. Removes execute permission from the owner  
B. Adds execute permission to the owner  
C. Adds execute permission to everyone  
D. Changes file ownership

### 14. What does permission `644` represent?

A. `rwxrwxrwx`  
B. `rw-r--r--`  
C. `rwxr-xr-x`  
D. `rw-------`

### 15. What does permission `755` represent?

A. `rwxr-xr-x`  
B. `rw-r--r--`  
C. `rw-------`  
D. `rwx------`

### 16. What is the usual maximum starting permission mode for a newly created regular file?

A. 777  
B. 755  
C. 666  
D. 600

### 17. What does umask control?

A. Usernames  
B. Default permissions for newly created files and directories  
C. Network connections  
D. Running processes

### 18. What is the numeric value of Setuid?

A. 1  
B. 2  
C. 4  
D. 7

### 19. Which special permission is commonly used on `/tmp`?

A. Setuid  
B. Setgid  
C. Sticky Bit  
D. Read-only bit

### 20. Which permission configuration is generally most appropriate for a private file that only its owner should read and modify?

A. 777  
B. 755  
C. 644  
D. 600

---

# SECTION B: SHORT ANSWER QUESTIONS

**10 Questions – 2 Marks Each**

Answer in your own words.

1. Explain the difference between a Linux user and a Linux group.

2. What are UID and GID?

3. Explain the purpose of `/etc/passwd` and `/etc/shadow`.

4. What is the difference between `useradd` and `adduser`?

5. Explain the difference between locking and deleting a user account.

6. What is the purpose of the sudo command?

7. Explain the difference between `chown` and `chgrp`.

8. Identify the three Linux permission categories and explain them.

9. Explain the difference between symbolic and numeric permission modes.

10. Explain the purpose of umask.

---

# SECTION C: LINUX COMMANDS AND SCENARIOS

**6 Questions – 5 Marks Each**

Write the correct Linux command for each scenario.

### 1. User Management

Create a user named `studentlab` with a home directory.

### 2. Group Management

Create a group named `cyberteam`.

### 3. Group Membership

Add `studentlab` to `cyberteam` without removing the user's existing supplementary groups.

### 4. File Ownership

Change the owner of `report.txt` to `studentlab`.

### 5. Symbolic Permissions

Add execute permission to the owner of `script.sh`.

### 6. Numeric Permissions

Set `confidential.txt` to permission `600`.

---

# SECTION D: PRACTICAL LABORATORY

**20 Marks**

## Lab Objective

Demonstrate your ability to manage Linux users, groups, ownership, and permissions.

Use your Kali Linux VM.

Work inside:

```bash
~/module07-lab/assessment
```

### Task 1: Create the Laboratory Directory

```bash
mkdir -p ~/module07-lab/assessment
cd ~/module07-lab/assessment
```

### Task 2: Create Two Practice Files

```bash
touch public.txt
touch confidential.txt
```

### Task 3: Create a Practice Directory

```bash
mkdir team-files
```

### Task 4: Apply Permissions

Set:

- `public.txt` to `644`.
- `confidential.txt` to `600`.
- `team-files` to `750`.

### Task 5: Inspect Permissions

Run:

```bash
ls -l
```

Then:

```bash
stat public.txt confidential.txt team-files
```

### Task 6: Create a Practice Group

Check whether the group already exists:

```bash
getent group cyberteam
```

If it does not exist, create it:

```bash
sudo groupadd cyberteam
```

### Task 7: Change Group Ownership

```bash
sudo chgrp cyberteam team-files
```

### Task 8: Enable Setgid on the Directory

```bash
sudo chmod g+s team-files
```

### Task 9: Verify the Result

```bash
ls -ld team-files
```

Look for `s` in the group execute position.

### Task 10: Inspect the Complete Laboratory

```bash
ls -la ~/module07-lab/assessment
```

Take note of:

- File permissions.
- Directory permissions.
- Ownership.
- Group ownership.
- Special permission indicators.

Do not modify system directories or existing system executables.

---

# SECTION E: CYBERSECURITY ANALYSIS

**2 Questions – 5 Marks Each**

### 1. Scenario: Exposed Confidential File

A Linux server contains a file called:

```text
customer-database.txt
```

Its permissions are:

```text
-rw-r--r--
```

The file contains customer information.

Answer:

a. What does the permission string mean?

b. What security problem exists?

c. What permission configuration would you recommend if only the owner should read and modify the file?

d. Write the Linux command that applies your recommended permissions.

### 2. Scenario: Suspicious Setuid File

During a security audit, you discover an unfamiliar executable with Setuid enabled.

Answer:

a. What does Setuid do?

b. Why can an incorrectly configured Setuid executable be dangerous?

c. Which command can help identify Setuid files across a Linux filesystem?

d. Why should an administrator investigate before changing its permissions?

---

# FINAL SUBMISSION

Before submitting, verify that you have:

- [ ] Answered all multiple-choice questions.
- [ ] Completed all short-answer questions.
- [ ] Written all six Linux commands.
- [ ] Completed the practical laboratory.
- [ ] Answered both cybersecurity scenarios.
- [ ] Reviewed your answers.

## Grading Guide

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Requires improvement |

**Pass mark: 50/100**

---

# END OF MODULE 07

**Module:** Users and Permissions

**Next Module:** Module 08 – Process Management

**Congratulations on completing the learning materials for Module 07!**