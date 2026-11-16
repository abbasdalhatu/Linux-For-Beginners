
# PRACTICAL LABS
# MODULE 01 – LINUX COMMAND-LINE MASTERY

## LAB 09: REAL-WORLD COMMAND-LINE CHALLENGES

---

## 1. Lab Objectives

This lab combines your previous lessons into practical scenarios.

By the end, you should be able to:

1. Navigate Linux directories efficiently.
2. Manage files and directories.
3. Search and process text.
4. Build command pipelines.
5. Redirect output into reports.
6. Inspect users and processes.
7. Collect system information.
8. Troubleshoot common errors.
9. Analyze simulated security logs.
10. Complete realistic command-line investigations.

## 2. Prepare Your Lab Environment

Create the challenge directory:

```bash
mkdir -p ~/practical-labs/module01/lab09
cd ~/practical-labs/module01/lab09
```

Create separate directories for each challenge:

```bash
mkdir -p challenge01
mkdir -p challenge02
mkdir -p challenge03
mkdir -p challenge04
mkdir -p challenge05
mkdir -p challenge06
```

Verify:

```bash
find . -maxdepth 1 -type d
```

---

# CHALLENGE 01: Linux File Management

## Scenario

You have joined an IT department as a junior Linux administrator.

Your supervisor asks you to organize a collection of project files.

### Task 1: Create the directory structure

```bash
mkdir -p challenge01/company/{documents,logs,backups,reports}
```

### Task 2: Create sample files

```bash
touch challenge01/company/documents/employee-list.txt
touch challenge01/company/documents/project-notes.txt
touch challenge01/company/logs/application.log
touch challenge01/company/logs/security.log
touch challenge01/company/backups/backup-info.txt
```

### Task 3: Add sample information

```bash
echo "Employee records - Training Data" > challenge01/company/documents/employee-list.txt

echo "Linux server deployment project" > challenge01/company/documents/project-notes.txt

echo "INFO Application started" > challenge01/company/logs/application.log

echo "INFO Security monitoring started" > challenge01/company/logs/security.log
```

### Task 4: Inspect the directory structure

```bash
find challenge01/company -type f
```

### Task 5: Inspect file information

```bash
ls -lah challenge01/company/documents
```

### Task 6: Copy a document

```bash
cp challenge01/company/documents/project-notes.txt challenge01/company/backups/
```

### Task 7: Verify the backup

```bash
ls -lah challenge01/company/backups
```

**Expected outcome:** You should have an organized directory structure containing documents, logs, backups, and reports.

---

# CHALLENGE 02: Linux Log Analysis

## Scenario

You are assisting a cybersecurity team investigating a simulated application log.

The log contains normal events, warnings, and errors.

### Task 1: Create the log file

```bash
cat > challenge02/application.log <<'EOF'
10:00 INFO Application started
10:01 INFO User logged in
10:02 WARNING High memory usage
10:03 ERROR Database connection failed
10:04 INFO User logged out
10:05 ERROR Authentication failed
10:06 WARNING Disk space low
10:07 INFO Backup completed
10:08 ERROR Database connection failed
10:09 INFO Application stopped
EOF
```

### Task 2: Display the log

```bash
cat challenge02/application.log
```

### Task 3: Find all errors

```bash
grep "ERROR" challenge02/application.log
```

### Task 4: Count errors

```bash
grep -c "ERROR" challenge02/application.log
```

### Task 5: Find warnings

```bash
grep "WARNING" challenge02/application.log
```

### Task 6: Count event types

```bash
awk '{print $2}' challenge02/application.log | sort | uniq -c
```

### Task 7: Extract error details

```bash
grep "ERROR" challenge02/application.log | cut -d' ' -f3-
```

### Task 8: Save the investigation

```bash
{
    echo "APPLICATION LOG INVESTIGATION"
    echo "Date: $(date)"
    echo
    echo "TOTAL ERRORS:"
    grep -c "ERROR" challenge02/application.log
    echo
    echo "ERROR EVENTS:"
    grep "ERROR" challenge02/application.log
    echo
    echo "WARNING EVENTS:"
    grep "WARNING" challenge02/application.log
} | tee challenge02/investigation-report.txt
```

**Expected outcome:** A report identifying all simulated errors and warnings.

---

# CHALLENGE 03: Linux System Administration

## Scenario

Your supervisor wants a basic health report for a Linux workstation.

Collect information about the system.

### Task 1: Identify the operating system

```bash
cat /etc/os-release
```

### Task 2: Identify the kernel

```bash
uname -r
```

### Task 3: Identify the current user

```bash
whoami
```

### Task 4: Inspect CPU information

```bash
lscpu
```

### Task 5: Inspect memory

```bash
free -h
```

### Task 6: Inspect storage

```bash
df -h
```

### Task 7: Inspect uptime

```bash
uptime
```

### Task 8: Inspect processes

```bash
ps aux --sort=-%cpu | head
```

### Task 9: Inspect network interfaces

```bash
ip -brief addr
```

### Task 10: Save the report

```bash
{
    echo "LINUX WORKSTATION HEALTH REPORT"
    date
    echo
    echo "OPERATING SYSTEM:"
    grep PRETTY_NAME /etc/os-release
    echo
    echo "KERNEL:"
    uname -r
    echo
    echo "CURRENT USER:"
    whoami
    echo
    echo "MEMORY:"
    free -h
    echo
    echo "STORAGE:"
    df -h
    echo
    echo "UPTIME:"
    uptime
    echo
    echo "NETWORK:"
    ip -brief addr
} | tee challenge03/system-health-report.txt
```

**Expected outcome:** A system health report saved in the challenge directory.

---

# CHALLENGE 04: User and Permission Investigation

## Scenario

An administrator asks you to inspect the permissions of a collection of documents.

### Task 1: Create the files

```bash
mkdir -p challenge04/access-control
touch challenge04/access-control/public.txt
touch challenge04/access-control/internal.txt
touch challenge04/access-control/restricted.txt
```

### Task 2: Add sample content

```bash
echo "Public training information" > challenge04/access-control/public.txt

echo "Internal training information" > challenge04/access-control/internal.txt

echo "Restricted training information" > challenge04/access-control/restricted.txt
```

### Task 3: Set different permissions

```bash
chmod 644 challenge04/access-control/public.txt
chmod 640 challenge04/access-control/internal.txt
chmod 600 challenge04/access-control/restricted.txt
```

### Task 4: Inspect permissions

```bash
ls -l challenge04/access-control
```

### Task 5: Inspect ownership

```bash
stat challenge04/access-control/restricted.txt
```

### Task 6: Understand the permission values

| Permission | Numeric value |
|---|---:|
| Read | 4 |
| Write | 2 |
| Execute | 1 |

For example:

- `644`: Owner has read/write; group and others have read.
- `640`: Owner has read/write; group has read; others have no permissions.
- `600`: Only the owner has read/write permissions.

### Task 7: Inspect your account

```bash
id
groups
whoami
```

**Expected outcome:** You should understand how Linux permissions control access to files.

Note: The `chmod` commands change permissions only on your own challenge files.

---

# CHALLENGE 05: Process Monitoring

## Scenario

You are investigating the processes running on a Linux workstation.

### Task 1: Display running processes

```bash
ps aux
```

### Task 2: Find the highest CPU-consuming processes

```bash
ps aux --sort=-%cpu | head
```

### Task 3: Find the highest memory-consuming processes

```bash
ps aux --sort=-%mem | head
```

### Task 4: Inspect process relationships

```bash
ps -ef --forest
```

### Task 5: Create a test process

```bash
sleep 300 &
```

### Task 6: Find the test process

```bash
pgrep -a sleep
```

### Task 7: Inspect the process

Use its actual PID:

```bash
ps -o pid,ppid,stat,comm -p PID
```

### Task 8: Stop your test process

```bash
kill -TERM PID
```

### Task 9: Verify termination

```bash
pgrep -a sleep
```

**Expected outcome:** You should be able to identify, inspect, and safely terminate your own test process.

---

# CHALLENGE 06: Simulated Security Investigation

## Scenario

You are a junior security analyst investigating a simulated authentication log.

Your task is to identify failed login attempts and summarize the activity.

All records in this challenge are fictional training data.

### Task 1: Create the security log

```bash
cat > challenge06/auth.log <<'EOF'
Nov 15 09:00:01 server sshd: Accepted password for admin from 192.0.2.10
Nov 15 09:01:14 server sshd: Failed password for invalid user guest from 192.0.2.20
Nov 15 09:02:30 server sshd: Failed password for admin from 192.0.2.20
Nov 15 09:03:45 server sshd: Accepted password for analyst from 192.0.2.30
Nov 15 09:04:12 server sshd: Failed password for root from 192.0.2.20
Nov 15 09:05:00 server sshd: Failed password for admin from 192.0.2.40
Nov 15 09:06:22 server sshd: Accepted password for admin from 192.0.2.10
Nov 15 09:07:18 server sshd: Failed password for guest from 192.0.2.40
EOF
```

The addresses use the documentation-only IPv4 ranges. They do not identify actual login sources.

### Task 2: Display all failed logins

```bash
grep "Failed password" challenge06/auth.log
```

### Task 3: Count failed logins

```bash
grep -c "Failed password" challenge06/auth.log
```

### Task 4: Extract the source IP addresses

```bash
grep "Failed password" challenge06/auth.log | awk '{print $NF}'
```

### Task 5: Count attempts per source IP

```bash
grep "Failed password" challenge06/auth.log | awk '{print $NF}' | sort | uniq -c
```

### Task 6: Sort the sources by attempt count

```bash
grep "Failed password" challenge06/auth.log | awk '{print $NF}' | sort | uniq -c | sort -nr
```

### Task 7: Find accepted logins

```bash
grep "Accepted password" challenge06/auth.log
```

### Task 8: Identify usernames in failed attempts

```bash
grep "Failed password" challenge06/auth.log | awk '{print $(NF-3)}'
```

### Task 9: Create an investigation report

```bash
{
    echo "SIMULATED AUTHENTICATION INVESTIGATION"
    echo "Date: $(date)"
    echo
    echo "TOTAL FAILED LOGIN ATTEMPTS:"
    grep -c "Failed password" challenge06/auth.log
    echo
    echo "FAILED LOGIN SOURCES:"
    grep "Failed password" challenge06/auth.log | awk '{print $NF}' | sort | uniq -c | sort -nr
    echo
    echo "FAILED LOGIN USERNAMES:"
    grep "Failed password" challenge06/auth.log | awk '{print $(NF-3)}'
    echo
    echo "SUCCESSFUL LOGINS:"
    grep "Accepted password" challenge06/auth.log
} | tee challenge06/security-investigation.txt
```

### Task 10: Write your findings

Answer:

1. How many failed login attempts occurred?
2. Which source address generated the most failures?
3. Which usernames were targeted?
4. Were there successful logins?
5. Why should an analyst investigate repeated failed authentication attempts?
6. What additional evidence would be needed before concluding that an account was compromised?

**Expected outcome:** A simulated authentication investigation report.

---

## 3. Final Integrated Challenge

You are now acting as a junior Linux system administrator and security analyst.

Complete the following tasks using the skills from Labs 01–08.

1. Create an organized directory structure.
2. Create and manage files.
3. Search a simulated application log.
4. Extract and count error messages.
5. Inspect user and group information.
6. Inspect file permissions.
7. Inspect running processes.
8. Collect CPU, memory, and disk information.
9. Inspect network interfaces.
10. Troubleshoot a simulated missing-file error.
11. Investigate a simulated authentication log.
12. Generate a final report.

Save your final report as:

```text
challenge09/final-investigation-report.txt
```

Your report should contain:

- Investigation date.
- Linux hostname.
- Current user.
- Operating system and kernel.
- Memory and disk information.
- Process information.
- Network interface information.
- Application log findings.
- Authentication log findings.
- Troubleshooting findings.
- Final conclusion.

---

## 4. Professional Reporting Guidelines

A professional technical report should distinguish observations from conclusions.

Use this structure:

### 1. Executive Summary
Briefly explain what you investigated.

### 2. Scope
Identify the local VM and simulated files examined.

### 3. Methodology
List the commands and techniques used.

### 4. Findings
Present the evidence collected.

### 5. Analysis
Explain what the evidence suggests.

### 6. Recommendations
Suggest appropriate improvements.

### 7. Conclusion
Summarize the investigation.

Never describe simulated events as real incidents. Do not claim an account was compromised solely because failed login attempts were recorded.

---

## 5. Knowledge Check

Answer these questions:

1. What is the purpose of Linux directory management?
2. Why are pipelines useful?
3. How can you extract information from log files?
4. What is the difference between `grep` and `awk`?
5. How do Linux permissions protect files?
6. How can you identify a process using its PID?
7. How can you safely stop a process?
8. Which commands inspect memory and disk usage?
9. How can you investigate failed login attempts?
10. Why is evidence important during security investigations?
11. What is the difference between an observation and a conclusion?
12. Why should technical reports clearly identify simulated data?

---

## 6. Lab Completion Checklist

- [ ] Completed Challenge 01: File Management.
- [ ] Completed Challenge 02: Log Analysis.
- [ ] Completed Challenge 03: System Administration.
- [ ] Completed Challenge 04: User and Permission Investigation.
- [ ] Completed Challenge 05: Process Monitoring.
- [ ] Completed Challenge 06: Simulated Security Investigation.
- [ ] Completed the Final Integrated Challenge.
- [ ] Generated the final report.
- [ ] Reviewed the professional reporting guidelines.
- [ ] Completed the knowledge check.

**END OF LAB 09**
