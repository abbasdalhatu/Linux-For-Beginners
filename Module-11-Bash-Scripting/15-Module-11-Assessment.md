
# Module 11 – Final Assessment: Bash Scripting

## 1. Assessment Overview

Welcome to the final assessment for Module 11 – Bash Scripting.

This assessment evaluates your understanding of Bash scripting, from basic script creation to practical Linux automation.

### Topics Covered

1. Introduction to Bash scripting.
2. Understanding the Bash shell.
3. Creating and executing scripts.
4. Variables and data types.
5. User input and output.
6. Command-line arguments.
7. Conditional statements.
8. Loops.
9. Functions.
10. Strings and numbers.
11. File operations and testing.
12. Exit codes and error handling.
13. Text processing and pipelines.
14. Practical Bash automation projects.

### Assessment Structure

| Section | Assessment Type |
|---|---|
| A | Multiple-choice questions |
| B | Short-answer questions |
| C | Command interpretation |
| D | Practical scripting exercises |
| E | Final integrated project |

---

# SECTION A: MULTIPLE-CHOICE QUESTIONS

Choose the correct answer.

### Question 1

What does Bash stand for?

A. Basic Advanced Shell  
B. Bourne Again Shell  
C. Binary Application Shell  
D. Basic Automated System Handler

### Question 2

Which symbol is commonly used to identify the beginning of a Bash script?

A. `//`  
B. `<!--`  
C. `#!`  
D. `##`

### Question 3

What does the following command do?

```bash
chmod +x script.sh
```

A. Deletes the script.  
B. Makes the script executable.  
C. Encrypts the script.  
D. Opens the script.

### Question 4

Which variable stores the first command-line argument?

A. `$0`  
B. `$1`  
C. `$#`  
D. `$?`

### Question 5

What does `$#` represent?

A. Number of arguments.  
B. Current username.  
C. Previous exit status.  
D. Script filename.

### Question 6

Which command reads input from a user?

A. `printf`  
B. `echo`  
C. `read`  
D. `cat`

### Question 7

Which statement is used for conditional execution?

A. `if`  
B. `loop`  
C. `repeat`  
D. `select`

### Question 8

Which keyword ends an `if` statement in Bash?

A. `end`  
B. `stop`  
C. `fi`  
D. `done`

### Question 9

Which loop is useful when you want to repeat an operation while a condition remains true?

A. `while`  
B. `case`  
C. `function`  
D. `return`

### Question 10

Which command defines a function in Bash?

A. `method`  
B. `function`  
C. `class`  
D. `object`

### Question 11

Which command displays the number of lines in a file?

A. `grep -n`  
B. `wc -l`  
C. `cut -l`  
D. `sort -l`

### Question 12

What does `grep` do?

A. Deletes files.  
B. Searches for matching text.  
C. Changes permissions.  
D. Creates directories.

### Question 13

What does `>` do?

A. Appends output.  
B. Redirects output and replaces existing file content.  
C. Deletes a directory.  
D. Starts a loop.

### Question 14

What does exit code `0` normally indicate?

A. Failure.  
B. Permission denied.  
C. Success.  
D. Command not found.

### Question 15

Which command checks the syntax of a Bash script without executing it?

A. `bash -n script.sh`  
B. `bash -x script.sh`  
C. `chmod script.sh`  
D. `grep script.sh`

### Question 16

Which command enables detection of failures within a pipeline?

A. `set -u`  
B. `set -o pipefail`  
C. `set -x`  
D. `set -v`

### Question 17

Which command extracts fields from delimiter-separated text?

A. `cut`  
B. `touch`  
C. `mkdir`  
D. `chmod`

### Question 18

What does `uniq` do?

A. Encrypts files.  
B. Handles adjacent repeated lines.  
C. Displays system information.  
D. Changes usernames.

### Question 19

Which command calculates a SHA-256 checksum?

A. `sha256sum`  
B. `md5edit`  
C. `hashfile`  
D. `checksum -a`

### Question 20

Which command displays the last lines of a file?

A. `head`  
B. `tail`  
C. `cut`  
D. `sort`

---

# SECTION B: SHORT-ANSWER QUESTIONS

Answer in your own words.

1. Explain what Bash scripting is.
2. Differentiate between a shell and a terminal.
3. What is the purpose of a shebang?
4. Explain the difference between `bash script.sh` and `./script.sh`.
5. What is the difference between a local variable and an environment variable?
6. Explain the purpose of `$0`, `$1`, `$#`, `$@`, and `$?`.
7. What is the difference between `if`, `elif`, and `else`?
8. Explain the difference between `for`, `while`, and `until` loops.
9. Why are functions useful in Bash scripts?
10. Explain the difference between `return` and `exit`.
11. What is the purpose of file testing operators such as `-f`, `-d`, and `-r`?
12. Explain standard input, standard output, and standard error.
13. What are exit codes?
14. Explain why `set -euo pipefail` is useful and why it does not replace explicit error handling.
15. How can `grep`, `awk`, and `sed` help a cybersecurity analyst?

---

# SECTION C: COMMAND INTERPRETATION

Explain what each command does.

### Question 1

```bash
mkdir -p ~/security-lab/logs
```

### Question 2

```bash
grep -in "failed login" security.log
```

### Question 3

```bash
find . -type f -name "*.sh"
```

### Question 4

```bash
chmod 755 monitor.sh
```

### Question 5

```bash
sort users.txt | uniq -c
```

### Question 6

```bash
df -h
```

### Question 7

```bash
awk '{print $1, $3}' students.txt
```

### Question 8

```bash
sha256sum important.txt
```

### Question 9

```bash
bash -n automation.sh
```

### Question 10

```bash
command -v python3
```

---

# SECTION D: PRACTICAL SCRIPTING EXERCISES

Complete these exercises inside your Kali Linux virtual machine.

Create your assessment directory:

```bash
mkdir -p ~/module11-lab/assessment
cd ~/module11-lab/assessment
```

## Exercise 1: Personal Information Script

Create:

`personal-info.sh`

Requirements:

- Ask the user for their name.
- Ask for their course.
- Ask for their academic level.
- Display a formatted summary.
- Reject empty input.

## Exercise 2: Number Classification Script

Create:

`number-checker.sh`

Requirements:

- Accept a number from the user.
- Validate that it is a whole number.
- Determine whether it is positive, negative, or zero.
- Determine whether it is even or odd.

## Exercise 3: File Inspection Script

Create:

`file-inspector.sh`

Requirements:

- Accept a filename as an argument.
- Check whether the file exists.
- Determine whether it is readable.
- Display its size.
- Display its line count.
- Return a non-zero exit status if the file is missing.

## Exercise 4: User Activity Analyzer

Create:

`activity-analyzer.sh`

Requirements:

- Read a sample text log.
- Count total entries.
- Count occurrences of `ERROR`.
- Count occurrences of `WARNING`.
- Extract IP-like addresses.
- Display unique addresses.

Use only the sample logs you create in your lab.

## Exercise 5: Backup Script

Create:

`safe-backup.sh`

Requirements:

- Accept a filename as an argument.
- Check whether the file exists.
- Create a backup directory inside the assessment folder.
- Create a timestamped backup.
- Compare the original and backup files.
- Report success or failure.

Do not back up system files.

---

# SECTION E: FINAL INTEGRATED PROJECT

## Project: Linux Administration and Security Toolkit

<badge color="success">Capstone project</badge>

### Project Objective

Develop a menu-driven Bash application that combines the most important skills learned throughout Module 11.

The application should provide several basic Linux administration and defensive security functions.

### Project Directory

```bash
mkdir -p ~/module11-lab/assessment/final-toolkit
cd ~/module11-lab/assessment/final-toolkit
```

### Required File

Create:

`linux-toolkit.sh`

### Required Menu

Your program should display:

```text
========================================
       LINUX ADMINISTRATION TOOLKIT
========================================

1. System Information
2. Disk Usage
3. Memory Usage
4. Network Information
5. File Inspection
6. Security Log Analysis
7. Create Backup
8. Exit

========================================
```

### Functional Requirements

**Option 1: System Information**

Display:

- Hostname.
- Current user.
- Kernel version.
- Operating system.

**Option 2: Disk Usage**

Display filesystem usage using:

```bash
df -h
```

**Option 3: Memory Usage**

Display memory information using:

```bash
free -h
```

**Option 4: Network Information**

Display network interface information using:

```bash
ip address
```

**Option 5: File Inspection**

Ask the user for a filename inside the toolkit's dedicated lab directory.

Display:

- File existence.
- File size.
- Number of lines.
- Read permissions.

Do not allow the exercise to inspect arbitrary system paths.

**Option 6: Security Log Analysis**

Analyze a sample log file and display:

- Total entries.
- Error count.
- Warning count.
- Failed login count.
- Unique IP-like addresses.

**Option 7: Create Backup**

Ask for a filename inside the lab directory and create a timestamped backup.

Verify the backup using `cmp`.

**Option 8: Exit**

Terminate the application gracefully.

### Technical Requirements

Your project must demonstrate:

- Variables.
- User input.
- Command-line arguments or input validation.
- Conditional statements.
- Loops.
- Functions.
- File operations.
- Pipelines.
- Exit codes.
- Error handling.
- Logging.

### Testing Requirements

Test your project using:

1. Valid input.
2. Invalid input.
3. Empty input.
4. Missing files.
5. Existing files.
6. Successful backups.
7. Incorrect menu selections.

Check the syntax:

```bash
bash -n linux-toolkit.sh
```

Trace execution when debugging:

```bash
bash -x linux-toolkit.sh
```

---

# SECTION F: FINAL REVIEW

Before completing Module 11, ensure you can confidently explain:

- What Bash is.
- How Linux executes scripts.
- How to create executable scripts.
- How to use variables and arguments.
- How to accept user input.
- How to write conditions and loops.
- How to define and call functions.
- How to work with strings and numbers.
- How to manipulate files.
- How to handle errors.
- How to process text.
- How to connect commands using pipelines.
- How to automate administrative tasks.
- How to create basic defensive cybersecurity tools.

## Module 11 Completion Checklist

- [ ] All 15 lesson notes completed.
- [ ] Basic Bash scripts created.
- [ ] Variables and arguments practised.
- [ ] Conditional statements and loops practised.
- [ ] Functions implemented.
- [ ] File operations tested.
- [ ] Exit codes and error handling practised.
- [ ] Text-processing commands practised.
- [ ] Automation projects completed.
- [ ] Final toolkit created and tested.

---

# FINAL MODULE 11 DECLARATION

By completing this module, you have developed a foundation in Bash scripting and Linux automation.

You can now use Bash to automate repetitive tasks, inspect files, process text, generate reports, and build basic defensive administration utilities.

**Next Module: Module 12 – Linux System Administration**

The next module will introduce more advanced Linux administration concepts, including:

- Managing Linux services.
- Monitoring system resources.
- Scheduling tasks.
- Managing system logs.
- Managing storage.
- System maintenance and troubleshooting.
