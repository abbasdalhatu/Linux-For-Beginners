# MODULE 08: PROCESS MANAGEMENT

## LESSON 08: SENDING SIGNALS TO LINUX PROCESSES

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux signals.
2. Understand how signals communicate with processes.
3. Identify common Linux signals.
4. Use the `kill` command to send signals.
5. Understand the difference between SIGTERM and SIGKILL.
6. Inspect available signals using Linux commands.
7. Recognize how signals are used in system administration and cybersecurity.

---

### 2. What Are Linux Signals?

A **signal** is a notification sent to a process to inform it that a particular event has occurred or that it should perform a specific action.

Linux uses signals as a communication mechanism between the operating system, processes, and users.

For example:

- A user presses `Ctrl+C`.
- A process is asked to terminate.
- A process is temporarily stopped.
- A process is instructed to reload its configuration.

Signals help the operating system manage processes.

### 3. How Signals Work

A simplified signal workflow:

1. A signal is generated.
2. The kernel identifies the target process.
3. The signal is delivered or becomes pending.
4. The process handles the signal according to its configured behavior.

A process can respond to a signal in different ways:

- Perform a default action.
- Execute a custom signal handler.
- Ignore the signal, where permitted.

Some signals cannot be caught, blocked, or ignored.

### 4. Common Linux Signals

Linux supports several signals. The following are especially important.

| Signal | Number (common Linux value) | Purpose |
|---|---:|---|
| SIGHUP | 1 | Traditionally indicates terminal hangup; sometimes used to request reload |
| SIGINT | 2 | Interrupts a process |
| SIGQUIT | 3 | Requests termination and may produce a core dump |
| SIGKILL | 9 | Forces immediate termination |
| SIGTERM | 15 | Requests graceful termination |
| SIGSTOP | 19 on x86/ARM Linux | Suspends a process |
| SIGTSTP | 20 on x86/ARM Linux | Requests terminal stop |
| SIGCONT | 18 on x86/ARM Linux | Resumes a stopped process |
| SIGCHLD | 17 on x86/ARM Linux | Notifies a parent about child-process state changes |

**Important:** Signal numbers can vary between Linux architectures. Prefer signal names when writing portable commands.

### 5. Understanding SIGINT

`SIGINT` is the interrupt signal.

When you press:

```text
Ctrl + C
```

in a terminal, the terminal normally sends SIGINT to the foreground process group.

Example:

```bash
sleep 300
```

Press `Ctrl+C`.

The sleep command should terminate.

This is useful when you want to interrupt a command that is running in the foreground.

### 6. Understanding SIGTERM

`SIGTERM` asks a process to terminate gracefully.

It gives the process an opportunity to:

- Save data.
- Close files.
- Release resources.
- Perform cleanup operations.

Example:

```bash
sleep 300 &
```

Find its PID:

```bash
pgrep -a sleep
```

Send SIGTERM:

```bash
kill -TERM PID
```

Replace `PID` with the actual process ID.

For example, if the PID is 2500:

```bash
kill -TERM 2500
```

Verify:

```bash
ps -p 2500
```

If the process has exited, it will no longer appear in the process listing.

### 7. Understanding SIGKILL

`SIGKILL` forces a process to terminate.

Unlike SIGTERM, SIGKILL cannot be caught or handled by the target process.

Example:

```bash
kill -KILL PID
```

Or:

```bash
kill -9 PID
```

**Important:** SIGKILL should not be your first choice.

It does not give the process an opportunity to clean up. Use SIGTERM first whenever practical.

A process in an uninterruptible kernel sleep state may not disappear immediately, even after SIGKILL is sent.

### 8. Understanding SIGSTOP and SIGCONT

SIGSTOP suspends a process.

Example:

```bash
sleep 300 &
```

Find its PID:

```bash
pgrep -a sleep
```

Suspend it:

```bash
kill -STOP PID
```

Inspect it:

```bash
ps -o pid,stat,comm -p PID
```

The process state should normally show `T` for stopped or traced.

Resume it:

```bash
kill -CONT PID
```

The process can continue running.

### 9. The kill Command

The `kill` command sends signals to processes or shell jobs.

Basic syntax:

```bash
kill SIGNAL PID
```

Examples:

```bash
kill -TERM 2500
kill -STOP 2500
kill -CONT 2500
kill -KILL 2500
```

You can also use signal names without the SIG prefix:

```bash
kill -TERM 2500
kill -INT 2500
```

To see supported signal names:

```bash
kill -l
```

The `kill` command is not limited to terminating processes. It sends signals.

### 10. Sending Signals by Name

You can use a signal name directly.

For example:

```bash
kill -SIGTERM 2500
```

This is equivalent to:

```bash
kill -TERM 2500
```

Both request graceful termination.

### 11. Sending Signals to Shell Jobs

Bash also allows you to target jobs using job specifications.

Example:

```bash
sleep 300 &
jobs
```

Send a termination signal to job 1:

```bash
kill -TERM %1
```

Check the job list:

```bash
jobs
```

Remember that `%1` is a shell job specification, while a number such as `2500` may identify a process ID.

### 12. Practical Laboratory: Working with Linux Signals

**Objective:** Practise sending signals to harmless test processes.

#### Step 1: Create a laboratory directory

```bash
mkdir -p ~/module08-lab/signals
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/signals
```

#### Step 3: Start a test process

```bash
sleep 300 &
```

#### Step 4: Find its PID

```bash
pgrep -a sleep
```

Record the PID belonging to the process you just started.

#### Step 5: Inspect the process

Replace `PID` with your actual process ID:

```bash
ps -o pid,ppid,stat,comm -p PID
```

#### Step 6: Suspend the process

```bash
kill -STOP PID
```

Inspect its state:

```bash
ps -o pid,stat,comm -p PID
```

#### Step 7: Resume the process

```bash
kill -CONT PID
```

Inspect it again:

```bash
ps -o pid,stat,comm -p PID
```

#### Step 8: Terminate it gracefully

```bash
kill -TERM PID
```

Verify:

```bash
ps -p PID
```

The process should have exited.

#### Step 9: Practise another test process

```bash
sleep 300 &
```

Find its PID:

```bash
pgrep -a sleep
```

Send SIGTERM to the new process using its actual PID.

**Safety note:** Only target the sleep process you created. Do not send signals to system services or unfamiliar processes during this exercise.

### 13. Common Mistakes

**Mistake 1: Assuming kill always means termination**

The command sends a signal. The signal determines the requested action.

**Mistake 2: Using SIGKILL too early**

Try SIGTERM first because it allows graceful cleanup.

**Mistake 3: Using the wrong PID**

Always verify the process ID before sending a signal.

**Mistake 4: Confusing SIGSTOP and SIGTERM**

- SIGSTOP suspends a process.
- SIGTERM requests termination.

**Mistake 5: Assuming every process can handle every signal**

SIGKILL and SIGSTOP cannot be caught or ignored by the target process.

### 14. Cybersecurity Applications

Signals are important in Linux security and administration.

**1. Incident response**

An administrator may need to stop a suspicious process while investigating an incident.

**2. Process management**

Signals allow administrators to request process termination or suspension.

**3. Service management**

Some services use SIGHUP or other signals to reload configuration.

**4. Malware analysis**

Analysts may observe how a program responds to signals in an isolated laboratory.

**5. System troubleshooting**

Signals can help control processes that are unresponsive or behaving unexpectedly.

Only investigate or control processes on systems you own or are authorized to administer.

### 15. Lesson Summary

In this lesson, we learned:

1. Signals are notifications delivered to processes.
2. SIGINT interrupts a foreground process.
3. SIGTERM requests graceful termination.
4. SIGKILL forces termination.
5. SIGSTOP suspends a process.
6. SIGCONT resumes a stopped process.
7. `kill` sends signals to processes or shell jobs.
8. `kill -l` lists available signals.
9. Signal names are generally more portable than numeric values.
10. Always verify the target PID before sending a signal.

### 16. Knowledge Check

Answer these questions in your own words.

1. What is a Linux signal?
2. What happens when you press Ctrl+C in a terminal?
3. What is the purpose of SIGTERM?
4. What is the purpose of SIGKILL?
5. Why should SIGTERM normally be tried before SIGKILL?
6. What does SIGSTOP do?
7. What does SIGCONT do?
8. What is the purpose of `kill -l`?
9. What is the difference between a PID and a job specification?
10. Why is sending signals to unfamiliar system processes risky?

### 17. Practical Assignment

- [ ] Create `~/module08-lab/signals`.
- [ ] Start a harmless sleep process.
- [ ] Find its PID using `pgrep`.
- [ ] Inspect its process state using `ps`.
- [ ] Suspend it using SIGSTOP.
- [ ] Confirm that it is stopped.
- [ ] Resume it using SIGCONT.
- [ ] Confirm that it is running again.
- [ ] Terminate it using SIGTERM.
- [ ] Verify that it has exited.
- [ ] Repeat the exercise with another sleep process.
- [ ] Complete all 10 knowledge-check questions.

**End of Lesson 08**

**Next Lesson:** Terminating Processes with `kill` and `pkill`.