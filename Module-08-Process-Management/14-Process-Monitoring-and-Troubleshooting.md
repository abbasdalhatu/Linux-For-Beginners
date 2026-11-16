# MODULE 08: PROCESS MANAGEMENT

## LESSON 14: PROCESS MONITORING AND TROUBLESHOOTING

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain why process monitoring is important.
2. Identify processes consuming excessive CPU.
3. Investigate memory usage.
4. Detect unresponsive processes.
5. Understand Linux load averages.
6. Monitor system resource usage.
7. Troubleshoot common process-related problems.
8. Use Linux monitoring tools during security investigations.

---

### 2. What Is Process Monitoring?

Process monitoring is the practice of observing and analyzing running processes to understand how they use system resources.

A Linux system runs many processes simultaneously.

Each process may consume:

- CPU time.
- RAM.
- Disk input/output.
- Network resources.
- System permissions.

Process monitoring helps administrators understand what is happening inside a Linux system.

For example, imagine your Kali Linux virtual machine suddenly becomes slow.

Possible causes include:

- A process consuming too much CPU.
- A program using excessive memory.
- Too many running processes.
- A process stuck in an uninterruptible sleep state.
- A background service malfunctioning.

Process monitoring helps identify the cause instead of guessing.

### 3. Important System Resources

Before troubleshooting processes, understand the resources they use.

#### A. CPU

CPU stands for Central Processing Unit.

It executes instructions from running processes.

A process consuming too much CPU may make the system slow.

#### B. RAM

RAM stands for Random Access Memory.

Running processes use RAM to store temporary data.

When available memory becomes very low, the system may begin using swap space.

#### C. Swap

Swap is disk space used as an extension of physical memory.

It can help when RAM is under pressure, but excessive swapping can make a system slow.

#### D. Disk I/O

Disk input/output measures how processes read from and write to storage.

A process performing heavy disk operations may affect system responsiveness.

### 4. Monitoring Processes with top

The `top` command provides a continuously updated view of running processes.

Run:

```bash
top
```

Important information includes:

- System uptime.
- Load averages.
- Number of tasks.
- CPU utilization.
- Memory utilization.
- Running processes.

Useful keyboard shortcuts:

| Key | Function |
|---|---|
| P | Sort by CPU usage |
| M | Sort by memory usage |
| T | Sort by accumulated CPU time |
| k | Send a signal to a process |
| q | Quit |

For this lesson, use `top` for observation. Do not terminate unfamiliar processes.

### 5. Identifying High CPU Usage

A process consuming excessive CPU may affect system performance.

Use:

```bash
ps -eo pid,ppid,comm,%cpu --sort=-%cpu
```

Explanation:

- `ps` displays process information.
- `-e` selects all processes.
- `-o` specifies output columns.
- `pid` displays the process ID.
- `ppid` displays the parent process ID.
- `comm` displays the command name.
- `%cpu` displays CPU usage.
- `--sort=-%cpu` sorts from highest to lowest CPU usage.

To display only the first ten entries:

```bash
ps -eo pid,ppid,comm,%cpu --sort=-%cpu | head
```

This helps identify processes using the most CPU resources.

**Important:** A high CPU percentage is not automatically evidence of malware. Legitimate programs may temporarily use significant CPU resources.

### 6. Identifying High Memory Usage

Memory-intensive processes can reduce available RAM.

Run:

```bash
ps -eo pid,ppid,comm,%mem,rss --sort=-%mem
```

Explanation:

| Column | Meaning |
|---|---|
| PID | Process ID |
| PPID | Parent process ID |
| COMMAND | Command name |
| %MEM | Percentage of physical memory used |
| RSS | Resident memory in kilobytes |

To show the ten highest memory consumers:

```bash
ps -eo pid,ppid,comm,%mem,rss --sort=-%mem | head
```

### 7. Checking Overall Memory Usage

Use:

```bash
free -h
```

Example output:

```text
               total        used        free      shared  buff/cache   available
Mem:            3.8Gi       1.2Gi       1.5Gi       100Mi       1.1Gi       2.3Gi
Swap:           2.0Gi          0B       2.0Gi
```

The numbers above are illustrative.

Important columns:

- Total: Installed or available memory.
- Used: Memory currently in use.
- Free: Completely unused memory.
- Buff/cache: Memory used for buffers and caches.
- Available: Estimated memory available for new applications.

**Important:** Linux uses otherwise-unused RAM for caching. Therefore, low `free` memory does not automatically mean the system is running out of memory. Pay attention to `available`.

### 8. Checking CPU Information and Load Average

Run:

```bash
uptime
```

Example:

```text
12:30:00 up 2 days, 3:15, 2 users, load average: 0.50, 0.75, 0.80
```

The three load-average values represent:

1. Average load over 1 minute.
2. Average load over 5 minutes.
3. Average load over 15 minutes.

Load average represents the average number of tasks that are runnable or waiting in uninterruptible sleep.

It is not simply a CPU percentage.

For example:

- On a one-core system, a load of 1.0 roughly represents one task continuously demanding CPU time.
- On a four-core system, a load of 4.0 roughly represents four tasks continuously demanding CPU time.

Interpretation depends on CPU count, workload, and I/O activity.

Check the number of available processing units:

```bash
nproc
```

### 9. Checking Running and Blocked Processes

Use:

```bash
ps -eo pid,stat,comm
```

Important process states:

| State | Meaning |
|---|---|
| R | Running or runnable |
| S | Interruptible sleep |
| D | Uninterruptible sleep |
| T | Stopped |
| Z | Zombie |
| I | Idle kernel thread |

To identify processes in uninterruptible sleep:

```bash
ps -eo pid,stat,comm | awk '$2 ~ /^D/'
```

A process in D state is commonly waiting for an operation such as disk or device I/O.

Do not assume it is malicious.

### 10. Investigating Unresponsive Processes

A program may become unresponsive because of:

- An infinite loop.
- Resource exhaustion.
- Waiting for an unavailable resource.
- A software defect.
- A network or storage operation that is not completing.

Start by identifying the process:

```bash
ps aux
```

Search for a particular process:

```bash
pgrep -a PROCESS_NAME
```

Inspect a specific PID:

```bash
ps -p PID -o pid,ppid,stat,%cpu,%mem,cmd
```

Replace `PID` with the actual process ID.

If you own the process and have decided it must be stopped, prefer a graceful termination:

```bash
kill -TERM PID
```

Verify:

```bash
ps -p PID
```

Use SIGKILL only when a process cannot be terminated normally and you understand the consequences:

```bash
kill -KILL PID
```

Never terminate an unfamiliar system process simply because it appears busy.

### 11. Monitoring Processes with watch

The `watch` command repeatedly executes another command and displays its output.

Example:

```bash
watch -n 2 'ps -eo pid,comm,%cpu,%mem --sort=-%cpu | head'
```

Explanation:

- `watch` repeats a command.
- `-n 2` refreshes every two seconds.
- `ps` displays process information.
- `head` limits the displayed output.

Press `Ctrl+C` to exit.

Another example:

```bash
watch -n 2 free -h
```

This repeatedly displays memory usage.

### 12. Checking System Resource Usage with vmstat

`vmstat` reports information about processes, memory, swap, CPU, and system activity.

Run:

```bash
vmstat 2 5
```

This displays five reports at two-second intervals.

Important columns include:

| Column | Meaning |
|---|---|
| r | Runnable processes |
| b | Processes blocked waiting for resources |
| swpd | Used virtual memory |
| free | Free memory |
| si | Swap-in activity |
| so | Swap-out activity |
| us | CPU time spent in user processes |
| sy | CPU time spent in system processes |
| id | Idle CPU time |
| wa | CPU time waiting for I/O |

The first report may represent averages since the last boot, depending on the system. Subsequent reports show activity over the specified interval.

### 13. Checking Disk Usage

Processes may become slow when storage is full.

Check filesystem capacity:

```bash
df -h
```

Check the size of your home directory:

```bash
du -sh ~
```

Check the largest directories inside your home directory:

```bash
du -h --max-depth=1 ~ 2>/dev/null | sort -hr
```

These commands help identify storage-related problems.

### 14. Checking Process Details Through /proc

Linux exposes process and system information through the virtual `/proc` filesystem.

Each running process normally has a directory named after its PID.

For example:

```bash
ls /proc
```

Inspect your current shell PID:

```bash
echo $$
```

If the output is `1234`, inspect:

```bash
cat /proc/1234/status
```

Replace `1234` with the PID you actually obtained.

The status file may contain:

- Process name.
- Process state.
- PID.
- Parent PID.
- User IDs.
- Memory information.
- Thread count.

Inspect command-line arguments:

```bash
cat /proc/1234/cmdline
```

The `/proc` filesystem provides useful information for troubleshooting and security analysis.

### 15. A Simple Troubleshooting Workflow

When a Linux system becomes slow, use a structured approach.

**Step 1: Check overall system load**

```bash
uptime
```

**Step 2: Check CPU usage**

```bash
top
```

**Step 3: Identify high CPU processes**

```bash
ps -eo pid,comm,%cpu --sort=-%cpu | head
```

**Step 4: Check memory**

```bash
free -h
```

**Step 5: Identify memory-intensive processes**

```bash
ps -eo pid,comm,%mem,rss --sort=-%mem | head
```

**Step 6: Check process states**

```bash
ps -eo pid,stat,comm
```

**Step 7: Check disk capacity**

```bash
df -h
```

**Step 8: Investigate suspicious or unresponsive processes**

```bash
ps -p PID -o pid,ppid,stat,%cpu,%mem,cmd
```

Replace `PID` with the process you want to investigate.

**Step 9: Review relevant service logs if a service is involved**

```bash
journalctl -u SERVICE_NAME -n 30
```

**Step 10: Apply a safe correction**

Only after understanding the problem should you consider restarting a service, stopping your own process, or freeing storage.

### 16. Cybersecurity Applications

Process monitoring is important in cybersecurity because attackers may execute malicious processes on compromised systems.

Security professionals can use process information to investigate:

**1. Suspicious processes**

Look for unfamiliar commands, unusual process names, and unexpected execution paths.

**2. Unexpected resource consumption**

Malware may consume excessive CPU or memory.

**3. Unauthorized persistence**

Investigate processes associated with unexpected services or scheduled tasks.

**4. Process relationships**

Use parent PID information to understand which process started another process.

**5. Incident response**

Capture process information before taking action so that evidence is not unnecessarily lost.

Useful commands:

```bash
ps aux
```

```bash
pstree -p
```

```bash
ps -eo pid,ppid,user,stat,%cpu,%mem,cmd
```

Remember: unusual process activity is an indicator for investigation, not proof of compromise.

### 17. Lesson Summary

In this lesson, we learned:

1. Process monitoring helps understand system activity.
2. `top` provides real-time process monitoring.
3. `ps` helps identify resource-intensive processes.
4. `free -h` displays memory information.
5. `uptime` displays load averages.
6. `nproc` reports available processing units.
7. `vmstat` reports system performance information.
8. `watch` repeatedly runs monitoring commands.
9. `/proc` exposes process information.
10. `df -h` helps investigate storage capacity.

### 18. Knowledge Check

Answer these questions in your own words.

1. What is process monitoring?
2. Why might a Linux system become slow?
3. Which command displays continuously updated process information?
4. Which command identifies processes using the most CPU?
5. What does `%MEM` represent?
6. What does `free -h` display?
7. What are the three load-average periods?
8. What does process state D mean?
9. What is the purpose of `vmstat`?
10. Why should cybersecurity professionals inspect process parent IDs?

### 19. Practical Assignment

Complete the following tasks inside your Kali Linux VM.

- [ ] Run `uptime`.
- [ ] Run `nproc`.
- [ ] Run `top` and observe the processes.
- [ ] Display the ten highest CPU-consuming processes.
- [ ] Display the ten highest memory-consuming processes.
- [ ] Run `free -h`.
- [ ] Run `vmstat 2 5`.
- [ ] Run `watch -n 2 free -h` and exit using Ctrl+C.
- [ ] Inspect your shell's process information through `/proc`.
- [ ] Run `df -h`.
- [ ] Write down three possible reasons for a slow Linux system.
- [ ] Complete the ten knowledge-check questions.

**Safety reminder:** This laboratory is observational. Do not kill unfamiliar processes, change system services, or modify system files.

**End of Lesson 14**

**Next Lesson:** Module 08 Assessment.