---
---

# Lesson 08: Init Systems and systemd

## 1. Introduction

When Linux finishes loading its kernel and mounting the root filesystem, it needs a mechanism to start and manage the programs required by the operating system.

This responsibility belongs to the **init system**.

An init system is responsible for starting essential services, managing system processes, and coordinating the operating system's userspace environment.

Modern Kali Linux installations normally use **systemd** as their init system.

In this lesson, you will learn:

- What an init system is.
- The history of Linux init systems.
- What systemd does.
- How Linux services work.
- How to inspect, start, stop, and restart services.
- How to investigate service failures.
- Why service management matters in cybersecurity.

## 2. What Is an Init System?

An init system is the first major userspace process started by the Linux kernel.

It normally runs with Process ID 1 (PID 1).

Its primary responsibility is to initialize and manage the userspace environment.

### What does an init system do?

An init system can:

1. Start system services.
2. Manage background processes.
3. Coordinate startup tasks.
4. Manage service dependencies.
5. Handle system shutdown.
6. Provide service-management functionality.
7. Help maintain the operating system's running environment.

### Example

Imagine starting your computer.

Linux needs several background services to operate correctly:

- Networking.
- Logging.
- Time synchronization.
- Device management.
- Display management.

The init system coordinates the startup of these components.

Without an appropriate init system, normal Linux startup and service management would be difficult.

## 3. Understanding the History of Linux Init Systems

Linux has used different init systems over time.

Understanding them helps explain why systemd exists.

### A. SysVinit

SysVinit is an older Linux and Unix-style initialization system.

It uses scripts to start and stop services.

These scripts are commonly organized around runlevels.

### B. Upstart

Upstart was developed to improve the startup process by supporting event-based service management.

It was used by some Linux distributions before systemd became widely adopted.

### C. systemd

systemd is a modern init system and service manager.

It provides functionality such as:

- Service management.
- Parallel service startup.
- Dependency management.
- Logging integration.
- System state management.
- Timer-based scheduling.

### Comparison

| Feature | SysVinit | Upstart | systemd |
|---|---|---|---|
| Startup model | Script-based | Event-based | Unit-based |
| Service management | Basic | Event-driven | Advanced |
| Dependency handling | Limited | Event-based | Explicit dependencies |
| Integrated journal | No | No | Yes |
| Common in modern Linux | Less common | Less common | Very common |

Different distributions may use different init systems. systemd is the normal choice for modern Kali Linux installations.

## 4. What Is systemd?

systemd is a system and service manager for Linux.

It is responsible for bringing the system into its configured operating state.

It is commonly the first userspace process started during boot.

Check whether systemd is running as PID 1:

```bash
ps -p 1 -o pid,comm,args
```

A system using systemd should normally show `systemd` as the command.

### Main responsibilities of systemd

1. Starting services.
2. Stopping services.
3. Restarting services.
4. Managing service dependencies.
5. Managing system targets.
6. Collecting and accessing system logs through journald.
7. Coordinating system shutdown and reboot.

## 5. Understanding systemd Units

systemd manages different system resources using objects called **units**.

A unit describes something that systemd can manage.

There are several types of units.

| Unit type | Purpose |
|---|---|
| `.service` | Manages a service or background process |
| `.target` | Groups units into a particular system state |
| `.socket` | Manages socket-based activation |
| `.mount` | Manages filesystem mount points |
| `.timer` | Schedules actions based on time |
| `.device` | Represents a device recognized by systemd |
| `.path` | Monitors filesystem paths |
| `.slice` | Organizes processes into resource-management groups |

### Example: Service unit

A service unit describes how systemd should manage a service.

Some common service units include:

```text
ssh.service
cron.service
NetworkManager.service
```

These are examples. Their availability depends on installed packages and system configuration.

## 6. Understanding Linux Services

A service is a program or background process that provides functionality to the operating system or other applications.

Services often operate without direct user interaction.

### Examples of Linux services

| Service | Purpose |
|---|---|
| SSH | Provides secure remote login |
| cron | Runs scheduled tasks |
| NetworkManager | Manages network connections |
| systemd-journald | Collects system logs |
| Bluetooth | Manages Bluetooth functionality |

Not every service is installed or enabled on every system.

### What is a background service?

A background service runs without requiring the user to keep a terminal window open.

For example, an SSH server can continue accepting authorized remote connections while the user works with other applications.

## 7. Managing Services with systemctl

`systemctl` is the main command-line tool used to interact with systemd.

It allows administrators to inspect and manage system services.

### 7.1 Check the status of a service

Syntax:

```bash
systemctl status service-name
```

Example:

```bash
systemctl status ssh
```

This displays information such as:

- Whether the service is loaded.
- Whether it is enabled.
- Whether it is active.
- Its recent logs.
- Its process information.

The SSH service may not be installed on your Kali system.

### 7.2 Start a service

Syntax:

```bash
sudo systemctl start service-name
```

Example:

```bash
sudo systemctl start ssh
```

This requests that systemd start the SSH service.

Only start services that you need and understand.

### 7.3 Stop a service

Syntax:

```bash
sudo systemctl stop service-name
```

Example:

```bash
sudo systemctl stop ssh
```

This requests that systemd stop the SSH service.

### 7.4 Restart a service

Syntax:

```bash
sudo systemctl restart service-name
```

Example:

```bash
sudo systemctl restart ssh
```

Restarting stops and starts the service again.

This is useful when applying configuration changes that require a restart.

### 7.5 Reload a service

Syntax:

```bash
sudo systemctl reload service-name
```

Some services support reloading their configuration without completely restarting.

Not all services support this operation.

### 7.6 Enable a service

Syntax:

```bash
sudo systemctl enable service-name
```

Enabling a service configures it to start automatically through its associated startup dependencies.

Enabling a service does not necessarily start it immediately.

### 7.7 Disable a service

Syntax:

```bash
sudo systemctl disable service-name
```

Disabling a service removes its normal automatic-start configuration.

Disabling a service does not necessarily stop an already-running service.

### 7.8 Enable and start a service together

```bash
sudo systemctl enable --now service-name
```

This enables the service for future startup and requests that it start immediately.

### Important command distinction

| Command | Meaning |
|---|---|
| `start` | Start a service now |
| `stop` | Stop a service now |
| `restart` | Restart a service |
| `reload` | Reload configuration when supported |
| `enable` | Configure automatic startup |
| `disable` | Remove automatic startup configuration |
| `status` | Inspect service status |

**Security warning:** Do not enable remote-access services simply for practice. Only expose services when necessary and properly secured.

## 8. Listing Services

systemd provides commands for viewing installed and active services.

### List currently running services

```bash
systemctl list-units --type=service --state=running
```

This shows services currently loaded and running.

### List all loaded service units

```bash
systemctl list-units --type=service --all
```

This includes loaded services in different states.

### List installed service unit files

```bash
systemctl list-unit-files --type=service
```

This displays service unit files and their enablement states.

### Understand service states

| State | Meaning |
|---|---|
| active | The unit is currently active |
| inactive | The unit is not currently active |
| failed | The unit encountered a failure |
| enabled | Configured to start automatically |
| disabled | Not configured for normal automatic startup |
| static | Not independently enabled in the usual way |

Remember that `active` and `enabled` describe different things.

A service can be active but disabled, or inactive but enabled.

## 9. Understanding systemd Targets

A target is a systemd unit that groups other units together to represent a particular system state.

Targets help organize the startup process.

They are conceptually similar to runlevels in older SysVinit systems.

### Common systemd targets

| Target | Purpose |
|---|---|
| `poweroff.target` | Shuts down the system |
| `rescue.target` | Provides a rescue environment |
| `multi-user.target` | Provides a multi-user, non-graphical system state |
| `graphical.target` | Provides a graphical system state |
| `reboot.target` | Restarts the system |
| `default.target` | Represents the system's configured default target |

### Check the default target

```bash
systemctl get-default
```

This displays the default target configured for startup.

### Check the current default target's details

```bash
systemctl status default.target
```

### Change the default target

Administrators can change the default target using:

```bash
sudo systemctl set-default multi-user.target
```

This changes future default startup behavior.

**Do not execute this command during the practical lab.** We are studying its purpose, not changing your Kali VM's startup configuration.

## 10. Understanding Service Dependencies

Some services require other services or system components before they can operate correctly.

systemd manages these relationships through dependencies and ordering rules.

### Example

A network-dependent application may need networking to be available before it can perform its intended task.

systemd uses unit relationships to coordinate startup.

Some important dependency directives include:

| Directive | Purpose |
|---|---|
| `Requires=` | Declares a required unit relationship |
| `Wants=` | Declares a weaker dependency |
| `After=` | Specifies startup ordering |
| `Before=` | Specifies that another unit should start later |
| `Conflicts=` | Declares units that should not run together |

### Important distinction

`After=` controls ordering. It does not, by itself, guarantee that the referenced service will be started.

Understanding dependencies is useful when troubleshooting services that fail during startup.

## 11. Understanding Service Logs

Logs help administrators understand what happened when a service started, stopped, or failed.

systemd works with `systemd-journald`, which collects system and service logs.

The main tool for inspecting these logs is `journalctl`.

### 11.1 View all journal entries for a service

```bash
journalctl -u ssh
```

Displays journal entries associated with the SSH service.

### 11.2 View recent service logs

```bash
journalctl -u ssh -n 30
```

Displays the latest 30 entries for that service.

### 11.3 View service logs from the current boot

```bash
journalctl -u ssh -b
```

Displays service logs from the current boot.

### 11.4 Follow logs in real time

```bash
journalctl -u ssh -f
```

This follows new log entries as they appear.

Press `Ctrl + C` to stop following the logs.

### Why are logs important?

Logs can reveal:

- Authentication failures.
- Service startup problems.
- Permission errors.
- Configuration mistakes.
- Unexpected service termination.
- Other operational issues.

## 12. Understanding Failed Services

A service may fail because of:

- Incorrect configuration.
- Missing files.
- Permission problems.
- Port conflicts.
- Missing dependencies.
- Insufficient resources.
- Hardware or networking problems.

### Check failed units

```bash
systemctl --failed
```

This lists units that systemd currently considers failed.

### Inspect a failed service

```bash
systemctl status service-name
```

Replace `service-name` with the actual service name.

### Inspect its logs

```bash
journalctl -u service-name -n 50
```

Review the recent log entries to investigate the problem.

### Troubleshooting approach

1. Identify the failed service.
2. Check its status.
3. Read its recent logs.
4. Identify the likely cause.
5. Review the relevant configuration.
6. Apply a safe correction.
7. Restart the service if appropriate.
8. Verify that it works correctly.

Never restart services repeatedly without investigating why they failed.

## 13. systemd and Cybersecurity

Service management is an important part of Linux security.

Every running service can introduce functionality and potentially expose an attack surface.

An attack surface is the collection of points through which an attacker might interact with a system.

### 13.1 Unnecessary services

Unnecessary services may:
- Open network ports.
- Consume system resources.
- Introduce vulnerabilities.
- Provide additional opportunities for unauthorized access.

### 13.2 Remote access services

Services such as SSH allow remote administration.

If improperly configured, they may expose a system to:
- Password guessing.
- Unauthorized access attempts.
- Weak authentication.
- Exploitation of vulnerable software.

### 13.3 Service misconfiguration

Poorly configured services may run with excessive privileges or expose sensitive information.

### 13.4 Defensive service management

Good security practices include:

1. Run only services that are required.
2. Keep service packages updated.
3. Use strong authentication for remote access.
4. Review services that listen on network interfaces.
5. Investigate failed and unusual services.
6. Review service logs regularly.
7. Avoid running applications with unnecessary root privileges.
8. Protect sensitive service configuration files.

### Inspect listening network sockets

You can use:

```bash
ss -tuln
```

This displays listening TCP and UDP sockets.

It is useful for identifying network services that may be accepting connections.

A listening socket does not automatically indicate a vulnerability. You must understand what application owns it and how it is configured.

## 14. Practical Lab: Managing and Inspecting systemd

### Lab objective

Learn to inspect systemd, list services, understand their states, and investigate service logs.

This lab is primarily read-only. You will not enable or disable services or change your default boot target.

### Step 1: Check PID 1

```bash
ps -p 1 -o pid,comm,args
```

Confirm which process is running as PID 1.

### Step 2: Check systemd version

```bash
systemctl --version
```

Observe the installed systemd version.

### Step 3: Check the default target

```bash
systemctl get-default
```

Record the result.

### Step 4: List running services

```bash
systemctl list-units --type=service --state=running
```

Observe the services currently running.

### Step 5: List all loaded services

```bash
systemctl list-units --type=service --all
```

Compare the output with Step 4.

### Step 6: List installed service unit files

```bash
systemctl list-unit-files --type=service
```

Observe the different enablement states.

### Step 7: Check failed services

```bash
systemctl --failed
```

If no failed services are displayed, that is perfectly fine.

### Step 8: Inspect the networking service

```bash
systemctl status NetworkManager
```

Observe its current state.

### Step 9: Inspect system logs

```bash
journalctl -b -n 40
```

Review recent messages from the current boot.

### Step 10: Inspect listening network sockets

```bash
ss -tuln
```

Observe the listening TCP and UDP sockets.

### Step 11: Inspect a service's logs

Use a service that exists on your system.

For example:

```bash
journalctl -u NetworkManager -n 30
```

Review the latest entries.

### Lab safety

Do not:
- Disable NetworkManager.
- Stop essential system services.
- Change your default target.
- Enable SSH just for practice.
- Modify systemd unit files.
- Disable security services.

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is an init system?
2. What is systemd?
3. Which process ID is normally assigned to the init system?
4. What is a daemon?
5. What is a systemd unit?
6. What is the purpose of a `.service` unit?
7. What is the difference between `start` and `enable`?
8. What is the difference between `stop` and `disable`?
9. Which command displays running services?
10. What is a systemd target?
11. What is the purpose of `journalctl`?
12. How can you identify failed services?
13. Why should unnecessary services be avoided?
14. What is an attack surface?
15. Why is service log monitoring important?

## 16. Practical Assignment

Write a report titled:

**Linux Service Management with systemd**

Include:

1. An explanation of an init system.
2. A comparison of SysVinit and systemd.
3. The role of systemd in Linux.
4. Five examples of systemd unit types.
5. The difference between active and enabled services.
6. Five useful `systemctl` commands.
7. The purpose of `journalctl`.
8. How to investigate a failed service.
9. Why unnecessary services create security risks.
10. Your observations from the practical lab.

Include relevant command outputs.

## 17. Key Terms

| Term | Definition |
|---|---|
| Init system | Software responsible for initializing userspace |
| systemd | Modern Linux system and service manager |
| PID 1 | Process ID normally assigned to the init system |
| Daemon | Background process that provides a service |
| Unit | Resource managed by systemd |
| Service | Background program providing functionality |
| Target | Unit grouping other units into a system state |
| systemctl | Command-line tool for managing systemd |
| journalctl | Tool for inspecting system journal logs |
| journald | systemd's logging service |
| Dependency | Relationship between systemd units |
| Attack surface | Points through which a system may be exposed to attack |

## 18. Exam Summary

Remember these important points:

- An init system initializes and manages the Linux userspace environment.
- systemd is commonly used by modern Linux distributions.
- systemd normally runs as PID 1.
- systemd manages services, targets, and other units.
- `systemctl` is used to inspect and manage services.
- `start` starts a service immediately.
- `enable` configures automatic startup.
- `stop` stops a service.
- `disable` removes normal automatic startup configuration.
- `journalctl` is used to inspect system and service logs.
- `systemctl --failed` identifies failed units.
- Unnecessary services can increase a system's attack surface.
- Monitoring services is an important Linux security practice.

## 19. Conclusion

You have learned how Linux initializes and manages its userspace environment through an init system.

You now understand systemd, service units, targets, service dependencies, and service logs.

You have also learned how to inspect and manage services using `systemctl` and investigate problems using `journalctl`.

These skills are essential for Linux administration, troubleshooting, system hardening, and ethical hacking.

**Next Lesson: Lesson 09 — Understanding the Linux Shell.**