
# PRACTICAL LABS
# MODULE 06 – LINUX SECURITY

## LAB 08: LINUX SERVICE SECURITY

### 1. Introduction

A Linux service is a background program that provides functionality to the operating system or other applications.

Examples include:

- SSH service.
- Web server.
- DNS service.
- Database service.
- Firewall service.
- Time synchronization service.
- Logging service.

Services are important because they allow Linux systems to perform tasks continuously.

However, unnecessary or poorly configured services can increase the attack surface of a computer.

**Service security** involves identifying, monitoring, and protecting services to reduce security risks.

### 2. Understanding the Attack Surface

An attack surface is the collection of components through which an attacker could potentially interact with a system.

Examples include:

- Network services.
- Open ports.
- Running applications.
- User accounts.
- Remote administration interfaces.
- Exposed APIs.

Consider this example:

```text
Linux Computer
     |
     |---- SSH Service
     |
     |---- Web Server
     |
     |---- Database Service
     |
     |---- DNS Service
```

Each service may introduce additional security requirements.

The goal of hardening is not to disable everything. It is to ensure that only necessary services operate with appropriate protections.

### 3. Understand systemd

Most modern Linux distributions use systemd to manage system services.

Systemd can:

- Start services.
- Stop services.
- Restart services.
- Monitor service status.
- Manage startup behavior.
- Record service events.
- Control service dependencies.

The `systemctl` command is commonly used to interact with systemd.

### 4. Create Your Lab Directory

Run:

```bash
mkdir -p ~/practical-labs/module06/lab08
cd ~/practical-labs/module06/lab08
```

### 5. Inspect Running Services

Run:

```bash
systemctl list-units --type=service --state=running --no-pager
```

This displays currently running services.

Observe:

- Service name.
- Load state.
- Active state.
- Sub-state.
- Description.

A running service is not automatically a security vulnerability.

You must understand what it does before deciding whether it needs attention.

### 6. Inspect All Loaded Services

Run:

```bash
systemctl list-units --type=service --all --no-pager
```

This includes loaded services that are not currently running.

Learn the meaning of common service states:

| State | Meaning |
|---|---|
| active | Service is active |
| inactive | Service is not active |
| failed | Service encountered a failure |
| activating | Service is starting |
| deactivating | Service is stopping |

### 7. Inspect Installed Service Definitions

Run:

```bash
systemctl list-unit-files --type=service --no-pager
```

This lists installed service unit files and their enablement states.

Common enablement states:

| State | Meaning |
|---|---|
| enabled | Configured to start automatically |
| disabled | Not configured for automatic startup |
| static | Cannot be enabled directly in the usual way |
| masked | Prevented from being started through systemd |
| generated | Created dynamically |

Remember:

**Running and enabled are different concepts.**

A service can be running without being enabled for automatic startup.

A service can also be enabled but currently inactive.

### 8. Inspect an Individual Service

Use SSH as an example:

```bash
systemctl status ssh --no-pager
```

If SSH is not installed, the command may report that the unit cannot be found.

You can also inspect the system logging service:

```bash
systemctl status systemd-journald --no-pager
```

Record the following:

- Service name.
- Current state.
- Whether it is active.
- Whether it has failed.
- Any useful status information.

### 9. Inspect Service Dependencies

Services may depend on other services.

Run:

```bash
systemctl list-dependencies ssh.service --no-pager
```

If SSH is unavailable, inspect another service:

```bash
systemctl list-dependencies systemd-journald.service --no-pager
```

Dependencies help administrators understand what might be affected by a service change.

Never disable a service simply because you do not recognize its name.

### 10. Inspect Listening Network Services

Services that listen on network interfaces may accept connections from other systems.

Run:

```bash
sudo ss -tulpn
```

The command displays:

- TCP listening sockets.
- UDP listening sockets.
- Local addresses.
- Ports.
- Associated processes where available.

For listening TCP sockets only:

```bash
sudo ss -ltnp
```

For listening UDP sockets only:

```bash
sudo ss -lunp
```

Pay attention to the local address.

Examples:

```text
127.0.0.1:PORT
0.0.0.0:PORT
[::1]:PORT
[::]:PORT
```

General interpretation:

- `127.0.0.1`: IPv4 loopback.
- `0.0.0.0`: All IPv4 interfaces.
- `::1`: IPv6 loopback.
- `[::]`: All IPv6 interfaces.

A service listening on all interfaces may be reachable from other network devices, depending on firewall rules and network configuration.

Do not assume that every listening socket is exposed to the internet.

### 11. Inspect Service Resource Usage

Run:

```bash
systemd-cgtop
```

This provides a live view of resource usage by systemd control groups.

Press `q` to exit.

You can also inspect processes:

```bash
ps aux --sort=-%mem | head -15
```

And:

```bash
ps aux --sort=-%cpu | head -15
```

These commands help identify processes consuming significant system resources.

Unexpected resource usage may justify further investigation.

### 12. Inspect a Service's Configuration

Systemd service definitions are commonly stored in:

```text
/etc/systemd/system/
/lib/systemd/system/
/usr/lib/systemd/system/
```

Inspect the systemd unit search path:

```bash
systemctl show -p UnitPath
```

Inspect the configuration of a known service:

```bash
systemctl cat systemd-journald.service
```

This displays the service unit configuration where available.

Look for:

- Service execution commands.
- User and group settings.
- Dependencies.
- Restart policies.
- Security restrictions.
- Additional configuration overrides.

Do not modify system service definitions during this lab.

### 13. Understand Service Hardening Options

Systemd provides security-related controls for services.

Examples include:

| Setting | Purpose |
|---|---|
| `NoNewPrivileges` | Prevents gaining additional privileges through certain execution mechanisms |
| `PrivateTmp` | Provides a private temporary directory |
| `ProtectSystem` | Restricts write access to system directories |
| `ProtectHome` | Restricts access to home directories |
| `ReadOnlyPaths` | Makes selected paths read-only |
| `PrivateDevices` | Restricts access to device nodes |
| `RestrictAddressFamilies` | Limits available network address families |

These controls must be applied carefully because they may affect application functionality.

### 14. Inspect Service Security Properties

If systemd supports the command, inspect the security posture of a service:

```bash
systemd-analyze security systemd-journald.service
```

The command may produce a list of security properties and an overall exposure assessment.

A higher exposure score does not automatically mean that the service is vulnerable.

Interpret the result in the context of the service's purpose and requirements.

If the command is unavailable, record that the feature is not supported by your installed systemd version.

### 15. Understand Service Logs

Service logs help administrators investigate:

- Service failures.
- Unexpected restarts.
- Authentication events.
- Configuration errors.
- Application crashes.
- Suspicious activity.

Inspect recent events for SSH:

```bash
sudo journalctl -u ssh -n 30 --no-pager
```

Inspect recent system logging events:

```bash
sudo journalctl -u systemd-journald -n 30 --no-pager
```

Display logs from the current boot:

```bash
sudo journalctl -b -n 50 --no-pager
```

A service that does not exist may have no corresponding journal entries.

### 16. Understand Service Hardening Decisions

Before changing a service, ask:

1. What is the service used for?
2. Is it required by the operating system?
3. Does another service depend on it?
4. Does it listen on a network interface?
5. Is it exposed beyond the local computer?
6. Is it running with unnecessary privileges?
7. Is it regularly updated?
8. Are its logs monitored?
9. Is there a documented recovery procedure?
10. Has the proposed change been tested?

This process prevents accidental service disruption.

### 17. Service Management Commands – Theory

The following commands are useful for administrators.

Check service status:

```bash
systemctl status SERVICE_NAME
```

Start a service:

```bash
sudo systemctl start SERVICE_NAME
```

Stop a service:

```bash
sudo systemctl stop SERVICE_NAME
```

Restart a service:

```bash
sudo systemctl restart SERVICE_NAME
```

Enable automatic startup:

```bash
sudo systemctl enable SERVICE_NAME
```

Disable automatic startup:

```bash
sudo systemctl disable SERVICE_NAME
```

These commands are provided for learning.

**Do not execute the start, stop, restart, enable, or disable commands during this lab.**

Your task is to inspect and document services, not change the system's operational configuration.

### 18. Build a Linux Service Security Audit Script

Create:

```bash
nano service-security-audit.sh
```

Enter:

```bash
#!/bin/bash

LAB="$HOME/practical-labs/module06/lab08"
REPORT="$LAB/service-security-report.txt"

{
    echo "LINUX SERVICE SECURITY AUDIT"
    echo "============================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo "User: $(whoami)"
    echo

    echo "RUNNING SERVICES"
    systemctl list-units --type=service --state=running --no-pager
    echo

    echo "SERVICE UNIT FILES"
    systemctl list-unit-files --type=service --no-pager
    echo

    echo "LISTENING TCP SOCKETS"
    sudo ss -ltnp
    echo

    echo "LISTENING UDP SOCKETS"
    sudo ss -lunp
    echo

    echo "SYSTEMD UNIT SEARCH PATH"
    systemctl show -p UnitPath
    echo

    echo "SSH SERVICE STATUS"
    systemctl status ssh --no-pager 2>&1 | head -20
    echo

    echo "JOURNALD SERVICE STATUS"
    systemctl status systemd-journald --no-pager 2>&1 | head -20
    echo

    echo "RECENT SYSTEM EVENTS"
    sudo journalctl -b -n 30 --no-pager

} > "$REPORT" 2>&1

echo "Service security report generated:"
echo "$REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x service-security-audit.sh
```

Run:

```bash
./service-security-audit.sh
```

Inspect:

```bash
less service-security-report.txt
```

Press `q` to exit.

### 19. Analyze Your Findings

Answer these questions:

1. How many running services did you observe?
2. What is systemd?
3. What is the difference between an active and enabled service?
4. Which service did you inspect individually?
5. What is the purpose of service dependencies?
6. What is a listening network socket?
7. What is the difference between `127.0.0.1` and `0.0.0.0`?
8. Why can unnecessary network services increase the attack surface?
9. What does `systemd-analyze security` help administrators understand?
10. Why should administrators inspect dependencies before disabling a service?
11. What is the purpose of service logs?
12. Name five systemd service-hardening options.

### 20. Lab Documentation

Create:

```bash
nano lab08-report.txt
```

Record:

```text
MODULE 06 – LAB 08
LINUX SERVICE SECURITY

1. Number of running services observed:
2. Service inspected:
3. Service state:
4. Is the service enabled?
5. What is systemd?
6. What is a service dependency?
7. Number of TCP listening sockets observed:
8. Number of UDP listening sockets observed:
9. Services listening on loopback:
10. Services listening on all interfaces:
11. Was systemd-analyze security available?
12. Five service-hardening options:
13. Why should unnecessary services be reviewed?
14. Why should service logs be monitored?
15. Three recommendations for improving service security:

SERVICE AUDIT REPORT:
Generated successfully: YES / NO

SYSTEM CONFIGURATION CHANGED:
NO

LAB STATUS:
COMPLETED / NOT COMPLETED
```

### 21. Completion Checklist

- [ ] Understood Linux services.
- [ ] Learned about systemd.
- [ ] Inspected running services.
- [ ] Inspected installed service definitions.
- [ ] Checked an individual service.
- [ ] Examined service dependencies.
- [ ] Inspected TCP listening sockets.
- [ ] Inspected UDP listening sockets.
- [ ] Reviewed service resource usage.
- [ ] Examined service configuration.
- [ ] Learned systemd security controls.
- [ ] Inspected service security properties where supported.
- [ ] Reviewed service logs.
- [ ] Generated the service security audit report.
- [ ] Completed lab documentation.
- [ ] Left system service configuration unchanged.

**LAB 08 STATUS: READY FOR PRACTICAL COMPLETION**
