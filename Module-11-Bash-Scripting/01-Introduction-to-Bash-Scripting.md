
# Module 11 – Lesson 01: Introduction to Bash Scripting

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define Bash scripting.
2. Explain what Bash is and why it is important.
3. Understand the difference between a command and a script.
4. Identify real-world uses of Bash scripting.
5. Create and execute your first Bash script.
6. Explain the importance of automation in Linux.

---

## 2. What Is Bash?

**Bash** stands for **Bourne Again Shell**.

It is a command-line interpreter used in Linux and other Unix-like operating systems.

Bash allows users to communicate with the operating system by entering commands.

For example:

```bash
pwd
```

This displays your current working directory.

Another example:

```bash
ls
```

This displays files and directories in your current location.

Bash receives commands, interprets them, and asks the operating system to execute them.

### Simple Explanation

Think of Bash as a translator between you and Linux.

You give Bash a command, Bash interprets it, and Linux performs the requested operation.

---

## 3. What Is Bash Scripting?

**Bash scripting** is the process of writing multiple Bash commands inside a text file so that they can be executed together as a program.

A Bash script is simply a file containing commands that Bash can execute.

For example, instead of manually running these commands one by one:

```bash
mkdir backup
cd backup
touch report.txt
echo "Backup completed"
```

You can place them inside one script and execute that script whenever needed.

This is called automation.

### Simple Definition

Bash scripting is the practice of combining Linux commands into a script to automate repetitive tasks.

---

## 4. Why Is Bash Scripting Important?

Bash scripting is important because it helps Linux users perform tasks faster and more efficiently.

Major benefits include:

### A. Automation

Bash can automatically perform repetitive tasks.

Example:
- Creating directories.
- Backing up files.
- Updating software.
- Checking system information.

### B. Time Saving

Instead of typing 20 commands individually, you can execute one script containing all 20 commands.

### C. System Administration

System administrators use Bash scripts to:
- Monitor system resources.
- Manage users.
- Check running processes.
- Organize files.
- Automate maintenance tasks.

### D. Cybersecurity

Cybersecurity professionals use Bash scripting to:
- Collect system information.
- Check open network ports.
- Monitor log files.
- Automate security checks.
- Analyze files.
- Perform authorized reconnaissance.

### E. Consistency

A script executes its instructions in the same order every time, reducing repetitive manual work.

---

## 5. Bash Commands vs Bash Scripts

There is an important difference between running a command and writing a script.

| Bash Command | Bash Script |
|---|---|
| Usually entered directly into the terminal | Saved inside a file |
| Often performs one immediate task | Can perform multiple tasks |
| Executes when entered | Executes when the script is launched |
| Example: `date` | Example: `system-info.sh` |

### Example of a Command

```bash
date
```

This displays the current date and time.

### Example of a Script

```bash
#!/bin/bash

echo "Welcome to Linux"
date
whoami
pwd
```

This script performs four operations.

---

## 6. Understanding the Shebang

Look at the first line of a Bash script:

```bash
#!/bin/bash
```

This is called the **shebang**.

It tells the operating system which interpreter should execute the script.

In this case:

- `#!` identifies the interpreter directive.
- `/bin/bash` identifies the Bash interpreter.

The shebang helps the system execute the script using Bash.

Another possible form is:

```bash
#!/usr/bin/env bash
```

This asks the environment to locate Bash using the system's PATH.

For this course, we will mainly use:

```bash
#!/bin/bash
```

---

## 7. Your First Bash Script

Let's create your first script inside Kali Linux.

### Step 1: Open the Terminal

Open your Kali Linux virtual machine in VMware.

### Step 2: Create a Practice Directory

```bash
mkdir -p ~/module11-lab
```

This creates a directory for your Bash scripting exercises.

### Step 3: Enter the Directory

```bash
cd ~/module11-lab
```

### Step 4: Create Your First Script

```bash
nano hello.sh
```

### Step 5: Write the Following Code

```bash
#!/bin/bash

echo "================================"
echo "WELCOME TO BASH SCRIPTING"
echo "================================"

echo "My first Bash script is working!"
echo "Current user:"
whoami

echo "Current directory:"
pwd

echo "Current date:"
date

echo "Lesson 01 completed successfully!"
```

### Step 6: Save the File

Inside Nano:

1. Press `CTRL + O` to save.
2. Press `ENTER` to confirm the filename.
3. Press `CTRL + X` to exit.

---

## 8. Making a Script Executable

Linux scripts need appropriate execution permissions to run directly.

Use:

```bash
chmod +x hello.sh
```

Explanation:

- `chmod` changes file permissions.
- `+x` adds execute permission.
- `hello.sh` is the script.

Verify the permission:

```bash
ls -l hello.sh
```

You should see an `x` in the permission information.

---

## 9. Running Your First Script

Execute:

```bash
./hello.sh
```

The `./` tells Linux to execute the file from the current directory.

You can also execute it directly through Bash:

```bash
bash hello.sh
```

Both methods should display the same output.

**Important:** Running `bash hello.sh` does not require the file to have execute permission.

---

## 10. Understanding the Script Line by Line

Consider:

```bash
#!/bin/bash
```

Specifies Bash as the interpreter.

```bash
echo "WELCOME TO BASH SCRIPTING"
```

Displays text in the terminal.

```bash
whoami
```

Displays the current username.

```bash
pwd
```

Displays the current working directory.

```bash
date
```

Displays the current date and time.

Bash executes these commands from top to bottom.

---

## 11. Important Bash Scripting Concepts

| Concept | Meaning |
|---|---|
| Bash | Command-line interpreter |
| Script | File containing executable commands |
| Shebang | Specifies the script interpreter |
| `echo` | Displays text |
| `chmod` | Changes file permissions |
| `+x` | Adds execute permission |
| `./script.sh` | Executes a script in the current directory |
| Automation | Performing tasks automatically |

---

## 12. Practical Exercise

Create another script named:

```bash
system-report.sh
```

Use Nano:

```bash
nano system-report.sh
```

Write the following:

```bash
#!/bin/bash

echo "========== SYSTEM REPORT =========="

echo "Username:"
whoami

echo "Hostname:"
hostname

echo "Current directory:"
pwd

echo "Date and time:"
date

echo "Disk usage:"
df -h

echo "Memory information:"
free -h

echo "==================================="
echo "SYSTEM REPORT COMPLETED"
```

Save the file and execute:

```bash
chmod +x system-report.sh
./system-report.sh
```

### Expected Result

Your terminal should display:
- Your Linux username.
- Your machine's hostname.
- Your current directory.
- The current date and time.
- Disk usage information.
- Memory information.

This is a basic example of a system information collection script.

---

## 13. Cybersecurity Application

Bash scripting is useful in defensive cybersecurity.

Imagine you are responsible for monitoring a Linux server.

Instead of manually checking system information every morning, you could create a script that collects:

1. Current username.
2. Hostname.
3. IP address.
4. Running processes.
5. Listening network ports.
6. Disk usage.
7. Memory usage.

This information can help administrators identify unusual system activity.

We will gradually build more advanced automation tools throughout this module.

Only collect information from systems you own or have permission to administer.

---

## 14. Lesson Summary

In this lesson, we learned:

- Bash means Bourne Again Shell.
- Bash is a command-line interpreter.
- Bash scripting combines commands inside a file.
- Scripts help automate repetitive tasks.
- The shebang identifies the interpreter.
- `chmod +x` makes a script executable.
- `./script.sh` runs an executable script.
- Bash is useful in system administration and cybersecurity.

---

## 15. Knowledge Check

Answer these questions in your notebook:

1. What does Bash stand for?
2. What is Bash scripting?
3. Explain three benefits of Bash scripting.
4. What is the purpose of `#!/bin/bash`?
5. What does the `echo` command do?
6. What is the difference between `bash script.sh` and `./script.sh`?
7. What does `chmod +x` do?
8. Give three examples of cybersecurity tasks that Bash can automate.

### Practical Challenge

Create a script named:

```bash
my-profile.sh
```

It should display:

- A welcome message.
- Your username.
- Your hostname.
- Your current directory.
- The current date.
- A completion message.

Make it executable and run it successfully.

**Lesson 01 Status: Complete when the practical exercises and challenge work successfully.**
        