---
---


# Module 11 – Lesson 12: Exit Codes and Error Handling

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain exit codes in Linux.
2. Understand the meaning of exit status `0` and non-zero statuses.
3. Use `$?` to inspect command results.
4. Use the `exit` command.
5. Handle errors using conditional statements.
6. Understand `set -e`, `set -u`, and `set -o pipefail`.
7. Use error messages and logging.
8. Build reliable Bash scripts with error handling.

---

## 2. What Are Exit Codes?

An exit code is a numeric value returned by a command or program when it finishes execution.

It communicates whether the operation succeeded or failed.

Linux uses exit codes to help scripts make decisions.

### The Two Main Categories

| Exit Code | Meaning |
|---|---|
| `0` | Success |
| Non-zero | Failure or another status |

For example:

```bash
ls
```

If the command succeeds, its exit status is normally `0`.

If a command fails, it may return a non-zero status.

**Important:** Non-zero does not always mean a system error. Some commands use different exit statuses to represent conditions such as "no match found."

---

## 3. Understanding the $? Variable

Bash provides a special variable called `$?`.

It contains the exit status of the most recently executed command.

### Example 1: Successful Command

```bash
ls
echo $?
```

Expected result:

```text
0
```

### Example 2: Failed Command

```bash
ls /directory-that-does-not-exist
echo $?
```

The command will fail, and its exit status will be non-zero.

### Important Rule

The `$?` value changes after every command.

Example:

```bash
ls
echo "Command completed"
echo $?
```

The last `echo` command succeeded, so the final result is `0`.

To preserve an exit code, store it immediately:

```bash
ls /nonexistent
status=$?

echo "Exit status: $status"
```

---

## 4. Using Exit Codes with if Statements

You can use exit statuses to make decisions.

### Example

```bash
#!/bin/bash

ls /home

if [[ $? -eq 0 ]]; then
    echo "Command executed successfully."
else
    echo "Command failed."
fi
```

A cleaner method is to place the command directly inside the condition:

```bash
#!/bin/bash

if ls /home >/dev/null 2>&1; then
    echo "Command executed successfully."
else
    echo "Command failed."
fi
```

This avoids accidentally checking the status of another command.

### Another Example: Checking a File

```bash
#!/bin/bash

if grep -q "ERROR" sample.log; then
    echo "An error was found."
else
    echo "No matching error was found."
fi
```

Here, `grep` returns a success status when it finds a match.

---

## 5. Understanding the exit Command

The `exit` command terminates a Bash script and returns an exit status.

### Example 1: Successful Exit

```bash
#!/bin/bash

echo "Program completed successfully."

exit 0
```

### Example 2: Error Exit

```bash
#!/bin/bash

echo "An error occurred."

exit 1
```

### Common Convention

| Status | Common Meaning |
|---|---|
| `0` | Success |
| `1` | General error |
| `2` | Incorrect command usage in many programs |
| `126` | Command found but cannot be executed |
| `127` | Command not found |
| `128 + signal` | Process terminated by a signal |

These are common conventions, not universal meanings for every program.

### Example: Validate Input

```bash
#!/bin/bash

read -r -p "Enter your age: " age

if [[ ! "$age" =~ ^[0-9]+$ ]]; then
    echo "ERROR: Enter a valid whole number."
    exit 1
fi

echo "Valid age: $age"
exit 0
```

---

## 6. Creating Error Messages

Good scripts should communicate errors clearly.

The `printf` command can display messages.

Example:

```bash
#!/bin/bash

read -r -p "Enter a filename: " filename

if [[ -z "$filename" ]]; then
    printf "ERROR: Filename cannot be empty.\n" >&2
    exit 1
fi

echo "Filename received: $filename"
```

### Understanding >&2

`>&2` redirects a message to standard error.

This separates error messages from normal program output.

Example:

```bash
echo "This is normal output."
echo "This is an error message." >&2
```

---

## 7. Handling Missing Files

Scripts should check whether required files exist before trying to use them.

Example:

```bash
#!/bin/bash

filename="report.txt"

if [[ ! -f "$filename" ]]; then
    echo "ERROR: File does not exist." >&2
    exit 1
fi

echo "File found."

cat "$filename"
```

This prevents the script from attempting to read a missing file without explaining the problem.

---

## 8. Understanding set -e

The command:

```bash
set -e
```

tells Bash to exit when certain commands fail.

Example:

```bash
#!/bin/bash

set -e

echo "Starting program..."

ls /home

echo "The previous command succeeded."
```

If an ordinary command fails under the relevant conditions, the script exits before continuing.

### Important Limitation

`set -e` does not behave like a universal error handler.

Its behavior has exceptions involving:
- Commands used in `if` conditions.
- Commands in `while` and `until` tests.
- Parts of `&&` and `||` lists.
- Pipelines and other shell contexts.

Therefore, do not rely on `set -e` alone. Check important operations explicitly.

---

## 9. Understanding set -u

The command:

```bash
set -u
```

causes Bash to report an error when an unset variable is expanded.

### Example Without set -u

```bash
#!/bin/bash

echo "Username: $username"
```

If `username` was never defined, Bash normally treats it as an empty value.

### Example With set -u

```bash
#!/bin/bash

set -u

echo "Username: $username"
```

Bash reports an unbound variable error.

### Safer Default Values

You can provide a default value:

```bash
#!/bin/bash

set -u

echo "Username: ${username:-Guest}"
```

If `username` is unset or empty, the script displays `Guest`.

---

## 10. Understanding set -o pipefail

A pipeline connects commands using the pipe symbol `|`.

Example:

```bash
cat sample.log | grep "ERROR"
```

By default, Bash normally uses the exit status of the last command in a pipeline.

The command:

```bash
set -o pipefail
```

makes a pipeline return a failure status if a command in the pipeline fails.

### Example

```bash
#!/bin/bash

set -o pipefail

false | true

echo "Pipeline status: $?"
```

Expected output:

```text
Pipeline status: 1
```

Without `pipefail`, the pipeline would normally report the status of `true`, which is `0`.

---

## 11. Combining Bash Safety Options

A common script configuration is:

```bash
#!/bin/bash

set -euo pipefail
```

This enables:

- `-e`: Exit on certain command failures.
- `-u`: Treat unset variables as errors.
- `-o pipefail`: Detect failures in pipelines.

This combination is often called strict mode.

### Example

```bash
#!/bin/bash

set -euo pipefail

name="Abbas"

echo "Welcome, $name"

echo "Script completed."
```

### Important Note

Strict mode is helpful, but it is not a replacement for proper input validation, explicit error handling, and testing.

---

## 12. Handling Errors with Functions

Functions can return status codes to communicate success or failure.

Example:

```bash
#!/bin/bash

check_file() {
    local target="$1"

    if [[ -f "$target" ]]; then
        echo "File found."
        return 0
    else
        echo "File not found." >&2
        return 1
    fi
}

if check_file "report.txt"; then
    echo "Continuing program."
else
    echo "Stopping program."
    exit 1
fi
```

This makes error handling reusable.

---

## 13. Logging Errors

Logging means recording events and messages for later inspection.

A script can write messages into a log file.

Example:

```bash
#!/bin/bash

logfile="application.log"

echo "Script started: $(date)" >> "$logfile"

if [[ -f "report.txt" ]]; then
    echo "SUCCESS: Report file found." >> "$logfile"
else
    echo "ERROR: Report file missing." >> "$logfile"
fi
```

View the log:

```bash
cat application.log
```

### Why Logging Matters

Logging helps administrators and security analysts:

- Troubleshoot problems.
- Review script activity.
- Record failures.
- Monitor automation.
- Investigate unexpected behavior.

For real security systems, logs should be protected against unauthorized modification.

---

## 14. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson12
cd ~/module11-lab/lesson12
```

### Step 2: Create an Exit Code Script

```bash
nano exit-status.sh
```

Enter:

```bash
#!/bin/bash

echo "Testing a successful command."

true
status=$?

echo "Successful command status: $status"

echo "Testing a failed command."

false
status=$?

echo "Failed command status: $status"
```

Execute:

```bash
chmod +x exit-status.sh
./exit-status.sh
```

Expected output:

```text
Testing a successful command.
Successful command status: 0
Testing a failed command.
Failed command status: 1
```

### Step 3: Create an Input Validation Script

```bash
nano validate-input.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter a whole number: " number

if [[ ! "$number" =~ ^[0-9]+$ ]]; then
    echo "ERROR: Invalid input." >&2
    exit 1
fi

echo "Valid input: $number"
exit 0
```

Execute:

```bash
chmod +x validate-input.sh
./validate-input.sh
```

Test:
- `25`
- `100`
- `hello`
- `12.5`

Check its exit status:

```bash
./validate-input.sh
echo $?
```

### Step 4: Create a Missing File Handler

```bash
nano file-handler.sh
```

Enter:

```bash
#!/bin/bash

filename="$1"

if [[ -z "$filename" ]]; then
    echo "ERROR: Provide a filename." >&2
    echo "Usage: $0 filename" >&2
    exit 1
fi

if [[ ! -f "$filename" ]]; then
    echo "ERROR: File does not exist." >&2
    exit 1
fi

echo "File found."

if ! cat "$filename"; then
    echo "ERROR: Unable to read file." >&2
    exit 1
fi

echo "File processed successfully."
```

Execute:

```bash
chmod +x file-handler.sh
```

Test with a file that exists:

```bash
echo "Linux error handling practice" > sample.txt
./file-handler.sh sample.txt
```

Test with a nonexistent file:

```bash
./file-handler.sh missing.txt
```

Test without an argument:

```bash
./file-handler.sh
```

### Step 5: Create a Logging Script

```bash
nano logger.sh
```

Enter:

```bash
#!/bin/bash

logfile="activity.log"

log_message() {
    local level="$1"
    local message="$2"

    printf "[%s] [%s] %s\n" \
        "$(date '+%Y-%m-%d %H:%M:%S')" \
        "$level" \
        "$message" >> "$logfile"
}

log_message "INFO" "Script started."

if [[ -f "sample.txt" ]]; then
    log_message "SUCCESS" "Sample file found."
else
    log_message "ERROR" "Sample file missing."
fi

log_message "INFO" "Script completed."

echo "Activity recorded in $logfile"
```

Execute:

```bash
chmod +x logger.sh
./logger.sh
```

View the log:

```bash
cat activity.log
```

---

## 15. Practical Cybersecurity Project: Reliable System Checker

Create a defensive script that checks whether important system information commands are available.

```bash
nano reliable-checker.sh
```

Enter:

```bash
#!/bin/bash

set -u

logfile="system-check.log"

log_message() {
    local level="$1"
    local message="$2"

    printf "[%s] [%s] %s\n" \
        "$(date '+%Y-%m-%d %H:%M:%S')" \
        "$level" \
        "$message" >> "$logfile"
}

check_command() {
    local command_name="$1"

    if command -v "$command_name" >/dev/null 2>&1; then
        echo "[OK] $command_name is available."
        log_message "SUCCESS" "$command_name is available."
        return 0
    else
        echo "[WARNING] $command_name is missing."
        log_message "WARNING" "$command_name is missing."
        return 1
    fi
}

echo "========== SYSTEM CHECKER =========="

check_command "bash"
check_command "uname"
check_command "df"
check_command "free"
check_command "ip"

echo "===================================="
echo "Check completed."

log_message "INFO" "System check completed."
```

Make it executable:

```bash
chmod +x reliable-checker.sh
```

Run:

```bash
./reliable-checker.sh
```

View the log:

```bash
cat system-check.log
```

### What Does This Project Demonstrate?

- Functions.
- Conditional statements.
- Command exit statuses.
- Error messages.
- Logging.
- Command availability checks.

This is a basic defensive system checker, not a complete security assessment.

---

## 16. Common Error-Handling Mistakes

| Mistake | Explanation | Solution |
|---|---|---|
| Ignoring exit codes | Failures go unnoticed | Check important command statuses |
| Using `$?` too late | Another command changes the value | Save it immediately |
| Assuming all non-zero codes mean the same thing | Programs use different status conventions | Understand the command's documentation |
| Using `set -e` alone | Some failures may not trigger automatic exit | Add explicit checks |
| Ignoring missing variables | Scripts may use incorrect values | Consider `set -u` |
| Not checking pipelines | Earlier failures may be hidden | Use `set -o pipefail` |
| Poor error messages | Users cannot identify the problem | Print useful messages to stderr |

### Useful Debugging Commands

Check syntax:

```bash
bash -n reliable-checker.sh
```

Trace execution:

```bash
bash -x reliable-checker.sh
```

Display the last exit status:

```bash
echo $?
```

---

## 17. Lesson Summary

In this lesson, we learned:

- Exit codes communicate command results.
- `0` usually means success.
- Non-zero values indicate failure or another status.
- `$?` stores the most recent exit status.
- `exit` terminates a script with a chosen status.
- `>&2` sends messages to standard error.
- `set -e` detects certain command failures.
- `set -u` detects unset variables.
- `set -o pipefail` detects pipeline failures.
- Functions can return status codes.
- Logging records script activity.
- Error handling makes scripts more reliable.

### Important Syntax

| Syntax | Purpose |
|---|---|
| `$?` | Previous command's exit status |
| `exit 0` | Exit successfully |
| `exit 1` | Exit with a general error status |
| `>&2` | Redirect output to standard error |
| `set -e` | Exit on certain command failures |
| `set -u` | Detect unset variables |
| `set -o pipefail` | Detect pipeline failures |
| `set -euo pipefail` | Enable common strict-mode options |
| `return 0` | Function success status |
| `return 1` | Function non-zero status |

---

## 18. Knowledge Check

Answer these questions in your notebook:

1. What is an exit code?
2. What does exit status `0` represent?
3. What does `$?` store?
4. Why should you save `$?` immediately?
5. What is the purpose of the `exit` command?
6. Explain the difference between stdout and stderr.
7. What does `set -e` do?
8. What does `set -u` do?
9. Why is `set -o pipefail` useful?
10. Why is error handling important in cybersecurity automation?

### Practical Assessment

Successfully create and execute:

1. `exit-status.sh`
2. `validate-input.sh`
3. `file-handler.sh`
4. `logger.sh`
5. `reliable-checker.sh`

Test successful commands, failed commands, invalid input, missing files, and generated log entries.

**Lesson 12 is complete when all five scripts work correctly.**
