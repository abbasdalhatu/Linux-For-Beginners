# PRACTICAL LABS
# MODULE 03 – BASH SCRIPTING CHALLENGES

## LAB 03: CONDITIONAL STATEMENTS

### 1. Lab Overview

**Objective:** Learn how to make Bash scripts take decisions based on conditions.

So far, your scripts have executed commands from top to bottom.

But what happens when a script needs to make a decision?

For example:

- If a user enters the correct password, display a success message.
- If a file exists, display its contents.
- If a number is greater than 10, report that it is greater.
- If a system log is missing, display a warning.

This is where conditional statements become useful.

### 2. What Is a Conditional Statement?

A conditional statement allows a program to execute different instructions depending on whether a condition is true or false.

In Bash, the most common conditional structure is:

```bash
if [ condition ]; then
    # Commands when condition is true
else
    # Commands when condition is false
fi
```

Important keywords:

| Keyword | Purpose |
|---|---|
| `if` | Starts a condition |
| `then` | Begins the commands for a true condition |
| `else` | Provides an alternative |
| `elif` | Checks another condition |
| `fi` | Ends the conditional statement |

### 3. Preparing Your Lab

Create your working directory:

```bash
mkdir -p ~/practical-labs/module03/lab03
cd ~/practical-labs/module03/lab03
```

Confirm:

```bash
pwd
```

### 4. Your First if Statement

Create:

```bash
nano first-if.sh
```

Enter:

```bash
#!/bin/bash

number=15

if [ "$number" -gt 10 ]; then
    echo "The number is greater than 10."
fi
```

Execute:

```bash
bash first-if.sh
```

Output:

```text
The number is greater than 10.
```

Explanation:

- `number=15` stores the value.
- `if` starts the condition.
- `-gt` means greater than.
- `then` starts the commands to execute if the condition is true.
- `fi` closes the condition.

### 5. Understanding Numeric Comparison Operators

Bash provides special operators for comparing integers.

| Operator | Meaning | Example |
|---|---|---|
| `-eq` | Equal to | `[ "$a" -eq "$b" ]` |
| `-ne` | Not equal to | `[ "$a" -ne "$b" ]` |
| `-gt` | Greater than | `[ "$a" -gt "$b" ]` |
| `-lt` | Less than | `[ "$a" -lt "$b" ]` |
| `-ge` | Greater than or equal to | `[ "$a" -ge "$b" ]` |
| `-le` | Less than or equal to | `[ "$a" -le "$b" ]` |

Example:

```bash
#!/bin/bash

a=20
b=10

if [ "$a" -gt "$b" ]; then
    echo "A is greater than B."
fi
```

Remember: These operators are for integer comparisons.

### 6. Using if and else

The `else` statement allows your script to execute another set of commands when the condition is false.

Create:

```bash
nano if-else.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter your age: " age

if [[ "$age" -ge 18 ]]; then
    echo "You are 18 or older."
else
    echo "You are below 18."
fi
```

Execute:

```bash
bash if-else.sh
```

Try entering different ages.

Note: This example assumes an integer input. We will learn better input validation in the exercises.

### 7. Using elif

Sometimes you need to check multiple conditions.

The `elif` keyword allows you to check additional conditions.

Create:

```bash
nano grade-checker.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter your score: " score

if [[ "$score" -ge 70 ]]; then
    echo "Grade: A"
elif [[ "$score" -ge 60 ]]; then
    echo "Grade: B"
elif [[ "$score" -ge 50 ]]; then
    echo "Grade: C"
else
    echo "Grade: F"
fi
```

Example:

```text
Enter your score: 75
Grade: A
```

The conditions are checked from top to bottom.

Once a condition is true, Bash executes its block and skips the remaining branches.

### 8. String Comparison

Conditional statements can also compare text.

Common string operators:

| Operator | Meaning |
|---|---|
| `=` | Strings are equal |
| `!=` | Strings are different |
| `-z` | String is empty |
| `-n` | String is not empty |

Example:

```bash
#!/bin/bash

password="admin123"

if [[ "$password" == "admin123" ]]; then
    echo "The values match."
else
    echo "The values do not match."
fi
```

Important: This is only a string-comparison exercise. Do not use hardcoded passwords as a real authentication system.

### 9. Checking Whether a File Exists

Bash can check files and directories.

Important file-test operators:

| Operator | Purpose |
|---|---|
| `-e` | Path exists |
| `-f` | Regular file exists |
| `-d` | Directory exists |
| `-r` | File is readable |
| `-w` | File is writable |
| `-x` | File is executable |
| `-s` | File exists and is not empty |

Create:

```bash
nano file-checker.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter a filename: " filename

if [[ -f "$filename" ]]; then
    echo "The file exists."
else
    echo "The file does not exist."
fi
```

Execute:

```bash
bash file-checker.sh
```

Test it using a file in your current lab directory.

For example:

```text
Enter a filename: first-if.sh
The file exists.
```

### 10. Checking Directories

Create:

```bash
nano directory-checker.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter a directory name: " directory

if [[ -d "$directory" ]]; then
    echo "Directory exists."
else
    echo "Directory does not exist."
fi
```

Test:

```bash
bash directory-checker.sh
```

You can enter `.` to check the current directory.

### 11. Logical Operators

Sometimes you need to combine multiple conditions.

Bash supports logical operators.

| Operator | Meaning |
|---|---|
| `&&` | AND |
| `||` | OR |
| `!` | NOT |

**AND example**

Both conditions must be true.

```bash
#!/bin/bash

age=25
score=80

if [[ "$age" -ge 18 && "$score" -ge 50 ]]; then
    echo "Both conditions are satisfied."
else
    echo "At least one condition failed."
fi
```

**OR example**

At least one condition must be true.

```bash
#!/bin/bash

day="Saturday"

if [[ "$day" == "Saturday" || "$day" == "Sunday" ]]; then
    echo "It is the weekend."
else
    echo "It is a weekday."
fi
```

**NOT example**

The `!` operator reverses a condition.

```bash
#!/bin/bash

file="report.txt"

if [[ ! -f "$file" ]]; then
    echo "The report file is missing."
fi
```

### 12. Using case Statements

The `case` statement is useful when a script needs to choose between several options.

Create:

```bash
nano menu.sh
```

Enter:

```bash
#!/bin/bash

echo "========== LINUX MENU =========="
echo "1. Show current user"
echo "2. Show current directory"
echo "3. Show system date"
echo "4. Show hostname"
echo "5. Exit"

read -r -p "Choose an option: " choice

case "$choice" in
    1)
        whoami
        ;;
    2)
        pwd
        ;;
    3)
        date
        ;;
    4)
        hostname
        ;;
    5)
        echo "Exiting menu."
        ;;
    *)
        echo "Invalid option."
        ;;
esac
```

Execute:

```bash
bash menu.sh
```

Understanding the syntax:

- `case` begins the selection structure.
- Each option has a matching pattern.
- `)` separates the pattern from its commands.
- `;;` ends an option.
- `*)` handles all unmatched options.
- `esac` closes the structure.

### 13. Input Validation with Conditions

A reliable script should reject invalid input.

Create:

```bash
nano validated-score.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter a score (0-100): " score

if [[ ! "$score" =~ ^[0-9]+$ ]]; then
    echo "Error: Enter whole numbers only."
    exit 1
fi

if (( score < 0 || score > 100 )); then
    echo "Error: Score must be between 0 and 100."
    exit 1
fi

echo "Valid score: $score"
```

This script uses a regular expression to check whether the input contains digits only.

The expression:

```bash
^[0-9]+$
```

means:

- `^` = beginning of input.
- `[0-9]` = a digit from 0 to 9.
- `+` = one or more digits.
- `$` = end of input.

The arithmetic condition then checks the permitted range.

### 14. Practical Exercise 1 – Number Checker

Create:

```text
number-checker.sh
```

Requirements:

1. Ask the user to enter a number.
2. Check whether it is greater than zero.
3. If it is greater than zero, display a positive-number message.
4. Otherwise, display that it is zero or negative.

### 15. Practical Exercise 2 – Grade Calculator

Create:

```text
grade-calculator.sh
```

Requirements:

- Ask the user for a score.
- Accept scores from 0 to 100 only.
- Reject letters and invalid values.
- Use these grading rules:

| Score | Grade |
|---|---|
| 70–100 | A |
| 60–69 | B |
| 50–59 | C |
| 0–49 | F |

Remember that your validation must happen before the grade comparison.

### 16. Practical Exercise 3 – File Security Checker

Create:

```text
file-security-checker.sh
```

Requirements:

1. Ask the user for a filename.
2. Check whether the file exists.
3. Check whether it is readable.
4. Check whether it is writable.
5. Check whether it is executable.
6. Display the results.

Use:

- `-f`
- `-r`
- `-w`
- `-x`

Only inspect files inside your lab directory.

### 17. Practical Exercise 4 – Interactive Linux Menu

Create:

```text
linux-menu.sh
```

Your menu must contain:

1. Display current username.
2. Display hostname.
3. Display current directory.
4. Display system date.
5. Exit.

Requirements:

- Use a `case` statement.
- Display an error for invalid options.
- Do not modify system files.

### 18. Syntax Validation

Check your scripts:

```bash
bash -n first-if.sh
bash -n if-else.sh
bash -n grade-checker.sh
bash -n file-checker.sh
bash -n directory-checker.sh
bash -n menu.sh
bash -n validated-score.sh
```

Inspect your files:

```bash
ls -l
```

### 19. Cybersecurity Application

Conditional statements are widely used in defensive security automation.

For example, an administrator may need to check whether a particular security log exists.

```bash
#!/bin/bash

log_file="/var/log/auth.log"

if [[ -f "$log_file" ]]; then
    echo "Authentication log exists."
else
    echo "Warning: Authentication log was not found."
fi
```

This script only checks for the existence of a log file.

It does not modify or delete anything.

Other security applications include:

- Checking whether a service is running.
- Identifying missing security files.
- Detecting unexpected file permissions.
- Checking whether a backup exists.
- Validating user input before an operation.

### 20. Knowledge Check

1. What is a conditional statement?
2. What does `if` do?
3. What is the purpose of `then`?
4. What is the difference between `else` and `elif`?
5. What does `-eq` mean?
6. What does `-f` check?
7. What does `-d` check?
8. What is the purpose of `&&`?
9. What does the `*)` pattern represent in a case statement?
10. Why is input validation important in security scripts?

### 21. Lab Completion Checklist

- [ ] Created the Lab 03 directory.
- [ ] Practised basic `if` statements.
- [ ] Used `if` and `else`.
- [ ] Used `elif`.
- [ ] Compared strings.
- [ ] Compared numbers.
- [ ] Checked file and directory existence.
- [ ] Used logical operators.
- [ ] Created a `case` menu.
- [ ] Practised input validation.
- [ ] Completed all four practical exercises.
- [ ] Checked Bash syntax.

### 22. Expected Outcome

By completing this lab, you should understand how to:

- Make decisions using Bash.
- Compare strings and numbers.
- Check files and directories.
- Combine multiple conditions.
- Build interactive command-line menus.
- Validate user input.
- Apply conditional logic to defensive security automation.

**Next Lab:** Loops and Iteration.