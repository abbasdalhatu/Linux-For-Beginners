
# Module 13 – Lesson 09: Security Updates and Patching

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain software updates and security patches.
2. Understand why patch management matters.
3. Identify common software vulnerabilities.
4. Understand how Linux package updates work.
5. Use APT to inspect available updates.
6. Understand security advisories.
7. Explain CVE and CVSS.
8. Understand kernel updates.
9. Identify risks associated with outdated software.
10. Perform a Linux security patch audit.

---

## 2. What Are Software Updates?

A software update is a newer version of a program or system component released by its developer or distributor.

Updates may introduce:

- New features.
- Performance improvements.
- Bug fixes.
- Security fixes.
- Compatibility improvements.

### Simple Example

Imagine your browser has a security vulnerability.

Its developer releases an updated version that fixes the problem.

Installing that update helps protect your computer from attacks that exploit the vulnerability.

---

## 3. What Is a Security Patch?

A security patch is a software update specifically intended to correct a security weakness.

For example:

A Linux application contains a vulnerability that allows an attacker to execute unauthorised code.

The software maintainer develops a fix and releases it as a security update.

Installing the patch helps close that vulnerability.

### Update vs Patch

| Software Update | Security Patch |
|---|---|
| General improvement or change | Fixes a security weakness |
| May include new features | Primarily addresses security issues |
| Can include bug fixes | Can include vulnerability fixes |
| May or may not be security-related | Security-focused |

A security patch can be distributed as part of a larger software update.

---

## 4. Why Security Updates Matter

Outdated software can expose Linux systems to known vulnerabilities.

Attackers may search for systems running vulnerable versions of:

- Linux kernels.
- Web servers.
- SSH services.
- Browsers.
- Database systems.
- Programming language runtimes.
- Network utilities.

### Consequences of Unpatched Vulnerabilities

1. Unauthorised access.
2. Privilege escalation.
3. Information disclosure.
4. Denial of service.
5. Remote code execution.
6. Data theft.
7. System compromise.

### Real-World Example

Imagine a university server running an outdated web application.

A security researcher identifies a vulnerability and the developer releases a fix.

If the university does not install the update, attackers may continue targeting the known weakness.

This is why patch management is an essential security responsibility.

---

## 5. Understanding Vulnerabilities

A vulnerability is a weakness in software, hardware, configuration, or system design that could be exploited to compromise security.

Examples include:

- Improper input validation.
- Weak authentication.
- Memory corruption.
- Insecure file permissions.
- Incorrect access controls.
- Outdated software components.

### Vulnerability Lifecycle

```text
Vulnerability Discovered
          |
          v
Security Analysis
          |
          v
Fix Developed
          |
          v
Security Patch Released
          |
          v
Patch Tested
          |
          v
Patch Installed
          |
          v
System Verified
```

Not every vulnerability follows exactly the same lifecycle, but this illustrates a common patch-management process.

---

## 6. Understanding CVE

CVE stands for **Common Vulnerabilities and Exposures**.

It is a system for identifying publicly known cybersecurity vulnerabilities.

Each CVE has a unique identifier.

Example format:

```text
CVE-2026-12345
```

This is an illustrative identifier, not a claim about a real vulnerability.

### CVE Identifier Structure

| Part | Meaning |
|---|---|
| CVE | Common Vulnerabilities and Exposures |
| 2026 | Year associated with the identifier |
| 12345 | Identifier number |

### Why CVE Matters

CVE helps:

- Identify known vulnerabilities.
- Share security information.
- Track security issues.
- Coordinate vulnerability management.
- Reference vulnerabilities in security reports.

When investigating a vulnerability, always verify the actual CVE details and affected software versions.

---

## 7. Understanding CVSS

CVSS stands for **Common Vulnerability Scoring System**.

It provides a standardised way of assessing vulnerability severity.

The commonly used CVSS v3.1 and v4.0 scales range from 0.0 to 10.0.

### CVSS v3.1 Severity Categories

| Score | Severity |
|---|---|
| 0.0 | None |
| 0.1–3.9 | Low |
| 4.0–6.9 | Medium |
| 7.0–8.9 | High |
| 9.0–10.0 | Critical |

CVSS v4.0 uses different severity bands, so always identify the version when interpreting a score.

### Factors That Affect Vulnerability Severity

- Attack complexity.
- Required privileges.
- User interaction.
- Network accessibility.
- Impact on confidentiality.
- Impact on integrity.
- Impact on availability.

### Important Security Principle

A vulnerability's score is not the only factor that determines patch priority.

Administrators should also consider:

- Whether the system is exposed.
- Whether exploitation is known to occur.
- Whether the vulnerability affects critical services.
- Whether a working mitigation exists.
- Whether the affected software is actually installed.

---

## 8. Understanding APT Package Management

Kali Linux is Debian-based and uses APT for package management.

APT helps administrators:

- Install packages.
- Remove packages.
- Upgrade packages.
- Retrieve package information.
- Manage dependencies.

### 8.1 Check APT Version

```bash
apt --version
```

### 8.2 Inspect Configured Repositories

```bash
grep -RhsE '^[[:space:]]*deb ' /etc/apt/sources.list /etc/apt/sources.list.d/ 2>/dev/null
```

This displays traditional repository entries. Modern Deb822 `.sources` files may require separate inspection.

### 8.3 Update Package Information

```bash
sudo apt update
```

This refreshes package information from configured repositories.

It does not automatically upgrade installed packages.

### 8.4 List Available Upgrades

```bash
apt list --upgradable
```

This displays packages with available newer versions according to the refreshed package information.

### 8.5 Simulate an Upgrade

```bash
sudo apt-get -s upgrade
```

The `-s` option simulates the operation rather than applying the upgrade.

This helps administrators inspect what an upgrade would do.

### 8.6 Upgrade Packages

The following command performs actual package upgrades:

```bash
sudo apt upgrade
```

Do not run this command automatically as part of this inspection lesson. Review the proposed changes, package sources, storage availability, and recovery arrangements before performing real upgrades.

---

## 9. Understanding Different APT Upgrade Commands

| Command | Purpose |
|---|---|
| `apt update` | Refreshes repository package information |
| `apt upgrade` | Upgrades installed packages |
| `apt-get upgrade` | Upgrades installed packages using APT |
| `apt full-upgrade` | Can install or remove packages to complete upgrades |
| `apt list --upgradable` | Lists available package upgrades |
| `apt show package` | Displays package information |
| `apt-cache policy package` | Shows package versions and repository priorities |

### Important Difference

`apt update` does not mean the operating system has been upgraded.

It only refreshes the information APT uses to determine which package versions are available.

---

## 10. Security Updates and Repository Trust

Linux distributions distribute packages through software repositories.

Repositories provide package metadata and software packages.

APT uses repository authentication mechanisms to verify package integrity and origin according to the configured trust system.

### Why Repository Trust Matters

If an attacker can manipulate the software supply chain, they may attempt to distribute malicious packages.

Good practices include:

1. Use trusted repositories.
2. Protect repository signing keys.
3. Avoid unknown package sources.
4. Review repository configuration.
5. Keep package management tools updated.
6. Investigate repository authentication warnings.

Never bypass package-signature verification simply to force an installation.

---

## 11. Kernel Security Updates

The Linux kernel is the core component of the operating system.

It manages:

- Processes.
- Memory.
- Hardware.
- Filesystems.
- Network operations.
- System calls.

Kernel vulnerabilities can have serious consequences because the kernel operates with high privileges.

### Inspect Your Running Kernel

```bash
uname -r
```

### Display Kernel Information

```bash
uname -a
```

### Inspect Installed Kernel Packages

```bash
dpkg -l | grep -E 'linux-image|linux-headers'
```

### Why Kernel Updates Matter

Kernel updates may fix vulnerabilities involving:

- Privilege escalation.
- Memory corruption.
- Network stack weaknesses.
- Filesystem security.
- Hardware interaction.

Some kernel updates require a reboot before the new kernel becomes active.

Do not remove your currently running kernel during a routine security audit.

---

## 12. Understanding Patch Management

Patch management is the process of identifying, evaluating, testing, deploying, and verifying software updates.

### Common Patch Management Stages

**Stage 1: Asset Identification**

Identify systems and installed software.

**Stage 2: Vulnerability Identification**

Determine which software components have known security weaknesses.

**Stage 3: Risk Assessment**

Determine the importance and urgency of the update.

**Stage 4: Testing**

Test patches in a controlled environment.

**Stage 5: Deployment**

Install approved patches.

**Stage 6: Verification**

Confirm that the updates were installed successfully.

**Stage 7: Documentation**

Record the work performed.

### Patch Management Flow

```text
Identify Systems
       |
       v
Inspect Installed Software
       |
       v
Identify Vulnerabilities
       |
       v
Assess Risk
       |
       v
Test Patches
       |
       v
Deploy Updates
       |
       v
Verify Installation
       |
       v
Document Results
```

---

## 13. Common Patch Management Risks

### Risk 1: Delayed Security Updates

Attackers may exploit vulnerabilities that already have fixes.

### Risk 2: Untrusted Repositories

Packages from unknown sources may introduce malicious software.

### Risk 3: Untested Updates

An update may affect compatibility or application functionality.

### Risk 4: Insufficient Storage

Updates may fail when disk space is insufficient.

### Risk 5: Incomplete Updates

Interrupted package operations can leave packages in an incomplete state.

### Risk 6: Reboot Requirements

Some updates do not become fully active until affected services or the system are restarted.

### Defensive Recommendations

- Establish a patch schedule.
- Prioritise critical vulnerabilities.
- Use trusted repositories.
- Test important updates.
- Maintain backups.
- Monitor disk space.
- Verify successful installation.
- Record update history.

---

# 14. Practical Lab: Linux Security Patch Audit

## Lab Objective

Inspect your Kali Linux package management environment, installed kernel, available updates, repository configuration, and storage capacity.

This is primarily an inspection exercise.

Do not perform a full system upgrade during this lesson.

### Step 1: Create Your Workspace

```bash
mkdir -p ~/module13-lab/lesson09
cd ~/module13-lab/lesson09
```

### Step 2: Inspect Your Linux Distribution

```bash
cat /etc/os-release
```

### Step 3: Inspect the APT Version

```bash
apt --version
```

### Step 4: Inspect Configured Repositories

```bash
grep -RhsE '^[[:space:]]*deb ' /etc/apt/sources.list /etc/apt/sources.list.d/ 2>/dev/null
```

If your system uses `.sources` files, inspect their repository configuration separately.

### Step 5: Inspect the Running Kernel

```bash
uname -r
```

### Step 6: Inspect Installed Kernel Packages

```bash
dpkg -l | grep -E 'linux-image|linux-headers'
```

### Step 7: Inspect Available Package Updates

```bash
apt list --upgradable
```

This information may be outdated if your package lists have not recently been refreshed.

### Step 8: Simulate a Package Upgrade

```bash
sudo apt-get -s upgrade
```

Review the proposed package changes without applying them.

### Step 9: Inspect Storage Capacity

```bash
df -h
```

Pay particular attention to the filesystem containing your root directory.

### Step 10: Inspect Recent Package Management Logs

```bash
sudo journalctl -b --no-pager | grep -Ei 'apt|dpkg' | tail -n 30
```

You can also inspect package history where available:

```bash
ls -l /var/log/apt/
```

### Step 11: Generate Your Patch Security Report

Run:

```bash
{
    echo "LINUX SECURITY UPDATES AND PATCH AUDIT"
    echo "======================================="
    echo
    echo "OPERATING SYSTEM"
    cat /etc/os-release
    echo
    echo "APT VERSION"
    apt --version
    echo
    echo "CONFIGURED TRADITIONAL REPOSITORIES"
    grep -RhsE '^[[:space:]]*deb ' /etc/apt/sources.list /etc/apt/sources.list.d/ 2>/dev/null
    echo
    echo "RUNNING KERNEL"
    uname -r
    echo
    echo "INSTALLED KERNEL PACKAGES"
    dpkg -l | grep -E 'linux-image|linux-headers'
    echo
    echo "AVAILABLE PACKAGE UPDATES"
    apt list --upgradable 2>/dev/null
    echo
    echo "SIMULATED PACKAGE UPGRADE"
    sudo apt-get -s upgrade
    echo
    echo "STORAGE CAPACITY"
    df -h
    echo
    echo "RECENT PACKAGE MANAGEMENT EVENTS"
    sudo journalctl -b --no-pager | grep -Ei 'apt|dpkg' | tail -n 30
    echo
    echo "APT LOG DIRECTORY"
    ls -l /var/log/apt/
} > patch-security-report.txt 2>&1
```

Inspect the report:

```bash
cat patch-security-report.txt
```

### Expected Files

```text
lesson09/
└── patch-security-report.txt
```

---

## 15. Real-World Cybersecurity Application

Imagine you are responsible for maintaining Linux servers at a university.

Your security audit discovers:

- Some servers have outdated software.
- One server runs an old kernel.
- Several applications have known vulnerabilities.
- Some machines use unknown software repositories.
- Patch installation is not documented.

Your recommendations should include:

1. Inventory all systems.
2. Identify vulnerable software.
3. Prioritise critical security updates.
4. Review repository trust.
5. Test patches before deployment.
6. Install approved updates.
7. Verify successful installation.
8. Reboot when necessary and authorised.
9. Maintain patch records.

This is how security teams reduce the risks associated with outdated software.

---

## 16. Knowledge Check

Answer these questions in your notebook.

1. What is a software update?
2. What is a security patch?
3. Why is patch management important?
4. What is a software vulnerability?
5. What does CVE stand for?
6. What is CVSS?
7. What is the difference between CVE and CVSS?
8. Why is an unpatched Linux kernel dangerous?
9. What is the purpose of `apt update`?
10. What does `apt upgrade` do?
11. What is the difference between `apt update` and `apt upgrade`?
12. What does `apt list --upgradable` display?
13. What is the purpose of software repositories?
14. Why should unknown repositories be avoided?
15. What is patch management?
16. Why should patches be tested?
17. Why might a kernel update require a reboot?
18. Identify five risks of poor patch management.

---

## 17. Practical Completion Checklist

- [ ] I understand software updates.
- [ ] I understand security patches.
- [ ] I understand software vulnerabilities.
- [ ] I understand CVE.
- [ ] I understand CVSS.
- [ ] I understand APT package management.
- [ ] I can inspect configured repositories.
- [ ] I can list available package updates.
- [ ] I understand kernel updates.
- [ ] I understand patch management.
- [ ] I can inspect package management logs.
- [ ] I completed the patch security audit.
- [ ] I generated my patch security report.

**Lesson 09 Complete: Security Updates and Patching.**
