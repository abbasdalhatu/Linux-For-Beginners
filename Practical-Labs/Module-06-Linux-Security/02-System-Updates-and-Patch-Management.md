
# PRACTICAL LABS
# MODULE 06 – LINUX SECURITY

## LAB 02: SYSTEM UPDATES AND PATCH MANAGEMENT

### 1. Introduction

One of the most important responsibilities of a Linux system administrator is keeping the operating system and installed applications updated.

Software developers regularly release updates to:

- Fix security vulnerabilities.
- Correct software bugs.
- Improve system performance.
- Add new features.
- Improve compatibility.
- Strengthen system stability.

An outdated Linux system may contain vulnerabilities that attackers can exploit.

**Patch management** is the process of identifying, evaluating, installing, and verifying software updates.

### 2. What Is a Security Patch?

A security patch is a software update designed to fix a security weakness.

Example:

Imagine a web server has a vulnerability that allows unauthorized users to access sensitive information.

The software developer investigates the problem and releases a security patch.

The administrator installs the update to reduce the risk.

### 3. Types of Software Updates

| Update type | Purpose |
|---|---|
| Security update | Fixes security vulnerabilities |
| Bug fix | Corrects software errors |
| Feature update | Introduces new functionality |
| Kernel update | Updates the Linux kernel |
| Package update | Updates installed applications |
| Dependency update | Updates software libraries |

### 4. Understanding APT

Kali Linux is based on Debian and uses APT (Advanced Package Tool) for package management.

APT helps administrators:

- Install packages.
- Remove packages.
- Update package information.
- Upgrade installed software.
- Resolve dependencies.
- Search package repositories.

Important commands:

| Command | Purpose |
|---|---|
| `sudo apt update` | Refreshes package information |
| `apt list --upgradable` | Lists available upgrades |
| `sudo apt upgrade` | Upgrades installed packages |
| `apt show PACKAGE` | Displays package information |
| `apt policy PACKAGE` | Shows package versions and repository information |
| `sudo apt autoremove` | Removes automatically installed packages no longer needed |

### 5. Understand the Difference Between Update and Upgrade

These commands are not the same.

**APT update**

```bash
sudo apt update
```

This refreshes the local package index.

It does not normally install package upgrades.

**APT upgrade**

```bash
sudo apt upgrade
```

This installs available upgrades for installed packages.

A simple analogy:

- `apt update` checks the catalogue for new versions.
- `apt upgrade` installs available newer versions.

### 6. Inspect Your Current System

Create the lab directory:

```bash
mkdir -p ~/practical-labs/module06/lab02
```

Move into it:

```bash
cd ~/practical-labs/module06/lab02
```

#### Task 1: Identify the Operating System

```bash
cat /etc/os-release
```

#### Task 2: Identify the Kernel Version

```bash
uname -r
```

#### Task 3: Check APT Version

```bash
apt --version
```

#### Task 4: Inspect Configured Repositories

```bash
grep -RhsE '^[[:space:]]*deb ' /etc/apt/sources.list /etc/apt/sources.list.d/ 2>/dev/null
```

This displays traditional APT repository entries where configured.

Modern Kali systems may also use deb822 repository files.

Inspect those with:

```bash
cat /etc/apt/sources.list.d/*.sources 2>/dev/null
```

Only use official, trusted repositories.

### 7. Practical Exercise – Refresh Package Information

Run:

```bash
sudo apt update
```

Observe the output.

Important information includes:

- Repository names.
- Package index downloads.
- Successful updates.
- Repository errors.
- Network connection failures.
- Signature verification warnings.

If you encounter an error, record it.

Do not bypass repository signature verification.

### 8. Check Available Package Upgrades

Run:

```bash
apt list --upgradable
```

This identifies packages with newer versions available.

You can also inspect an individual package:

```bash
apt policy PACKAGE_NAME
```

Replace `PACKAGE_NAME` with an actual package name.

For example:

```bash
apt policy openssh-server
```

If the package is not installed, APT may still display available repository information.

### 9. Understand the Upgrade Process

Before installing upgrades, consider:

1. Is the system using trusted repositories?
2. Is there sufficient disk space?
3. Is the VM snapshot or backup available?
4. Are any important applications running?
5. Could an update require a restart?
6. Is the system currently stable?

To review upgrades:

```bash
apt list --upgradable
```

If you are ready to update your Kali VM and have sufficient time and disk space:

```bash
sudo apt upgrade
```

Read the package changes before confirming.

Do not interrupt an active package installation.

**Important:** A full Kali distribution upgrade can be substantial. It is not required for this lab. Avoid `full-upgrade` or `dist-upgrade` unless you have a specific reason and have reviewed the changes.

### 10. Inspect Installed Package Information

Run:

```bash
dpkg -l | head -20
```

This displays a sample of installed packages.

To inspect a specific package:

```bash
dpkg -s bash
```

Look for:

- Package name.
- Version.
- Architecture.
- Installation status.
- Description.

### 11. Understand Package Removal

Sometimes software is no longer required.

APT provides commands for removing packages.

**Remove a package:**

```bash
sudo apt remove PACKAGE_NAME
```

**Remove a package and its system configuration files:**

```bash
sudo apt purge PACKAGE_NAME
```

**Remove automatically installed dependencies that are no longer required:**

```bash
sudo apt autoremove
```

Do not run removal commands blindly.

Always inspect the proposed package removal list before confirming.

For this lab, inspect what APT would remove without making changes:

```bash
sudo apt autoremove --dry-run
```

### 12. Understand Kernel Updates

The Linux kernel manages:

- Hardware communication.
- Processes.
- Memory.
- Networking.
- Filesystems.
- System security mechanisms.

Kernel updates may include important security fixes.

Inspect your current kernel:

```bash
uname -r
```

Inspect installed kernel packages:

```bash
dpkg -l 'linux-image*' | grep '^ii'
```

A kernel update may require a reboot before the new kernel becomes active.

Do not remove your currently working kernel simply because a newer one is available.

### 13. Inspect Package Installation History

APT logs can help administrators investigate package changes.

Run:

```bash
sudo less /var/log/apt/history.log
```

Press `q` to exit.

You can also inspect the terminal output history:

```bash
sudo less /var/log/apt/term.log
```

These logs can help establish:

- When packages were installed.
- When packages were upgraded.
- Which packages were removed.
- Whether an installation encountered problems.

### 14. Practical Exercise – Create an Update Inventory

Create a script:

```bash
nano update-inventory.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module06/lab02/update-inventory.txt"

{
    echo "LINUX UPDATE AND PATCH INVENTORY"
    echo "================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "OPERATING SYSTEM"
    cat /etc/os-release
    echo

    echo "KERNEL VERSION"
    uname -r
    echo

    echo "APT VERSION"
    apt --version
    echo

    echo "AVAILABLE PACKAGE UPGRADES"
    apt list --upgradable 2>/dev/null
    echo

    echo "SAMPLE INSTALLED PACKAGES"
    dpkg -l | head -20
    echo

    echo "APT REPOSITORY CONFIGURATION"
    grep -RhsE '^[[:space:]]*deb ' \
        /etc/apt/sources.list \
        /etc/apt/sources.list.d/ 2>/dev/null
    echo

    echo "RECENT APT HISTORY"
    if [ -f /var/log/apt/history.log ]; then
        tail -40 /var/log/apt/history.log
    else
        echo "APT history log not found."
    fi

} > "$REPORT" 2>&1

echo "Update inventory generated:"
echo "$REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x update-inventory.sh
```

Run:

```bash
./update-inventory.sh
```

Inspect the report:

```bash
less update-inventory.txt
```

### 15. Understand Why Patch Management Matters in Cybersecurity

Consider this example:

A company operates a Linux server running an outdated application.

A security vulnerability is publicly disclosed.

An attacker identifies that the server is running the vulnerable version.

If the administrator has not installed the appropriate security patch, the server may remain exposed.

A responsible administrator should:

1. Identify the affected software.
2. Determine whether the vulnerability applies.
3. Check trusted vendor security advisories.
4. Evaluate the available patch.
5. Plan the update.
6. Install the update.
7. Verify the installed version.
8. Document the result.

### 16. Knowledge Check

1. What is patch management?
2. What is a security patch?
3. Why should Linux systems be updated?
4. What is APT?
5. What does `sudo apt update` do?
6. What does `sudo apt upgrade` do?
7. What is the difference between update and upgrade?
8. What does `apt list --upgradable` show?
9. What is the purpose of `dpkg`?
10. What is a Linux kernel update?
11. Why should package removal be reviewed?
12. What is the purpose of `/var/log/apt/history.log`?
13. Why should software come from trusted repositories?
14. Why might an update require a reboot?
15. How does patch management improve cybersecurity?

### 17. Lab Documentation

Create:

```bash
nano lab02-report.txt
```

Record:

```text
MODULE 06 – LAB 02
SYSTEM UPDATES AND PATCH MANAGEMENT

1. Linux distribution:
2. Kernel version:
3. APT version:
4. Configured package repositories:
5. Was apt update successful?
6. Were upgradeable packages available?
7. Which packages were listed?
8. Was an upgrade performed?
9. Were any repository errors observed?
10. What does apt upgrade do?
11. What is a security patch?
12. Why are trusted repositories important?
13. What did I learn?

UPDATE INVENTORY:
Generated successfully: YES / NO

LAB STATUS:
COMPLETED / NOT COMPLETED
```

### 18. Completion Checklist

- [ ] Understood patch management.
- [ ] Learned the purpose of security patches.
- [ ] Understood APT.
- [ ] Identified the difference between update and upgrade.
- [ ] Inspected configured repositories.
- [ ] Refreshed package information.
- [ ] Inspected available upgrades.
- [ ] Reviewed installed package information.
- [ ] Studied package removal.
- [ ] Inspected kernel information.
- [ ] Reviewed APT history logs.
- [ ] Generated the update inventory.
- [ ] Completed the lab report.

**LAB 02 STATUS: READY FOR PRACTICAL COMPLETION**
