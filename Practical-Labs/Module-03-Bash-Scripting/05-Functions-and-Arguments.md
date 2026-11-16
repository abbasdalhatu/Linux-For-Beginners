# PRACTICAL LABS
# MODULE 03 – BASH SCRIPTING CHALLENGES

## LAB 05: FUNCTIONS AND ARGUMENTS

### 1. Lab Overview

**Objective:** Learn how to create reusable Bash functions and pass arguments to scripts and functions.

Imagine you have a script that displays system information.

You might need to display the same information several times.

Instead of writing the same commands repeatedly, you can create a function and call it whenever necessary.

Functions make scripts:

- Easier to organize.
- Easier to maintain.
- More reusable.
- Less repetitive.
- Easier to troubleshoot.

Arguments allow scripts and functions to receive information when they are called.

### 2. What Is a Function?

A function is a named block of commands that performs a particular task.

Example:

```bash
#!/bin/bash

greet() {
    echo "Welcome to Bash scripting!"
}

greet
```

Output:

```text
Welcome to Bash scripting!
```

Explanation:

- `greet` is the function name.
- `{ }` contain the function commands.
- `greet` at the bottom calls the function.

A function does not normally execute just because it has been defined. You must call it.

### 3. Preparing Your Lab

Create your working directory:

```bash
mkdir -p ~/practical-labs/module03/lab05
cd ~/practical-labs/module03/lab05
```

Confirm:

```bash
pwd
```

### 4. Creating a Function

Create:

```bash
nano functions.sh
```

Enter:

```bash
#!/bin/bash

show_welcome() {
    echo "=============================="
    echo "  LINUX ADMINISTRATION LAB"
    echo "=============================="
}

show_welcome
```

Execute:

```bash
bash functions.sh
```

The function displays the welcome banner.

### 5. Creating Multiple Functions

A script can contain multiple functions.

Example:

```bash
#!/bin/bash

show_user() {
    echo "Current User:"
    whoami
}

show_directory() {
    echo "Current Directory:"
    pwd
}

show_date() {
    echo "Current Date:"
    date
}

show_user
show_directory
show_date
```

Each function performs a different task.

You can call them in any required order after they have been defined.

### 6. Passing Arguments to a Function

Functions can receive information through arguments.

Example:

```bash
#!/bin/bash

greet_user() {
    echo "Hello, $1!"
}

greet_user "Abbas"
```

Output:

```text
Hello, Abbas!
```

Inside the function:

- `$1` represents the first argument.
- `$2` represents the second argument.
- `$3` represents the third argument.

Example:

```bash
#!/bin/bash

student_info() {
    echo "Name: $1"
    echo "Department: $2"
    echo "Level: $3"
}

student_info "Abbas" "Computer Science" "Level 3"
```

Output:

```text
Name: Abbas
Department: Computer Science
Level: Level 3
```

### 7. Understanding Script Arguments

Arguments can also be passed directly to a Bash script when executing it.

Create:

```bash
nano script-arguments.sh
```

Enter:

```bash
#!/bin/bash

echo "Script Name: $0"
echo "First Argument: $1"
echo "Second Argument: $2"
echo "Third Argument: $3"
```

Execute:

```bash
bash script-arguments.sh Abbas Cybersecurity Level3
```

Output:

```text
Script Name: script-arguments.sh
First Argument: Abbas
Second Argument: Cybersecurity
Third Argument: Level3
```

Important special parameters:

| Parameter | Meaning |
|---|---|
| `$0` | Script name or invocation path |
| `$1` | First positional argument |
| `$2` | Second positional argument |
| `$3` | Third positional argument |
| `$#` | Number of positional arguments |
| `$@` | All positional arguments |
| `$*` | All positional arguments as a combined expansion |
| `$?` | Exit status of the previous command |

### 8. Understanding "$@" and "$*"

Both can represent all positional arguments, but they behave differently when quoted.

**Using "$@"**

```bash
#!/bin/bash

for item in "$@"
do
    echo "Argument: $item"
done
```

Execute:

```bash
bash arguments-loop.sh "Linux Security" Bash Networking
```

Output:

```text
Argument: Linux Security
Argument: Bash
Argument: Networking
```

Each argument remains a separate item, including arguments containing spaces.

**Using "$*"**

When quoted as `"$*"`, all positional arguments are combined into one string, separated by the first character of `IFS` (normally a space).

For most scripts that need to process arguments individually, prefer `"$@"`.

### 9. Checking the Number of Arguments

You can use `$#` to check how many arguments were provided.

Create:

```bash
nano argument-count.sh
```

Enter:

```bash
#!/bin/bash

echo "Number of arguments: $#"

if [[ "$#" -eq 0 ]]; then
    echo "No arguments were supplied."
else
    echo "Arguments were supplied."
fi
```

Execute:

```bash
bash argument-count.sh
```

Then try:

```bash
bash argument-count.sh Linux Bash Security
```

Compare the results.

### 10. Returning Values from Functions

Bash functions use `return` to provide an exit status.

A return status is an integer from 0 to 255.

Conventionally:

- `0` means success.
- A non-zero value indicates failure or another status.

Example:

```bash
#!/bin/bash

check_file() {
    if [[ -f "$1" ]]; then
        return 0
    else
        return 1
    fi
}

check_file "report.txt"

if [[ "$?" -eq 0 ]]; then
    echo "File exists."
else
    echo "File does not exist."
fi
```

Note: `$?` must be checked immediately after the function call because another command can replace its value.

A clearer approach is:

```bash
if check_file "report.txt"; then
    echo "File exists."
else
    echo "File does not exist."
fi
```

Bash functions can also send text or calculated values to standard output.

### 11. Capturing Function Output

Example:

```bash
#!/bin/bash

get_username() {
    whoami
}

current_user=$(get_username)

echo "Current user: $current_user"
```

The function produces output, and command substitution stores it in a variable.

This is useful when a function calculates or retrieves information.

### 12. Local Variables

Variables created inside functions are not automatically private.

Use `local` to limit a variable's scope to its function.

Example:

```bash
#!/bin/bash

show_message() {
    local message="This is a local variable."

    echo "$message"
}

show_message
```

The `local` keyword helps prevent accidental interference with variables elsewhere in the script.

### 13. Creating a Reusable System Information Function

Create:

```bash
nano system-functions.sh
```

Enter:

```bash
#!/bin/bash

show_hostname() {
    echo "Hostname: $(hostname)"
}

show_user() {
    echo "Current User: $(whoami)"
}

show_kernel() {
    echo "Kernel: $(uname -r)"
}

show_uptime() {
    echo "Uptime: $(uptime -p)"
}

show_system_report() {
    echo "========== SYSTEM REPORT =========="

    show_hostname
    show_user
    show_kernel
    show_uptime

    echo "==================================="
}

show_system_report
```

Execute:

```bash
bash system-functions.sh
```

Notice how the main function calls smaller functions.

This is an example of modular programming.

### 14. Input Validation for Arguments

A reliable script should check whether required arguments were provided.

Create:

```bash
nano validated-arguments.sh
```

Enter:

```bash
#!/bin/bash

if [[ "$#" -ne 1 ]]; then
    echo "Usage: bash validated-arguments.sh <filename>"
    exit 1
fi

filename="$1"

if [[ -f "$filename" ]]; then
    echo "File exists: $filename"
else
    echo "File not found: $filename"
    exit 1
fi
```

Execute:

```bash
bash validated-arguments.sh
```

Then provide a filename:

```bash
bash validated-arguments.sh functions.sh
```

The script requires exactly one argument.

It checks the file without executing or modifying it.

### 15. Practical Exercise 1 – Welcome Function

Create:

```text
welcome-function.sh
```

Requirements:

- Create a function called `welcome`.
- Display a welcome message.
- Call the function three times.

### 16. Practical Exercise 2 – Student Information Function

Create:

```text
student-function.sh
```

Requirements:

1. Create a function called `student_info`.
2. Accept three arguments:
   - Name.
   - Department.
   - Academic level.
3. Display the supplied information.

Example execution:

```bash
bash student-function.sh Abbas "Computer Science" "Level 3"
```

### 17. Practical Exercise 3 – Argument Inspector

Create:

```text
argument-inspector.sh
```

Requirements:

- Display the script name.
- Display the number of arguments.
- Display each argument separately.
- Use a loop and `"$@"`.

### 18. Practical Exercise 4 – File Verification Function

Create:

```text
file-verification.sh
```

Requirements:

1. Create a function called `check_file`.
2. Accept a filename as an argument.
3. Check whether the file exists.
4. Return success when it exists.
5. Return failure when it does not exist.
6. Display an appropriate message.

Test using files inside your lab directory.

### 19. Practical Exercise 5 – System Report Generator

Create:

```text
system-report.sh
```

Requirements:

Create separate functions to display:

- Hostname.
- Current username.
- Current directory.
- Kernel version.
- System uptime.

Create a final function called `generate_report` that calls all the other functions.

### 20. Syntax Validation

Validate your scripts:

```bash
bash -n functions.sh
bash -n script-arguments.sh
bash -n argument-count.sh
bash -n system-functions.sh
bash -n validated-arguments.sh
```

Inspect your directory:

```bash
ls -l
```

### 21. Cybersecurity Application

Functions and arguments are important when developing defensive security tools.

For example, an analyst may create a reusable function to check whether a particular file exists.

```bash
#!/bin/bash

check_security_file() {
    local filename="$1"

    if [[ -f "$filename" ]]; then
        echo "[FOUND] $filename"
        return 0
    else
        echo "[MISSING] $filename"
        return 1
    fi
}

check_security_file "./security-report.txt"
```

This script checks a file in the current directory.

It does not modify the file or perform any unauthorized activity.

Functions can help organize larger defensive tools into separate components, such as:

- File verification.
- Permission inspection.
- Log analysis.
- System information collection.
- Report generation.

### 22. Knowledge Check

1. What is a Bash function?
2. Why are functions useful?
3. How do you define a function?
4. What does `$1` represent?
5. What does `$#` represent?
6. What is the difference between `"$@"` and `"$*"`?
7. What does `$0` represent?
8. What is the purpose of `return 0`?
9. What does the `local` keyword do?
10. How can functions improve cybersecurity automation?

### 23. Lab Completion Checklist

- [ ] Created the Lab 05 directory.
- [ ] Created and called Bash functions.
- [ ] Created multiple functions in one script.
- [ ] Passed arguments to functions.
- [ ] Passed arguments to scripts.
- [ ] Used positional parameters.
- [ ] Practised `$#`, `"$@"`, and `"$*"`.
- [ ] Used return statuses.
- [ ] Captured function output.
- [ ] Practised local variables.
- [ ] Completed all five practical exercises.
- [ ] Checked Bash syntax.

### 24. Expected Outcome

By completing this lab, you should understand how to:

- Define reusable Bash functions.
- Pass arguments to scripts and functions.
- Use positional parameters.
- Process multiple arguments.
- Return and check exit statuses.
- Capture function output.
- Organize larger scripts into reusable components.

**Next Lab:** File Processing and Text Manipulation.