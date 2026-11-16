
# PRACTICAL LABS
# MODULE 04 – LINUX SYSTEM ADMINISTRATION

## LAB 03: PROCESS MANAGEMENT

### 1. Lab Overview

A process is a program that is currently running on a computer.

Whenever you open a terminal, launch a browser, execute a script, or start a service, Linux creates or manages processes.

System administrators must understand how to:

- Identify running processes.
- Monitor CPU and memory consumption.
- Understand Process IDs (PIDs).
- Inspect parent and child processes.
- Manage foreground and background processes.
- Send signals to processes.
- Investigate suspicious or resource-intensive processes.

Process management is also important in cybersecurity because malicious software, unauthorized scripts, and resource-exhaustion attacks can affect system performance.

**Lab environment:** Kali Linux VM.

**Safety:** Perform all activities inside your own virtual machine. Do not terminate unknown system processes.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain what a process is.
2. Identify processes using their PIDs.
3. Monitor running processes.
4. Understand process states.
5. Manage foreground and background tasks.
6. Use Linux signals safely.
7. Inspect process relationships.
8. Generate a process-monitoring report.

### 3. Understanding Processes

A process is an instance of a running program.

For example, when you execute:

```bash
sleep 100
```

Linux starts a process that remains active for 100 seconds.

Every process has a Process ID (PID).

Some important process terms:

| Term | Meaning |
|---|---|
| PID | Unique process identifier |
| PPID | Parent Process ID |
| UID | User who owns the process |
| CPU | Processor resources used |
| RAM | Memory consumed |
| Process state | Current execution condition |

### 4. Inspect Running Processes

Display processes associated with your terminal:

```bash
ps
```

Display detailed information about all processes:

```bash
ps aux
```

Understand the columns:

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

Display a simplified process list:

```bash
ps -ef
```

### 5. Monitor Processes in Real Time

Run:

```bash
top
```

The `top` command provides continuously updated information about running processes.

Important information includes:

- System uptime.
- Number of users.
- CPU utilization.
- Memory usage.
- Running processes.
- Process IDs.
- Resource consumption.

Useful keyboard controls inside `top`:

| Key | Action |
|---|---|
| `q` | Quit |
| `P` | Sort by CPU usage |
| `M` | Sort by memory usage |
| `k` | Send a signal to a process |
| `h` | Display help |

For this lab, use `P`, `M`, and `q`. Avoid terminating unfamiliar processes.

### 6. Understand Process States

Linux uses process states to describe what a process is doing.

Common process states include:

| State | Meaning |
|---|---|
| R | Running or runnable |
| S | Interruptible sleep |
| D | Uninterruptible sleep |
| T | Stopped |
| Z | Zombie |
| I | Idle kernel thread |

Inspect process states:

```bash
ps -eo pid,ppid,stat,comm
```

Display the state of your current shell:

```bash
ps -o pid,ppid,stat,comm -p $$
```

**Important:** A zombie process has finished executing, but its parent has not yet collected its exit status.

### 7. Inspect a Specific Process

Start a harmless test process:

```bash
sleep 300 &
```

The `&` symbol starts the process in the background.

Display background jobs:

```bash
jobs
```

Find the sleep process:

```bash
pgrep -a sleep
```

Display detailed information about it:

```bash
ps -o pid,ppid,user,stat,%cpu,%mem,comm -C sleep
```

You can also inspect it using its PID:

```bash
ps -p PID -f
```

Replace `PID` with the actual process ID.

### 8. Understand Foreground and Background Processes

A foreground process occupies the terminal while it runs.

Example:

```bash
sleep 30
```

A background process runs without occupying the terminal.

Example:

```bash
sleep 30 &
```

Start another process:

```bash
sleep 200 &
```

Inspect your jobs:

```bash
jobs -l
```

Pause a foreground process using:

```text
CTRL + Z
```

Resume a stopped job in the background:

```bash
bg
```

Bring a background job into the foreground:

```bash
fg
```

Use these commands only with your test processes.

### 9. Understanding Linux Signals

Signals are messages sent to processes to request or notify an action.

Common signals:

| Signal | Number | Purpose |
|---|---:|---|
| SIGHUP | 1 | Hangup |
| SIGINT | 2 | Interrupt |
| SIGKILL | 9 | Force termination |
| SIGTERM | 15 | Request termination |
| SIGSTOP | 19 | Stop process |
| SIGCONT | 18 | Continue stopped process |

Send a normal termination request to your test process:

```bash
kill -TERM PID
```

Replace `PID` with the actual PID.

Verify that it has stopped:

```bash
pgrep -a sleep
```

You can also use:

```bash
kill -15 PID
```

**Security note:** SIGTERM is generally preferred over SIGKILL because it gives a process an opportunity to perform cleanup.

SIGKILL should be reserved for situations where normal termination fails and you have confirmed the target process is safe to terminate.

### 10. Inspect Parent and Child Processes

Processes can create other processes.

The process that creates another process is called its parent.

The newly created process is called its child.

Display a process tree:

```bash
pstree
```

If `pstree` is unavailable, use:

```bash
ps -ef --forest
```

Display process relationships:

```bash
ps -eo pid,ppid,user,comm --forest
```

Inspect the parent PID of your current shell:

```bash
echo $$
```

```bash
ps -o pid,ppid,comm -p $$
```

### 11. Find Processes by Name

Search for a running process:

```bash
pgrep bash
```

Search and display process names:

```bash
pgrep -a bash
```

Find processes belonging to a particular user:

```bash
pgrep -u "$USER" -a
```

Search for a command using `ps`:

```bash
ps aux | grep sleep
```

Note: `grep` may also display its own search process. `pgrep` is often cleaner for process-name searches.

### 12. Inspect Resource Consumption

Display CPU and memory usage:

```bash
ps aux --sort=-%cpu | head
```

Display the processes consuming the most memory:

```bash
ps aux --sort=-%mem | head
```

Display system memory:

```bash
free -h
```

Display system load:

```bash
uptime
```

These commands help administrators identify resource-intensive processes.

### 13. Cybersecurity Application: Process Investigation

During incident response, an administrator may investigate an unknown process.

Important questions include:

1. What is the process name?
2. What is its PID?
3. Which user owns it?
4. What command started it?
5. What is its parent process?
6. How much CPU and memory does it consume?
7. Does it belong to an expected application?

Inspect a process:

```bash
ps -p PID -o pid,ppid,user,stat,%cpu,%mem,args
```

Replace `PID` with the process ID you are investigating.

Inspect its executable path when available:

```bash
readlink -f /proc/PID/exe
```

Inspect its working directory:

```bash
readlink -f /proc/PID/cwd
```

Inspect its command line:

```bash
tr '\0' ' ' < /proc/PID/cmdline
```

Replace `PID` with the actual process ID.

**Important:** These commands are investigative techniques, not proof that a process is malicious. Always compare findings with trusted system information and application records.

### 14. Build a Process Monitoring Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module04/lab03
```

Create a Bash script:

```bash
nano ~/practical-labs/module04/lab03/process-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module04/lab03/process-report.txt"

{
    echo "LINUX PROCESS MONITORING REPORT"
    echo "================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "SYSTEM UPTIME"
    uptime
    echo

    echo "MEMORY INFORMATION"
    free -h
    echo

    echo "TOP CPU-CONSUMING PROCESSES"
    ps aux --sort=-%cpu | head -n 10
    echo

    echo "TOP MEMORY-CONSUMING PROCESSES"
    ps aux --sort=-%mem | head -n 10
    echo

    echo "PROCESS COUNT"
    ps -e --no-headers | wc -l

} > "$REPORT"

echo "Report generated: $REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x ~/practical-labs/module04/lab03/process-report.sh
```

Execute it:

```bash
~/practical-labs/module04/lab03/process-report.sh
```

Read the report:

```bash
cat ~/practical-labs/module04/lab03/process-report.txt
```

### 15. Practical Exercises

**Exercise 1 – Process Identification**

Run:

```bash
ps aux
ps -ef
```

Identify five processes and record their PIDs and owners.

**Exercise 2 – Real-Time Monitoring**

Run:

```bash
top
```

Identify the process consuming the most CPU and the process consuming the most memory.

Exit using `q`.

**Exercise 3 – Background Process Management**

Run:

```bash
sleep 300 &
jobs -l
pgrep -a sleep
```

Identify the PID of your test process.

**Exercise 4 – Signal Management**

Terminate only your test process:

```bash
kill -TERM PID
```

Verify:

```bash
pgrep -a sleep
```

**Exercise 5 – Process Investigation**

Choose a normal process running in your VM and inspect its:

- PID.
- Parent PID.
- Owner.
- Process state.
- Command line.

Use:

```bash
ps -p PID -o pid,ppid,user,stat,args
```

**Exercise 6 – Report Generation**

Execute your monitoring script and inspect the generated report.

### 16. Lab Documentation

Create:

```bash
nano ~/practical-labs/module04/lab03/lab03-report.txt
```

Record:

```text
MODULE 04 – LAB 03
PROCESS MANAGEMENT

1. Number of processes observed:
2. Example process name:
3. Example PID:
4. Process owner:
5. Highest CPU-consuming process:
6. Highest memory-consuming process:
7. Difference between foreground and background:
8. Meaning of SIGTERM:
9. Meaning of a zombie process:
10. What I learned:
```

Save with `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 17. Cleanup

If any test sleep processes remain, identify them:

```bash
pgrep -a sleep
```

Terminate only the test processes you started:

```bash
kill -TERM PID
```

Replace `PID` with the correct test process ID.

Do not terminate unrelated system processes.

### 18. Knowledge Check

1. What is a process?
2. What does PID mean?
3. What is the difference between PID and PPID?
4. What is the purpose of `ps aux`?
5. What is the difference between `top` and `ps`?
6. What does the `&` symbol do?
7. What is the difference between SIGTERM and SIGKILL?
8. What is a zombie process?
9. Why would a cybersecurity analyst inspect `/proc/PID/exe`?
10. Why should administrators avoid terminating unknown processes?

### 19. Lab Completion Checklist

- [ ] Inspected running processes.
- [ ] Used `top` to monitor resources.
- [ ] Identified process states.
- [ ] Created a background test process.
- [ ] Inspected process IDs and relationships.
- [ ] Practised safe signal management.
- [ ] Investigated a normal process.
- [ ] Generated the process monitoring report.
- [ ] Completed the lab documentation.
- [ ] Cleaned up test processes.

**LAB 03 STATUS: READY FOR PRACTICAL COMPLETION**
