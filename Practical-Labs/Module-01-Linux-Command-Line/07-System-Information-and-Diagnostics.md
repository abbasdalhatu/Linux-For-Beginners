
# PRACTICAL LABS
# MODULE 01 – LINUX COMMAND-LINE MASTERY

## LAB 07: SYSTEM INFORMATION AND DIAGNOSTICS

---

## 1. Lab Objectives

By the end of this lab, you should be able to:

1. Identify Linux system information.
2. Inspect kernel and operating system details.
3. Monitor CPU and memory usage.
4. Inspect disk space and filesystem usage.
5. Check uptime and system load.
6. Inspect hardware information.
7. Examine mounted filesystems.
8. Inspect network interface information.
9. Identify system resource problems.
10. Generate a system diagnostic report.

## 2. Prepare Your Lab Environment

```bash
mkdir -p ~/practical-labs/module01/lab07
cd ~/practical-labs/module01/lab07
```

Verify:

```bash
pwd
```

---

## 3. Exercise 1: Identify the Linux Operating System

Run:

```bash
uname
```

This displays the operating system kernel name.

Display detailed kernel information:

```bash
uname -a
```

Display the kernel release:

```bash
uname -r
```

Display the machine architecture:

```bash
uname -m
```

Display the operating system information:

```bash
cat /etc/os-release
```

On Kali Linux, this identifies the distribution and release information.

You can also use:

```bash
hostnamectl
```

This may display:

- Operating system
- Kernel version
- Architecture
- Hostname
- Hardware information

**Security relevance:** Knowing the OS and kernel version helps administrators understand compatibility, patch status, and potential security exposure. A version number alone does not prove that a system is vulnerable.

---

## 4. Exercise 2: Inspect CPU Information

Run:

```bash
lscpu
```

This displays processor information, including:

- Architecture
- CPU model
- Number of CPUs
- Threads per core
- Cores per socket
- Virtualization information

Display processor information directly:

```bash
cat /proc/cpuinfo
```

Display only the first section:

```bash
head -n 25 /proc/cpuinfo
```

Count the available processing units:

```bash
nproc
```

### Practical task

Run:

```bash
echo "Available processing units:"
nproc
```

Record the CPU architecture and number of available processing units in your notebook.

---

## 5. Exercise 3: Monitor Memory Usage

Run:

```bash
free -h
```

Understand the output:

| Column | Meaning |
|---|---|
| total | Total available memory |
| used | Memory currently used |
| free | Completely unused memory |
| shared | Memory used by shared memory |
| buff/cache | Memory used for buffers and cache |
| available | Estimated memory available to applications |

### Inspect memory information

```bash
cat /proc/meminfo
```

View the first few entries:

```bash
head -n 15 /proc/meminfo
```

Find total memory:

```bash
grep MemTotal /proc/meminfo
```

Find available memory:

```bash
grep MemAvailable /proc/meminfo
```

### Practical task

Run:

```bash
free -h
```

Record:

1. Total memory.
2. Used memory.
3. Available memory.
4. Swap usage.

Remember that Linux uses otherwise-unused RAM for caching. High memory usage does not automatically mean that the system has a memory problem.

---

## 6. Exercise 4: Inspect Swap Memory

Swap is disk space used as an extension of memory management.

Inspect swap usage:

```bash
swapon --show
```

Another option:

```bash
free -h
```

Inspect swap information:

```bash
cat /proc/swaps
```

If no swap device appears, your VM may not have configured swap.

Do not create or resize swap partitions during this introductory diagnostic lab.

---

## 7. Exercise 5: Inspect Disk Space

Run:

```bash
df -h
```

The `df` command reports filesystem disk usage.

Understand the columns:

| Column | Meaning |
|---|---|
| Filesystem | Filesystem or storage device |
| Size | Total filesystem size |
| Used | Space currently used |
| Avail | Available space |
| Use% | Percentage of space used |
| Mounted on | Mount location |

Display filesystem types:

```bash
df -Th
```

Inspect the filesystem containing your home directory:

```bash
df -h "$HOME"
```

### Practical task

Run:

```bash
df -hT
```

Record the filesystem containing your home directory and its available space.

---

## 8. Exercise 6: Inspect Directory Storage Usage

The `du` command estimates the storage space used by files and directories.

Inspect your current lab directory:

```bash
du -sh .
```

Inspect your practical-labs directory:

```bash
du -sh ~/practical-labs
```

Display the sizes of individual entries:

```bash
du -h --max-depth=1 ~/practical-labs
```

Find the largest entries in your lab directory:

```bash
du -ah ~/practical-labs | sort -hr | head -n 10
```

These commands help identify directories consuming significant storage.

**Safety note:** These are inspection commands. Do not delete files simply because they appear large. Investigate what they contain first.

---

## 9. Exercise 7: Inspect System Uptime and Load

Run:

```bash
uptime
```

Example:

```text
12:30:00 up 2 days, 3:15, 2 users, load average: 0.20, 0.35, 0.40
```

The output includes:

- Current system time.
- Time since the system started.
- Number of logged-in users.
- Load averages over 1, 5, and 15 minutes.

### Understand load average

Load average represents the average number of tasks that are runnable or waiting for certain resources.

A load average of 2 does not mean CPU usage is automatically 200%.

Interpret load relative to the number of available CPU processing units and whether tasks are waiting for CPU or other resources.

Inspect the CPU count:

```bash
nproc
```

### Practical task

Run:

```bash
uptime
nproc
```

Record the three load averages and the number of available processing units.

---

## 10. Exercise 8: Inspect Running Services

Modern Kali Linux uses systemd for service management.

Check whether systemd is available:

```bash
ps -p 1 -o comm=
```

If it returns `systemd`, inspect active services:

```bash
systemctl list-units --type=service --state=running
```

Inspect all service units:

```bash
systemctl list-unit-files --type=service
```

Check a specific service:

```bash
systemctl status ssh
```

The SSH service may not be installed or running. That is not necessarily a problem.

You can also inspect listening services using:

```bash
ss -tuln
```

This displays listening TCP and UDP sockets.

For this lab, inspect only. Do not enable, disable, or modify services.

---

## 11. Exercise 9: Inspect Mounted Filesystems

Run:

```bash
mount
```

This displays mounted filesystems.

For a more readable overview:

```bash
findmnt
```

Display filesystem information:

```bash
lsblk
```

Include filesystem types and mount points:

```bash
lsblk -f
```

Understand common terms:

| Term | Meaning |
|---|---|
| Device | Storage device or partition |
| Mount point | Directory where a filesystem is accessible |
| Filesystem | Structure used to store files |
| UUID | Unique filesystem identifier |

Inspect the filesystem containing your home directory:

```bash
findmnt "$HOME"
```

This helps you understand how Linux storage is organized.

---

## 12. Exercise 10: Inspect Hardware Information

Linux exposes hardware information through several tools.

### PCI devices

```bash
lspci
```

### USB devices

```bash
lsusb
```

### Block storage devices

```bash
lsblk
```

### Hardware summary

```bash
sudo lshw -short
```

`lshw` may not be installed. If required, install it from Kali's official repositories:

```bash
sudo apt update
sudo apt install lshw
```

Then run:

```bash
sudo lshw -short
```

Some hardware information may be unavailable inside a virtual machine because VMware presents virtual hardware to the guest operating system.

That is expected.

---

## 13. Exercise 11: Inspect Kernel Messages

The Linux kernel records messages about hardware, drivers, and system activity.

Display recent kernel messages:

```bash
dmesg | tail -n 30
```

Search for errors:

```bash
dmesg --level=err
```

Display warning messages:

```bash
dmesg --level=warn
```

Some systems restrict access to kernel messages. If permission is denied, use:

```bash
sudo dmesg --level=err
```

### Important security concept

Kernel messages can help diagnose:

- Hardware detection problems.
- Driver failures.
- Storage errors.
- Network device problems.
- Kernel warnings.

Do not assume every warning indicates an active security incident.

---

## 14. Exercise 12: Inspect System Logs

System logs help administrators investigate operating system activity.

Check whether the system journal is available:

```bash
command -v journalctl
```

Display recent journal entries:

```bash
journalctl -n 30
```

Display entries from the current boot:

```bash
journalctl -b
```

Display errors from the current boot:

```bash
journalctl -b -p err
```

Follow new journal entries:

```bash
journalctl -f
```

Press `CTRL + C` to stop following the journal.

Some journal entries may require administrative privileges.

Use:

```bash
sudo journalctl -b -p err
```

when access is restricted.

---

## 15. Exercise 13: Inspect Network Interface Information

Display network interfaces:

```bash
ip link
```

Display assigned IP addresses:

```bash
ip addr
```

Display the routing table:

```bash
ip route
```

Display listening sockets:

```bash
ss -tuln
```

Inspect network statistics:

```bash
ip -s link
```

These commands are useful when diagnosing:

- Missing IP addresses.
- Disconnected interfaces.
- Unexpected network routes.
- Packet errors and dropped packets.
- Unexpected listening services.

Because Kali is running inside VMware, its network interfaces and addresses may differ from those on your Windows host.

Do not change VMware network settings during this exercise.

---

## 16. Exercise 14: Build a System Diagnostic Script

Create:

```bash
nano system-diagnostics.sh
```

Enter:

```bash
#!/bin/bash

REPORT="system-diagnostics-report.txt"

{
    echo "======================================"
    echo "       LINUX SYSTEM DIAGNOSTICS"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "OPERATING SYSTEM:"
    cat /etc/os-release

    echo
    echo "KERNEL:"
    uname -a

    echo
    echo "CPU:"
    lscpu | head -n 15

    echo
    echo "MEMORY:"
    free -h

    echo
    echo "DISK USAGE:"
    df -h

    echo
    echo "SYSTEM UPTIME:"
    uptime

    echo
    echo "BLOCK DEVICES:"
    lsblk

    echo
    echo "NETWORK INTERFACES:"
    ip -brief addr

    echo
    echo "LISTENING SOCKETS:"
    ss -tuln

    echo
    echo "======================================"
    echo "       DIAGNOSTICS COMPLETED"
    echo "======================================"

} 2>&1 | tee "$REPORT"

echo
echo "Report saved to: $REPORT"
```

Make it executable:

```bash
chmod +x system-diagnostics.sh
```

Run:

```bash
./system-diagnostics.sh
```

Inspect the saved report:

```bash
less system-diagnostics-report.txt
```

The script collects diagnostic information and displays it while saving a copy.

Remember that diagnostic reports can contain hostnames, IP addresses, usernames, and other system details. Keep them private when sharing.

---

## 17. Practical Challenge

Perform a complete diagnostic inspection of your Kali VM.

Collect the following information:

1. Linux distribution and version.
2. Kernel version.
3. CPU architecture.
4. Number of processing units.
5. Total and available memory.
6. Swap usage.
7. Disk space available.
8. Top five largest directories in your practical-labs folder.
9. System uptime and load average.
10. Running system services.
11. Mounted filesystems.
12. Network interface addresses.
13. Listening TCP and UDP sockets.
14. Recent kernel errors.
15. Current boot errors from the system journal, if available.

Save your findings in:

```text
system-diagnostics-report.txt
```

Write a short conclusion explaining whether you observed any unusual resource usage or diagnostic errors.

Do not modify system configuration while performing this assessment.

---

## 18. Knowledge Check

Answer these questions in your notebook:

1. What does `uname -a` display?
2. What is the purpose of `/etc/os-release`?
3. What does `lscpu` display?
4. What is the difference between RAM and swap?
5. What does `free -h` display?
6. What is the purpose of `df -h`?
7. What is the difference between `df` and `du`?
8. What does system load average represent?
9. What is the purpose of `systemctl`?
10. What does `lsblk` display?
11. What is the purpose of `findmnt`?
12. What does `dmesg` display?
13. What is `journalctl` used for?
14. What does `ip addr` display?
15. Why is system diagnostics important in cybersecurity?

---

## 19. Lab Completion Checklist

- [ ] Identified the Linux distribution.
- [ ] Inspected kernel information.
- [ ] Inspected CPU information.
- [ ] Checked memory and swap usage.
- [ ] Checked disk space.
- [ ] Inspected directory storage usage.
- [ ] Checked uptime and load averages.
- [ ] Inspected running services.
- [ ] Examined mounted filesystems.
- [ ] Inspected hardware information.
- [ ] Examined kernel messages.
- [ ] Inspected system logs.
- [ ] Checked network interfaces.
- [ ] Built the diagnostic script.
- [ ] Completed the practical challenge.

**END OF LAB 07**
