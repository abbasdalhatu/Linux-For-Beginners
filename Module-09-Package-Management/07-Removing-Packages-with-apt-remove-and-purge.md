# Module 09 – Lesson 07: Removing Packages with `apt remove` and `apt purge`

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain why Linux packages are removed.
2. Remove software using `apt remove`.
3. Completely remove package configuration files using `apt purge`.
4. Understand the difference between `remove` and `purge`.
5. Identify and remove unnecessary dependencies safely.
6. Verify package removal.
7. Understand the security importance of software removal.

---

## 2. What Is Package Removal?

Package removal is the process of uninstalling software from a Linux system.

Sometimes you install an application for testing and no longer need it.

Other times, software may become:

- Unnecessary.
- Outdated.
- Unsupported.
- Vulnerable.
- Replaced by another application.

Linux provides package-management commands that allow administrators to remove software safely.

For Debian-based systems such as Kali Linux, APT provides two important commands:

```bash
sudo apt remove package-name
```

And:

```bash
sudo apt purge package-name
```

Although both remove software, they handle configuration files differently.

---

## 3. Understanding `apt remove`

The command:

```bash
sudo apt remove tree
```

removes the installed `tree` package.

However, it generally leaves system-wide configuration files associated with the package behind.

This can be useful when you may want to reinstall the software later and preserve its configuration.

### Example

```bash
sudo apt remove tree
```

APT will calculate the removal plan and show which packages will be removed.

It may ask:

```text
Do you want to continue? [Y/n]
```

Review the proposed changes before confirming.

Type `Y` and press Enter to proceed.

---

## 4. Understanding `apt purge`

The command:

```bash
sudo apt purge tree
```

removes the package and its package-managed system configuration files.

### Why use purge?

Use purge when you want to remove the package and its associated system configuration files rather than retain them.

However, remember:

- Purge does not necessarily delete files created manually by the user.
- Files stored in a home directory may remain.
- User-created data may need separate investigation.
- Not every application stores all its settings in package-managed locations.

### Example

```bash
sudo apt purge tree
```

APT will show the packages and configuration changes involved before proceeding.

---

## 5. Difference Between `apt remove` and `apt purge`

This is one of the most important distinctions in package management.

| Feature | `apt remove` | `apt purge` |
|---|---|---|
| Removes installed package | Yes | Yes |
| Removes package-managed system configuration | Generally no | Yes |
| May leave configuration files | Yes | Generally no |
| Removes manually created user files | No | No |
| Useful for reinstalling while retaining configuration | Yes | No, not generally |

### Easy way to remember

- `remove` = remove the application but generally retain package configuration.
- `purge` = remove the application and its package-managed configuration.

---

## 6. Understanding Package Status After Removal

DPKG tracks the state of installed packages.

You can inspect package information with:

```bash
dpkg -l tree
```

After using `apt remove`, you may see a status similar to:

```text
rc  tree
```

The status `rc` means:

- `r` = package removal requested.
- `c` = configuration files remain.

After a successful purge, the package's configuration files should also be removed.

The package may no longer appear in the normal installed-package listing.

You can inspect package state using:

```bash
dpkg -s tree
```

If the package is no longer installed, DPKG will report that information.

---

## 7. Understanding Unused Dependencies

When APT installs an application, it may also install additional packages required by that application.

These are called dependencies.

Consider:

```text
Application A
    |
    +---- Library B
    |
    +---- Library C
    |
    +---- Utility D
```

Suppose Application A is removed and Library B, Library C, and Utility D are no longer required by any installed software.

APT may identify them as automatically installed packages that are no longer needed.

These are called orphaned or unused dependencies.

### Inspect unused dependencies

```bash
sudo apt autoremove
```

This command calculates which automatically installed packages can be removed.

It normally asks for confirmation.

**Important:** Always review the proposed removal list. Do not approve it if it includes packages you still need.

### Understand the difference

| Command | Purpose |
|---|---|
| `apt remove` | Removes a package |
| `apt purge` | Removes a package and its package-managed configuration |
| `apt autoremove` | Removes automatically installed packages that are no longer needed |

---

## 8. Understanding `autoremove --purge`

APT also supports:

```bash
sudo apt autoremove --purge
```

This removes eligible unused automatically installed packages and purges their package-managed configuration files.

This command can make more changes than a simple package removal.

Therefore, inspect the proposed list carefully before confirming.

For this course, you do not need to execute it unless you have reviewed the proposed changes and understand them.

---

## 9. Removing Multiple Packages

APT allows you to remove several packages in one command.

Example:

```bash
sudo apt remove tree curl wget
```

This requests removal of all three packages.

You can also purge multiple packages:

```bash
sudo apt purge tree curl wget
```

Before proceeding, review the full removal summary.

Avoid removing important system packages simply because you do not recognize their names.

---

## 10. Checking Whether a Package Is Installed

Before removing software, inspect its installation status.

### A. Check package information

```bash
dpkg -s tree
```

### B. Check package listing

```bash
dpkg -l tree
```

### C. Check the executable location

```bash
command -v tree
```

### D. Inspect package information through APT

```bash
apt policy tree
```

These commands help you determine whether the package is installed and what information is available about it.

---

## 11. Safe Package Removal Workflow

A responsible administrator should follow a predictable process.

### Step 1: Identify the package

```bash
apt show tree
```

### Step 2: Check whether it is installed

```bash
dpkg -s tree
```

### Step 3: Decide whether to remove or purge

Use `remove` if you want to retain package-managed configuration files.

Use `purge` if you want to remove the package and those configuration files.

### Step 4: Review the removal plan

```bash
sudo apt remove tree
```

Read the output before confirming.

### Step 5: Verify the result

```bash
dpkg -l tree
```

### Step 6: Check unused dependencies

```bash
sudo apt autoremove
```

Review the proposed list before confirming.

---

## 12. Common Package Removal Errors

### A. Package is not installed

You may see:

```text
Package 'tree' is not installed
```

This means the requested package is not currently installed.

Check:

```bash
dpkg -l tree
```

### B. Permission denied

You may receive a permission error if you attempt to remove system software without administrative privileges.

Use:

```bash
sudo apt remove package-name
```

### C. Dependency problems

Removing one package may affect other packages that depend on it.

APT normally checks dependency relationships and may refuse an unsafe operation.

Do not force removal without understanding the consequences.

### D. Disk or package database problems

APT may fail because of package database problems or insufficient storage.

Useful inspection commands include:

```bash
df -h
```

And:

```bash
sudo dpkg --audit
```

The second command checks for partially installed or inconsistent package states.

---

## 13. Cybersecurity Connection: Removing Unnecessary Software

Every installed application represents software that may need maintenance.

Some applications expose:

- Network services.
- Open ports.
- Vulnerable libraries.
- Administrative interfaces.
- Background processes.

If software is unnecessary, removing it can help reduce the system's attack surface.

This is related to the cybersecurity principle of **minimizing the attack surface**.

However, do not remove packages blindly.

A package may be required by another application or by the operating system.

A good administrator removes unnecessary software only after checking its purpose and dependencies.

---

## 14. Practical Lab: Removing Installed Utilities

In Lesson 06, you installed:

- `tree`
- `curl`
- `wget`

We will use these packages for a controlled removal exercise.

### Task 1: Check the installed packages

```bash
dpkg -s tree
```

```bash
dpkg -s curl
```

```bash
dpkg -s wget
```

### Task 2: Check the executable locations

```bash
command -v tree
```

```bash
command -v curl
```

```bash
command -v wget
```

### Task 3: Remove tree

```bash
sudo apt remove tree
```

Review the removal summary and confirm.

### Task 4: Verify removal

```bash
dpkg -l tree
```

Also try:

```bash
command -v tree
```

The executable should no longer be available through the normal command search path.

### Task 5: Inspect its package state

```bash
dpkg -s tree
```

If removal succeeded, it should report that the package is no longer installed.

### Task 6: Inspect unused dependencies

```bash
sudo apt autoremove
```

Read the proposed removal list carefully.

You may cancel with `n` if you are uncertain or if the proposed changes are unexpected.

### Task 7: Purge the remaining configuration, if any

If the package still has an `rc` status, you can use:

```bash
sudo apt purge tree
```

If it has already been fully removed and purged, there may be nothing left to do.

### Task 8: Verify the final state

```bash
dpkg -l tree
```

---

## 15. Knowledge Check

Answer these questions in your own words.

1. What is package removal?
2. What does `apt remove` do?
3. What does `apt purge` do?
4. What is the difference between remove and purge?
5. What does the DPKG status `rc` mean?
6. What does `apt autoremove` do?
7. Why should you inspect the removal summary?
8. Can purge delete manually created files in a user's home directory?
9. Why can unnecessary software increase security risks?
10. Why should you avoid removing unknown system packages?

---

## 16. Practical Assignment

Complete the following tasks:

- [ ] Check whether `tree` is installed.
- [ ] Inspect `curl` and `wget`.
- [ ] Remove `tree` using `apt remove`.
- [ ] Verify the removal using `dpkg -l tree`.
- [ ] Inspect the package status.
- [ ] Inspect unused dependencies using `apt autoremove`.
- [ ] Review the proposed removal list.
- [ ] Purge `tree` if residual package configuration remains.
- [ ] Verify the final package state.
- [ ] Write down three differences between `apt remove` and `apt purge`.

## Lesson Summary

In this lesson, you learned:

- How to remove Linux packages.
- How `apt remove` works.
- How `apt purge` works.
- The difference between package removal and purging.
- How to identify unused dependencies.
- How to verify package removal.
- Why removing unnecessary software can improve system security.

**Next Lesson: Module 09 – Lesson 08: Upgrading Packages with APT.**