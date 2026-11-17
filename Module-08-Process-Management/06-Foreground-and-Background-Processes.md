---
---

# MODULE 08: PROCESS MANAGEMENT

## LESSON 06: FOREGROUND AND BACKGROUND PROCESSES

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain foreground processes.
2. Explain background processes.
3. Understand how terminal control works.
4. Run commands in the background using `&`.
5. Suspend foreground processes using `Ctrl+Z`.
6. Understand the difference between suspending and terminating a process.
7. Identify foreground and background processes using Linux commands.

---

### 2. What Is a Foreground Process?

A **foreground process** is a process that runs directly under the control of your current terminal session.

When you start a foreground command, the terminal normally waits for that command to finish before accepting another command.

Example:

```bash
sleep 60
```

After executing this command:

- The terminal becomes occupied by the sleep command.
- You cannot enter another shell command in that same terminal until it finishes or is interrupted.
- You can use `Ctrl+C` to interrupt it.

Foreground processes are useful when you want to interact directly with a command or observe its output.

### 3. What Is a Background Process?

A **background process** runs without occupying the terminal in the same way as a foreground process.

This allows you to continue using the shell while the command runs.

Linux provides a simple way to start a command in the background: append `&` to the command.

Example:

```bash
sleep 60 &
```

The shell starts the command as a background job and normally displays information similar to:

```text
[1] 2500
```

Here:

- `1` is the job number.
- `2500` is an example PID.

Your actual output will differ.

You can immediately enter another command.

### 4. Foreground vs Background Processes

| Foreground Process | Background Process |
|---|---|
| Occupies the terminal while running | Allows the shell to accept more commands |
| Usually receives terminal input | Normally does not receive interactive terminal input |
| Can be interrupted using Ctrl+C | Can be managed through shell job control |
| Runs under terminal control | Runs without occupying the foreground |

**Important:** Background does not mean the process is independent of the terminal or guaranteed to survive logout. A normal background job may still be affected when its shell exits.

### 5. Running a Command in the Background

The `&` symbol tells the shell to start a command asynchronously.

Syntax:

```bash
command &
```

Example:

```bash
sleep 120 &
```

You should receive a job number and PID.

Now run:

```bash
echo "I can continue using my terminal."
```

The message should appear while the sleep process continues running.

### 6. Understanding Job Numbers and PIDs

When you start a background process, the shell may display:

```text
[1] 2500
```

These numbers have different meanings.

| Number | Meaning |
|---|---|
| 1 | Shell job number |
| 2500 | Process ID |

A job number is used by the shell's job-control commands.

A PID identifies the process within Linux.

They are not interchangeable.

For example:

```bash
jobs
```

displays shell jobs.

Whereas:

```bash
pgrep -a sleep
```

helps locate processes by name.

### 7. Viewing Background Jobs with jobs

The `jobs` command displays jobs associated with your current shell.

Example:

```bash
sleep 120 &
```

Then run:

```bash
jobs
```

Possible output:

```text
[1]+ Running    sleep 120 &
```

The exact output may differ.

Common job states include:

- Running
- Stopped
- Done

**Remember:** `jobs` normally displays jobs managed by the current shell, not every background process running on the system.

### 8. Suspending a Foreground Process with Ctrl+Z

You can temporarily suspend a foreground process by pressing:

```text
Ctrl + Z
```

Example:

```bash
sleep 300
```

While it is running, press `Ctrl+Z`.

The shell should display a message similar to:

```text
[1]+ Stopped    sleep 300
```

The process has been suspended, not terminated.

You can inspect it using:

```bash
jobs
```

Or:

```bash
ps -eo pid,ppid,stat,comm
```

A stopped process commonly appears with a `T` state.

### 9. Understanding the Difference Between Ctrl+C and Ctrl+Z

These keyboard shortcuts perform different operations.

| Shortcut | Function |
|---|---|
| Ctrl+C | Normally sends SIGINT to interrupt the foreground process group |
| Ctrl+Z | Normally sends SIGTSTP to suspend the foreground process group |

**Ctrl+C:** Requests interruption.

**Ctrl+Z:** Temporarily stops execution so you can manage the job.

This distinction is important when working with long-running commands.

### 10. Important Background Execution Examples

#### Example 1: Run sleep in the background

```bash
sleep 120 &
```

#### Example 2: Run a command and display its PID

```bash
sleep 120 & echo $!
```

The special variable `$!` expands to the PID of the most recently started background job in that shell.

#### Example 3: Display all current shell jobs

```bash
jobs
```

#### Example 4: Display jobs with their PIDs

```bash
jobs -l
```

#### Example 5: Display running processes

```bash
ps aux
```

### 11. Practical Laboratory: Working with Foreground and Background Processes

**Objective:** Learn how to start, inspect, suspend, and resume processes.

#### Step 1: Create your practice directory

```bash
mkdir -p ~/module08-lab/foreground-background
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/foreground-background
```

#### Step 3: Start a foreground process

```bash
sleep 120
```

Observe that the terminal is occupied.

Press `Ctrl+C` to interrupt it.

#### Step 4: Start a background process

```bash
sleep 180 &
```

Observe the job number and PID displayed by the shell.

#### Step 5: Confirm that the shell is available

```bash
echo "Terminal is available."
```

The message should appear even though sleep is still running.

#### Step 6: View your jobs

```bash
jobs
```

You should see the sleep job.

#### Step 7: View job details

```bash
jobs -l
```

Observe the job number and PID.

#### Step 8: Suspend a foreground process

Start another process:

```bash
sleep 300
```

Press `Ctrl+Z`.

Now execute:

```bash
jobs
```

The process should appear as stopped.

#### Step 9: Resume the stopped process in the background

Run:

```bash
bg
```

This resumes the current stopped job in the background.

Check:

```bash
jobs
```

It should now show the job as running.

#### Step 10: Return a background job to the foreground

Run:

```bash
fg
```

The current job should return to the foreground.

Press `Ctrl+C` to terminate it.

#### Step 11: Inspect the remaining jobs

```bash
jobs
```

You may still have the first sleep process running.

#### Step 12: Finish the laboratory

Allow the first `sleep 180` process to finish, or manage it using the shell job-control commands covered in the next lesson.

### 12. Understanding Terminal Job Control

The shell manages jobs started from that shell.

It tracks:

- Job numbers.
- Job states.
- Foreground jobs.
- Background jobs.
- Suspended jobs.

The shell provides commands such as:

| Command | Purpose |
|---|---|
| `jobs` | Lists current shell jobs |
| `jobs -l` | Lists jobs with process IDs |
| `bg` | Resumes a stopped job in the background |
| `fg` | Brings a job to the foreground |
| `command &` | Starts a command in the background |

We will study `jobs`, `bg`, and `fg` more deeply in the next lesson.

### 13. Cybersecurity Applications

Understanding foreground and background execution is useful in cybersecurity.

**1. Identifying unexpected background activity**

Administrators can investigate processes that continue running while no visible terminal interaction is taking place.

**2. Understanding long-running services**

Many legitimate system services operate in the background.

**3. Investigating suspicious execution**

Security analysts may inspect commands that launch unexpected background processes.

**4. System administration**

Background execution allows administrators to run long tasks without keeping the terminal occupied.

**5. Incident response**

Understanding process execution helps analysts reconstruct how applications were launched and whether they remained active.

Remember: a background process is not automatically suspicious. Many normal Linux services and applications run without occupying a terminal.

### 14. Common Mistakes

**Mistake 1: Confusing job numbers with PIDs**

`[1]` is a job number, while the number following it is commonly the PID.

**Mistake 2: Thinking Ctrl+Z terminates a process**

Ctrl+Z normally suspends the foreground job.

**Mistake 3: Thinking Ctrl+C and Ctrl+Z do the same thing**

Ctrl+C requests interruption; Ctrl+Z normally suspends the job.

**Mistake 4: Assuming jobs displays every system process**

`jobs` displays jobs associated with the current shell.

**Mistake 5: Assuming background processes always survive logout**

Ordinary background jobs may be affected when their controlling shell exits.

### 15. Lesson Summary

In this lesson, we learned:

1. Foreground processes occupy the terminal while running.
2. Background processes allow the shell to accept additional commands.
3. `&` starts a command in the background.
4. `jobs` lists jobs associated with the current shell.
5. `jobs -l` displays job details, including PIDs.
6. `Ctrl+Z` suspends a foreground job.
7. `Ctrl+C` normally interrupts the foreground process group.
8. `bg` resumes a stopped job in the background.
9. `fg` brings a job into the foreground.
10. Background execution is useful for system administration and monitoring.

### 16. Knowledge Check

Answer these questions in your own words.

1. What is a foreground process?
2. What is a background process?
3. Which symbol starts a command in the background?
4. What is the difference between a job number and a PID?
5. What does the `jobs` command do?
6. What is the purpose of `$!`?
7. What happens when you press Ctrl+Z?
8. What is the difference between Ctrl+C and Ctrl+Z?
9. What does the `bg` command do?
10. What does the `fg` command do?

### 17. Practical Assignment

Complete these tasks in your Kali Linux virtual machine:

- [ ] Create `~/module08-lab/foreground-background`.
- [ ] Start `sleep 120` in the foreground.
- [ ] Interrupt it using Ctrl+C.
- [ ] Start `sleep 180` in the background using `&`.
- [ ] Display a message in the same terminal.
- [ ] Inspect jobs using `jobs`.
- [ ] Inspect job PIDs using `jobs -l`.
- [ ] Start `sleep 300` in the foreground.
- [ ] Suspend it using Ctrl+Z.
- [ ] Resume it in the background using `bg`.
- [ ] Bring it to the foreground using `fg`.
- [ ] Interrupt it using Ctrl+C.
- [ ] Inspect the remaining jobs.
- [ ] Complete the 10 knowledge-check questions.

**End of Lesson 06**

**Next Lesson:** Managing Processes with `jobs`, `fg`, and `bg`.