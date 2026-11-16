
# LINUX SERVICES AND SYSTEMD

## 1. What Is a Linux Service?

A service is a program that runs in the background to provide a function to the operating system or other applications.

Examples:
- SSH service.
- Web server.
- Database service.
- Network management service.

## 2. What Is systemd?

systemd is a system and service manager used by many modern Linux distributions.

It manages:
- System startup.
- Background services.
- System targets.
- Service dependencies.
- System logs through journald.

## 3. Check systemd

Check whether systemd is running as the system manager:

```bash
systemctl status
```

Display the systemd version:

```bash
systemctl --version
```

## 4. View Running Services

List active services:

```bash
systemctl list-units --type=service --state=running
```

List all service units:

```bash
systemctl list-units --type=service
```

Check the status of a service:

```bash
systemctl status ssh
```

The service name may differ depending on the Linux distribution.

## 5. Start and Stop Services

Start a service:

```bash
sudo systemctl start ssh
```

Stop a service:

```bash
sudo systemctl stop ssh
```

Restart a service:

```bash
sudo systemctl restart ssh
```

Reload a service's configuration:

```bash
sudo systemctl reload ssh
```

## 6. Enable and Disable Services

Enable a service at system startup:

```bash
sudo systemctl enable ssh
```

Disable automatic startup:

```bash
sudo systemctl disable ssh
```

Enable and start a service immediately:

```bash
sudo systemctl enable --now ssh
```

Disable and stop a service:

```bash
sudo systemctl disable --now ssh
```

## 7. Check Whether a Service Is Active

Check active status:

```bash
systemctl is-active ssh
```

Check whether it starts automatically:

```bash
systemctl is-enabled ssh
```

Check whether it has failed:

```bash
systemctl is-failed ssh
```

## 8. Understanding Service States

| State | Meaning |
|---|---|
| active | Service is running or active |
| inactive | Service is not currently active |
| failed | Service encountered an error |
| enabled | Configured to start automatically |
| disabled | Not configured for automatic startup |

## 9. View System Logs

Display recent system messages:

```bash
journalctl
```

Display logs for a particular service:

```bash
journalctl -u ssh
```

Display the latest 50 log entries:

```bash
journalctl -n 50
```

Follow logs in real time:

```bash
journalctl -f
```

Display logs from the current boot:

```bash
journalctl -b
```

## 10. Security Applications

Service management is important in cybersecurity because it helps administrators:

- Identify unauthorized services.
- Detect unexpected background processes.
- Investigate failed services.
- Disable unnecessary services.
- Monitor SSH and other important services.
- Review system activity through logs.

## 11. Security Best Practices

- Keep important services updated.
- Disable services that are not required.
- Protect remote administration services.
- Monitor service failures.
- Review service logs regularly.
- Investigate unfamiliar services before stopping them.

Never disable an unfamiliar system service without understanding its purpose.

## 12. Practice Exercises

1. Explain what a Linux service is.
2. What is systemd?
3. Display running services.
4. Check the status of the SSH service.
5. Explain the difference between enable and start.
6. Display the latest 50 system log entries.
7. Explain why unnecessary services should be disabled.

## Summary

systemd manages services and important system operations on many Linux distributions. Learning systemctl and journalctl is essential for Linux administration and security monitoring.
