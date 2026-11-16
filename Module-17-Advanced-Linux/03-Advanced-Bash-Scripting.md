
# MODULE 17 – ADVANCED LINUX
## LESSON 03: ADVANCED BASH SCRIPTING

---

## 1. Introduction to Advanced Bash Scripting

Bash scripting is the process of writing commands inside a file so that Linux can execute them automatically.

Instead of manually entering commands one by one, we can create a script that performs multiple operations.

For example, a system administrator might write a script to:

- Check available disk space.
- Monitor memory usage.
- Identify running processes.
- Back up important files.
- Check network connectivity.
- Generate system reports.
- Automate repetitive administrative tasks.

### Example of a simple Bash script

```bash
#!/bin/bash

echo "Welcome to Advanced Linux"
echo "Current user: $(whoami)"
echo "Current directory: $(pwd)"
```

Save this as:

`hello.sh`

Execute it using:

```bash
bash hello.sh
```

### Learning objectives

By the end of this lesson, you should be able to:

1. Create and execute Bash scripts.
2. Use variables and command substitution.
3. Apply conditional statements.
4. Use loops.
5. Create functions.
6. Accept command-line arguments.
7. Validate user input.
8. Handle errors.
9. Generate automated system reports.
10. Write a practical Linux administration script.

---

## 2. Understanding the Shebang

The first line of many Bash scripts is called the shebang.

```bash
#!/bin/bash
```

It tells the operating system which interpreter should execute the script when it is launched as a program.

Another common form is:

```bash
#!/usr/bin/env bash
```

This asks the environment to locate Bash using the current `PATH`.

### Example

```bash
#!/bin/bash

echo "My first advanced Bash script"
```

Save as:

`first-script.sh`

Make it executable:

```bash
chmod +x first-script.sh
```

Run it:

```bash
./first-script.sh
```

### Understanding chmod

The command:

```bash
chmod +x first-script.sh
```

adds executable permission to the file.

Check its permissions:

```bash
ls -l first-script.sh
```

---

## 3. Variables in Bash

Variables store information that scripts can reuse.

### Creating variables

```bash
#!/bin/bash

NAME="Abbas"
COURSE="Advanced Linux"

echo "Student: $NAME"
echo "Course: $COURSE"
```

Important rules:

- Do not put spaces around `=`.
- Variable names are case-sensitive.
- Use `$` to access a variable's value.
- Quote variables when appropriate.

Correct:

```bash
NAME="Linux"
```

Incorrect:

```bash
NAME = "Linux"
```

### Read-only variables

You can prevent a variable from being changed:

```bash
readonly SYSTEM_NAME="Kali Linux"
```

### Removing variables

```bash
unset NAME
```

---

## 4. User Input

Bash scripts can request information from users.

Use the `read` command.

### Example

```bash
#!/bin/bash

echo "Enter your name:"
read NAME

echo "Welcome, $NAME"
```

A shorter version:

```bash
#!/bin/bash

read -p "Enter your name: " NAME

echo "Welcome, $NAME"
```

### Silent input

The `-s` option hides typed input.

It is useful for password prompts.

```bash
#!/bin/bash

read -s -p "Enter a password: " PASSWORD
echo

echo "Input received."
unset PASSWORD
```

This demonstration does not authenticate or securely store a password. Avoid placing real passwords in ordinary script variables or logs.

---

## 5. Conditional Statements

Conditional statements allow a script to make decisions.

The main Bash conditional structures include:

- `if`
- `elif`
- `else`
- `case`

### A. The if statement

```bash
#!/bin/bash

NUMBER=10

if [ "$NUMBER" -gt 5 ]; then
    echo "Number is greater than 5"
fi
```

### B. if and else

```bash
#!/bin/bash

NUMBER=3

if [ "$NUMBER" -gt 5 ]; then
    echo "Number is greater than 5"
else
    echo "Number is 5 or less"
fi
```

### C. if, elif and else

```bash
#!/bin/bash

SCORE=75

if [ "$SCORE" -ge 80 ]; then
    echo "Grade A"
elif [ "$SCORE" -ge 70 ]; then
    echo "Grade B"
elif [ "$SCORE" -ge 60 ]; then
    echo "Grade C"
else
    echo "Below grade C"
fi
```

### Common numerical comparison operators

| Operator | Meaning |
|---|---|
| `-eq` | Equal to |
| `-ne` | Not equal to |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater than or equal to |
| `-le` | Less than or equal to |

### String comparisons

```bash
if [ "$NAME" = "Abbas" ]; then
    echo "Name matched"
fi
```

Useful operators:

| Operator | Meaning |
|---|---|
| `=` | Strings are equal |
| `!=` | Strings are different |
| `-z` | String is empty |
| `-n` | String is not empty |

### File tests

Bash can check whether files and directories exist.

| Test | Purpose |
|---|---|
| `-e` | Path exists |
| `-f` | Regular file exists |
| `-d` | Directory exists |
| `-r` | File is readable |
| `-w` | File is writable |
| `-x` | File is executable |

Example:

```bash
#!/bin/bash

FILE="report.txt"

if [ -f "$FILE" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

---

## 6. Loops in Bash

Loops repeat commands.

Bash supports several loop structures.

### A. for loop

A `for` loop repeats an operation over a list of values.

```bash
#!/bin/bash

for NAME in Abbas Yusuf Ibrahim
do
    echo "Hello, $NAME"
done
```

### B. while loop

A `while` loop continues while its condition remains true.

```bash
#!/bin/bash

COUNT=1

while [ "$COUNT" -le 5 ]
do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done
```

### C. until loop

An `until` loop continues until its condition becomes true.

```bash
#!/bin/bash

COUNT=1

until [ "$COUNT" -gt 5 ]
do
    echo "Count: $COUNT"
    COUNT=$((COUNT + 1))
done
```

### D. break statement

Stops a loop immediately.

```bash
#!/bin/bash

for NUMBER in 1 2 3 4 5
do
    if [ "$NUMBER" -eq 3 ]; then
        break
    fi

    echo "$NUMBER"
done
```

Output:

```text
1
2
```

### E. continue statement

Skips the current iteration and moves to the next one.

```bash
#!/bin/bash

for NUMBER in 1 2 3 4 5
do
    if [ "$NUMBER" -eq 3 ]; then
        continue
    fi

    echo "$NUMBER"
done
```

Output:

```text
1
2
4
5
```

---

## 7. Bash Functions

A function is a reusable block of commands.

Functions help organize scripts and reduce repeated code.

### Creating a function

```bash
#!/bin/bash

greet() {
    echo "Welcome to Linux"
}

greet
```

### Function with parameters

```bash
#!/bin/bash

greet() {
    echo "Hello, $1"
}

greet "Abbas"
```

Here:

- `$1` represents the first argument passed to the function.
- `$2` represents the second argument.
- `$3` represents the third argument.

### Function with multiple parameters

```bash
#!/bin/bash

student_info() {
    echo "Name: $1"
    echo "Course: $2"
}

student_info "Abbas" "Computer Science"
```

### Return status

A Bash function can return an exit status.

```bash
#!/bin/bash

check_directory() {
    if [ -d "$1" ]; then
        return 0
    else
        return 1
    fi
}

if check_directory "$HOME"; then
    echo "Home directory exists"
else
    echo "Home directory not found"
fi
```

A function's `return` statement is primarily for status codes, not returning arbitrary text. Use command substitution when you need to capture text output.

---

## 8. Command-Line Arguments

Scripts can accept information when they are executed.

Example:

```bash
#!/bin/bash

echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"
```

Save as:

`arguments.sh`

Execute:

```bash
bash arguments.sh Linux Security
```

Output:

```text
First argument: Linux
Second argument: Security
All arguments: Linux Security
Number of arguments: 2
```

### Important argument variables

| Variable | Meaning |
|---|---|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `$@` | All arguments as separate words |
| `$#` | Number of arguments |
| `$?` | Previous command's exit status |
| `$$` | Current shell process ID |

### Handling missing arguments

```bash
#!/bin/bash

if [ "$#" -lt 1 ]; then
    echo "Usage: $0 <name>"
    exit 1
fi

echo "Welcome, $1"
```

This checks that the user supplied at least one argument.

---

## 9. Arithmetic Operations

Bash supports basic arithmetic.

### Addition

```bash
A=10
B=5

RESULT=$((A + B))

echo "$RESULT"
```

### Other operations

| Operator | Operation |
|---|---|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Integer division |
| `%` | Remainder |

Example:

```bash
A=20
B=4

echo "Addition: $((A + B))"
echo "Subtraction: $((A - B))"
echo "Multiplication: $((A * B))"
echo "Division: $((A / B))"
echo "Remainder: $((A % B))"
```

Remember that normal Bash arithmetic uses integer arithmetic unless you use another tool for decimal calculations.

---

## 10. Error Handling

Reliable scripts should handle errors instead of silently continuing.

### A. Check command success

```bash
#!/bin/bash

mkdir -p test-folder

if [ "$?" -eq 0 ]; then
    echo "Directory operation completed"
else
    echo "Directory operation failed"
fi
```

A more direct approach is:

```bash
if mkdir -p test-folder; then
    echo "Directory operation completed"
else
    echo "Directory operation failed"
fi
```

### B. Exit codes

```bash
#!/bin/bash

if [ ! -d "$HOME" ]; then
    echo "Home directory not found"
    exit 1
fi

echo "Home directory exists"
exit 0
```

### C. Strict-mode options

Many Bash scripts use:

```bash
set -Eeuo pipefail
```

These options provide stricter error handling:

- `-E`: Allows ERR traps to be inherited in more contexts.
- `-e`: Exits on many unhandled command failures.
- `-u`: Treats unset variables as errors.
- `-o pipefail`: Makes a pipeline fail if a command within it fails.

Important: `set -e` has exceptions and context-dependent behaviour. It is not a replacement for deliberate error checking.

### D. Debugging

Run a script with tracing:

```bash
bash -x script.sh
```

This displays commands as Bash executes them.

To check syntax without executing the script:

```bash
bash -n script.sh
```

---

## 11. Practical Project: Linux System Information Script

Now we will build a useful administration script.

### Project objective

Create a script that automatically collects basic information about your Kali Linux virtual machine.

The script will display:

- Current username.
- Hostname.
- Operating-system information.
- Kernel version.
- CPU information.
- Memory usage.
- Disk usage.
- System uptime.

### Step 1: Create the project directory

```bash
mkdir -p ~/module17-lab/lesson03
cd ~/module17-lab/lesson03
```

### Step 2: Create the script

```bash
nano system-info.sh
```

### Step 3: Enter the following code

```bash
#!/bin/bash

set -u

echo "================================="
echo "       LINUX SYSTEM REPORT"
echo "================================="

echo

echo "USERNAME:"
whoami

echo
echo "HOSTNAME:"
hostname

echo
echo "OPERATING SYSTEM:"
grep '^PRETTY_NAME=' /etc/os-release | cut -d= -f2- | tr -d '"'

echo
echo "KERNEL VERSION:"
uname -r

echo
echo "CPU INFORMATION:"
lscpu | grep -E 'Architecture|Model name|CPU\(s\):' | head -n 5

echo
echo "MEMORY INFORMATION:"
free -h

echo
echo "DISK USAGE:"
df -h /

echo
echo "SYSTEM UPTIME:"
uptime -p

echo
echo "================================="
echo "       REPORT COMPLETED"
echo "================================="
```

### Step 4: Save the script

In Nano:

- Press `CTRL + O`.
- Press `ENTER`.
- Press `CTRL + X`.

### Step 5: Make it executable

```bash
chmod +x system-info.sh
```

### Step 6: Execute it

```bash
./system-info.sh
```

### Step 7: Save the output

```bash
./system-info.sh > system-report.txt
```

View the saved report:

```bash
cat system-report.txt
```

### Step 8: Validate the script

Check its syntax:

```bash
bash -n system-info.sh
```

If no syntax errors are reported, Bash has successfully parsed the script.

### Expected result

You should have two files:

```text
system-info.sh
system-report.txt
```

The first file contains your Bash program. The second contains the generated system report.

---

## 12. Practical Exercise: Create a Disk Space Checker

Create another script:

```bash
nano disk-check.sh
```

Enter:

```bash
#!/bin/bash

set -u

echo "DISK SPACE CHECKER"
echo "=================="

DISK_USAGE=$(df -P / | awk 'NR==2 {gsub(/%/, "", $5); print $5}')

if [ -z "$DISK_USAGE" ]; then
    echo "Unable to determine disk usage."
    exit 1
fi

echo "Root filesystem usage: ${DISK_USAGE}%"

if [ "$DISK_USAGE" -ge 90 ]; then
    echo "WARNING: Disk usage is critically high."
elif [ "$DISK_USAGE" -ge 75 ]; then
    echo "NOTICE: Disk usage is getting high."
else
    echo "Disk usage is within the configured limits."
fi
```

Save it, then execute:

```bash
chmod +x disk-check.sh
./disk-check.sh
```

The script reads disk usage and displays a message based on the percentage.

The thresholds are examples, not universal operating-system limits.

---

## 13. Bash Scripting Security Principles

Bash scripts can perform powerful administrative operations. Poorly written scripts can damage files or expose sensitive information.

Follow these principles:

1. Validate user input.
2. Quote variables when appropriate.
3. Avoid executing untrusted input as commands.
4. Avoid unnecessary root privileges.
5. Use absolute paths when reliability matters.
6. Handle command failures.
7. Protect sensitive files.
8. Review scripts before execution.
9. Never store passwords in plaintext scripts.
10. Test scripts inside your laboratory environment.

### Example: Safer variable quoting

Prefer:

```bash
mkdir -p "$HOME/my-folder"
```

Avoid:

```bash
mkdir -p $HOME/my-folder
```

Quoting protects paths containing spaces and prevents unintended word splitting and wildcard expansion.

### Important warning

Never execute downloaded scripts with root privileges without inspecting and understanding their contents.

---

## 14. Knowledge Check

Answer these questions in your notebook.

1. What is Bash scripting?
2. What is the purpose of a shebang?
3. Explain how to make a Bash script executable.
4. What is a variable?
5. What does the `read` command do?
6. Explain the difference between `if`, `elif`, and `else`.
7. What is the difference between a `for` loop and a `while` loop?
8. What is a Bash function?
9. What do `$1`, `$2`, `$@`, and `$#` represent?
10. What does `set -Eeuo pipefail` do?
11. What is the purpose of `bash -x`?
12. Why should scripts validate user input?

---

## 15. Important Bash Commands Summary

| Command or Syntax | Purpose |
|---|---|
| `#!/bin/bash` | Select Bash interpreter |
| `chmod +x script.sh` | Add executable permission |
| `read NAME` | Accept user input |
| `if` | Make decisions |
| `for` | Iterate over a list |
| `while` | Repeat while a condition is true |
| `until` | Repeat until a condition becomes true |
| `break` | Exit a loop |
| `continue` | Skip to next loop iteration |
| `function_name()` | Define a function |
| `$1` | First script argument |
| `$@` | All script arguments |
| `$#` | Number of arguments |
| `$(( ))` | Perform arithmetic |
| `exit 1` | Exit with a failure status |
| `bash -n script.sh` | Check script syntax |
| `bash -x script.sh` | Trace script execution |
| `set -Eeuo pipefail` | Enable stricter error handling |

---

## 16. Lesson Summary

In this lesson, you learned how to write advanced Bash scripts.

You covered:

- Script creation and execution.
- Shebangs.
- Variables.
- User input.
- Conditional statements.
- Loops.
- Functions.
- Command-line arguments.
- Arithmetic operations.
- Error handling.
- Script debugging.
- Linux system information automation.
- Disk-space monitoring.

You also created two practical scripts:

1. `system-info.sh`
2. `disk-check.sh`

These are useful foundations for Linux system administration, cybersecurity automation, and software engineering.

### Completion checklist

- [ ] Understand the Bash shebang.
- [ ] Create and execute a script.
- [ ] Use variables and user input.
- [ ] Practise conditional statements.
- [ ] Practise loops.
- [ ] Create functions.
- [ ] Understand command-line arguments.
- [ ] Practise arithmetic.
- [ ] Understand error handling.
- [ ] Build `system-info.sh`.
- [ ] Build `disk-check.sh`.
- [ ] Validate both scripts.
- [ ] Answer the knowledge-check questions.

**END OF LESSON 03**
