---
---

# MODULE 08: PROCESS MANAGEMENT

## LESSON 05: UNDERSTANDING LINUX PROCESS STATES

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define a process state.
2. Explain the major Linux process states.
3. Understand process-state codes displayed by `ps`.
4. Identify running, sleeping, stopped, and zombie processes.
5. Understand the difference between interruptible and uninterruptible sleep.
6. Monitor process states using Linux commands.
7. Explain why process states matter in system administration and cybersecurity.

---

### 2. What Is a Process State?

A **process state** describes the current condition of a process during its lifecycle.

A process does not continuously execute instructions.

Sometimes it is using the CPU. At other times, it waits for input, waits for a resource, or stops executing.

Linux tracks these conditions so the kernel can manage processes efficiently.

For example:

- A process calculating data may be running.
- A process waiting for keyboard input may be sleeping.
- A process paused by a stop signal may be stopped.
- A process that has finished but has not yet been collected by its parent may be a zombie.

### 3. Major Linux Process States

Linux uses several process states.

The most important states to understand initially are:

| State | Code | Meaning |
|---|---|---|
| Running or runnable | R | Executing or ready to execute |
| Interruptible sleep | S | Waiting for an event and can be awakened by signals |
| Uninterruptible sleep | D | Waiting for certain operations, commonly I/O |
| Stopped | T | Execution has been stopped |
| Zombie | Z | Terminated, but its exit status has not yet been collected by its parent |
| Idle | I | Idle kernel thread |

These codes are commonly visible in the `STAT` column of `ps`.

### 4. Running or Runnable State (R)

The `R` state represents a process that is:

- Currently executing on a CPU, or
- Ready to execute and waiting for CPU time.

Example:

```bash
yes
```

This command continuously produces output and can consume CPU resources.

You can inspect it from another terminal.

However, it may produce a large amount of output, so a safer demonstration is:

```bash
yes > /dev/null
```

This runs continuously without filling the terminal with output.

Stop it with `Ctrl+C`.

### 5. Interruptible Sleep State (S)

The `S` state means a process is sleeping while waiting for an event.

This is a very common process state.

Examples include:

- A process waiting for keyboard input.
- An application waiting for data.
- A process waiting for a timer.
- A service waiting for an event.

Example:

```bash
sleep 300
```

The sleep process waits for the specified time.

It will commonly appear with an `S` state.

You can interrupt it using `Ctrl+C` when it is running in the foreground.

### 6. Uninterruptible Sleep State (D)

The `D` state represents a process waiting in an uninterruptible kernel sleep.

It commonly occurs while a process waits for certain kernel operations, especially I/O operations.

For example, a process may be waiting for a storage device to complete an operation.

Important characteristics:

- The process is not executing normally.
- It is waiting for a kernel-level event.
- Ordinary signals do not immediately interrupt this state.
- It usually returns to another state after the awaited operation completes.

**Important:** A process in state `D` is not automatically malicious. It may indicate slow storage, hardware problems, or other I/O delays.

Do not attempt to manufacture a D-state process on your virtual machine. We will identify the state conceptually and through existing system processes if present.

### 7. Stopped State (T)

A process in the `T` state has been stopped.

For example, pressing `Ctrl+Z` while a foreground process is running normally suspends it through the terminal's job-control mechanism.

The process remains present but is no longer executing normally.

Example:

```bash
sleep 300
```

Press:

```text
Ctrl + Z
```

The shell reports that the job has been stopped.

You can inspect it with:

```bash
ps -o pid,ppid,stat,comm
```

Later lessons will explain foreground and background jobs in greater detail.

### 8. Zombie State (Z)

A zombie process is a process that has finished executing but whose parent has not yet collected its exit status.

The process is no longer executing.

However, a small amount of process information remains in the kernel's process table until the parent collects that status.

A zombie process may appear with a `Z` in the `STAT` column.

Example process relationship:

```text
Parent Process
      |
      v
Zombie Child (Z)
```

A zombie process:

- Has already terminated.
- Does not execute instructions.
- Does not consume normal running-process CPU time.
- Retains a process-table entry until it is reaped.

A large number of persistent zombies can indicate a problem with an application's process management.

**Important:** A zombie is not the same as a process that is still running in the background.

### 9. Idle State (I)

The `I` state represents an idle kernel thread.

Kernel threads perform internal operating-system tasks.

This state is less important for ordinary application monitoring, but you may encounter it when examining detailed process listings.

### 10. Understanding STAT Modifiers

The `STAT` column can contain additional characters besides the main process-state code.

For example:

| Character | Meaning |
|---|---|
| `<` | High-priority process |
| `N` | Low-priority process |
| `L` | Pages locked into memory |
| `s` | Session leader |
| `l` | Multithreaded process |
| `+` | Process is in the foreground process group |

Example:

```text
STAT
S
R
Ss
Sl
T
Z
```

Interpretation:

- `S`: Interruptible sleep.
- `R`: Running or runnable.
- `Ss`: Sleeping session leader.
- `Sl`: Sleeping multithreaded process.
- `T`: Stopped.
- `Z`: Zombie.

The first character normally represents the main process state.

### 11. Viewing Process States with ps

To display process states:

```bash
ps -eo pid,ppid,stat,comm
```

Example output:

```text
PID   PPID  STAT  COMMAND
1     0     Ss    systemd
1500  1400  Ss    bash
1650  1500  S     sleep
```

Your results will differ.

To find processes in a particular state, you can use `ps` with `awk`.

For example, to display processes whose main state is sleeping:

```bash
ps -eo pid,stat,comm | awk '$2 ~ /^S/ {print}'
```

To display processes whose main state is running or runnable:

```bash
ps -eo pid,stat,comm | awk '$2 ~ /^R/ {print}'
```

To search for zombie processes:

```bash
ps -eo pid,ppid,stat,comm | awk '$3 ~ /^Z/ {print}'
```

If no zombie processes appear, that is perfectly normal.

### 12. Practical Laboratory: Exploring Process States

**Objective:** Observe different process states safely.

#### Step 1: Create your practice directory

```bash
mkdir -p ~/module08-lab/process-states
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/process-states
```

#### Step 3: View existing process states

```bash
ps -eo pid,ppid,stat,comm
```

Identify some processes in the `S` and `R` states if present.

#### Step 4: Create a sleeping process

Run:

```bash
sleep 300
```

Leave it running.

Open another terminal and execute:

```bash
ps -eo pid,ppid,stat,comm | grep '[s]leep'
```

Observe its process state.

It will commonly appear as `S`.

#### Step 5: Stop the sleeping process temporarily

Return to the first terminal and press:

```text
Ctrl + Z
```

The shell should report that the job has been stopped.

#### Step 6: Inspect the stopped process

In either terminal, run:

```bash
ps -eo pid,ppid,stat,comm | grep '[s]leep'
```

Observe the state.

It should normally begin with `T`.

#### Step 7: Resume the process

Return to the first terminal and execute:

```bash
fg
```

The process should return to the foreground.

#### Step 8: Terminate the process

Press:

```text
Ctrl + C
```

The process should terminate.

#### Step 9: Check for zombie processes

Run:

```bash
ps -eo pid,ppid,stat,comm | awk '$3 ~ /^Z/ {print}'
```

You may see no output. This is normal.

#### Step 10: Observe process-state modifiers

Run:

```bash
ps -eo pid,ppid,stat,comm
```

Look for examples such as `Ss`, `Sl`, or `S`.

Write down any different state codes you find.

### 13. Process States in Cybersecurity

Understanding process states helps security professionals investigate system behavior.

**1. Identifying resource problems**

A process consuming CPU continuously may be running or runnable for extended periods.

**2. Investigating unresponsive applications**

A process waiting for I/O may appear in an uninterruptible sleep state.

**3. Detecting application-management problems**

Persistent zombie processes can indicate that a parent application is failing to collect child-process exit statuses.

**4. Investigating suspicious activity**

Unexpected stopped or running processes may deserve investigation in the context of other system evidence.

**5. Understanding system health**

A process-state overview can help administrators identify unusual system conditions.

Remember: no individual process state proves that malware is present. Always investigate the process's identity, command, parent, resource usage, and behavior.

### 14. Common Mistakes

**Mistake 1: Assuming S means a process is broken**

Sleeping is normal and common.

**Mistake 2: Confusing D with Z**

- `D` means uninterruptible sleep.
- `Z` means the process has terminated but has not yet been reaped.

**Mistake 3: Thinking a stopped process has terminated**

A stopped process still exists. It can be resumed.

**Mistake 4: Assuming every R process is consuming a full CPU core**

A runnable process may be waiting for CPU time rather than actively executing.

**Mistake 5: Assuming zombie processes are actively running**

A zombie has already terminated and is waiting for its parent to collect its exit status.

### 15. Lesson Summary

In this lesson, we learned:

1. A process state describes the current condition of a process.
2. `R` represents running or runnable.
3. `S` represents interruptible sleep.
4. `D` represents uninterruptible sleep.
5. `T` represents stopped.
6. `Z` represents zombie.
7. `I` represents an idle kernel thread.
8. The `STAT` column displays process states and modifiers.
9. The `ps` command helps us inspect process states.
10. Process-state analysis supports system troubleshooting and cybersecurity investigations.

### 16. Knowledge Check

Answer these questions in your own words.

1. What is a process state?
2. What does the `R` state represent?
3. What does the `S` state mean?
4. Explain the difference between `S` and `D`.
5. What happens when a process enters the `T` state?
6. What is a zombie process?
7. Why does a zombie process remain in the process table?
8. What does the `I` state represent?
9. Which `ps` column displays process-state information?
10. Why should a security analyst understand process states?

### 17. Practical Assignment

Complete these tasks in your Kali Linux virtual machine:

- [ ] Create `~/module08-lab/process-states`.
- [ ] Run `ps -eo pid,ppid,stat,comm`.
- [ ] Identify processes in the `S` and `R` states where present.
- [ ] Start `sleep 300`.
- [ ] Observe its state from another terminal.
- [ ] Press `Ctrl+Z` and inspect its stopped state.
- [ ] Resume it using `fg`.
- [ ] Terminate it using `Ctrl+C`.
- [ ] Search for zombie processes.
- [ ] Observe the `STAT` modifiers.
- [ ] Complete the 10 knowledge-check questions.

**End of Lesson 05**

**Next Lesson:** Foreground and Background Processes.