
# MODULE 08 – LOG ANALYSIS AND INCIDENT RESPONSE

# LAB 06: SUSPICIOUS PROCESS AND ACTIVITY DETECTION

## 1. Introduction

A Linux system runs many processes in the background.

These processes perform different tasks, including:

- Running applications.
- Managing network connections.
- Handling user sessions.
- Managing system services.
- Performing scheduled tasks.
- Maintaining system operations.

However, attackers may also attempt to execute unauthorized processes after gaining access to a system.

Security analysts therefore monitor processes and system activity to identify unusual behavior.

In this lab, you will learn how to inspect Linux processes, identify suspicious indicators, examine network connections, and investigate scheduled activities.

**Important:** A process that looks unusual is not automatically malicious. Always investigate before reaching a conclusion.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Understand Linux processes.
2. Inspect running processes.
3. Identify process owners and IDs.
4. Understand parent and child processes.
5. Monitor CPU and memory usage.
6. Inspect network connections.
7. Examine running services.
8. Inspect scheduled tasks.
9. Identify suspicious activity indicators.
10. Produce a process investigation report.

## 3. Understanding Linux Processes

A process is an instance of a running program.

Every process normally has a unique Process ID (PID).

Example:

```text
PID     USER      COMMAND
1000    root      systemd
1200    root      sshd
1500    kali      bash
1600    kali      python
```

Important process terms:

| Term | Meaning |
|---|---|
| PID | Process ID |
| PPID | Parent Process ID |
| USER | Process owner |
| COMMAND | Executed command |
| CPU | Processor usage |
| MEM | Memory usage |

A process may start another process. This creates a parent-child relationship.

## 4. Practical Exercise 1: Display Running Processes

Run:

```bash
ps aux
```

This displays running processes.

Understand the columns:

- USER: account running the process.
- PID: process identifier.
- %CPU: CPU usage.
- %MEM: memory usage.
- STAT: process state.
- COMMAND: command associated with the process.

Display a shorter list:

```bash
ps aux | head -n 20
```

Identify:

1. Five running processes.
2. Their PIDs.
3. Their owners.
4. Their commands.

Record your observations.

## 5. Practical Exercise 2: Inspect Process Relationships

Run:

```bash
ps -ef
```

This displays processes in a different format.

Important columns:

- UID: process owner.
- PID: process identifier.
- PPID: parent process identifier.
- CMD: command.

Inspect the process tree:

```bash
pstree
```

If unavailable, check:

```bash
command -v pstree
```

A process tree helps analysts understand which process started another process.

For example:

```text
systemd
 ├── sshd
 │    └── sshd
 │         └── bash
 └── cron
```

Questions:

1. Which process is the parent?
2. Which processes are children?
3. Why is process ancestry useful during investigations?

## 6. Practical Exercise 3: Monitor System Activity

Run:

```bash
top
```

Observe:

- CPU utilization.
- Memory utilization.
- Running processes.
- Process IDs.
- Process owners.
- System load.

Press `q` to exit.

If installed, you may also run:

```bash
htop
```

Check whether it is available:

```bash
command -v htop
```

Do not terminate processes during this lab.

### Why This Matters

An unexpected process consuming large amounts of CPU or memory may deserve investigation.

However, legitimate activities such as updates, backups, and application workloads can also consume significant resources.

## 7. Practical Exercise 4: Inspect a Specific Process

Choose a process PID from your process list.

For example:

```bash
ps -p 1 -o pid,ppid,user,stat,%cpu,%mem,cmd
```

This inspects PID 1.

Replace `1` with another PID to inspect a different process.

You can also display process details using:

```bash
ps -o pid,ppid,user,group,stat,etime,cmd -p 1
```

Record:

- PID.
- PPID.
- Owner.
- Process state.
- Execution time.
- Command.

Do not assume that a process is suspicious just because you do not recognize its name.

## 8. Practical Exercise 5: Inspect Network Connections

Processes can communicate over networks.

Security analysts inspect network connections to understand which services and applications are communicating.

Run:

```bash
ss -tunap
```

The options mean:

- `t`: TCP.
- `u`: UDP.
- `n`: Numeric addresses and ports.
- `a`: All sockets.
- `p`: Display process information when permitted.

Inspect listening TCP sockets:

```bash
ss -tlnp
```

Inspect established TCP connections:

```bash
ss -tnp state established
```

Look for:

1. Local addresses.
2. Remote addresses.
3. Listening ports.
4. Process names.
5. Unexpected connections.

Some process information may require administrator privileges.

## 9. Practical Exercise 6: Inspect Open Files

A process may access files, sockets, and other system resources.

If `lsof` is installed, check:

```bash
command -v lsof
```

Inspect open files:

```bash
sudo lsof | head -n 30
```

Inspect files opened by a particular process:

```bash
sudo lsof -p 1
```

Replace `1` with a process ID of interest.

Why this matters:

- A process accessing an unexpected file may require investigation.
- Unexpected network sockets may be relevant.
- Open files can help identify application behavior.

Do not modify or delete files during this exercise.

## 10. Practical Exercise 7: Inspect System Services

Linux services often run continuously in the background.

List running services:

```bash
systemctl list-units --type=service --state=running
```

List installed service unit files:

```bash
systemctl list-unit-files --type=service
```

Inspect an individual service:

```bash
systemctl status cron
```

You may also inspect:

```bash
systemctl status ssh
```

If a service is unavailable, record that fact.

Look for:

- Service name.
- Description.
- Active state.
- Main PID.
- Recent log messages.

A service that is unfamiliar is not automatically malicious. Check its purpose and package ownership before drawing conclusions.

## 11. Practical Exercise 8: Inspect Scheduled Tasks

Attackers may attempt to establish persistence through scheduled tasks.

Linux uses scheduling mechanisms such as cron and systemd timers.

### A. Inspect System Cron Configuration

Read the system cron configuration:

```bash
sudo ls -lah /etc/cron.d/
```

Inspect cron directories:

```bash
sudo ls -lah /etc/cron.hourly/
sudo ls -lah /etc/cron.daily/
```

Read the main cron configuration:

```bash
sudo less /etc/crontab
```

Press `q` to exit.

### B. Inspect Your User's Scheduled Tasks

Run:

```bash
crontab -l
```

### C. Inspect Systemd Timers

Run:

```bash
systemctl list-timers --all
```

Look for:

- Unfamiliar scheduled commands.
- Unexpected execution intervals.
- Unknown scripts.
- Unusual file locations.
- Unexpected privileged execution.

Do not edit, delete, or disable scheduled tasks.

## 12. Practical Exercise 9: Inspect Process-Related Logs

Process and service activity may be recorded in system logs.

Inspect recent system messages:

```bash
sudo journalctl -p warning --since today --no-pager
```

Inspect service logs:

```bash
sudo journalctl -u cron --no-pager
```

Inspect kernel messages:

```bash
sudo journalctl -k --no-pager | tail -n 30
```

Search for service failures:

```bash
sudo journalctl --no-pager | grep -iE 'failed|error|terminated'
```

Record any relevant findings.

Remember that a warning or error does not automatically indicate an attack.

## 13. Practical Exercise 10: Analyze a Fictional Suspicious Activity Scenario

A security team receives a report about unusual system activity.

The following fictional evidence is supplied:

### Process Observations

```text
PID     PPID    USER       COMMAND
100     1       root       systemd
500     100     root       cron
800     500     www-data   /tmp/update-service
900     800     www-data   python3 /tmp/worker.py
```

### Network Observations

```text
PROCESS          LOCAL PORT       REMOTE ADDRESS
worker.py        0.0.0.0:8080     LISTEN
worker.py        192.0.2.10:5000  203.0.113.20:443
```

### Scheduled Task Observation

```text
*/5 * * * * www-data /tmp/update-service
```

All addresses are fictional documentation examples.

### Investigation Questions

1. Which process has PID 800?
2. Which process has PID 900?
3. What is the parent-child relationship?
4. Why might execution from `/tmp` deserve investigation?
5. What does the scheduled task do?
6. Why might a five-minute execution interval deserve attention?
7. What additional evidence should be collected?
8. Can you conclude that the processes are malicious from this information alone?

### Investigation Guidance

Examine:

- File ownership.
- File permissions.
- Package ownership, if applicable.
- File hashes.
- Process ancestry.
- Execution timestamps.
- Relevant service and authentication logs.
- Network connection context.

A temporary directory can contain legitimate files. Its location alone does not establish malicious activity.

## 14. Practical Exercise 11: Create a Process Investigation Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module08/lab06
```

Enter it:

```bash
cd ~/practical-labs/module08/lab06
```

Create the report:

```bash
nano suspicious-process-investigation-report.txt
```

Use this template:

```text
==================================================
MODULE 08 - LAB 06
SUSPICIOUS PROCESS AND ACTIVITY DETECTION
==================================================

1. SYSTEM INFORMATION

Hostname:
Date:
Operating System:

2. PROCESS INSPECTION

Processes Examined:
Process IDs:
Process Owners:
Parent Processes:
Observations:

3. RESOURCE MONITORING

High CPU Processes:
High Memory Processes:
Unusual Activity:

4. NETWORK INSPECTION

Listening Ports:
Established Connections:
Associated Processes:
Unexpected Connections:

5. SERVICE INSPECTION

Services Examined:
Running Services:
Failed Services:
Observations:

6. SCHEDULED TASK INSPECTION

Cron Configuration:
User Scheduled Tasks:
Systemd Timers:
Unusual Entries:

7. LOG INVESTIGATION

Relevant Log Sources:
Warnings:
Errors:
Related Events:

8. SECURITY ASSESSMENT

Suspicious Indicators:
Supporting Evidence:
Alternative Explanations:
Confidence Level:

9. RECOMMENDATIONS

1.
2.
3.
4.
5.

10. CONCLUSION

Investigation Summary:
Further Investigation Required:

==================================================
END OF REPORT
==================================================
```

Save and exit:

- CTRL + O
- ENTER
- CTRL + X

## 15. Knowledge Check

1. What is a Linux process?
2. What does PID mean?
3. What does PPID mean?
4. What command displays running processes?
5. What is the purpose of `pstree`?
6. How does `ss -tlnp` help security analysts?
7. What information can `lsof` provide?
8. Why should scheduled tasks be inspected?
9. Why is an unfamiliar process not automatically malware?
10. What evidence should be collected before declaring a process malicious?

## 16. Lab Completion Checklist

- [ ] Inspected running processes.
- [ ] Examined parent-child relationships.
- [ ] Monitored CPU and memory.
- [ ] Inspected individual process details.
- [ ] Examined listening ports and connections.
- [ ] Inspected open files where available.
- [ ] Reviewed running services.
- [ ] Inspected cron configuration.
- [ ] Reviewed systemd timers.
- [ ] Examined process-related logs.
- [ ] Analyzed the fictional incident.
- [ ] Created the investigation report.
- [ ] Completed the knowledge check.

## 17. Expected Outcome

You should now understand how Linux security analysts inspect processes, services, network connections, scheduled tasks, and system logs.

These techniques form an important foundation for detecting suspicious activity and investigating possible persistence mechanisms.

**Safety reminder:** This lab is read-only. Do not kill processes, delete files, disable services, modify cron jobs, or execute suspicious scripts. Conduct all cybersecurity investigations only on systems you own or are authorized to inspect.
