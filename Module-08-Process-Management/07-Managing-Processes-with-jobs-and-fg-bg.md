---
---

# MODULE 08: PROCESS MANAGEMENT

## LESSON 07: MANAGING PROCESSES WITH JOBS, FG, AND BG

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux shell job control.
2. Use `jobs` to list active shell jobs.
3. Understand job numbers and process IDs.
4. Resume stopped jobs using `bg`.
5. Bring background jobs into the foreground using `fg`.
6. Select specific jobs using job specifications.
7. Manage multiple jobs within one terminal session.

---

### 2. What Is Job Control?

**Job control** is a feature provided by interactive shells such as Bash that allows users to manage multiple commands from one terminal session.

Instead of waiting for one command to finish before running another, you can:

- Start commands in the background.
- Suspend foreground commands.
- Resume suspended commands.
- Bring background jobs into the foreground.
- View the status of your shell jobs.

For example, imagine you are:

- Running a long file search.
- Downloading a file.
- Monitoring a process.
- Working with another terminal command.

Job control helps you manage these activities more efficiently.

### 3. Understanding Jobs

A **job** is a command or pipeline that the shell manages as a unit.

A job may contain one or more processes.

For example:

```bash
sleep 300 &
```

Bash starts the command as a background job.

You may see:

```text
[1] 2500
```

This means:

- `1` is the shell job number.
- `2500` is an example process ID.

A job number belongs to the current shell's job-control system.

A PID identifies a process in the operating system.

### 4. The jobs Command

The `jobs` command displays jobs managed by your current shell.

Syntax:

```bash
jobs
```

Example:

```bash
sleep 300 &
jobs
```

Possible output:

```text
[1]+ Running    sleep 300 &
```

The output tells you that:

- Job 1 exists.
- It is currently running.
- It was started in the background.

Common job statuses include:

| Status | Meaning |
|---|---|
| Running | The job is executing or has processes running |
| Stopped | The job has been suspended |
| Done | The job has completed |

A completed job may remain visible until the shell reports or clears its status.

### 5. Understanding jobs -l

To display job information with process IDs, use:

```bash
jobs -l
```

Example:

```text
[1]+ 2500 Running    sleep 300 &
```

Here, `2500` is the PID shown for the job's process.

This is useful when you need to connect shell job information with process information from commands such as `ps`.

### 6. Understanding Job Specifications

Bash provides special syntax for referring to jobs.

| Job specification | Meaning |
|---|---|
| `%1` | Job number 1 |
| `%2` | Job number 2 |
| `%+` | Current job |
| `%-` | Previous job |
| `%string` | Job whose command begins with the specified string |
| `%?string` | Job whose command contains the specified string |

The current job is generally the job that would be selected by `fg` or `bg` when no job specification is provided.

### 7. Understanding the fg Command

The `fg` command brings a job into the foreground.

Syntax:

```bash
fg
```

To select a particular job:

```bash
fg %1
```

Example:

```bash
sleep 300 &
jobs
fg %1
```

The selected job returns to the foreground.

The terminal becomes occupied by that job until it finishes or is interrupted.

Press `Ctrl+C` if you want to interrupt the foreground sleep process.

### 8. Understanding the bg Command

The `bg` command resumes a stopped job in the background.

Syntax:

```bash
bg
```

To select a particular job:

```bash
bg %1
```

Example:

```bash
sleep 300
```

Press:

```text
Ctrl+Z
```

The process becomes stopped.

Now execute:

```bash
bg %1
```

The shell resumes the job in the background.

Verify its status:

```bash
jobs
```

### 9. Managing Multiple Jobs

You can manage multiple jobs in one shell.

Example:

```bash
sleep 300 &
sleep 400 &
sleep 500 &
```

View them:

```bash
jobs
```

You should see three jobs.

Your job numbers will normally be assigned in the order they are created within that shell.

To inspect their process IDs:

```bash
jobs -l
```

You can select an individual job:

```bash
fg %2
```

This brings job 2 into the foreground.

To suspend it again, press `Ctrl+Z`.

Then resume it in the background:

```bash
bg %2
```

### 10. Understanding Current and Previous Jobs

Bash tracks a current job and a previous job.

The symbols are:

- `%+`: Current job.
- `%-`: Previous job.

You can display job information using:

```bash
jobs
```

The shell commonly marks the current job with `+` and the previous job with `-`.

These markers can change as jobs are created, completed, or moved between foreground and background.

### 11. Important Difference Between jobs and ps

These commands serve different purposes.

| Command | Purpose |
|---|---|
| `jobs` | Displays jobs managed by the current shell |
| `jobs -l` | Displays shell jobs with process IDs |
| `ps` | Displays process information |
| `pgrep` | Searches for process IDs by name |

A process started by another application or another terminal will not normally appear in your current shell's `jobs` output.

However, it may appear in `ps`.

### 12. Practical Laboratory: Managing Multiple Jobs

**Objective:** Practise managing multiple processes using Bash job control.

#### Step 1: Create your practice directory

```bash
mkdir -p ~/module08-lab/job-control
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/job-control
```

#### Step 3: Start three background jobs

```bash
sleep 300 &
sleep 400 &
sleep 500 &
```

Observe the job numbers and PIDs.

#### Step 4: Display all jobs

```bash
jobs
```

You should see three running jobs.

#### Step 5: Display their PIDs

```bash
jobs -l
```

Record the job numbers and PIDs.

#### Step 6: Bring the second job into the foreground

```bash
fg %2
```

The second sleep process should occupy the terminal.

#### Step 7: Suspend the second job

Press:

```text
Ctrl + Z
```

#### Step 8: Verify its status

```bash
jobs
```

The second job should appear as stopped.

#### Step 9: Resume the second job in the background

```bash
bg %2
```

Verify:

```bash
jobs
```

#### Step 10: Inspect the first job

```bash
fg %1
```

Press `Ctrl+Z` to suspend it.

Then run:

```bash
jobs
```

#### Step 11: Resume the first job

```bash
bg %1
```

#### Step 12: Bring the third job into the foreground

```bash
fg %3
```

Press `Ctrl+Z` to suspend it.

#### Step 13: Resume the third job

```bash
bg %3
```

#### Step 14: Inspect all jobs

```bash
jobs -l
```

You should now have three background jobs.

#### Step 15: Clean up your laboratory

Because these are temporary sleep processes, allow them to finish naturally or use the following commands to interrupt the selected jobs:

```bash
fg %1
```

Press `Ctrl+C`.

Repeat for the remaining jobs:

```bash
fg %2
```

Press `Ctrl+C`.

Then:

```bash
fg %3
```

Press `Ctrl+C`.

Finally, verify:

```bash
jobs
```

There should be no remaining running or stopped jobs from this exercise.

**Note:** If a job has already completed, the shell may report that it no longer exists. That is normal.

### 13. Common Mistakes

**Mistake 1: Using a PID where a job number is expected**

Example:

```bash
fg 2500
```

This is not the normal way to select a shell job.

Use:

```bash
fg %1
```

**Mistake 2: Thinking bg terminates a process**

`bg` resumes a stopped job in the background.

It does not terminate it.

**Mistake 3: Thinking fg creates a new process**

`fg` brings an existing shell job into the foreground.

**Mistake 4: Assuming jobs displays all system processes**

It displays jobs managed by the current shell.

**Mistake 5: Forgetting to clean up laboratory jobs**

Temporary jobs should be completed or stopped after the exercise.

### 14. Cybersecurity Applications

Job control is useful in practical Linux administration and security work.

**1. Managing long-running tasks**

Administrators can run tasks in the background while continuing other terminal work.

**2. Monitoring command execution**

Job-control information helps users understand which commands are still running.

**3. Investigating unexpected execution**

Understanding foreground and background jobs helps analysts investigate unusual terminal activity.

**4. Troubleshooting**

A suspended command may explain why an expected operation has stopped making progress.

**5. Managing administrative workflows**

Job control makes terminal-based maintenance tasks more efficient.

### 15. Lesson Summary

In this lesson, we learned:

1. Job control is a Bash feature for managing shell jobs.
2. A job may contain one or more processes.
3. `jobs` displays jobs managed by the current shell.
4. `jobs -l` displays process IDs alongside job information.
5. `fg` brings a job into the foreground.
6. `bg` resumes a stopped job in the background.
7. `%1` selects job number 1.
8. `%+` represents the current job.
9. `%-` represents the previous job.
10. `jobs` and `ps` have different purposes.

### 16. Knowledge Check

Answer these questions in your own words.

1. What is Linux shell job control?
2. What is a job?
3. What does the `jobs` command display?
4. What additional information does `jobs -l` provide?
5. What is the purpose of `fg`?
6. What is the purpose of `bg`?
7. What does `%2` represent?
8. What is the difference between `%+` and `%-`?
9. Why might a process appear in `ps` but not in `jobs`?
10. Why is job control useful to Linux administrators?

### 17. Practical Assignment

Complete these tasks in your Kali Linux virtual machine:

- [ ] Create `~/module08-lab/job-control`.
- [ ] Start three sleep processes in the background.
- [ ] Display jobs using `jobs`.
- [ ] Display job PIDs using `jobs -l`.
- [ ] Bring job 2 into the foreground using `fg %2`.
- [ ] Suspend it using `Ctrl+Z`.
- [ ] Resume it using `bg %2`.
- [ ] Practise the same operations with jobs 1 and 3.
- [ ] Display all jobs again.
- [ ] Clean up the temporary jobs.
- [ ] Verify that no laboratory jobs remain.
- [ ] Complete the 10 knowledge-check questions.

**End of Lesson 07**

**Next Lesson:** Sending Signals to Linux Processes.