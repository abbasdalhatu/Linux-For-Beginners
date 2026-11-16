
# PRACTICAL LABS
# MODULE 01 – LINUX COMMAND-LINE MASTERY

## LAB 10: MODULE 01 PRACTICAL ASSESSMENT

---

## 1. Assessment Overview

This assessment evaluates your ability to use the Linux command line independently.

It covers the following topics:

1. Terminal environment and navigation.
2. File and directory operations.
3. Text processing and searching.
4. Pipes and redirection.
5. Users and environment variables.
6. Process inspection and control.
7. System information and diagnostics.
8. Command-line troubleshooting.
9. Real-world command-line challenges.

### Assessment Rules

- Perform all exercises inside your Kali Linux virtual machine.
- Use your dedicated assessment directory.
- Do not modify important system files.
- Do not terminate unknown processes.
- Use simulated data for security investigations.
- Record your commands and results.
- Explain what your commands accomplish.

**Total: 100 marks**

| Section | Marks |
|---|---:|
| A. Terminal and filesystem | 10 |
| B. Text processing | 10 |
| C. Pipes and redirection | 10 |
| D. Users and environment | 10 |
| E. Process management | 10 |
| F. System diagnostics | 10 |
| G. Troubleshooting | 10 |
| H. Security log investigation | 20 |
| I. Automation and reporting | 10 |
| **Total** | **100** |

---

# SECTION A: TERMINAL AND FILESYSTEM (10 MARKS)

## Task 1: Environment inspection

Run:

```bash
pwd
whoami
hostname
date
uname -a
```

Record the output.

## Task 2: Directory creation

Create this structure:

```text
module01-assessment/
├── documents/
├── logs/
├── reports/
├── scripts/
└── backups/
```

Command:

```bash
mkdir -p ~/practical-labs/module01-assessment/{documents,logs,reports,scripts,backups}
```

## Task 3: File management

Create:

```bash
touch ~/practical-labs/module01-assessment/documents/notes.txt
touch ~/practical-labs/module01-assessment/logs/system.log
touch ~/practical-labs/module01-assessment/reports/summary.txt
```

Add content to each file.

Copy `notes.txt` into the backups directory.

Verify the structure using:

```bash
find ~/practical-labs/module01-assessment -type f
```

---

# SECTION B: TEXT PROCESSING (10 MARKS)

## Task 1: Create a sample dataset

```bash
cat > ~/practical-labs/module01-assessment/documents/students.txt <<'EOF'
Abbas,Computer Science,85
Musa,Networking,72
Amina,Computer Science,91
Fatima,Cybersecurity,88
Ibrahim,Networking,65
Zainab,Cybersecurity,95
Abbas,Computer Science,85
EOF
```

## Task 2: Perform text analysis

1. Display the file.
2. Count the number of lines.
3. Search for Computer Science.
4. Display line numbers for Cybersecurity.
5. Extract student names.
6. Sort the names alphabetically.
7. Remove duplicate adjacent names after sorting.
8. Display the number of records.

Useful commands:

```bash
cat
wc
grep
cut
sort
uniq
awk
```

## Task 3: Save your results

Create:

```text
reports/text-analysis.txt
```

Save the analysis results in that file.

---

# SECTION C: PIPES AND REDIRECTION (10 MARKS)

## Task 1: Output redirection

```bash
echo "Linux Assessment" > assessment.txt
```

Append another line:

```bash
echo "Module 01" >> assessment.txt
```

## Task 2: Error redirection

```bash
ls /missing-assessment-directory 2> error-report.txt
```

Inspect the error report.

## Task 3: Pipeline challenge

Count the different departments in the student dataset.

```bash
cut -d',' -f2 documents/students.txt | sort | uniq -c
```

## Task 4: Use tee

Generate a system information report and display it while saving it to:

```text
reports/system-information.txt
```

---

# SECTION D: USERS AND ENVIRONMENT VARIABLES (10 MARKS)

Complete the following:

1. Display your current username.
2. Display your UID.
3. Display your primary group.
4. Display your supplementary groups.
5. Display your home directory.
6. Display your current shell.
7. Display your PATH variable.
8. Create three shell variables.
9. Export at least two variables.
10. Test whether an exported variable is inherited by a child Bash process.

Example:

```bash
export ASSESSMENT_NAME="Linux Practical"
export ASSESSMENT_LEVEL="Module 01"
```

Test:

```bash
bash -c 'echo "$ASSESSMENT_NAME"'
```

Record your observations.

---

# SECTION E: PROCESS MANAGEMENT (10 MARKS)

## Task 1: Process inspection

```bash
ps aux
```

## Task 2: Find Bash processes

```bash
pgrep -a bash
```

## Task 3: Create a background process

```bash
sleep 300 &
```

## Task 4: Inspect the process

Find its PID and inspect:

```bash
ps -o pid,ppid,stat,comm -p PID
```

Replace `PID` with the actual process ID.

## Task 5: Terminate your test process

```bash
kill -TERM PID
```

Verify that it has stopped.

## Task 6: Resource inspection

Display the processes using the most CPU and memory.

```bash
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
```

---

# SECTION F: SYSTEM DIAGNOSTICS (10 MARKS)

Collect the following information:

| Information | Command |
|---|---|
| Operating system | `cat /etc/os-release` |
| Kernel | `uname -r` |
| CPU | `lscpu` |
| Memory | `free -h` |
| Disk space | `df -h` |
| Directory size | `du -sh ~/practical-labs` |
| Uptime | `uptime` |
| Block devices | `lsblk` |
| Network interfaces | `ip -brief addr` |
| Routing table | `ip route` |
| Listening sockets | `ss -tuln` |

Save your results to:

```text
reports/diagnostics.txt
```

Write a short explanation of the system's current resource condition.

---

# SECTION G: TROUBLESHOOTING (10 MARKS)

Perform these controlled troubleshooting exercises.

## Task 1: Missing command

Attempt to execute:

```bash
unknowncommand123
```

Explain the error and identify how you would investigate it.

## Task 2: Missing file

```bash
cat nonexistent.txt
```

Investigate why the command fails.

## Task 3: Permission error

Create a test file:

```bash
echo "Permission test" > permission-test.txt
chmod u-r permission-test.txt
```

Attempt to read it.

Inspect its permissions and restore read permission:

```bash
chmod u+r permission-test.txt
```

Verify that the file is readable.

## Task 4: Network diagnosis

Run:

```bash
ip -brief addr
ip route
getent hosts debian.org
```

Explain what each command tells you.

Do not change network configuration to manufacture an error.

---

# SECTION H: SECURITY LOG INVESTIGATION (20 MARKS)

## Scenario

You are working as a junior security analyst.

Your team provides a simulated authentication log and asks you to investigate failed login attempts.

All records are fictional.

## Task 1: Create the log

```bash
cat > logs/authentication.log <<'EOF'
Nov 15 08:00:01 server sshd: Accepted password for admin from 192.0.2.10
Nov 15 08:01:15 server sshd: Failed password for guest from 192.0.2.20
Nov 15 08:02:22 server sshd: Failed password for admin from 192.0.2.20
Nov 15 08:03:31 server sshd: Failed password for root from 192.0.2.20
Nov 15 08:04:44 server sshd: Accepted password for analyst from 192.0.2.30
Nov 15 08:05:12 server sshd: Failed password for guest from 192.0.2.40
Nov 15 08:06:18 server sshd: Failed password for admin from 192.0.2.20
Nov 15 08:07:25 server sshd: Accepted password for admin from 192.0.2.10
Nov 15 08:08:33 server sshd: Failed password for root from 192.0.2.40
Nov 15 08:09:40 server sshd: Failed password for guest from 192.0.2.20
EOF
```

## Task 2: Identify failed authentication attempts

```bash
grep "Failed password" logs/authentication.log
```

## Task 3: Count failed attempts

```bash
grep -c "Failed password" logs/authentication.log
```

## Task 4: Extract source addresses

```bash
grep "Failed password" logs/authentication.log | awk '{print $NF}'
```

## Task 5: Count attempts per source address

```bash
grep "Failed password" logs/authentication.log | awk '{print $NF}' | sort | uniq -c | sort -nr
```

## Task 6: Identify targeted usernames

```bash
grep "Failed password" logs/authentication.log | awk '{print $(NF-3)}'
```

## Task 7: Identify successful logins

```bash
grep "Accepted password" logs/authentication.log
```

## Task 8: Prepare your findings

Answer these questions:

1. How many failed login attempts occurred?
2. Which source address generated the most failures?
3. Which usernames were targeted?
4. How many successful login events were recorded?
5. Why might repeated failures deserve investigation?
6. What additional evidence would you need before declaring an account compromised?

## Task 9: Produce a report

Save your investigation to:

```text
reports/security-investigation.txt
```

Your report must contain:

- Investigation title.
- Date of investigation.
- Total failed attempts.
- Source address statistics.
- Targeted usernames.
- Successful login events.
- Findings and recommendations.
- A statement identifying the records as simulated training data.

---

# SECTION I: AUTOMATION AND REPORTING (10 MARKS)

## Task 1: Create the final assessment script

Create:

```bash
nano scripts/final-report.sh
```

Enter:

```bash
#!/bin/bash

BASE="$HOME/practical-labs/module01-assessment"
REPORT="$BASE/reports/final-assessment-report.txt"

{
    echo "======================================"
    echo "       MODULE 01 FINAL REPORT"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "CURRENT USER:"
    whoami

    echo
    echo "OPERATING SYSTEM:"
    grep PRETTY_NAME /etc/os-release

    echo
    echo "KERNEL:"
    uname -r

    echo
    echo "CPU:"
    nproc

    echo
    echo "MEMORY:"
    free -h

    echo
    echo "DISK:"
    df -h

    echo
    echo "UPTIME:"
    uptime

    echo
    echo "NETWORK INTERFACES:"
    ip -brief addr

    echo
    echo "RUNNING PROCESSES:"
    ps -eo pid,comm --sort=pid | head -n 15

    echo
    echo "STUDENT DATASET:"
    wc -l "$BASE/documents/students.txt"

    echo
    echo "AUTHENTICATION LOG:"
    grep -c "Failed password" "$BASE/logs/authentication.log"

    echo
    echo "======================================"
    echo "       ASSESSMENT COMPLETED"
    echo "======================================"

} 2>&1 | tee "$REPORT"

echo
echo "Final report saved to: $REPORT"
```

Make it executable:

```bash
chmod +x scripts/final-report.sh
```

Run:

```bash
./scripts/final-report.sh
```

Inspect:

```bash
less reports/final-assessment-report.txt
```

---

## 7. Assessment Submission Checklist

- [ ] Section A completed.
- [ ] Section B completed.
- [ ] Section C completed.
- [ ] Section D completed.
- [ ] Section E completed.
- [ ] Section F completed.
- [ ] Section G completed.
- [ ] Section H completed.
- [ ] Section I completed.
- [ ] All reports saved.
- [ ] Final report generated.
- [ ] All temporary processes terminated.

---

## 8. Grading Guide

| Score | Performance |
|---|---|
| 90–100 | Excellent command-line proficiency |
| 80–89 | Very good practical proficiency |
| 70–79 | Good understanding |
| 60–69 | Developing practical skills |
| 50–59 | Basic understanding; further practice recommended |
| Below 50 | Revisit the lessons and repeat the exercises |

The pass mark for this assessment is **50/100**.

## 9. Module 01 Completion Statement

After completing this assessment, you should be comfortable with:

- Linux terminal navigation.
- File and directory management.
- Text processing.
- Pipelines and redirection.
- Users and environment variables.
- Process inspection and control.
- System diagnostics.
- Command-line troubleshooting.
- Basic log analysis.
- Shell-based reporting.

**END OF MODULE 01 – LINUX COMMAND-LINE MASTERY**

**NEXT MODULE: MODULE 02 – FILESYSTEM AND PERMISSION LABS**
