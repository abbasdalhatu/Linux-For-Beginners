---
---

# MODULE 02: KALI LINUX SETUP – FINAL ASSESSMENT

**Course:** Linux for Ethical Hackers  
**Module:** 02 – Kali Linux Setup  
**Total Marks:** 100  
**Pass Mark:** 50%

## SECTION A: MULTIPLE-CHOICE QUESTIONS (30 MARKS)

Choose the correct answer. Each question carries 3 marks.

1. Kali Linux is primarily based on which Linux distribution?

   A. Fedora  
   B. Debian  
   C. Arch Linux  
   D. Ubuntu

2. Which organization maintains Kali Linux?

   A. Microsoft  
   B. Google  
   C. Offensive Security  
   D. Red Hat

3. What is virtualization?

   A. Deleting an operating system  
   B. Running virtual computers using software  
   C. Encrypting a hard drive  
   D. Installing applications

4. VMware Workstation is an example of which hypervisor type?

   A. Type 1  
   B. Type 2  
   C. Type 3  
   D. Type 4

5. Which VMware network mode allows a virtual machine to share the host's network connection through NAT?

   A. Bridged  
   B. Host-only  
   C. NAT  
   D. Offline

6. Which command updates Kali's package information?

   A. `sudo apt update`  
   B. `sudo apt delete`  
   C. `sudo apt remove`  
   D. `sudo apt stop`

7. What does UID 0 represent in Linux?

   A. Guest user  
   B. Normal user  
   C. Root user  
   D. Network user

8. Which command displays network interfaces and IP addresses?

   A. `ls`  
   B. `ip addr`  
   C. `pwd`  
   D. `whoami`

9. What is the primary purpose of a VMware snapshot?

   A. To increase RAM  
   B. To save a virtual machine's state at a point in time  
   C. To install Kali automatically  
   D. To remove malware from Windows

10. Which command displays filesystem disk usage?

    A. `free -h`  
    B. `top`  
    C. `df -h`  
    D. `ip route`

## SECTION B: TRUE OR FALSE (20 MARKS)

Each question carries 2 marks.

1. Kali Linux is a Debian-based distribution.
2. VMware allows multiple operating systems to run as virtual machines.
3. NAT and host-only networking provide exactly the same functionality.
4. The `apt update` command installs all available package upgrades.
5. The root account has UID 0.
6. A snapshot is a complete replacement for a separate backup.
7. The `ip route` command displays routing information.
8. The `free -h` command displays memory information.
9. Linux logs can help administrators investigate system problems.
10. Ethical hackers must obtain authorization before testing systems they do not own.

## SECTION C: SHORT-ANSWER QUESTIONS (30 MARKS)

Each question carries 5 marks.

1. Explain what Kali Linux is and state two of its uses.

2. Differentiate between NAT, Bridged, and Host-only networking.

3. Explain the difference between `apt update` and `apt upgrade`.

4. What is the difference between the root user and a normal Linux user?

5. Explain why snapshots and separate backups are both important.

6. List five commands used to troubleshoot a Kali Linux system and explain what each does.

## SECTION D: PRACTICAL ASSESSMENT (20 MARKS)

Complete these tasks inside your Kali virtual machine.

### Task 1: System Information (4 marks)

Run:

```bash
uname -a
hostname
whoami
```

Explain what each command displays.

### Task 2: Network Configuration (4 marks)

Run:

```bash
ip addr
ip route
```

Identify your network interface, IP address, and default route if available.

### Task 3: System Resources (4 marks)

Run:

```bash
free -h
df -h
```

Explain what the output tells you about memory and storage.

### Task 4: Package Management (4 marks)

Run:

```bash
sudo apt update
apt search tree
```

Explain the purpose of both commands.

### Task 5: System Logs (4 marks)

Run:

```bash
sudo journalctl -n 20
```

Explain why system logs are useful.

## ASSESSMENT COMPLETION

Submit your answers to Sections A, B, and C, and complete the practical tasks in Section D.

**Pass requirement:** 50/100.

**Remember:** Understanding the commands and explaining their purpose is more important than memorizing them.

After completing this assessment, proceed to Module 03: Linux Filesystem.