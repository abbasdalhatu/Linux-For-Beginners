---
---


# Module 11 – Lesson 05: User Input and Output

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand input and output in Bash.
2. Use `echo` and `printf` to display information.
3. Use `read` to collect user input.
4. Store user input in variables.
5. Understand input prompts.
6. Use `read` options.
7. Create interactive Bash scripts.
8. Build a simple interactive system information tool.

---

## 2. What Is Input and Output?

Input and output are fundamental programming concepts.

**Input** is information supplied to a program.

**Output** is information produced by a program.

For example, imagine a program asks for your name.

- Input: Abbas
- Processing: The program stores your name.
- Output: Welcome, Abbas!

In Bash, we commonly use:

| Command | Purpose |
|---|---|
| `read` | Collects user input |
| `echo` | Displays output |
| `printf` | Formats and displays output |

---

## 3. Understanding Standard Input and Output

Linux uses three standard data streams.

| Stream | Name | File Descriptor | Purpose |
|---|---|---:|---|
| Standard Input | stdin | 0 | Receives input |
| Standard Output | stdout | 1 | Displays normal output |
| Standard Error | stderr | 2 | Displays error messages |

### A. Standard Input (stdin)

Standard input normally comes from the keyboard.

Example:

```bash
read name
```

Bash waits for the user to enter a value.

### B. Standard Output (stdout)

Standard output displays normal program results.

Example:

```bash
echo "Welcome to Linux"
```

### C. Standard Error (stderr)

Standard error displays error messages.

Example:

```bash
ls /directory-that-does-not-exist
```

The error message is sent to standard error.

Understanding these streams will become important when we study redirection and pipelines.

---

## 4. Using the echo Command

The `echo` command displays text or variable values.

Example:

```bash
echo "Welcome to Bash"
```

Output:

```text
Welcome to Bash
```

### Display Variables

```bash
name="Abbas"

echo "Hello, $name"
```

Output:

```text
Hello, Abbas
```

### Display Multiple Values

```bash
name="Abbas"
course="Computer Science"

echo "Name: $name"
echo "Course: $course"
```

### Using Special Characters

You can use `echo -e` to interpret certain escape sequences.

Example:

```bash
echo -e "Name:\tAbbas\nCourse:\tComputer Science"
```

Possible output:

```text
Name:   Abbas
Course: Computer Science
```

For more predictable formatting, `printf` is generally preferred.

---

## 5. Understanding the printf Command

`printf` displays formatted output.

Unlike `echo`, it gives you more control over how information is displayed.

Example:

```bash
printf "Welcome to Bash scripting\n"
```

### Important Format Specifiers

| Specifier | Meaning |
|---|---|
| `%s` | String |
| `%d` | Integer |
| `%f` | Floating-point number |
| `\n` | New line |
| `\t` | Tab |

### Example 1: Displaying a String

```bash
name="Abbas"

printf "Student name: %s\n" "$name"
```

Output:

```text
Student name: Abbas
```

### Example 2: Displaying an Integer

```bash
age=21

printf "Age: %d\n" "$age"
```

Output:

```text
Age: 21
```

### Example 3: Displaying Multiple Variables

```bash
name="Abbas"
course="Computer Science"
level=300

printf "Name: %s\nCourse: %s\nLevel: %d\n" "$name" "$course" "$level"
```

`printf` is especially useful when creating clean reports and menus.

---

## 6. Using the read Command

The `read` command collects information entered by a user.

Basic syntax:

```bash
read variable_name
```

Example:

```bash
echo "Enter your name:"
read name

echo "Welcome, $name"
```

When the script runs:

1. It displays a message.
2. It waits for user input.
3. It stores the input in `name`.
4. It displays the stored value.

---

## 7. Using read with a Prompt

You can display a prompt directly with `read`.

Example:

```bash
read -p "Enter your name: " name

echo "Welcome, $name"
```

The `-p` option displays a prompt before collecting input.

This makes scripts more interactive.

### Another Example

```bash
read -p "Enter your university: " university

echo "University: $university"
```

---

## 8. Using read with Multiple Variables

Bash can collect multiple values in one `read` command.

Example:

```bash
read -p "Enter your first and last name: " first_name last_name

echo "First name: $first_name"
echo "Last name: $last_name"
```

Input:

```text
Abbas Muhammad
```

Output:

```text
First name: Abbas
Last name: Muhammad
```

Bash assigns the words to the variables in order. If extra words are entered, the last variable receives the remaining text.

---

## 9. Understanding Important read Options

The `read` command supports several useful options.

| Option | Purpose |
|---|---|
| `-p` | Displays a prompt |
| `-s` | Hides typed input |
| `-r` | Prevents backslashes from being treated as escape characters |
| `-t` | Sets an input timeout |
| `-n` | Reads a limited number of characters |
| `-a` | Stores input words in an array |

### A. Using -p

```bash
read -p "Enter your name: " name
```

### B. Using -s

Useful when collecting a password interactively.

```bash
read -s -p "Enter password: " password
echo
echo "Password received."
```

The password is not displayed while typing.

**Security note:** Hiding input does not encrypt or securely store the password. Never print passwords or save them in ordinary log files.

### C. Using -t

Sets a timeout.

```bash
if read -t 10 -p "Enter your name within 10 seconds: " name; then
    echo "Welcome, $name"
else
    echo "Input timed out."
fi
```

### D. Using -r

```bash
read -r -p "Enter a file path: " filepath
```

This preserves backslashes as literal input.

It is generally a good habit to use `read -r` when collecting ordinary text.

---

## 10. Combining Input and Output

You can combine `read`, variables, `echo`, and `printf` to create interactive programs.

Example:

```bash
#!/bin/bash

read -r -p "Enter your name: " name
read -r -p "Enter your course: " course

printf "\n========== STUDENT PROFILE ==========\n"
printf "Name: %s\n" "$name"
printf "Course: %s\n" "$course"
printf "=====================================\n"
```

This program collects two pieces of information and displays them in a formatted report.

---

## 11. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson05
cd ~/module11-lab/lesson05
```

### Step 2: Create an Interactive Greeting Script

```bash
nano greeting.sh
```

Enter:

```bash
#!/bin/bash

echo "========== WELCOME =========="

read -r -p "Enter your name: " name

echo "Hello, $name!"
echo "Welcome to Bash scripting."

echo "============================="
```

Save the file.

Make it executable:

```bash
chmod +x greeting.sh
```

Run it:

```bash
./greeting.sh
```

Enter your name when prompted.

### Step 3: Create a Student Information Script

```bash
nano student-info.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter your full name: " name
read -r -p "Enter your course: " course
read -r -p "Enter your university: " university
read -r -p "Enter your level: " level

printf "\n========== STUDENT INFORMATION ==========\n"

printf "Name: %s\n" "$name"
printf "Course: %s\n" "$course"
printf "University: %s\n" "$university"
printf "Level: %s\n" "$level"

printf "=========================================\n"
```

Save and execute:

```bash
chmod +x student-info.sh
./student-info.sh
```

Enter sample information and inspect the output.

---

## 12. Understanding Input Validation

Interactive scripts should not blindly accept every input.

For example, imagine a program asks for an age.

The user might enter:
- `21`
- `hello`
- An empty value.

A simple validation check can reject empty input.

Example:

```bash
#!/bin/bash

read -r -p "Enter your name: " name

if [[ -z "$name" ]]; then
    echo "Error: Name cannot be empty."
else
    echo "Welcome, $name"
fi
```

Explanation:

- `[[ ... ]]` performs a conditional test.
- `-z` checks whether a string is empty.
- The `if` statement chooses what to do.

We will study conditional statements in detail in Lesson 07.

### Validate an Integer

Example:

```bash
#!/bin/bash

read -r -p "Enter your age: " age

if [[ "$age" =~ ^[0-9]+$ ]]; then
    echo "Valid integer: $age"
else
    echo "Please enter a positive whole number."
fi
```

The regular expression checks that the input contains one or more digits.

---

## 13. Practical Challenge: Create an Interactive System Report

Create:

```bash
nano interactive-report.sh
```

Enter:

```bash
#!/bin/bash

echo "===================================="
echo "       SYSTEM REPORT GENERATOR"
echo "===================================="

read -r -p "Enter your name: " operator
read -r -p "Enter report purpose: " purpose

echo
echo "Generating report..."

printf "\n========== SYSTEM REPORT ==========\n"

printf "Operator: %s\n" "$operator"
printf "Purpose: %s\n" "$purpose"
printf "Username: %s\n" "$(whoami)"
printf "Hostname: %s\n" "$(hostname)"
printf "Current date: %s\n" "$(date)"
printf "Current directory: %s\n" "$(pwd)"

printf "===================================\n"
printf "REPORT GENERATED SUCCESSFULLY\n"
```

Make it executable:

```bash
chmod +x interactive-report.sh
```

Run:

```bash
./interactive-report.sh
```

Enter your information and verify the generated report.

---

## 14. Cybersecurity Application

Input and output are important in cybersecurity automation.

Consider a defensive security tool that asks an administrator for:

- A log file to inspect.
- A directory to check.
- A network interface to monitor.
- A report filename.

The script collects the information and processes it.

For example, a security monitoring tool might ask:

```bash
read -r -p "Enter log file path: " logfile
```

It can then inspect the specified log file.

However, scripts must validate user input before using it in file operations or commands.

Never assume that user-supplied input is safe.

---

## 15. Lesson Summary

In this lesson, we learned:

- Input is information supplied to a program.
- Output is information produced by a program.
- Linux provides stdin, stdout, and stderr.
- `echo` displays information.
- `printf` provides formatted output.
- `read` collects user input.
- `read -p` displays an input prompt.
- `read -s` hides typed input.
- `read -r` preserves backslashes.
- Input validation helps prevent incorrect data from being processed.

### Important Commands

| Command | Purpose |
|---|---|
| `echo "Hello"` | Display text |
| `printf "%s\n" "$name"` | Formatted output |
| `read name` | Collect input |
| `read -r name` | Collect literal text |
| `read -p "Name: " name` | Prompt for input |
| `read -s password` | Hide typed input |
| `read -t 10 name` | Set input timeout |
| `[[ -z "$name" ]]` | Check for empty input |

---

## 16. Knowledge Check

Answer these questions in your notebook:

1. Define input and output.
2. What are stdin, stdout, and stderr?
3. What is the purpose of the `echo` command?
4. How is `printf` different from `echo`?
5. What does the `read` command do?
6. What is the purpose of `read -p`?
7. Why is `read -s` useful?
8. What does `read -r` do?
9. Why should user input be validated?
10. Explain how a Bash script can collect and display information.

### Practical Assessment

Successfully create and execute:

1. `greeting.sh`
2. `student-info.sh`
3. `interactive-report.sh`

Your scripts must collect user input and display the information correctly.

**Lesson 05 is complete when all three scripts work successfully.**
