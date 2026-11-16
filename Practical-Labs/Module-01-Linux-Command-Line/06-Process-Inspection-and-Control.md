
# PRACTICAL LABS
# MODULE 01 – LINUX COMMAND-LINE MASTERY

## LAB 06: PROCESS INSPECTION AND CONTROL

---

## 1. Lab Objectives

By the end of this practical lab, you should be able to:

1. Understand Linux processes.
2. Identify running processes.
3. Inspect process IDs (PIDs).
4. Monitor system activity.
5. Understand foreground and background processes.
6. Start and stop processes safely.
7. Understand Linux signals.
8. Inspect process resource usage.
9. Identify parent and child processes.
10. Build a simple process-monitoring script.

## 2. Prepare Your Lab Environment

```bash
mkdir -p ~/practical-labs/module01/lab06
cd ~/practical-labs/module01/lab06
```

Verify:

```bash
pwd
```

---

## 3. Exercise 1: Understand Linux Processes

A process is a running instance of a program.

For example:

- Opening Firefox creates running processes.
- Running a Bash script creates a process.
- Executing `ls` creates a process.
- Running a Python program creates a process.

Linux assigns each process a unique Process ID called a PID.

### Inspect your current shell

```bash
echo $$
```

This displays the PID of your current Bash shell.

Inspect the parent process ID:

```bash
echo $PPID
```

The PPID identifies the parent process of the current shell.

Inspect your current process:

```bash
ps
```

---

## 4. Exercise 2: Display Running Processes

Use `ps` to inspect processes.

### Basic process list

```bash
ps
```

### Processes belonging to all users

```bash
ps aux
```

### Display processes in a tree

```bash
ps -ef --forest
```

### Display selected information

```bash
ps -eo pid,ppid,user,stat,comm
```

Understand the columns:

| Column | Meaning |
|---|---|
| PID | Process ID |
| PPID | Parent Process ID |
| USER | Process owner |
| STAT | Process state |
| COMMAND | Executed command |

### Filter processes

Find Bash processes:

```bash
ps aux | grep bash
```

Find Python processes:

```bash
ps aux | grep python
```

A `grep` command may also appear in its own results because it contains the search term.

A cleaner alternative is:

```bash
pgrep -a bash
```

Or:

```bash
pgrep -a python
```

---

## 5. Exercise 3: Understand Process States

Linux processes can exist in different states.

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
ps -eo pid,stat,comm
```

### Understand a zombie process

A zombie process is a process that has completed execution but whose parent has not yet collected its exit status.

It does not mean the process is actively running.

Inspect possible zombie processes:

```bash
ps -eo pid,ppid,stat,comm | grep ' Z'
```

Do not attempt to kill arbitrary system processes while investigating process states.

---

## 6. Exercise 4: Monitor Processes with top

Run:

```bash
top
```

`top` displays real-time information about running processes.

You can observe:

- CPU usage
- Memory usage
- Process IDs
- Process owners
- System load
- Running tasks

Useful keys inside `top`:

| Key | Function |
|---|---|
| `q` | Quit |
| `P` | Sort by CPU usage |
| `M` | Sort by memory usage |
| `1` | Show individual CPU information |
| `k` | Request a process termination signal |

For this lab, use only viewing and sorting features. Exit with `q`.

### Alternative: htop

Check whether it is installed:

```bash
command -v htop
```

If it is unavailable, you may install it through Kali's official package repositories:

```bash
sudo apt update
sudo apt install htop
```

Run:

```bash
htop
```

Exit with `q`.

---

## 7. Exercise 5: Find a Process by Name

Linux provides several process-search commands.

### Using pgrep

```bash
pgrep bash
```

Display matching process names:

```bash
pgrep -a bash
```

### Using pidof

```bash
pidof bash
```

`pidof` may not return a result for every command, especially when the executable or process name differs.

### Using ps and grep

```bash
ps aux | grep firefox
```

These commands are useful when investigating which programs are running.

---

## 8. Exercise 6: Understand Foreground Processes

A foreground process runs directly in your current terminal session.

Example:

```bash
sleep 60
```

The terminal remains occupied until the command finishes.

You can interrupt this process with:

```text
CTRL + C
```

This normally sends the SIGINT signal to the foreground process.

### Try another example

```bash
sleep 30
```

Press `CTRL + C` to interrupt it.

---

## 9. Exercise 7: Run Background Processes

A background process runs without keeping the terminal occupied.

Start a process in the background:

```bash
sleep 300 &
```

The shell should display a job number and PID.

Inspect jobs:

```bash
jobs
```

Display background processes:

```bash
ps aux | grep sleep
```

You can also inspect the sleep process using:

```bash
pgrep -a sleep
```

### Understand the ampersand

The `&` symbol instructs the shell to start a command in the background.

Example:

```bash
sleep 120 &
```

Your terminal becomes available immediately.

---

## 10. Exercise 8: Understand Jobs and Job Control

Create a foreground process:

```bash
sleep 300
```

Press:

```text
CTRL + Z
```

This suspends the foreground job.

Inspect:

```bash
jobs
```

Resume the job in the background:

```bash
bg
```

Inspect again:

```bash
jobs
```

Bring the job back to the foreground:

```bash
fg
```

Press `CTRL + C` to terminate it.

### Important commands

| Command | Purpose |
|---|---|
| `jobs` | List shell jobs |
| `bg` | Resume a suspended job in the background |
| `fg` | Bring a job to the foreground |
| `CTRL + C` | Interrupt the foreground process |
| `CTRL + Z` | Suspend the foreground process |

---

## 11. Exercise 9: Understand Linux Signals

Signals are notifications sent to processes.

They can request that a process terminate, stop, continue, or perform another action.

List available signals:

```bash
kill -l
```

Important signals:

| Signal | Number | Purpose |
|---|---:|---|
| SIGHUP | 1 | Hangup |
| SIGINT | 2 | Interrupt |
| SIGKILL | 9 | Force termination |
| SIGTERM | 15 | Request termination |
| SIGSTOP | 19 on common x86 Linux systems | Stop a process |
| SIGCONT | 18 on common x86 Linux systems | Continue a stopped process |

Signal numbers can vary across architectures. Prefer signal names when writing portable commands.

### Understand SIGTERM

SIGTERM requests that a process terminate gracefully.

Start a lab process:

```bash
sleep 300 &
```

Find its PID:

```bash
pgrep -a sleep
```

Terminate the specific lab process:

```bash
kill -TERM PID
```

Replace `PID` with the actual number returned by `pgrep`.

Confirm:

```bash
pgrep -a sleep
```

The process should no longer appear if it has terminated.

### Understand SIGKILL

SIGKILL immediately terminates a process and cannot be caught or ignored.

Example syntax:

```bash
kill -KILL PID
```

Use it only when a process cannot be stopped normally and you have verified that it is your own lab process.

**Preferred approach:** Try SIGTERM first. Reserve SIGKILL for situations where graceful termination fails.

---

## 12. Exercise 10: Inspect Process Resource Usage

Use:

```bash
ps aux --sort=-%cpu | head
```

This displays processes with the highest CPU usage near the top.

Inspect memory usage:

```bash
ps aux --sort=-%mem | head
```

Inspect memory statistics:

```bash
free -h
```

Inspect system load:

```bash
uptime
```

Inspect CPU information:

```bash
lscpu
```

These commands are useful during system troubleshooting and performance investigations.

---

## 13. Exercise 11: Understand Parent and Child Processes

Linux processes can create other processes.

A parent process starts a child process.

Inspect the process tree:

```bash
pstree -p
```

If `pstree` is unavailable, check:

```bash
command -v pstree
```

It is provided by the `psmisc` package on Kali/Debian systems:

```bash
sudo apt install psmisc
```

Inspect process relationships:

```bash
ps -eo pid,ppid,comm --forest
```

### Practical task

Start a background process:

```bash
sleep 300 &
```

Find its PID:

```bash
pgrep -a sleep
```

Inspect its parent:

```bash
ps -o pid,ppid,comm -p PID
```

Replace `PID` with the actual process ID.

This helps you understand how the shell starts and manages processes.

---

## 14. Exercise 12: Build a Process Monitoring Script

Create:

```bash
nano process-monitor.sh
```

Enter:

```bash
#!/bin/bash

REPORT="process-report.txt"

{
    echo "=================================="
    echo "       PROCESS MONITOR REPORT"
    echo "=================================="

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
    echo "SYSTEM UPTIME:"
    uptime

    echo
    echo "MEMORY INFORMATION:"
    free -h

    echo
    echo "TOP CPU PROCESSES:"
    ps aux --sort=-%cpu | head -n 6

    echo
    echo "TOP MEMORY PROCESSES:"
    ps aux --sort=-%mem | head -n 6

    echo
    echo "CURRENT USER PROCESSES:"
    ps -u "$(whoami)" -o pid,ppid,stat,comm

    echo
    echo "=================================="
    echo "       REPORT COMPLETED"
    echo "=================================="

} > "$REPORT"

echo "Process report saved to $REPORT"
```

Make it executable:

```bash
chmod +x process-monitor.sh
```

Run:

```bash
./process-monitor.sh
```

Inspect:

```bash
less process-report.txt
```

---

## 15. Practical Challenge

Complete the following tasks:

1. Display your current shell PID.
2. Display all running processes.
3. Find Bash processes.
4. Start a background `sleep` process.
5. Identify its PID.
6. Inspect its PPID.
7. Inspect its process state.
8. Send it SIGTERM.
9. Confirm that it has stopped.
10. Display the top CPU-consuming processes.
11. Display the top memory-consuming processes.
12. Generate a process monitoring report.

Use only your own lab-created processes when practicing process termination.

---

## 16. Knowledge Check

Answer these questions in your notebook:

1. What is a process?
2. What is a PID?
3. What is a PPID?
4. What does `ps aux` display?
5. What is the purpose of `top`?
6. What is the difference between foreground and background processes?
7. What does `&` do?
8. What is the purpose of `jobs`?
9. What is the difference between `bg` and `fg`?
10. What is a Linux signal?
11. What is the difference between SIGTERM and SIGKILL?
12. What is a zombie process?
13. What does `pgrep` do?
14. Why should you avoid terminating unknown system processes?

---

## 17. Lab Completion Checklist

- [ ] Understood Linux processes.
- [ ] Identified PIDs and PPIDs.
- [ ] Used `ps`.
- [ ] Used `top`.
- [ ] Inspected process states.
- [ ] Used `pgrep`.
- [ ] Started foreground processes.
- [ ] Started background processes.
- [ ] Practiced `jobs`, `bg`, and `fg`.
- [ ] Practiced Linux signals.
- [ ] Inspected CPU and memory usage.
- [ ] Examined parent-child relationships.
- [ ] Created a process monitoring script.
- [ ] Completed the practical challenge.

**END OF LAB 06**
