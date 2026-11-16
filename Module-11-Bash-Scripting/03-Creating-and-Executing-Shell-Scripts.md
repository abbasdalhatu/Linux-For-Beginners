
# Module 11 – Lesson 03: Creating and Executing Shell Scripts

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Create Bash script files.
2. Understand Bash script structure.
3. Use different methods to execute scripts.
4. Understand Linux file permissions.
5. Debug basic script errors.
6. Use comments in Bash scripts.
7. Organize scripts professionally.

---

## 2. What Is a Shell Script?

A shell script is a text file containing commands that a shell interpreter executes.

Instead of entering commands individually, you can save them in a file and execute them together.

Example:

```bash
#!/bin/bash

echo "Starting system check..."
whoami
hostname
date
echo "System check completed."
```

The script executes each instruction in order.

---

## 3. Understanding Bash Script Structure

A typical Bash script contains several parts.

### A. Shebang

```bash
#!/bin/bash
```

Specifies Bash as the interpreter.

### B. Comments

```bash
# This is a comment
```

Comments explain what the script does. Bash ignores them during normal execution.

### C. Commands

```bash
echo "Hello Linux"
date
whoami
```

These are the instructions executed by Bash.

### D. Script Organization

A professional script should have:
- A shebang.
- A descriptive header.
- Clearly organized commands.
- Meaningful variable and function names.
- Useful output messages.
- Appropriate error handling.

Example:

```bash
#!/bin/bash

# Script: system-check.sh
# Purpose: Display basic system information

echo "System Check"
hostname
date
```

---

## 4. Creating Bash Scripts

There are several ways to create a script.

### Method 1: Using Nano

```bash
nano first-script.sh
```

Enter your code, then:

- `CTRL + O` to save.
- `ENTER` to confirm.
- `CTRL + X` to exit.

### Method 2: Using Touch

```bash
touch first-script.sh
```

This creates an empty file.

You can edit it later using Nano.

### Method 3: Using a Here-Document

A here-document allows you to write multiple lines directly into a file.

Example:

```bash
cat <<'EOF' > first-script.sh
#!/bin/bash

echo "My first script"
whoami
date
EOF
```

The quoted `EOF` prevents shell expansion while creating the file.

Inspect it:

```bash
cat first-script.sh
```

---

## 5. Understanding File Extensions

Bash scripts commonly use the `.sh` extension.

Examples:

- `backup.sh`
- `system-check.sh`
- `network-report.sh`
- `user-management.sh`

However, the `.sh` extension is not mandatory. The operating system identifies the script through its contents and execution method.

---

## 6. Making a Script Executable

Linux uses file permissions to control access.

To add execute permission:

```bash
chmod +x first-script.sh
```

Inspect permissions:

```bash
ls -l first-script.sh
```

Example:

```text
-rwxr-xr-x 1 kali kali 45 Nov 15 10:00 first-script.sh
```

The permission characters indicate:

| Permission | Meaning |
|---|---|
| `r` | Read |
| `w` | Write |
| `x` | Execute |
| `-` | Permission not granted |

The three permission groups represent:

1. Owner.
2. Group.
3. Others.

### Understanding chmod 755

You may also see:

```bash
chmod 755 first-script.sh
```

This means:

- Owner: read, write, execute.
- Group: read, execute.
- Others: read, execute.

For ordinary personal scripts, use the minimum permissions needed.

---

## 7. Different Ways to Execute Bash Scripts

There are three useful methods.

### Method 1: Execute Directly

```bash
./first-script.sh
```

Requirements:
- The script must have execute permission.
- The interpreter specified in the shebang must be available.

### Method 2: Execute Through Bash

```bash
bash first-script.sh
```

Bash reads and executes the file.

The execute permission is not required for this method, but the file must be readable.

### Method 3: Execute Using the Absolute Path

```bash
/bin/bash /home/kali/module11-lab/lesson03/first-script.sh
```

This specifies the interpreter and the complete file location.

---

## 8. Understanding Relative and Absolute Paths

### Absolute Path

An absolute path begins from the root directory `/`.

Example:

```bash
/home/kali/module11-lab/lesson03/first-script.sh
```

### Relative Path

A relative path is based on your current working directory.

Example:

```bash
./first-script.sh
```

The `./` means the current directory.

Check your current directory:

```bash
pwd
```

---

## 9. Using Comments in Bash Scripts

Comments help explain code.

A comment begins with `#`.

Example:

```bash
#!/bin/bash

# Display a welcome message
echo "Welcome to Bash scripting"

# Display the current user
whoami
```

Good comments should explain why something is being done when the purpose is not obvious.

Avoid unnecessary comments that simply repeat the command.

---

## 10. Handling Common Script Errors

Scripts can fail for different reasons.

### Error 1: Permission Denied

Example:

```text
Permission denied
```

Possible solution:

```bash
chmod +x first-script.sh
```

### Error 2: Command Not Found

Example:

```text
unknowncommand: command not found
```

Possible causes:
- Typographical errors.
- Missing software.
- Incorrect PATH configuration.

### Error 3: No Such File or Directory

Example:

```text
bash: first-script.sh: No such file or directory
```

Check:

```bash
pwd
ls -l
```

### Error 4: Incorrect Shebang

A script may fail if the interpreter path is incorrect.

Check:

```bash
which bash
```

The typical Kali location is `/usr/bin/bash` or `/bin/bash`, depending on the system's filesystem layout.

### Error 5: Windows Line Endings

Scripts edited in Windows may contain CRLF line endings, which can sometimes cause errors in Linux.

Inspect:

```bash
file first-script.sh
```

If necessary, convert with:

```bash
dos2unix first-script.sh
```

Install `dos2unix` only if it is not already available and you need it.

---

## 11. Debugging Bash Scripts

Debugging means finding and correcting errors in a program.

Bash provides useful debugging options.

### Method 1: Syntax Checking

```bash
bash -n first-script.sh
```

This checks the script's syntax without executing its commands.

### Method 2: Execution Tracing

```bash
bash -x first-script.sh
```

This displays commands as Bash executes them.

### Method 3: Verbose Execution

```bash
bash -v first-script.sh
```

This displays script lines as Bash reads them.

These options are useful when investigating unexpected behavior.

---

## 12. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson03
cd ~/module11-lab/lesson03
```

### Step 2: Create a Script

```bash
nano student-report.sh
```

Enter:

```bash
#!/bin/bash

# Student System Report

echo "================================"
echo "       STUDENT REPORT"
echo "================================"

echo "Username:"
whoami

echo "Hostname:"
hostname

echo "Date:"
date

echo "Home Directory:"
echo "$HOME"

echo "Current Directory:"
pwd

echo "================================"
echo "REPORT COMPLETED"
```

Save the file.

### Step 3: Inspect the Script

```bash
cat student-report.sh
```

### Step 4: Check Syntax

```bash
bash -n student-report.sh
```

If no output appears, the syntax check passed.

### Step 5: Add Execute Permission

```bash
chmod +x student-report.sh
```

### Step 6: Run the Script

```bash
./student-report.sh
```

### Step 7: Debug the Script

```bash
bash -x student-report.sh
```

Observe how Bash displays the commands it executes.

---

## 13. Practical Challenge: Create a Network Information Script

Create:

```bash
nano network-info.sh
```

Enter:

```bash
#!/bin/bash

# Script: network-info.sh
# Purpose: Display basic network information

echo "========== NETWORK REPORT =========="

echo "Hostname:"
hostname

echo "IP Address Information:"
ip -br addr

echo "Routing Information:"
ip route

echo "Listening Network Sockets:"
ss -tuln

echo "===================================="
echo "NETWORK REPORT COMPLETED"
```

Save the file.

Check syntax:

```bash
bash -n network-info.sh
```

Make it executable:

```bash
chmod +x network-info.sh
```

Execute:

```bash
./network-info.sh
```

**Note:** The script only displays information. It does not modify network settings or scan external systems.

---

## 14. Good Practices for Writing Bash Scripts

Follow these professional practices:

1. Always use a suitable shebang.
2. Give scripts meaningful filenames.
3. Use descriptive variable names.
4. Add comments where necessary.
5. Check syntax before execution.
6. Use appropriate file permissions.
7. Avoid unnecessary root privileges.
8. Keep scripts organized.
9. Test scripts in a safe environment.
10. Never execute unknown scripts without inspecting their contents.

---

## 15. Lesson Summary

In this lesson, we learned:

- How to create Bash scripts.
- How to structure scripts.
- How to use the shebang and comments.
- How to make scripts executable.
- Different ways of running scripts.
- Absolute and relative paths.
- Common script errors.
- Bash syntax checking and debugging.

### Important Commands

| Command | Purpose |
|---|---|
| `nano file.sh` | Create or edit a script |
| `touch file.sh` | Create an empty file |
| `cat file.sh` | Display file contents |
| `chmod +x file.sh` | Add execute permission |
| `./file.sh` | Execute a script directly |
| `bash file.sh` | Execute using Bash |
| `bash -n file.sh` | Check syntax |
| `bash -x file.sh` | Trace execution |
| `bash -v file.sh` | Display script lines while reading |
| `file file.sh` | Identify file type |

---

## 16. Knowledge Check

Answer in your notebook:

1. What is a Bash script?
2. What is the purpose of the shebang?
3. What does `chmod +x` do?
4. Explain the difference between `bash script.sh` and `./script.sh`.
5. What is the difference between an absolute and relative path?
6. Why are comments important?
7. What does `bash -n` do?
8. What is the purpose of `bash -x`?
9. Mention four common Bash script errors.
10. Why should unknown scripts be inspected before execution?

### Practical Assessment

Successfully create and execute:

- `student-report.sh`
- `network-info.sh`

Run a syntax check on both scripts and demonstrate execution tracing on at least one.

**Lesson 03 is complete when both scripts execute successfully.**
