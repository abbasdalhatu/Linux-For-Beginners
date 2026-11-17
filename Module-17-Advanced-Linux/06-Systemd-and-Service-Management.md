---
---


# MODULE 17 – ADVANCED LINUX
## LESSON 06: SYSTEMD AND SERVICE MANAGEMENT

---

## 1. Introduction to systemd

`systemd` is a system and service manager used by many modern Linux distributions, including Kali Linux.

It is responsible for initializing and managing important parts of the operating system.

When Linux starts, systemd commonly becomes the first userspace process, running as PID 1.

It manages:

- System services.
- Background processes.
- Startup targets.
- Mount points.
- Timers.
- System sockets.
- System logging integration.
- Service dependencies.

### What is a service?

A service is a program or process that performs a function in the background.

Examples include:

| Service | Purpose |
|---|---|
| `ssh` | Secure remote login |
| `cron` | Scheduled tasks |
| `NetworkManager` | Network connection management |
| `systemd-journald` | System logging |
| `cups` | Printing services |

Not every Linux installation has all these services installed or enabled.

### Learning objectives

By the end of this lesson, you should be able to:

1. Explain the purpose of systemd.
2. Understand systemd units.
3. Inspect system services.
4. Start and stop services.
5. Enable and disable services.
6. Understand systemd targets.
7. Inspect service dependencies.
8. Read service logs.
9. Create a custom user service.
10. Troubleshoot service failures.

---

## 2. Understanding systemd Units

A systemd unit is a configuration object that describes a resource managed by systemd.

Different unit types manage different resources.

### Common unit types

| Unit extension | Purpose |
|---|---|
| `.service` | Background services |
| `.timer` | Scheduled tasks |
| `.socket` | Socket-based activation |
| `.target` | Groups of units |
| `.mount` | Filesystem mount points |
| `.automount` | Automatic mounting |
| `.path` | Path-based activation |
| `.device` | Device units |
| `.slice` | Resource-management groups |
| `.scope` | Externally created process groups |

### Display systemd units

```bash
systemctl list-units
```

Display all unit files:

```bash
systemctl list-unit-files
```

Display service units:

```bash
systemctl list-units --type=service
```

Display failed units:

```bash
systemctl --failed
```

### Why units matter

Systemd uses unit files to understand:

- What should be started.
- When it should start.
- What it depends on.
- How it should behave.
- What should happen when it stops or fails.

---

## 3. Checking Service Status

The `systemctl status` command provides information about a service.

Example:

```bash
systemctl status cron
```

It can display:

- Service description.
- Loaded unit file.
- Enabled or disabled state.
- Active or inactive state.
- Main process ID.
- Recent log messages.

### Inspect another service

```bash
systemctl status NetworkManager
```

### Check whether a service is active

```bash
systemctl is-active cron
```

Possible responses include:

```text
active
inactive
failed
unknown
```

### Check whether a service is enabled

```bash
systemctl is-enabled cron
```

This checks whether the service is configured to start automatically through its unit enablement configuration.

### Important distinction

`active` and `enabled` do not mean the same thing.

- Active: The service is currently running.
- Enabled: The service is configured to start automatically under its configured activation conditions.

A service can be active but disabled, or enabled but currently inactive.

---

## 4. Starting and Stopping Services

Systemd provides commands for managing services.

### A. Start a service

```bash
sudo systemctl start SERVICE
```

Example:

```bash
sudo systemctl start cron
```

### B. Stop a service

```bash
sudo systemctl stop SERVICE
```

Example:

```bash
sudo systemctl stop cron
```

### C. Restart a service

```bash
sudo systemctl restart SERVICE
```

This stops and starts the service.

### D. Reload service configuration

```bash
sudo systemctl reload SERVICE
```

This requests that a service reload its configuration without performing a full restart, if supported.

### E. Reload systemd unit definitions

```bash
sudo systemctl daemon-reload
```

This makes systemd reread unit files after changes.

It does not restart services automatically.

### Important warning

Do not stop important services randomly.

For example, stopping networking services can interrupt your network connection. Always inspect a service and understand its role before changing it.

---

## 5. Enabling and Disabling Services

Starting a service does not necessarily configure it to start automatically after reboot.

That is the purpose of enabling a service.

### Enable a service

```bash
sudo systemctl enable SERVICE
```

### Disable a service

```bash
sudo systemctl disable SERVICE
```

### Enable and start immediately

```bash
sudo systemctl enable --now SERVICE
```

### Disable and stop immediately

```bash
sudo systemctl disable --now SERVICE
```

### Example

Inspect cron:

```bash
systemctl status cron
```

If you need it to start automatically:

```bash
sudo systemctl enable cron
```

### Understanding the difference

| Command | Current state | Future startup |
|---|---|---|
| `start` | Starts service | No automatic startup change |
| `stop` | Stops service | No automatic startup change |
| `enable` | Does not necessarily start it | Configures automatic startup |
| `disable` | Does not necessarily stop it | Removes automatic startup enablement |
| `restart` | Restarts service | No automatic startup change |

---

## 6. Understanding systemd Targets

A target is a systemd unit used to group other units and represent a particular system state.

Targets are somewhat similar to traditional Linux runlevels.

### Common targets

| Target | Purpose |
|---|---|
| `poweroff.target` | Shut down system |
| `rescue.target` | Rescue environment |
| `multi-user.target` | Multi-user non-graphical environment |
| `graphical.target` | Graphical environment |
| `reboot.target` | Restart system |
| `default.target` | Default startup target |

### Display the default target

```bash
systemctl get-default
```

### List available targets

```bash
systemctl list-units --type=target
```

### Display target status

```bash
systemctl status multi-user.target
```

### Understand target dependencies

```bash
systemctl list-dependencies multi-user.target
```

This displays units pulled in by the selected target.

### Important warning

Do not change your default boot target during this lesson. We will inspect targets without modifying your Kali boot configuration.

---

## 7. Understanding systemd Service Files

Systemd service units are usually stored in directories such as:

```text
/etc/systemd/system/
/usr/lib/systemd/system/
/lib/systemd/system/
```

The exact locations depend on the Linux distribution and filesystem layout.

User-level services are commonly stored in:

```text
~/.config/systemd/user/
```

### Example service file

```ini
[Unit]
Description=Example Linux Service

[Service]
Type=oneshot
ExecStart=/usr/bin/echo "Hello from systemd"

[Install]
WantedBy=default.target
```

### Explanation

**[Unit]**

Contains general information and dependencies.

**[Service]**

Defines how the service executes.

**[Install]**

Defines how the unit is connected to startup targets when enabled.

### Common service options

| Option | Purpose |
|---|---|
| `Description` | Describes the unit |
| `After` | Specifies ordering after another unit |
| `Requires` | Declares a required dependency |
| `Wants` | Declares a weaker dependency |
| `Type` | Defines service execution behaviour |
| `ExecStart` | Defines the start command |
| `ExecStop` | Defines an optional stop command |
| `Restart` | Controls restart behaviour |
| `User` | Selects the service user |
| `WorkingDirectory` | Sets the working directory |

---

## 8. Understanding Service Types

The `Type` setting determines how systemd considers a service to have started.

### Common types

| Type | Description |
|---|---|
| `simple` | Main process starts directly |
| `exec` | Main process is considered started after successful execution |
| `oneshot` | Performs a task and exits |
| `forking` | Traditional daemon forks into the background |
| `notify` | Service signals readiness using systemd notification |

### Example: oneshot service

A oneshot service performs a task and then exits.

Example:

```ini
[Service]
Type=oneshot
ExecStart=/usr/bin/touch /tmp/systemd-example.txt
```

### Example: simple service

A simple service runs a foreground process.

Example:

```ini
[Service]
Type=simple
ExecStart=/usr/bin/sleep infinity
```

The second example is suitable for a temporary demonstration, not a useful production service.

---

## 9. Inspecting Service Dependencies

Services sometimes depend on other services or system resources.

For example, a network application may require networking to be available.

### Display dependencies

```bash
systemctl list-dependencies SERVICE
```

### Display reverse dependencies

```bash
systemctl list-dependencies --reverse SERVICE
```

### Inspect a unit's properties

```bash
systemctl show cron
```

Display one property:

```bash
systemctl show cron -p ActiveState
```

Display another property:

```bash
systemctl show cron -p MainPID
```

### Why dependencies matter

Dependencies help explain:

- Why a service starts.
- What it needs before starting.
- Which units may be affected by failures.
- Why startup ordering matters.

---

## 10. Understanding systemd Logs

Systemd integrates with the journal logging system.

The `journalctl` command allows administrators to inspect logs.

### Display all available journal entries

```bash
journalctl
```

### Display recent entries

```bash
journalctl -n 20
```

### Display logs from the current boot

```bash
journalctl -b
```

### Display logs for a service

```bash
journalctl -u cron
```

### Display recent service logs

```bash
journalctl -u cron -n 30
```

### Follow logs in real time

```bash
journalctl -f
```

Press `CTRL + C` to exit.

### Follow one service

```bash
journalctl -u cron -f
```

### Why logs matter

Logs help administrators investigate:

- Service startup failures.
- Permission errors.
- Configuration mistakes.
- Application crashes.
- Dependency failures.
- Unexpected system behaviour.

---

## 11. Practical Lab: Inspecting systemd

This lab is primarily read-only.

### Step 1: Inspect systemd

```bash
systemctl status
```

Record the PID of the system manager if displayed.

On a typical systemd-based installation, this should be PID 1.

### Step 2: List service units

```bash
systemctl list-units --type=service
```

### Step 3: List failed units

```bash
systemctl --failed
```

### Step 4: Inspect cron

```bash
systemctl status cron
```

### Step 5: Check service activation state

```bash
systemctl is-active cron
```

### Step 6: Check whether cron is enabled

```bash
systemctl is-enabled cron
```

### Step 7: Inspect systemd targets

```bash
systemctl get-default
```

### Step 8: Inspect target dependencies

```bash
systemctl list-dependencies multi-user.target
```

### Step 9: Inspect recent boot logs

```bash
journalctl -b -n 30
```

### Step 10: Inspect cron logs

```bash
journalctl -u cron -n 20
```

Record the results.

---

## 12. Practical Project: Create a User-Level systemd Service

Now we will create a service that writes a timestamp to a file.

It will run once when manually started.

This is a safe user-level demonstration that does not require root privileges.

### Step 1: Create a project directory

```bash
mkdir -p ~/module17-lab/lesson06
mkdir -p ~/.config/systemd/user
```

### Step 2: Create the service file

```bash
nano ~/.config/systemd/user/module17-report.service
```

Enter:

```ini
[Unit]
Description=Module 17 Systemd Report Service

[Service]
Type=oneshot
ExecStart=/bin/bash -c 'echo "Service executed at: $(date)" >> %h/module17-lab/lesson06/service-report.txt'
```

### Step 3: Reload systemd

```bash
systemctl --user daemon-reload
```

### Step 4: Start the service

```bash
systemctl --user start module17-report.service
```

### Step 5: Check its status

```bash
systemctl --user status module17-report.service
```

A oneshot service may show as inactive after successful execution. Check its exit status and logs rather than expecting it to remain running.

### Step 6: Inspect the generated report

```bash
cat ~/module17-lab/lesson06/service-report.txt
```

### Step 7: Inspect service logs

```bash
journalctl --user -u module17-report.service
```

### Step 8: Run it again

```bash
systemctl --user start module17-report.service
```

Check the report again:

```bash
cat ~/module17-lab/lesson06/service-report.txt
```

You should see another timestamp.

### Step 9: Remove the demonstration unit

```bash
rm ~/.config/systemd/user/module17-report.service
```

Reload systemd:

```bash
systemctl --user daemon-reload
```

The generated report remains available for your notes.

---

## 13. Troubleshooting Failed Services

Sometimes services fail to start.

Common causes include:

1. Incorrect configuration.
2. Missing dependencies.
3. Incorrect permissions.
4. Missing executable files.
5. Port conflicts.
6. Resource exhaustion.
7. Invalid unit files.

### Step 1: Check service status

```bash
systemctl status SERVICE
```

### Step 2: Inspect logs

```bash
journalctl -u SERVICE -n 50
```

### Step 3: Validate unit files

```bash
systemd-analyze verify /path/to/service.service
```

Use the actual path to the unit file.

### Step 4: Reload configuration

```bash
sudo systemctl daemon-reload
```

### Step 5: Try starting the service

```bash
sudo systemctl start SERVICE
```

### Step 6: Check status again

```bash
systemctl status SERVICE
```

Do not repeatedly restart a failed production service without understanding the cause.

---

## 14. systemd Security Principles

Systemd can help improve system security when services are configured carefully.

Important principles include:

### A. Least privilege

Run services using dedicated, unprivileged users when possible.

### B. Restrict filesystem access

Systemd offers settings that can limit which parts of the filesystem a service can access.

### C. Restrict privileges

Options such as `NoNewPrivileges=yes` can prevent a service from gaining additional privileges through certain execution paths.

### D. Restrict device access

Systemd supports device-access restrictions for compatible service configurations.

### E. Protect sensitive files

Do not expose credentials or secrets through publicly readable unit files.

### Example hardening options

```ini
[Service]
NoNewPrivileges=yes
PrivateTmp=yes
ProtectSystem=strict
ProtectHome=yes
```

These settings are examples, not universal defaults. Their effects depend on the application and its filesystem requirements.

Always test service compatibility before applying hardening settings to important services.

---

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is systemd?
2. What is a systemd unit?
3. Explain the purpose of `.service`, `.timer`, and `.target` units.
4. What is the difference between `start` and `enable`?
5. What does `daemon-reload` do?
6. What is a systemd target?
7. What is the purpose of `ExecStart`?
8. Explain the difference between `simple` and `oneshot`.
9. What does `journalctl -u` do?
10. What is the purpose of service dependencies?
11. Identify four common causes of service failure.
12. Explain two systemd security-hardening options.

---

## 16. Important Commands Summary

| Command | Purpose |
|---|---|
| `systemctl status` | Display system manager status |
| `systemctl list-units` | List loaded units |
| `systemctl list-unit-files` | List installed unit files |
| `systemctl --failed` | List failed units |
| `systemctl status SERVICE` | Inspect a service |
| `systemctl start SERVICE` | Start a service |
| `systemctl stop SERVICE` | Stop a service |
| `systemctl restart SERVICE` | Restart a service |
| `systemctl reload SERVICE` | Reload service configuration |
| `systemctl enable SERVICE` | Enable automatic startup |
| `systemctl disable SERVICE` | Disable automatic startup |
| `systemctl daemon-reload` | Reload systemd unit definitions |
| `systemctl get-default` | Display default target |
| `systemctl list-timers --all` | Display timers |
| `systemctl list-dependencies TARGET` | Display unit dependencies |
| `systemctl show SERVICE` | Display service properties |
| `journalctl -b` | Display current boot logs |
| `journalctl -u SERVICE` | Display service logs |
| `systemd-analyze verify FILE` | Validate a unit file |

---

## 17. Lesson Summary

In this lesson, you studied systemd and Linux service administration.

You learned:

- What systemd does.
- Systemd unit types.
- Service inspection.
- Starting and stopping services.
- Enabling and disabling services.
- Systemd targets.
- Service configuration files.
- Service types.
- Unit dependencies.
- Journal logging.
- Service troubleshooting.
- Basic service hardening.

You also created a user-level systemd service and inspected its execution.

### Completion checklist

- [ ] Understand systemd.
- [ ] Understand systemd units.
- [ ] Inspect available services.
- [ ] Understand service states.
- [ ] Practise service-management commands.
- [ ] Understand systemd targets.
- [ ] Inspect service dependencies.
- [ ] Read systemd logs.
- [ ] Create a user-level service.
- [ ] Verify service execution.
- [ ] Remove the demonstration unit.
- [ ] Understand systemd security principles.
- [ ] Answer the knowledge-check questions.

**END OF LESSON 06**
