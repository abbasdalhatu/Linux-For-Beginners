---
---


# Module 12 – Lesson 08: Swap Memory Management

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain virtual memory.
2. Understand what swap memory is.
3. Differentiate RAM from swap.
4. Identify active swap areas.
5. Monitor swap usage.
6. Understand swap partitions and swap files.
7. Safely create and activate a temporary swap file.
8. Understand the security implications of swap memory.

---

## 2. What Is RAM?

RAM stands for Random Access Memory.

It is the computer's temporary working memory.

When you open applications, run commands, or execute programs, Linux uses RAM to store information that those programs need quickly.

### Example

Imagine opening:

- Firefox
- A terminal
- A Python program
- A text editor

All these applications consume RAM.

RAM is fast, but its capacity is limited.

When memory becomes insufficient, Linux needs another way to manage memory demands.

This is where virtual memory and swap come in.

---

## 3. What Is Virtual Memory?

Virtual memory is a memory-management technique that gives processes an address space independent of the physical RAM layout.

Linux uses virtual memory to manage memory allocation, isolation, and access.

It can also use storage-backed swap space to hold certain memory pages when physical RAM is under pressure.

### Simple analogy

Imagine your study desk is RAM.

Your bookshelf is storage.

When your desk becomes crowded, you move some books to the bookshelf so you can continue working.

The bookshelf is not as fast as your desk, but it gives you additional space.

---

## 4. What Is Swap Memory?

Swap is storage space that Linux can use to hold memory pages that are not currently needed in physical RAM.

Swap can be provided through:

- A swap partition.
- A swap file.

### Important points

- RAM is much faster than disk-based swap.
- Swap is not a replacement for sufficient RAM.
- Heavy swap usage can make a system slower.
- Swap can help prevent memory exhaustion.

### Example

Suppose a computer has:

- 8 GB RAM
- 2 GB swap

The system has 8 GB of physical RAM and an additional 2 GB of configured swap capacity.

This does not mean the computer has 10 GB of equally fast RAM.

---

## 5. Swap Partition vs Swap File

Linux supports two common ways of providing swap.

### A. Swap Partition

A dedicated disk partition used for swap.

Example device name:

```text
/dev/sda3
```

The device name is illustrative.

### B. Swap File

A regular file on an existing filesystem that Linux uses as swap space.

Example:

```text
/swapfile
```

### Comparison

| Feature | Swap Partition | Swap File |
|---|---|---|
| Location | Dedicated partition | Regular file |
| Setup | Requires partition management | Usually easier |
| Resizing | Can require partition changes | Often easier |
| Flexibility | Less flexible | More flexible |
| Common use | Dedicated storage layout | Many modern installations |

For learning and administration, swap files are useful because they can be created without repartitioning a disk.

---

## 6. Checking Memory Usage

Linux provides several commands for inspecting RAM and swap.

### Command 1: free

```bash
free -h
```

Displays RAM and swap information in human-readable units.

Example output:

```text
               total   used   free
Mem:            7.6Gi  2.1Gi  3.2Gi
Swap:           2.0Gi  0.0Gi  2.0Gi
```

The values are illustrative.

### Command 2: swapon

```bash
swapon --show
```

Displays active swap areas.

### Command 3: /proc/meminfo

```bash
grep -E 'MemTotal|MemAvailable|SwapTotal|SwapFree' /proc/meminfo
```

Displays important memory statistics.

### Command 4: vmstat

```bash
vmstat 1 5
```

Displays system memory, process, and performance statistics every second for five reports.

Important columns include:

- `si`: Swap pages brought into RAM.
- `so`: Swap pages moved out of RAM.
- `free`: Free physical memory.
- `swpd`: Amount of used swap.

Repeated nonzero swap activity may indicate memory pressure, but it should be interpreted alongside other system information.

---

## 7. Understanding Swap Activation

A swap area must be activated before Linux can use it.

The command used is:

```bash
sudo swapon DEVICE_OR_FILE
```

To deactivate a swap area:

```bash
sudo swapoff DEVICE_OR_FILE
```

Inspect active swap:

```bash
swapon --show
```

Inspect total memory:

```bash
free -h
```

**Warning:** Do not deactivate your system's existing swap configuration during this lesson. We will work with a temporary swap file created specifically for the exercise.

---

## 8. Understanding Swappiness

Swappiness is a Linux kernel setting that influences how aggressively the system uses swap relative to keeping memory pages in RAM.

Inspect the current setting:

```bash
cat /proc/sys/vm/swappiness
```

Example:

```text
60
```

This is an illustrative value.

The setting ranges from 0 to 200 on modern Linux kernels, although its exact interpretation depends on kernel behaviour and configuration.

A higher value generally encourages more proactive swapping.

A lower value generally favours keeping memory pages in RAM.

### Important

Swappiness is not a percentage of RAM that must be filled before swapping begins.

Do not change this kernel setting during the practical exercise.

---

## 9. Understanding Out-of-Memory Management

The Linux kernel includes an Out-Of-Memory (OOM) mechanism.

When the system cannot satisfy memory requirements, the kernel may terminate processes to recover memory.

Inspect recent kernel messages:

```bash
sudo journalctl -k | grep -i -E 'out of memory|oom|killed process'
```

No output may simply mean no matching events were recorded.

### Why is this important?

On a server, memory exhaustion can affect:

- Websites.
- Databases.
- Applications.
- Network services.
- Security monitoring tools.

Administrators monitor memory usage to identify problems before they become serious.

---

## 10. Practical Lab: Creating a Temporary Swap File

### Objective

Create a small swap file inside your personal course lab, activate it, inspect it, and deactivate it safely.

We will use a 128 MiB swap file.

This exercise does not require repartitioning your VM disk.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson08
cd ~/module12-lab/lesson08
```

### Step 2: Create a 128 MiB file

```bash
dd if=/dev/zero of=temporary-swapfile bs=1M count=128 status=progress
```

This creates a regular file containing 128 MiB of zeroed data.

### Step 3: Set restrictive permissions

```bash
chmod 600 temporary-swapfile
```

Why?

Swap may contain sensitive memory contents. Restrictive permissions help prevent other ordinary users from reading the file.

Verify:

```bash
ls -lh temporary-swapfile
```

### Step 4: Prepare the file as swap

```bash
sudo mkswap temporary-swapfile
```

This writes swap-area metadata into the file.

### Step 5: Activate the temporary swap file

```bash
sudo swapon "$PWD/temporary-swapfile"
```

The file path is resolved from your current lesson directory.

### Step 6: Verify activation

```bash
swapon --show
```

You should see the temporary swap file listed.

### Step 7: Inspect memory usage

```bash
free -h
```

Compare the swap total with the output you observed before activation.

### Step 8: Inspect swap information

```bash
cat /proc/swaps
```

This provides another view of active swap areas.

### Step 9: Deactivate the temporary swap file

```bash
sudo swapoff "$PWD/temporary-swapfile"
```

This removes the temporary swap area from active use.

### Step 10: Verify deactivation

```bash
swapon --show
```

The temporary swap file should no longer appear.

Your system's original swap configuration, if any, should remain unchanged.

### Step 11: Remove the temporary file

Only after successfully deactivating it:

```bash
rm temporary-swapfile
```

### Step 12: Save a memory report

Run:

```bash
{
    echo "LINUX SWAP MANAGEMENT REPORT"
    echo "============================"
    echo
    echo "Memory information:"
    free -h
    echo
    echo "Active swap:"
    swapon --show
    echo
    echo "Swappiness:"
    cat /proc/sys/vm/swappiness
} > swap-report.txt
```

Display your report:

```bash
cat swap-report.txt
```

---

## 11. Cybersecurity Connection

Swap memory has security implications because memory pages can contain sensitive information.

Examples include:

- Authentication tokens.
- Application data.
- Portions of documents.
- Encryption-related information.
- Other temporary application data.

If swap storage is not adequately protected, sensitive information may become accessible to someone who can read the underlying storage.

### Security considerations

1. Restrict permissions on swap files.
2. Protect the underlying storage.
3. Consider encrypted swap where appropriate.
4. Understand how hibernation interacts with swap.
5. Monitor unexpected memory pressure.
6. Avoid exposing sensitive memory data in reports.

### Swap and encryption

Linux systems can use encrypted swap configurations.

Encryption helps protect swap contents when the storage device is accessed outside the running system.

The exact setup depends on the distribution and storage configuration.

---

## 12. Lesson Summary

You learned:

1. RAM is the computer's fast temporary working memory.
2. Virtual memory provides processes with managed address spaces.
3. Swap stores selected memory pages on storage.
4. Linux supports swap partitions and swap files.
5. `free -h` displays memory usage.
6. `swapon --show` displays active swap areas.
7. `swapoff` deactivates swap.
8. Swappiness influences swap behaviour.
9. The OOM mechanism handles severe memory exhaustion.
10. Swap must be protected because it may contain sensitive information.

---

## 13. Knowledge Check

1. What does RAM stand for?
2. What is virtual memory?
3. What is swap memory?
4. State two differences between RAM and swap.
5. What is the difference between a swap partition and a swap file?
6. Which command displays active swap areas?
7. What does `mkswap` do?
8. What does `swapon` do?
9. What does `swapoff` do?
10. What is swappiness?
11. What does OOM stand for?
12. Why should swap storage be protected?

---

## 14. Practical Completion Checklist

- [ ] I understand RAM and virtual memory.
- [ ] I understand swap memory.
- [ ] I understand swap partitions and swap files.
- [ ] I inspected memory using `free -h`.
- [ ] I inspected active swap using `swapon --show`.
- [ ] I checked the swappiness setting.
- [ ] I created a 128 MiB temporary swap file.
- [ ] I set restrictive file permissions.
- [ ] I prepared and activated the swap file.
- [ ] I verified its activation.
- [ ] I deactivated it successfully.
- [ ] I removed the temporary file.
- [ ] I generated my swap report.
- [ ] I understand the security implications of swap memory.

**Lesson 08 is complete when you understand swap management and successfully complete the temporary swap-file exercise.**
