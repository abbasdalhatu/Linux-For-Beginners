---
---


# Module 12 – Lesson 09: Managing System Processes and Services

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a Linux process is.
2. Understand process IDs (PIDs) and parent process IDs (PPIDs).
3. Identify running processes.
4. Understand foreground and background processes.
5. Monitor process resource consumption.
6. Understand process priorities.
7. Manage processes safely.
8. Understand the relationship between processes and services.
9. Investigate suspicious processes from a cybersecurity perspective.

---

## 2. What Is a Process?

A process is an instance of a running program.

Whenever you execute a program, Linux creates a process to manage its execution.

### Examples

When you:

- Open Firefox, Linux runs browser processes.
- Execute a Python script, Linux creates a Python process.
- Run a command such as `ls`, Linux creates a process to execute it.
- Start an SSH server, Linux runs processes responsible for the service.

### Program vs Process

These terms are related but different.

| Program | Process |
|---|---|
| A stored set of instructions | A running instance of a program |
| Exists as a file | Exists while executing |
| Does not necessarily consume CPU | Can consume CPU and memory |
| Example: `script.py` | Example: running `python script.py` |

**Remember:** A program is something you can execute. A process is that program while it is running.

---

## 3. Understanding Process IDs (PID)

Every Linux process has a Process ID (PID).

A PID is a number used by the operating system to identify a process.

Example:

```text
PID     COMMAND
1       systemd
450     sshd
820     firefox
1200    python
```

These values are illustrative.

### Important process identifiers

**PID:** Process ID.

**PPID:** Parent Process ID.

The PPID identifies the process that created another process.

### Inspect your current shell

```bash
echo $$
```

This displays the PID of your current shell.

Inspect its parent:

```bash
ps -o pid,ppid,comm -p $$
```

This displays:

- PID
- PPID
- Command name

---

## 4. Viewing Running Processes

Linux provides several tools for process inspection.

### A. ps

The `ps` command displays process information.

```bash
ps
```

Display processes belonging to your current terminal:

```bash
ps -f
```

Display all processes:

```bash
ps aux
```

Important columns include:

| Column | Meaning |
|---|---|
| USER | Process owner |
| PID | Process ID |
| %CPU | CPU usage |
| %MEM | Memory usage |
| VSZ | Virtual memory size |
| RSS | Resident memory |
| STAT | Process state |
| COMMAND | Executed command |

### B. top

```bash
top
```

Displays a live view of running processes and system resource usage.

Useful keyboard controls:

- `q`: Quit.
- `P`: Sort by CPU usage.
- `M`: Sort by memory usage.

### C. pgrep

Search for processes by name.

```bash
pgrep bash
```

Display process IDs and command names:

```bash
pgrep -a bash
```

### D. pstree

Display processes in a tree structure:

```bash
pstree
```

This helps you understand parent-child process relationships.

If the command is unavailable, you can inspect it using:

```bash
command -v pstree
```

Do not install anything just for this exercise.

---

## 5. Understanding Process States

Linux processes can exist in different states.

Common process states include:

| State | Meaning |
|---|---|
| R | Running or runnable |
| S | Interruptible sleep |
| D | Uninterruptible sleep |
| T | Stopped |
| Z | Zombie |

### Running (R)

The process is currently executing or ready to execute.

### Sleeping (S)

The process is waiting for an event or resource.

### Uninterruptible Sleep (D)

The process is waiting for certain kernel operations, often involving I/O.

### Stopped (T)

The process has been suspended.

### Zombie (Z)

A process has finished executing, but its parent has not yet collected its exit status.

A zombie does not continue executing, although its process-table entry remains temporarily.

---

## 6. Foreground and Background Processes

A foreground process runs while controlling the terminal.

Example:

```bash
sleep 30
```

The terminal waits until the command finishes.

A background process runs without keeping the terminal occupied.

Example:

```bash
sleep 60 &
```

The `&` symbol tells the shell to start the command in the background.

### Display background jobs

```bash
jobs
```

### Bring a job into the foreground

```bash
fg
```

### Suspend a foreground process

Press:

```text
CTRL + Z
```

This suspends the foreground job.

### Resume it in the background

```bash
bg
```

### Important

These job-control commands manage jobs associated with your current shell.

---

## 7. Managing Processes with Signals

Linux uses signals to communicate with processes.

A signal can request that a process stop, continue, or terminate.

### Common signals

| Signal | Number | Purpose |
|---|---:|---|
| SIGHUP | 1 | Hangup |
| SIGINT | 2 | Interrupt |
| SIGKILL | 9 | Force termination |
| SIGTERM | 15 | Request termination |
| SIGSTOP | 19 on common x86 systems | Stop process |
| SIGCONT | 18 on common x86 systems | Continue process |

Signal numbers can vary by architecture. Signal names are more portable.

### A. kill

The `kill` command sends a signal to a process.

General syntax:

```bash
kill -SIGNAL PID
```

Example:

```bash
kill -TERM 1234
```

This requests termination of the process with PID 1234.

The PID is illustrative.

### B. pkill

Search for processes by name and send a signal.

```bash
pkill -TERM process_name
```

Be careful: multiple processes may match the name.

### C. killall

Send a signal to processes matching a command name.

```bash
killall -TERM process_name
```

Use these commands carefully, especially on production systems.

**Important:** Prefer SIGTERM first. SIGKILL prevents a process from handling a graceful shutdown.

Never terminate an unknown system process just because its name looks unfamiliar.

---

## 8. Understanding Process Priority

Linux schedules processes so they can share CPU time.

Priority and niceness influence how processes compete for CPU resources.

### Nice value

The nice value generally ranges from:

```text
-20 to +19
```

| Value | General Meaning |
|---|---|
| -20 | Highest scheduling priority preference |
| 0 | Default nice value |
| +19 | Lowest scheduling priority preference |

A lower nice value generally gives a process a more favourable CPU scheduling priority.

### Inspect process priority

```bash
ps -eo pid,ni,comm
```

### Start a process with a nice value

```bash
nice -n 10 sleep 60
```

This starts a sleep process with a nice value of 10.

### Change the priority of an existing process

```bash
renice 10 -p PID
```

Replace `PID` with a process you own and have identified.

Avoid changing the priority of system-critical processes.

---

## 9. Monitoring CPU and Memory Consumption

Administrators monitor processes to identify resource problems.

### View CPU and memory usage

```bash
ps aux
```

### Sort by CPU usage

```bash
ps aux --sort=-%cpu
```

### Sort by memory usage

```bash
ps aux --sort=-%mem
```

### Display memory usage

```bash
free -h
```

### Monitor continuously

```bash
top
```

### Why is this important?

A process consuming unusually high resources may indicate:

- An application malfunction.
- A memory leak.
- Excessive workload.
- An unexpected background task.
- Potentially malicious activity.

High resource usage alone does not prove malware.

---

## 10. Understanding Processes and Services

A process and a service are related, but they are not identical.

A process is a running program instance.

A service is a system function that is usually managed in the background.

For example:

- SSH service provides remote login.
- Web server service delivers web pages.
- Database service manages stored data.
- Logging service records system events.

On modern Kali Linux systems, systemd commonly manages services.

### Inspect a service

```bash
systemctl status ssh
```

The SSH service may not be installed or active on your system. That is fine.

### List running services

```bash
systemctl list-units --type=service --state=running
```

### Inspect service processes

```bash
ps aux
```

### View service logs

```bash
journalctl -u ssh --no-pager -n 20
```

A service can manage one or more processes, depending on its design.

---

## 11. Practical Lab: Process Management

### Objective

Create a controlled background process, inspect it, change its priority, and terminate it safely.

All exercises will use a harmless `sleep` process.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson09
cd ~/module12-lab/lesson09
```

### Step 2: Start a background process

```bash
sleep 300 &
```

The terminal should display a job number and PID.

### Step 3: Display background jobs

```bash
jobs -l
```

Record the PID.

### Step 4: Inspect the process

```bash
ps -p PID -o pid,ppid,stat,ni,comm
```

Replace `PID` with the actual number from your terminal.

### Step 5: Inspect its parent process

```bash
ps -o pid,ppid,comm -p PID
```

Replace `PID` with your recorded process ID.

### Step 6: Inspect process information

```bash
ps aux | grep '[s]leep'
```

This helps locate the test process.

### Step 7: Change its nice value

```bash
renice 10 -p PID
```

Replace `PID` with your test process ID.

### Step 8: Verify the nice value

```bash
ps -p PID -o pid,ni,comm
```

The nice value should now be 10.

### Step 9: Request graceful termination

```bash
kill -TERM PID
```

Replace `PID` with the same test process ID.

### Step 10: Verify termination

```bash
ps -p PID -o pid,stat,comm
```

The process should no longer appear.

If it has already exited, the command may simply report no matching process.

---

## 12. Practical Lab: Generate a Process Report

Create a report using:

```bash
{
    echo "LINUX PROCESS MANAGEMENT REPORT"
    echo "================================"
    echo
    echo "Current user:"
    whoami
    echo
    echo "Current shell PID:"
    echo $$
    echo
    echo "Top CPU-consuming processes:"
    ps aux --sort=-%cpu | head -n 6
    echo
    echo "Top memory-consuming processes:"
    ps aux --sort=-%mem | head -n 6
    echo
    echo "Running services:"
    systemctl list-units --type=service --state=running --no-pager
} > process-report.txt
```

Display your report:

```bash
cat process-report.txt
```

---

## 13. Cybersecurity Connection: Process Investigation

Process inspection is an important cybersecurity skill.

During incident response, security professionals investigate processes to identify suspicious activity.

They may examine:

1. Process names.
2. Executable locations.
3. Process owners.
4. Parent-child relationships.
5. CPU and memory consumption.
6. Network connections.
7. Startup and service configurations.

### Example investigation commands

List processes:

```bash
ps aux
```

Inspect a specific PID:

```bash
ps -fp PID
```

Inspect its executable path:

```bash
readlink -f /proc/PID/exe
```

Inspect its open network connections where permissions allow:

```bash
sudo ss -tpn
```

Replace `PID` with a real process ID.

### Security warning

Do not assume that a process is malicious simply because:

- Its name is unfamiliar.
- It consumes high CPU.
- It runs as root.
- It has a network connection.

Investigate the executable, parent process, ownership, command-line arguments, and surrounding system activity before drawing conclusions.

---

## 14. Lesson Summary

You learned:

1. A process is a running instance of a program.
2. Every process has a PID.
3. PPID identifies the parent process.
4. `ps`, `top`, and `pgrep` help inspect processes.
5. Linux processes have different execution states.
6. Background processes can run without occupying the terminal.
7. Signals allow processes to receive management instructions.
8. Nice values influence CPU scheduling priority.
9. Services provide background system functions.
10. Process investigation is important in cybersecurity.

---

## 15. Knowledge Check

1. What is a Linux process?
2. What is the difference between a program and a process?
3. What does PID mean?
4. What does PPID mean?
5. Which command displays all processes?
6. What is the purpose of `top`?
7. What does the `&` symbol do?
8. What is the difference between SIGTERM and SIGKILL?
9. What is a zombie process?
10. What is the purpose of the nice value?
11. What is the difference between a process and a service?
12. Why do cybersecurity professionals investigate running processes?

---

## 16. Practical Completion Checklist

- [ ] I understand processes and PIDs.
- [ ] I understand parent and child processes.
- [ ] I inspected processes using `ps`.
- [ ] I used `top` to monitor processes.
- [ ] I understand foreground and background jobs.
- [ ] I understand common process signals.
- [ ] I inspected process priorities.
- [ ] I changed the nice value of my test process.
- [ ] I safely terminated my test process.
- [ ] I generated my process report.
- [ ] I understand how process investigation supports cybersecurity.

**Lesson 09 is complete when you understand the concepts and successfully complete the process-management lab.**
