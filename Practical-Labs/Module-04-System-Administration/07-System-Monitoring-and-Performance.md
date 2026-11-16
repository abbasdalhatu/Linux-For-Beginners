
# PRACTICAL LABS
# MODULE 04 – LINUX SYSTEM ADMINISTRATION

## LAB 07: SYSTEM MONITORING AND PERFORMANCE

### 1. Lab Overview

System monitoring is the process of observing a computer's performance, resource consumption, and overall health.

A Linux administrator must ensure that the system operates efficiently and that resources are available when needed.

Important resources include:

- CPU – Central Processing Unit.
- RAM – Random Access Memory.
- Disk – Storage capacity and performance.
- Network – Incoming and outgoing traffic.
- Processes – Programs currently running.
- System load – Workload placed on the CPU.

System monitoring is also important in cybersecurity because unusual resource consumption can indicate misconfiguration, denial-of-service conditions, or suspicious activity.

**Lab environment:** Kali Linux VM.

**Safety:** This lab uses monitoring and diagnostic commands. Do not intentionally overload your VM or terminate unfamiliar processes.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Monitor CPU utilization.
2. Inspect memory consumption.
3. Understand system load averages.
4. Monitor disk performance.
5. Inspect network statistics.
6. Identify resource-intensive processes.
7. Understand performance bottlenecks.
8. Generate a system performance report.

### 3. Understand System Performance

System performance describes how efficiently a computer uses its resources.

A performance problem may occur when:

- CPU usage remains unusually high.
- Available memory becomes very low.
- Disk space becomes exhausted.
- Storage operations become slow.
- Network connections become congested.
- Too many processes compete for resources.

A system administrator should collect evidence before deciding what is causing a performance problem.

### 4. Monitor CPU Information

The CPU executes instructions and performs calculations.

Display CPU information:

```bash
lscpu
```

Display the number of available processors:

```bash
nproc
```

Display CPU-related statistics:

```bash
mpstat
```

If `mpstat` is unavailable, it is provided by the `sysstat` package. You may skip this command rather than installing additional software.

Monitor CPU usage in real time:

```bash
top
```

Inside `top`, press:

- `P` to sort by CPU usage.
- `1` to display individual CPU information.
- `q` to exit.

### 5. Understand System Load Average

Load average describes the average number of processes that are running or waiting for CPU time, along with certain processes in uninterruptible sleep.

Display load averages:

```bash
uptime
```

Example:

```text
12:30:00 up 2 hours, 2 users, load average: 0.50, 0.40, 0.30
```

The three values represent:

1. Average load over the last 1 minute.
2. Average load over the last 5 minutes.
3. Average load over the last 15 minutes.

Inspect the load information directly:

```bash
cat /proc/loadavg
```

**Important:** Load average must be interpreted relative to the number of CPU cores. A load of 2 means something different on a one-core system compared with an eight-core system.

### 6. Monitor Memory Usage

RAM stores data and instructions currently being used by applications and the operating system.

Display memory usage:

```bash
free -h
```

Display memory information:

```bash
cat /proc/meminfo
```

Display selected memory values:

```bash
grep -E 'MemTotal|MemAvailable|MemFree|SwapTotal|SwapFree' /proc/meminfo
```

Important terms:

| Term | Meaning |
|---|---|
| MemTotal | Total physical memory |
| MemFree | Completely unused memory |
| MemAvailable | Estimated memory available for applications |
| SwapTotal | Total swap space |
| SwapFree | Available swap space |

Linux uses available RAM for caching, so low `MemFree` alone does not necessarily indicate a memory problem.

### 7. Inspect Memory-Consuming Processes

Display the processes using the most memory:

```bash
ps aux --sort=-%mem | head -n 10
```

Display CPU-intensive processes:

```bash
ps aux --sort=-%cpu | head -n 10
```

Use:

```bash
top
```

Press `M` to sort processes by memory usage.

This helps administrators identify which applications consume the most resources.

### 8. Monitor Disk Usage

Storage exhaustion can affect system performance and availability.

Display filesystem usage:

```bash
df -h
```

Inspect your home directory:

```bash
du -sh ~
```

Identify large directories:

```bash
du -h --max-depth=1 ~ | sort -hr | head
```

Inspect inode availability:

```bash
df -i
```

Remember that disk capacity and inode availability are different resources.

### 9. Monitor Disk I/O

Disk I/O refers to data being read from or written to storage devices.

Inspect disk statistics:

```bash
cat /proc/diskstats
```

Display block-device information:

```bash
lsblk
```

If `iostat` is available, run:

```bash
iostat
```

For extended statistics:

```bash
iostat -xz 1 3
```

This requests three reports at one-second intervals.

If the command is unavailable, skip it. It is supplied by the `sysstat` package.

Disk I/O monitoring helps identify storage bottlenecks.

### 10. Monitor Network Performance

Network monitoring helps administrators understand communication activity.

Display network interfaces:

```bash
ip -br addr
```

Display interface statistics:

```bash
ip -s link
```

Display routing information:

```bash
ip route
```

Display active and listening connections:

```bash
ss -s
```

Display listening TCP and UDP sockets:

```bash
ss -tuln
```

Display interface counters:

```bash
cat /proc/net/dev
```

Important network statistics include:

- Received bytes.
- Transmitted bytes.
- Received packets.
- Transmitted packets.
- Dropped packets.
- Transmission errors.

These statistics can help identify network problems.

### 11. Inspect Process Resource Consumption

Display all processes:

```bash
ps aux
```

Sort by CPU usage:

```bash
ps aux --sort=-%cpu | head
```

Sort by memory usage:

```bash
ps aux --sort=-%mem | head
```

Display process count:

```bash
ps -e --no-headers | wc -l
```

Display processes belonging to the current user:

```bash
ps -u "$USER" -f
```

### 12. Understand Performance Bottlenecks

A bottleneck occurs when one system resource limits overall performance.

Examples:

| Bottleneck | Possible symptom |
|---|---|
| CPU | Slow application execution |
| RAM | Heavy swapping or application delays |
| Disk | Slow file operations |
| Network | Slow communication |
| Process overload | Excessive resource competition |

A useful troubleshooting approach is:

1. Observe the symptom.
2. Identify the affected resource.
3. Collect relevant statistics.
4. Identify processes or services involved.
5. Check system logs.
6. Determine the likely cause.
7. Recommend a controlled correction.
8. Monitor the system after the correction.

Never make major system changes based on one measurement alone.

### 13. Cybersecurity Application: Performance Anomaly Detection

Unexpected resource consumption may be associated with:

- Misconfigured applications.
- Uncontrolled background tasks.
- Resource-exhaustion attacks.
- Unauthorized scripts.
- Cryptocurrency-mining malware.
- Unexpected network activity.

A performance anomaly is not proof of a security incident.

Investigate using multiple sources of evidence.

Useful commands:

```bash
top
```

```bash
ps aux --sort=-%cpu | head
```

```bash
ps aux --sort=-%mem | head
```

```bash
ss -tuln
```

```bash
journalctl -p warning -b -n 30
```

Compare the results with normal system behavior before drawing conclusions.

### 14. Create a System Performance Monitoring Script

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module04/lab07
```

Create the script:

```bash
nano ~/practical-labs/module04/lab07/performance-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module04/lab07/performance-report.txt"

{
    echo "LINUX SYSTEM PERFORMANCE REPORT"
    echo "================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "CPU INFORMATION"
    lscpu | head -n 15
    echo

    echo "PROCESSOR COUNT"
    nproc
    echo

    echo "SYSTEM LOAD"
    uptime
    cat /proc/loadavg
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

    echo "DISK USAGE"
    df -h
    echo

    echo "NETWORK INTERFACE STATISTICS"
    ip -s link
    echo

    echo "NETWORK CONNECTION SUMMARY"
    ss -s
    echo

    echo "PROCESS COUNT"
    ps -e --no-headers | wc -l

} > "$REPORT" 2>&1

echo "Report generated: $REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x ~/practical-labs/module04/lab07/performance-report.sh
```

Execute it:

```bash
~/practical-labs/module04/lab07/performance-report.sh
```

Read the report:

```bash
cat ~/practical-labs/module04/lab07/performance-report.txt
```

### 15. Practical Exercises

**Exercise 1 – CPU Monitoring**

Run:

```bash
lscpu
nproc
uptime
```

Record the CPU model, number of processors, and system load averages.

**Exercise 2 – Memory Monitoring**

Run:

```bash
free -h
cat /proc/meminfo
```

Record total memory, available memory, and swap information.

**Exercise 3 – Process Analysis**

Run:

```bash
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
```

Identify the processes consuming the most CPU and memory.

**Exercise 4 – Disk Monitoring**

Run:

```bash
df -h
df -i
du -sh ~
```

Record filesystem utilization and inode usage.

**Exercise 5 – Network Monitoring**

Run:

```bash
ip -s link
ip route
ss -s
```

Identify your network interfaces and inspect their statistics.

**Exercise 6 – Performance Investigation**

Open:

```bash
top
```

Observe the CPU, memory, load averages, and running processes.

Exit with `q`.

**Exercise 7 – Generate the Report**

Execute:

```bash
~/practical-labs/module04/lab07/performance-report.sh
```

Inspect the generated report and identify any unusual resource usage.

### 16. Lab Documentation

Create:

```bash
nano ~/practical-labs/module04/lab07/lab07-report.txt
```

Record:

```text
MODULE 04 – LAB 07
SYSTEM MONITORING AND PERFORMANCE

1. CPU model:
2. Number of available processors:
3. One-minute load average:
4. Five-minute load average:
5. Fifteen-minute load average:
6. Total RAM:
7. Available RAM:
8. Root filesystem usage:
9. Highest CPU-consuming process:
10. Highest memory-consuming process:
11. Number of running processes:
12. What I learned:
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 17. Knowledge Check

1. What is system monitoring?
2. What does CPU utilization measure?
3. What are system load averages?
4. What is the difference between MemFree and MemAvailable?
5. Why should administrators monitor swap usage?
6. What does `iostat` help investigate?
7. What does `ip -s link` display?
8. What is a performance bottleneck?
9. How can unexpected CPU usage indicate a security problem?
10. Why should administrators collect multiple sources of evidence before declaring a security incident?

### 18. Lab Completion Checklist

- [ ] Inspected CPU information.
- [ ] Examined system load averages.
- [ ] Monitored RAM and swap.
- [ ] Identified CPU-intensive processes.
- [ ] Identified memory-intensive processes.
- [ ] Inspected disk usage.
- [ ] Examined network statistics.
- [ ] Investigated performance bottlenecks.
- [ ] Generated the performance monitoring report.
- [ ] Completed lab documentation.

**LAB 07 STATUS: READY FOR PRACTICAL COMPLETION**
