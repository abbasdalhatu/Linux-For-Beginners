
# MODULE 17 – ADVANCED LINUX
## LESSON 13: LOGGING AND SYSTEM AUDITING

---

## 1. Introduction to Linux Logging

Linux logging is the process of recording events and activities that occur within an operating system.

Logs help administrators understand what happened on a computer, identify errors, investigate security incidents, and monitor system health.

Linux systems generate logs for:

- User authentication.
- System startup and shutdown.
- Application activity.
- Kernel events.
- Network services.
- Security events.
- Scheduled tasks.
- System errors.

### Learning objectives

By the end of this lesson, you should be able to:

1. Explain Linux logging.
2. Understand system log files.
3. Use `journalctl`.
4. Understand rsyslog.
5. Monitor authentication events.
6. Understand Linux auditing.
7. Use auditd and audit utilities.
8. Inspect system service logs.
9. Monitor suspicious events.
10. Build a Linux log analysis report.

---

## 2. Understanding the Importance of Logs

Logs provide historical information about system activities.

### Example

Suppose a Linux server suddenly stops responding.

An administrator can inspect logs to investigate:

- Whether a service crashed.
- Whether the system experienced memory pressure.
- Whether an authentication failure occurred.
- Whether the kernel reported an error.
- Whether a recent configuration change caused the problem.

### Why logging matters

| Purpose | Explanation |
|---|---|
| Troubleshooting | Helps identify system errors |
| Security monitoring | Reveals suspicious activities |
| Accountability | Records important user actions |
| Performance analysis | Helps investigate resource problems |
| Incident response | Supports investigation and evidence collection |
| Compliance | Helps meet recordkeeping requirements |

---

## 3. Linux Log File Locations

Many Linux systems store log files inside:

```text
/var/log/
```

### Display log directories

```bash
ls -lah /var/log/
```

### Common log files

| Log file | Purpose |
|---|---|
| `/var/log/syslog` | General system messages on systems using rsyslog |
| `/var/log/auth.log` | Authentication events on many Debian-based systems |
| `/var/log/kern.log` | Kernel messages on systems configured to use it |
| `/var/log/dpkg.log` | Package-management activity |
| `/var/log/apt/` | APT package-management logs |
| `/var/log/faillog` | Failed-login information on systems using this mechanism |
| `/var/log/btmp` | Failed login records |
| `/var/log/wtmp` | Login and logout records |

Not every distribution uses every file. Some systems rely primarily on the systemd journal.

### Inspect a log file

```bash
sudo less /var/log/auth.log
```

If the file does not exist, use the system journal instead.

Press `q` to exit.

### Display the most recent entries

```bash
sudo tail -n 20 /var/log/auth.log
```

### Follow a log in real time

```bash
sudo tail -f /var/log/auth.log
```

Press `Ctrl+C` to stop.

---

## 4. Understanding systemd Journal

Many modern Linux distributions use systemd's journal to collect system events.

The main command for inspecting journal entries is:

```bash
journalctl
```

### Display all available journal entries

```bash
journalctl
```

### Display current boot logs

```bash
journalctl -b
```

### Display previous boot logs

```bash
journalctl -b -1
```

This works only if previous-boot journal data is available.

### Display recent entries

```bash
journalctl -n 50
```

### Follow logs in real time

```bash
journalctl -f
```

### Display logs from the current boot with explanations

```bash
journalctl -b -p warning
```

### Common priority levels

| Priority | Name | Meaning |
|---|---|---|
| 0 | Emergency | System unusable |
| 1 | Alert | Immediate action required |
| 2 | Critical | Critical condition |
| 3 | Error | Error condition |
| 4 | Warning | Warning condition |
| 5 | Notice | Significant normal event |
| 6 | Informational | Informational message |
| 7 | Debug | Debugging information |

### Filter by priority

Show errors:

```bash
journalctl -p err
```

Show warnings and more severe events:

```bash
journalctl -p warning
```

Show only critical events:

```bash
journalctl -p crit
```

---

## 5. Filtering Journal Logs

Large systems can generate thousands of log entries.

Filtering helps administrators locate relevant information.

### Filter by service

```bash
journalctl -u ssh
```

### Filter SSH logs from the current boot

```bash
journalctl -u ssh -b
```

### Filter by time

```bash
journalctl --since today
```

```bash
journalctl --since "1 hour ago"
```

### Filter between two dates

```bash
journalctl --since "2026-11-14 08:00:00" --until "2026-11-14 18:00:00"
```

Adjust the example dates to the period you are investigating.

### Filter by executable

```bash
journalctl /usr/bin/sudo
```

Availability depends on whether the executable's events were recorded with the relevant metadata.

### Output logs in JSON format

```bash
journalctl -o json -n 5
```

JSON output is useful when integrating logs with automated analysis tools.

---

## 6. Understanding rsyslog

`rsyslog` is a system logging service that receives, processes, and stores log messages.

It can collect messages from:

- Local applications.
- System services.
- Kernel facilities.
- Remote logging sources.

### Check whether rsyslog is installed

```bash
command -v rsyslogd
```

### Check service status

```bash
systemctl status rsyslog
```

The service may not be installed or enabled on systems that rely primarily on journald.

### Main configuration location

```text
/etc/rsyslog.conf
```

Additional configuration files may be located in:

```text
/etc/rsyslog.d/
```

### Inspect configuration

```bash
cat /etc/rsyslog.conf
```

### Why centralized logging matters

Organizations often collect logs from multiple servers in one location.

Benefits include:

1. Easier monitoring.
2. Centralized investigation.
3. Better incident response.
4. Improved log retention.
5. Reduced dependence on individual machines.

For security, remote logs should be transmitted and stored using appropriate protections.

Do not modify rsyslog configuration during this lesson.

---

## 7. Authentication Log Analysis

Authentication logs help administrators investigate login activity.

They may record:

- Successful logins.
- Failed logins.
- Sudo activity.
- SSH authentication attempts.
- Account-related events.

### Inspect authentication logs

```bash
sudo tail -n 50 /var/log/auth.log
```

If unavailable:

```bash
journalctl -b | grep -i authentication
```

### Search for failed authentication events

```bash
sudo grep -i "failed" /var/log/auth.log | tail -n 20
```

### Search for successful SSH login events

```bash
sudo grep -i "accepted" /var/log/auth.log | tail -n 20
```

These searches work only if the relevant events are stored in that file.

### Inspect login history

```bash
last -n 20
```

### Inspect failed login history

```bash
sudo lastb -n 20
```

### Important security considerations

A failed login does not automatically indicate an attack.

Possible explanations include:

- Incorrect passwords.
- Forgotten credentials.
- Automated service attempts.
- Unauthorized login attempts.
- Configuration errors.

Always investigate the surrounding context.

---

## 8. Kernel Log Analysis

The Linux kernel records important events related to hardware, drivers, devices, and system operations.

### Display kernel messages

```bash
dmesg
```

### Display recent kernel messages

```bash
dmesg | tail -n 30
```

### Display readable timestamps

```bash
dmesg -T
```

### Search for errors

```bash
dmesg --level=err
```

### Search for warnings

```bash
dmesg --level=warn
```

### Inspect kernel messages through journalctl

```bash
journalctl -k -b
```

### Why kernel logs matter

They can help investigate:

- Driver failures.
- Hardware errors.
- Storage problems.
- Network interface issues.
- Kernel warnings.
- Device initialization failures.

Do not assume every kernel warning represents a serious failure.

---

## 9. Service Log Monitoring

Linux services often generate their own journal entries.

### Display running services

```bash
systemctl list-units --type=service --state=running
```

### Display failed services

```bash
systemctl --failed
```

### Inspect a service

```bash
systemctl status ssh
```

### Display service logs

```bash
journalctl -u ssh
```

### Follow service logs

```bash
journalctl -u ssh -f
```

Replace `ssh` with the name of another service when investigating it.

### Why service logs matter

They help identify:

- Service startup failures.
- Configuration errors.
- Unexpected restarts.
- Application crashes.
- Authentication issues.

---

## 10. Linux Auditing Fundamentals

Logging and auditing are related, but they are not identical.

### Logging

Logging records events generated by applications and system components.

### Auditing

Auditing focuses on recording and examining activities relevant to accountability, security, and policy enforcement.

Examples:

- Changes to sensitive files.
- Privileged operations.
- User authentication events.
- Security policy changes.
- Access to protected resources.

### Logging vs auditing

| Feature | Logging | Auditing |
|---|---|---|
| Main purpose | Record system events | Track security-relevant activities |
| Scope | Broad system activity | Defined audit events |
| Examples | Service errors | Sensitive file access |
| Tools | journald, rsyslog | auditd, auditctl, ausearch |

---

## 11. Understanding auditd

`auditd` is the Linux Audit daemon.

It records security-related events according to configured audit rules.

### Check availability

```bash
command -v auditctl
```

### Inspect audit service status

```bash
systemctl status auditd
```

### Inspect current audit rules

```bash
sudo auditctl -l
```

### Inspect audit subsystem status

```bash
sudo auditctl -s
```

### Search audit events

```bash
sudo ausearch -m USER_LOGIN
```

### Display an audit report

```bash
sudo aureport
```

### Important tools

| Tool | Purpose |
|---|---|
| `auditd` | Audit event collection daemon |
| `auditctl` | Inspect and manage audit rules |
| `ausearch` | Search audit records |
| `aureport` | Generate audit summaries |

Auditd may not be installed or configured in your Kali environment.

For this lesson, inspect availability and existing settings only. Do not add or delete audit rules.

---

## 12. Log Rotation and Retention

Log files can grow over time.

Log rotation prevents them from consuming excessive disk space.

Linux commonly uses `logrotate` to manage log files.

### Check availability

```bash
command -v logrotate
```

### Inspect the main configuration

```bash
cat /etc/logrotate.conf
```

### Inspect additional configurations

```bash
ls /etc/logrotate.d/
```

### Important log rotation terms

| Term | Meaning |
|---|---|
| Rotation | Replacing an active log with an archived one |
| Compression | Reducing archived log size |
| Retention | How long logs are kept |
| Frequency | How often logs are rotated |
| Archive | Older rotated log files |

### Why retention matters

Logs are useful only if they remain available long enough for investigation.

Organizations must balance:

- Storage capacity.
- Security requirements.
- Legal requirements.
- Privacy requirements.
- Investigation needs.

---

## 13. Common Security Events to Investigate

Administrators should monitor unusual patterns.

| Event | Possible explanation |
|---|---|
| Repeated failed logins | Password mistakes or unauthorized attempts |
| Unexpected sudo usage | Privilege misuse or administration |
| Unknown listening services | New application or unwanted service |
| Repeated service crashes | Software or configuration problem |
| Unexpected account creation | Legitimate administration or suspicious activity |
| Modified security configuration | Approved change or possible compromise |
| Unusual scheduled jobs | Automation or persistence activity |

A single event rarely proves malicious activity.

Investigate timestamps, user identities, process details, and related events.

---

## 14. Practical Lab: Linux Log Inspection

This exercise uses read-only inspection commands.

### Step 1: Create your lab directory

```bash
mkdir -p ~/module17-lab/lesson13
cd ~/module17-lab/lesson13
```

### Step 2: Inspect the log directory

```bash
ls -lah /var/log/
```

### Step 3: Inspect current boot logs

```bash
journalctl -b -n 50
```

### Step 4: Inspect warning events

```bash
journalctl -p warning -b
```

### Step 5: Inspect error events

```bash
journalctl -p err -b
```

### Step 6: Inspect authentication events

```bash
journalctl -b | grep -iE 'authentication|sudo|ssh'
```

### Step 7: Inspect kernel messages

```bash
dmesg -T | tail -n 30
```

### Step 8: Inspect service failures

```bash
systemctl --failed
```

### Step 9: Inspect login history

```bash
last -n 10
```

### Step 10: Check logging services

```bash
systemctl status systemd-journald
```

```bash
systemctl status rsyslog
```

One of these services may be unavailable depending on your system configuration.

### Step 11: Check auditd

```bash
command -v auditctl
```

If available:

```bash
sudo auditctl -s
```

### Step 12: Inspect log rotation

```bash
cat /etc/logrotate.conf
```

Record your findings in your notebook.

---

## 15. Practical Project: Build a Linux Log Analysis Report

Create a Bash script that gathers useful system log information.

### Step 1: Create the script

```bash
nano log-analysis.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson13/log-analysis-report.txt"

{
    echo "======================================"
    echo "       LINUX LOG ANALYSIS REPORT"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "CURRENT BOOT:"
    journalctl -b -n 10 --no-pager

    echo
    echo "WARNING EVENTS:"
    journalctl -p warning -b -n 30 --no-pager

    echo
    echo "ERROR EVENTS:"
    journalctl -p err -b -n 30 --no-pager

    echo
    echo "AUTHENTICATION EVENTS:"
    journalctl -b -n 100 --no-pager |
        grep -iE 'authentication|sudo|ssh' |
        tail -n 30

    echo
    echo "KERNEL MESSAGES:"
    dmesg -T | tail -n 30

    echo
    echo "FAILED SERVICES:"
    systemctl --failed

    echo
    echo "RECENT LOGIN HISTORY:"
    last -n 10

    echo
    echo "LOG ROTATION CONFIGURATION:"
    if command -v logrotate >/dev/null 2>&1; then
        cat /etc/logrotate.conf
    else
        echo "logrotate is not installed."
    fi

    echo
    echo "AUDIT TOOL AVAILABILITY:"
    if command -v auditctl >/dev/null 2>&1; then
        sudo auditctl -s
    else
        echo "Audit tools are not installed."
    fi

    echo
    echo "======================================"
    echo "       REPORT COMPLETE"
    echo "======================================"

} > "$REPORT" 2>&1

echo "Log analysis report saved to: $REPORT"
```

### Step 2: Make the script executable

```bash
chmod +x log-analysis.sh
```

### Step 3: Run the script

```bash
./log-analysis.sh
```

### Step 4: Read the report

```bash
less ~/module17-lab/lesson13/log-analysis-report.txt
```

Press `q` to exit.

### Step 5: Verify the report

```bash
ls -lh ~/module17-lab/lesson13/
```

**Security note:** System logs can contain usernames, IP addresses, authentication information, and other sensitive details. Keep the report private and do not publish it without reviewing its contents.

---

## 16. Professional Log Investigation Workflow

A security analyst should investigate log events systematically.

### Phase 1: Identify the event

Determine what happened.

Example:

A user account experienced several failed authentication attempts.

### Phase 2: Identify the timestamp

Record when the event occurred.

### Phase 3: Identify the source

Determine which account, process, service, or system generated the event.

### Phase 4: Collect related evidence

Examine:

- Authentication logs.
- Service logs.
- Kernel events.
- Process information.
- Network connections.

### Phase 5: Establish context

Determine whether the event is expected or unusual.

### Phase 6: Evaluate risk

Consider:

- Potential impact.
- Frequency.
- Account privileges.
- System exposure.
- Related events.

### Phase 7: Document findings

Record:

- Event description.
- Timestamp.
- Evidence.
- Affected component.
- Risk assessment.
- Recommended action.

### Phase 8: Verify and close

After an approved remediation, inspect the relevant logs to confirm that the problem has been addressed.

---

## 17. Important Security Recommendations

1. Enable appropriate logging.
2. Monitor authentication events.
3. Protect log files from unauthorized modification.
4. Configure suitable log retention.
5. Use centralized logging where appropriate.
6. Synchronize system clocks.
7. Monitor privileged activities.
8. Restrict access to sensitive logs.
9. Investigate unexpected system events.
10. Preserve evidence during incident response.

### Why time synchronization matters

Accurate timestamps help investigators correlate events across multiple machines.

Time synchronization services commonly use Network Time Protocol (NTP).

Inspect system time:

```bash
timedatectl
```

Inspect time synchronization status:

```bash
timedatectl timesync-status
```

The second command may not be supported by every configured time synchronization service.

---

## 18. Knowledge Check

Answer these questions in your notebook.

1. What is Linux logging?
2. Why are logs important to system administrators?
3. Where are many Linux log files stored?
4. What is systemd journald?
5. What is the purpose of `journalctl`?
6. Differentiate `journalctl -b` and `journalctl -f`.
7. What is rsyslog?
8. Why are authentication logs important?
9. What information can kernel logs provide?
10. Differentiate logging and auditing.
11. What is auditd?
12. What is the purpose of `ausearch`?
13. What is log rotation?
14. Why is centralized logging useful?
15. Why is time synchronization important during security investigations?

---

## 19. Important Commands Summary

| Command | Purpose |
|---|---|
| `ls -lah /var/log/` | Inspect log directory |
| `journalctl` | Display system journal |
| `journalctl -b` | Display current boot logs |
| `journalctl -f` | Follow journal events |
| `journalctl -p err` | Filter error events |
| `journalctl -u ssh` | Inspect SSH service logs |
| `journalctl --since today` | Filter logs by time |
| `dmesg` | Display kernel messages |
| `tail -f` | Follow a log file |
| `grep` | Search log content |
| `last` | Inspect login history |
| `lastb` | Inspect failed login records |
| `systemctl --failed` | Inspect failed services |
| `auditctl -l` | Inspect audit rules |
| `ausearch` | Search audit records |
| `aureport` | Generate audit reports |
| `logrotate` | Manage log rotation |
| `timedatectl` | Inspect system time |

---

## 20. Lesson Summary

In this lesson, you studied Linux logging and system auditing.

You learned:

- Linux logging fundamentals.
- Important log directories.
- systemd journal.
- `journalctl`.
- rsyslog.
- Authentication log analysis.
- Kernel log analysis.
- Service log monitoring.
- Linux auditing.
- auditd.
- Log rotation.
- Security event investigation.
- Professional log investigation workflows.

You also built a Linux log analysis report.

### Completion checklist

- [ ] Understand Linux logging.
- [ ] Inspect `/var/log/`.
- [ ] Use `journalctl`.
- [ ] Understand rsyslog.
- [ ] Inspect authentication events.
- [ ] Inspect kernel messages.
- [ ] Inspect service logs.
- [ ] Understand auditd.
- [ ] Understand log rotation.
- [ ] Complete the log inspection lab.
- [ ] Build the log analysis report.
- [ ] Understand professional log investigation.

**END OF LESSON 13**
