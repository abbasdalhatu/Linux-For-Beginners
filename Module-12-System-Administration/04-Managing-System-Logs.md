
# Module 12 – Lesson 04: Managing System Logs

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what Linux system logs are.
2. Understand why logs are important.
3. Identify common Linux log locations.
4. Use `journalctl` to inspect system logs.
5. Search logs for specific events.
6. Monitor logs in real time.
7. Create and analyse a basic log investigation report.

---

## 2. What Are Linux System Logs?

Linux system logs are records of events and activities that occur on a computer.

They help administrators understand what is happening inside a Linux system.

Logs can record:

- System startup and shutdown.
- User authentication attempts.
- Service failures.
- Application errors.
- Network-related events.
- Security-related activities.
- Hardware and kernel messages.

### Real-world example

Imagine a university server suddenly becomes unavailable.

The administrator needs to investigate:

- When did the problem begin?
- Did a service fail?
- Was there an authentication problem?
- Did the system run out of memory?
- Were there unusual system events?

System logs provide evidence that helps answer these questions.

**Important:** Logs are one of the most useful sources of evidence during troubleshooting and security investigations.

---

## 3. Why Are Logs Important?

Linux administrators use logs for several reasons.

### A. Troubleshooting

Logs help identify why an application or service stopped working.

### B. Security Monitoring

Logs can reveal suspicious login attempts and unusual system activities.

### C. Performance Investigation

Logs may contain information about resource problems and service delays.

### D. Accountability

Logs can help establish what happened and when it happened.

### E. Incident Investigation

Security professionals use logs to reconstruct events during an investigation.

---

## 4. Understanding Linux Log Locations

Many Linux distributions store traditional log files inside:

```bash
/var/log
```

Inspect the directory:

```bash
ls -lah /var/log
```

Some common log files and directories include:

| Location | Purpose |
|---|---|
| `/var/log/syslog` | General system messages on systems using rsyslog |
| `/var/log/auth.log` | Authentication-related events on systems using this log |
| `/var/log/kern.log` | Kernel-related messages on systems using this log |
| `/var/log/dpkg.log` | Debian package-management activity |
| `/var/log/apt/` | APT package-management logs |
| `/var/log/journal/` | Persistent systemd journal storage, when configured |

Not every Linux distribution uses every file listed above.

Kali Linux is Debian-based, so some Debian-style log locations may be available.

Check which files actually exist on your system before attempting to read them.

---

## 5. Understanding the systemd Journal

Modern Linux distributions commonly use `systemd-journald` to collect system messages.

The main command for viewing these records is:

```bash
journalctl
```

It allows administrators to inspect events from:

- The Linux kernel.
- System services.
- System startup.
- Applications that send messages to the journal.

### Display all available journal entries

```bash
journalctl
```

The output may open in a pager.

Press `q` to exit.

### Display the most recent entries

```bash
journalctl -n 20
```

This displays the latest 20 entries.

### Display logs from the current boot

```bash
journalctl -b
```

This is useful when investigating problems that occurred after the latest startup.

### Display logs from the previous boot

```bash
journalctl -b -1
```

This works when previous-boot journal records are available.

---

## 6. Understanding Log Priority Levels

Linux logs can have different severity levels.

Common systemd journal priorities include:

| Level | Name | Meaning |
|---|---|---|
| 0 | Emergency | System is unusable |
| 1 | Alert | Immediate action required |
| 2 | Critical | Serious problem |
| 3 | Error | An error occurred |
| 4 | Warning | Something may need attention |
| 5 | Notice | Significant normal event |
| 6 | Informational | General information |
| 7 | Debug | Detailed debugging information |

### Display error messages

```bash
journalctl -p err
```

### Display warning messages and more severe events

```bash
journalctl -p warning
```

### Display critical events

```bash
journalctl -p crit
```

These filters help administrators focus on important events.

---

## 7. Filtering Logs by Time

Administrators often investigate events that occurred within a particular period.

### Show logs from today

```bash
journalctl --since today
```

### Show logs from the last hour

```bash
journalctl --since "1 hour ago"
```

### Show logs from the last 30 minutes

```bash
journalctl --since "30 minutes ago"
```

### Show logs between two times

```bash
journalctl --since "2026-11-15 08:00:00" --until "2026-11-15 09:00:00"
```

Use timestamps appropriate to your system's local time.

---

## 8. Inspecting Service Logs

You can investigate the logs of a particular systemd service.

General syntax:

```bash
journalctl -u SERVICE_NAME
```

Example:

```bash
journalctl -u ssh
```

Display the latest 20 SSH service entries:

```bash
journalctl -u ssh -n 20
```

Display SSH service errors:

```bash
journalctl -u ssh -p err
```

Follow SSH service logs in real time:

```bash
journalctl -u ssh -f
```

Press `Ctrl + C` to stop following.

If SSH is not installed or has no journal entries, select another existing service.

---

## 9. Searching Logs with grep

`grep` allows administrators to search text for particular words or patterns.

Example:

```bash
journalctl | grep -i error
```

This searches journal output for the word `error`, ignoring case.

Search for failed events:

```bash
journalctl | grep -i failed
```

Search authentication-related messages:

```bash
journalctl | grep -i authentication
```

The results depend on the events recorded by your system.

### Understanding useful grep options

| Option | Purpose |
|---|---|
| `-i` | Ignore uppercase/lowercase differences |
| `-n` | Display matching line numbers |
| `-v` | Display lines that do not match |
| `-r` | Search directories recursively |
| `-E` | Use extended regular expressions |

Example:

```bash
journalctl | grep -in "error"
```

This displays matching lines with their line numbers.

---

## 10. Understanding Real-Time Log Monitoring

Sometimes administrators need to watch events as they happen.

The `-f` option follows new journal entries.

Example:

```bash
journalctl -f
```

The terminal continuously displays new journal messages.

Press `Ctrl + C` to stop.

You can also monitor a traditional log file with:

```bash
tail -f /var/log/syslog
```

This works only if that file exists.

### Why is real-time monitoring useful?

Imagine monitoring a web server during a maintenance operation.

The administrator can watch for:

- Service startup messages.
- Connection errors.
- Application failures.
- Unexpected warnings.

This makes it easier to identify problems immediately.

---

## 11. Understanding Log Rotation

Log files can grow over time.

If they are never managed, they may consume significant storage.

**Log rotation** is the process of managing log files by:

- Renaming older logs.
- Creating new log files.
- Compressing older records.
- Removing records according to retention policies.

A common Linux tool for this purpose is:

```bash
logrotate
```

Check whether it is installed:

```bash
command -v logrotate
```

Inspect its configuration if available:

```bash
ls -l /etc/logrotate.conf
```

Do not modify system log rotation settings during this lesson.

---

## 12. Practical Lab: Linux Log Investigation

### Objective

Investigate your Kali Linux journal and create a log analysis report.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson04
cd ~/module12-lab/lesson04
```

### Step 2: Inspect available traditional logs

```bash
ls -lah /var/log
```

Identify which log files and directories exist.

### Step 3: Display recent journal entries

```bash
journalctl -n 20 --no-pager
```

Observe the messages and timestamps.

### Step 4: Inspect current boot logs

```bash
journalctl -b -n 20 --no-pager
```

### Step 5: Search for error messages

```bash
journalctl -p err -n 20 --no-pager
```

You may see errors, or there may be no matching entries.

### Step 6: Search for warning messages

```bash
journalctl -p warning -n 20 --no-pager
```

### Step 7: Inspect logs from the last hour

```bash
journalctl --since "1 hour ago" -n 30 --no-pager
```

### Step 8: Search for failed events

```bash
journalctl --no-pager | grep -i failed | tail -20
```

No matching results is acceptable.

### Step 9: Check a service's logs

```bash
journalctl -u ssh -n 20 --no-pager
```

If SSH is unavailable, use another service that exists on your system.

### Step 10: Generate your investigation report

Run:

```bash
{
echo "LINUX SYSTEM LOG INVESTIGATION REPORT"
echo "====================================="
echo ""
echo "RECENT SYSTEM EVENTS:"
journalctl -n 15 --no-pager
echo ""
echo "CURRENT BOOT EVENTS:"
journalctl -b -n 15 --no-pager
echo ""
echo "RECENT ERRORS:"
journalctl -p err -n 15 --no-pager
echo ""
echo "RECENT WARNINGS:"
journalctl -p warning -n 15 --no-pager
echo ""
echo "FAILED EVENTS:"
journalctl --no-pager | grep -i failed | tail -10
} > log-investigation-report.txt 2>&1
```

### Step 11: Read your report

```bash
less log-investigation-report.txt
```

Press `q` to exit.

### Step 12: Verify the report

```bash
ls -lh log-investigation-report.txt
wc -l log-investigation-report.txt
```

Your report should contain recent system events, warnings, errors, and matching failed-event messages.

---

## 13. Cybersecurity Connection: Log Analysis

Log analysis is an important part of Security Operations Centre (SOC) work.

SOC analysts investigate system and network events to identify possible security incidents.

Examples of suspicious activities include:

- Repeated failed login attempts.
- Unexpected privilege escalation.
- Unusual service activity.
- Unexpected system reboots.
- Unrecognised account activity.

However, one failed login does not automatically mean an attack occurred.

Analysts investigate patterns, timestamps, account names, source addresses, and related events before drawing conclusions.

### Example investigation workflow

1. Identify a suspicious event.
2. Record its timestamp.
3. Identify the affected user or service.
4. Search for related events.
5. Compare events across relevant logs.
6. Document the findings.

**Remember:** Logs provide evidence, but they must be interpreted carefully.

---

## 14. Lesson Summary

You learned:

1. Linux logs record system and application activities.
2. `/var/log` stores many traditional log files.
3. `journalctl` displays systemd journal entries.
4. `journalctl -u` filters logs by service.
5. `journalctl -p` filters logs by priority.
6. `--since` and `--until` filter events by time.
7. `grep` searches log output.
8. `tail -f` and `journalctl -f` support live monitoring.
9. Log rotation helps manage growing log files.
10. Log analysis is essential for troubleshooting and cybersecurity investigations.

---

## 15. Knowledge Check

1. What are Linux system logs?
2. Why are logs important to system administrators?
3. Where are traditional Linux log files commonly stored?
4. What is the purpose of `journalctl`?
5. Which command displays logs from the current boot?
6. What does `journalctl -p err` do?
7. How can you inspect logs from the last hour?
8. What is the purpose of `grep`?
9. What is log rotation?
10. How can log analysis help a SOC analyst investigate suspicious activity?

---

## 16. Practical Completion Checklist

- [ ] I understand what Linux system logs are.
- [ ] I inspected `/var/log`.
- [ ] I viewed recent journal entries.
- [ ] I inspected current boot logs.
- [ ] I filtered errors and warnings.
- [ ] I searched for failed events.
- [ ] I inspected service logs.
- [ ] I generated `log-investigation-report.txt`.
- [ ] I understand how log analysis supports cybersecurity.

**Lesson 04 is complete when you understand the concepts and successfully generate your log investigation report.**
