
# PRACTICAL LABS
# MODULE 04 – LINUX SYSTEM ADMINISTRATION

## LAB 04: SYSTEMD AND SERVICE MANAGEMENT

### 1. Lab Overview

Linux uses background programs called **services** or **daemons** to provide essential system functionality.

Examples include:

- SSH – remote system access.
- Cron – scheduled task execution.
- NetworkManager – network connection management.
- Apache – web server functionality.
- Docker – container management.

On modern Linux distributions, including Kali Linux, systemd is commonly used to initialize the system and manage services.

As a system administrator, understanding systemd helps you monitor services, investigate failures, and maintain system security.

**Lab environment:** Your Kali Linux virtual machine.

**Safety:** This lab focuses mainly on inspection. Do not disable essential services or change your VM's network configuration.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain systemd.
2. Understand systemd units.
3. Inspect running services.
4. Check service status.
5. Understand service startup behavior.
6. Read service logs using journalctl.
7. Investigate service failures.
8. Generate a service monitoring report.

### 3. What Is systemd?

Systemd is a Linux system and service manager.

It is responsible for several activities:

- Starting system services during boot.
- Managing background services.
- Monitoring service states.
- Handling system startup targets.
- Managing system logs through journald.
- Coordinating dependencies between services.

Check whether systemd is running:

```bash
ps -p 1 -o pid,comm,args
```

On a system using systemd, PID 1 should normally identify `systemd`.

Check its version:

```bash
systemctl --version
```

### 4. Understanding systemd Units

Systemd manages different types of resources called units.

Common unit types:

| Unit type | Purpose |
|---|---|
| `.service` | Background services |
| `.socket` | Socket-based activation |
| `.target` | Groups of units |
| `.timer` | Scheduled activation |
| `.mount` | Filesystem mount points |
| `.automount` | Automatic mounting |
| `.device` | Device management |

Examples:

```bash
systemctl list-units --type=service
```

Display loaded service units.

List all available service unit files:

```bash
systemctl list-unit-files --type=service
```

### 5. Inspect Service Status

The `systemctl status` command displays information about a service.

Inspect the cron service:

```bash
systemctl status cron
```

Possible service states include:

| State | Meaning |
|---|---|
| active (running) | Service is running |
| active (exited) | Service completed its startup task |
| inactive | Service is not currently running |
| failed | Service encountered an error |
| activating | Service is starting |
| deactivating | Service is stopping |

Check another service:

```bash
systemctl status NetworkManager
```

Note that service names and availability may differ between Linux distributions.

### 6. List Running Services

Display currently active services:

```bash
systemctl list-units --type=service --state=running
```

Display failed services:

```bash
systemctl --failed
```

Display all loaded service units:

```bash
systemctl list-units --type=service --all
```

Count active services:

```bash
systemctl list-units --type=service --state=running --no-legend | wc -l
```

### 7. Understand Service Startup Behavior

A service can be running now without necessarily being configured to start automatically at boot.

Check whether a service is enabled:

```bash
systemctl is-enabled cron
```

Check whether it is active:

```bash
systemctl is-active cron
```

These commands answer different questions:

- `is-enabled` checks startup configuration.
- `is-active` checks current runtime state.

A service may be enabled but inactive, or active but disabled.

### 8. Inspect Service Dependencies

Services may depend on other system components.

Inspect cron dependencies:

```bash
systemctl list-dependencies cron
```

Display reverse dependencies:

```bash
systemctl list-dependencies --reverse cron
```

Inspect a service's configuration:

```bash
systemctl cat cron
```

This displays the unit file and any applicable overrides.

### 9. Understanding Service Management Commands

The following commands are useful for administration.

| Command | Purpose |
|---|---|
| `systemctl status NAME` | Inspect service status |
| `systemctl start NAME` | Start a service |
| `systemctl stop NAME` | Stop a service |
| `systemctl restart NAME` | Restart a service |
| `systemctl reload NAME` | Reload configuration when supported |
| `systemctl enable NAME` | Enable startup at boot |
| `systemctl disable NAME` | Disable startup at boot |
| `systemctl is-active NAME` | Check runtime state |
| `systemctl is-enabled NAME` | Check startup configuration |

For this lab, focus on the inspection commands. Do not execute start, stop, restart, enable, or disable against system services.

### 10. Introduction to journalctl

Systemd commonly works with the systemd journal, which stores system and service logs.

The `journalctl` command allows administrators to inspect these records.

Display recent journal entries:

```bash
journalctl -n 20
```

Display logs from the current boot:

```bash
journalctl -b
```

Display logs from the previous boot, if available:

```bash
journalctl -b -1
```

Display logs associated with cron:

```bash
journalctl -u cron
```

Display recent cron logs:

```bash
journalctl -u cron -n 20
```

Follow logs in real time:

```bash
journalctl -f
```

Exit using `CTRL + C`.

### 11. Investigate Service Errors

A system administrator should know how to investigate a service that fails.

Start with:

```bash
systemctl --failed
```

Inspect the service:

```bash
systemctl status SERVICE_NAME
```

Read its recent logs:

```bash
journalctl -u SERVICE_NAME -n 50
```

Inspect the service's unit configuration:

```bash
systemctl cat SERVICE_NAME
```

Replace `SERVICE_NAME` with the actual service name.

Look for:

- Permission errors.
- Missing configuration files.
- Missing dependencies.
- Port conflicts.
- Incorrect executable paths.
- Failed startup commands.

Never assume that a failed service is malicious. Investigate the evidence first.

### 12. Security Application: Service Auditing

Unnecessary or unexpected services can increase a system's attack surface.

A security administrator should review:

1. Which services are running?
2. Which services listen for network connections?
3. Which services start automatically?
4. Which user runs each service?
5. Are any services failing unexpectedly?
6. Are service configurations appropriate?

List listening network sockets:

```bash
sudo ss -tulpen
```

Important options:

- `-t` – TCP sockets.
- `-u` – UDP sockets.
- `-l` – Listening sockets.
- `-p` – Show process information.
- `-e` – Show extended information.
- `-n` – Display numerical addresses and ports.

Inspect service-related processes:

```bash
ps aux | grep -E 'cron|ssh|apache|nginx'
```

The result depends on which applications are installed and running.

**Security note:** A listening port is not automatically a vulnerability. Evaluate the service, exposure, configuration, and access controls.

### 13. Create a Service Monitoring Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module04/lab04
```

Create a script:

```bash
nano ~/practical-labs/module04/lab04/service-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module04/lab04/service-report.txt"

{
    echo "LINUX SERVICE MONITORING REPORT"
    echo "================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "SYSTEMD VERSION"
    systemctl --version | head -n 1
    echo

    echo "RUNNING SERVICES"
    systemctl list-units \
        --type=service \
        --state=running \
        --no-pager
    echo

    echo "FAILED SERVICES"
    systemctl --failed --no-pager
    echo

    echo "CRON SERVICE STATUS"
    systemctl status cron --no-pager
    echo

    echo "LISTENING NETWORK SOCKETS"
    ss -tuln
    echo

} > "$REPORT" 2>&1

echo "Report generated: $REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x ~/practical-labs/module04/lab04/service-report.sh
```

Execute it:

```bash
~/practical-labs/module04/lab04/service-report.sh
```

Read the report:

```bash
cat ~/practical-labs/module04/lab04/service-report.txt
```

### 14. Practical Exercises

**Exercise 1 – Identify systemd**

Run:

```bash
ps -p 1 -o pid,comm,args
systemctl --version
```

Record the PID 1 process and systemd version.

**Exercise 2 – Service Inspection**

Run:

```bash
systemctl status cron
systemctl status NetworkManager
```

Record the state of each service.

**Exercise 3 – Service Enumeration**

Run:

```bash
systemctl list-units --type=service --state=running
systemctl --failed
```

Identify the number of active and failed services.

**Exercise 4 – Startup Configuration**

Run:

```bash
systemctl is-enabled cron
systemctl is-active cron
```

Explain the difference between the two results.

**Exercise 5 – Log Investigation**

Run:

```bash
journalctl -u cron -n 20
journalctl -b -n 30
```

Identify any useful service or system messages.

**Exercise 6 – Network Service Inspection**

Run:

```bash
sudo ss -tulpen
```

Identify any listening TCP or UDP ports and their associated processes where available.

**Exercise 7 – Report Generation**

Execute your service monitoring script and inspect the report.

### 15. Lab Documentation

Create:

```bash
nano ~/practical-labs/module04/lab04/lab04-report.txt
```

Record:

```text
MODULE 04 – LAB 04
SYSTEMD AND SERVICE MANAGEMENT

1. PID 1 process:
2. Systemd version:
3. First service inspected:
4. First service state:
5. Second service inspected:
6. Number of running services:
7. Number of failed services:
8. Difference between active and enabled:
9. What journalctl does:
10. What I learned:
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 16. Knowledge Check

1. What is systemd?
2. What is a systemd unit?
3. What is the purpose of a `.service` unit?
4. What is the difference between `active` and `enabled`?
5. Which command lists running services?
6. What does `systemctl --failed` display?
7. What is the purpose of journalctl?
8. Why should administrators inspect service logs?
9. What is a listening network socket?
10. How can unnecessary services increase the attack surface?

### 17. Lab Completion Checklist

- [ ] Identified systemd.
- [ ] Inspected systemd units.
- [ ] Checked service status.
- [ ] Listed running services.
- [ ] Checked failed services.
- [ ] Inspected startup configuration.
- [ ] Examined service dependencies.
- [ ] Read system and service logs.
- [ ] Inspected listening network sockets.
- [ ] Generated the service monitoring report.
- [ ] Completed lab documentation.

**LAB 04 STATUS: READY FOR PRACTICAL COMPLETION**
