# MODULE 08: PROCESS MANAGEMENT

## LESSON 03: VIEWING PROCESSES WITH PS

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain the purpose of the `ps` command.
2. Display processes using different `ps` options.
3. Understand the difference between `ps`, `ps aux`, and `ps -ef`.
4. Select specific process information for display.
5. Filter process listings.
6. Use `ps` to support basic system monitoring and security investigations.

---

### 2. What Is the ps Command?

The `ps` command stands for **Process Status**.

It is a Linux command used to display information about processes.

Linux may be running many processes simultaneously. The `ps` command allows administrators and users to inspect those processes.

You can use it to discover:

- Which processes are running.
- The PID of a process.
- The PPID of a process.
- Which user owns a process.
- How long a process has been running.
- Which command started a process.
- The current process state.

**Basic syntax:**

```bash
ps [options]
```

The options determine which processes are displayed and what information appears.

### 3. Basic ps Command

Run:

```bash
ps
```

This normally displays processes associated with your current terminal session.

Example:

```text
    PID TTY          TIME CMD
   1500 pts/0    00:00:00 bash
   1650 pts/0    00:00:00 ps
```

Explanation:

| Column | Meaning |
|---|---|
| PID | Process ID |
| TTY | Terminal associated with the process |
| TIME | Accumulated CPU time |
| CMD | Command name |

Your output will differ depending on your system.

### 4. Understanding ps aux

One of the most frequently used process-monitoring commands is:

```bash
ps aux
```

It displays a broad list of processes, including processes that are not attached to your current terminal.

Let's understand the options:

- `a`: Selects processes associated with terminals, including other users' terminals.
- `u`: Displays user-oriented information.
- `x`: Includes processes without a controlling terminal.

Together, these options provide a broad process listing.

Example:

```bash
ps aux
```

Important columns:

| Column | Meaning |
|---|---|
| USER | Process owner |
| PID | Process ID |
| %CPU | CPU usage percentage |
| %MEM | Memory usage percentage |
| VSZ | Virtual memory size |
| RSS | Resident memory size |
| TTY | Controlling terminal |
| STAT | Process state |
| START | Process start information |
| TIME | Accumulated CPU time |
| COMMAND | Command and arguments |

### 5. Understanding CPU and Memory Columns

Two useful columns in `ps aux` are `%CPU` and `%MEM`.

**%CPU**

Shows the percentage of CPU capacity attributed to the process over the reporting interval used by `ps`.

A process with high CPU usage may be performing intensive calculations or other work.

**%MEM**

Shows the percentage of physical memory represented by the process's resident memory.

A process consuming a large amount of memory may affect system performance.

**Important:** High CPU or memory usage does not automatically mean a process is malicious. Many legitimate applications use substantial resources.

### 6. Understanding ps -ef

Another common command is:

```bash
ps -ef
```

This displays processes in full-format listing style.

Example:

```text
UID      PID  PPID  C STIME TTY          TIME CMD
root       1     0  0 10:00 ?        00:00:02 systemd
kali    1500  1400  0 10:15 pts/0    00:00:00 bash
kali    1650  1500  0 10:16 pts/0    00:00:00 ps -ef
```

Explanation:

| Column | Meaning |
|---|---|
| UID | User associated with the process |
| PID | Process ID |
| PPID | Parent Process ID |
| C | CPU utilization information |
| STIME | Process start time |
| TTY | Controlling terminal |
| TIME | Accumulated CPU time |
| CMD | Command and arguments |

### 7. Comparing ps, ps aux, and ps -ef

| Command | Purpose |
|---|---|
| `ps` | Displays processes associated with the current terminal |
| `ps aux` | Displays a broad list using BSD-style options |
| `ps -ef` | Displays a full-format process listing using Unix-style options |

Both `ps aux` and `ps -ef` are useful for inspecting processes across the system.

They use different option styles and output formats.

### 8. Selecting Specific Columns

You do not always need every process column.

The `-o` option lets you select the information you want.

Example:

```bash
ps -o pid,ppid,comm
```

This displays:

- PID
- PPID
- Command name

Another example:

```bash
ps -eo pid,user,%cpu,%mem,comm
```

This displays selected information for processes across the system.

You can also use:

```bash
ps -eo pid,ppid,user,stat,etime,cmd
```

This is useful when investigating process relationships and execution time.

### 9. Filtering Processes

A system may contain many processes. Filtering makes it easier to find a particular process.

#### Method 1: Using grep

To search for processes containing the word `bash`:

```bash
ps aux | grep bash
```

The pipe symbol (`|`) sends the output of one command to another command.

Here:

1. `ps aux` displays processes.
2. `grep bash` searches the output for lines containing `bash`.

To avoid matching the grep command itself in a simple search, you can use:

```bash
ps aux | grep '[b]ash'
```

#### Method 2: Using pgrep

You can search directly by process name:

```bash
pgrep bash
```

To display the process name alongside its PID:

```bash
pgrep -a bash
```

#### Method 3: Using ps with grep

To find a running sleep process:

```bash
ps -ef | grep '[s]leep'
```

### 10. Viewing a Specific Process

If you know a process's PID, you can inspect it directly.

Example:

```bash
ps -p 1
```

This displays information about PID 1.

To select particular columns:

```bash
ps -p 1 -o pid,ppid,user,stat,comm
```

Replace `1` with the PID you want to inspect.

### 11. Viewing Processes in a Tree Format

A process tree displays parent-child relationships.

The `pstree` command can provide a visual representation.

Example:

```bash
pstree
```

To include process identifiers:

```bash
pstree -p
```

If the command is unavailable, you can install the package that provides it:

```bash
sudo apt update
sudo apt install psmisc
```

Only install it if needed.

The process tree helps you understand which applications launched other processes.

### 12. Practical Laboratory: Exploring Processes with ps

**Objective:** Use different `ps` options to inspect processes on Kali Linux.

#### Step 1: Create your practice directory

```bash
mkdir -p ~/module08-lab/ps-practice
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/ps-practice
```

#### Step 3: Display current terminal processes

```bash
ps
```

Observe the output.

#### Step 4: Display a broad process listing

```bash
ps aux
```

Identify the following columns:

- USER
- PID
- %CPU
- %MEM
- STAT
- COMMAND

#### Step 5: Display processes using full format

```bash
ps -ef
```

Find the PID and PPID columns.

#### Step 6: Select specific columns

```bash
ps -eo pid,ppid,user,stat,comm
```

Observe how the output becomes more focused.

#### Step 7: Find Bash processes

```bash
pgrep -a bash
```

Record at least one PID if a Bash process is running.

#### Step 8: Inspect a specific PID

Use the PID you recorded:

```bash
ps -p YOUR_PID -o pid,ppid,user,stat,comm
```

Replace `YOUR_PID` with the actual number.

#### Step 9: Create a temporary process

Run:

```bash
sleep 180
```

Leave it running.

Open another terminal and execute:

```bash
ps aux | grep '[s]leep 180'
```

Observe the process.

#### Step 10: Inspect its PID and PPID

Use the PID displayed in the previous command:

```bash
ps -p YOUR_PID -o pid,ppid,user,stat,etime,cmd
```

Replace `YOUR_PID` with the actual PID.

#### Step 11: Stop the temporary process

Return to the first terminal and press:

```text
Ctrl + C
```

### 13. Cybersecurity Applications of ps

The `ps` command is useful during security monitoring and incident response.

**1. Suspicious process identification**

Security analysts inspect running processes to identify unfamiliar or unexpected activity.

**2. Process ancestry investigation**

The PPID column helps analysts understand which process launched another process.

**3. Resource monitoring**

High CPU or memory usage can help identify processes that require further investigation.

**4. Command-line inspection**

The `CMD` column can reveal the executable and arguments used to start a process.

**5. Incident response**

Process listings can provide useful evidence when investigating suspicious system behavior.

**Security reminder:** A process name can be misleading. Investigate the executable path, owner, command line, parent process, and behavior before deciding whether it is malicious.

### 14. Common Mistakes

**Mistake 1: Confusing PID with PPID**

- PID identifies the process itself.
- PPID identifies its parent.

**Mistake 2: Assuming ps displays every process by default**

The basic `ps` command usually focuses on the current terminal session.

Use `ps aux` or `ps -ef` for a broader view.

**Mistake 3: Assuming high CPU usage means malware**

Legitimate applications can consume significant resources.

**Mistake 4: Forgetting that process IDs change**

A process may terminate, and its PID may later be reused.

**Mistake 5: Treating grep results as proof**

A matching process name is only a starting point for investigation.

### 15. Lesson Summary

In this lesson, we learned:

1. `ps` means Process Status.
2. `ps` displays process information.
3. `ps aux` provides a broad process listing.
4. `ps -ef` displays processes in full-format style.
5. The `-o` option selects specific output columns.
6. `grep` and `pgrep` help locate processes.
7. `ps -p` inspects a specific PID.
8. `pstree` displays process relationships.
9. Process inspection is an important system-administration and cybersecurity skill.

### 16. Knowledge Check

Answer these questions in your own words.

1. What does `ps` stand for?
2. What is the purpose of `ps aux`?
3. What is the difference between `ps aux` and `ps -ef`?
4. What does the `%CPU` column represent?
5. What does the `%MEM` column represent?
6. What is the purpose of the `-o` option?
7. Which command can display a process with a specific PID?
8. What does `pgrep -a` display?
9. What is the purpose of `pstree -p`?
10. Why should a security analyst investigate a suspicious process beyond checking its name?

### 17. Practical Assignment

Complete these tasks in your Kali Linux virtual machine:

- [ ] Create `~/module08-lab/ps-practice`.
- [ ] Run `ps`.
- [ ] Run `ps aux`.
- [ ] Run `ps -ef`.
- [ ] Run `ps -eo pid,ppid,user,stat,comm`.
- [ ] Find Bash processes using `pgrep -a bash`.
- [ ] Inspect a process using `ps -p`.
- [ ] Start `sleep 180`.
- [ ] Find it using `ps aux | grep '[s]leep 180'`.
- [ ] Inspect its PID and PPID.
- [ ] Stop it using `Ctrl+C`.
- [ ] Complete the 10 knowledge-check questions.

**End of Lesson 03**

**Next Lesson:** Monitoring Processes with `top` and `htop`.