---
---


# Module 11 – Lesson 07: Conditional Statements

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain conditional statements.
2. Understand how `if`, `elif`, and `else` work.
3. Use comparison operators.
4. Compare strings and numbers.
5. Understand file and directory tests.
6. Use the `case` statement.
7. Combine conditions using logical operators.
8. Build practical decision-making Bash scripts.

---

## 2. What Are Conditional Statements?

A conditional statement allows a program to make decisions based on whether a condition is true or false.

In programming, conditions determine which instructions should execute.

### Real-World Example

Imagine a university grading system.

- If a student's score is 70 or above, the grade is A.
- If the score is 60 or above, the grade is B.
- If the score is 50 or above, the grade is C.
- Otherwise, the student fails.

A Bash script can perform these decisions automatically.

### Simple Definition

A conditional statement is a programming structure that executes different instructions depending on whether a condition is satisfied.

---

## 3. Understanding the if Statement

The `if` statement executes commands when a condition is true.

### Basic Syntax

```bash
if [[ condition ]]; then
    commands
fi
```

Explanation:

- `if` begins the condition.
- `[[ condition ]]` evaluates the condition.
- `then` begins the commands to execute if the condition is true.
- `fi` ends the conditional statement.

### Example

```bash
#!/bin/bash

age=21

if [[ $age -ge 18 ]]; then
    echo "You are an adult."
fi
```

Output:

```text
You are an adult.
```

The condition is true because 21 is greater than or equal to 18.

---

## 4. Understanding else

The `else` statement provides an alternative when the `if` condition is false.

### Syntax

```bash
if [[ condition ]]; then
    commands
else
    alternative_commands
fi
```

### Example

```bash
#!/bin/bash

age=16

if [[ $age -ge 18 ]]; then
    echo "You are an adult."
else
    echo "You are below 18."
fi
```

Output:

```text
You are below 18.
```

Only one branch executes.

---

## 5. Understanding elif

The `elif` statement allows a program to check multiple conditions.

### Syntax

```bash
if [[ condition1 ]]; then
    commands
elif [[ condition2 ]]; then
    commands
elif [[ condition3 ]]; then
    commands
else
    alternative_commands
fi
```

### Example: Student Grading System

```bash
#!/bin/bash

score=75

if [[ $score -ge 70 ]]; then
    echo "Grade: A"
elif [[ $score -ge 60 ]]; then
    echo "Grade: B"
elif [[ $score -ge 50 ]]; then
    echo "Grade: C"
else
    echo "Grade: F"
fi
```

Output:

```text
Grade: A
```

Bash checks the conditions from top to bottom. Once a condition is true, its branch executes and the remaining branches are skipped.

---

## 6. Numeric Comparison Operators

Bash provides special operators for comparing integers.

| Operator | Meaning |
|---|---|
| `-eq` | Equal to |
| `-ne` | Not equal to |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater than or equal to |
| `-le` | Less than or equal to |

### Example 1: Equal to

```bash
a=10
b=10

if [[ $a -eq $b ]]; then
    echo "Both numbers are equal."
fi
```

### Example 2: Greater than

```bash
a=20
b=10

if [[ $a -gt $b ]]; then
    echo "A is greater than B."
fi
```

### Example 3: Less than

```bash
a=5
b=10

if [[ $a -lt $b ]]; then
    echo "A is less than B."
fi
```

**Important:** These numeric comparison operators are intended for integer values.

---

## 7. String Comparison Operators

Strings are sequences of characters.

Bash supports string comparisons.

| Operator | Meaning |
|---|---|
| `=` | Strings are equal |
| `==` | Strings are equal |
| `!=` | Strings are not equal |
| `-z` | String is empty |
| `-n` | String is not empty |

### Example 1: Comparing Strings

```bash
#!/bin/bash

password="linux123"

if [[ "$password" == "linux123" ]]; then
    echo "Values match."
else
    echo "Values do not match."
fi
```

### Example 2: Checking Empty Input

```bash
#!/bin/bash

name=""

if [[ -z "$name" ]]; then
    echo "Name is empty."
else
    echo "Name has been provided."
fi
```

### Example 3: Checking Non-Empty Input

```bash
#!/bin/bash

name="Abbas"

if [[ -n "$name" ]]; then
    echo "Name is available."
fi
```

**Security note:** Comparing a string with a password inside a script is only a demonstration. Do not store real passwords in plaintext scripts.

---

## 8. Logical Operators

Logical operators allow you to combine multiple conditions.

### A. AND Operator

The AND operator requires all conditions to be true.

Inside `[[ ... ]]`, use `&&`.

Example:

```bash
#!/bin/bash

age=21
citizen="yes"

if [[ $age -ge 18 && $citizen == "yes" ]]; then
    echo "Both conditions are true."
else
    echo "At least one condition is false."
fi
```

### B. OR Operator

The OR operator requires at least one condition to be true.

Inside `[[ ... ]]`, use `||`.

Example:

```bash
#!/bin/bash

day="Saturday"

if [[ $day == "Saturday" || $day == "Sunday" ]]; then
    echo "It is the weekend."
else
    echo "It is a weekday."
fi
```

### C. NOT Operator

The NOT operator reverses a condition.

Use `!`.

Example:

```bash
#!/bin/bash

name=""

if [[ ! -n "$name" ]]; then
    echo "Name is empty."
fi
```

---

## 9. File and Directory Conditional Tests

Bash can check whether files and directories exist and inspect their properties.

These tests are especially useful in automation.

| Test | Meaning |
|---|---|
| `-e` | Path exists |
| `-f` | Path is a regular file |
| `-d` | Path is a directory |
| `-r` | Path is readable |
| `-w` | Path is writable |
| `-x` | Path is executable |
| `-s` | File exists and has a size greater than zero |

### Example 1: Check Whether a File Exists

```bash
#!/bin/bash

if [[ -e "report.txt" ]]; then
    echo "The path exists."
else
    echo "The path does not exist."
fi
```

### Example 2: Check Whether a File Is Regular

```bash
if [[ -f "report.txt" ]]; then
    echo "This is a regular file."
fi
```

### Example 3: Check Whether a Directory Exists

```bash
if [[ -d "backup" ]]; then
    echo "Backup directory exists."
else
    echo "Backup directory does not exist."
fi
```

### Example 4: Check File Permissions

```bash
if [[ -r "report.txt" ]]; then
    echo "File is readable."
fi
```

---

## 10. Understanding the case Statement

The `case` statement is useful when comparing one value against several possible options.

It can make scripts cleaner than using many `elif` statements.

### Syntax

```bash
case "$variable" in
    pattern1)
        commands
        ;;
    pattern2)
        commands
        ;;
    *)
        default_commands
        ;;
esac
```

Explanation:

- `case` starts the selection.
- Each pattern represents a possible value.
- `)` separates the pattern from its commands.
- `;;` ends that option.
- `*` matches anything not previously matched.
- `esac` ends the statement.

### Example: Menu Selection

```bash
#!/bin/bash

read -r -p "Choose an option (1-3): " choice

case "$choice" in
    1)
        echo "You selected Information."
        ;;
    2)
        echo "You selected Security."
        ;;
    3)
        echo "You selected Administration."
        ;;
    *)
        echo "Invalid selection."
        ;;
esac
```

---

## 11. Using Conditions with Command Exit Status

Linux commands return an exit status after execution.

- `0` generally means success.
- A non-zero value generally indicates failure or another status.

You can use commands directly in conditions.

Example:

```bash
#!/bin/bash

if grep -q "ERROR" application.log; then
    echo "An error was found."
else
    echo "No matching error was found."
fi
```

Here:
- `grep` searches for the text `ERROR`.
- `-q` suppresses normal output.
- The `if` statement checks whether the command succeeded.

This technique is important in system administration and cybersecurity automation.

---

## 12. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson07
cd ~/module11-lab/lesson07
```

### Step 2: Create a Number Comparison Script

```bash
nano number-check.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter a number: " number

if [[ ! "$number" =~ ^-?[0-9]+$ ]]; then
    echo "Error: Enter a valid integer."
elif [[ $number -gt 0 ]]; then
    echo "The number is positive."
elif [[ $number -lt 0 ]]; then
    echo "The number is negative."
else
    echo "The number is zero."
fi
```

Save and execute:

```bash
chmod +x number-check.sh
./number-check.sh
```

Test:
- `10`
- `-5`
- `0`
- `hello`

Observe the different results.

### Step 3: Create a File Checker

```bash
nano file-check.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter a file or directory path: " target

if [[ -f "$target" ]]; then
    echo "A regular file exists."
elif [[ -d "$target" ]]; then
    echo "A directory exists."
elif [[ -e "$target" ]]; then
    echo "The path exists but is not a regular file or directory."
else
    echo "The path does not exist."
fi
```

Execute:

```bash
chmod +x file-check.sh
./file-check.sh
```

Test it with:
- An existing file.
- An existing directory.
- A nonexistent path.

### Step 4: Create a Menu Script

```bash
nano menu.sh
```

Enter:

```bash
#!/bin/bash

echo "========== LINUX MENU =========="
echo "1. Display username"
echo "2. Display hostname"
echo "3. Display current directory"
echo "4. Display date"
echo "================================"

read -r -p "Select an option: " choice

case "$choice" in
    1)
        whoami
        ;;
    2)
        hostname
        ;;
    3)
        pwd
        ;;
    4)
        date
        ;;
    *)
        echo "Invalid option."
        ;;
esac
```

Execute:

```bash
chmod +x menu.sh
./menu.sh
```

Try all four options and an invalid option.

---

## 13. Practical Challenge: Build a Security Status Checker

Create:

```bash
nano security-check.sh
```

Enter:

```bash
#!/bin/bash

echo "========== SECURITY STATUS CHECK =========="

read -r -p "Enter a file path to inspect: " target

if [[ ! -e "$target" ]]; then
    echo "WARNING: Path does not exist."

elif [[ -f "$target" ]]; then
    echo "Regular file detected."

    if [[ -r "$target" ]]; then
        echo "Read permission: Available"
    else
        echo "Read permission: Not available"
    fi

    if [[ -w "$target" ]]; then
        echo "Write permission: Available"
    else
        echo "Write permission: Not available"
    fi

    if [[ -x "$target" ]]; then
        echo "Execute permission: Available"
    else
        echo "Execute permission: Not available"
    fi

elif [[ -d "$target" ]]; then
    echo "Directory detected."

    if [[ -r "$target" && -x "$target" ]]; then
        echo "Directory can be listed and traversed."
    else
        echo "Directory access is restricted."
    fi
fi

echo "==========================================="
echo "CHECK COMPLETED"
```

Make it executable:

```bash
chmod +x security-check.sh
```

Run:

```bash
./security-check.sh
```

Test it against files and directories inside your lab directory.

**Important:** This is a basic permission inspection tool, not a complete security audit.

---

## 14. Cybersecurity Applications

Conditional statements are essential in defensive security tools.

Examples include:

### A. File Integrity Checking

A script can check whether an expected file exists before calculating its hash.

### B. Log Monitoring

A script can check whether suspicious keywords appear in a log file.

### C. Permission Auditing

A script can identify files with unexpected permissions.

### D. Service Monitoring

A script can check whether an important service is running.

### E. Input Validation

A script can reject invalid values before performing an operation.

Conditional logic allows these tools to make decisions automatically.

---

## 15. Lesson Summary

In this lesson, we learned:

- Conditional statements allow scripts to make decisions.
- `if` executes commands when a condition is true.
- `else` provides an alternative.
- `elif` checks additional conditions.
- Numeric operators compare integers.
- String operators compare text.
- Logical operators combine conditions.
- File tests inspect files and directories.
- `case` handles multiple possible choices.
- Command exit statuses can be used in conditions.

### Important Syntax

| Syntax | Purpose |
|---|---|
| `if` | Starts a condition |
| `elif` | Checks another condition |
| `else` | Provides an alternative |
| `fi` | Ends an if statement |
| `case` | Starts pattern-based selection |
| `esac` | Ends a case statement |
| `-eq` | Equal |
| `-ne` | Not equal |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater than or equal |
| `-le` | Less than or equal |
| `-f` | Regular file exists |
| `-d` | Directory exists |
| `-e` | Path exists |
| `&&` | Logical AND |
| `||` | Logical OR |
| `!` | Logical NOT |

---

## 16. Knowledge Check

Answer these questions in your notebook:

1. What are conditional statements?
2. Explain the purpose of `if`, `elif`, and `else`.
3. What does `fi` represent?
4. Differentiate `-eq` and `-ne`.
5. What is the difference between string and numeric comparisons?
6. Explain the use of `&&` and `||`.
7. What does `-f` check?
8. What does `-d` check?
9. What is the purpose of the `case` statement?
10. How can conditional statements improve a cybersecurity script?

### Practical Assessment

Successfully create and execute:

1. `number-check.sh`
2. `file-check.sh`
3. `menu.sh`
4. `security-check.sh`

Test valid inputs, invalid inputs, existing paths, and nonexistent paths.

**Lesson 07 is complete when all four scripts work correctly.**
