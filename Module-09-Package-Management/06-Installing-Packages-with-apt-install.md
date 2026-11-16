# Module 09 – Lesson 06: Installing Packages with `apt install`

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain how APT installs software.
2. Install packages using `apt install`.
3. Install multiple packages in one command.
4. Understand package dependencies.
5. Verify successful package installation.
6. Identify and troubleshoot common installation errors.
7. Install software safely on Kali Linux.

---

## 2. What Is Package Installation?

Package installation is the process of adding software to a Linux system so that it can be used.

For example, if you want to install the `tree` utility, you can use:

```bash
sudo apt install tree
```

APT finds the package in your configured repositories, downloads it and any required dependencies, and coordinates its installation through DPKG.

### Simple analogy

Imagine installing an application on your smartphone.

You open an app store, search for the application, select Install, and wait for the installation to finish.

APT performs a similar function on Debian-based Linux distributions.

---

## 3. Understanding `apt install`

Consider:

```bash
sudo apt install nano
```

Let's break it down:

| Component | Meaning |
|---|---|
| `sudo` | Runs the command with administrative privileges |
| `apt` | Package management interface |
| `install` | Requests installation of software |
| `nano` | Name of the package |

APT checks whether the package is available, calculates dependencies, and prepares the installation.

---

## 4. How APT Installs a Package

When you execute:

```bash
sudo apt install nano
```

The process generally involves:

1. Reading the current package indexes.
2. Searching for the requested package.
3. Identifying its available version.
4. Checking required dependencies.
5. Calculating the installation plan.
6. Downloading required package files.
7. Passing package installation tasks to DPKG.
8. Configuring the installed software.

This is why APT is more convenient than manually installing individual `.deb` files.

---

## 5. Installing Your First Package

Let's use `tree` as our example.

The `tree` utility displays files and directories in a tree-like structure.

### Step 1: Refresh package information

```bash
sudo apt update
```

### Step 2: Install tree

```bash
sudo apt install tree
```

### Step 3: Confirm the installation

```bash
tree --version
```

If the installation was successful, the command should display version information.

### Step 4: Test the program

```bash
tree ~
```

This displays the contents of your home directory in a tree-like format.

---

## 6. Installing Multiple Packages

APT can install several packages in one command.

Example:

```bash
sudo apt install tree nano curl wget
```

APT will attempt to install all four requested packages and their dependencies.

### Why install multiple packages together?

- Saves time.
- Reduces repeated commands.
- Makes basic system setup easier.
- Allows APT to calculate the combined dependency requirements.

You should still review the installation summary before confirming.

---

## 7. Understanding Dependencies During Installation

A package may depend on other packages to function correctly.

For example, imagine:

```text
Application A
    |
    +---- Library B
    |
    +---- Library C
    |
    +---- Utility D
```

If Application A requires Library B and Library C, APT identifies those dependencies and includes them in the installation plan.

This is one of the main advantages of APT.

### What happens if dependencies are missing?

APT generally attempts to retrieve the required dependencies automatically.

If it cannot resolve them, it may report an error and stop the installation.

We will study dependencies in greater detail in Lesson 11.

---

## 8. Understanding the Installation Confirmation

During installation, APT may display output similar to:

```text
The following NEW packages will be installed:
  tree

Need to get 50.0 kB of archives.
After this operation, 120 kB of additional disk space will be used.

Do you want to continue? [Y/n]
```

This is illustrative output. Actual package sizes and messages vary.

Before confirming, understand these sections:

| Output | Meaning |
|---|---|
| `NEW packages` | Packages that will be installed |
| `Need to get` | Approximate download size |
| `additional disk space` | Extra storage required |
| `Do you want to continue?` | Requests confirmation |

Type:

```text
Y
```

Then press Enter to proceed.

If you do not want to continue, type:

```text
n
```

Then press Enter.

**Important:** Always review the package list. Do not automatically approve an unexpected installation, especially if APT proposes removing important software.

---

## 9. Installing a Specific Package Version

Sometimes you may need a particular package version.

First inspect the available versions:

```bash
apt policy nano
```

You may see an output containing:

```text
Installed: 7.2-1
Candidate: 7.2-1
Version table:
   7.2-1 500
```

The values above are illustrative.

If a particular version is available, APT supports this syntax:

```bash
sudo apt install package-name=version
```

For example:

```bash
sudo apt install nano=7.2-1
```

Only use a version that is actually available in your configured repositories.

Older versions may contain known vulnerabilities or dependency problems.

---

## 10. Installing Packages Without Interactive Confirmation

APT supports the `-y` option.

Example:

```bash
sudo apt install tree -y
```

The `-y` option automatically answers yes to confirmation prompts.

This is useful in carefully controlled automation.

However, beginners should normally avoid using it until they understand the installation plan.

Why?

Because you may miss an unexpected change, such as the removal of another package.

For learning and normal manual installation, use:

```bash
sudo apt install tree
```

Review the summary before confirming.

---

## 11. Verifying Installed Packages

After installing software, you should verify that it is installed correctly.

### A. Check the program version

```bash
tree --version
```

### B. Check package status

```bash
dpkg -s tree
```

Look for:

```text
Status: install ok installed
```

### C. Find the executable location

```bash
command -v tree
```

This displays the location of the executable if it is available through your command search path.

### D. List installed package files

```bash
dpkg -L tree
```

This shows files installed by the package.

### E. Search the installed package list

```bash
dpkg -l tree
```

These checks help confirm that the installation completed successfully.

---

## 12. Understanding Package Installation Locations

When a package is installed, its files may be placed in different directories.

Common locations include:

| Directory | Purpose |
|---|---|
| `/usr/bin/` | User commands and executable programs |
| `/usr/sbin/` | System administration commands |
| `/usr/lib/` | Application libraries |
| `/etc/` | Configuration files |
| `/usr/share/` | Architecture-independent application data |
| `/var/lib/` | Application state and package data |

The exact locations depend on the package.

You can inspect them using:

```bash
dpkg -L tree
```

---

## 13. Installing Software Safely

Package installation gives software access to your system.

Therefore, installation should be treated as a security decision.

### Recommended practices

1. Prefer official Kali repositories.
2. Check package names before installation.
3. Inspect package information using `apt show`.
4. Review the installation summary.
5. Avoid unknown third-party repositories.
6. Do not execute random installation scripts as root.
7. Verify installation after completion.
8. Keep your package information current.

### Inspect a package before installing

For example:

```bash
apt show tree
```

This can display:

- Package name.
- Version.
- Description.
- Dependencies.
- Package size.
- Maintainer information.

Package metadata is useful, but it does not replace verifying that the software source is trustworthy.

---

## 14. Common Installation Errors

### A. Unable to locate package

Example:

```text
E: Unable to locate package example
```

Possible causes:

- Incorrect package name.
- Package not available in configured repositories.
- Outdated package indexes.
- Repository configuration problems.

Try:

```bash
sudo apt update
```

Then check the package name:

```bash
apt search example
```

Replace `example` with the package you are investigating.

### B. Unable to correct problems

APT may report dependency or package conflict problems.

Inspect the package:

```bash
apt policy package-name
```

Avoid forcing installations without understanding the conflict.

### C. Permission denied

If you execute an installation command without administrative privileges, you may receive a permission error.

Use:

```bash
sudo apt install package-name
```

### D. No space left on device

Check available storage:

```bash
df -h
```

You may need to investigate which filesystem is full before attempting further installations.

### E. Network errors

If APT cannot download packages, investigate your network connection.

Useful commands:

```bash
ip addr
ip route
getent hosts http.kali.org
```

Do not change repository configuration simply because a temporary network error occurred.

---

## 15. Practical Lab: Installing Useful Linux Utilities

In this lab, you will install a few lightweight utilities from your configured repositories.

### Task 1: Refresh package information

```bash
sudo apt update
```

### Task 2: Inspect the tree package

```bash
apt show tree
```

### Task 3: Install tree

```bash
sudo apt install tree
```

Review the installation summary and confirm.

### Task 4: Verify installation

```bash
tree --version
```

### Task 5: Display your home directory

```bash
tree ~
```

### Task 6: Install curl

```bash
sudo apt install curl
```

Verify:

```bash
curl --version
```

### Task 7: Install wget

```bash
sudo apt install wget
```

Verify:

```bash
wget --version
```

### Task 8: Inspect installed package information

```bash
dpkg -s tree
```

### Task 9: Inspect installed package files

```bash
dpkg -L tree
```

### Task 10: Check available storage

```bash
df -h
```

---

## 16. Cybersecurity Connection

Linux security professionals frequently install tools for:

- Network troubleshooting.
- System monitoring.
- File analysis.
- Log investigation.
- Vulnerability assessment.
- Defensive security administration.

However, installing a security tool does not automatically make a system secure.

Every installed package increases the amount of software that must be maintained and monitored.

This is sometimes described as the system's **software attack surface**.

A good administrator installs only what is needed, uses trusted sources, and keeps software maintained.

---

## 17. Knowledge Check

Answer these questions in your own words.

1. What does `apt install` do?
2. Why is `sudo` normally required when installing packages?
3. What is a package dependency?
4. How can you install multiple packages in one command?
5. What does the `-y` option do?
6. How can you check whether a package is installed?
7. Which command displays the files installed by a package?
8. What does `Unable to locate package` mean?
9. Why should you inspect the installation summary?
10. How can installing unnecessary software affect system security?

---

## 18. Practical Assignment

Complete the following:

- [ ] Refresh package information using `sudo apt update`.
- [ ] Inspect `tree` using `apt show tree`.
- [ ] Install `tree`.
- [ ] Verify it using `tree --version`.
- [ ] Display your home directory using `tree ~`.
- [ ] Install `curl`.
- [ ] Install `wget`.
- [ ] Verify both installations.
- [ ] Inspect `tree` using `dpkg -s tree`.
- [ ] List its installed files using `dpkg -L tree`.
- [ ] Check storage using `df -h`.

## Lesson Summary

In this lesson, you learned:

- How APT installs packages.
- How to install individual and multiple packages.
- How dependencies are handled.
- How to inspect installation summaries.
- How to verify installed software.
- How to troubleshoot common installation errors.
- Why package installation is relevant to cybersecurity.

**Next Lesson: Module 09 – Lesson 07: Removing Packages with `apt remove` and `apt purge`.**