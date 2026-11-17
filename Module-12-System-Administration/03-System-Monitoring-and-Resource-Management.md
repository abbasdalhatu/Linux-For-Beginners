---
---


# Module 12 – Lesson 03: System Monitoring and Resource Management

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux system monitoring.
2. Understand CPU, RAM, disk, and network resources.
3. Monitor running processes.
4. Identify resource-intensive applications.
5. Check memory and storage usage.
6. Understand load averages.
7. Generate a basic system monitoring report.

---

## 2. What Is System Monitoring?

System monitoring is the process of observing a computer's performance, resource usage, and operational condition.

A Linux administrator monitors a system to determine whether it is operating normally.

For example, imagine a university Linux server hosting an LMS.

If hundreds of students access the LMS simultaneously, the server may experience:

- High CPU usage.
- Insufficient available memory.
- High disk usage.
- Increased network traffic.
- Slow application responses.

Monitoring helps administrators identify these problems before they become serious.

---

## 3. Understanding Computer Resources

A Linux computer depends on several important resources.

### A. CPU (Central Processing Unit)

The CPU executes instructions and performs calculations.

Examples of CPU-intensive activities include:

- Video processing.
- Database operations.
- Running multiple virtual machines.
- Compiling software.
- Running intensive security analysis.

When CPU usage becomes unusually high, the computer may become slow.

Check CPU information:

```bash
lscpu
```

Check CPU activity:

```bash
top
```

### B. RAM (Random Access Memory)

RAM temporarily stores information that running programs need.

Examples:

- Browser tabs.
- Running applications.
- Virtual machines.
- Background services.

Check memory usage:

```bash
free -h
```

Example output:

```text
               total        used        free
Mem:            15Gi        5Gi         8Gi
Swap:          2.0Gi        0B          2.0Gi
```

These figures are illustrative.

Important terms:

- Total: Installed or available memory represented by the system.
- Used: Memory currently in use.
- Free: Completely unused memory.
- Available: Estimated memory available for new applications without swapping.

Linux uses available memory for caching, so low `free` memory does not automatically mean the system is running out of RAM.

### C. Storage

Storage holds files, programs, and operating system data.

Check disk usage:

```bash
df -h
```

Check directory sizes:

```bash
du -sh ~
```

Find the size of folders in your current directory:

```bash
du -sh ./*
```

### D. Network Resources

Network monitoring helps administrators understand network activity.

Useful commands:

```bash
ip addr
```

```bash
ip route
```

```bash
ss -tuln
```

The `ss` command displays socket information.

---

## 4. Monitoring Processes

A process is a running instance of a program.

For example, when you open a terminal, a shell process runs.

When you open a browser, additional processes are created.

### Display running processes

```bash
ps aux
```

This displays processes belonging to different users.

### Display processes interactively

```bash
top
```

The `top` interface updates periodically.

Important columns include:

| Column | Meaning |
|---|---|
| PID | Process ID |
| USER | Process owner |
| %CPU | CPU usage |
| %MEM | Memory usage |
| TIME+ | Accumulated CPU time |
| COMMAND | Program or command |

Press `q` to exit `top`.

### Alternative: htop

`htop` provides an interactive and more visual process-monitoring interface.

Check whether it is installed:

```bash
command -v htop
```

If it is already installed:

```bash
htop
```

If it is not installed, you can continue using `top`. Installing extra software is not necessary for this lesson.

---

## 5. Understanding Load Average

Load average indicates the average number of processes that are running or waiting for CPU time over different periods.

Check it using:

```bash
uptime
```

Example:

```text
load average: 0.45, 0.60, 0.75
```

The three values represent:

1. Average load over 1 minute.
2. Average load over 5 minutes.
3. Average load over 15 minutes.

### Understanding the values

A load average of 1.0 has different implications depending on the number of CPU cores.

For example:

- On a 1-core system, 1.0 represents approximately one core's worth of load.
- On a 4-core system, 1.0 represents approximately one-quarter of total CPU capacity.

A load average above the available CPU capacity may indicate sustained CPU contention, but it is not automatically proof of a problem.

Always investigate alongside CPU usage, process activity, and system responsiveness.

---

## 6. Monitoring Memory Usage

The following commands help administrators inspect memory.

### Display memory in human-readable format

```bash
free -h
```

### Display memory repeatedly

```bash
watch -n 2 free -h
```

This refreshes the output every two seconds.

Press `Ctrl + C` to stop.

### Display detailed memory information

```bash
cat /proc/meminfo
```

The `/proc` filesystem provides information about the running Linux kernel and system resources.

---

## 7. Monitoring Disk Usage

Storage problems can cause applications and services to malfunction.

### Check filesystem usage

```bash
df -h
```

### Check inode usage

```bash
df -i
```

An inode stores filesystem metadata about a file or directory.

A filesystem can run out of available inodes even when it still has some storage space available.

### Check directory size

```bash
du -sh ~/module12-lab
```

This displays the total size of your course lab directory.

---

## 8. Understanding Resource Bottlenecks

A bottleneck occurs when one system resource limits overall performance.

Examples:

| Bottleneck | Possible Symptoms |
|---|---|
| CPU | Slow calculations and applications |
| RAM | Heavy swapping and application slowdowns |
| Disk | Slow file operations or insufficient storage |
| Network | Slow downloads and connection delays |

### Example: High memory usage

Suppose a Linux server has 8 GB of RAM and applications consume nearly all available memory.

The system may start using swap space more heavily.

This can reduce performance because disk-based memory is generally slower than RAM.

An administrator should investigate which applications are consuming memory before taking corrective action.

Useful commands:

```bash
free -h
```

```bash
ps aux --sort=-%mem | head
```

The second command displays processes ordered by memory consumption.

### Example: High CPU usage

Use:

```bash
ps aux --sort=-%cpu | head
```

This displays processes ordered by CPU consumption.

Do not terminate processes simply because they consume many resources. First identify their purpose.

---

## 9. Practical Lab: Linux Resource Monitoring

### Objective

Perform a basic performance inspection of your Kali Linux virtual machine.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson03
cd ~/module12-lab/lesson03
```

### Step 2: Inspect CPU information

```bash
lscpu
```

Observe the CPU architecture and number of available CPU cores.

### Step 3: Check memory usage

```bash
free -h
```

Identify total, used, free, and available memory.

### Step 4: Check disk usage

```bash
df -h
```

Identify the filesystem containing your home directory.

### Step 5: Check system load

```bash
uptime
```

Identify the three load-average values.

### Step 6: Inspect running processes

```bash
ps aux | head -15
```

Observe the process IDs, users, CPU usage, and memory usage.

### Step 7: Find the most memory-intensive processes

```bash
ps aux --sort=-%mem | head -10
```

### Step 8: Find the most CPU-intensive processes

```bash
ps aux --sort=-%cpu | head -10
```

### Step 9: Inspect network sockets

```bash
ss -tuln
```

Observe the listening TCP and UDP sockets.

### Step 10: Generate a monitoring report

Run:

```bash
{
echo "LINUX SYSTEM MONITORING REPORT"
echo "=============================="
echo ""
echo "CPU INFORMATION:"
lscpu
echo ""
echo "MEMORY USAGE:"
free -h
echo ""
echo "DISK USAGE:"
df -h
echo ""
echo "SYSTEM UPTIME AND LOAD:"
uptime
echo ""
echo "TOP MEMORY-CONSUMING PROCESSES:"
ps aux --sort=-%mem | head -10
echo ""
echo "TOP CPU-CONSUMING PROCESSES:"
ps aux --sort=-%cpu | head -10
echo ""
echo "NETWORK SOCKETS:"
ss -tuln
} > monitoring-report.txt
```

### Step 11: View your report

```bash
cat monitoring-report.txt
```

### Step 12: Verify the report

```bash
ls -lh monitoring-report.txt
wc -l monitoring-report.txt
```

You should now have a system monitoring report containing CPU, memory, storage, process, and network information.

---

## 10. Cybersecurity Connection

System monitoring is important in cybersecurity because unusual resource consumption can sometimes indicate suspicious activity.

Examples include:

- An unknown process consuming excessive CPU.
- Unexpected network services listening on ports.
- Unusual memory consumption.
- Unexpected background processes.
- Sudden increases in disk usage.

However, high resource consumption alone does not prove malicious activity.

Security professionals correlate system performance information with process ownership, executable locations, network connections, and system logs before reaching conclusions.

---

## 11. Lesson Summary

You learned:

1. System monitoring helps administrators understand computer performance.
2. CPU performs processing operations.
3. RAM temporarily stores information used by running applications.
4. Storage holds files and programs.
5. Network monitoring provides information about network activity.
6. `top` and `ps` help inspect running processes.
7. `free` checks memory usage.
8. `df` checks filesystem storage.
9. `uptime` displays system uptime and load averages.
10. Resource monitoring helps detect performance problems and investigate suspicious activity.

---

## 12. Knowledge Check

1. What is system monitoring?
2. What is the purpose of the CPU?
3. What is the difference between RAM and storage?
4. Which command displays CPU information?
5. Which command displays memory usage?
6. What is a Linux process?
7. What is the purpose of `top`?
8. What do the three load-average values represent?
9. What is the difference between `df` and `du`?
10. Why is system monitoring important in cybersecurity?

---

## 13. Practical Completion Checklist

- [ ] I understand CPU, RAM, storage, and network resources.
- [ ] I inspected CPU information.
- [ ] I checked memory usage.
- [ ] I inspected filesystem storage.
- [ ] I checked system uptime and load averages.
- [ ] I inspected running processes.
- [ ] I identified the most CPU- and memory-intensive processes.
- [ ] I inspected network sockets.
- [ ] I generated `monitoring-report.txt`.

**Lesson 03 is complete when you understand the concepts and successfully generate your monitoring report.**
