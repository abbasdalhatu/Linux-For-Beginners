# Module 09 – Lesson 08: Upgrading Packages with APT

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain why Linux packages need upgrading.
2. Understand how `apt upgrade` works.
3. Differentiate between `apt update` and `apt upgrade`.
4. Understand the difference between `upgrade` and `full-upgrade`.
5. Identify packages that have available upgrades.
6. Perform package upgrades safely.
7. Understand the security importance of software updates.

---

## 2. What Is Package Upgrading?

Package upgrading is the process of replacing installed software versions with newer available versions.

Software developers regularly release updates to:

- Fix security vulnerabilities.
- Correct software bugs.
- Improve performance.
- Add features.
- Improve compatibility.
- Fix stability problems.

For example, suppose you have an older version of a software package installed.

The repository later provides a newer version.

APT can download and install the newer version.

This process is called package upgrading.

---

## 3. Understanding `apt upgrade`

The command:

```bash
sudo apt upgrade
```

upgrades installed packages when newer versions are available and the upgrade can be performed without removing installed packages.

APT checks the current package information, calculates the required changes, and presents an upgrade summary.

It may ask for confirmation before continuing.

### Example

```bash
sudo apt upgrade
```

You may see output similar to:

```text
The following packages will be upgraded:
  curl wget nano

3 upgraded, 0 newly installed, 0 to remove.
Do you want to continue? [Y/n]
```

This is illustrative output. Your actual results will depend on your installed packages and repositories.

Review the proposed changes before confirming.

---

## 4. Difference Between `apt update` and `apt upgrade`

These commands are related but perform different operations.

### A. `apt update`

```bash
sudo apt update
```

Refreshes local package information from configured repositories.

It does not normally upgrade installed software.

### B. `apt upgrade`

```bash
sudo apt upgrade
```

Installs newer versions of eligible packages that are already installed.

### Comparison

| Feature | `apt update` | `apt upgrade` |
|---|---|---|
| Refreshes package indexes | Yes | No |
| Installs newer software versions | No | Yes |
| Changes installed packages | No | Yes |
| Helps identify available upgrades | Yes | Uses available package information |

### Easy way to remember

- `update` = refresh the software catalogue.
- `upgrade` = upgrade installed software.

### Recommended basic workflow

```bash
sudo apt update
sudo apt upgrade
```

Run them in that order.

---

## 5. Understanding the Upgrade Process

When you execute:

```bash
sudo apt upgrade
```

APT generally performs these operations:

1. Reads current package information.
2. Checks installed packages.
3. Compares installed versions with available versions.
4. Calculates which packages can be upgraded.
5. Resolves dependency requirements.
6. Displays the proposed changes.
7. Downloads required package files.
8. Coordinates installation through DPKG.
9. Configures the upgraded packages.

This is why APT is useful for maintaining Debian-based Linux systems.

---

## 6. Checking Available Upgrades Before Installing

Before upgrading packages, you should inspect what is available.

### A. Refresh package information

```bash
sudo apt update
```

### B. List upgradeable packages

```bash
apt list --upgradable
```

This displays installed packages for which newer versions are available according to the current package information.

### C. Inspect a specific package

```bash
apt policy nano
```

This displays information about the installed version, candidate version, and available versions.

### D. Inspect package details

```bash
apt show nano
```

This displays package metadata such as its description, dependencies, version, and size.

These commands help you understand what may change before performing an upgrade.

---

## 7. Understanding `apt upgrade` and `apt full-upgrade`

APT provides another important command:

```bash
sudo apt full-upgrade
```

The difference is important.

### A. Normal upgrade

```bash
sudo apt upgrade
```

Upgrades installed packages without removing installed packages.

If an upgrade requires removing an installed package, APT may hold back that upgrade.

### B. Full upgrade

```bash
sudo apt full-upgrade
```

Allows APT to resolve some upgrade situations by installing new dependencies or removing existing packages when necessary.

This can make more substantial system changes.

### Comparison

| Feature | `apt upgrade` | `apt full-upgrade` |
|---|---|---|
| Upgrades packages | Yes | Yes |
| Installs new dependencies when needed | May not perform changes requiring them | Yes |
| Can remove installed packages | No, not as part of its normal upgrade operation | Yes |
| Potential system changes | More limited | More extensive |

### Important warning

Do not run `full-upgrade` blindly.

On Kali Linux, upgrades can involve significant package changes.

Always inspect the proposed changes, especially the list of packages scheduled for removal.

---

## 8. Understanding Held-Back Packages

Sometimes APT does not upgrade a package during a normal upgrade.

This may happen when:

- New dependencies are required.
- Package relationships have changed.
- The upgrade would require removing another package.
- The package is intentionally held.

You can inspect held packages with:

```bash
apt-mark showhold
```

If a package is held, APT may avoid upgrading it automatically.

Do not remove a hold unless you understand why it was applied.

### Inspect a specific package

```bash
apt policy package-name
```

Replace `package-name` with the package you want to investigate.

---

## 9. Understanding Upgrade Summaries

Before installing updates, APT may show a summary.

Example:

```text
The following packages will be upgraded:
  package-a package-b package-c

3 upgraded, 0 newly installed, 0 to remove.
Need to get 25.0 MB of archives.
After this operation, 5 MB of additional disk space will be used.
Do you want to continue? [Y/n]
```

Understand the important parts:

| Output | Meaning |
|---|---|
| `upgraded` | Existing packages receiving newer versions |
| `newly installed` | Additional packages being installed |
| `to remove` | Packages that will be removed |
| `Need to get` | Approximate download size |
| `additional disk space` | Expected storage change |

If APT proposes unexpected removals, stop and investigate.

---

## 10. Checking System Storage Before Upgrading

Large upgrades may require significant storage.

Check available space:

```bash
df -h
```

Pay particular attention to the filesystem containing `/`.

You can also inspect the size of APT's package cache:

```bash
sudo du -sh /var/cache/apt/archives/
```

This helps you understand how much storage is occupied by downloaded package archives.

Do not delete files manually from system directories without understanding their purpose.

---

## 11. Security Importance of Package Upgrades

One of the most important reasons to upgrade software is security.

Software vulnerabilities can allow attackers to:

- Execute malicious code.
- Escalate privileges.
- Access sensitive information.
- Disrupt services.
- Compromise systems.

Developers release security updates to address known vulnerabilities.

For example:

Imagine a network utility has a vulnerability that allows remote code execution.

A corrected package version becomes available.

Upgrading the affected package may provide the security fix.

However, installing an update does not guarantee that every vulnerability has been eliminated.

Administrators must also:

- Monitor security advisories.
- Check package versions.
- Review system configurations.
- Remove unnecessary software.
- Monitor system logs.

---

## 12. Common Upgrade Errors

### A. Package dependency problems

APT may report that some packages have unmet dependencies.

Possible causes include:

- Incomplete previous installations.
- Repository inconsistencies.
- Conflicting package versions.

Inspect the package:

```bash
apt policy package-name
```

You can also inspect package database problems:

```bash
sudo dpkg --audit
```

### B. Network connection problems

APT may fail to download packages.

Check your network:

```bash
ip addr
```

Inspect the default route:

```bash
ip route
```

Test DNS:

```bash
getent hosts http.kali.org
```

### C. Insufficient storage

Check available space:

```bash
df -h
```

### D. Repository signature errors

If APT reports signature verification problems, investigate the repository configuration and trusted signing information.

Do not disable signature verification to force an upgrade.

### E. Interrupted package operations

If an installation or upgrade was interrupted, some packages may remain partially configured.

Inspect the package database:

```bash
sudo dpkg --audit
```

Do not immediately run repair commands without first understanding the reported problem.

---

## 13. A Safe Package Upgrade Workflow

Use this workflow when maintaining your Kali system.

### Step 1: Check available storage

```bash
df -h
```

### Step 2: Refresh package information

```bash
sudo apt update
```

### Step 3: Inspect available upgrades

```bash
apt list --upgradable
```

### Step 4: Inspect important package policies

```bash
apt policy nano
```

### Step 5: Perform a normal upgrade

```bash
sudo apt upgrade
```

Review the proposed changes before confirming.

### Step 6: Verify package versions

```bash
apt policy nano
```

### Step 7: Inspect package status

```bash
dpkg -s nano
```

This workflow helps you upgrade software while understanding the changes being made.

---

## 14. Practical Lab: Understanding Package Upgrades

We will first inspect your system and then perform a normal package upgrade.

### Task 1: Check storage

```bash
df -h
```

### Task 2: Refresh package information

```bash
sudo apt update
```

Wait for the operation to complete.

### Task 3: List upgradeable packages

```bash
apt list --upgradable
```

Observe whether updates are available.

### Task 4: Inspect nano

```bash
apt policy nano
```

Record the installed and candidate versions, if available.

### Task 5: Inspect held packages

```bash
apt-mark showhold
```

If nothing is displayed, there may be no packages currently held.

### Task 6: Perform a normal upgrade

```bash
sudo apt upgrade
```

Review the summary.

If the proposed changes are reasonable, confirm the operation.

If you see unexpected removals or other concerning changes, cancel and investigate.

### Task 7: Verify nano

```bash
nano --version
```

### Task 8: Inspect its package status

```bash
dpkg -s nano
```

### Task 9: Check storage again

```bash
df -h
```

Compare the available storage with your earlier result.

**Note:** If no packages need upgrading, that is a valid result. Do not install unnecessary software just to create an upgrade exercise.

---

## 15. Knowledge Check

Answer these questions in your own words.

1. What is package upgrading?
2. What does `apt upgrade` do?
3. What is the difference between `apt update` and `apt upgrade`?
4. What is the purpose of `apt full-upgrade`?
5. Why can `full-upgrade` be more risky than a normal upgrade?
6. What are held-back packages?
7. Which command lists upgradeable packages?
8. Which command displays held packages?
9. Why should you inspect the upgrade summary?
10. How do package upgrades contribute to system security?

---

## 16. Practical Assignment

Complete the following tasks:

- [ ] Check available storage using `df -h`.
- [ ] Refresh package information using `sudo apt update`.
- [ ] List upgradeable packages using `apt list --upgradable`.
- [ ] Inspect `nano` using `apt policy nano`.
- [ ] Check held packages using `apt-mark showhold`.
- [ ] Run `sudo apt upgrade`.
- [ ] Review the proposed changes before confirming.
- [ ] Verify `nano` using `nano --version`.
- [ ] Inspect the package using `dpkg -s nano`.
- [ ] Check storage again.
- [ ] Write down the difference between `upgrade` and `full-upgrade`.

---

## Lesson Summary

In this lesson, you learned:

- What package upgrading means.
- How `apt upgrade` works.
- The difference between updating and upgrading.
- The difference between `upgrade` and `full-upgrade`.
- How to inspect available updates.
- How held packages work.
- How to troubleshoot common upgrade problems.
- Why software updates are essential for cybersecurity.

**Next Lesson: Module 09 – Lesson 09: Searching and Inspecting Packages.**