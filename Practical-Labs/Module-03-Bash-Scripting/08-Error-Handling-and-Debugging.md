# PRACTICAL LABS
# MODULE 03 – BASH SCRIPTING CHALLENGES

## LAB 08: ERROR HANDLING AND DEBUGGING

### 1. Lab Overview

**Objective:** Learn how to identify, understand, prevent, and correct errors in Bash scripts.

A Bash script does not always execute successfully. Errors can occur because:

- A file does not exist.
- A directory cannot be accessed.
- A command is misspelled.
- A variable contains an unexpected value.
- A user enters invalid information.
- A command fails because of insufficient permissions.
- A script contains incorrect syntax.

A good Bash programmer does not simply write scripts that work under ideal conditions. They write scripts that can detect problems and respond appropriately.

### 2. Prepare Your Lab Environment

```bash
mkdir -p ~/practical-labs/module03/lab08
cd ~/practical-labs/module03/lab08
```

Confirm your working directory:

```bash
pwd
```

Create a sample file:

```bash
echo "Linux debugging practice" > sample.txt
```

### 3. Understanding Exit Status

Every Linux command returns an exit status after execution.

The special variable `$?` stores the exit status of the most recently executed command.

Common values:

| Exit status | Meaning |
|---|---|
| `0` | Successful execution |
| `1` | General error |
| `2` | Incorrect command usage or shell-related error |
| Other non-zero values | Different types of failure |

Try:

```bash
ls
echo $?
```

Expected result: `0`, assuming the directory listing succeeds.

Now try:

```bash
ls missing-file.txt
echo $?
```

The command should fail and return a non-zero status.

**Important:** The exact non-zero value depends on the command and error.

### 4. Using if Statements to Detect Errors

Create:

```bash
nano file-check.sh
```

Enter:

```bash
#!/bin/bash

if [[ -f "sample.txt" ]]; then
    echo "File exists."
else
    echo "File does not exist."
fi
```

Execute:

```bash
bash file-check.sh
```

Now rename the sample file:

```bash
mv sample.txt renamed.txt
```

Run the script again:

```bash
bash file-check.sh
```

It should report that the file does not exist.

Restore the original name:

```bash
mv renamed.txt sample.txt
```

This demonstrates how scripts can check conditions before attempting an operation.

### 5. Checking Whether Commands Succeed

Create:

```bash
nano command-check.sh
```

Enter:

```bash
#!/bin/bash

if ls sample.txt; then
    echo "Command executed successfully."
else
    echo "Command failed."
fi
```

Execute:

```bash
bash command-check.sh
```

The script uses the command's exit status to decide which message to display.

### 6. Using `&&` and `||`

These operators help control command execution.

**The `&&` operator**

Runs the second command only when the first command succeeds.

```bash
mkdir test-directory && echo "Directory created."
```

**The `||` operator**

Runs the second command when the first command fails.

```bash
ls missing-file.txt || echo "File could not be found."
```

You can combine both:

```bash
mkdir test-directory && echo "Success" || echo "Operation failed"
```

Be aware that combining commands this way can sometimes obscure which operation failed. For more complex scripts, use explicit `if` statements.

### 7. Using `exit` to Stop a Script

The `exit` command terminates a script and returns an exit status.

Create:

```bash
nano stop-on-error.sh
```

Enter:

```bash
#!/bin/bash

echo "Starting script..."

if [[ ! -f "sample.txt" ]]; then
    echo "Error: sample.txt is missing."
    exit 1
fi

echo "File found."
echo "Script completed."
exit 0
```

Execute:

```bash
bash stop-on-error.sh
```

Check its exit status:

```bash
echo $?
```

Expected result: `0` when the file exists.

Rename the file and test again:

```bash
mv sample.txt renamed.txt
bash stop-on-error.sh
echo $?
```

Expected result: `1`.

Restore it:

```bash
mv renamed.txt sample.txt
```

### 8. Understanding `set -e`, `set -u`, and `set -o pipefail`

Bash provides options that help scripts detect certain failures.

**`set -e`**

Stops a script when an unhandled command failure occurs in many normal execution contexts.

**`set -u`**

Treats references to unset variables as errors.

**`set -o pipefail`**

Makes a pipeline report failure if a command within it fails, rather than relying only on the final command's status.

Together:

```bash
set -euo pipefail
```

This is a useful starting point for many Bash scripts, but it is not a complete error-handling system. Certain constructs, such as conditions and pipelines, have special Bash rules.

Create:

```bash
nano strict-mode.sh
```

Enter:

```bash
#!/bin/bash

set -euo pipefail

name="Linux Student"

echo "Welcome, $name"

echo "Checking sample file..."

if [[ -f "sample.txt" ]]; then
    echo "Sample file exists."
else
    echo "Sample file is missing."
fi

echo "Script completed."
```

Execute:

```bash
bash strict-mode.sh
```

### 9. Detecting Unset Variables

Create:

```bash
nano variable-error.sh
```

Enter:

```bash
#!/bin/bash

set -u

username="student"

echo "Username: $username"

echo "Department: $department"
```

Execute:

```bash
bash variable-error.sh
```

The script should report an unbound variable because `department` was never defined.

Correct it:

```bash
nano variable-error.sh
```

Replace the contents with:

```bash
#!/bin/bash

set -u

username="student"
department="Computer Science"

echo "Username: $username"
echo "Department: $department"
```

Run it again.

### 10. Bash Syntax Checking

You can check Bash syntax without executing the script.

Use:

```bash
bash -n script-name.sh
```

Example:

```bash
bash -n strict-mode.sh
```

No output normally means Bash found no syntax errors.

Create an intentionally incorrect script:

```bash
nano syntax-error.sh
```

Enter:

```bash
#!/bin/bash

if [[ -f "sample.txt" ]
then
    echo "File found."
fi
```

Notice the missing closing bracket.

Check it:

```bash
bash -n syntax-error.sh
```

Bash should identify the syntax problem.

Correct the condition:

```bash
if [[ -f "sample.txt" ]]; then
```

Check it again.

### 11. Debugging with `bash -x`

The `-x` option displays commands as Bash executes them.

Create:

```bash
nano debug-example.sh
```

Enter:

```bash
#!/bin/bash

name="Student"

directory="debug-output"

mkdir -p "$directory"

echo "Welcome, $name"

touch "$directory/report.txt"

echo "Report created."
```

Run normally:

```bash
bash debug-example.sh
```

Now run in debugging mode:

```bash
bash -x debug-example.sh
```

Observe how Bash displays the commands before executing them.

You can also enable tracing inside a script:

```bash
set -x
```

And disable it:

```bash
set +x
```

Use tracing carefully. Debug output may expose passwords, tokens, or other sensitive values if your scripts handle them.

### 12. Handling Invalid User Input

A reliable script should validate user input.

Create:

```bash
nano input-validation.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter your age: " age

if [[ ! "$age" =~ ^[0-9]+$ ]]; then
    echo "Error: Enter a valid whole number."
    exit 1
fi

if (( age < 1 || age > 120 )); then
    echo "Error: Age must be between 1 and 120."
    exit 1
fi

echo "Input accepted."
```

Execute:

```bash
bash input-validation.sh
```

Test different inputs:

- `20`
- `abc`
- `0`
- `150`
- `25.5`

Only valid whole numbers from 1 to 120 should be accepted.

### 13. Creating a Reusable Error Message Function

Functions help avoid repeating error-handling code.

Create:

```bash
nano error-function.sh
```

Enter:

```bash
#!/bin/bash

show_error() {
    echo "ERROR: $1" >&2
}

if [[ ! -f "sample.txt" ]]; then
    show_error "Required file is missing."
    exit 1
fi

echo "All checks passed."
```

Execute:

```bash
bash error-function.sh
```

Understanding:

- `show_error()` defines a function.
- `$1` represents the first argument passed to the function.
- `>&2` sends the message to standard error.
- `exit 1` indicates failure.

### 14. Logging Script Activity

Logging allows administrators to understand what a script did and when it did it.

Create:

```bash
nano activity-logger.sh
```

Enter:

```bash
#!/bin/bash

log_file="activity.log"

log_message() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" >> "$log_file"
}

log_message "Script started."

if [[ -f "sample.txt" ]]; then
    log_message "Sample file found."
else
    log_message "Sample file missing."
fi

log_message "Script finished."

echo "Activity logged."
```

Execute:

```bash
bash activity-logger.sh
```

Read the log:

```bash
cat activity.log
```

The log records events with timestamps.

Never store passwords, private keys, or authentication tokens in ordinary activity logs.

### 15. Practical Exercise 1 – File Validation

Create:

`file-validation.sh`

Requirements:

1. Check whether `sample.txt` exists.
2. Check whether it is readable.
3. Display a success message if both conditions are true.
4. Display an error message otherwise.
5. Return an appropriate exit status.

Useful tests:

```bash
[[ -f "sample.txt" ]]
[[ -r "sample.txt" ]]
```

### 16. Practical Exercise 2 – Safe Directory Creator

Create:

`safe-directory.sh`

Requirements:

1. Ask the user for a directory name.
2. Reject empty input.
3. Reject names containing `/`.
4. Create the directory inside the current lab directory only.
5. Display an appropriate success or error message.

Do not accept absolute paths or arbitrary directory paths.

### 17. Practical Exercise 3 – Debugging Challenge

Create:

`debug-challenge.sh`

Use this intentionally incorrect script:

```bash
#!/bin/bash

set -u

username="student"

if [[ -f "sample.txt" ]
then
    echo "Welcome, $user"
fi
```

Identify and correct:

- The syntax error.
- The incorrect variable name.
- Any missing error-handling logic.

Validate your correction using:

```bash
bash -n debug-challenge.sh
```

Then execute:

```bash
bash -x debug-challenge.sh
```

### 18. Practical Exercise 4 – Error Logging

Create:

`error-logger.sh`

Requirements:

1. Create a function called `log_error`.
2. Accept an error message as an argument.
3. Add the current date and time.
4. Write the message to `errors.log`.
5. Print a confirmation message.

Test it using:

```bash
bash error-logger.sh
```

### 19. Practical Exercise 5 – Reliable System Report

Create:

`reliable-report.sh`

Requirements:

1. Enable appropriate Bash strict-mode options.
2. Define a report filename.
3. Check whether the current directory is writable.
4. Generate a report containing the date, hostname, username, and kernel version.
5. Display an error message if report generation fails.
6. Return an appropriate exit status.

Keep the report inside your Lab 08 directory.

### 20. Final Script Validation

Check your scripts:

```bash
for file in *.sh
do
    echo "Checking $file"
    bash -n "$file"
done
```

Inspect the exit status of your latest command:

```bash
echo $?
```

List your lab files:

```bash
find . -maxdepth 2 -type f
```

### 21. Knowledge Check

1. What is an exit status?
2. What does exit status `0` indicate?
3. What does `$?` contain?
4. What is the purpose of `exit 1`?
5. What does `set -e` do?
6. What does `set -u` detect?
7. What is the purpose of `pipefail`?
8. What does `bash -n` do?
9. What does `bash -x` do?
10. What is standard error?
11. Why should scripts validate user input?
12. Why should sensitive information never be written to logs?

### 22. Lab Completion Checklist

- [ ] Created the Lab 08 directory.
- [ ] Studied exit statuses.
- [ ] Practised command success checks.
- [ ] Used `if`, `&&`, and `||`.
- [ ] Practised script termination with `exit`.
- [ ] Studied Bash strict-mode options.
- [ ] Identified an unset variable.
- [ ] Detected and corrected syntax errors.
- [ ] Used Bash debugging mode.
- [ ] Validated user input.
- [ ] Created reusable error-handling functions.
- [ ] Generated activity logs.
- [ ] Completed all five practical exercises.
- [ ] Validated Bash syntax.

### 23. Expected Outcome

After completing this lab, you should be able to:

- Understand command exit statuses.
- Detect failed operations.
- Validate files and user input.
- Stop scripts when necessary.
- Apply Bash strict-mode options appropriately.
- Identify syntax errors.
- Debug scripts using execution tracing.
- Record script activity.
- Build more reliable automation tools.

**Next Lab:** Security Automation Scripts.