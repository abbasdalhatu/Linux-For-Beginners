---
---

# MODULE 08: PROCESS MANAGEMENT

## LESSON 11: MANAGING PROCESSES WITH NICE AND RENICE

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain the purpose of `nice` and `renice`.
2. Start processes with a selected nice value.
3. Change the nice value of an existing process.
4. Inspect process niceness using `ps`.
5. Understand Linux permissions when changing nice values.
6. Compare process scheduling behavior.
7. Apply safe priority management techniques.

---

### 2. Introduction

In the previous lesson, we learned that Linux uses niceness to influence the scheduling weight of normal processes.

We also learned two important facts:

- A lower nice value generally gives a process higher scheduling weight.
- A higher nice value generally gives a process lower scheduling weight.

Now we will learn two commands:

| Command | Purpose |
|---|---|
| `nice` | Starts a new command with a selected nice value |
| `renice` | Changes the nice value of an existing process |

The main difference is that `nice` is used when starting a command, while `renice` is used after a process already exists.

### 3. Understanding the nice Command

The `nice` command starts a program with a specified nice value.

Syntax:

```bash
nice -n VALUE COMMAND
```

Example:

```bash
nice -n 10 sleep 300 &
```

This starts a sleep process with a nice value of 10.

Inspect it:

```bash
pgrep -a sleep
```

Then:

```bash
ps -o pid,ni,pri,comm -p PID
```

Replace `PID` with the actual process ID.

You should see the process's nice value in the `NI` column.

### 4. Understanding Default Niceness

When you start a normal process without changing its niceness, it generally inherits its parent's nice value.

A typical interactive shell has a nice value of 0.

Check your current shell:

```bash
ps -o pid,ni,comm -p $$
```

Start a normal process:

```bash
sleep 300 &
```

Inspect the process:

```bash
pgrep -a sleep
```

Then check its nice value:

```bash
ps -o pid,ni,comm -p PID
```

If your shell has nice value 0, the process will normally inherit that value.

### 5. Understanding the renice Command

The `renice` command changes the nice value of an existing process.

Syntax:

```bash
renice VALUE -p PID
```

Example:

```bash
renice 10 -p 2500
```

This requests a nice value of 10 for the process with PID 2500.

Inspect the result:

```bash
ps -o pid,ni,pri,comm -p 2500
```

**Important:** Replace example PIDs with actual PIDs from your own laboratory.

### 6. Changing Niceness Step by Step

Suppose a process currently has a nice value of 0.

You want to lower its scheduling weight by changing its nice value to 10.

Step 1: Start a test process.

```bash
sleep 300 &
```

Step 2: Find its PID.

```bash
pgrep -a sleep
```

Step 3: Inspect its current nice value.

```bash
ps -o pid,ni,comm -p PID
```

Step 4: Change its nice value.

```bash
renice 10 -p PID
```

Step 5: Verify the change.

```bash
ps -o pid,ni,comm -p PID
```

The `NI` column should now show 10 if the operation succeeded.

### 7. Understanding Permission Requirements

Linux restricts changes to process niceness.

Generally:

- A process owner can increase the nice value of their own process.
- Decreasing the nice value, which increases scheduling weight, normally requires additional privilege.
- Root or an appropriately privileged administrator can make permitted priority adjustments.

For example:

```bash
renice 15 -p 2500
```

Increasing the nice value is normally permitted for the process owner.

However:

```bash
renice -5 -p 2500
```

requests a lower nice value and generally requires additional privileges.

Do not use administrative privileges simply to experiment on unfamiliar processes.

### 8. Understanding renice Options

Some useful options include:

| Option | Purpose |
|---|---|
| `-p` | Target a process ID |
| `-g` | Target a process group |
| `-u` | Target processes belonging to a user |

Examples:

Change one process:

```bash
renice 10 -p 2500
```

Change processes in a process group:

```bash
renice 10 -g 3000
```

Change processes belonging to a user:

```bash
renice 10 -u username
```

Be careful with group and user options because they may affect multiple processes.

For this course, practise with individual PIDs only.

### 9. Comparing nice and renice

| Feature | nice | renice |
|---|---|---|
| Main purpose | Starts a command with a selected nice value | Changes an existing process's nice value |
| Target | New command | Existing process |
| Typical syntax | `nice -n 10 command` | `renice 10 -p PID` |
| Useful when | Launching background work | Adjusting a running task |

Example using `nice`:

```bash
nice -n 10 sleep 300 &
```

Example using `renice`:

```bash
renice 15 -p PID
```

### 10. Monitoring Changes

After changing a nice value, verify the result.

Using `ps`:

```bash
ps -eo pid,ni,pri,comm
```

To inspect one process:

```bash
ps -o pid,ni,pri,comm -p PID
```

Using `top`:

```bash
top
```

Look at the `NI` column.

Press `q` to exit.

### 11. Understanding Scheduling Limitations

Changing niceness does not guarantee that a process will execute immediately or receive a fixed percentage of CPU time.

Actual scheduling behavior depends on:

- The scheduling class.
- Other runnable tasks.
- CPU availability.
- CPU affinity and system configuration.
- Kernel scheduling decisions.

Niceness is an influence on scheduling weight, not a guarantee of execution order.

### 12. Practical Laboratory: Using nice and renice

**Objective:** Start a process with a selected nice value and modify its value while running.

#### Step 1: Create your laboratory directory

```bash
mkdir -p ~/module08-lab/nice-renice
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/nice-renice
```

#### Step 3: Start a process with default niceness

```bash
sleep 300 &
```

#### Step 4: Find its PID

```bash
pgrep -a sleep
```

Record its PID as `PID1`.

#### Step 5: Inspect its current nice value

```bash
ps -o pid,ni,pri,comm -p PID1
```

#### Step 6: Start another process with nice value 10

```bash
nice -n 10 sleep 300 &
```

Find its PID and record it as `PID2`.

#### Step 7: Inspect both processes

```bash
ps -o pid,ni,pri,comm -p PID1,PID2
```

Compare their nice values.

#### Step 8: Change the first process's nice value

```bash
renice 10 -p PID1
```

#### Step 9: Verify the change

```bash
ps -o pid,ni,pri,comm -p PID1
```

The first process should now have a nice value of 10.

#### Step 10: Change the second process's nice value

```bash
renice 15 -p PID2
```

#### Step 11: Verify both processes

```bash
ps -o pid,ni,pri,comm -p PID1,PID2
```

You should now see the two test processes with nice values of 10 and 15.

#### Step 12: Clean up

Terminate only the two processes you created:

```bash
kill -TERM PID1 PID2
```

Verify:

```bash
ps -p PID1,PID2
```

If the processes have exited, they should no longer appear as running processes.

### 13. Common Mistakes

**Mistake 1: Using nice to change an existing process**

Use `renice` for an existing process.

**Mistake 2: Using renice before identifying the PID**

Always inspect the process first.

**Mistake 3: Assuming every user can lower a nice value**

Lowering the nice value generally requires additional privileges.

**Mistake 4: Confusing priority with niceness**

They are related but are not the same thing.

**Mistake 5: Changing multiple processes unintentionally**

Use individual PIDs during practice.

### 14. Cybersecurity and Administration Applications

**1. Resource management**

Administrators can reduce the scheduling weight of CPU-intensive background tasks.

**2. Security monitoring**

Process priority information can help explain system resource behavior.

**3. System performance**

Changing niceness can help keep interactive applications responsive during heavy background work.

**4. Controlled testing**

Developers can compare process behavior under different nice values.

**5. Incident investigation**

Understanding process scheduling helps administrators investigate unusual resource usage.

### 15. Lesson Summary

In this lesson, we learned:

1. `nice` starts a command with a selected nice value.
2. `renice` changes the nice value of an existing process.
3. Normal nice values range from -20 to +19.
4. Higher nice values generally mean lower scheduling weight.
5. Changing niceness is subject to Linux permissions.
6. `renice -p` targets an individual PID.
7. `ps` displays process niceness.
8. `top` provides real-time monitoring.
9. Niceness influences scheduling but does not guarantee CPU time.
10. Always identify the correct process before changing its scheduling settings.

### 16. Knowledge Check

Answer these questions in your own words.

1. What is the purpose of the `nice` command?
2. What is the purpose of the `renice` command?
3. What is the main difference between nice and renice?
4. What does `renice 10 -p 2500` do?
5. Why do you need a PID when using `renice -p`?
6. What is the usual nice-value range?
7. Why does lowering a nice value normally require additional privileges?
8. Which command can display the NI column?
9. Does increasing niceness guarantee that other processes will always run first?
10. Why should administrators avoid changing multiple processes unnecessarily?

### 17. Practical Assignment

- [ ] Create `~/module08-lab/nice-renice`.
- [ ] Start a normal sleep process.
- [ ] Identify its PID.
- [ ] Inspect its nice value.
- [ ] Start another sleep process using `nice -n 10`.
- [ ] Compare both processes using `ps`.
- [ ] Use `renice` to change the first process's nice value to 10.
- [ ] Change the second process's nice value to 15.
- [ ] Verify both changes.
- [ ] Terminate only your test processes using SIGTERM.
- [ ] Verify that both processes have exited.
- [ ] Complete all 10 knowledge-check questions.

**End of Lesson 11**

**Next Lesson:** Understanding Daemons and Services.