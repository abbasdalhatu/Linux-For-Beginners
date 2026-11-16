# PRACTICAL LABS
# MODULE 03 – BASH SCRIPTING CHALLENGES

## LAB 01: BASH SCRIPTING FUNDAMENTALS

### 1. Lab Overview

**Objective:** Understand Bash scripting and create your first working Bash script.

In previous modules, you learned how to execute Linux commands manually. Bash scripting allows you to combine multiple commands into a single file and execute them automatically.

For example, instead of manually running five commands to inspect your Linux system, you can write one script that performs all five tasks.

### 2. What Is Bash?

Bash stands for **Bourne Again Shell**.

It is a command-line interpreter and scripting language commonly used on Linux systems.

Bash allows administrators and cybersecurity professionals to:

- Automate repetitive tasks.
- Manage files and directories.
- Monitor system resources.
- Process log files.
- Perform security checks.
- Create system administration tools.

### 3. What Is a Shell Script?

A shell script is a text file containing commands that the shell executes in sequence.

Example:

```bash
#!/bin/bash

echo "Welcome to Linux!"
echo "This is my first Bash script."
pwd
whoami
date
```

This script prints messages and displays the current directory, username, and date.

### 4. Understanding the Shebang

The first line of many Bash scripts is:

```bash
#!/bin/bash
```

This is called the **shebang**.

It tells the operating system which interpreter should execute the script.

Common examples:

| Shebang | Purpose |
|---|---|
| `#!/bin/bash` | Uses Bash |
| `#!/bin/sh` | Uses the system's POSIX-compatible shell |
| `#!/usr/bin/env bash` | Locates Bash through the environment PATH |

For this module, we will primarily use:

```bash
#!/bin/bash
```

### 5. Preparing Your Practical Lab

Open your Kali Linux virtual machine.

Create a dedicated working directory:

```bash
mkdir -p ~/practical-labs/module03/lab01
```

Enter the directory:

```bash
cd ~/practical-labs/module03/lab01
```

Confirm your location:

```bash
pwd
```

All exercises in this lab will remain inside this directory.

### 6. Creating Your First Bash Script

Create a file:

```bash
nano hello.sh
```

Enter:

```bash
#!/bin/bash

echo "================================"
echo "       MY FIRST BASH SCRIPT"
echo "================================"

echo "Welcome to Bash scripting!"
echo "I am learning Linux automation."

echo ""
echo "Current user:"
whoami

echo ""
echo "Current directory:"
pwd

echo ""
echo "Current date:"
date

echo ""
echo "Bash scripting is working!"
```

Save the file:

1. Press `CTRL + O`.
2. Press `ENTER`.
3. Press `CTRL + X`.

### 7. Understanding the Script

Let's examine the commands.

**`echo`**

Displays text on the terminal.

```bash
echo "Hello World"
```

**`whoami`**

Displays the username of the current user.

```bash
whoami
```

**`pwd`**

Displays the current working directory.

```bash
pwd
```

**`date`**

Displays the current system date and time.

```bash
date
```

Bash executes these commands from top to bottom.

### 8. Executing a Bash Script

There are two common ways to execute your script.

**Method A: Run using Bash**

```bash
bash hello.sh
```

This executes the script using Bash without requiring executable permission.

**Method B: Make the script executable**

First:

```bash
chmod +x hello.sh
```

Then:

```bash
./hello.sh
```

The `chmod +x` command adds executable permission.

The `./` tells Linux to execute the file from the current directory.

### 9. Inspecting Script Permissions

Run:

```bash
ls -l hello.sh
```

Example output:

```text
-rwxr-xr-x 1 kali kali 350 Nov 15 12:00 hello.sh
```

The `x` characters indicate executable permission.

Remember:

- `r` = Read
- `w` = Write
- `x` = Execute

### 10. Creating a System Information Script

Now create another script:

```bash
nano system-info.sh
```

Enter:

```bash
#!/bin/bash

echo "========== SYSTEM INFORMATION =========="

echo ""
echo "Hostname:"
hostname

echo ""
echo "Current User:"
whoami

echo ""
echo "Current Directory:"
pwd

echo ""
echo "System Uptime:"
uptime

echo ""
echo "Kernel Information:"
uname -r

echo ""
echo "Memory Information:"
free -h

echo ""
echo "Disk Usage:"
df -h

echo ""
echo "========== REPORT COMPLETE =========="
```

Save the file.

Execute it:

```bash
bash system-info.sh
```

This script collects basic information about your Kali Linux virtual machine.

**Cybersecurity application:** System administrators and security analysts use scripts like this to collect basic system information during routine inspections.

### 11. Understanding Comments

Comments are notes written inside scripts. Bash does not execute them.

A comment begins with `#`.

Example:

```bash
#!/bin/bash

# Display a welcome message
echo "Welcome to Linux"

# Display current username
whoami
```

Comments help make scripts easier to understand and maintain.

### 12. Checking Script Syntax

Bash provides a syntax-checking option:

```bash
bash -n hello.sh
```

If there are no syntax errors, Bash normally produces no output.

This checks syntax without executing the script.

Try:

```bash
bash -n system-info.sh
```

### 13. Viewing Script Contents

Use:

```bash
cat hello.sh
```

Or:

```bash
cat system-info.sh
```

You can also inspect both files:

```bash
ls -l
```

### 14. Practical Exercises

Complete these exercises inside:

```text
~/practical-labs/module03/lab01/
```

**Exercise 1 – Welcome Script**

Create:

```text
welcome.sh
```

The script must:

- Display your welcome message.
- Display your current username.
- Display your current Linux directory.
- Display the current date.

**Exercise 2 – Linux Information Script**

Create:

```text
linux-report.sh
```

The script must display:

- Hostname.
- Kernel version.
- System uptime.
- Memory usage.
- Disk usage.

**Exercise 3 – Script Permissions**

For both scripts:

1. Inspect their permissions.
2. Add executable permission.
3. Execute them using `./filename.sh`.
4. Confirm that they work.

**Exercise 4 – Syntax Validation**

Run:

```bash
bash -n welcome.sh
bash -n linux-report.sh
```

Confirm that there are no syntax errors.

### 15. Troubleshooting

**Problem: Permission denied**

Possible solution:

```bash
chmod +x hello.sh
```

**Problem: No such file or directory**

Check your current directory:

```bash
pwd
```

List available files:

```bash
ls -l
```

**Problem: Bad interpreter**

Inspect the first line:

```bash
head -n 1 hello.sh
```

It should contain:

```bash
#!/bin/bash
```

**Problem: Script does not execute**

Try:

```bash
bash hello.sh
```

This helps determine whether the issue is related to executable permissions or script execution.

### 16. Knowledge Check

1. What does Bash stand for?
2. What is a shell script?
3. What is the purpose of `#!/bin/bash`?
4. What does `echo` do?
5. What is the difference between `bash script.sh` and `./script.sh`?
6. What does `chmod +x` do?
7. What command checks Bash syntax without executing a script?
8. Why are comments useful in Bash scripts?

### 17. Lab Completion Checklist

- [ ] Created the lab working directory.
- [ ] Created `hello.sh`.
- [ ] Successfully executed the first script.
- [ ] Created `system-info.sh`.
- [ ] Successfully collected system information.
- [ ] Practised executable permissions.
- [ ] Checked script syntax.
- [ ] Completed the practical exercises.

### 18. Expected Outcome

By completing this lab, you should understand:

- What Bash scripting is.
- How to create a Bash script.
- How to use the shebang.
- How to execute scripts.
- How executable permissions work.
- How to validate basic Bash syntax.
- How scripts can automate Linux administration tasks.

**Next Lab:** Variables and User Input.