# MODULE 08: PROCESS MANAGEMENT

## LESSON 04: MONITORING PROCESSES WITH TOP AND HTOP

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain the purpose of real-time process monitoring.
2. Use the `top` command to monitor processes.
3. Understand CPU and memory information displayed by `top`.
4. Use `htop` for interactive process monitoring.
5. Compare `top` and `htop`.
6. Identify processes consuming excessive system resources.
7. Apply process-monitoring skills to cybersecurity investigations.

---

### 2. What Is Process Monitoring?

Process monitoring is the activity of observing running processes and their use of system resources.

Linux systems may run many applications and background services simultaneously.

Monitoring helps you understand:

- Which processes are running.
- How much CPU they are using.
- How much memory they consume.
- Which users own them.
- How long they have been running.
- Whether system resources are becoming overloaded.

For example, if your Kali Linux virtual machine becomes slow, process monitoring can help you identify applications consuming excessive CPU or memory.

There are two important Linux monitoring commands we will study:

1. `top`
2. `htop`

### 3. Understanding the top Command

The `top` command displays a continuously updating view of system activity.

Unlike `ps`, which produces a process listing at the time you execute it, `top` refreshes its display periodically.

Run:

```bash
top
```

You will see a screen containing system information and a list of processes.

Press:

```text
q
```

to exit.

### 4. Understanding the top Display

The `top` interface contains several important sections.

#### A. System Uptime

The first line displays information about:

- Current system time.
- How long the system has been running.
- Number of logged-in users.
- System load averages.

Example:

```text
top - 12:30:00 up 2:15, 1 user, load average: 0.15, 0.20, 0.10
```

The values are illustrative.

**Load average** represents the average system load over three periods:

- 1 minute.
- 5 minutes.
- 15 minutes.

On Linux, load average includes tasks running or waiting for CPU time, as well as tasks in certain uninterruptible states.

Interpretation depends on the number of available CPU cores.

#### B. Tasks

The task summary shows information about processes and tasks.

You may see:

- Total tasks.
- Running tasks.
- Sleeping tasks.
- Stopped tasks.
- Zombie tasks.

Example:

```text
Tasks: 120 total, 1 running, 119 sleeping, 0 stopped, 0 zombie
```

The numbers will differ on your system.

#### C. CPU Usage

The CPU summary shows how CPU time is being used.

Common fields include:

| Field | Meaning |
|---|---|
| us | Time spent running user-space processes |
| sy | Time spent executing kernel code |
| ni | Time spent running processes with adjusted priority |
| id | Idle CPU time |
| wa | Time waiting for I/O |
| hi | Time handling hardware interrupts |
| si | Time handling software interrupts |
| st | Time stolen by a hypervisor from a virtual machine |

A high CPU usage value may indicate intensive processing, but it is not automatically a sign of a problem.

#### D. Memory Usage

The memory section shows RAM and swap information.

Important concepts:

**Total memory:** The total physical RAM available to the system.

**Free memory:** RAM that is currently unused.

**Used memory:** RAM currently in use.

**Available memory:** An estimate of memory available for new applications without requiring swapping.

Linux uses available RAM for caching, so low free memory alone does not necessarily mean the system is running out of memory.

**Swap:** Disk space used as additional memory when RAM pressure requires it.

#### E. Process List

The lower section displays individual processes.

Important columns include:

| Column | Meaning |
|---|---|
| PID | Process ID |
| USER | Process owner |
| PR | Scheduling priority |
| NI | Niceness value |
| VIRT | Total virtual memory |
| RES | Resident physical memory |
| SHR | Shared memory |
| S | Process state |
| %CPU | CPU usage |
| %MEM | Memory usage |
| TIME+ | Accumulated CPU time |
| COMMAND | Process command name |

### 5. Useful top Keyboard Shortcuts

While `top` is running, you can use these keys:

| Key | Function |
|---|---|
| `q` | Quit |
| `P` | Sort by CPU usage |
| `M` | Sort by memory usage |
| `T` | Sort by accumulated CPU time |
| `k` | Prompt to send a signal to a process |
| `r` | Change process priority (renice) |
| `1` | Toggle individual CPU-core information |
| `h` | Display help |

For this lesson, focus mainly on viewing and sorting processes.

Do not experiment with the `k` or `r` controls on important system processes.

### 6. Understanding htop

`htop` is an interactive process-monitoring tool.

It provides a more visual interface than the traditional `top` command.

Features include:

- CPU usage bars.
- Memory usage bars.
- Process sorting.
- Process searching.
- Interactive navigation.
- Process tree viewing.
- Convenient process selection.

Start it using:

```bash
htop
```

If it is not installed, install it using:

```bash
sudo apt update
sudo apt install htop
```

Only install it if necessary.

### 7. Understanding the htop Interface

When you open `htop`, you will normally see:

**A. CPU meters**

These show CPU activity.

On a multi-core system, individual CPU meters may be displayed.

**B. Memory meter**

This shows physical memory usage.

**C. Swap meter**

This shows swap usage.

**D. Process list**

This displays running processes and their resource consumption.

**E. Function key menu**

The bottom of the interface displays available operations.

Common controls include:

| Key | Function |
|---|---|
| F1 | Help |
| F2 | Setup |
| F3 | Search |
| F4 | Filter |
| F5 | Tree view |
| F6 | Sort |
| F10 | Quit |

The exact available controls may vary by version and terminal.

### 8. Comparing top and htop

| Feature | top | htop |
|---|---|---|
| Real-time monitoring | Yes | Yes |
| Process listing | Yes | Yes |
| CPU information | Yes | Yes |
| Memory information | Yes | Yes |
| Visual CPU meters | Basic | More visual |
| Interactive navigation | Yes | More convenient |
| Process tree | Limited | Built-in tree view |
| Installation | Commonly preinstalled | May require installation |

Both commands are useful.

You should learn `top` because it is widely available on Linux systems, while `htop` provides a convenient interactive experience.

### 9. Practical Laboratory: Monitoring System Activity

**Objective:** Monitor processes and understand how CPU and memory usage are displayed.

#### Step 1: Create your practice directory

```bash
mkdir -p ~/module08-lab/monitoring
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/monitoring
```

#### Step 3: Launch top

```bash
top
```

Observe:

- System uptime.
- Load averages.
- Task summary.
- CPU usage.
- Memory usage.
- Running processes.

Press `q` to exit.

#### Step 4: Sort processes by CPU usage

Launch:

```bash
top
```

Press:

```text
P
```

Observe how the process list changes.

Press `q` to exit.

#### Step 5: Sort processes by memory usage

Launch:

```bash
top
```

Press:

```text
M
```

Observe the process list.

Press `q` to exit.

#### Step 6: Check whether htop is installed

```bash
command -v htop
```

If the command returns a path, htop is installed.

If it returns nothing, install it:

```bash
sudo apt update
sudo apt install htop
```

#### Step 7: Launch htop

```bash
htop
```

Observe the CPU, memory, and process displays.

#### Step 8: Explore htop

Practice the following:

1. Press `F3` to search for a process.
2. Press `F4` to filter processes.
3. Press `F5` to view the process tree.
4. Press `F6` to explore sorting options.
5. Press `F10` to exit.

If your function keys are intercepted by the terminal or host operating system, use the on-screen menu or the alternative controls shown by your version.

#### Step 9: Compare both tools

Run `top` and `htop` separately.

Write down three differences you notice.

### 10. Practical Exercise: Observe a Running Process

We will use a harmless process to observe how a running task appears in monitoring tools.

#### Step 1: Open Terminal 1

Run:

```bash
sleep 300
```

Leave it running.

#### Step 2: Open Terminal 2

Run:

```bash
top
```

Find the `sleep` process in the process list.

Observe its PID and resource usage.

Press `q` to exit.

#### Step 3: Open htop

Run:

```bash
htop
```

Find the same process.

Observe how it appears in the interactive interface.

#### Step 4: Stop the process

Return to Terminal 1 and press:

```text
Ctrl + C
```

The process should terminate.

### 11. Process Monitoring in Cybersecurity

Real-time monitoring is useful in cybersecurity operations.

**1. Detecting unusual resource usage**

An unexpected process consuming large amounts of CPU or memory may require investigation.

**2. Investigating suspicious applications**

Analysts can inspect process names, owners, and resource usage.

**3. Identifying resource exhaustion**

Malicious or malfunctioning software can consume excessive resources and reduce system availability.

**4. Monitoring virtual machines**

Security professionals use monitoring tools to understand the performance of isolated laboratory environments.

**5. Supporting incident response**

Process monitoring provides information that can help analysts understand what was running during an investigation.

Remember: resource usage is only one indicator. A proper investigation should also examine executable paths, command arguments, parent processes, network connections, and system logs.

### 12. Common Mistakes

**Mistake 1: Thinking low free RAM always means a problem**

Linux uses available memory for caching and other purposes.

**Mistake 2: Confusing load average with CPU percentage**

Load average and CPU utilization measure different things.

**Mistake 3: Assuming every high-CPU process is malicious**

Legitimate applications can require substantial processing power.

**Mistake 4: Forgetting to exit monitoring tools**

Use `q` to exit `top` and `F10` to exit `htop`.

**Mistake 5: Installing unnecessary packages**

Check whether `htop` is already installed before installing it.

### 13. Lesson Summary

In this lesson, we learned:

1. Process monitoring helps us observe running processes.
2. `top` provides continuously updated system information.
3. `htop` offers an interactive process-monitoring interface.
4. CPU usage and memory usage help us understand system performance.
5. Load average describes system load over three time periods.
6. Linux uses memory for caching, so free memory alone is not a complete performance indicator.
7. Monitoring tools are useful for system administration and cybersecurity.

### 14. Knowledge Check

Answer these questions in your own words.

1. What is process monitoring?
2. What is the purpose of the `top` command?
3. What is the main difference between `top` and `ps`?
4. What does load average represent?
5. What does the `%CPU` column show?
6. What is the difference between free memory and available memory?
7. What is swap memory?
8. Which key sorts processes by CPU usage in `top`?
9. What is one advantage of `htop` over `top`?
10. Why is process monitoring useful in cybersecurity?

### 15. Practical Assignment

Complete these tasks in your Kali Linux virtual machine:

- [ ] Create `~/module08-lab/monitoring`.
- [ ] Run `top`.
- [ ] Identify the system uptime and load averages.
- [ ] Identify CPU usage information.
- [ ] Identify memory and swap information.
- [ ] Sort processes by CPU using `P`.
- [ ] Sort processes by memory using `M`.
- [ ] Check whether `htop` is installed.
- [ ] Install `htop` if necessary.
- [ ] Run `htop`.
- [ ] Explore its search, filter, tree, and sort features.
- [ ] Compare `top` and `htop`.
- [ ] Observe a harmless `sleep 300` process in both tools.
- [ ] Stop the process using `Ctrl+C`.
- [ ] Complete the 10 knowledge-check questions.

**End of Lesson 04**

**Next Lesson:** Understanding Linux Process States.