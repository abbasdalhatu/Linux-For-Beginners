
# MODULE 17 – ADVANCED LINUX
## LESSON 04: LINUX AUTOMATION AND SCHEDULING

---

## 1. Introduction to Linux Automation

Linux automation is the process of using scripts, commands, and scheduling tools to perform tasks automatically.

Instead of manually repeating the same operations, administrators configure Linux to perform them at specific times or events.

### Examples of Linux automation

- Automatically backing up files.
- Monitoring disk space.
- Cleaning temporary files.
- Generating system reports.
- Checking network connectivity.
- Rotating and managing logs.
- Updating monitoring records.
- Running maintenance scripts.

### Why automation is important

Automation helps administrators:

1. Save time.
2. Reduce repetitive work.
3. Improve consistency.
4. Reduce human error.
5. Monitor systems continuously.
6. Perform scheduled maintenance.

### Learning objectives

By the end of this lesson, you should understand:

- Linux task scheduling.
- Cron and crontab.
- Systemd timers.
- One-time task scheduling with `at`.
- Automated backups.
- Script scheduling.
- Logging automated tasks.
- Monitoring scheduled jobs.

---

## 2. Understanding Cron

Cron is a time-based job scheduler used by Linux and Unix-like operating systems.

It allows commands and scripts to execute automatically at specified times.

For example, you can configure Linux to:

- Run a backup every day.
- Generate a report every week.
- Check disk space every hour.
- Run a maintenance script every month.

The cron daemon is commonly called `cron`.

### Check cron service status

```bash
systemctl status cron
```

If the service is installed but inactive, an administrator may start it with:

```bash
sudo systemctl start cron
```

Enable it to start automatically at boot:

```bash
sudo systemctl enable cron
```

Only perform service changes when needed and authorized.

### Check whether cron is installed

```bash
which crontab
```

If unavailable, inspect the package:

```bash
dpkg -l cron
```

On Debian-based systems, the package can be installed using:

```bash
sudo apt install cron
```

---

## 3. Understanding Crontab

A crontab is a file containing scheduled cron jobs.

To open your user's crontab:

```bash
crontab -e
```

To display scheduled jobs:

```bash
crontab -l
```

To remove your user's crontab:

```bash
crontab -r
```

**Warning:** `crontab -r` removes the entire user's crontab. Do not run it unless you intentionally want to remove all those scheduled jobs.

### Crontab syntax

A typical cron entry contains five time fields followed by a command.

```text
* * * * * command
```

The five fields represent:

| Position | Field | Allowed values |
|---|---|---|
| 1 | Minute | 0–59 |
| 2 | Hour | 0–23 |
| 3 | Day of month | 1–31 |
| 4 | Month | 1–12 |
| 5 | Day of week | 0–7 |

In many cron implementations, both 0 and 7 represent Sunday.

### Example

```text
30 8 * * * /home/user/backup.sh
```

Meaning:

Run `backup.sh` every day at 8:30 AM.

---

## 4. Understanding Cron Operators

Cron supports special characters that make scheduling easier.

### A. Asterisk (*)

Represents every possible value in that field.

```text
* * * * * command
```

Runs every minute.

### B. Comma (,)

Specifies multiple values.

```text
0 8,16 * * * command
```

Runs at 8:00 AM and 4:00 PM daily.

### C. Hyphen (-)

Specifies a range.

```text
0 9 * * 1-5 command
```

Runs at 9:00 AM from Monday through Friday.

### D. Slash (/)

Specifies an interval.

```text
*/10 * * * * command
```

Runs every 10 minutes.

### More examples

| Cron expression | Meaning |
|---|---|
| `0 * * * *` | Every hour |
| `0 0 * * *` | Every day at midnight |
| `0 7 * * 1` | Every Monday at 7 AM |
| `30 18 * * 5` | Every Friday at 6:30 PM |
| `*/15 * * * *` | Every 15 minutes |
| `0 8 1 * *` | First day of every month at 8 AM |

Always consider the machine's configured timezone when interpreting cron schedules.

---

## 5. Creating Your First Scheduled Task

We will create a harmless script that records the current date and time.

### Step 1: Create the laboratory directory

```bash
mkdir -p ~/module17-lab/lesson04
cd ~/module17-lab/lesson04
```

### Step 2: Create a script

```bash
nano time-log.sh
```

Enter:

```bash
#!/bin/bash

echo "Automation executed at: $(date)" >> "$HOME/module17-lab/lesson04/automation.log"
```

Save the file.

### Step 3: Make it executable

```bash
chmod +x time-log.sh
```

### Step 4: Test it manually

```bash
./time-log.sh
```

View the log:

```bash
cat automation.log
```

### Step 5: Find the absolute path

```bash
realpath time-log.sh
```

Copy the displayed path.

### Step 6: Schedule the task

Open your crontab:

```bash
crontab -e
```

Add this example entry, replacing the path with the actual absolute path:

```text
*/5 * * * * /home/YOUR_USERNAME/module17-lab/lesson04/time-log.sh
```

This schedules the script every five minutes.

Save and exit the editor.

### Step 7: Verify the scheduled task

```bash
crontab -l
```

Wait for the next five-minute interval.

Then inspect:

```bash
cat ~/module17-lab/lesson04/automation.log
```

You should see timestamps added by the scheduled script.

### Step 8: Remove the test job

When you finish testing, open:

```bash
crontab -e
```

Remove only the test entry you created and save.

This prevents unnecessary repeated logging.

---

## 6. Cron Environment Variables

Cron jobs often run with a limited environment compared with an interactive terminal.

This means a command that works manually might fail when scheduled.

For example, cron may not have the same `PATH` variable as your terminal.

### Best practices

1. Use absolute paths.
2. Avoid relying on interactive shell configuration.
3. Redirect output to a log file.
4. Check script permissions.
5. Ensure the required service is running.
6. Test the script manually before scheduling it.

Example:

```text
*/10 * * * * /usr/bin/python3 /home/user/script.py >> /home/user/script.log 2>&1
```

This runs a Python script every 10 minutes and records normal output and errors.

---

## 7. Understanding Systemd Timers

Systemd timers are another method of scheduling tasks in Linux.

They are integrated with the systemd service manager.

A systemd timer normally works with a corresponding service unit.

### Cron versus systemd timers

| Cron | Systemd timer |
|---|---|
| Traditional scheduler | Integrated with systemd |
| Uses crontab entries | Uses unit files |
| Simple time-based scheduling | Supports more advanced scheduling |
| Limited service integration | Integrates with systemd services |
| Familiar and widely used | Useful for system-level automation |

### Inspect existing timers

```bash
systemctl list-timers --all
```

This displays systemd timers, including timers that have not yet run.

### Inspect a timer

```bash
systemctl status timer-name.timer
```

Replace `timer-name.timer` with an actual timer unit.

### Timer examples

Systemd supports calendar expressions.

Examples:

```text
OnCalendar=daily
```

Runs daily.

```text
OnCalendar=weekly
```

Runs weekly.

```text
OnCalendar=hourly
```

Runs hourly.

```text
OnCalendar=Mon..Fri 09:00
```

Runs on weekdays at 9 AM.

---

## 8. Practical Lab: Create a Systemd Timer

We will create a user-level timer that writes timestamps into a log file.

This does not require root privileges.

### Step 1: Create the user systemd directory

```bash
mkdir -p ~/.config/systemd/user
```

### Step 2: Create the service unit

```bash
nano ~/.config/systemd/user/module17-log.service
```

Enter:

```ini
[Unit]
Description=Module 17 Automated Timestamp Logger

[Service]
Type=oneshot
ExecStart=/bin/bash -c 'echo "Timer executed at: $(date)" >> %h/module17-lab/lesson04/systemd-timer.log'
```

### Step 3: Create the timer unit

```bash
nano ~/.config/systemd/user/module17-log.timer
```

Enter:

```ini
[Unit]
Description=Run Module 17 Timestamp Logger Every Five Minutes

[Timer]
OnCalendar=*:0/5
Persistent=true
Unit=module17-log.service

[Install]
WantedBy=timers.target
```

### Step 4: Reload user systemd configuration

```bash
systemctl --user daemon-reload
```

### Step 5: Enable and start the timer

```bash
systemctl --user enable --now module17-log.timer
```

### Step 6: Check timer status

```bash
systemctl --user status module17-log.timer
```

### Step 7: List user timers

```bash
systemctl --user list-timers --all
```

### Step 8: Inspect service logs

```bash
journalctl --user -u module17-log.service
```

### Step 9: Inspect the generated file

```bash
cat ~/module17-lab/lesson04/systemd-timer.log
```

### Step 10: Clean up the timer

After testing, disable and stop the timer:

```bash
systemctl --user disable --now module17-log.timer
```

Remove the two unit files:

```bash
rm ~/.config/systemd/user/module17-log.timer
rm ~/.config/systemd/user/module17-log.service
```

Reload the configuration:

```bash
systemctl --user daemon-reload
```

This removes the scheduled test configuration while preserving your log file.

---

## 9. Understanding the at Command

The `at` command schedules a task to run once at a specified time.

Unlike cron, which is mainly used for recurring tasks, `at` is designed for one-time scheduling.

### Check whether at is installed

```bash
which at
```

### Schedule a one-time task

```bash
echo 'date >> ~/module17-lab/lesson04/at-log.txt' | at now + 2 minutes
```

This schedules a command to run approximately two minutes later.

### List pending jobs

```bash
atq
```

### Remove a scheduled job

```bash
atrm JOB_NUMBER
```

Replace `JOB_NUMBER` with the actual job number shown by `atq`.

### Check the at service

On Debian-based systems, the service is commonly called `atd`.

```bash
systemctl status atd
```

If the package is not installed, an administrator can install it using:

```bash
sudo apt install at
```

Only enable or start the service if you need to use `at`.

---

## 10. Automated Backups

Backups are copies of data stored separately so that information can be recovered if the original files are lost or damaged.

Linux administrators commonly automate backups.

### Types of backups

**Full backup**

Copies all selected data.

**Incremental backup**

Copies data changed since the most recent backup of any type.

**Differential backup**

Copies data changed since the last full backup.

### Using tar

The `tar` utility can create compressed archives.

Example:

```bash
tar -czf backup.tar.gz important-files/
```

Options:

| Option | Meaning |
|---|---|
| `-c` | Create archive |
| `-x` | Extract archive |
| `-z` | Use gzip compression |
| `-v` | Show files being processed |
| `-f` | Specify archive filename |

### View archive contents

```bash
tar -tzf backup.tar.gz
```

### Extract an archive

```bash
tar -xzf backup.tar.gz
```

Extract only into a suitable destination directory.

Never extract untrusted archives directly into sensitive system locations.

---

## 11. Practical Project: Automated Backup Script

We will create a backup script for a directory inside our laboratory.

### Step 1: Create sample data

```bash
mkdir -p ~/module17-lab/lesson04/backup-source
```

Create sample files:

```bash
echo "Linux automation practice" > ~/module17-lab/lesson04/backup-source/notes.txt

echo "Backup demonstration" > ~/module17-lab/lesson04/backup-source/report.txt
```

### Step 2: Create the backup script

```bash
nano ~/module17-lab/lesson04/backup.sh
```

Enter:

```bash
#!/bin/bash

set -Eeuo pipefail

SOURCE="$HOME/module17-lab/lesson04/backup-source"
DESTINATION="$HOME/module17-lab/lesson04/backups"

mkdir -p "$DESTINATION"

if [ ! -d "$SOURCE" ]; then
    echo "Backup source directory does not exist."
    exit 1
fi

BACKUP_NAME="backup-$(date +%Y%m%d-%H%M%S).tar.gz"

tar -czf "$DESTINATION/$BACKUP_NAME" -C "$SOURCE" .

echo "Backup completed successfully."
echo "Saved to: $DESTINATION/$BACKUP_NAME"
```

### Step 3: Make it executable

```bash
chmod +x ~/module17-lab/lesson04/backup.sh
```

### Step 4: Run the backup

```bash
~/module17-lab/lesson04/backup.sh
```

### Step 5: Inspect backup files

```bash
ls -lh ~/module17-lab/lesson04/backups
```

### Step 6: Verify archive contents

```bash
tar -tzf ~/module17-lab/lesson04/backups/backup-*.tar.gz
```

If multiple archives exist, use the exact filename displayed by `ls` instead of the wildcard.

### Step 7: Test restoration

Create a temporary restoration directory:

```bash
mkdir -p ~/module17-lab/lesson04/restore-test
```

Extract one archive:

```bash
tar -xzf ~/module17-lab/lesson04/backups/backup-YYYYMMDD-HHMMSS.tar.gz -C ~/module17-lab/lesson04/restore-test
```

Replace the example timestamp with the real filename.

Inspect the restored files:

```bash
ls -l ~/module17-lab/lesson04/restore-test
```

A backup is not fully trusted until restoration has been tested.

---

## 12. Monitoring Automated Tasks

Automation should be monitored to ensure scheduled tasks are actually working.

### Inspect cron logs

Depending on the distribution and logging configuration:

```bash
journalctl -u cron
```

On some Debian-based installations, cron messages may be recorded in:

```bash
grep CRON /var/log/syslog
```

The log file might not exist if that logging service is not configured.

### Inspect systemd timer activity

```bash
systemctl list-timers --all
```

### Inspect a systemd service

```bash
systemctl --user status module17-log.service
```

### Inspect service logs

```bash
journalctl --user -u module17-log.service
```

### Why monitoring matters

An automated task can fail because of:

- Incorrect file paths.
- Missing permissions.
- Missing dependencies.
- Incorrect schedules.
- Service failures.
- Insufficient disk space.
- Script errors.

---

## 13. Automation Security Principles

Automated jobs may run without direct user supervision.

Therefore, security is important.

Follow these practices:

1. Use the least privilege necessary.
2. Avoid running every scheduled task as root.
3. Protect scripts from unauthorized modification.
4. Use trusted absolute paths.
5. Validate inputs.
6. Monitor execution logs.
7. Avoid placing passwords directly in scripts.
8. Test backup restoration.
9. Remove obsolete scheduled jobs.
10. Review cron and systemd timer configurations regularly.

### Important security commands

List your scheduled cron jobs:

```bash
crontab -l
```

List system timers:

```bash
systemctl list-timers --all
```

Inspect system cron configuration:

```bash
ls -la /etc/cron.d/
```

Inspect user cron spool metadata:

```bash
sudo ls -la /var/spool/cron/crontabs/
```

The final command requires administrator privileges and is for inspection only. Do not edit spool files directly; use `crontab`.

---

## 14. Knowledge Check

Answer these questions in your notebook.

1. What is Linux automation?
2. What is cron?
3. What is a crontab?
4. Explain the five fields in a cron expression.
5. What does `*/10 * * * *` mean?
6. What is the difference between cron and systemd timers?
7. What is the purpose of the `at` command?
8. Explain the difference between full, incremental, and differential backups.
9. What does `tar -czf` do?
10. Why should backups be tested through restoration?
11. Why are absolute paths useful in scheduled jobs?
12. Identify four reasons an automated task might fail.

---

## 15. Important Commands Summary

| Command | Purpose |
|---|---|
| `systemctl status cron` | Inspect cron service |
| `crontab -e` | Edit user's scheduled jobs |
| `crontab -l` | List user's scheduled jobs |
| `crontab -r` | Remove user's entire crontab |
| `systemctl list-timers --all` | List systemd timers |
| `systemctl --user daemon-reload` | Reload user unit configuration |
| `at now + 2 minutes` | Schedule a one-time task |
| `atq` | List pending at jobs |
| `atrm JOB_NUMBER` | Remove a pending at job |
| `tar -czf` | Create compressed archive |
| `tar -tzf` | List archive contents |
| `tar -xzf` | Extract compressed archive |
| `journalctl -u cron` | Inspect cron service logs |
| `journalctl --user -u NAME.service` | Inspect a user service's logs |

---

## 16. Lesson Summary

In this lesson, you learned how to automate Linux administration tasks.

You covered:

- Linux automation fundamentals.
- Cron and crontab.
- Cron scheduling expressions.
- Systemd timers.
- One-time scheduling with `at`.
- Automated backups.
- Archive creation and restoration.
- Monitoring scheduled jobs.
- Automation security.

You also completed two practical projects:

1. Automated timestamp logging.
2. Automated backup creation and restoration.

### Completion checklist

- [ ] Understand Linux automation.
- [ ] Understand cron and crontab.
- [ ] Understand cron scheduling expressions.
- [ ] Create and test a cron job.
- [ ] Understand systemd timers.
- [ ] Create and test a user-level systemd timer.
- [ ] Understand one-time scheduling with `at`.
- [ ] Create an automated backup script.
- [ ] Verify backup contents.
- [ ] Test backup restoration.
- [ ] Understand automation security.
- [ ] Answer the knowledge-check questions.

**END OF LESSON 04**
