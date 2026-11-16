
# MODULE 17 – ADVANCED LINUX
## LESSON 05: ADVANCED PROCESS MANAGEMENT

---

## 1. Introduction to Process Management

A process is a program currently being executed by the operating system.

Whenever you open a terminal, launch a browser, execute a script, or start a system service, Linux manages one or more processes.

Process management involves controlling how programs execute and how they use system resources.

Linux administrators use process management to:

- Monitor running applications.
- Identify resource-intensive processes.
- Stop unresponsive programs.
- Manage background tasks.
- Control process priorities.
- Investigate system performance.
- Troubleshoot application failures.

### Learning objectives

By the end of this lesson, you should be able to:

1. Explain Linux process states.
2. Identify process IDs and parent process IDs.
3. Monitor processes.
4. Manage foreground and background processes.
5. Send signals to processes.
6. Understand process priorities.
7. Inspect process resource consumption.
8. Understand process trees.
9. Diagnose unresponsive processes.
10. Perform practical process-management exercises.

---

## 2. Understanding Process Fundamentals

A process is an executing instance of a program.

For example:

- A Bash script stored on disk is a program.
- When you execute the script, Linux creates a process to run it.

Every process has information associated with it.

### Important process identifiers

| Identifier | Meaning |
|---|---|
| PID | Process ID |
| PPID | Parent Process ID |
| UID | User ID |
| GID | Group ID |
| PGID | Process Group ID |
| SID | Session ID |

### PID – Process ID

A PID uniquely identifies a process while it exists.

Display processes:

```bash
ps aux
```

Find the PID of a process:

```bash
pgrep bash
```

### PPID – Parent Process ID

The PPID identifies the process that created another process.

Display your current shell's PID:

```bash
echo $$
```

Display its parent PID:

```bash
echo $PPID
```

Inspect both:

```bash
ps -p $$ -o pid,ppid,comm
```

### Why process identifiers matter

PIDs are used when:

- Monitoring applications.
- Sending signals.
- Investigating system behaviour.
- Managing services.
- Troubleshooting software.

---

## 3. Understanding Linux Process States

A Linux process can exist in different states during execution.

Common process states include:

| State | Meaning |
|---|---|
| R | Running or runnable |
| S | Interruptible sleep |
| D | Uninterruptible sleep, commonly waiting for I/O |
| T | Stopped or traced |
| Z | Zombie process |
| I | Idle kernel thread |

### A. Running or runnable (R)

The process is executing or waiting for CPU time.

### B. Sleeping (S)

The process is waiting for an event or resource.

For example, a program waiting for keyboard input may be sleeping.

### C. Uninterruptible sleep (D)

The process is generally waiting for a kernel operation, often related to I/O.

A process in this state may not respond immediately to ordinary signals.

### D. Stopped (T)

The process has been suspended, for example by a stop signal.

### E. Zombie (Z)

A zombie is a process that has terminated, but its parent has not yet collected its exit status.

A zombie does not continue executing normally.

### Inspect process states

```bash
ps -eo pid,ppid,state,comm
```

This displays:

- PID.
- PPID.
- Process state.
- Command name.

---

## 4. Monitoring Processes with ps

The `ps` command displays information about running processes.

### Display processes in the current terminal

```bash
ps
```

### Display all processes

```bash
ps aux
```

### Display process hierarchy

```bash
ps -ef --forest
```

### Display selected information

```bash
ps -eo pid,ppid,user,%cpu,%mem,stat,comm
```

### Display one specific process

```bash
ps -p PID -f
```

Replace `PID` with an actual process ID.

### Understanding ps columns

| Column | Meaning |
|---|---|
| PID | Process identifier |
| PPID | Parent process identifier |
| USER | Process owner |
| %CPU | CPU utilization |
| %MEM | Memory utilization |
| STAT | Process state |
| COMMAND | Executed command |

### Practical example

```bash
ps aux | grep firefox
```

This searches process output for entries containing `firefox`.

Note: `grep` may also match its own search process. For cleaner matching, use:

```bash
pgrep -a firefox
```

---

## 5. Monitoring Processes with top

`top` is an interactive Linux process-monitoring tool.

Run:

```bash
top
```

It displays:

- CPU usage.
- Memory usage.
- Running processes.
- Process IDs.
- Process owners.
- System load averages.

### Important top keys

| Key | Function |
|---|---|
| `q` | Quit |
| `P` | Sort by CPU usage |
| `M` | Sort by memory usage |
| `k` | Send a signal to a process |
| `1` | Show individual CPU information |
| `h` | Display help |

### Understanding load average

The load average represents the average number of tasks that are runnable or in uninterruptible sleep over different time periods.

`top` commonly displays:

- 1-minute load average.
- 5-minute load average.
- 15-minute load average.

Load average is not the same as CPU utilization. Its interpretation depends partly on the number of CPU cores and the system workload.

---

## 6. Monitoring Processes with htop

`htop` provides an interactive process-monitoring interface.

Check whether it is installed:

```bash
which htop
```

If unavailable:

```bash
sudo apt install htop
```

Launch it:

```bash
htop
```

Features include:

- Interactive process selection.
- CPU and memory visualization.
- Process-tree display.
- Process filtering.
- Process termination.

Use `F10` or `q` to exit.

Only terminate processes you understand and are authorized to manage.

---

## 7. Foreground and Background Processes

Linux supports running processes in the foreground and background.

### A. Foreground process

A foreground process runs while the terminal remains attached to it.

Example:

```bash
sleep 30
```

The terminal waits until the command completes.

### B. Background process

Add `&` to run a command in the background.

```bash
sleep 60 &
```

The terminal returns control while the process continues running.

### Display background jobs

```bash
jobs
```

### Suspend a foreground process

Press:

```text
CTRL + Z
```

This suspends the foreground process.

### Resume a suspended job in the background

```bash
bg
```

### Bring a job into the foreground

```bash
fg
```

### Manage a specific job

```bash
jobs
```

Then:

```bash
fg %1
```

Here `%1` represents job number 1, if it exists.

### Important distinction

A shell job number such as `%1` is not the same as a system-wide PID.

---

## 8. Process Signals

Signals are notifications sent to processes to request or control certain actions.

Linux provides different signals for different purposes.

### Important signals

| Signal | Number | Purpose |
|---|---:|---|
| SIGHUP | 1 | Hangup notification |
| SIGINT | 2 | Interrupt |
| SIGQUIT | 3 | Quit and possibly produce a core dump |
| SIGKILL | 9 | Force termination |
| SIGTERM | 15 | Request termination |
| SIGSTOP | 19 on x86 | Stop process |
| SIGCONT | 18 on x86 | Continue stopped process |

Signal numbers can vary on different architectures. Prefer signal names when possible.

### Display available signals

```bash
kill -l
```

### Send SIGTERM

```bash
kill -TERM PID
```

This requests that the process terminate gracefully.

### Send SIGKILL

```bash
kill -KILL PID
```

This forcefully terminates the process.

SIGKILL cannot be caught, blocked, or ignored.

### Important security practice

Use SIGTERM first when appropriate. SIGKILL should be a last resort because the process cannot perform its normal cleanup.

### Find a process by name

```bash
pgrep -a sleep
```

### Terminate a process by PID

```bash
kill PID
```

Always verify the PID before sending a signal.

---

## 9. Understanding kill, pkill and killall

Linux provides several commands for sending signals.

### A. kill

Sends a signal to a specified PID.

```bash
kill -TERM 1234
```

### B. pkill

Finds processes using a name or pattern and sends a signal.

```bash
pkill -TERM sleep
```

Use carefully because multiple processes may match.

### C. killall

Sends a signal to processes matching a command name.

```bash
killall -TERM sleep
```

This can affect multiple processes with that name.

### Comparison

| Command | Main purpose |
|---|---|
| `kill` | Target a PID |
| `pkill` | Target processes by pattern |
| `killall` | Target processes by command name |

For learning and administration, inspect matching processes before terminating anything.

---

## 10. Process Priority and Nice Values

Linux uses scheduling priorities to help determine how processes compete for CPU time.

A process has a nice value that influences its scheduling priority.

For normal scheduling:

- Lower nice values generally mean higher scheduling priority.
- Higher nice values generally mean lower scheduling priority.

The normal nice-value range is:

```text
-20 to +19
```

### Display process priorities

```bash
ps -eo pid,ni,pri,comm
```

### Start a process with a higher nice value

```bash
nice -n 10 sleep 100
```

This starts the process with a nice value of 10.

### Start with a lower nice value

```bash
sudo nice -n -5 sleep 100
```

Changing priority to a negative nice value generally requires elevated privileges.

### Change an existing process priority

```bash
renice 10 -p PID
```

Replace `PID` with the actual process ID.

### Important distinction

Nice values influence scheduling priority; they do not reserve CPU time or guarantee a particular performance level.

---

## 11. Understanding Process Trees

Linux processes can create other processes.

This creates a parent-child relationship.

For example:

```text
systemd
   |
   +--- terminal
   |      |
   |      +--- bash
   |             |
   |             +--- python
   |
   +--- cron
```

### Display the process tree

```bash
pstree
```

### Display detailed process hierarchy

```bash
ps -ef --forest
```

### Display parent and child information

```bash
ps -o pid,ppid,comm -p $$
```

### Why process trees matter

They help administrators:

- Understand process relationships.
- Identify parent processes.
- Investigate unexpected applications.
- Troubleshoot child-process failures.
- Understand how services launch programs.

---

## 12. Understanding Zombie Processes

A zombie process is a process that has finished execution but whose parent has not yet collected its termination status.

It is shown with state `Z`.

### Identify zombie processes

```bash
ps -eo pid,ppid,state,comm | awk '$3 == "Z"'
```

### Why zombies occur

When a child process exits:

1. The kernel records its termination information.
2. The parent should collect the child's exit status.
3. If the parent does not do so, the child remains as a zombie entry.

### How zombies are handled

The parent process normally needs to collect the child's exit status.

If the parent exits, the zombie is generally adopted by an appropriate reaper process, commonly PID 1 or another configured subreaper, which can collect it.

### Important warning

Do not attempt to kill a zombie directly. It has already terminated. Investigate the parent process instead.

---

## 13. Process Information in /proc

Linux exposes process-related information through the virtual `/proc` filesystem.

Each running process usually has a directory named after its PID.

Example:

```bash
ls /proc
```

Inspect your current shell:

```bash
ls /proc/$$
```

Display process status:

```bash
cat /proc/$$/status
```

Display the command line:

```bash
cat /proc/$$/cmdline
```

Display executable information:

```bash
readlink /proc/$$/exe
```

Display the working directory:

```bash
readlink /proc/$$/cwd
```

### Important files

| File | Information |
|---|---|
| `/proc/PID/status` | Process status |
| `/proc/PID/cmdline` | Command-line arguments |
| `/proc/PID/exe` | Executable reference |
| `/proc/PID/cwd` | Current working directory |
| `/proc/PID/fd/` | Open file descriptors |

Access to some process information may be restricted by permissions or system security settings.

---

## 14. Practical Lab: Process Management

We will create controlled processes and practise monitoring them.

### Lab objectives

- Start a process.
- Identify its PID.
- Inspect its state.
- Run it in the background.
- Suspend and resume a job.
- Terminate a process safely.
- Inspect process information.

### Step 1: Create the laboratory directory

```bash
mkdir -p ~/module17-lab/lesson05
cd ~/module17-lab/lesson05
```

### Step 2: Start a test process

```bash
sleep 300 &
```

The shell should display a job number and PID.

### Step 3: Record the PID

```bash
jobs -l
```

### Step 4: Inspect the process

```bash
ps -p PID -o pid,ppid,state,ni,comm
```

Replace `PID` with the actual PID.

### Step 5: Inspect its process information

```bash
cat /proc/PID/status
```

Replace `PID` with the actual PID.

### Step 6: Monitor it

```bash
top -p PID
```

Press `q` to exit.

### Step 7: Send SIGTERM

```bash
kill -TERM PID
```

Replace `PID` with your test process ID.

### Step 8: Verify termination

```bash
ps -p PID
```

If the process has terminated, it should no longer appear.

### Step 9: Start another test process

```bash
sleep 300 &
```

Record its PID.

### Step 10: Practise process priority

```bash
nice -n 10 sleep 300 &
```

Find its PID:

```bash
pgrep -a sleep
```

Inspect its nice value:

```bash
ps -eo pid,ni,comm | grep sleep
```

Terminate only the test processes you created.

---

## 15. Practical Project: Process Monitoring Script

Now we will create a simple process-monitoring script.

### Step 1: Create the script

```bash
nano process-monitor.sh
```

### Step 2: Enter the code

```bash
#!/bin/bash

set -u

echo "================================"
echo "       PROCESS MONITOR"
echo "================================"

echo

echo "CURRENT USER:"
whoami

echo
echo "CURRENT SHELL:"
echo "$SHELL"

echo
echo "SYSTEM UPTIME:"
uptime

echo
echo "TOP CPU-CONSUMING PROCESSES:"
ps -eo pid,user,%cpu,%mem,comm --sort=-%cpu | head -n 6

echo
echo "TOP MEMORY-CONSUMING PROCESSES:"
ps -eo pid,user,%cpu,%mem,comm --sort=-%mem | head -n 6

echo
echo "PROCESS COUNT:"
ps -e --no-headers | wc -l

echo
echo "PROCESS STATE SUMMARY:"
ps -eo stat= | awk '
{
    state = substr($1, 1, 1)
    count[state]++
}
END {
    for (state in count)
        print state, count[state]
}'

echo
echo "================================"
echo "      MONITORING COMPLETED"
echo "================================"
```

### Step 3: Save and make executable

```bash
chmod +x process-monitor.sh
```

### Step 4: Execute

```bash
./process-monitor.sh
```

### Step 5: Save the report

```bash
./process-monitor.sh > process-report.txt
```

View the report:

```bash
cat process-report.txt
```

### Step 6: Validate the script

```bash
bash -n process-monitor.sh
```

No output normally indicates that Bash found no syntax errors.

---

## 16. Process Management Security Principles

Process management is important in cybersecurity because malicious or unexpected software also runs as processes.

Administrators should:

1. Monitor unfamiliar processes.
2. Check process ownership.
3. Inspect command-line arguments.
4. Investigate unusual resource consumption.
5. Check parent-child relationships.
6. Review unexpected executables.
7. Avoid terminating critical system processes without investigation.
8. Use the least privilege necessary.
9. Preserve relevant logs during investigations.
10. Verify process identity before sending signals.

### Useful investigation commands

```bash
ps aux
```

```bash
pstree
```

```bash
pgrep -a PROCESS_NAME
```

```bash
cat /proc/PID/status
```

```bash
readlink /proc/PID/exe
```

These commands help you investigate processes without immediately modifying them.

---

## 17. Knowledge Check

Answer these questions in your notebook.

1. What is a Linux process?
2. Explain the difference between PID and PPID.
3. Identify five common Linux process states.
4. What is the purpose of `ps aux`?
5. What is the difference between `top` and `htop`?
6. Explain foreground and background processes.
7. What does `CTRL + Z` do?
8. What is the purpose of the `bg` command?
9. Explain the difference between SIGTERM and SIGKILL.
10. What is a nice value?
11. What is a zombie process?
12. Why should you inspect a process before terminating it?
13. What is the purpose of `/proc/PID/status`?
14. Explain the difference between `kill`, `pkill`, and `killall`.

---

## 18. Important Commands Summary

| Command | Purpose |
|---|---|
| `ps aux` | Display running processes |
| `ps -ef --forest` | Display process hierarchy |
| `pgrep -a NAME` | Find processes by name |
| `pstree` | Display process tree |
| `top` | Interactive process monitoring |
| `htop` | Advanced interactive monitoring |
| `jobs -l` | Display shell jobs and PIDs |
| `bg` | Resume a suspended job in background |
| `fg` | Bring a job to foreground |
| `kill -TERM PID` | Request process termination |
| `kill -KILL PID` | Force process termination |
| `kill -l` | List available signals |
| `nice -n 10 COMMAND` | Start with a specified nice value |
| `renice 10 -p PID` | Change process nice value |
| `uptime` | Display system uptime and load averages |
| `cat /proc/PID/status` | Display process status |
| `bash -n script.sh` | Validate Bash syntax |

---

## 19. Lesson Summary

In this lesson, you learned how Linux manages running programs.

You covered:

- Process fundamentals.
- PID and PPID.
- Process states.
- Process monitoring.
- Foreground and background execution.
- Process signals.
- Process termination.
- Nice values and scheduling priority.
- Process trees.
- Zombie processes.
- The `/proc` filesystem.
- Process-monitoring automation.

You also created:

1. `process-monitor.sh`
2. `process-report.txt`

### Completion checklist

- [ ] Understand process fundamentals.
- [ ] Identify PID and PPID.
- [ ] Understand process states.
- [ ] Practise `ps`, `top`, and `htop`.
- [ ] Practise foreground and background jobs.
- [ ] Understand Linux signals.
- [ ] Practise safe process termination.
- [ ] Understand nice values.
- [ ] Understand process trees.
- [ ] Understand zombie processes.
- [ ] Inspect `/proc`.
- [ ] Create `process-monitor.sh`.
- [ ] Save `process-report.txt`.
- [ ] Answer the knowledge-check questions.

**END OF LESSON 05**
