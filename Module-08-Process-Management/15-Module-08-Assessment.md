# MODULE 08: PROCESS MANAGEMENT

## LESSON 15: MODULE ASSESSMENT

### 1. Assessment Overview

This assessment evaluates your understanding of Linux process management.

It covers:

1. Introduction to Linux processes.
2. Process IDs and parent process IDs.
3. Viewing processes with `ps`.
4. Monitoring processes with `top` and `htop`.
5. Linux process states.
6. Foreground and background processes.
7. Job control using `jobs`, `fg`, and `bg`.
8. Linux signals.
9. Terminating processes.
10. Process priority and niceness.
11. Daemons and services.
12. Managing services with `systemctl`.
13. Process monitoring and troubleshooting.

### 2. Assessment Instructions

- Answer every question.
- Attempt the practical tasks in your Kali Linux VM.
- Do not guess when you can verify something using a command.
- Use your own words for explanations.
- Only practise on systems you own or have permission to administer.

**Total Marks: 100**

| Section | Description | Marks |
|---|---|---:|
| A | Multiple Choice Questions | 20 |
| B | Short Answer Questions | 20 |
| C | Command Interpretation | 20 |
| D | Practical Laboratory | 25 |
| E | Cybersecurity Scenario | 15 |
| **Total** | | **100** |

---

## SECTION A: MULTIPLE CHOICE QUESTIONS

**20 Questions – 1 Mark Each**

Choose the correct answer.

**1. What is a process in Linux?**

A. A physical computer component

B. A running instance of a program

C. A Linux directory

D. A network cable

**2. What does PID mean?**

A. Process Internal Directory

B. Program Installation Data

C. Process Identifier

D. Primary Internet Device

**3. Which command displays all processes in a detailed BSD-style format?**

A. `mkdir`

B. `ps aux`

C. `pwd`

D. `touch`

**4. Which process normally runs as PID 1 on a systemd-based Linux system?**

A. bash

B. cron

C. systemd

D. ssh

**5. Which command provides real-time process monitoring?**

A. `top`

B. `cat`

C. `cp`

D. `chmod`

**6. What does process state R mean?**

A. Removed

B. Running or runnable

C. Restarted

D. Reserved

**7. Which keyboard shortcut suspends a foreground process in a terminal?**

A. Ctrl+C

B. Ctrl+Z

C. Ctrl+D

D. Ctrl+P

**8. Which command displays the current shell's background and stopped jobs?**

A. `jobs`

B. `users`

C. `groups`

D. `whoami`

**9. Which command resumes a stopped job in the foreground?**

A. `bg`

B. `kill`

C. `fg`

D. `ps`

**10. Which signal requests graceful process termination?**

A. SIGKILL

B. SIGTERM

C. SIGSTOP

D. SIGCONT

**11. Which signal cannot be caught or ignored by a process?**

A. SIGINT

B. SIGHUP

C. SIGTERM

D. SIGKILL

**12. Which command searches for processes by name?**

A. `pgrep`

B. `pwd`

C. `ls`

D. `df`

**13. What is the purpose of nice values?**

A. To manage file permissions

B. To influence CPU scheduling priority

C. To configure network addresses

D. To encrypt files

**14. Which nice value represents the highest scheduling preference among ordinary nice values?**

A. +19

B. +10

C. 0

D. -20

**15. What is a daemon?**

A. A background process that provides a service or performs a task

B. A Linux user account

C. A type of file permission

D. A storage device

**16. Which command displays the status of a systemd service?**

A. `systemctl status`

B. `chmod status`

C. `ps status`

D. `service create`

**17. What does `systemctl enable` generally do?**

A. Deletes a service

B. Configures automatic activation

C. Immediately kills a service

D. Changes the service's password

**18. Which command displays available memory information?**

A. `free -h`

B. `kill -9`

C. `who`

D. `touch`

**19. What does the Linux load average represent?**

A. The amount of disk space available

B. The average number of runnable and uninterruptible tasks over time

C. The number of user accounts

D. The number of installed packages

**20. Which directory exposes process and kernel information through a virtual filesystem?**

A. `/home`

B. `/etc`

C. `/proc`

D. `/boot`

---

## SECTION B: SHORT ANSWER QUESTIONS

**10 Questions – 2 Marks Each**

Answer in your own words.

1. Explain the difference between a program and a process.

2. What is the difference between PID and PPID?

3. Explain the purpose of `ps aux`.

4. Differentiate between foreground and background processes.

5. Explain the difference between SIGTERM and SIGKILL.

6. What is the difference between `nice` and `renice`?

7. Explain the difference between a daemon and a service.

8. Differentiate between `systemctl start` and `systemctl enable`.

9. What is a zombie process?

10. Explain why process monitoring is important in cybersecurity.

---

## SECTION C: COMMAND INTERPRETATION

**5 Questions – 4 Marks Each**

Explain what each command does.

### Question 1

```bash
ps -eo pid,ppid,comm,%cpu --sort=-%cpu
```

Explain:

- What information is displayed?
- What does PID mean?
- What does PPID mean?
- Why is `%cpu` included?

### Question 2

```bash
sleep 300 &
```

Explain:

- What does `sleep 300` do?
- What does `&` do?
- How can you find the process ID?

### Question 3

```bash
kill -TERM 1234
```

Explain:

- What does the command attempt to do?
- What does TERM mean?
- What does `1234` represent?
- Why is graceful termination preferred?

### Question 4

```bash
sudo systemctl enable --now cron
```

Explain:

- What does `sudo` do?
- What does `enable` do?
- What does `--now` add?
- What is the purpose of the cron service?

### Question 5

```bash
free -h
```

Explain:

- What information does the command display?
- What does `-h` mean?
- What is swap?
- Why is the available-memory value useful?

---

## SECTION D: PRACTICAL LABORATORY

**25 Marks**

Complete the following exercises inside your Kali Linux VM.

### Exercise 1: Process Identification – 5 Marks

Run:

```bash
sleep 300 &
```

Then run:

```bash
echo $!
```

Tasks:

1. Identify the sleep process PID.
2. Display its details using `ps`.
3. Identify its PPID.
4. Display its current process state.
5. Terminate your test process safely.

### Exercise 2: Process Monitoring – 5 Marks

Run:

```bash
top
```

Tasks:

1. Identify the CPU usage section.
2. Identify the memory usage section.
3. Observe the running processes.
4. Sort processes by CPU usage.
5. Exit using `q`.

### Exercise 3: Job Control – 5 Marks

Run:

```bash
sleep 200
```

Tasks:

1. Suspend the process using Ctrl+Z.
2. Display jobs using `jobs`.
3. Resume it in the background using `bg`.
4. Bring it to the foreground using `fg`.
5. Terminate it using Ctrl+C.

### Exercise 4: Service Investigation – 5 Marks

Run:

```bash
systemctl --failed
```

Then run:

```bash
systemctl list-units --type=service --state=running
```

Tasks:

1. Identify running services.
2. Inspect one service using `systemctl status`.
3. Identify its current state.
4. Explain why administrators monitor failed services.
5. Do not stop or disable essential services.

### Exercise 5: System Resource Monitoring – 5 Marks

Run:

```bash
uptime
```

```bash
free -h
```

```bash
vmstat 2 5
```

Tasks:

1. Record the system load average.
2. Identify total memory.
3. Identify available memory.
4. Explain what swap activity means.
5. Explain one possible reason for high system load.

---

## SECTION E: CYBERSECURITY SCENARIO

**15 Marks**

### Scenario: Suspicious Process Investigation

Imagine you are a junior Linux security analyst.

You are investigating a Linux workstation because its performance has suddenly become very slow.

During your investigation, you notice:

- One unfamiliar process is consuming a large amount of CPU.
- The process has an unexpected parent process.
- The process appears to be running under an ordinary user account.
- You do not know whether it is legitimate or malicious.

### Questions

**1. Initial Investigation – 3 Marks**

Which Linux commands would you use to inspect the process?

Provide at least three commands.

**2. Process Relationships – 3 Marks**

Why is examining the PID and PPID important during this investigation?

**3. Resource Investigation – 3 Marks**

How would you determine whether the process is responsible for excessive CPU or memory usage?

**4. Evidence Preservation – 3 Marks**

Why should you investigate and document the process before terminating it?

**5. Incident Response – 3 Marks**

What would you do if the process were confirmed to be malicious?

Mention at least three responsible actions.

---

## 3. Final Practical Checklist

Before submitting your assessment, verify that you have completed the following:

- [ ] Understood Linux processes and process identifiers.
- [ ] Used `ps` to inspect processes.
- [ ] Used `top` to monitor system activity.
- [ ] Identified common process states.
- [ ] Practised foreground and background execution.
- [ ] Used `jobs`, `fg`, and `bg`.
- [ ] Understood Linux signals.
- [ ] Used `kill` and `pgrep` safely.
- [ ] Understood nice values and process priority.
- [ ] Inspected daemons and services.
- [ ] Practised service management with `systemctl`.
- [ ] Used monitoring commands to investigate system performance.
- [ ] Completed the cybersecurity scenario.

---

## 4. Grading Guide

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Needs Improvement |

**Pass mark: 50%**

A score of 80% or above is a strong target.

---

## 5. Completion Statement

After completing this module, you should be able to explain how Linux manages processes, inspect running applications, control process execution, manage services, and investigate common performance problems.

These skills form an important foundation for Linux system administration, cybersecurity, and penetration testing.

**END OF MODULE 08 – PROCESS MANAGEMENT**

**NEXT MODULE: MODULE 09 – LINUX PACKAGE MANAGEMENT**