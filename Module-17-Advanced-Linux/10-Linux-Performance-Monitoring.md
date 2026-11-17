---
---


# MODULE 17 – ADVANCED LINUX
## LESSON 10: LINUX PERFORMANCE MONITORING

---

## 1. Introduction to Performance Monitoring

Linux performance monitoring is the process of observing system resources to understand how efficiently a computer operates.

A Linux administrator must understand how to identify performance problems before they cause system failures.

Performance monitoring focuses on:

- CPU utilization.
- Memory utilization.
- Disk activity.
- Network activity.
- Running processes.
- System load.
- Resource consumption.
- System bottlenecks.

### Learning objectives

By the end of this lesson, you should be able to:

1. Monitor CPU utilization.
2. Inspect memory usage.
3. Monitor disk performance.
4. Inspect network statistics.
5. Understand system load averages.
6. Monitor running processes.
7. Identify resource bottlenecks.
8. Use Linux performance tools.
9. Generate performance reports.
10. Troubleshoot basic performance problems.

---

## 2. Understanding System Resources

A computer relies on several resources to operate efficiently.

### A. CPU

The Central Processing Unit executes instructions.

High CPU utilization may indicate:

- Heavy application workloads.
- Intensive calculations.
- Background processes.
- Software bugs.
- Unusual process activity.

### B. RAM

Random Access Memory stores data currently being used by applications and the operating system.

Insufficient memory may cause:

- Slow application performance.
- Increased memory pressure.
- Swap activity.
- Application termination under severe memory pressure.

### C. Storage

Storage devices hold files, applications, and system data.

Storage problems may include:

- Low available space.
- High disk latency.
- Excessive input/output operations.
- Filesystem errors.

### D. Network

Network performance affects communication between computers and online services.

Problems may include:

- High latency.
- Packet loss.
- Limited bandwidth.
- Interface errors.

---

## 3. Understanding CPU Monitoring

CPU monitoring helps administrators determine how much processing capacity is being used.

### Display CPU information

```bash
lscpu
```

### Display CPU utilization

```bash
top
```

Press `q` to exit.

### Understand CPU percentages

Common CPU categories include:

| Category | Meaning |
|---|---|
| `us` | User-space processes |
| `sy` | Kernel/system processes |
| `ni` | Processes with adjusted nice values |
| `id` | Idle CPU time |
| `wa` | Time waiting for I/O |
| `hi` | Hardware interrupt processing |
| `si` | Software interrupt processing |
| `st` | Time stolen by a hypervisor |

### Understanding high CPU utilization

High CPU usage is not automatically a problem.

For example, video rendering or scientific calculations may legitimately use a large percentage of CPU capacity.

Investigate unexpected or sustained high usage.

### Display CPU statistics

```bash
mpstat
```

If unavailable:

```bash
sudo apt update
sudo apt install sysstat
```

Display CPU statistics every second:

```bash
mpstat 1 5
```

This collects five reports at one-second intervals.

---

## 4. Understanding System Load Average

Load average represents the average number of tasks that are runnable or waiting in uninterruptible sleep.

Linux commonly displays three load-average values.

Example:

```text
load average: 0.40, 0.65, 0.80
```

These values represent averages over:

- 1 minute.
- 5 minutes.
- 15 minutes.

### Display load average

```bash
uptime
```

Another method:

```bash
cat /proc/loadavg
```

### Understanding load average

Load average must be interpreted in relation to the number of CPU cores.

For example:

| CPU cores | Load average | General interpretation |
|---|---:|---|
| 2 | 0.5 | Relatively light load |
| 2 | 2.0 | Around full runnable capacity |
| 4 | 2.0 | Moderate load |
| 4 | 8.0 | Potentially overloaded |

These are simplified examples, not universal performance thresholds.

I/O-bound tasks can also contribute to load average.

### Display CPU count

```bash
nproc
```

---

## 5. Understanding Memory Monitoring

Memory monitoring helps administrators understand how RAM is being used.

### Display memory information

```bash
free -h
```

### Example

```text
               total        used        free
Mem:            8Gi         3Gi         1Gi
Swap:           2Gi         0Gi         2Gi
```

The values are illustrative.

### Important memory terms

| Term | Meaning |
|---|---|
| Total | Total available memory |
| Used | Memory currently in use |
| Free | Completely unused memory |
| Available | Estimated memory available for new applications |
| Buff/cache | Memory used for buffers and filesystem cache |
| Swap | Storage-backed memory support |

### Inspect detailed memory information

```bash
cat /proc/meminfo
```

Display memory statistics:

```bash
vmstat
```

Display memory statistics every second:

```bash
vmstat 1 5
```

### Understanding swap activity

```bash
swapon --show
```

Swap activity can increase when physical memory is under pressure.

However, the existence of swap usage does not automatically indicate a problem.

---

## 6. Understanding Disk Performance

Disk performance affects application startup, file access, databases, and system responsiveness.

Important storage-performance concepts include:

- Read operations.
- Write operations.
- IOPS.
- Throughput.
- Latency.
- Queue depth.

### A. IOPS

Input/Output Operations Per Second measures how many storage operations are completed per second.

### B. Throughput

Throughput measures the amount of data transferred over a period of time.

### C. Latency

Latency is the time required to complete an operation.

### Inspect disk usage

```bash
df -h
```

### Inspect storage devices

```bash
lsblk
```

### Monitor disk activity

Check whether `iostat` is available:

```bash
command -v iostat
```

If unavailable:

```bash
sudo apt install sysstat
```

Run:

```bash
iostat -xz 1 5
```

This displays extended disk statistics at one-second intervals.

### Important metrics

| Metric | Meaning |
|---|---|
| `r/s` | Read operations per second |
| `w/s` | Write operations per second |
| `rkB/s` | Read throughput |
| `wkB/s` | Write throughput |
| `await` | Average I/O request time |
| `%util` | Device utilization indicator |

Interpretation depends on the storage device and workload. `%util` alone is not a universal measure of disk saturation, particularly with parallel SSD and virtual storage devices.

---

## 7. Understanding Network Performance

Network monitoring helps administrators identify communication problems.

### Display interface statistics

```bash
ip -s link
```

### Display network statistics

```bash
ss -s
```

### Display interface traffic

```bash
cat /proc/net/dev
```

### Inspect routing

```bash
ip route
```

### Test local network latency

```bash
ping -c 5 127.0.0.1
```

### Test an authorized network destination

Use a destination you own or are permitted to test.

```bash
ping -c 5 YOUR_LAB_IP
```

### Network performance concepts

| Metric | Meaning |
|---|---|
| Latency | Time required for a response |
| Bandwidth | Maximum data-transfer capacity |
| Throughput | Actual data-transfer rate |
| Packet loss | Packets that fail to reach their destination |
| Jitter | Variation in packet delay |

---

## 8. Understanding Process Resource Consumption

Processes consume CPU, RAM, and other system resources.

Monitoring them helps identify resource-intensive applications.

### Display running processes

```bash
ps aux
```

### Sort processes by CPU usage

```bash
ps aux --sort=-%cpu
```

### Sort processes by memory usage

```bash
ps aux --sort=-%mem
```

### Display processes interactively

```bash
top
```

### Use htop

Check availability:

```bash
command -v htop
```

Install if required:

```bash
sudo apt install htop
```

Run:

```bash
htop
```

### Why process monitoring matters

It helps identify:

- CPU-intensive processes.
- Memory-heavy applications.
- Unresponsive programs.
- Unexpected background tasks.
- Resource competition.

Do not terminate processes merely because they consume substantial resources. Investigate their purpose first.

---

## 9. Understanding Performance Bottlenecks

A performance bottleneck occurs when one system resource limits overall performance.

### Common bottlenecks

| Bottleneck | Possible symptoms |
|---|---|
| CPU | High sustained CPU usage |
| RAM | Memory pressure and heavy swapping |
| Disk | High I/O latency |
| Network | High latency or packet loss |
| Application | Excessive resource consumption |

### Example 1: CPU bottleneck

A program performs intensive calculations.

Possible observations:

- High CPU usage.
- One or more heavily loaded CPU cores.
- High process CPU percentages.

### Example 2: Memory bottleneck

Too many applications are running simultaneously.

Possible observations:

- Low available memory.
- Increasing swap activity.
- Slower application performance.

### Example 3: Disk bottleneck

An application performs many storage operations.

Possible observations:

- High disk wait time.
- Elevated I/O latency.
- Slow file operations.

### Example 4: Network bottleneck

A system has difficulty communicating with another system.

Possible observations:

- High latency.
- Packet loss.
- Interface errors.
- Low throughput.

### Troubleshooting principle

Do not assume that every slow computer has a CPU problem.

Measure the system before deciding what needs attention.

---

## 10. Understanding Linux Performance Tools

Linux provides several utilities for monitoring system performance.

| Tool | Purpose |
|---|---|
| `top` | Live process and resource monitoring |
| `htop` | Interactive process monitoring |
| `vmstat` | Memory and system statistics |
| `iostat` | Storage I/O statistics |
| `mpstat` | CPU statistics |
| `free` | Memory information |
| `uptime` | System uptime and load average |
| `sar` | Historical system activity |
| `pidstat` | Per-process resource statistics |
| `iotop` | Process-level disk I/O monitoring |

Some utilities are supplied by the `sysstat` package or require separate installation.

### Install monitoring utilities

```bash
sudo apt update
sudo apt install sysstat htop iotop
```

### Inspect process statistics

```bash
pidstat 1 5
```

### Inspect system activity

```bash
sar -u 1 5
```

If historical data collection has not been configured, some `sar` features may not provide past records.

---

## 11. Practical Lab: System Performance Inspection

This exercise uses monitoring commands and does not intentionally overload your VM.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module17-lab/lesson10
cd ~/module17-lab/lesson10
```

### Step 2: Inspect CPU architecture

```bash
lscpu
```

### Step 3: Inspect CPU count

```bash
nproc
```

### Step 4: Inspect system load

```bash
uptime
```

### Step 5: Inspect memory

```bash
free -h
```

### Step 6: Inspect virtual memory statistics

```bash
vmstat 1 5
```

### Step 7: Inspect disk capacity

```bash
df -h
```

### Step 8: Inspect disk performance

```bash
iostat -xz 1 5
```

If `iostat` is unavailable, install the `sysstat` package.

### Step 9: Inspect network statistics

```bash
ip -s link
```

### Step 10: Inspect processes

```bash
ps aux --sort=-%cpu | head -n 10
```

### Step 11: Inspect memory-heavy processes

```bash
ps aux --sort=-%mem | head -n 10
```

### Step 12: Inspect live performance

```bash
top
```

Press `q` to exit.

Record the observations.

---

## 12. Practical Project: Build a Linux Performance Report

Create a Bash script that collects performance information.

### Step 1: Create the script

```bash
nano performance-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson10/performance-report.txt"

{
    echo "======================================"
    echo "       LINUX PERFORMANCE REPORT"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "CPU INFORMATION:"
    lscpu

    echo
    echo "CPU COUNT:"
    nproc

    echo
    echo "SYSTEM LOAD:"
    uptime

    echo
    echo "MEMORY INFORMATION:"
    free -h

    echo
    echo "VIRTUAL MEMORY STATISTICS:"
    vmstat 1 3

    echo
    echo "DISK CAPACITY:"
    df -h

    echo
    echo "DISK STATISTICS:"
    if command -v iostat >/dev/null 2>&1; then
        iostat -xz 1 2
    else
        echo "iostat is not installed."
    fi

    echo
    echo "TOP CPU PROCESSES:"
    ps aux --sort=-%cpu | head -n 10

    echo
    echo "TOP MEMORY PROCESSES:"
    ps aux --sort=-%mem | head -n 10

    echo
    echo "NETWORK STATISTICS:"
    ip -s link

    echo
    echo "======================================"
    echo "          REPORT COMPLETE"
    echo "======================================"

} > "$REPORT" 2>&1

echo "Performance report saved to: $REPORT"
```

### Step 2: Make the script executable

```bash
chmod +x performance-report.sh
```

### Step 3: Run the script

```bash
./performance-report.sh
```

### Step 4: Read the report

```bash
cat ~/module17-lab/lesson10/performance-report.txt
```

### Step 5: Verify the report

```bash
ls -lh ~/module17-lab/lesson10/
```

---

## 13. Performance Monitoring and Cybersecurity

Performance monitoring can also support security investigations.

Unexpected resource consumption may indicate:

- An unauthorized process.
- A compromised application.
- Cryptocurrency mining malware.
- Excessive network communication.
- A denial-of-service condition.
- Resource exhaustion.

However, high resource usage alone does not prove malicious activity.

### Security investigation workflow

1. Identify unusual resource consumption.
2. Identify the responsible process.
3. Inspect its executable path.
4. Inspect its parent process.
5. Examine its network connections.
6. Review relevant system logs.
7. Compare findings with expected system behaviour.
8. Document evidence before making changes.

Useful commands:

```bash
ps aux --sort=-%cpu | head
```

```bash
ss -tulpn
```

```bash
journalctl -p warning -b
```

```bash
pstree -p
```

Do not delete suspicious files or terminate unknown processes before collecting relevant evidence and understanding their role.

---

## 14. Performance Optimization Principles

Performance optimization should be based on evidence.

Important principles include:

### A. Reduce unnecessary background processes

Keep only required services running.

### B. Manage memory efficiently

Avoid running too many memory-intensive applications simultaneously.

### C. Monitor disk capacity

Maintain sufficient free storage space.

### D. Investigate I/O bottlenecks

Determine whether applications are waiting on storage operations.

### E. Optimize applications

Use efficient algorithms and appropriate data structures.

### F. Monitor network performance

Investigate latency, packet loss, and throughput problems.

### G. Establish performance baselines

Record normal system behaviour so unusual changes can be identified.

---

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is Linux performance monitoring?
2. Identify four important system resources.
3. What is CPU utilization?
4. What does system load average represent?
5. What is the difference between RAM and swap?
6. What is IOPS?
7. Explain disk throughput and latency.
8. What does `vmstat` monitor?
9. What is the purpose of `iostat`?
10. How can you identify CPU-intensive processes?
11. What is a performance bottleneck?
12. Explain four possible causes of poor system performance.
13. How can performance monitoring support cybersecurity investigations?
14. Why should administrators establish performance baselines?

---

## 16. Important Commands Summary

| Command | Purpose |
|---|---|
| `lscpu` | Display CPU information |
| `nproc` | Display available processing units |
| `top` | Monitor processes and resources |
| `htop` | Interactive performance monitoring |
| `uptime` | Display uptime and load average |
| `free -h` | Display memory usage |
| `vmstat 1 5` | Monitor virtual memory statistics |
| `iostat -xz 1 5` | Monitor storage performance |
| `mpstat 1 5` | Monitor CPU statistics |
| `pidstat 1 5` | Monitor process statistics |
| `sar -u 1 5` | Display CPU activity |
| `df -h` | Display disk capacity |
| `ip -s link` | Display network statistics |
| `ps aux --sort=-%cpu` | Sort processes by CPU usage |
| `ps aux --sort=-%mem` | Sort processes by memory usage |

---

## 17. Lesson Summary

In this lesson, you studied Linux performance monitoring.

You learned:

- CPU monitoring.
- System load average.
- Memory monitoring.
- Disk performance.
- Network performance.
- Process resource consumption.
- Performance bottlenecks.
- Linux monitoring utilities.
- Performance troubleshooting.
- Security-related performance investigations.

You also built a performance-reporting script.

### Completion checklist

- [ ] Understand CPU utilization.
- [ ] Understand load averages.
- [ ] Monitor memory and swap.
- [ ] Inspect disk performance.
- [ ] Inspect network statistics.
- [ ] Monitor resource-intensive processes.
- [ ] Understand performance bottlenecks.
- [ ] Use Linux performance tools.
- [ ] Build the performance-report script.
- [ ] Understand performance monitoring in cybersecurity.

**END OF LESSON 10**
