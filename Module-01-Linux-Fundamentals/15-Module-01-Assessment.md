# Module 01 – Final Assessment

# Linux Fundamentals: Introduction to Linux

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Introduction to Linux  
**Assessment:** Final Module Assessment  
**Level:** Beginner to Intermediate

---

## 1. Assessment Overview

Welcome to the final assessment for Module 01.

Throughout this module, you have studied the foundations of Linux, its history, architecture, distributions, command-line interface, terminal, users, permissions, and operating environment.

This assessment evaluates your theoretical knowledge and practical ability to work with Linux.

### Assessment Objectives

By completing this assessment, you should demonstrate that you can:

1. Explain Linux and its history.
2. Describe how operating systems work.
3. Identify popular Linux distributions.
4. Explain Linux architecture.
5. Compare Linux and Windows.
6. Understand the Linux kernel and shell.
7. Explain open-source software and GNU.
8. Identify Linux advantages and disadvantages.
9. Explain how Linux is used in cybersecurity.
10. Use essential Linux commands.
11. Navigate the Linux terminal.
12. Understand root and sudo.
13. Inspect a Linux environment.
14. Apply basic Linux knowledge in practical situations.

## 2. Assessment Instructions

Read these instructions carefully before answering.

1. Complete the assessment independently.
2. Do not immediately search for answers online.
3. Use your own words when answering theory questions.
4. Perform practical exercises inside your Kali Linux virtual machine.
5. Record the commands you execute.
6. Include actual command outputs where requested.
7. Do not perform tests against systems you do not own or have permission to assess.
8. Review your answers before submitting.

**Total Marks: 100**

**Recommended Duration: 2 hours**

**Pass Mark: 50%**

---

# SECTION A – MULTIPLE-CHOICE QUESTIONS

**Total: 20 Marks**

Choose the correct answer. Each question carries 1 mark.

### Question 1

What is Linux primarily?

A. A programming language

B. An operating system kernel

C. A database

D. A web browser

### Question 2

Who originally developed the Linux kernel?

A. Bill Gates

B. Richard Stallman

C. Linus Torvalds

D. Dennis Ritchie

### Question 3

In which year was the Linux kernel first announced?

A. 1983

B. 1991

C. 1995

D. 2001

### Question 4

What does GNU stand for?

A. General Network Utility

B. GNU's Not Unix

C. Global Network Unit

D. Graphical New Unix

### Question 5

What is the core component of Linux?

A. Shell

B. Terminal

C. Kernel

D. Desktop

### Question 6

Which of the following is a Debian-based Linux distribution?

A. Fedora

B. Arch Linux

C. Kali Linux

D. Red Hat Enterprise Linux

### Question 7

Which command displays the current working directory?

A. `ls`

B. `pwd`

C. `cd`

D. `mkdir`

### Question 8

Which command lists files and directories?

A. `ls`

B. `touch`

C. `rm`

D. `whoami`

### Question 9

Which command displays the current username?

A. `hostname`

B. `uname`

C. `whoami`

D. `date`

### Question 10

What is the root directory in Linux?

A. `/home`

B. `/root`

C. `/etc`

D. `/`

### Question 11

What is the purpose of the Linux shell?

A. To manage computer hardware directly

B. To interpret and execute commands

C. To store all system files

D. To replace the kernel

### Question 12

Which command displays Linux kernel information?

A. `uname -a`

B. `pwd`

C. `mkdir`

D. `clear`

### Question 13

What does UID mean?

A. User Information Directory

B. User Identification Data

C. User ID

D. Universal Internet Device

### Question 14

What is the UID of the root account?

A. 1000

B. 500

C. 1

D. 0

### Question 15

What is the primary purpose of sudo?

A. To delete users

B. To execute authorised commands with elevated privileges

C. To display files

D. To change the Linux distribution

### Question 16

Which command displays running processes?

A. `ps aux`

B. `pwd`

C. `touch`

D. `cat`

### Question 17

Which directory commonly stores Linux system logs?

A. `/home`

B. `/var/log`

C. `/bin`

D. `/root`

### Question 18

Which command displays network interfaces and their IP addresses?

A. `free -h`

B. `ip addr`

C. `df -h`

D. `whoami`

### Question 19

What does the Principle of Least Privilege mean?

A. Every user should have root access.

B. Users should receive only the permissions they need.

C. All files should have full permissions.

D. Security controls should be disabled.

### Question 20

Which of the following is a cybersecurity tool commonly used for network discovery?

A. Microsoft Word

B. Nmap

C. Calculator

D. Paint

---

# SECTION B – SHORT-ANSWER QUESTIONS

**Total: 20 Marks**

Answer all questions. Each question carries 2 marks.

### Question 1

Define Linux and explain the difference between the Linux kernel and a Linux distribution.

### Question 2

Briefly explain the history of Linux, mentioning Linus Torvalds and the GNU Project.

### Question 3

State four important functions of an operating system.

### Question 4

What is a Linux distribution? Give four examples.

### Question 5

Explain the difference between a terminal, shell, and kernel.

### Question 6

What is the difference between open-source and proprietary software?

### Question 7

State four advantages of using Linux.

### Question 8

Explain two ways Linux is used in cybersecurity.

### Question 9

What is the difference between an absolute path and a relative path?

### Question 10

Explain the difference between `sudo` and `su`.

---

# SECTION C – LINUX COMMAND-LINE PRACTICAL

**Total: 20 Marks**

Perform these exercises inside your Kali Linux virtual machine.

Record each command and its output in your assessment file.

### Task 1 – System Identification (3 Marks)

Run:

```bash
whoami
```

```bash
hostname
```

```bash
uname -a
```

Explain what each command displays.

### Task 2 – Filesystem Navigation (3 Marks)

Run:

```bash
pwd
```

```bash
ls -la
```

```bash
cd /etc
```

```bash
pwd
```

Explain what happened after executing each command.

### Task 3 – Directory and File Management (3 Marks)

Create a directory called:

```text
Module01-Assessment
```

Enter the directory.

Create a file called:

```text
linux-notes.txt
```

Write the following text into it:

```text
I am learning Linux for Ethical Hackers.
```

Use a suitable command to display the file contents.

### Task 4 – User and Privilege Inspection (3 Marks)

Run:

```bash
id
```

```bash
id -u
```

```bash
sudo -l
```

Explain what information each command provides.

### Task 5 – System Resource Inspection (3 Marks)

Run:

```bash
lscpu
```

```bash
free -h
```

```bash
df -h
```

Explain the purpose of each command.

### Task 6 – Network Inspection (3 Marks)

Run:

```bash
ip addr
```

```bash
ip route
```

```bash
ss -tuln
```

Explain what each command displays.

### Task 7 – Process Inspection (2 Marks)

Run:

```bash
ps aux | head
```

Then explain what a process is and what PID means.

---

# SECTION D – CYBERSECURITY SCENARIOS

**Total: 20 Marks**

Read each scenario carefully and provide a suitable answer.

### Scenario 1 – Suspicious User Privileges (5 Marks)

You are inspecting a Linux computer during an authorised security assessment.

You discover that an ordinary user has unnecessary administrative privileges.

Answer:

1. What is the security risk?
2. Why should ordinary users not receive unnecessary privileges?
3. Which Linux command can help inspect the user's identity and groups?
4. What security principle should be applied?
5. What would you recommend to the system administrator?

### Scenario 2 – Suspicious Network Service (5 Marks)

You are investigating a Linux server and discover an unexpected listening network port.

Answer:

1. What command can help identify listening ports?
2. Why might an unexpected listening port be dangerous?
3. How can you investigate the process associated with the service?
4. What should you check before disabling the service?
5. Why is documenting your findings important?

### Scenario 3 – System Log Investigation (5 Marks)

A system administrator reports several failed login attempts.

Answer:

1. Where might authentication-related logs be stored?
2. Which command can display recent system journal entries?
3. Why are authentication logs important?
4. What information might help identify suspicious login activity?
5. What should you recommend to improve account security?

### Scenario 4 – File Permission Misconfiguration (5 Marks)

During a security assessment, you discover a sensitive configuration file that allows every user to modify it.

Answer:

1. Why is this dangerous?
2. Which command can inspect the file's permissions?
3. Which command can change file permissions?
4. What is the Principle of Least Privilege?
5. What should the administrator do to correct the problem?

---

# SECTION E – FINAL PRACTICAL CHALLENGE

**Total: 20 Marks**

This section combines several concepts from Module 01.

Complete the following challenge inside your Kali Linux virtual machine.

## Challenge: Linux System Inspection

Your task is to perform a basic inspection of your Kali Linux system.

Create a directory:

```bash
mkdir -p ~/Module01-Final-Challenge
```

Enter it:

```bash
cd ~/Module01-Final-Challenge
```

Create a report file:

```bash
touch system-report.txt
```

### Task 1 – Identify the System (3 Marks)

Collect:

- Linux distribution.
- Kernel version.
- Hostname.
- Current username.

Useful commands:

```bash
cat /etc/os-release
uname -r
hostname
whoami
```

### Task 2 – Inspect User Privileges (3 Marks)

Collect:

- Current UID.
- Current GID.
- Group membership.
- Sudo permissions.

Useful commands:

```bash
id
groups
sudo -l
```

### Task 3 – Inspect System Resources (3 Marks)

Collect:

- CPU information.
- Memory usage.
- Storage usage.

Useful commands:

```bash
lscpu
free -h
df -h
```

### Task 4 – Inspect Processes and Services (3 Marks)

Collect:

- Running process information.
- Active system services.
- Failed system services.

Useful commands:

```bash
ps aux | head
systemctl list-units --type=service --state=running
systemctl --failed
```

### Task 5 – Inspect Networking (3 Marks)

Collect:

- Network interfaces.
- IP addresses.
- Routing information.
- Listening network sockets.

Useful commands:

```bash
ip addr
ip route
ss -tuln
```

### Task 6 – Create Your Final Report (5 Marks)

Use a text editor to write your findings into:

```text
system-report.txt
```

Your report must contain:

1. System identification.
2. User and privilege information.
3. CPU, RAM, and storage information.
4. Process and service information.
5. Network information.
6. A short conclusion about the state of your system.

You may use this command to edit the file:

```bash
nano system-report.txt
```

Save your report using `Ctrl + O`, press `Enter`, and exit using `Ctrl + X`.

Verify that the file exists:

```bash
ls -l system-report.txt
```

**Security note:** Your report is for your own lab. Remove sensitive details before sharing it publicly.

---

# 3. Assessment Marking Scheme

| Section | Marks |
|---|---:|
| Section A – Multiple Choice | 20 |
| Section B – Short Answers | 20 |
| Section C – Linux Practical | 20 |
| Section D – Cybersecurity Scenarios | 20 |
| Section E – Final Practical Challenge | 20 |
| **Total** | **100** |

## 4. Grading System

| Score | Grade | Performance |
|---|---|---|
| 80–100 | A | Excellent |
| 70–79 | B | Very Good |
| 60–69 | C | Good |
| 50–59 | D | Pass |
| 0–49 | F | Needs Improvement |

**Passing requirement: 50/100.**

The score is based on your demonstrated understanding and completed practical work.

## 5. Final Submission Checklist

Before considering Module 01 complete, confirm that you have:

- [ ] Answered all 20 multiple-choice questions.
- [ ] Answered all 10 short-answer questions.
- [ ] Completed all command-line practical tasks.
- [ ] Answered all four cybersecurity scenarios.
- [ ] Completed the final practical challenge.
- [ ] Created `system-report.txt`.
- [ ] Reviewed your answers.
- [ ] Saved your assessment file in the correct module folder.

## 6. Final Reflection

After completing this assessment, answer these questions:

1. Which lesson did you find most interesting?
2. Which Linux commands can you now use confidently?
3. Which topic was the most difficult?
4. What would you like to practise more?
5. How has your understanding of Linux changed since Lesson 01?

## 7. Module 01 Completion

Congratulations on reaching the end of Module 01.

You have studied the foundations of Linux, including its history, architecture, distributions, command-line interface, terminal fundamentals, users, permissions, and operating environment.

These concepts form the foundation for more advanced Linux administration and ethical hacking.

**Do not rush into the next module without completing this assessment.**

### Next Step

After completing the assessment, we will review your answers, identify weak areas, and confirm your readiness for:

**MODULE 02 – LINUX COMMAND-LINE MASTERY**

*Your journey toward becoming a skilled Linux user and ethical hacker continues.*