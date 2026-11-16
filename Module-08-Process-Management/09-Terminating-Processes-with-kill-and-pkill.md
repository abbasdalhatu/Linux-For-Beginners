# MODULE 08: PROCESS MANAGEMENT

## LESSON 09: TERMINATING PROCESSES WITH KILL AND PKILL

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain process termination.
2. Understand the difference between `kill` and `pkill`.
3. Terminate processes using their PIDs.
4. Terminate processes using their names.
5. Understand graceful and forced termination.
6. Use `pgrep` to identify processes before terminating them.
7. Apply safe process termination techniques.

---

### 2. What Is Process Termination?

Process termination is the act of ending a running process.

A process may terminate because:

- It completes its assigned task.
- A user interrupts it.
- The operating system terminates it.
- It receives a termination signal.
- It encounters a fatal error.

For example:

```bash
sleep 300
```

This command runs for 300 seconds.

You can allow it to finish naturally or terminate it before the time expires.

Linux provides commands such as `kill` and `pkill` to manage process termination.

### 3. Understanding the kill Command

The `kill` command sends a signal to a process or job.

Its most common use is requesting process termination.

Syntax:

```bash
kill [SIGNAL] PID
```

Example:

```bash
sleep 300 &
```

Find its PID:

```bash
pgrep -a sleep
```

Suppose the PID is `2500`.

Terminate it gracefully:

```bash
kill 2500
```

By default, `kill` sends SIGTERM.

You can also specify the signal:

```bash
kill -TERM 2500
```

### 4. Understanding Graceful Termination

Graceful termination means requesting a process to stop while giving it an opportunity to clean up.

SIGTERM is commonly used for this purpose.

A process may use the opportunity to:

- Save data.
- Close files.
- Release resources.
- Finish cleanup operations.

Example:

```bash
kill -TERM 2500
```

The process may terminate after handling the signal.

However, a process can catch, handle, or ignore SIGTERM. Therefore, sending SIGTERM does not guarantee immediate termination.

### 5. Understanding Forced Termination

Sometimes a process does not respond to SIGTERM.

Linux provides SIGKILL for forced termination.

Example:

```bash
kill -KILL 2500
```

Equivalent:

```bash
kill -9 2500
```

SIGKILL cannot be caught, blocked, or ignored by the target process.

**Important:** Use SIGKILL only when necessary. It does not give the process an opportunity to perform normal cleanup.

### 6. Understanding the pkill Command

The `pkill` command sends a signal to processes selected by name or other matching criteria.

Unlike `kill`, which commonly targets a specific PID, `pkill` can target processes by their names.

Syntax:

```bash
pkill [OPTIONS] PROCESS_NAME
```

Example:

```bash
pkill sleep
```

This sends the default SIGTERM signal to processes whose names match `sleep`.

**Warning:** This can affect multiple matching processes, not just one. Do not run it blindly on a system with important running processes.

### 7. Understanding pgrep Before pkill

Before using `pkill`, use `pgrep` to inspect matching processes.

Example:

```bash
pgrep -a sleep
```

Possible output:

```text
2500 sleep 300
2510 sleep 400
```

This helps you identify the processes that match your search.

You can then decide whether to terminate a particular PID with `kill` or use a carefully scoped `pkill` command.

### 8. Useful pkill Options

| Option | Purpose |
|---|---|
| `-f` | Match against the full command line |
| `-x` | Require an exact process-name match |
| `-u USER` | Match processes owned by a user |
| `-TERM` | Send SIGTERM |
| `-KILL` | Send SIGKILL |
| `-l` | List signal names (availability can vary by implementation) |

Examples:

Exact process-name match:

```bash
pkill -x sleep
```

Send SIGTERM:

```bash
pkill -TERM -x sleep
```

Match a complete command line:

```bash
pkill -f "sleep 300"
```

Be careful with `-f`: it matches command-line text and can select more than one process if the pattern is not specific enough.

### 9. Difference Between kill and pkill

| Feature | kill | pkill |
|---|---|---|
| Main selection method | PID or job specification | Process-name or pattern matching |
| Target selection | Usually specific | Can match multiple processes |
| Default signal | SIGTERM | SIGTERM |
| Useful for | Controlling a known process | Selecting processes by name |
| Risk | Wrong PID | Overly broad match |

Example using `kill`:

```bash
kill -TERM 2500
```

Example using `pkill`:

```bash
pkill -TERM -x sleep
```

The second command may affect every matching `sleep` process that you are permitted to signal.

### 10. Checking Whether a Process Has Terminated

After sending a termination signal, verify the result.

Using `pgrep`:

```bash
pgrep -a sleep
```

Using `ps`:

```bash
ps -p 2500
```

If the process has exited, it should no longer appear as a running process.

A process may briefly remain visible as a zombie until its parent collects its exit status.

### 11. Understanding Permissions

Linux does not allow every user to terminate every process.

Generally:

- A user can signal processes they own.
- Root can signal processes across users, subject to kernel restrictions.
- Security policies and namespaces may impose additional restrictions.

You may see an error such as:

```text
Operation not permitted
```

This means the operation was not authorized.

Do not automatically solve this by using `sudo`. First identify the process and determine whether you are authorized to manage it.

### 12. Practical Laboratory: Safe Process Termination

**Objective:** Practise terminating only the harmless processes you create.

#### Step 1: Create your laboratory directory

```bash
mkdir -p ~/module08-lab/termination
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/termination
```

#### Step 3: Start two test processes

```bash
sleep 300 &
sleep 400 &
```

#### Step 4: Identify the processes

```bash
pgrep -a sleep
```

Record both PIDs.

#### Step 5: Inspect their details

```bash
ps -eo pid,ppid,stat,comm | grep sleep
```

#### Step 6: Terminate the first process using kill

Replace `PID1` with the first PID:

```bash
kill -TERM PID1
```

Verify:

```bash
ps -p PID1
```

#### Step 7: Inspect the remaining test process

```bash
pgrep -a sleep
```

Confirm that your second test process remains.

#### Step 8: Terminate the second process using pkill

Because the lab contains only the second test process at this point, run:

```bash
pkill -TERM -x sleep
```

#### Step 9: Verify cleanup

```bash
pgrep -a sleep
```

No output should appear if no other matching sleep processes exist.

If other sleep processes appear, inspect them rather than terminating them blindly.

### 13. Common Mistakes

**Mistake 1: Confusing kill with forced termination**

`kill` normally sends SIGTERM, not SIGKILL.

**Mistake 2: Using pkill without checking the target**

A broad pattern can terminate multiple processes.

**Mistake 3: Using SIGKILL unnecessarily**

Try graceful termination first.

**Mistake 4: Assuming a process has terminated without verification**

Always inspect the result using `ps` or `pgrep`.

**Mistake 5: Terminating system processes without authorization**

Some processes are essential to system operation. Never terminate unfamiliar system processes just to experiment.

### 14. Cybersecurity Applications

Process termination is useful in several security situations.

**1. Incident response**

An administrator may need to stop a confirmed malicious process.

**2. Malware investigation**

Analysts can control test processes in isolated environments.

**3. Resource management**

Unnecessary processes can consume CPU and memory.

**4. System administration**

Administrators may stop applications before performing maintenance.

**5. Security monitoring**

Unexpected processes can be investigated and, when authorized, stopped.

### 15. Lesson Summary

In this lesson, we learned:

1. Process termination means ending a running process.
2. `kill` commonly targets a process using its PID.
3. `pkill` selects processes using names or patterns.
4. SIGTERM requests graceful termination.
5. SIGKILL forces termination.
6. `pgrep` helps identify processes before termination.
7. `ps` helps verify process status.
8. Linux permissions control who can signal processes.
9. Broad process matching can be dangerous.
10. Always verify the target before terminating it.

### 16. Knowledge Check

Answer these questions in your own words.

1. What is process termination?
2. What is the default signal sent by `kill`?
3. What is the difference between SIGTERM and SIGKILL?
4. What is the purpose of `pkill`?
5. What is the difference between `kill` and `pkill`?
6. Why should you run `pgrep` before using `pkill`?
7. What does the `-x` option do?
8. What does the `-f` option do?
9. Why should you avoid terminating unfamiliar system processes?
10. How can you verify that a process has exited?

### 17. Practical Assignment

- [ ] Create `~/module08-lab/termination`.
- [ ] Start two harmless sleep processes.
- [ ] Identify them using `pgrep`.
- [ ] Inspect them using `ps`.
- [ ] Terminate one using `kill -TERM`.
- [ ] Verify its termination.
- [ ] Terminate the remaining test process using `pkill`.
- [ ] Verify that your test processes have exited.
- [ ] Explain the difference between `kill` and `pkill`.
- [ ] Complete all 10 knowledge-check questions.

**End of Lesson 09**

**Next Lesson:** Process Priority and Niceness.