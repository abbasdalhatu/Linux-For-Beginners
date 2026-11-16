
# PRACTICAL LABS
# MODULE 01 – LINUX COMMAND-LINE MASTERY

## LAB 05: USERS AND ENVIRONMENT VARIABLES

---

## 1. Lab Objectives

By the end of this lab, you should be able to:

1. Identify the currently logged-in Linux user.
2. Inspect user and group information.
3. Understand Linux user IDs and group IDs.
4. Examine environment variables.
5. Create and modify shell variables.
6. Understand the difference between local and exported variables.
7. Use the PATH environment variable.
8. Inspect user account information safely.
9. Build a simple user-information script.

## 2. Prepare Your Lab Environment

```bash
mkdir -p ~/practical-labs/module01/lab05
cd ~/practical-labs/module01/lab05
```

Verify:

```bash
pwd
```

All practical exercises will take place in this directory.

---

## 3. Exercise 1: Identify the Current User

Run:

```bash
whoami
```

This displays the username of the currently logged-in user.

Run:

```bash
id
```

This displays your user ID, primary group ID, and supplementary groups.

Example:

```text
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo)
```

Your actual output may differ.

### Understand the output

| Term | Meaning |
|---|---|
| `uid` | User ID |
| `gid` | Primary group ID |
| `groups` | Groups the user belongs to |
| `sudo` | A group commonly associated with administrative privileges on Kali and Debian systems |

Try:

```bash
id -u
id -g
id -Gn
```

These commands display:

- Your numerical user ID.
- Your numerical primary group ID.
- Your group names.

---

## 4. Exercise 2: Inspect Logged-in Users

Run:

```bash
who
```

Displays users currently logged in.

Run:

```bash
w
```

Displays logged-in users and information about their activity.

Run:

```bash
users
```

Displays usernames currently logged in.

Inspect your username:

```bash
logname
```

Note: `logname` may fail in some terminal, container, or privilege-escalation environments. `whoami` identifies the effective current user.

---

## 5. Exercise 3: Inspect User Account Information

Linux stores local user account information in:

```text
/etc/passwd
```

View the file:

```bash
cat /etc/passwd
```

For a shorter view:

```bash
head /etc/passwd
```

Find your current user:

```bash
grep "^$(whoami):" /etc/passwd
```

### Understand the structure

A typical entry looks like:

```text
kali:x:1000:1000:Kali User:/home/kali:/bin/bash
```

The fields are separated by colons.

| Field | Meaning |
|---|---|
| `kali` | Username |
| `x` | Password information is stored separately |
| `1000` | User ID |
| `1000` | Primary group ID |
| `Kali User` | User description |
| `/home/kali` | Home directory |
| `/bin/bash` | Login shell |

The `/etc/passwd` file is not where modern Linux systems normally store actual password hashes.

Password hashes are generally stored in `/etc/shadow`, which has restricted access.

Inspect your account's entry without attempting to read protected password information:

```bash
getent passwd "$(whoami)"
```

---

## 6. Exercise 4: Understand Linux Groups

Groups help Linux administrators organize permissions.

Inspect your groups:

```bash
groups
```

Inspect another account if it exists:

```bash
groups root
```

Display group information:

```bash
getent group
```

Display your primary group:

```bash
id -gn
```

### Understand group management

Common administrative commands include:

```bash
groupadd
groupmod
groupdel
usermod
```

These commands change system account configuration and generally require administrative privileges.

For this introductory lab, inspect the commands but do not modify your real Kali user or groups.

Read their documentation:

```bash
man groupadd
man usermod
```

---

## 7. Exercise 5: Understand Environment Variables

Environment variables are named values that provide information to programs and shells.

Examples include:

- `HOME`
- `USER`
- `SHELL`
- `PATH`
- `PWD`
- `LANG`
- `TERM`

Display a variable:

```bash
echo "$HOME"
```

Display your username:

```bash
echo "$USER"
```

Display your shell:

```bash
echo "$SHELL"
```

Display your current directory:

```bash
echo "$PWD"
```

Display your language setting:

```bash
echo "$LANG"
```

Display your terminal type:

```bash
echo "$TERM"
```

---

## 8. Exercise 6: Display All Environment Variables

Run:

```bash
printenv
```

Another option is:

```bash
env
```

Display one variable:

```bash
printenv HOME
```

Search for variables containing PATH:

```bash
printenv | grep PATH
```

Count environment variables:

```bash
printenv | wc -l
```

Save your environment information:

```bash
printenv > environment-report.txt
```

Inspect:

```bash
less environment-report.txt
```

Remember that environment variables can sometimes contain sensitive information, such as tokens or credentials. Do not share complete environment dumps publicly.

---

## 9. Exercise 7: Create Shell Variables

A shell variable stores a value for use in the current shell.

Create a variable:

```bash
STUDENT="Linux Student"
```

Display it:

```bash
echo "$STUDENT"
```

Create more variables:

```bash
COURSE="Cybersecurity"
LEVEL="Beginner"
LAB="Linux Command Line"
```

Display them:

```bash
echo "$COURSE"
echo "$LEVEL"
echo "$LAB"
```

### Important assignment rule

Correct:

```bash
NAME="Abbas"
```

Incorrect:

```bash
NAME = "Abbas"
```

Do not put spaces around the assignment operator.

---

## 10. Exercise 8: Understand Local and Exported Variables

A normal shell variable is available in the current shell.

Example:

```bash
MY_VARIABLE="Linux Practice"
```

Display it:

```bash
echo "$MY_VARIABLE"
```

Check whether it is exported:

```bash
printenv MY_VARIABLE
```

It normally will not appear because it has not been exported.

Now export it:

```bash
export MY_VARIABLE
```

Check again:

```bash
printenv MY_VARIABLE
```

It should now appear.

### Test variable inheritance

Run:

```bash
bash -c 'echo "$MY_VARIABLE"'
```

The child Bash process should receive the exported variable.

Now create another unexported variable:

```bash
LOCAL_ONLY="Private shell variable"
```

Run:

```bash
bash -c 'echo "$LOCAL_ONLY"'
```

The child process normally will not receive it.

### Key difference

| Local shell variable | Exported variable |
|---|---|
| Available in the current shell | Available in the current shell |
| Not automatically inherited by child processes | Inherited by child processes |
| Created through assignment | Made available to child processes using `export` |

---

## 11. Exercise 9: Modify and Remove Variables

Change a variable:

```bash
COURSE="Linux Security"
```

Display it:

```bash
echo "$COURSE"
```

Remove a variable:

```bash
unset COURSE
```

Try displaying it:

```bash
echo "$COURSE"
```

The output should be empty.

Check whether it exists in the environment:

```bash
printenv COURSE
```

It should produce no value.

Note that `unset` removes the variable from the current shell. It does not delete files or modify Linux user accounts.

---

## 12. Exercise 10: Understand the PATH Variable

The `PATH` variable tells the shell where to search for executable programs when you type a command.

Display it:

```bash
echo "$PATH"
```

The output may resemble:

```text
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Each directory is separated by a colon.

When you execute:

```bash
ls
```

the shell searches the directories listed in PATH to locate the executable.

Find the location of a command:

```bash
which ls
```

Another useful command:

```bash
command -v python3
```

Inspect a command's executable location:

```bash
type ls
```

### Important security concept

If an untrusted directory is placed early in PATH, a malicious executable with the same name as a legitimate command could be executed instead.

For this reason:

- Do not add untrusted directories to PATH.
- Avoid placing the current directory (`.`) at the beginning of PATH.
- Use trusted executable locations.
- Inspect unexpected PATH changes.

---

## 13. Exercise 11: Create Your Own Environment Variable

Create:

```bash
export LAB_NAME="Linux Practical Training"
export LAB_MODULE="Module 01"
export LAB_NUMBER="Lab 05"
```

Display:

```bash
echo "$LAB_NAME"
echo "$LAB_MODULE"
echo "$LAB_NUMBER"
```

Confirm that they are exported:

```bash
printenv LAB_NAME
printenv LAB_MODULE
printenv LAB_NUMBER
```

Check inheritance:

```bash
bash -c 'echo "$LAB_NAME"'
```

---

## 14. Exercise 12: Understand Shell Startup Files

Shell configuration files can define variables and customize your shell.

Common files include:

| File | Purpose |
|---|---|
| `~/.bashrc` | Bash interactive shell configuration |
| `~/.profile` | Login-session configuration |
| `~/.bash_profile` | Bash login configuration when present |
| `/etc/environment` | System-wide environment assignments on systems using it |
| `/etc/profile` | System-wide login shell configuration |

Inspect your Bash configuration:

```bash
ls -la ~ | grep bash
```

Inspect your `.bashrc` if it exists:

```bash
less ~/.bashrc
```

You can add personal environment variables to your own configuration file.

For this lab, do not change system-wide configuration files.

### Optional practice: Make a variable persistent

Open your own `.bashrc`:

```bash
nano ~/.bashrc
```

Add these lines at the bottom:

```bash
export MY_LINUX_COURSE="Practical Linux Training"
export MY_SECURITY_LEVEL="Beginner"
```

Save the file.

Load the changes:

```bash
source ~/.bashrc
```

Test:

```bash
echo "$MY_LINUX_COURSE"
echo "$MY_SECURITY_LEVEL"
```

These settings should become available in future interactive Bash sessions that load your `.bashrc`.

---

## 15. Exercise 13: Build a User Information Script

Create:

```bash
nano user-information.sh
```

Enter:

```bash
#!/bin/bash

echo "================================="
echo "       USER INFORMATION"
echo "================================="

echo
echo "CURRENT USER:"
whoami

echo
echo "USER ID:"
id -u

echo
echo "PRIMARY GROUP:"
id -gn

echo
echo "HOME DIRECTORY:"
echo "$HOME"

echo
echo "CURRENT SHELL:"
echo "$SHELL"

echo
echo "CURRENT DIRECTORY:"
echo "$PWD"

echo
echo "PATH:"
echo "$PATH"

echo
echo "HOSTNAME:"
hostname

echo
echo "================================="
echo "       REPORT COMPLETED"
echo "================================="
```

Make it executable:

```bash
chmod +x user-information.sh
```

Run:

```bash
./user-information.sh
```

Save the report:

```bash
./user-information.sh > user-report.txt
```

Inspect:

```bash
cat user-report.txt
```

---

## 16. Practical Challenge

Complete the following tasks without changing your actual Linux account configuration:

1. Identify your current username.
2. Find your numerical UID.
3. Display your primary group.
4. Display all supplementary groups.
5. Display your home directory.
6. Display your current shell.
7. Display your PATH.
8. Create three shell variables.
9. Export at least two of them.
10. Confirm that an exported variable is inherited by a child Bash process.
11. Create a user-information report.
12. Save the report to `challenge-report.txt`.

Useful commands:

```bash
whoami
id
id -u
id -gn
groups
printenv
echo "$HOME"
echo "$SHELL"
echo "$PATH"
export
```

---

## 17. Knowledge Check

Answer these questions in your notebook:

1. What is a Linux user account?
2. What does UID mean?
3. What does GID mean?
4. What is the purpose of Linux groups?
5. What information is stored in `/etc/passwd`?
6. What is the purpose of `/etc/shadow`?
7. What is an environment variable?
8. What does `printenv` do?
9. What is the difference between a shell variable and an exported variable?
10. What does `unset` do?
11. What is the purpose of PATH?
12. Why should untrusted directories not be placed early in PATH?
13. What is the purpose of `.bashrc`?
14. What does `source ~/.bashrc` do?

---

## 18. Lab Completion Checklist

- [ ] Identified the current Linux user.
- [ ] Inspected UID and GID.
- [ ] Inspected logged-in users.
- [ ] Examined `/etc/passwd`.
- [ ] Inspected groups.
- [ ] Displayed environment variables.
- [ ] Created shell variables.
- [ ] Exported variables.
- [ ] Tested child-process inheritance.
- [ ] Modified and removed variables.
- [ ] Examined PATH.
- [ ] Inspected Bash startup files.
- [ ] Built the user-information script.
- [ ] Completed the practical challenge.

**END OF LAB 05**
