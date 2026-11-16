
# PRACTICAL LABS
# MODULE 04 – LINUX SYSTEM ADMINISTRATION

## LAB 05: PACKAGE MANAGEMENT AND UPDATES

### 1. Lab Overview

Linux package management is the process of installing, updating, configuring, and removing software.

Kali Linux is based on Debian and uses the APT package management system.

APT allows administrators to install software from configured repositories while managing dependencies.

As a Linux system administrator and cybersecurity student, you need to understand package management because security tools, system utilities, libraries, and applications depend on it.

**Lab environment:** Kali Linux VM.

**Safety:** This lab begins with inspection and simulation. Do not perform a full system upgrade unless you have checked available disk space, VM snapshots, and the consequences of the upgrade.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain Linux packages and repositories.
2. Understand the APT package manager.
3. Inspect installed software.
4. Search for available packages.
5. Examine package information.
6. Understand package installation and removal.
7. Inspect package updates.
8. Generate a package inventory report.

### 3. What Is a Linux Package?

A package is a collection of files and metadata used to install software.

A package may contain:

- Executable programs.
- Configuration files.
- Libraries.
- Documentation.
- Installation scripts.
- Dependency information.

Debian-based distributions commonly use `.deb` package files.

Examples of software packages include:

- `curl`
- `git`
- `vim`
- `nmap`
- `python3`

### 4. Understanding APT

APT stands for Advanced Package Tool.

It manages software installation and updates on Debian-based distributions.

Common APT commands:

| Command | Purpose |
|---|---|
| `apt update` | Refresh repository package information |
| `apt upgrade` | Upgrade installed packages |
| `apt install` | Install a package |
| `apt remove` | Remove a package |
| `apt purge` | Remove a package and its configuration files |
| `apt search` | Search for packages |
| `apt show` | Display package information |
| `apt list --installed` | List installed packages |
| `apt autoremove` | Remove automatically installed packages no longer needed |

Important distinction:

- `apt update` updates package lists.
- `apt upgrade` installs available package upgrades.

The first does not automatically upgrade installed software.

### 5. Inspect Your Package Manager

Check the APT version:

```bash
apt --version
```

Inspect the operating system:

```bash
cat /etc/os-release
```

Inspect available storage:

```bash
df -h
```

Check the configured package repositories:

```bash
grep -R '^deb ' /etc/apt/sources.list /etc/apt/sources.list.d/ 2>/dev/null
```

Some modern repository configurations use `.sources` files, so also inspect:

```bash
ls -l /etc/apt/sources.list.d/
```

Do not modify repository configuration during this exercise.

### 6. Search for Software Packages

Search for a package:

```bash
apt search curl
```

Search for Git:

```bash
apt search git
```

Search for Wireshark:

```bash
apt search wireshark
```

You can also use:

```bash
apt-cache search nmap
```

These commands help administrators discover available software.

### 7. Inspect Package Information

Display detailed information about a package:

```bash
apt show curl
```

Inspect Git:

```bash
apt show git
```

Important package information includes:

| Field | Meaning |
|---|---|
| Package | Software package name |
| Version | Available package version |
| Architecture | Supported system architecture |
| Depends | Required dependencies |
| Description | Package purpose |
| Size | Package download size |
| Installed-Size | Approximate installed size |

Inspect package metadata without installing anything:

```bash
apt-cache policy curl
```

This displays installed and repository candidate versions when available.

### 8. List Installed Packages

Display installed packages:

```bash
apt list --installed
```

Display the number of installed packages:

```bash
dpkg -l | grep '^ii' | wc -l
```

Inspect a specific package:

```bash
dpkg -s curl
```

Find files installed by a package:

```bash
dpkg -L curl
```

If a package is not installed, these commands may report that it is unavailable locally.

### 9. Understand Package Dependencies

Software often requires other packages to work correctly.

These additional packages are called dependencies.

Inspect dependencies:

```bash
apt-cache depends curl
```

Inspect reverse dependencies:

```bash
apt-cache rdepends curl
```

Why are dependencies important?

Imagine installing a web application that requires:

- Python.
- A database library.
- An encryption library.

APT helps identify and manage these dependencies.

### 10. Simulate Package Installation

Before installing software, administrators can simulate the operation.

Example:

```bash
sudo apt-get -s install tree
```

The `-s` option simulates the package operation without installing anything.

Inspect the proposed changes carefully.

Pay attention to:

- Packages that would be installed.
- Packages that would be upgraded.
- Packages that would be removed.
- Dependency changes.

You can also simulate removal:

```bash
sudo apt-get -s remove tree
```

These simulations are useful for planning changes safely.

### 11. Understand Package Updates

Refresh package information:

```bash
sudo apt update
```

This command contacts configured repositories and updates local package metadata.

Inspect available upgrades:

```bash
apt list --upgradable
```

Simulate an upgrade:

```bash
sudo apt-get -s upgrade
```

Review the proposed changes before considering an actual upgrade.

**Important:** Do not run a full distribution upgrade during this lab. Kali is a rolling-release distribution, and large upgrades can change many packages and require significant disk space.

### 12. Understand Package Installation and Removal

These are common administrative operations.

Install a package:

```bash
sudo apt install PACKAGE_NAME
```

Remove a package:

```bash
sudo apt remove PACKAGE_NAME
```

Remove a package and its configuration files:

```bash
sudo apt purge PACKAGE_NAME
```

Remove unused automatically installed dependencies:

```bash
sudo apt autoremove
```

For this lab, use simulations first. Only install a package if you genuinely need it and have checked the VM's available storage.

Never remove core system packages simply to experiment.

### 13. Inspect Package Integrity

The Debian package database can help investigate installed software.

List package files:

```bash
dpkg -L bash
```

Check package installation status:

```bash
dpkg -s bash
```

Verify package-managed files where supported:

```bash
sudo dpkg -V bash
```

No output can mean that no verification differences were reported. It does not prove that the entire system is free from compromise.

### 14. Cybersecurity Application: Software Inventory

Software inventory is an important part of system security.

An administrator should know:

1. What software is installed?
2. Which versions are installed?
3. Which packages have available updates?
4. Which software is no longer needed?
5. Which packages have unusual dependencies?
6. Are packages coming from trusted repositories?

Generate a list of installed packages:

```bash
dpkg-query -W -f='${Package}\t${Version}\n'
```

Display packages that can be upgraded:

```bash
apt list --upgradable
```

Inspect configured repositories:

```bash
apt-cache policy
```

**Security principle:** Install software from trusted repositories and verify the source of downloaded packages. Avoid executing unknown installation scripts as root.

### 15. Create a Package Inventory Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module04/lab05
```

Create the script:

```bash
nano ~/practical-labs/module04/lab05/package-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module04/lab05/package-report.txt"

{
    echo "LINUX PACKAGE INVENTORY REPORT"
    echo "================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "APT VERSION"
    apt --version
    echo

    echo "INSTALLED PACKAGE COUNT"
    dpkg -l | grep '^ii' | wc -l
    echo

    echo "INSTALLED PACKAGE SAMPLE"
    dpkg-query -W -f='${Package}\t${Version}\n' | head -n 30
    echo

    echo "AVAILABLE PACKAGE UPDATES"
    apt list --upgradable 2>/dev/null
    echo

    echo "STORAGE INFORMATION"
    df -h /
    echo

    echo "CURL PACKAGE INFORMATION"
    apt-cache policy curl

} > "$REPORT" 2>&1

echo "Report generated: $REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x ~/practical-labs/module04/lab05/package-report.sh
```

Execute it:

```bash
~/practical-labs/module04/lab05/package-report.sh
```

Read the report:

```bash
cat ~/practical-labs/module04/lab05/package-report.txt
```

### 16. Practical Exercises

**Exercise 1 – Package Manager Identification**

Run:

```bash
apt --version
cat /etc/os-release
```

Record the package manager and distribution.

**Exercise 2 – Package Search**

Search for:

```bash
apt search curl
apt search git
apt search wireshark
```

Record the purpose of each package.

**Exercise 3 – Package Inspection**

Run:

```bash
apt show curl
apt-cache policy curl
dpkg -s bash
```

Identify the installed or candidate version where available.

**Exercise 4 – Installed Software Inventory**

Run:

```bash
apt list --installed
dpkg -l | grep '^ii' | wc -l
```

Record the approximate number of installed packages.

**Exercise 5 – Dependency Investigation**

Run:

```bash
apt-cache depends curl
apt-cache rdepends curl
```

Explain the difference between dependencies and reverse dependencies.

**Exercise 6 – Simulated Installation**

Run:

```bash
sudo apt-get -s install tree
```

Identify the changes APT proposes.

Do not install the package just for this exercise.

**Exercise 7 – Update Inspection**

Run:

```bash
apt list --upgradable
sudo apt-get -s upgrade
```

Inspect the proposed changes without performing the upgrade.

**Exercise 8 – Report Generation**

Execute your package inventory script and inspect the generated report.

### 17. Lab Documentation

Create:

```bash
nano ~/practical-labs/module04/lab05/lab05-report.txt
```

Record:

```text
MODULE 04 – LAB 05
PACKAGE MANAGEMENT AND UPDATES

1. Linux distribution:
2. Package manager:
3. APT version:
4. Example package searched:
5. Example package version:
6. Number of installed packages:
7. What apt update does:
8. Difference between apt update and apt upgrade:
9. What a package dependency is:
10. What I learned:
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 18. Knowledge Check

1. What is a Linux package?
2. What does APT stand for?
3. What is the purpose of a software repository?
4. What is the difference between `apt update` and `apt upgrade`?
5. What does `apt search` do?
6. What information does `apt show` provide?
7. What is a package dependency?
8. What does `dpkg -l` display?
9. Why is package installation simulation useful?
10. Why should administrators use trusted software repositories?

### 19. Lab Completion Checklist

- [ ] Identified the APT package manager.
- [ ] Inspected repository configuration.
- [ ] Searched for packages.
- [ ] Examined package information.
- [ ] Listed installed packages.
- [ ] Investigated package dependencies.
- [ ] Simulated package installation.
- [ ] Inspected available updates.
- [ ] Generated a package inventory report.
- [ ] Completed lab documentation.

**LAB 05 STATUS: READY FOR PRACTICAL COMPLETION**
