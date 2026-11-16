
# Module 12 – Lesson 02: Managing Linux Services with systemd

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a Linux service is.
2. Understand the role of systemd.
3. Identify the difference between starting and enabling a service.
4. Check service status.
5. Start, stop, restart, and reload services.
6. Inspect service logs.
7. Understand how system administrators manage services securely.

---

## 2. What Is a Linux Service?

A Linux service is a background program that performs a specific task without requiring continuous interaction from the user.

Services are sometimes called **daemons**.

Examples include:

- SSH: allows remote access to a Linux computer.
- Cron: runs scheduled tasks.
- Apache: provides web server functionality.
- MySQL: manages database services.
- NetworkManager: manages network connections.

### Real-world example

Imagine a university website.

For students to access the website, a web server must be running in the background.

The administrator does not need to open the web server manually every time someone visits the website.

The service runs in the background and responds to requests.

---

## 3. What Is systemd?

`systemd` is a system and service manager used by many modern Linux distributions, including Kali Linux.

It is responsible for important startup and system-management operations.

Its responsibilities include:

- Starting system services during boot.
- Stopping services.
- Restarting services.
- Managing service dependencies.
- Monitoring service states.
- Managing system targets.
- Recording service-related events through the journal.

The main command used to interact with systemd is:

```bash
systemctl
```

Think of `systemd` as a manager responsible for coordinating important background services.

---

## 4. Understanding systemctl

`systemctl` allows administrators to control and inspect systemd-managed services.

General syntax:

```bash
systemctl ACTION SERVICE_NAME
```

Examples:

```bash
systemctl status ssh
systemctl start ssh
systemctl stop ssh
```

Some operations require administrative privileges.

For example:

```bash
sudo systemctl restart ssh
```

Use administrative commands only when necessary and only on systems you are authorised to manage.

---

## 5. Checking Service Status

Before changing a service, inspect its current state.

Example:

```bash
systemctl status ssh
```

The output may contain:

- Loaded: whether the service unit was found and loaded.
- Active: whether the service is currently running.
- Main PID: the main process ID.
- Logs: recent messages associated with the service.

### Understanding common states

| State | Meaning |
|---|---|
| `active (running)` | Service is running |
| `inactive (dead)` | Service is not running |
| `failed` | Service encountered an error |
| `activating` | Service is starting |
| `deactivating` | Service is stopping |

A service can be installed but not running.

A service can also be running without being enabled to start automatically at boot.

These are different conditions.

---

## 6. Starting a Service

The `start` operation starts a service immediately.

General syntax:

```bash
sudo systemctl start SERVICE_NAME
```

Example:

```bash
sudo systemctl start ssh
```

This attempts to start the SSH service.

Verify:

```bash
systemctl status ssh
```

If the service is unavailable or not installed, systemctl may report that its unit could not be found.

Do not install additional software just to complete this lesson.

---

## 7. Stopping a Service

The `stop` operation stops a currently running service.

General syntax:

```bash
sudo systemctl stop SERVICE_NAME
```

Example:

```bash
sudo systemctl stop ssh
```

**Security note:** Stopping SSH can disconnect remote users. Only stop it when you understand the consequences and have another way to access the machine.

For this lesson, do not stop important networking, security, or system services.

---

## 8. Restarting a Service

The `restart` operation stops and starts a service again.

General syntax:

```bash
sudo systemctl restart SERVICE_NAME
```

Example:

```bash
sudo systemctl restart ssh
```

Administrators commonly restart services after changing their configuration.

Always inspect the service afterward:

```bash
systemctl status ssh
```

---

## 9. Reloading a Service

Some services support reloading their configuration without fully restarting.

General syntax:

```bash
sudo systemctl reload SERVICE_NAME
```

Example:

```bash
sudo systemctl reload apache2
```

This example applies only if Apache is installed and supports the requested reload operation.

### Restart versus reload

| Restart | Reload |
|---|---|
| Stops and starts the service | Requests a configuration reload |
| May interrupt active connections | Often avoids a full service restart |
| Used when a full restart is required | Used when supported configuration changes can be applied without restarting |

Not every service supports reload.

---

## 10. Understanding enable and disable

A service may be configured to start automatically when Linux boots.

### Enable a service

```bash
sudo systemctl enable SERVICE_NAME
```

This configures automatic startup at boot.

### Disable a service

```bash
sudo systemctl disable SERVICE_NAME
```

This removes its automatic-start configuration.

### Important difference

| Command | Meaning |
|---|---|
| `start` | Starts the service now |
| `stop` | Stops the service now |
| `enable` | Configures automatic startup |
| `disable` | Removes automatic startup configuration |
| `restart` | Restarts the service |
| `reload` | Reloads configuration when supported |

**Remember:** `enable` does not necessarily start a service immediately.

To enable and start a service together:

```bash
sudo systemctl enable --now SERVICE_NAME
```

Use this only when you genuinely want the service to run now and automatically at boot.

---

## 11. Listing Services

To list currently loaded service units:

```bash
systemctl list-units --type=service
```

To list all installed service unit files:

```bash
systemctl list-unit-files --type=service
```

To show only active services:

```bash
systemctl list-units --type=service --state=active
```

To show failed services:

```bash
systemctl --failed
```

These commands are useful when investigating system problems.

---

## 12. Inspecting Service Logs

System administrators use logs to understand what happened when a service starts, stops, or fails.

The command used to inspect systemd journal logs is:

```bash
journalctl
```

To inspect logs for a particular service:

```bash
journalctl -u ssh
```

To display recent service logs:

```bash
journalctl -u ssh -n 20
```

To follow new log messages:

```bash
journalctl -u ssh -f
```

Press `Ctrl + C` to stop following logs.

You can also view logs from the current boot:

```bash
journalctl -b
```

Logs help administrators troubleshoot problems instead of guessing.

---

## 13. Understanding Service Unit Files

systemd uses unit files to describe services and other system resources.

Common unit types include:

| Unit Type | Purpose |
|---|---|
| `.service` | Manages services |
| `.target` | Groups system units |
| `.socket` | Manages socket activation |
| `.timer` | Schedules systemd tasks |
| `.mount` | Manages mount points |

A service unit commonly contains sections such as:

- `[Unit]`: Description and dependencies.
- `[Service]`: How the service runs.
- `[Install]`: Installation and startup configuration.

You can inspect an existing unit without changing it:

```bash
systemctl cat ssh
```

The result depends on whether the SSH service is installed.

Do not edit system service files during this introductory lesson.

---

## 14. Practical Lab: Inspecting systemd Services

### Objective

Investigate services running in your Kali Linux virtual machine.

All exercises in this lesson are inspection-only. You do not need to start, stop, enable, or disable system services.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson02
cd ~/module12-lab/lesson02
```

### Step 2: Check systemd availability

```bash
systemctl --version
```

Record the version displayed.

### Step 3: List loaded services

```bash
systemctl list-units --type=service
```

Observe the service names and their states.

### Step 4: List installed service unit files

```bash
systemctl list-unit-files --type=service
```

Notice the difference between installed unit files and currently loaded service units.

### Step 5: Check active services

```bash
systemctl list-units --type=service --state=active
```

### Step 6: Check failed services

```bash
systemctl --failed
```

No failed services is a perfectly acceptable result.

### Step 7: Inspect a service

Try:

```bash
systemctl status ssh
```

If SSH is not installed, inspect another service from your list instead.

### Step 8: Inspect recent logs

For SSH, if its unit exists:

```bash
journalctl -u ssh -n 10 --no-pager
```

If SSH is unavailable, use another existing service name.

### Step 9: Create your report

```bash
{
echo "SYSTEMD SERVICE INSPECTION REPORT"
echo "================================="
echo ""
echo "Systemd Version:"
systemctl --version
echo ""
echo "Active Services:"
systemctl list-units --type=service --state=active --no-pager
echo ""
echo "Failed Services:"
systemctl --failed --no-pager
} > service-report.txt
```

### Step 10: Read your report

```bash
cat service-report.txt
```

Confirm that the report contains systemd version information, active services, and failed-service information.

---

## 15. Cybersecurity Connection

Service management is an important part of Linux security.

Every unnecessary network-facing service can increase the system's attack surface.

For example:

- An exposed SSH service may attract password-guessing attempts.
- An outdated web server may contain known vulnerabilities.
- An unnecessary database service may expose sensitive information.
- Poor service configuration may allow unauthorised access.

A security administrator should:

1. Identify running services.
2. Understand why each service is needed.
3. Review its configuration.
4. Monitor its logs.
5. Remove or disable unnecessary services only after assessing their dependencies and operational impact.

**Important:** Never disable an unfamiliar service simply because you do not recognise its name.

---

## 16. Lesson Summary

In this lesson, you learned:

1. A Linux service is a background program.
2. systemd manages services and other system units.
3. systemctl is the main command for managing systemd.
4. `start`, `stop`, `restart`, and `reload` control service operations.
5. `enable` and `disable` control automatic startup configuration.
6. `journalctl` helps investigate service events.
7. Service inspection is important for system administration and cybersecurity.

---

## 17. Knowledge Check

1. What is a Linux service?
2. What is systemd?
3. Which command manages systemd services?
4. What does `systemctl status ssh` do?
5. What is the difference between `start` and `enable`?
6. What is the difference between `restart` and `reload`?
7. Which command lists failed services?
8. What is the purpose of journalctl?
9. What is a `.service` unit file?
10. Why should administrators avoid disabling unfamiliar services?

---

## 18. Practical Completion Checklist

- [ ] I understand Linux services.
- [ ] I understand the role of systemd.
- [ ] I checked the systemd version.
- [ ] I listed loaded and installed services.
- [ ] I inspected active and failed services.
- [ ] I inspected service status and logs.
- [ ] I created and viewed `service-report.txt`.
- [ ] I understand why service management matters for cybersecurity.

**Lesson 02 is complete when you understand the concepts and successfully generate your service inspection report.**
