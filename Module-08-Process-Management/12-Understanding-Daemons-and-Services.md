# MODULE 08: PROCESS MANAGEMENT

## LESSON 12: UNDERSTANDING DAEMONS AND SERVICES

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a daemon is.
2. Understand the difference between daemons and ordinary processes.
3. Explain what Linux services are.
4. Understand the role of systemd.
5. Identify common Linux daemons.
6. Inspect background processes.
7. Understand how daemons support Linux administration and cybersecurity.

---

### 2. What Is a Daemon?

A **daemon** is a background process that performs a particular task or provides a service without requiring continuous direct interaction from a user.

Daemons commonly start automatically during system boot or when needed by another application.

They can continue running while users perform other activities.

For example, a Linux system may have daemons responsible for:

- Managing network connections.
- Scheduling tasks.
- Logging system events.
- Providing SSH remote access.
- Managing printing.
- Resolving domain names.

A daemon is still a process. It has a PID, consumes system resources, and is managed by the Linux operating system.

### 3. Why Are Daemons Important?

Imagine a Linux computer without background services.

You might have to manually start network management, logging, and other essential functions every time you use the computer.

Daemons help automate these activities.

They allow Linux to provide services continuously and efficiently.

### 4. Understanding the Name Daemon

The term daemon comes from computing terminology for a background program that performs tasks automatically.

In Linux and Unix systems, daemon names traditionally often end with the letter `d`.

Examples:

| Daemon | Purpose |
|---|---|
| `sshd` | Provides SSH remote access |
| `cron` | Runs scheduled tasks |
| `rsyslogd` | Handles system logging |
| `systemd` | System and service manager |
| `NetworkManager` | Manages network connections |
| `cupsd` | Manages printing |

Not every daemon ends in `d`, and not every process ending in `d` is necessarily a daemon.

### 5. What Is a Linux Service?

A **service** is a system capability or background function that can be managed by the operating system.

For example:

- SSH service.
- Networking service.
- Logging service.
- Time synchronization service.

On modern Linux distributions such as Kali Linux, systemd commonly manages system services.

A service is often implemented by one or more processes.

Therefore:

- A process is a running instance of a program.
- A daemon is a type of background process.
- A service is a managed system function that may be provided by one or more processes.

These terms are related, but they are not interchangeable.

### 6. Understanding systemd

`systemd` is a system and service manager used by many modern Linux distributions.

It is commonly responsible for:

1. Starting system services during boot.
2. Managing service dependencies.
3. Monitoring service states.
4. Starting and stopping services.
5. Managing system targets.
6. Supporting logging through journald.

On a typical systemd-based Linux installation, systemd runs as PID 1.

Check it using:

```bash
ps -p 1 -o pid,comm,args
```

You may see output showing:

```text
PID  COMMAND
1    systemd
```

The exact output can differ depending on your environment.

### 7. Understanding Service Units

Systemd uses unit files to describe resources it manages.

Common unit types include:

| Unit type | Purpose |
|---|---|
| `.service` | System service |
| `.socket` | Socket activation |
| `.timer` | Scheduled activation |
| `.mount` | Filesystem mount |
| `.target` | Groups units into a system state |
| `.path` | Activates units when filesystem paths change |

For this lesson, we will concentrate on `.service` units.

A service unit contains configuration describing how systemd should manage a service.

### 8. Identifying Running Daemons

You can inspect background processes using:

```bash
ps aux
```

To search for a specific process:

```bash
pgrep -a sshd
```

To inspect processes with a name ending in `d`:

```bash
ps -eo pid,ppid,stat,comm | grep d
```

Be aware that this last command is only a rough text search. It does not reliably identify all daemons.

A better approach is to inspect known services using systemd.

### 9. Understanding Service Status

Use the following command to inspect a service:

```bash
systemctl status SERVICE_NAME
```

For example:

```bash
systemctl status ssh
```

The output may contain:

- Loaded: whether the unit file was found and loaded.
- Active: whether the service is running or stopped.
- Main PID: the main process ID.
- Tasks: number of tasks associated with the service.
- Logs: recent service messages.

A service may not be installed or available on every Kali Linux installation.

### 10. Common Service States

| State | Meaning |
|---|---|
| active (running) | Service is currently running |
| active (exited) | Service completed its configured task successfully |
| inactive (dead) | Service is not currently active |
| failed | Service encountered a failure |
| activating | Service is starting |
| deactivating | Service is stopping |

A service showing `active (exited)` is not necessarily broken. Some one-shot services complete their task and exit successfully.

### 11. Understanding Daemons and Process Management

Daemons can be inspected using the same process-management commands you have already learned.

Examples:

Inspect all processes:

```bash
ps -ef
```

Search for SSH:

```bash
pgrep -a sshd
```

Inspect a process:

```bash
ps -p PID -o pid,ppid,stat,comm
```

Inspect resource usage:

```bash
top
```

The important point is that daemons are processes and can be investigated using ordinary Linux process tools.

However, system services should normally be managed through their service manager rather than by directly terminating their processes.

### 12. Daemons Versus Services

| Feature | Daemon | Service |
|---|---|---|
| Meaning | Background process | Managed system function |
| Main role | Performs background work | Provides a capability |
| Process required? | Yes, it is a process | Often, but not always continuously |
| Management | Process and service tools | Commonly systemd |
| Example | `sshd` process | SSH service |

### 13. Practical Laboratory: Exploring Daemons and Services

**Objective:** Identify systemd, inspect processes, and explore service states without modifying system services.

#### Step 1: Create a laboratory directory

```bash
mkdir -p ~/module08-lab/daemons-services
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/daemons-services
```

#### Step 3: Identify PID 1

```bash
ps -p 1 -o pid,ppid,comm,args
```

Observe which process manages system initialization.

#### Step 4: Inspect running processes

```bash
ps -ef
```

Look for background processes.

#### Step 5: Inspect the systemd version

```bash
systemctl --version
```

This command works on systems that use systemd.

#### Step 6: List running services

```bash
systemctl list-units --type=service --state=running
```

Observe the services currently running.

#### Step 7: Inspect a service

```bash
systemctl status cron
```

If `cron` is not installed or the unit is unavailable, inspect another service listed in Step 6.

#### Step 8: Inspect failed services

```bash
systemctl --failed
```

This displays services or other units that have failed.

An empty list is a normal result.

#### Step 9: Search for a daemon process

```bash
pgrep -a systemd
```

Also try:

```bash
pgrep -a cron
```

A process may not appear if the corresponding service is not installed or running.

#### Step 10: Compare process and service information

Choose one running service from Step 6.

Inspect its status:

```bash
systemctl status SERVICE_NAME
```

Then identify its processes using the PID or process name shown in the output.

**Important:** This laboratory is observational. Do not stop, disable, or terminate system services during this exercise.

### 14. Cybersecurity Applications

Daemons and services are important in cybersecurity because they provide functionality that may also introduce security exposure.

**1. Attack surface management**

Every unnecessary network-facing service may increase a system's attack surface.

**2. Service enumeration**

Security professionals inspect running services to understand what a system provides.

**3. SSH security**

The SSH daemon allows remote access and should be configured securely.

**4. Incident response**

Unexpected background services may require investigation.

**5. System hardening**

Administrators should review services and disable unnecessary ones only after understanding their dependencies and operational requirements.

**6. Log monitoring**

Logging daemons help preserve records that can support security investigations.

### 15. Lesson Summary

In this lesson, we learned:

1. A daemon is a background process.
2. Daemons perform tasks without continuous user interaction.
3. A service is a managed system function.
4. systemd manages services on many modern Linux distributions.
5. systemd commonly runs as PID 1.
6. Service units describe resources managed by systemd.
7. `systemctl status` inspects service state.
8. `systemctl list-units` displays loaded units.
9. `ps` and `pgrep` help inspect daemon processes.
10. Unnecessary services should be reviewed carefully as part of system hardening.

### 16. Knowledge Check

Answer these questions in your own words.

1. What is a daemon?
2. Why do Linux systems use daemons?
3. What is a Linux service?
4. What is the difference between a daemon and a service?
5. What is systemd?
6. What is the usual PID of systemd?
7. What is a `.service` unit?
8. What does `systemctl status` display?
9. Why should administrators inspect unnecessary services?
10. Why should system services normally be managed through systemd instead of directly killing their processes?

### 17. Practical Assignment

- [ ] Create `~/module08-lab/daemons-services`.
- [ ] Identify PID 1 using `ps`.
- [ ] Inspect running processes using `ps -ef`.
- [ ] Check the systemd version.
- [ ] List running services.
- [ ] Inspect one available service.
- [ ] Display failed services.
- [ ] Search for a daemon using `pgrep`.
- [ ] Explain the difference between a daemon and a service.
- [ ] Complete all 10 knowledge-check questions.

**End of Lesson 12**

**Next Lesson:** Managing Services with `systemctl`.