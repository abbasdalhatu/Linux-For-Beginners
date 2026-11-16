# MODULE 08: PROCESS MANAGEMENT

## LESSON 02: UNDERSTANDING PROCESS IDs AND PPIDs

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define a Process ID (PID).
2. Define a Parent Process ID (PPID).
3. Understand how Linux assigns process identifiers.
4. Explain parent-child process relationships.
5. Identify PIDs and PPIDs using Linux commands.
6. Understand why PIDs and PPIDs are useful in cybersecurity.

---

### 2. What Is a Process ID (PID)?

A **Process ID (PID)** is a numerical identifier assigned to a process by the Linux kernel.

Linux uses PIDs to distinguish between running processes.

For example, your Kali Linux system might have processes with these identifiers:

| Process | Example PID |
|---|---:|
| systemd | 1 |
| Bash shell | 1450 |
| Firefox | 2200 |
| Terminal application | 1800 |

These numbers are examples only. Your actual PIDs will be different.

**Important:** A PID identifies a process while it exists. After a process terminates, its PID may eventually be reused by another process.

### 3. Why Does Linux Use PIDs?

Linux may run hundreds of processes simultaneously.

Without identifiers, the operating system would have difficulty distinguishing between them.

PIDs help Linux:

1. Identify individual processes.
2. Monitor running processes.
3. Send signals to specific processes.
4. Manage process relationships.
5. Track process execution.
6. Perform system administration and troubleshooting.

For example, if a particular application becomes unresponsive, an administrator can identify its PID before deciding how to handle it.

### 4. What Is a Parent Process ID (PPID)?

A **Parent Process ID (PPID)** identifies the process that created another process.

When one process creates another, the original process is called the parent, while the newly created process is called the child.

Example:

```text
Parent Process
      |
      v
Child Process
      |
      v
Another Child Process
```

Each process has its own PID, and its PPID identifies its parent.

**Example:**

| Process | PID | PPID |
|---|---:|---:|
| Bash | 1500 | 1200 |
| Sleep | 1600 | 1500 |

In this example:

- Bash has PID 1500.
- Sleep has PID 1600.
- Sleep's PPID is 1500.
- Therefore, Bash is the parent of the sleep process.

### 5. Understanding Parent and Child Processes

Linux processes frequently create other processes.

For example, when you execute a command inside Bash, the shell may create a child process to run that command.

Consider:

```bash
sleep 60
```

Your Bash shell may create a child process to execute `sleep`.

The relationship could look like this:

```text
Bash Shell
PID: 1500
    |
    v
sleep 60
PID: 1600
PPID: 1500
```

The numbers are illustrative.

This relationship is important because it helps us understand how programs are launched and how processes are connected.

### 6. How Linux Creates Processes

Linux commonly creates processes through system calls such as:

**fork()**

Creates a new process by duplicating the calling process's execution context.

**execve()**

Replaces the current process image with a new program.

A common sequence is:

1. A parent process calls `fork()`.
2. The kernel creates a child process.
3. The child may call an `exec` function.
4. The new program begins executing inside the child process.

Modern Linux also has other process-creation mechanisms, but this sequence is a useful foundation.

### 7. Identifying Your Current Shell PID

Linux provides a simple command for displaying the PID of your current Bash shell.

```bash
echo $$
```

Example output:

```text
1500
```

The number is the PID of the current Bash process.

Your result will be different.

### 8. Viewing PID and PPID with ps

The `ps` command can display both PID and PPID.

Run:

```bash
ps -o pid,ppid,comm
```

Explanation:

| Option | Meaning |
|---|---|
| `-o` | Select output columns |
| `pid` | Process ID |
| `ppid` | Parent Process ID |
| `comm` | Command name |

Example output:

```text
    PID    PPID COMMAND
   1500    1200 bash
   1600    1500 sleep
```

This allows you to inspect process relationships.

### 9. Viewing Detailed Process Information

You can use:

```bash
ps -ef
```

This displays a full-format process listing.

Important columns include:

| Column | Meaning |
|---|---|
| UID | User associated with the process |
| PID | Process ID |
| PPID | Parent Process ID |
| C | CPU utilization information |
| STIME | Process start time |
| TTY | Controlling terminal |
| TIME | Accumulated CPU time |
| CMD | Command that started the process |

To display only selected information:

```bash
ps -eo pid,ppid,user,stat,comm
```

### 10. Finding a Process by Name

You can use `pgrep` to find process IDs by process name.

Example:

```bash
pgrep sleep
```

If a sleep process is running, its PID should be displayed.

To display the process name alongside its PID:

```bash
pgrep -a sleep
```

This is useful when you need to identify a process without searching through a long process listing.

### 11. Understanding the Init Process

On most modern Kali Linux installations, `systemd` is the initial userspace process.

It normally has:

```text
PID: 1
```

It starts and manages many system services and processes during system startup.

A simplified process tree might look like:

```text
systemd (PID 1)
    |
    +--- system service
    |
    +--- terminal service
           |
           +--- bash
                  |
                  +--- sleep
```

The exact process tree on your system will differ.

When a parent process terminates while its children are still running, those children may be adopted by another process, commonly PID 1 or a configured subreaper.

### 12. Practical Laboratory: Exploring PID and PPID

**Objective:** Create a child process and inspect its PID and PPID.

#### Step 1: Create your practice directory

```bash
mkdir -p ~/module08-lab/pid-ppid
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/pid-ppid
```

#### Step 3: Display your Bash PID

```bash
echo $$
```

Write down the result.

#### Step 4: Start a sleep process

```bash
sleep 300
```

Leave this terminal running.

#### Step 5: Open another terminal

In the second terminal, execute:

```bash
ps -eo pid,ppid,user,stat,comm | grep -E 'PID|bash|sleep'
```

Look for the `sleep` process.

Record its PID and PPID.

#### Step 6: Compare the identifiers

Your output should show that the sleep process has its own PID.

Its PPID should normally match the PID of the shell that launched it.

Remember that your actual output may contain additional Bash processes.

#### Step 7: Find the sleep process directly

```bash
pgrep -a sleep
```

Observe the PID and command.

#### Step 8: Stop the process safely

Return to the first terminal and press:

```text
Ctrl + C
```

#### Step 9: Verify termination

Run:

```bash
pgrep -a sleep
```

The process you created should no longer appear.

### 13. Cybersecurity Importance of PID and PPID

Understanding process identifiers is valuable during security investigations.

**1. Investigating suspicious processes**

A security analyst can identify a suspicious process using its PID.

**2. Understanding process ancestry**

PPID information helps investigators understand which process launched another process.

**3. Detecting unusual execution chains**

For example, an unexpected application launching a command interpreter may deserve investigation.

However, this behavior is not automatically malicious; legitimate applications can also launch shells or other programs.

**4. Troubleshooting**

Administrators can identify which process is consuming resources or causing problems.

**5. Incident response**

During an investigation, recording process IDs and parent-child relationships can help reconstruct system activity.

### 14. Important Terms

| Term | Definition |
|---|---|
| PID | Process Identifier |
| PPID | Parent Process ID |
| Parent Process | Process that creates another process |
| Child Process | Process created by another process |
| fork() | System call used to create a new process |
| execve() | System call that replaces a process image with a new program |
| systemd | Common Linux userspace initialization and service manager |
| Process Tree | Hierarchical representation of process relationships |

### 15. Lesson Summary

In this lesson, we learned:

1. Every Linux process has a PID while it exists.
2. PPID identifies the parent process.
3. Parent processes can create child processes.
4. Linux uses process-creation mechanisms such as `fork()` and `execve()`.
5. `echo $$` displays the current Bash shell's PID.
6. `ps -o pid,ppid,comm` displays process identifiers and command names.
7. `ps -ef` displays detailed process information.
8. `pgrep` helps locate processes by name.
9. PID and PPID relationships are useful for cybersecurity investigations.

### 16. Knowledge Check

Answer these questions in your own words.

1. What is a Process ID?
2. What does PPID mean?
3. Why does Linux assign PIDs to processes?
4. Explain the relationship between a parent process and a child process.
5. Which command displays the PID of your current Bash shell?
6. Which `ps` command displays PID, PPID, and command name?
7. What is the purpose of `pgrep`?
8. What is the normal PID of systemd on a standard modern Kali installation?
9. What does the `fork()` system call do?
10. Why is understanding PPID useful when investigating suspicious activity?

### 17. Practical Assignment

Complete these tasks in your Kali Linux virtual machine:

- [ ] Create `~/module08-lab/pid-ppid`.
- [ ] Display your current shell PID using `echo $$`.
- [ ] Run `ps -o pid,ppid,comm`.
- [ ] Run `ps -ef`.
- [ ] Run `ps -eo pid,ppid,user,stat,comm`.
- [ ] Start `sleep 300`.
- [ ] Find its PID using `pgrep -a sleep`.
- [ ] Identify its PPID using `ps`.
- [ ] Compare the sleep process's PPID with the parent shell's PID.
- [ ] Stop the sleep process using `Ctrl+C`.
- [ ] Complete the 10 knowledge-check questions.

**End of Lesson 02**

**Next Lesson:** Viewing Processes with `ps`.