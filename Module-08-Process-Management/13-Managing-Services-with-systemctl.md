# MODULE 08: PROCESS MANAGEMENT

## LESSON 13: MANAGING SERVICES WITH SYSTEMCTL

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain the purpose of `systemctl`.
2. Check the status of Linux services.
3. Start and stop services.
4. Restart and reload services.
5. Enable and disable services at boot.
6. Understand the difference between starting and enabling a service.
7. Manage services safely in Kali Linux.

---

### 2. What Is systemctl?

`systemctl` is a command-line utility used to control and inspect systemd.

Systemd is the system and service manager used by many modern Linux distributions, including Kali Linux.

With `systemctl`, administrators can:

- Start services.
- Stop services.
- Restart services.
- Reload service configurations.
- Check service status.
- Enable services at boot.
- Disable services at boot.
- Inspect failed services.

It is one of the most important commands for Linux system administration.

### 3. Checking Whether systemd Is Available

Before using systemctl, check whether systemd is running as your system's init system.

Run:

```bash
ps -p 1 -o pid,comm,args
```

On a typical systemd-based installation, you should see systemd as PID 1.

Check its version:

```bash
systemctl --version
```

If systemd is not running as PID 1, some systemctl operations may not work normally.

### 4. Checking Service Status

The `status` operation displays information about a service.

Syntax:

```bash
systemctl status SERVICE_NAME
```

Example:

```bash
systemctl status cron
```

Possible information includes:

- Service name.
- Whether the unit file is loaded.
- Whether the service is active.
- Main process ID.
- Recent log messages.

Common service states:

| State | Meaning |
|---|---|
| active (running) | Service is running |
| active (exited) | Service completed a one-shot task |
| inactive (dead) | Service is not active |
| failed | Service encountered an error |
| activating | Service is starting |
| deactivating | Service is stopping |

Press `q` to exit the status display when it opens in a pager.

### 5. Starting a Service

The `start` operation starts a service immediately.

Syntax:

```bash
sudo systemctl start SERVICE_NAME
```

Example:

```bash
sudo systemctl start cron
```

This requests that systemd start the cron service.

Check its status:

```bash
systemctl status cron
```

**Important:** Starting a service does not automatically enable it at boot.

### 6. Stopping a Service

The `stop` operation requests that systemd stop a running service.

Syntax:

```bash
sudo systemctl stop SERVICE_NAME
```

Example:

```bash
sudo systemctl stop cron
```

This stops the cron service.

Check:

```bash
systemctl status cron
```

Only stop services when you understand their purpose and have permission to manage them.

Do not stop essential system services during ordinary practice.

### 7. Restarting a Service

The `restart` operation stops and starts a service again.

Syntax:

```bash
sudo systemctl restart SERVICE_NAME
```

Example:

```bash
sudo systemctl restart cron
```

This can be useful when:

- A service becomes unresponsive.
- A configuration change requires a restart.
- An administrator needs to reinitialize a service.

A restart can temporarily interrupt the service.

### 8. Reloading a Service

Some services support configuration reloads without a full restart.

Syntax:

```bash
sudo systemctl reload SERVICE_NAME
```

Example:

```bash
sudo systemctl reload SERVICE_NAME
```

The service must support reload operations.

If it does not, systemd may report that reloading is not supported.

A reload is different from a restart:

| Reload | Restart |
|---|---|
| Requests configuration reload | Stops and starts the service |
| May avoid interrupting existing connections | May interrupt active connections |
| Requires service support | Uses the service's restart behavior |

### 9. Understanding enable and disable

Systemd can configure services to start automatically during boot.

The `enable` operation configures automatic startup.

Syntax:

```bash
sudo systemctl enable SERVICE_NAME
```

Example:

```bash
sudo systemctl enable cron
```

The `disable` operation removes the automatic-start configuration.

Syntax:

```bash
sudo systemctl disable SERVICE_NAME
```

Example:

```bash
sudo systemctl disable cron
```

Disabling a service does not necessarily stop it immediately.

Likewise, enabling a service does not necessarily start it immediately.

### 10. Understanding enable --now and disable --now

You can combine operations.

Start a service and enable it at boot:

```bash
sudo systemctl enable --now SERVICE_NAME
```

Stop a service and disable it at boot:

```bash
sudo systemctl disable --now SERVICE_NAME
```

These commands perform both actions.

Be careful when using them on important services.

### 11. Listing Services

To list currently loaded service units:

```bash
systemctl list-units --type=service
```

To show running services:

```bash
systemctl list-units --type=service --state=running
```

To show failed units:

```bash
systemctl --failed
```

To list installed service unit files and their enablement states:

```bash
systemctl list-unit-files --type=service
```

These commands help administrators understand the service configuration of a Linux system.

### 12. Understanding Service Enablement States

The enablement state describes how a unit is configured for automatic activation.

| State | Meaning |
|---|---|
| enabled | Configured for automatic activation through its installation configuration |
| disabled | Not configured for automatic activation |
| static | Cannot normally be enabled directly because it lacks the necessary installation configuration |
| masked | Prevented from being started through systemd |
| indirect | Usually activated through another unit |
| generated | Created dynamically by a generator |

A service can be running while disabled.

A service can also be enabled while currently stopped.

Runtime state and boot enablement are different concepts.

### 13. Viewing Service Logs

Systemd integrates with the journal logging system.

Use `journalctl` to inspect logs.

Example:

```bash
journalctl -u cron
```

To display recent entries:

```bash
journalctl -u cron -n 20
```

To follow new log entries:

```bash
journalctl -u cron -f
```

Press `Ctrl+C` to stop following the log.

You may need elevated permissions to access some logs.

### 14. Practical Laboratory: Managing a Test Service

**Objective:** Explore systemctl without changing essential system services.

For this laboratory, we will create a harmless custom service that runs the `sleep` command.

The service will be created only inside your home directory.

#### Step 1: Create the laboratory directory

```bash
mkdir -p ~/module08-lab/systemctl
```

#### Step 2: Enter the directory

```bash
cd ~/module08-lab/systemctl
```

#### Step 3: Create a custom systemd unit directory

```bash
mkdir -p ~/.config/systemd/user
```

#### Step 4: Create a test service

Open a text editor:

```bash
nano ~/.config/systemd/user/linux-lab.service
```

Enter:

```ini
[Unit]
Description=Linux Process Management Practice Service

[Service]
Type=simple
ExecStart=/usr/bin/sleep 300
Restart=no
```

Save the file:

- Press `Ctrl+O`.
- Press `Enter`.
- Press `Ctrl+X`.

#### Step 5: Reload the user systemd manager

```bash
systemctl --user daemon-reload
```

This makes systemd recognize the new unit file.

#### Step 6: Start the test service

```bash
systemctl --user start linux-lab.service
```

#### Step 7: Check its status

```bash
systemctl --user status linux-lab.service
```

You should see that the service is active while its sleep process is running.

#### Step 8: Inspect its process

```bash
pgrep -a sleep
```

Identify the sleep process started by your test service.

#### Step 9: Restart the service

```bash
systemctl --user restart linux-lab.service
```

Check its status again:

```bash
systemctl --user status linux-lab.service
```

#### Step 10: Stop the service

```bash
systemctl --user stop linux-lab.service
```

Verify:

```bash
systemctl --user status linux-lab.service
```

It should now be inactive.

#### Step 11: Enable the service for user-session startup

```bash
systemctl --user enable linux-lab.service
```

Inspect the enablement state:

```bash
systemctl --user is-enabled linux-lab.service
```

It should report `enabled`.

**Note:** User services normally start with the user's systemd session. They do not automatically become system-wide boot services.

#### Step 12: Disable the test service

```bash
systemctl --user disable linux-lab.service
```

Verify:

```bash
systemctl --user is-enabled linux-lab.service
```

It should report `disabled`.

#### Step 13: Remove the test service

Stop it if necessary:

```bash
systemctl --user stop linux-lab.service
```

Remove the test unit:

```bash
rm ~/.config/systemd/user/linux-lab.service
```

Reload the user manager:

```bash
systemctl --user daemon-reload
```

Reset any recorded failed state:

```bash
systemctl --user reset-failed linux-lab.service
```

Verify that the test unit is no longer available:

```bash
systemctl --user status linux-lab.service
```

It should report that the unit cannot be found.

**Important:** If your Kali environment does not provide a running user systemd manager, the `--user` commands may fail. Do not attempt to replace PID 1 or change your VM's initialization system for this exercise. In that case, inspect the error and practise the read-only system-wide commands from earlier steps.

### 15. Common Mistakes

**Mistake 1: Confusing start and enable**

- `start` runs a service now.
- `enable` configures automatic activation.

**Mistake 2: Assuming disable stops a service**

Disabling boot activation does not automatically stop an already-running service.

**Mistake 3: Restarting important services unnecessarily**

Restarting network or security services may interrupt your work.

**Mistake 4: Editing system unit files carelessly**

Incorrect service configuration can prevent a service from starting.

**Mistake 5: Forgetting daemon-reload**

After creating or modifying a systemd unit file, run:

```bash
systemctl --user daemon-reload
```

For system-wide unit files, use:

```bash
sudo systemctl daemon-reload
```

### 16. Cybersecurity Applications

**1. Service hardening**

Administrators can review services and disable unnecessary ones after checking dependencies.

**2. Incident response**

Services can be inspected when investigating suspicious background activity.

**3. SSH administration**

The SSH service can be inspected and maintained through systemd.

**4. Log investigation**

`journalctl` provides service logs that can support security investigations.

**5. Persistence investigation**

Unexpected services configured to start automatically may deserve investigation.

**6. System reliability**

Correct service management helps ensure essential services remain available.

### 17. Lesson Summary

In this lesson, we learned:

1. `systemctl` manages systemd.
2. `status` inspects a service.
3. `start` starts a service.
4. `stop` stops a service.
5. `restart` restarts a service.
6. `reload` requests configuration reload.
7. `enable` configures automatic activation.
8. `disable` removes automatic activation.
9. `journalctl` displays systemd journal logs.
10. User services can be managed with `systemctl --user`.

### 18. Knowledge Check

Answer these questions in your own words.

1. What is systemctl?
2. What does `systemctl status` do?
3. What is the difference between start and enable?
4. What does the stop operation do?
5. What is the difference between reload and restart?
6. What does `enable --now` do?
7. Does disabling a service automatically stop it?
8. What does `systemctl --failed` display?
9. What is the purpose of `journalctl -u`?
10. Why should administrators be careful when modifying essential services?

### 19. Practical Assignment

- [ ] Create `~/module08-lab/systemctl`.
- [ ] Inspect systemd availability.
- [ ] List running services.
- [ ] Inspect one available service.
- [ ] Create the harmless user-level test service.
- [ ] Reload the user systemd manager.
- [ ] Start the test service.
- [ ] Inspect its status.
- [ ] Restart the test service.
- [ ] Stop the test service.
- [ ] Enable and disable it.
- [ ] Remove the test service and reload the manager.
- [ ] Complete all 10 knowledge-check questions.

**End of Lesson 13**

**Next Lesson:** Process Monitoring and Troubleshooting.