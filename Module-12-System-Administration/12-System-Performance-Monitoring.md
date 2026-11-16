
# Module 12 – Lesson 12: System Performance Monitoring

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain system performance monitoring.
2. Understand CPU performance.
3. Monitor RAM and swap usage.
4. Monitor disk performance.
5. Monitor network performance.
6. Understand system load averages.
7. Identify resource-intensive processes.
8. Use Linux performance-monitoring tools.
9. Recognise performance bottlenecks.
10. Generate a system performance report.

---

## 2. What Is System Performance Monitoring?

System performance monitoring is the process of observing and analysing how efficiently a computer uses its resources.

These resources include:

- CPU
- RAM
- Storage
- Network
- Running processes

### Real-world example

Imagine a university server hosting an online learning platform.

Students complain that the website is slow.

A system administrator needs to investigate:

- Is the CPU overloaded?
- Is RAM almost exhausted?
- Is the disk responding slowly?
- Is network traffic unusually high?
- Is one process consuming excessive resources?

Performance monitoring helps identify the cause.

---

## 3. Understanding CPU Performance

CPU stands for Central Processing Unit.

It executes instructions and performs calculations.

When a CPU is under heavy workload, applications may become slower.

### Important CPU concepts

**CPU utilisation:** How much CPU processing capacity is being used.

**CPU cores:** Independent processing units within a processor.

**Load average:** A measure of the average system workload over time.

### Check CPU information

```bash
lscpu
```

### Monitor CPU activity

```bash
top
```

### Display CPU statistics

```bash
mpstat
```

If `mpstat` is unavailable, check whether the command exists:

```bash
command -v mpstat
```

It is provided by the `sysstat` package on Debian-based systems.

You do not need to install it for this lesson.

---

## 4. Understanding Load Average

Linux displays load averages through commands such as:

```bash
uptime
```

Example:

```text
load average: 0.45, 0.70, 0.90
```

These three values represent average system load over:

1. The last 1 minute.
2. The last 5 minutes.
3. The last 15 minutes.

### Interpreting load average

Suppose a computer has 4 CPU cores.

A load average of 4 roughly indicates four units of runnable or uninterruptible workload on average.

A load average above 4 may indicate more work than the CPU can handle immediately.

However, load average is not a direct CPU utilisation percentage. It also includes certain tasks waiting in uninterruptible sleep, often for I/O.

Always examine CPU utilisation and I/O activity alongside load average.

### Check the number of CPU cores

```bash
nproc
```

This displays the number of processing units available to the current process.

---

## 5. Monitoring RAM Performance

RAM is temporary memory used by running programs.

Insufficient available memory can cause performance problems.

### Check RAM usage

```bash
free -h
```

Important fields include:

| Field | Meaning |
|---|---|
| total | Total memory |
| used | Memory currently used |
| free | Completely unused memory |
| shared | Shared memory |
| buff/cache | Buffers and cache |
| available | Estimated memory available for new applications |

### Important concept

Linux uses available memory for caching to improve performance.

Therefore, low `free` memory does not automatically mean the system is running out of RAM.

Pay attention to the `available` value.

### Monitor memory continuously

```bash
watch -n 2 free -h
```

This refreshes memory information every two seconds.

Press `CTRL + C` to stop.

---

## 6. Monitoring Swap Performance

Swap provides storage-backed memory for selected pages.

Excessive swapping can slow down a system.

### Inspect active swap

```bash
swapon --show
```

### Inspect memory and swap

```bash
free -h
```

### Monitor swap activity

```bash
vmstat 1 5
```

Important columns:

- `si`: Memory pages swapped into RAM.
- `so`: Memory pages swapped out to storage.
- `swpd`: Amount of used swap.

Repeated swap activity under heavy memory pressure may indicate that the system needs more memory or that applications are consuming too much RAM.

---

## 7. Monitoring Disk Performance

Storage performance affects application loading, file transfers, databases, and system responsiveness.

### Check disk usage

```bash
df -h
```

### Inspect storage devices

```bash
lsblk
```

### Inspect directory sizes

```bash
du -sh ~
```

### Monitor disk I/O

```bash
iostat
```

If unavailable:

```bash
command -v iostat
```

`iostat` is also provided by the `sysstat` package on Debian-based systems.

### Important disk performance terms

| Term | Meaning |
|---|---|
| IOPS | Input/output operations per second |
| Throughput | Amount of data transferred per unit of time |
| Latency | Time required to complete an operation |
| Utilisation | How busy a storage device is |

A disk may have plenty of free space but still perform poorly because of high I/O activity or latency.

---

## 8. Monitoring Network Performance

Network performance affects communication between computers and servers.

Important network performance measurements include:

- Bandwidth
- Throughput
- Latency
- Packet loss
- Connection count

### Check network interfaces

```bash
ip addr
```

### Check routing

```bash
ip route
```

### Display network connections

```bash
ss -tuln
```

### Test network latency

```bash
ping -c 4 1.1.1.1
```

The output displays round-trip time measurements when responses are received.

### Inspect interface statistics

```bash
ip -s link
```

This displays transmitted and received packet and byte counters, along with error and drop counters.

These counters can help identify network-interface problems.

---

## 9. Identifying Resource-Intensive Processes

Sometimes a single application consumes excessive system resources.

Linux provides tools for identifying such processes.

### Sort by CPU usage

```bash
ps aux --sort=-%cpu
```

### Sort by memory usage

```bash
ps aux --sort=-%mem
```

### Display process information

```bash
ps -eo pid,ppid,%cpu,%mem,stat,comm
```

### Monitor processes interactively

```bash
top
```

### Example scenario

Suppose a Python program consumes 95% CPU for several minutes.

Possible causes include:

- An infinite loop.
- Heavy computation.
- Excessive background processing.
- Poorly optimised code.

High CPU usage is not automatically malicious. Investigate the process before taking action.

---

## 10. Understanding Performance Bottlenecks

A bottleneck occurs when one resource limits the overall performance of a system.

### A. CPU bottleneck

Symptoms:

- High CPU usage.
- Slow application processing.
- High runnable process count.

### B. Memory bottleneck

Symptoms:

- Very low available RAM.
- Heavy swap activity.
- Applications becoming unresponsive.

### C. Storage bottleneck

Symptoms:

- Slow file operations.
- High disk I/O wait.
- Delayed application loading.

### D. Network bottleneck

Symptoms:

- Slow downloads.
- High latency.
- Packet loss.
- Network congestion.

### Important lesson

A slow computer does not always need more RAM or a faster CPU.

The correct solution depends on identifying the actual bottleneck.

---

## 11. Practical Lab: Linux Performance Investigation

### Objective

Inspect CPU, memory, storage, network, and process activity in your Kali Linux VM.

All activities are diagnostic and do not require system configuration changes.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson12
cd ~/module12-lab/lesson12
```

### Step 2: Inspect CPU information

```bash
lscpu
```

Record:

- CPU architecture.
- Number of CPUs.
- CPU model.

### Step 3: Check available CPU units

```bash
nproc
```

### Step 4: Inspect load average

```bash
uptime
```

Record the three load-average values.

### Step 5: Inspect memory

```bash
free -h
```

Pay particular attention to available RAM and swap usage.

### Step 6: Inspect swap activity

```bash
vmstat 1 5
```

Record the `si` and `so` values.

### Step 7: Inspect storage

```bash
lsblk
df -h
```

Identify your mounted filesystems and available space.

### Step 8: Inspect network interfaces

```bash
ip -s link
```

Record packet counts and any visible errors or drops.

### Step 9: Inspect network connections

```bash
ss -tuln
```

Identify any listening TCP or UDP ports.

### Step 10: Identify CPU-intensive processes

```bash
ps aux --sort=-%cpu | head -n 10
```

### Step 11: Identify memory-intensive processes

```bash
ps aux --sort=-%mem | head -n 10
```

### Step 12: Observe system performance

```bash
top
```

Observe CPU and memory activity.

Press `q` to exit.

---

## 12. Practical Lab: Generate a Performance Report

Run the following command:

```bash
{
    echo "LINUX SYSTEM PERFORMANCE REPORT"
    echo "================================"
    echo
    echo "Date:"
    date
    echo
    echo "Hostname:"
    hostname
    echo
    echo "CPU Information:"
    lscpu
    echo
    echo "Available CPU Units:"
    nproc
    echo
    echo "System Load:"
    uptime
    echo
    echo "Memory Information:"
    free -h
    echo
    echo "Swap Information:"
    swapon --show
    echo
    echo "Storage Information:"
    df -h
    echo
    echo "Network Interface Statistics:"
    ip -s link
    echo
    echo "Top CPU-Consuming Processes:"
    ps aux --sort=-%cpu | head -n 10
    echo
    echo "Top Memory-Consuming Processes:"
    ps aux --sort=-%mem | head -n 10
    echo
    echo "Listening Network Ports:"
    ss -tuln
} > performance-report.txt
```

Display your report:

```bash
cat performance-report.txt
```

Your report should be saved at:

```text
~/module12-lab/lesson12/performance-report.txt
```

---

## 13. Cybersecurity Connection

Performance monitoring is important in cybersecurity because malicious activity can affect system resources.

Examples include:

### A. Cryptomining malware

Malicious software may consume excessive CPU resources to mine cryptocurrency.

### B. Denial-of-Service activity

A system may experience excessive requests or network traffic that affects availability.

### C. Resource exhaustion

A malicious or malfunctioning application may consume excessive RAM or storage.

### D. Suspicious background processes

Unexpected processes may indicate unauthorised software or persistence.

### Security investigation approach

When investigating unusual resource usage:

1. Identify the affected resource.
2. Find the process responsible.
3. Inspect its PID and parent PID.
4. Examine its executable location.
5. Inspect network connections.
6. Review relevant system logs.
7. Compare findings with normal system behaviour.

Do not terminate processes based only on high resource consumption.

---

## 14. Lesson Summary

You learned:

1. Performance monitoring evaluates system resource usage.
2. CPU utilisation measures processor activity.
3. Load average represents system workload over time.
4. RAM and swap influence memory performance.
5. Disk performance involves throughput, latency, and IOPS.
6. Network performance involves bandwidth, latency, and packet loss.
7. `top` and `ps` help identify resource-intensive processes.
8. Bottlenecks can occur in CPU, memory, storage, or networking.
9. Monitoring supports troubleshooting and cybersecurity investigations.

---

## 15. Knowledge Check

1. What is system performance monitoring?
2. What does CPU stand for?
3. What are CPU cores?
4. What do the three load-average values represent?
5. Why is low free RAM not always a problem?
6. What does `vmstat` monitor?
7. What does IOPS mean?
8. What is network latency?
9. What is a performance bottleneck?
10. Which command displays processes sorted by CPU usage?
11. How can excessive CPU usage relate to cybersecurity?
12. Why should suspicious processes be investigated before termination?

---

## 16. Practical Completion Checklist

- [ ] I understand system performance monitoring.
- [ ] I inspected CPU information.
- [ ] I checked CPU availability.
- [ ] I examined load averages.
- [ ] I inspected RAM and swap.
- [ ] I inspected disk usage.
- [ ] I examined network-interface statistics.
- [ ] I identified resource-intensive processes.
- [ ] I used `top` to observe performance.
- [ ] I generated my performance report.
- [ ] I understand common performance bottlenecks.
- [ ] I understand the cybersecurity applications of performance monitoring.

**Lesson 12 is complete when you understand the concepts and successfully generate your performance report.**
