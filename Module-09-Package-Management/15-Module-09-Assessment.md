---
---

# Module 09 – Final Assessment: Linux Package Management

## 1. Assessment Information

**Module:** 09 – Linux Package Management

**Assessment Type:** Theory, Practical Commands, Troubleshooting, and Security

**Difficulty:** Beginner to Intermediate

**Total Marks:** 100

**Recommended Time:** 90–120 minutes

### Assessment Objectives

This assessment evaluates your understanding of:

- Linux package management.
- APT and DPKG.
- Repository management.
- Package installation and removal.
- Package upgrades.
- Package dependencies.
- DEB files.
- Troubleshooting.
- Package-management security.

---

# PART A: Multiple-Choice Questions (20 Marks)

**Instruction:** Choose the correct answer. Each question carries 2 marks.

### 1. What is the primary purpose of a Linux package manager?

A. Manage computer hardware.

B. Install, update, and remove software.

C. Create user accounts.

D. Manage network connections.

### 2. Which package manager is commonly used in Kali Linux?

A. RPM

B. Pacman

C. APT

D. Chocolatey

### 3. What does the following command do?

```bash
sudo apt update
```

A. Upgrades all installed packages.

B. Removes outdated packages.

C. Refreshes package information from repositories.

D. Deletes package caches.

### 4. Which command installs a package using APT?

A. `apt remove nano`

B. `sudo apt install nano`

C. `dpkg -r nano`

D. `apt list nano`

### 5. What is DPKG primarily responsible for?

A. Managing Debian packages locally.

B. Managing Windows applications.

C. Managing network routing.

D. Creating Linux users.

### 6. Which command removes a package and its package-managed configuration files?

A. `apt search nano`

B. `apt update nano`

C. `sudo apt purge nano`

D. `apt show nano`

### 7. What is a package dependency?

A. A software password.

B. Another software component required by a package.

C. A Linux user account.

D. A network protocol.

### 8. Which command displays the files installed by a package?

A. `dpkg -L nano`

B. `dpkg -r nano`

C. `apt update`

D. `apt purge nano`

### 9. What does this command do?

```bash
sudo dpkg -i package.deb
```

A. Removes a package.

B. Installs a local DEB package.

C. Searches repositories.

D. Updates package indexes.

### 10. Why should you install software from trusted repositories?

A. To make the terminal colorful.

B. To avoid using Linux commands.

C. To reduce the risk of installing malicious or modified software.

D. To increase screen resolution.

---

# PART B: Short-Answer Questions (20 Marks)

**Instruction:** Answer all questions. Each question carries 4 marks.

### 1. Explain the difference between APT and DPKG.

### 2. What is a Linux software repository? Explain its importance.

### 3. Differentiate between `apt remove` and `apt purge`.

### 4. Explain what package dependencies are and why they matter.

### 5. Explain two security risks associated with installing DEB packages from unknown sources.

---

# PART C: Linux Command Practical (30 Marks)

**Instruction:** Perform the following tasks in your Kali Linux virtual machine.

Each task carries 3 marks.

### Task 1: Check APT Version

Run a command to display the installed APT version.

### Task 2: Check DPKG Version

Run a command to display the installed DPKG version.

### Task 3: Update Package Indexes

Run the appropriate command to refresh package information.

### Task 4: Search for Software

Search the repositories for the `nano` package.

### Task 5: Inspect Package Information

Display detailed information about nano.

### Task 6: Inspect Installed Package Status

Use DPKG to check whether nano is installed.

### Task 7: List Package Files

Display the files installed by nano.

### Task 8: Inspect Package Dependencies

Display the dependencies of nano.

### Task 9: Check Package Upgrades

Display packages that have available upgrades.

### Task 10: Check Package Database Health

Run a command that checks for partially installed or inconsistent package states.

---

# PART D: Troubleshooting Scenarios (15 Marks)

**Instruction:** Read each scenario and provide the appropriate command or troubleshooting approach. Each question carries 5 marks.

### Scenario 1: Unable to Locate Package

You execute:

```bash
sudo apt install example-tool
```

The terminal displays:

```text
E: Unable to locate package example-tool
```

Answer:

1. What does the error mean?
2. State three possible causes.
3. Provide two commands you would use to investigate the problem.

### Scenario 2: Unmet Dependencies

You attempt to install a local DEB package, but DPKG reports missing dependencies.

Answer:

1. Why does this happen?
2. Why might APT be more appropriate for installing the local package?
3. Write the appropriate APT installation command.

### Scenario 3: Package Manager Lock

You attempt to install software but receive a package-manager lock error.

Answer:

1. What is a package-manager lock?
2. Why does Linux use it?
3. Write a command to inspect active APT and DPKG processes.
4. What should you do if another package operation is running?

---

# PART E: Cybersecurity and Package Security (15 Marks)

**Instruction:** Answer all questions. Each question carries 5 marks.

### 1. Repository Security

Explain how repository signatures help protect Linux systems from unauthorized repository metadata changes.

### 2. Software Supply-Chain Attacks

Explain what a software supply-chain attack is and provide one example of how it could affect a Linux system.

### 3. Security Investigation

You discover an unfamiliar package installed on a Linux machine.

Describe five steps you would take to investigate it using package-management tools.

---

# 2. Practical Assessment Evidence

Create a directory for your assessment evidence:

```bash
mkdir -p ~/module09-lab/assessment
```

Create a text file:

```bash
nano ~/module09-lab/assessment/assessment-results.txt
```

Record the following:

1. Your APT version.
2. Your DPKG version.
3. The result of `apt policy nano`.
4. The result of `dpkg -s nano`.
5. The result of `apt depends nano`.
6. The result of `sudo dpkg --audit`.
7. Your observations about configured repositories.
8. Five package-management security practices.

Save the file using:

- `CTRL + O`
- `ENTER`
- `CTRL + X`

Verify that it exists:

```bash
cat ~/module09-lab/assessment/assessment-results.txt
```

Do not include passwords, private keys, or other sensitive information in your assessment evidence.

---

# 3. Marking Scheme

| Section | Marks |
|---|---:|
| Part A – Multiple Choice | 20 |
| Part B – Short Answers | 20 |
| Part C – Practical Commands | 30 |
| Part D – Troubleshooting | 15 |
| Part E – Cybersecurity | 15 |
| **Total** | **100** |

## Performance Guide

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Requires Improvement |

**Pass mark: 50/100**

---

# 4. Final Revision Checklist

Before considering Module 09 complete, make sure you can explain and use:

- [ ] APT and DPKG.
- [ ] Linux software repositories.
- [ ] `apt update`.
- [ ] `apt install`.
- [ ] `apt remove`.
- [ ] `apt purge`.
- [ ] `apt upgrade`.
- [ ] `apt search`.
- [ ] `apt show`.
- [ ] `apt policy`.
- [ ] `apt depends`.
- [ ] `dpkg -i`.
- [ ] `dpkg -l`.
- [ ] `dpkg -s`.
- [ ] `dpkg -L`.
- [ ] `dpkg -S`.
- [ ] DEB package inspection.
- [ ] Dependency troubleshooting.
- [ ] Package security and repository verification.

---

# 5. Final Reflection

After completing this module, answer these questions:

1. What is the most important thing you learned?
2. Which package-management command do you find most useful?
3. Which topic was the most difficult?
4. How can package management help a cybersecurity professional?
5. What would you like to improve before moving to the next module?

## Congratulations!

You have reached the end of **Module 09 – Linux Package Management**.

You have studied how Linux software is installed, updated, removed, inspected, verified, and secured.

**Next Module: Module 10 – Linux Networking.**

This module will introduce the networking knowledge needed for Linux administration, network troubleshooting, and cybersecurity.