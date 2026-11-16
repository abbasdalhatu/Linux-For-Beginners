# PRACTICAL LABS
# MODULE 03 – BASH SCRIPTING CHALLENGES

## LAB 02: VARIABLES AND USER INPUT

### 1. Lab Overview

**Objective:** Learn how to store information in variables, retrieve user input, and use stored values in Bash scripts.

In Lab 01, you created scripts that executed commands automatically.

Now you will learn how to make scripts interactive.

For example, instead of writing:

```bash
echo "Welcome, Abbas"
```

You can create a script that asks for someone's name and welcomes them automatically.

This is possible through **variables and user input**.

### 2. What Is a Variable?

A variable is a named location used to store information that a script can access and use.

Example:

```bash
#!/bin/bash

name="Abbas"

echo "$name"
```

Output:

```text
Abbas
```

Here:

- `name` is the variable.
- `Abbas` is the stored value.
- `$name` retrieves the stored value.

### 3. Creating Variables

Create a working directory:

```bash
mkdir -p ~/practical-labs/module03/lab02
cd ~/practical-labs/module03/lab02
```

Create your first variable script:

```bash
nano variables.sh
```

Enter:

```bash
#!/bin/bash

name="Abbas"
course="Computer Science"
level="Level 3"

echo "Student Information"
echo "Name: $name"
echo "Course: $course"
echo "Level: $level"
```

Save and execute:

```bash
bash variables.sh
```

### 4. Rules for Naming Variables

Follow these rules:

1. Variable names should begin with a letter or underscore.
2. They can contain letters, numbers, and underscores.
3. They must not contain spaces.
4. Variable names are case-sensitive.
5. Do not put spaces around the assignment operator.

Correct:

```bash
name="Abbas"
student_level=3
user_name="kali"
```

Incorrect:

```bash
name = "Abbas"
student level=3
```

Remember:

```bash
name="Abbas"
```

is correct because there are no spaces around `=`.

### 5. Accessing Variables

Use `$` to retrieve a variable's value.

```bash
name="Abbas"

echo "$name"
```

You can also use braces:

```bash
echo "${name}"
```

Braces are especially useful when joining a variable with other text.

Example:

```bash
name="Abbas"

echo "${name} is learning Bash."
```

Output:

```text
Abbas is learning Bash.
```

### 6. Collecting User Input with read

The `read` command allows a script to receive information from a user.

Create:

```bash
nano input.sh
```

Enter:

```bash
#!/bin/bash

echo "What is your name?"

read name

echo "Welcome, $name!"
```

Execute:

```bash
bash input.sh
```

The script waits for you to enter your name.

Example:

```text
What is your name?
Abbas
Welcome, Abbas!
```

### 7. Using read -p

You can display a question and collect input on the same line.

Example:

```bash
#!/bin/bash

read -p "Enter your name: " name

echo "Welcome, $name!"
```

The `-p` option displays a prompt before collecting input.

### 8. Collecting Multiple Inputs

A script can collect several values.

Create:

```bash
nano student-info.sh
```

Enter:

```bash
#!/bin/bash

read -p "Enter your name: " name
read -p "Enter your department: " department
read -p "Enter your level: " level

echo ""
echo "========== STUDENT PROFILE =========="

echo "Name: $name"
echo "Department: $department"
echo "Level: $level"

echo "====================================="
```

Execute:

```bash
bash student-info.sh
```

Enter your information when prompted.

### 9. Working with Numbers

Bash variables can store numeric values.

Example:

```bash
#!/bin/bash

number1=20
number2=10

result=$((number1 + number2))

echo "Result: $result"
```

Output:

```text
Result: 30
```

Bash supports basic arithmetic operations.

| Operator | Meaning | Example |
|---|---|---|
| `+` | Addition | `5 + 3` |
| `-` | Subtraction | `5 - 3` |
| `*` | Multiplication | `5 * 3` |
| `/` | Integer division | `10 / 2` |
| `%` | Remainder | `10 % 3` |

Example:

```bash
#!/bin/bash

a=15
b=4

echo "Addition: $((a + b))"
echo "Subtraction: $((a - b))"
echo "Multiplication: $((a * b))"
echo "Division: $((a / b))"
echo "Remainder: $((a % b))"
```

Note: Bash arithmetic uses integers by default. For example, `15 / 4` produces `3`, not `3.75`.

### 10. Command Substitution

Command substitution allows you to store the output of a Linux command inside a variable.

Example:

```bash
#!/bin/bash

current_user=$(whoami)
current_directory=$(pwd)
current_date=$(date)

echo "User: $current_user"
echo "Directory: $current_directory"
echo "Date: $current_date"
```

The syntax is:

```bash
variable=$(command)
```

This is useful when creating automated system reports.

### 11. Environment Variables

Environment variables provide information about the current shell environment and system session.

Examples:

```bash
echo "$HOME"
echo "$USER"
echo "$PATH"
echo "$SHELL"
```

| Variable | Purpose |
|---|---|
| `$HOME` | Current user's home directory |
| `$USER` | Current username |
| `$PATH` | Directories searched for executable commands |
| `$SHELL` | User's configured shell |

To view environment variables:

```bash
env
```

To inspect a specific variable:

```bash
printenv HOME
```

### 12. Exporting Variables

Normally, a variable created in a shell is available to that shell and its child commands only when exported.

Example:

```bash
MY_NAME="Abbas"

export MY_NAME
```

The `export` command makes the variable available to child processes.

You can also combine assignment and export:

```bash
export PROJECT_NAME="Cybersecurity-Lab"
```

### 13. Safe User Input

Never assume that user input is always correct.

For example, a script expecting a number might receive letters or unexpected symbols.

You should validate input before using it in important operations.

Create:

```bash
nano safe-input.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter your name: " name

if [[ -z "$name" ]]; then
    echo "Error: Name cannot be empty."
    exit 1
fi

echo "Welcome, $name!"
```

Explanation:

- `read -r` reads input without interpreting backslashes as escapes.
- `-z` checks whether a string is empty.
- `exit 1` ends the script with a non-zero status to indicate failure.
- `"$name"` safely quotes the variable.

### 14. Practical Exercise 1 – Personal Profile

Create:

```text
profile.sh
```

Requirements:

- Ask for your name.
- Ask for your department.
- Ask for your academic level.
- Display all the collected information.

### 15. Practical Exercise 2 – Simple Calculator

Create:

```text
calculator.sh
```

Requirements:

1. Ask for the first number.
2. Ask for the second number.
3. Calculate their sum.
4. Calculate their difference.
5. Calculate their product.
6. Display all results.

Starter structure:

```bash
#!/bin/bash

read -p "First number: " first
read -p "Second number: " second

# Add your calculations here
```

For this exercise, enter whole numbers only.

### 16. Practical Exercise 3 – System Information

Create:

```text
system-user.sh
```

Requirements:

- Store the current username in a variable.
- Store the hostname in another variable.
- Store the current directory in another variable.
- Display all three values.

Use command substitution.

### 17. Practical Exercise 4 – Input Validation

Create:

```text
input-validation.sh
```

Requirements:

- Ask the user for their name.
- Check whether the input is empty.
- Display an error if it is empty.
- Display a welcome message if a name is provided.

### 18. Syntax and Execution Checks

Validate your scripts:

```bash
bash -n variables.sh
bash -n input.sh
bash -n student-info.sh
```

Give your completed scripts executable permission:

```bash
chmod +x *.sh
```

Inspect your files:

```bash
ls -l
```

### 19. Cybersecurity Application

Variables are important in security automation.

For example, an administrator might store:

- A username being investigated.
- A log file location.
- A system hostname.
- A report filename.
- A date and time for an investigation.

Example:

```bash
#!/bin/bash

target_user=$(whoami)
log_file="/var/log/auth.log"

echo "Current user: $target_user"
echo "Log location: $log_file"
```

This example only displays information; it does not modify system logs.

**Security reminder:** Do not place passwords, API keys, or other secrets directly inside scripts or expose them in terminal output.

### 20. Knowledge Check

1. What is a variable?
2. Why do we use `$` before a variable name?
3. What does the `read` command do?
4. What is the purpose of `read -p`?
5. What does `$HOME` represent?
6. What is command substitution?
7. What does `export` do?
8. Why should user input be validated?
9. What does `exit 1` indicate?
10. Why should variables generally be enclosed in double quotes when used?

### 21. Lab Completion Checklist

- [ ] Created the Lab 02 working directory.
- [ ] Created variables and stored information.
- [ ] Retrieved variable values.
- [ ] Collected user input using `read`.
- [ ] Collected multiple inputs.
- [ ] Performed basic arithmetic.
- [ ] Used command substitution.
- [ ] Inspected environment variables.
- [ ] Practised input validation.
- [ ] Completed all four practical exercises.
- [ ] Checked Bash syntax.

### 22. Expected Outcome

By completing this lab, you should understand how to:

- Create and use Bash variables.
- Collect information interactively.
- Perform basic arithmetic.
- Store Linux command output.
- Access environment variables.
- Export variables.
- Validate user input safely.

**Next Lab:** Conditional Statements.