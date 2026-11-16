
# MODULE 08 – LOG ANALYSIS AND INCIDENT RESPONSE

# LAB 02: SYSTEM LOGS AND JOURNALD

## 1. Introduction

In Linux, system logging is essential for monitoring operating system activities, troubleshooting errors, and investigating security incidents.

One of the most important Linux logging components is **systemd-journald**.

Systemd-journald collects and manages log messages from different parts of the operating system.

It receives messages from:

- The Linux kernel.
- System services.
- Applications.
- Authentication processes.
- System startup and shutdown.
- Other system components.

In this lab, you will learn how to investigate system logs using `journalctl` and understand how the journal organizes information.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain systemd-journald.
2. Understand how journal entries are stored.
3. Filter logs by date and time.
4. Filter logs by service.
5. Search logs by priority.
6. Inspect boot-related events.
7. Export journal records.
8. Identify important system events.

## 3. Understanding systemd-journald

`systemd-journald` is a system service responsible for collecting and storing log information.

Unlike traditional plain-text logs, the systemd journal stores structured records containing fields such as:

- Timestamp.
- Hostname.
- Process ID.
- Service name.
- Message priority.
- Event description.

The journal can store logs in:

### A. Volatile Storage

Logs are stored temporarily, commonly under:

```text
/run/log/journal/
```

These logs normally disappear after a reboot.

### B. Persistent Storage

Logs are stored across reboots, commonly under:

```text
/var/log/journal/
```

Persistent storage depends on system configuration.

## 4. Practical Exercise 1: Check Journald Status

Run:

```bash
systemctl status systemd-journald
```

Observe:

- Service status.
- Process ID.
- Memory usage.
- Recent service messages.

Check whether the service is enabled or active:

```bash
systemctl is-active systemd-journald
```

Expected result:

```text
active
```

Inspect the service definition:

```bash
systemctl cat systemd-journald
```

Record your observations.

## 5. Practical Exercise 2: Inspect Journal Storage

Check whether persistent journal storage exists:

```bash
sudo ls -lah /var/log/journal/
```

Check volatile storage:

```bash
sudo ls -lah /run/log/journal/
```

One or both locations may exist.

Check journal disk usage:

```bash
sudo journalctl --disk-usage
```

This displays the amount of disk space currently occupied by journal files.

Record:

- Whether persistent storage exists.
- Whether volatile storage exists.
- Current journal disk usage.

Do not delete or clean journal files.

## 6. Practical Exercise 3: View Recent Journal Entries

Display the latest 20 entries:

```bash
sudo journalctl -n 20 --no-pager
```

Display the latest 50 entries:

```bash
sudo journalctl -n 50 --no-pager
```

Display entries in reverse chronological order:

```bash
sudo journalctl -r -n 20 --no-pager
```

The `-r` option displays newer entries first.

Identify:

1. The most recent event.
2. The service that generated it.
3. Its message.
4. Whether it indicates normal activity or a possible problem.

## 7. Practical Exercise 4: Filter Logs by Time

Security investigations often require examining events within a particular time period.

Display logs from today:

```bash
sudo journalctl --since today
```

Display logs from the last hour:

```bash
sudo journalctl --since "1 hour ago"
```

Display logs from the last 30 minutes:

```bash
sudo journalctl --since "30 minutes ago"
```

Display logs between two timestamps:

```bash
sudo journalctl --since "2026-11-15 09:00:00" --until "2026-11-15 12:00:00"
```

The example date is illustrative. Use an appropriate date and time for your investigation.

**Why this matters:** Filtering by time helps analysts narrow down large volumes of logs during an incident investigation.

## 8. Practical Exercise 5: Filter Logs by Service

Systemd services generate their own journal entries.

List running services:

```bash
systemctl list-units --type=service --state=running
```

Inspect NetworkManager logs if the service exists:

```bash
sudo journalctl -u NetworkManager --no-pager
```

Display the latest 20 NetworkManager entries:

```bash
sudo journalctl -u NetworkManager -n 20 --no-pager
```

Inspect SSH logs if the unit exists:

```bash
sudo journalctl -u ssh --no-pager
```

If that unit is unavailable, check:

```bash
systemctl list-units --all | grep -i ssh
```

Some distributions use `sshd` instead of `ssh`.

## 9. Practical Exercise 6: Filter by Priority

Journal messages have priority levels.

| Priority | Name | Meaning |
|---|---|---|
| 0 | Emergency | System unusable |
| 1 | Alert | Immediate action required |
| 2 | Critical | Critical condition |
| 3 | Error | Error occurred |
| 4 | Warning | Potential problem |
| 5 | Notice | Significant normal event |
| 6 | Informational | General information |
| 7 | Debug | Diagnostic information |

Display error messages:

```bash
sudo journalctl -p err --no-pager
```

Display critical messages:

```bash
sudo journalctl -p crit --no-pager
```

Display warning messages:

```bash
sudo journalctl -p warning --no-pager
```

Display errors and more severe messages:

```bash
sudo journalctl -p err..emerg --no-pager
```

Remember: lower priority numbers indicate greater severity.

## 10. Practical Exercise 7: Inspect Kernel Logs

The Linux kernel generates messages about:

- Hardware detection.
- Device drivers.
- Network interfaces.
- Storage devices.
- Kernel errors.
- System-level events.

Display kernel messages:

```bash
sudo journalctl -k --no-pager
```

Display the latest 30 kernel messages:

```bash
sudo journalctl -k -n 30 --no-pager
```

You can also use:

```bash
sudo dmesg
```

Display recent kernel messages:

```bash
sudo dmesg --level=err,warn
```

Record any warnings or errors you find.

A warning is not automatically evidence of a security incident. Consider the event's context.

## 11. Practical Exercise 8: Follow Logs in Real Time

Security administrators sometimes monitor logs while applications and services are running.

Run:

```bash
sudo journalctl -f
```

This displays new journal entries as they arrive.

Leave the terminal running briefly.

Open a second terminal and perform a harmless action, such as:

```bash
date
```

Or inspect a service:

```bash
systemctl status NetworkManager
```

Observe whether new journal entries appear.

Return to the first terminal and stop monitoring:

```text
CTRL + C
```

## 12. Practical Exercise 9: Export Journal Entries

Log records sometimes need to be saved for later analysis.

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module08/lab02
```

Enter it:

```bash
cd ~/practical-labs/module08/lab02
```

Export the latest 100 journal entries:

```bash
sudo journalctl -n 100 --no-pager > system-journal.txt
```

Check the file:

```bash
ls -lh system-journal.txt
```

Inspect its contents:

```bash
less system-journal.txt
```

Press `q` to exit.

**Important:** Exported logs may contain sensitive information. Protect them and do not upload them publicly.

## 13. Practical Exercise 10: Analyze a Fictional Journal Entry

Study this example:

```text
Nov 15 10:15:20 kali sshd[1400]: Failed password for invalid user test from 192.0.2.40 port 50020 ssh2
```

Break it down:

| Component | Value |
|---|---|
| Date | Nov 15 |
| Time | 10:15:20 |
| Hostname | kali |
| Service | sshd |
| Process ID | 1400 |
| Event | Failed password |
| Username | test |
| Source IP | 192.0.2.40 |
| Protocol | SSH |

### Investigation Questions

1. Which service generated the event?
2. What type of event occurred?
3. Which username was attempted?
4. What was the source IP address?
5. Why might this event be relevant to a security investigation?
6. What additional evidence would help determine whether the activity is malicious?

Remember: A single failed login does not prove that a system has been compromised.

## 14. Practical Exercise 11: Create a Journald Investigation Report

Create your report:

```bash
nano journald-investigation-report.txt
```

Use this template:

```text
==================================================
MODULE 08 - LAB 02
SYSTEM LOGS AND JOURNALD
==================================================

1. JOURNALD STATUS

Service Status:
Process ID:
Observations:

2. JOURNAL STORAGE

Persistent Storage:
Volatile Storage:
Journal Disk Usage:

3. RECENT JOURNAL ENTRIES

Latest Event:
Service:
Message:
Observation:

4. TIME-BASED FILTERING

Command Used:
Time Range:
Findings:

5. SERVICE LOG ANALYSIS

Service Name:
Command Used:
Findings:

6. PRIORITY FILTERING

Errors Found:
Warnings Found:
Critical Events:

7. KERNEL LOG ANALYSIS

Kernel Warnings:
Kernel Errors:
Observations:

8. REAL-TIME MONITORING

Monitoring Command:
Observed Events:

9. JOURNAL EXPORT

Exported File:
Number of Entries Requested:
File Size:

10. SECURITY ANALYSIS

Important Findings:
Possible Risks:
Recommended Actions:

==================================================
END OF REPORT
==================================================
```

Save and exit:

- CTRL + O
- ENTER
- CTRL + X

## 15. Knowledge Check

1. What is systemd-journald?
2. What is the purpose of `journalctl`?
3. What is the difference between persistent and volatile journal storage?
4. What does `journalctl -f` do?
5. How can you display logs from the last hour?
6. What does `journalctl -u` do?
7. What does `journalctl -k` display?
8. What is the purpose of journal priority levels?
9. Why is exporting logs useful during an investigation?
10. Why should exported logs be protected?

## 16. Lab Completion Checklist

- [ ] Checked systemd-journald status.
- [ ] Inspected journal storage.
- [ ] Checked journal disk usage.
- [ ] Viewed recent journal entries.
- [ ] Filtered logs by time.
- [ ] Filtered logs by service.
- [ ] Filtered logs by priority.
- [ ] Inspected kernel messages.
- [ ] Monitored journal events in real time.
- [ ] Exported journal entries.
- [ ] Analyzed a fictional security event.
- [ ] Created `journald-investigation-report.txt`.

## 17. Expected Outcome

You should now understand how systemd-journald collects and organizes Linux system events.

You should also be comfortable using `journalctl` to investigate events by time, service, priority, and system component.

**Security reminder:** Preserve relevant logs during investigations. Avoid deleting, editing, or modifying evidence.

