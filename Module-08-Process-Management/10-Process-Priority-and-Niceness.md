---
---

# MODULE 08: PROCESS MANAGEMENT

## LESSON 10: PROCESS PRIORITY AND NICENESS

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain process priority.
2. Understand CPU scheduling.
3. Explain the Linux nice value.
4. Understand the relationship between nice values and process priority.
5. Inspect process priorities using `ps` and `top`.
6. Understand the difference between priority and niceness.
7. Recognize how process scheduling helps Linux manage resources.

---

### 2. What Is Process Priority?

Process priority determines how favorably a process is considered when Linux schedules CPU time.

A computer may run many processes simultaneously, but the CPU has limited processing capacity.

Linux uses a scheduler to decide which runnable process should execute.

For example, your Kali Linux VM might be running:

- A web browser.
- A terminal.
- A file manager.
- A background update.
- A security monitoring tool.

The scheduler manages access to CPU time so that processes can make progress.

**Important:** Priority influences scheduling, but it does not guarantee that a process will always run before another process.

### 3. Understanding CPU Scheduling

CPU scheduling is the process of selecting which runnable process or thread gets CPU time.

A simplified workflow:

1. Processes become ready to execute.
2. The scheduler considers runnable tasks.
3. A task receives CPU time.
4. The scheduler may select another task.
5. This continues while processes remain runnable.

Modern Linux uses the fair scheduling infrastructure, including EEVDF for normal scheduling tasks on current kernels.

The scheduler considers factors such as task scheduling class, weight, and eligibility.

Niceness influences the weight of normal scheduling tasks.

### 4. What Is Niceness?

**Niceness** is a value that influences the relative scheduling weight of a normal Linux process.

Its usual range is:

```text
-20 to +19
```

| Nice value | Meaning |
|---:|---|
| -20 | Highest scheduling weight among normal nice values |
| -10 | Higher scheduling weight |
| 0 | Default niceness |
| +10 | Lower scheduling weight |
| +19 | Lowest scheduling weight among normal nice values |

Remember:

- A lower nice value generally means a higher scheduling weight.
- A higher nice value generally means a lower scheduling weight.

### 5. Why Is It Called Niceness?

The term comes from the idea of a process being considerate toward other processes.

A process with a higher nice value is generally more willing to receive a smaller share of CPU time relative to competing normal tasks.

For example:

- A resource-intensive background task might use nice value `10`.
- An interactive application might remain at the default nice value `0`.

This can help the system remain responsive.

### 6. Understanding the Difference Between Priority and Niceness

Priority and niceness are related, but they are not identical.

| Priority | Niceness |
|---|---|
| Affects scheduling decisions | Influences scheduling weight for normal tasks |
| Managed by the Linux scheduler | Can be adjusted with `nice` and `renice` |
| Depends on scheduling class and other factors | Normally ranges from -20 to +19 |
| Not represented by one universal simple scale | Uses a defined nice-value scale |

Linux also supports real-time scheduling classes, which have different scheduling behavior and are not controlled by the normal nice-value scale in the same way.

### 7. Viewing Process Priority with ps

You can inspect process priority using `ps`.

Example:

```bash
ps -eo pid,ni,pri,comm
```

Meaning:

| Field | Meaning |
|---|---|
| PID | Process ID |
| NI | Nice value |
| PRI | Scheduler priority display |
| COMMAND | Command name |

Example output:

```text
PID   NI   PRI  COMMAND
1200   0    19  bash
1250   5    14  sleep
1300  10     9  python
```

These are illustrative values. Actual output depends on the kernel and scheduling class.

For normal Linux processes, the `PRI` field is commonly displayed as a derived priority value. Do not confuse it with the nice value.

### 8. Viewing Niceness with top

The `top` command provides real-time process information.

Run:

```bash
top
```

Look for columns such as:

- PID
- PR
- NI
- CPU%
- MEM%
- COMMAND

The `NI` column displays the nice value.

The `PR` column displays scheduling priority information.

Press:

```text
q
```

to exit `top`.

### 9. Understanding the nice Command

The `nice` command starts a new command with a specified niceness value.

Syntax:

```bash
nice -n VALUE COMMAND
```

Example:

```bash
nice -n 10 sleep 300
```

This starts the sleep command with a nice value of 10.

Check its niceness:

```bash
ps -eo pid,ni,pri,comm | grep sleep
```

Another example:

```bash
nice -n 15 python3 --version
```

This demonstrates starting a command with a higher nice value, although the command finishes quickly.

**Important:** Increasing niceness generally requires no special privilege. Decreasing niceness below the permitted level generally requires additional privileges.

### 10. Understanding Default Niceness

Most normal processes start with a nice value of 0 unless the launching environment or parent process establishes another value.

Check your current shell:

```bash
ps -o pid,ni,comm -p $$
```

The `NI` field shows the shell's nice value.

### 11. Why Adjust Process Niceness?

Adjusting niceness can help when several processes compete for CPU resources.

Examples:

**Background file processing**

A large file-processing task can run with a higher nice value so interactive work gets a more favorable scheduling weight.

**System maintenance**

A resource-intensive maintenance command may be assigned lower scheduling weight.

**Research and testing**

Developers can compare how different nice values influence CPU scheduling under controlled conditions.

### 12. Practical Laboratory: Inspecting Process Niceness

**Objective:** Observe nice values using harmless test processes.

#### Step 1: Create a laboratory directory

```bash
mkdir -p ~/module08-lab/niceness
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/niceness
```

#### Step 3: Start a default-nice process

```bash
sleep 300 &
```

#### Step 4: Start a process with higher niceness

```bash
nice -n 10 sleep 300 &
```

#### Step 5: Find the processes

```bash
pgrep -a sleep
```

Record their PIDs.

#### Step 6: Inspect their nice values

```bash
ps -eo pid,ni,pri,comm | grep sleep
```

You should find one process with nice value 0 and another with nice value 10, assuming your environment has no inherited scheduling adjustments.

#### Step 7: Inspect them using top

```bash
top
```

Find the two sleep processes and compare their `NI` values.

Press `q` to exit.

#### Step 8: Clean up your test processes

Identify the exact PIDs using:

```bash
pgrep -a sleep
```

Terminate only the two processes created in this laboratory:

```bash
kill -TERM PID1 PID2
```

Replace `PID1` and `PID2` with the actual PIDs you recorded.

Verify:

```bash
ps -p PID1,PID2
```

### 13. Common Mistakes

**Mistake 1: Thinking a higher nice value means higher priority**

It generally means lower scheduling weight for normal tasks.

**Mistake 2: Confusing NI and PRI**

- NI is the nice value.
- PRI is the scheduler priority display.

**Mistake 3: Assuming nice guarantees CPU time**

Niceness influences scheduling; it does not guarantee execution order or a fixed CPU percentage.

**Mistake 4: Assuming every scheduling class uses nice values**

Real-time scheduling classes have different priority mechanisms.

**Mistake 5: Changing priority without understanding the workload**

Unnecessary scheduling changes can make system performance harder to understand.

### 14. Cybersecurity and Administration Applications

**1. Security monitoring**

Monitoring process scheduling information can help administrators understand resource consumption.

**2. Background analysis**

A CPU-intensive analysis task can be assigned lower scheduling weight to reduce its impact on interactive work.

**3. System performance**

Administrators can investigate whether CPU competition contributes to slow system response.

**4. Resource management**

Niceness provides a way to influence how normal processes share CPU resources.

**5. Troubleshooting**

Comparing NI and PRI values can help explain differences in scheduling behavior.

### 15. Lesson Summary

In this lesson, we learned:

1. Process priority influences CPU scheduling.
2. Linux uses a scheduler to manage runnable tasks.
3. Niceness influences scheduling weight for normal tasks.
4. Nice values generally range from -20 to +19.
5. Lower nice values mean higher scheduling weight.
6. Higher nice values mean lower scheduling weight.
7. `ps` can display NI and PRI.
8. `top` provides real-time process information.
9. `nice` starts a command with a chosen nice value.
10. Niceness does not guarantee a fixed CPU share.

### 16. Knowledge Check

Answer these questions in your own words.

1. What is process priority?
2. What is CPU scheduling?
3. What is niceness in Linux?
4. What is the normal range of nice values?
5. Which nice value represents the default?
6. What does a nice value of +10 generally indicate?
7. What is the difference between NI and PRI?
8. Which command starts a process with a selected nice value?
9. Why might an administrator increase the nice value of a background task?
10. Does a nice value guarantee a fixed percentage of CPU time?

### 17. Practical Assignment

- [ ] Create `~/module08-lab/niceness`.
- [ ] Start one normal sleep process.
- [ ] Start another sleep process using `nice -n 10`.
- [ ] Identify both processes using `pgrep`.
- [ ] Inspect their NI and PRI values using `ps`.
- [ ] Compare their values using `top`.
- [ ] Explain why the two nice values differ.
- [ ] Terminate only your test processes using SIGTERM.
- [ ] Verify that both processes have exited.
- [ ] Complete all 10 knowledge-check questions.

**End of Lesson 10**

**Next Lesson:** Managing Processes with `nice` and `renice`.