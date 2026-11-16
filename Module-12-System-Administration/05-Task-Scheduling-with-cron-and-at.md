
# Module 12 – Lesson 05: Task Scheduling with cron and at

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain task scheduling in Linux.
2. Understand cron and crontab.
3. Understand the five fields of a cron expression.
4. Create and inspect scheduled tasks.
5. Understand one-time scheduling with `at`.
6. Manage scheduled tasks safely.
7. Create a simple automated system report.

---

## 2. What Is Task Scheduling?

Task scheduling is the process of configuring a computer to execute commands or programs automatically at specific times.

Instead of manually running the same command every day, an administrator can schedule Linux to execute it automatically.

### Real-world examples

A Linux administrator may schedule tasks to:

- Back up important files every night.
- Generate daily system reports.
- Check available disk space.
- Run maintenance scripts.
- Clean temporary application files.
- Perform routine monitoring.

### Example

Imagine a university server that needs to generate a system report every morning.

Without scheduling, an administrator must remember to run the report manually.

With task scheduling, Linux can generate it automatically.

---

## 3. Understanding cron

`cron` is a Linux scheduling service that executes commands or scripts at specified times.

It is commonly used for recurring tasks.

Examples:

- Every minute.
- Every hour.
- Every day.
- Every Sunday.
- On the first day of every month.

A scheduled cron task is called a **cron job**.

The command used to manage a user's cron jobs is:

```bash
crontab
```

---

## 4. Understanding crontab

A crontab is a file containing scheduled cron instructions.

To view your current user's cron jobs:

```bash
crontab -l
```

If no jobs exist, Linux may report that no crontab is installed for the user.

To edit your cron jobs:

```bash
crontab -e
```

This opens the user's crontab in a text editor.

To remove all cron jobs belonging to your current user:

```bash
crontab -r
```

**Warning:** Do not run `crontab -r` during this lesson. It removes the entire current user's crontab.

---

## 5. Understanding Cron Syntax

A standard cron expression has five time fields followed by the command.

General format:

```text
* * * * * command
```

The five fields represent:

| Field | Meaning | Allowed values |
|---|---|---|
| 1 | Minute | 0–59 |
| 2 | Hour | 0–23 |
| 3 | Day of month | 1–31 |
| 4 | Month | 1–12 |
| 5 | Day of week | 0–7 |

In many cron implementations, both `0` and `7` represent Sunday.

### Example

```text
30 8 * * * /home/user/backup.sh
```

Meaning:

Run `backup.sh` every day at 8:30 AM.

### Understanding the asterisk (*)

The asterisk means every possible value in that field.

For example:

```text
* * * * * command
```

This means every minute.

---

## 6. Practical Cron Examples

### Example 1: Every minute

```text
* * * * * echo "Hello Linux"
```

Runs every minute.

### Example 2: Every hour

```text
0 * * * * echo "Hourly task"
```

Runs at the beginning of every hour.

### Example 3: Every day at midnight

```text
0 0 * * * echo "Daily task"
```

Runs at 12:00 AM.

### Example 4: Every day at 6:30 AM

```text
30 6 * * * echo "Morning task"
```

### Example 5: Every Sunday at 9:00 AM

```text
0 9 * * 0 echo "Weekly task"
```

### Example 6: First day of every month

```text
0 0 1 * * echo "Monthly task"
```

### Example 7: Every five minutes

```text
*/5 * * * * echo "Five-minute task"
```

The `*/5` expression means every five minutes.

---

## 7. Understanding Special Cron Characters

| Character | Meaning | Example |
|---|---|---|
| `*` | Every value | `* * * * *` |
| `,` | Multiple values | `0 8,16 * * *` |
| `-` | Range | `0 9-17 * * *` |
| `/` | Step interval | `*/10 * * * *` |

### Example: Multiple hours

```text
0 8,16 * * * command
```

Runs at 8:00 AM and 4:00 PM every day.

### Example: Hour range

```text
0 9-17 * * * command
```

Runs every hour from 9:00 AM through 5:00 PM.

### Example: Every ten minutes

```text
*/10 * * * * command
```

Runs every ten minutes.

---

## 8. Understanding Cron Service Status

On systemd-based Linux systems, cron may run as a service.

Check its status:

```bash
systemctl status cron
```

On some distributions, the service may have a different name.

To inspect whether cron is installed:

```bash
command -v cron
```

To inspect the service unit:

```bash
systemctl list-unit-files --type=service | grep cron
```

Do not start or modify the system service unless it is necessary and you understand the effect.

---

## 9. Understanding at Scheduling

While cron is designed for recurring tasks, `at` is used to schedule a command or task to run once at a specified future time.

Examples:

- Run a backup once tonight.
- Generate a report tomorrow morning.
- Execute a maintenance command once next week.

General syntax:

```bash
at TIME
```

Example:

```bash
at 10:30
```

This opens an interactive prompt for entering commands scheduled for that time.

After entering a command, press `Ctrl + D` to submit the job.

The exact time expression and service availability depend on the installed `at` implementation.

### Inspect scheduled at jobs

```bash
atq
```

This lists pending jobs.

### Remove a scheduled job

```bash
atrm JOB_ID
```

Replace `JOB_ID` with the actual job number.

Only remove jobs that you created or are authorised to manage.

---

## 10. Cron vs at

| Feature | cron | at |
|---|---|---|
| Purpose | Recurring tasks | One-time tasks |
| Scheduling | Repeated execution | Single scheduled execution |
| Example | Every day at 8 AM | Tomorrow at 8 AM |
| Management | `crontab` | `at`, `atq`, `atrm` |

Remember:

- Use cron for repeated tasks.
- Use at for one-time future tasks.

---

## 11. Practical Lab: Creating a Scheduled System Report

### Objective

Create a simple report generator and schedule it to run automatically using cron.

All files will remain inside your personal lab directory.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson05
cd ~/module12-lab/lesson05
```

### Step 2: Create a monitoring script

```bash
nano system-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module12-lab/lesson05/scheduled-report.txt"

{
    echo "AUTOMATED LINUX SYSTEM REPORT"
    echo "============================="
    echo "Date: $(date)"
    echo ""
    echo "HOSTNAME:"
    hostname
    echo ""
    echo "UPTIME:"
    uptime
    echo ""
    echo "MEMORY:"
    free -h
    echo ""
    echo "DISK USAGE:"
    df -h
} > "$REPORT"
```

Save the file:

- Press `Ctrl + O`.
- Press `Enter`.
- Press `Ctrl + X`.

### Step 3: Make the script executable

```bash
chmod u+x system-report.sh
```

### Step 4: Run the script manually

```bash
./system-report.sh
```

### Step 5: Inspect the generated report

```bash
cat scheduled-report.txt
```

Confirm that the report contains the system information.

### Step 6: Check your current crontab

```bash
crontab -l
```

### Step 7: Add a temporary cron job

Open your crontab:

```bash
crontab -e
```

Add this line:

```text
*/5 * * * * /bin/bash /home/kali/module12-lab/lesson05/system-report.sh
```

This example assumes your Kali username is `kali`.

If your username is different, replace `/home/kali` with your actual home directory.

The job runs every five minutes.

Save and exit the editor.

### Step 8: Verify your cron job

```bash
crontab -l
```

Confirm that the scheduled command appears.

### Step 9: Wait and verify

Wait approximately five minutes, then run:

```bash
cat scheduled-report.txt
```

Check whether the report's timestamp has updated.

If it has not updated, investigate whether the cron service is running and whether your system permits cron jobs.

### Step 10: Remove the temporary cron job

After confirming the task works, open the crontab again:

```bash
crontab -e
```

Delete only the temporary line you added.

Save and exit.

Verify:

```bash
crontab -l
```

The temporary job should no longer appear.

**Important:** Do not use `crontab -r` because it would remove every cron job belonging to your user.

---

## 12. Optional Practical: Understanding at

Check whether `at` is available:

```bash
command -v at
```

If it is installed and the service is available, you can inspect its status:

```bash
systemctl status atd
```

The `atd` service handles scheduled one-time jobs on many Linux distributions.

You do not need to install it for this lesson.

The main objective is to understand the difference between recurring cron jobs and one-time at jobs.

---

## 13. Cybersecurity Connection

Task scheduling can be useful in both system administration and cybersecurity.

### Legitimate administrative uses

- Scheduled security reports.
- Regular backups.
- Routine system monitoring.
- Automated maintenance.

### Security investigation

Attackers sometimes abuse scheduled tasks to maintain persistence on compromised systems.

For example, an unauthorised cron job could repeatedly execute a suspicious script.

Security administrators should inspect scheduled tasks when investigating suspicious activity.

Useful inspection commands include:

```bash
crontab -l
```

```bash
ls -lah /etc/cron.d/
```

```bash
ls -lah /etc/cron.daily/
```

```bash
ls -lah /etc/cron.hourly/
```

Only inspect these locations during this exercise. Do not modify system-wide scheduled tasks.

---

## 14. Lesson Summary

You learned:

1. Task scheduling automates command execution.
2. Cron is used for recurring tasks.
3. Crontab stores scheduled cron jobs.
4. Cron uses five time fields.
5. Special characters control scheduling patterns.
6. At schedules one-time tasks.
7. `atq` lists pending one-time jobs.
8. `atrm` removes a selected pending job.
9. Scheduled tasks can automate system administration.
10. Unauthorised scheduled tasks may indicate suspicious activity.

---

## 15. Knowledge Check

1. What is task scheduling?
2. What is cron?
3. What is a cron job?
4. What is the purpose of `crontab -l`?
5. What are the five fields in a cron expression?
6. What does `*/5 * * * *` mean?
7. What is the difference between cron and at?
8. What does `atq` do?
9. Why should administrators inspect scheduled tasks?
10. How can attackers abuse scheduled tasks?

---

## 16. Practical Completion Checklist

- [ ] I understand task scheduling.
- [ ] I understand cron and crontab.
- [ ] I understand the five cron fields.
- [ ] I can interpret common cron expressions.
- [ ] I created `system-report.sh`.
- [ ] I ran the script successfully.
- [ ] I scheduled a temporary cron job.
- [ ] I verified that the scheduled task worked.
- [ ] I removed my temporary cron job.
- [ ] I understand one-time scheduling with at.
- [ ] I understand the cybersecurity implications of scheduled tasks.

**Lesson 05 is complete when you understand the concepts, successfully run your report script, and verify and remove your temporary cron job.**
