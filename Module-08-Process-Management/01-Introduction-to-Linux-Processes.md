---
---

# MODULE 08: PROCESS MANAGEMENT

## LESSON 01: INTRODUCTION TO LINUX PROCESSES

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define a Linux process.
2. Explain the difference between a program and a process.
3. Understand the basic process lifecycle.
4. Explain the role of the Linux kernel in process management.
5. Understand CPU time, memory allocation, and process scheduling.
6. Identify why process management matters in cybersecurity.

---

### 2. What Is a Process?

A **process** is an instance of a program that is currently executing.

Whenever you open a terminal, launch a browser, run a command, or start an application, Linux creates or uses a process to perform that activity.

A process is more than just the program's instructions. It also has information and resources associated with its execution, such as:

- A Process ID (PID).
- Memory allocated for its execution.
- CPU time.
- An execution state.
- Open files and other resources.
- Information maintained by the Linux kernel.

**Simple explanation:**

Think of a program as a recipe and a process as someone actively following that recipe.

The recipe exists even when nobody is cooking. Similarly, a program can exist as a file on your computer without currently running.

Once you execute the program, it becomes a running process.

### 3. Program vs Process

These two terms are related, but they are not identical.

| Program | Process |
|---|---|
| A set of instructions stored in a file | A running instance of a program |
| Usually stored on disk | Managed by the operating system |
| Can exist without running | Exists while executing or during its lifecycle |
| Does not need CPU time while inactive | Receives CPU time when scheduled |
| Can be executed multiple times | Each execution can create a separate process |

**Example:**

Suppose you have a program called `calculator`.

When it is stored on your computer, it is a program.

When you launch it, Linux creates a process to execute it.

If you open two separate calculator instances, they may have different process IDs.

### 4. What Is Process Management?

Process management is the operating system's responsibility for controlling and coordinating running processes.

Linux must manage multiple processes efficiently, even when several applications appear to run simultaneously.

The Linux kernel performs important process-management activities, including:

1. **Process creation:** Starting new processes.
2. **Process scheduling:** Deciding which process gets CPU time.
3. **Resource allocation:** Managing memory and other resources.
4. **Process synchronization:** Coordinating processes that share resources.
5. **Process termination:** Handling processes that finish or are stopped.
6. **Process monitoring:** Maintaining information about active processes.

### 5. The Role of the Linux Kernel

The Linux kernel is the core component of the operating system.

It acts as an intermediary between applications and computer hardware.

For process management, the kernel:

- Creates and maintains process information.
- Assigns process identifiers.
- Schedules processes for CPU execution.
- Manages memory resources.
- Handles signals sent to processes.
- Controls access to system resources.

**Important:** Ordinary applications do not independently control the CPU or physical memory. They request resources through the operating system, and the kernel manages access.

### 6. Basic Process Lifecycle

A process passes through different stages during its lifetime.

A simplified lifecycle looks like this:

1. **Creation:** A process is created.
2. **Ready:** It is prepared and waiting for CPU time.
3. **Running:** It is currently executing on a CPU.
4. **Waiting:** It is waiting for an event or resource, such as input or disk activity.
5. **Termination:** It finishes or is stopped.

A simplified diagram:

```text
       CREATION
           |
           v
         READY
           |
           v
        RUNNING
        /     \
       v       v
   WAITING   TERMINATED
       |
       v
     READY
```

This is an introductory model. Real Linux process states and transitions are more detailed and will be studied later.

### 7. CPU Time and Process Scheduling

A CPU executes instructions for processes.

When multiple processes need CPU time, Linux uses a scheduler to decide which process should execute.

For example, imagine you are:

- Playing music.
- Browsing the internet.
- Downloading a file.
- Running a terminal command.

These activities can all involve separate processes.

The scheduler distributes CPU execution time among eligible tasks.

This helps the computer remain responsive while handling multiple activities.

### 8. What Is Context Switching?

A **context switch** happens when the CPU stops executing one task and switches to another.

The operating system saves the execution information needed to resume the first task and restores the information for the next task.

For example:

1. Process A is executing.
2. The scheduler decides to run Process B.
3. The system saves the necessary execution state of Process A.
4. The CPU switches to Process B.
5. Process A can resume later.

Context switching helps Linux share CPU resources among many tasks.

However, context switching also has a performance cost, so it is not completely free.

### 9. Processes and Threads

A process can contain one or more threads.

A **thread** is an execution unit within a process.

Consider a web browser:

- The browser process may manage several threads.
- One thread may handle user-interface activity.
- Another may handle network operations.
- Another may perform background work.

Threads within the same process can share resources, including memory.

**Key distinction:**

- A process provides an environment and resources for execution.
- A thread is an execution path within that process.

We will study this distinction further when it becomes relevant to process monitoring.

### 10. Introducing Basic Process Commands

Linux provides commands for viewing and monitoring processes.

#### Command 1: ps

Displays information about processes associated with your terminal.

```bash
ps
```

#### Command 2: ps aux

Displays a broader list of processes running on the system.

```bash
ps aux
```

Some important output fields include:

| Field | Meaning |
|---|---|
| USER | User who owns the process |
| PID | Process ID |
| %CPU | CPU usage percentage |
| %MEM | Memory usage percentage |
| STAT | Process state |
| COMMAND | Command associated with the process |

We will examine these commands in detail in later lessons.

#### Command 3: top

Displays a continuously updating view of running processes and system resource usage.

```bash
top
```

Press `q` to exit.

#### Command 4: echo $$

Displays the process ID of your current Bash shell.

```bash
echo $$
```

The number displayed is the PID of that shell.

#### Command 5: sleep

Starts a harmless process that waits for a specified number of seconds.

```bash
sleep 60
```

This command will run for 60 seconds unless interrupted.

Press `Ctrl+C` to stop it.

### 11. Practical Laboratory: Understanding a Process

**Objective:** Create a harmless process, observe it, and understand how Linux identifies it.

#### Step 1: Create a practice directory

```bash
mkdir -p ~/module08-lab/processes
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/processes
```

#### Step 3: Start a harmless process

```bash
sleep 120
```

The terminal will remain occupied while the process runs.

#### Step 4: Open another terminal

Keep the first terminal running.

Open another terminal window in Kali Linux.

#### Step 5: Find the sleep process

Run:

```bash
ps aux | grep '[s]leep 120'
```

Observe the output.

Find the PID associated with the `sleep 120` command.

#### Step 6: Understand the result

You should see information about the process, including:

- The user running it.
- Its PID.
- Its CPU usage.
- Its memory usage.
- The command that started it.

The PID is a unique identifier for that particular running process at that moment.

#### Step 7: Stop the process safely

Return to the first terminal and press:

```text
Ctrl + C
```

The foreground `sleep` process should stop.

#### Step 8: Verify that it has stopped

In the second terminal, run:

```bash
ps aux | grep '[s]leep 120'
```

The process should no longer appear.

**Lab completed!**

### 12. Process Management in Cybersecurity

Understanding processes is important for cybersecurity professionals and system administrators.

Process monitoring can help identify:

**1. Suspicious processes**

An unfamiliar process may deserve investigation, especially if its command, location, or behavior is unexpected.

**2. Resource abuse**

A process consuming unusually high CPU or memory resources may affect system performance.

**3. Unauthorized activity**

Process information can help administrators investigate programs running without authorization.

**4. Malware investigation**

Security analysts examine process names, parent-child relationships, execution paths, resource usage, and network activity when investigating possible malware.

**5. System troubleshooting**

Administrators use process information to diagnose applications that are frozen, consuming excessive resources, or behaving unexpectedly.

Remember: an unfamiliar process name alone does not prove that a system is infected. Investigate its location, owner, command line, and behavior before drawing conclusions.

### 13. Important Terms to Remember

| Term | Definition |
|---|---|
| Process | An instance of a program being executed |
| PID | Process Identifier |
| Kernel | Core component of the operating system |
| Scheduler | Kernel component responsible for selecting tasks for CPU execution |
| CPU Time | Time a process spends executing on the CPU |
| Context Switch | Switching CPU execution from one task to another |
| Thread | An execution unit within a process |
| Process Lifecycle | Stages a process passes through during its existence |

### 14. Lesson Summary

In this lesson, we learned that:

1. A process is a running instance of a program.
2. Programs and processes are different.
3. Linux uses the kernel to manage processes and system resources.
4. Processes pass through different lifecycle stages.
5. The scheduler distributes CPU time among tasks.
6. Context switching allows the CPU to move between tasks.
7. A process can contain multiple threads.
8. Commands such as `ps`, `top`, `sleep`, and `echo $$` help us begin exploring processes.
9. Process monitoring is an important skill in cybersecurity.

### 15. Knowledge Check

Answer these questions in your own words.

1. What is a Linux process?
2. Explain the difference between a program and a process.
3. What is the role of the Linux kernel in process management?
4. What is a process scheduler?
5. Explain context switching.
6. What is the difference between a process and a thread?
7. Which command displays a broad list of running processes?
8. What does the `echo $$` command display?
9. Why is process monitoring important in cybersecurity?
10. What happens when you press `Ctrl+C` while running `sleep 120` in the foreground?

### 16. Practical Assignment

Complete these tasks in your Kali Linux virtual machine:

- [ ] Create `~/module08-lab/processes`.
- [ ] Run `ps`.
- [ ] Run `ps aux`.
- [ ] Run `top` and exit using `q`.
- [ ] Run `echo $$`.
- [ ] Start `sleep 120` in one terminal.
- [ ] Find it from another terminal using `ps aux | grep '[s]leep 120'`.
- [ ] Stop it using `Ctrl+C`.
- [ ] Verify that it has stopped.
- [ ] Answer the 10 knowledge-check questions.

**End of Lesson 01**

**Next Lesson:** Understanding Process IDs (PID) and Parent Process IDs (PPID).