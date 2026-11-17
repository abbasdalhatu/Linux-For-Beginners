---
---

# MODULE 09: LINUX PACKAGE MANAGEMENT

## LESSON 03: UNDERSTANDING APT AND DPKG

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what APT is.
2. Explain what DPKG is.
3. Understand the relationship between APT and DPKG.
4. Identify the differences between both tools.
5. Understand how packages are installed.
6. Inspect package databases.
7. Use basic APT and DPKG commands.
8. Understand their importance in Linux administration and cybersecurity.

---

### 2. Introduction to APT and DPKG

Kali Linux is based on Debian.

Debian-based distributions use two important package-management tools:

- APT
- DPKG

Both tools help manage software packages, but they operate at different levels.

Think of them this way:

- APT is the higher-level package manager that handles repositories, downloads, and dependency resolution.
- DPKG is the lower-level tool that installs, removes, and manages Debian package files.

APT can use DPKG to perform package installation and removal operations.

### 3. What Is APT?

APT stands for Advanced Package Tool.

It is a package-management system used by Debian-based Linux distributions.

APT allows administrators to:

- Install software.
- Remove software.
- Upgrade installed packages.
- Search for available packages.
- Inspect package information.
- Download packages from repositories.
- Resolve dependencies.

APT communicates with configured software repositories to obtain package information and software.

Example:

```bash
sudo apt install curl
```

APT checks the package information, resolves dependencies, downloads the necessary files, and coordinates their installation.

### 4. Important APT Commands

| Command | Purpose |
|---|---|
| `apt update` | Refreshes package lists |
| `apt upgrade` | Upgrades installed packages |
| `apt install` | Installs packages |
| `apt remove` | Removes installed packages |
| `apt purge` | Removes packages and their system configuration files |
| `apt search` | Searches for packages |
| `apt show` | Displays package information |
| `apt list --installed` | Lists installed packages |
| `apt autoremove` | Removes automatically installed packages no longer required |

Examples:

Search for Nano:

```bash
apt search nano
```

Inspect Nano:

```bash
apt show nano
```

Install Nano:

```bash
sudo apt install nano
```

Check installed packages:

```bash
apt list --installed
```

### 5. What Is DPKG?

DPKG stands for Debian Package.

It is the lower-level package-management system used by Debian and its derivatives.

DPKG works directly with `.deb` package files and the local package database.

It can:

- Install local Debian packages.
- Remove packages.
- Inspect package information.
- List installed packages.
- Display package contents.
- Verify package installation status.

Example:

```bash
sudo dpkg -i package.deb
```

This command attempts to install a local Debian package.

However, DPKG does not automatically download missing dependencies from configured repositories.

If dependencies are missing, the installation may remain incomplete until the problem is resolved.

### 6. Important DPKG Commands

| Command | Purpose |
|---|---|
| `dpkg -i package.deb` | Installs a local package |
| `dpkg -r package` | Removes a package |
| `dpkg -P package` | Purges a package |
| `dpkg -l` | Lists package information |
| `dpkg -s package` | Displays package status |
| `dpkg -L package` | Lists installed package files |
| `dpkg -S /path/file` | Identifies which installed package owns a file |
| `dpkg -I package.deb` | Displays information about a local package archive |
| `dpkg -c package.deb` | Lists the contents of a local package archive |

Examples:

Check Nano:

```bash
dpkg -s nano
```

List Nano files:

```bash
dpkg -L nano
```

List installed packages:

```bash
dpkg -l
```

Find which installed package owns a file:

```bash
dpkg -S /usr/bin/nano
```

The file path must exist and be owned by an installed package for this command to return a result.

### 7. Understanding the Difference Between APT and DPKG

This is one of the most important concepts in Linux package management.

| Feature | APT | DPKG |
|---|---|---|
| Level | Higher-level | Lower-level |
| Repository support | Yes | No direct repository management |
| Dependency resolution | Yes | Does not automatically resolve dependencies |
| Installs packages | Yes | Yes |
| Removes packages | Yes | Yes |
| Works with `.deb` files | Yes | Yes |
| Downloads packages | Yes | No |
| Maintains package database | Uses DPKG's package-management system | Directly manages the package database |

**Simple analogy:**

Imagine a supermarket.

- APT is like a shopping assistant who finds products, checks what else you need, and obtains them.
- DPKG is like the worker who handles the actual unpacking and installation of the products.

Both are important, but they have different responsibilities.

### 8. How APT and DPKG Work Together

When you run:

```bash
sudo apt install curl
```

A simplified process occurs:

1. APT identifies the requested package.
2. APT checks its package information.
3. APT calculates required dependencies.
4. APT downloads the required package files.
5. APT coordinates package installation through DPKG.
6. DPKG unpacks and configures the packages.
7. The package database records their installation status.

This is why APT is usually more convenient for ordinary package management.

### 9. Understanding the DPKG Package Database

DPKG maintains information about installed packages.

Important locations include:

**Package status information:**

```text
/var/lib/dpkg/status
```

This file stores package records and installation information.

Inspect its beginning:

```bash
head -n 20 /var/lib/dpkg/status
```

**Package metadata records:**

```text
/var/lib/dpkg/info/
```

This directory contains package-related information such as file lists and maintenance scripts.

Inspect it:

```bash
ls /var/lib/dpkg/info/ | head
```

**Important:** These locations are managed by the package-management system. Do not manually edit or delete their contents.

### 10. Understanding Package Installation States

DPKG tracks the status of packages.

When you run:

```bash
dpkg -l
```

You may see a status column containing two or three characters.

Common status combinations include:

| Status | Meaning |
|---|---|
| `ii` | Desired state is install; package is installed |
| `rc` | Package was removed, but configuration files remain |
| `un` | Package is not installed |
| `iU` | Package is unpacked but not fully configured |
| `iF` | Package is in a failed configuration state |

The exact output may vary depending on the package and its current state.

For ordinary installed packages, `ii` is the status you will commonly encounter.

### 11. Understanding Package Configuration

Installing a package involves more than extracting files.

Some packages require configuration after installation.

DPKG handles package configuration through its package-maintenance mechanisms.

Inspect the configuration status of a package:

```bash
dpkg -s curl
```

Look for a line similar to:

```text
Status: install ok installed
```

This indicates that the package is installed and its configuration completed successfully.

### 12. Practical Laboratory: Exploring APT and DPKG

**Objective:** Practise inspecting packages and understand how both tools work.

This laboratory is observational. You do not need to install or remove anything.

#### Step 1: Check APT version

```bash
apt --version
```

#### Step 2: Check DPKG version

```bash
dpkg --version
```

#### Step 3: Search for curl

```bash
apt search curl
```

#### Step 4: Inspect curl package information

```bash
apt show curl
```

#### Step 5: Check whether curl is installed

```bash
dpkg -s curl
```

If curl is not installed, choose another installed package.

#### Step 6: List installed packages

```bash
dpkg -l | less
```

Press `q` to exit.

#### Step 7: Count installed packages

```bash
dpkg -l | grep '^ii' | wc -l
```

#### Step 8: Display files belonging to curl

If installed:

```bash
dpkg -L curl
```

#### Step 9: Identify which package owns a file

If curl is installed:

```bash
dpkg -S /usr/bin/curl
```

#### Step 10: Inspect the DPKG status database

```bash
head -n 20 /var/lib/dpkg/status
```

#### Step 11: Inspect package metadata files

```bash
ls /var/lib/dpkg/info/ | head
```

#### Step 12: Compare both tools

Write down three differences between APT and DPKG.

### 13. Common Package Management Mistakes

**Mistake 1: Assuming DPKG automatically resolves dependencies**

DPKG installs local package files but does not automatically download missing dependencies.

**Mistake 2: Confusing apt update with apt upgrade**

- `apt update` refreshes package information.
- `apt upgrade` upgrades installed packages.

**Mistake 3: Installing unknown DEB files**

A local package may contain unwanted or malicious software. Always verify its source.

**Mistake 4: Editing the DPKG database manually**

The package database is managed by the package-management system. Manual modifications can damage package records.

**Mistake 5: Ignoring package installation errors**

An incomplete package installation can leave software in an inconsistent state.

### 14. Cybersecurity Applications

APT and DPKG are important for Linux security operations.

**1. Software inventory**

Security professionals can identify installed software using DPKG.

**2. Vulnerability management**

Installed package versions can be compared with security advisories.

**3. Software integrity**

Package verification tools can help investigate unexpected file modifications.

**4. Dependency investigation**

Understanding dependencies helps identify components that may introduce vulnerabilities.

**5. System maintenance**

APT helps administrators obtain security updates from trusted repositories.

**6. Incident investigation**

The package database can help establish which software is installed on a system.

### 15. Lesson Summary

In this lesson, we learned:

1. APT is a higher-level package-management tool.
2. DPKG is a lower-level Debian package-management tool.
3. APT manages repositories and dependencies.
4. DPKG works directly with Debian package files.
5. APT coordinates package installation through DPKG.
6. DPKG maintains the local package database.
7. `dpkg -s` inspects package status.
8. `dpkg -L` lists package files.
9. `dpkg -S` identifies file ownership.
10. Both tools are important for Linux administration and security.

### 16. Knowledge Check

Answer these questions in your own words.

1. What does APT stand for?
2. What is DPKG?
3. Explain the difference between APT and DPKG.
4. Why is APT considered a higher-level package manager?
5. Why can a DPKG installation fail because of missing dependencies?
6. Which command displays installed packages?
7. Which command displays files belonging to a package?
8. What is the purpose of `/var/lib/dpkg/status`?
9. What does the status `ii` mean?
10. Why should administrators avoid manually editing the DPKG database?

### 17. Practical Assignment

- [ ] Check the APT version.
- [ ] Check the DPKG version.
- [ ] Search for curl using APT.
- [ ] Inspect curl using `apt show`.
- [ ] Check whether curl is installed.
- [ ] List installed packages using DPKG.
- [ ] Count installed packages.
- [ ] List files belonging to an installed package.
- [ ] Identify which package owns one executable.
- [ ] Inspect the DPKG status database.
- [ ] Write down three differences between APT and DPKG.
- [ ] Complete all ten knowledge-check questions.

**Safety reminder:** Do not manually modify the DPKG database or install unknown package files.

**END OF LESSON 03**

**NEXT LESSON: WORKING WITH APT REPOSITORIES**