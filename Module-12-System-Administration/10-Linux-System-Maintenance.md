---
---


# Module 12 – Lesson 10: Linux System Maintenance

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux system maintenance.
2. Understand why regular maintenance is important.
3. Update package information and installed software.
4. Identify unnecessary packages and cached files.
5. Monitor disk space and system resources.
6. Inspect system health and failed services.
7. Understand basic maintenance security practices.
8. Create a Linux system maintenance report.

---

## 2. What Is Linux System Maintenance?

Linux system maintenance refers to the activities performed to keep a Linux computer secure, stable, efficient, and reliable.

Think of it like maintaining a car.

A car needs:
- Regular inspection.
- Oil changes.
- Cleaning.
- Replacement of worn-out parts.
- Safety checks.

Likewise, a Linux system needs:
- Software updates.
- Storage monitoring.
- Log inspection.
- Security checks.
- Package maintenance.
- Backup verification.

### Why is maintenance important?

Regular maintenance helps:

1. Reduce security vulnerabilities.
2. Prevent storage problems.
3. Improve system reliability.
4. Identify failed services.
5. Detect unusual system behaviour.
6. Keep applications updated.
7. Reduce unnecessary resource consumption.

---

## 3. Updating Linux Software

One of the most important maintenance activities is keeping software updated.

Kali Linux uses the APT package-management system.

### Step 1: Update package information

```bash
sudo apt update
```

This retrieves updated package information from configured repositories.

**Important:** `apt update` does not itself upgrade installed packages.

### Step 2: Upgrade installed packages

```bash
sudo apt upgrade
```

This installs available upgrades for installed packages, subject to APT's dependency and removal rules.

Review the proposed changes before confirming.

### Step 3: Full upgrade

```bash
sudo apt full-upgrade
```

This can install, upgrade, or remove packages to resolve dependency changes.

It may make more substantial changes than a normal upgrade.

Use it carefully and review the package list before accepting.

### Step 4: Inspect package information

```bash
apt list --upgradable
```

This displays packages with available upgrades.

### Important maintenance advice

- Use trusted repositories.
- Read package-manager prompts.
- Avoid interrupting upgrades.
- Keep enough free storage.
- Do not run multiple package managers simultaneously.

For this lesson, inspect package information first. You do not need to perform a full system upgrade.

---

## 4. Cleaning Unnecessary Packages

Over time, Linux may retain packages and downloaded files that are no longer needed.

APT provides maintenance commands.

### A. Remove unnecessary dependencies

```bash
sudo apt autoremove
```

This removes automatically installed packages that are no longer required.

Review the proposed removals before confirming.

### B. Clean downloaded package archives

```bash
sudo apt clean
```

This removes downloaded package files from APT's cache.

### C. Remove obsolete cached package files

```bash
sudo apt autoclean
```

This removes cached package files that can no longer be downloaded from configured repositories.

### Important distinction

| Command | Purpose |
|---|---|
| `apt clean` | Removes all cached package archives |
| `apt autoclean` | Removes obsolete cached archives |
| `apt autoremove` | Removes unnecessary automatically installed packages |

Never run cleanup commands without understanding what they remove.

---

## 5. Monitoring Disk Space

Storage monitoring is essential because a full filesystem can cause applications and services to fail.

### Check filesystem usage

```bash
df -h
```

Displays filesystem capacity and available space.

### Check filesystem types

```bash
df -Th
```

Displays filesystem types alongside usage information.

### Check directory sizes

```bash
du -sh ~
```

Displays the total size of your home directory.

### Find large directories

```bash
du -h --max-depth=1 ~
```

This displays the approximate sizes of directories directly inside your home directory.

### Check inode usage

```bash
df -i
```

A filesystem can run out of available inodes even when some storage capacity remains.

---

## 6. Monitoring System Health

Administrators regularly inspect important system resources.

### A. Check system uptime

```bash
uptime
```

Displays how long the system has been running and its load averages.

### B. Check memory usage

```bash
free -h
```

Displays RAM and swap usage.

### C. Check CPU information

```bash
lscpu
```

Displays processor information.

### D. Check running processes

```bash
ps aux
```

Displays running processes.

### E. Check failed systemd services

```bash
systemctl --failed
```

Displays units that systemd considers failed.

No failed services is a good sign, although it does not prove that every system component is healthy.

### F. Check system logs

```bash
journalctl -p err -b --no-pager
```

Displays error-priority journal messages from the current boot.

Some errors may be harmless or expected. Investigate them in context.

---

## 7. Checking Network Health

A properly maintained Linux system should also have working network connectivity.

### Check IP addresses

```bash
ip addr
```

### Check routing information

```bash
ip route
```

### Check listening network services

```bash
ss -tuln
```

### Test DNS resolution

```bash
getent hosts example.com
```

### Test network connectivity

```bash
ping -c 4 1.1.1.1
```

If this test succeeds but domain resolution fails, investigate DNS configuration.

If it fails, that does not automatically prove the network is disconnected; ICMP traffic may be filtered.

---

## 8. Security Maintenance

System maintenance is not only about performance. It is also about protecting the computer.

### A. Review user accounts

```bash
getent passwd
```

Displays account information from configured user databases.

Inspect users with administrative privileges:

```bash
getent group sudo
```

### B. Inspect listening services

```bash
ss -tuln
```

Identify services listening on network ports.

Investigate services you do not recognise before taking action.

### C. Check failed login information

On systems that record authentication events in the system journal:

```bash
sudo journalctl -b | grep -i -E 'failed password|authentication failure'
```

Log availability and wording depend on the system configuration.

### D. Inspect file permissions

```bash
ls -l
```

Displays permissions and ownership for files in the current directory.

### E. Check for available security updates

```bash
apt list --upgradable
```

This lists available package upgrades, but does not tell you by itself which ones are security-related.

---

## 9. Managing Temporary Files

Temporary files are commonly stored in:

```text
/tmp
/var/tmp
```

These directories are used by applications and system processes.

Inspect temporary directory usage:

```bash
du -sh /tmp
```

Inspect files in `/tmp`:

```bash
ls -lah /tmp
```

### Important security rule

Never delete files from system temporary directories indiscriminately.

Applications may still be using them.

During this lesson, inspect temporary files only. Do not perform system-wide deletion.

---

## 10. Understanding System Reboots

Sometimes maintenance requires restarting the system.

Common commands include:

```bash
sudo reboot
```

To shut down:

```bash
sudo shutdown -h now
```

To schedule a shutdown:

```bash
sudo shutdown -h +10
```

This schedules shutdown in ten minutes.

Cancel a scheduled shutdown:

```bash
sudo shutdown -c
```

### Before rebooting

1. Save your work.
2. Close important applications.
3. Check for active package upgrades.
4. Warn other users if the machine is shared.
5. Confirm that critical services can restart.

For this lesson, do not reboot unless necessary.

---

## 11. Practical Lab: Linux System Maintenance Inspection

### Objective

Perform a maintenance inspection of your Kali Linux VM without making unnecessary system-wide changes.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson10
cd ~/module12-lab/lesson10
```

### Step 2: Check system information

```bash
hostnamectl
```

Record your hostname and operating system information.

### Step 3: Check system uptime

```bash
uptime
```

### Step 4: Check memory usage

```bash
free -h
```

### Step 5: Check disk usage

```bash
df -h
```

### Step 6: Check inode usage

```bash
df -i
```

### Step 7: Check large home directories

```bash
du -h --max-depth=1 ~
```

### Step 8: Inspect available package upgrades

```bash
apt list --upgradable
```

This is an inspection command. It does not install upgrades.

### Step 9: Inspect failed services

```bash
systemctl --failed
```

### Step 10: Inspect recent system errors

```bash
sudo journalctl -p err -b --no-pager -n 30
```

### Step 11: Inspect network configuration

```bash
ip addr
ip route
ss -tuln
```

### Step 12: Inspect administrative users

```bash
getent group sudo
```

### Step 13: Generate your maintenance report

Run:

```bash
{
    echo "LINUX SYSTEM MAINTENANCE REPORT"
    echo "==============================="
    echo
    echo "Date:"
    date
    echo
    echo "Hostname:"
    hostname
    echo
    echo "Operating System:"
    cat /etc/os-release
    echo
    echo "System Uptime:"
    uptime
    echo
    echo "Memory Usage:"
    free -h
    echo
    echo "Disk Usage:"
    df -h
    echo
    echo "Failed Services:"
    systemctl --failed --no-pager
    echo
    echo "Listening Network Sockets:"
    ss -tuln
    echo
    echo "Administrative Group:"
    getent group sudo
} > maintenance-report.txt
```

Display your report:

```bash
cat maintenance-report.txt
```

Your report should be saved at:

```text
~/module12-lab/lesson10/maintenance-report.txt
```

---

## 12. Cybersecurity Connection: Why Maintenance Matters

An outdated Linux system may contain known vulnerabilities.

For example:

1. An outdated web server may contain a security flaw.
2. An attacker may exploit the vulnerable service.
3. The attacker may gain unauthorised access.
4. The attacker may attempt privilege escalation.
5. The attacker may establish persistence.

Regular maintenance reduces the risk of these situations.

### Security administrator responsibilities

A Linux security administrator should:

- Review available updates.
- Monitor system logs.
- Inspect active services.
- Review user accounts.
- Monitor storage capacity.
- Check network exposure.
- Maintain reliable backups.
- Investigate unusual processes.
- Document maintenance activities.

Maintenance should be performed according to an approved schedule, especially on production systems.

---

## 13. Lesson Summary

You learned:

1. Linux system maintenance keeps systems reliable and secure.
2. `apt update` refreshes package information.
3. `apt upgrade` upgrades installed packages.
4. `apt autoremove` removes unnecessary dependencies.
5. `apt clean` clears downloaded package archives.
6. `df` and `du` help monitor storage.
7. `free` monitors memory.
8. `systemctl --failed` identifies failed units.
9. `journalctl` helps investigate system errors.
10. Regular maintenance helps reduce security risks.

---

## 14. Knowledge Check

1. What is Linux system maintenance?
2. Why should Linux systems be updated?
3. What is the difference between `apt update` and `apt upgrade`?
4. What does `apt autoremove` do?
5. What is the purpose of `apt clean`?
6. Which command displays filesystem usage?
7. Which command displays memory usage?
8. How can you identify failed systemd services?
9. Why should temporary files be inspected before deletion?
10. Why is regular system maintenance important in cybersecurity?

---

## 15. Practical Completion Checklist

- [ ] I understand Linux system maintenance.
- [ ] I understand APT update and upgrade operations.
- [ ] I understand package cleanup commands.
- [ ] I inspected disk usage.
- [ ] I inspected inode usage.
- [ ] I inspected memory and CPU information.
- [ ] I checked failed services.
- [ ] I inspected system logs.
- [ ] I checked network configuration.
- [ ] I reviewed administrative users.
- [ ] I generated my maintenance report.
- [ ] I understand the relationship between maintenance and cybersecurity.

**Lesson 10 is complete when you understand the concepts and successfully generate your maintenance report.**
