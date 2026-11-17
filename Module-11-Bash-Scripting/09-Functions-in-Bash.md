---
---


# Module 11 – Lesson 09: Functions in Bash

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what functions are.
2. Create and call Bash functions.
3. Pass arguments to functions.
4. Return status codes from functions.
5. Use local and global variables.
6. Understand function scope.
7. Organize scripts using reusable functions.
8. Build a simple system information toolkit.

---

## 2. What Is a Function?

A function is a reusable block of code designed to perform a specific task.

Instead of writing the same commands repeatedly, you can put them inside a function and call that function whenever needed.

### Real-World Example

Imagine building a Linux administration toolkit.

You may need to:
- Display the current username.
- Display the hostname.
- Display the current date.
- Check available disk space.

Instead of writing these commands repeatedly, you can create separate functions for each task.

### Benefits of Functions

- Code reuse.
- Better organization.
- Easier debugging.
- Reduced repetition.
- Easier maintenance.
- Improved readability.

---

## 3. Creating a Function

### Basic Syntax

```bash
function_name() {
    commands
}
```

You can also use:

```bash
function function_name {
    commands
}
```

Both forms are valid in Bash.

### Example

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

- `greet()` defines the function.
- `{ }` contain its commands.
- `greet` calls the function.

**Important:** Defining a function does not execute it. You must call it.

---

## 4. Calling Multiple Functions

You can define several functions in one script.

Example:

```bash
#!/bin/bash

greet() {
    echo "Welcome!"
}

show_date() {
    date
}

show_user() {
    whoami
}

greet
show_date
show_user
```

Each function performs a different task.

---

## 5. Passing Arguments to Functions

Functions can receive arguments just like ordinary Bash scripts.

Inside a function:

| Variable | Meaning |
|---|---|
| `$1` | First argument |
| `$2` | Second argument |
| `$3` | Third argument |
| `$#` | Number of arguments |
| `$@` | All arguments |

### Example

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

### Example with Two Arguments

```bash
#!/bin/bash

student_info() {
    echo "Name: $1"
    echo "Level: $2"
}

student_info "Abbas" "Level 3"
```

Output:

```text
Name: Abbas
Level: Level 3
```

### Example with Multiple Arguments

```bash
#!/bin/bash

show_arguments() {
    echo "Total arguments: $#"

    for argument in "$@"
    do
        echo "Argument: $argument"
    done
}

show_arguments "Linux" "Bash" "Cybersecurity"
```

---

## 6. Local and Global Variables

Bash supports variables with different scopes.

### A. Global Variables

A global variable can be accessed throughout the script, including inside functions.

Example:

```bash
#!/bin/bash

name="Abbas"

show_name() {
    echo "Name: $name"
}

show_name
```

### B. Local Variables

A local variable is declared using `local` inside a function.

It is available within that function and its nested function calls.

Example:

```bash
#!/bin/bash

show_student() {
    local name="Abbas"

    echo "Inside function: $name"
}

show_student
```

### Example: Local Versus Global

```bash
#!/bin/bash

name="Global Name"

show_name() {
    local name="Local Name"

    echo "Inside function: $name"
}

show_name

echo "Outside function: $name"
```

Output:

```text
Inside function: Local Name
Outside function: Global Name
```

The local variable does not overwrite the global variable.

**Best practice:** Use `local` for variables that are only needed inside a function.

---

## 7. Returning Values from Functions

Bash functions have a special `return` command.

However, `return` is used to return an exit status, not an ordinary string or general-purpose value.

By convention:
- `0` means success.
- A non-zero value means failure or another status.

### Example

```bash
#!/bin/bash

check_number() {
    if [[ $1 -gt 0 ]]; then
        return 0
    else
        return 1
    fi
}

check_number 10

if [[ $? -eq 0 ]]; then
    echo "The number is positive."
else
    echo "The number is not positive."
fi
```

### Understanding `$?`

`$?` contains the exit status of the most recently executed command.

Be careful: another command can overwrite it.

A cleaner approach is to use the function directly in an `if` statement:

```bash
if check_number 10; then
    echo "The number is positive."
else
    echo "The number is not positive."
fi
```

---

## 8. Returning Text Using Command Substitution

If you want a function to produce text or a calculated value, use `echo` or `printf` and capture its output.

### Example

```bash
#!/bin/bash

get_username() {
    whoami
}

current_user=$(get_username)

echo "Current user: $current_user"
```

### Example: Calculator Function

```bash
#!/bin/bash

add_numbers() {
    local result=$(( $1 + $2 ))
    echo "$result"
}

answer=$(add_numbers 20 30)

echo "Result: $answer"
```

Output:

```text
Result: 50
```

**Important:** Command substitution captures standard output. Avoid printing unrelated messages inside a function when you intend to capture a clean result.

---

## 9. Functions with Conditional Statements

Functions can contain conditions.

Example:

```bash
#!/bin/bash

check_file() {
    local target="$1"

    if [[ -f "$target" ]]; then
        echo "Regular file exists."
    elif [[ -d "$target" ]]; then
        echo "Directory exists."
    else
        echo "Path does not exist or is another type."
    fi
}

check_file "$HOME"
```

This function checks a supplied path and reports its type.

---

## 10. Functions with Loops

Functions can also contain loops.

Example:

```bash
#!/bin/bash

count_numbers() {
    local limit="$1"

    for ((number=1; number<=limit; number++))
    do
        echo "$number"
    done
}

count_numbers 5
```

Output:

```text
1
2
3
4
5
```

This makes repetitive operations reusable.

---

## 11. Organizing a Script with Functions

A well-organized script often contains:

1. A shebang.
2. Function definitions.
3. Main program logic.
4. Function calls.

Example:

```bash
#!/bin/bash

show_header() {
    echo "=============================="
    echo "       LINUX TOOLKIT"
    echo "=============================="
}

show_user() {
    echo "Username: $(whoami)"
}

show_host() {
    echo "Hostname: $(hostname)"
}

show_date() {
    echo "Date: $(date)"
}

main() {
    show_header
    show_user
    show_host
    show_date
}

main
```

This structure makes the script easier to understand and maintain.

---

## 12. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson09
cd ~/module11-lab/lesson09
```

### Step 2: Create a Greeting Function

```bash
nano greeting.sh
```

Enter:

```bash
#!/bin/bash

greet() {
    echo "Welcome to Linux administration."
}

greet
```

Execute:

```bash
chmod +x greeting.sh
./greeting.sh
```

### Step 3: Create a Student Information Function

```bash
nano student-info.sh
```

Enter:

```bash
#!/bin/bash

student_info() {
    local name="$1"
    local level="$2"

    echo "Student Name: $name"
    echo "Academic Level: $level"
}

student_info "Abbas" "Level 3"
```

Execute:

```bash
chmod +x student-info.sh
./student-info.sh
```

### Step 4: Create a Calculator Function

```bash
nano calculator.sh
```

Enter:

```bash
#!/bin/bash

add() {
    local result=$(( $1 + $2 ))
    echo "$result"
}

subtract() {
    local result=$(( $1 - $2 ))
    echo "$result"
}

multiply() {
    local result=$(( $1 * $2 ))
    echo "$result"
}

echo "Addition: $(add 20 10)"
echo "Subtraction: $(subtract 20 10)"
echo "Multiplication: $(multiply 20 10)"
```

Execute:

```bash
chmod +x calculator.sh
./calculator.sh
```

Expected output:

```text
Addition: 30
Subtraction: 10
Multiplication: 200
```

### Step 5: Create a File Inspection Function

```bash
nano file-inspector.sh
```

Enter:

```bash
#!/bin/bash

inspect_file() {
    local target="$1"

    if [[ -f "$target" ]]; then
        echo "Regular file detected."

        if [[ -r "$target" ]]; then
            echo "Readable: Yes"
        else
            echo "Readable: No"
        fi

        if [[ -w "$target" ]]; then
            echo "Writable: Yes"
        else
            echo "Writable: No"
        fi

    elif [[ -d "$target" ]]; then
        echo "Directory detected."
    else
        echo "Path not found or unsupported."
        return 1
    fi
}

inspect_file "$1"
```

Execute:

```bash
chmod +x file-inspector.sh
./file-inspector.sh /etc/passwd
```

Try another path:

```bash
./file-inspector.sh "$HOME"
```

The script only inspects the paths you provide; it does not change permissions or modify files.

---

## 13. Practical Cybersecurity Project: System Information Toolkit

Create a script that uses several functions to display basic system information.

```bash
nano system-toolkit.sh
```

Enter:

```bash
#!/bin/bash

show_header() {
    echo "===================================="
    echo "       SYSTEM INFORMATION TOOL"
    echo "===================================="
}

show_user() {
    echo "Current User: $(whoami)"
}

show_hostname() {
    echo "Hostname: $(hostname)"
}

show_kernel() {
    echo "Kernel: $(uname -r)"
}

show_uptime() {
    echo "System Uptime: $(uptime -p)"
}

show_disk() {
    echo "Disk Usage:"
    df -h "$HOME"
}

show_memory() {
    echo "Memory Information:"
    free -h
}

main() {
    show_header
    show_user
    show_hostname
    show_kernel
    show_uptime
    show_disk
    show_memory

    echo "===================================="
    echo "Information collection completed."
}

main
```

Make it executable:

```bash
chmod +x system-toolkit.sh
```

Run:

```bash
./system-toolkit.sh
```

### Cybersecurity Application

This project demonstrates how functions can organize a basic system inventory tool.

Defensive security teams use similar techniques to collect information about:
- User accounts.
- Hostnames.
- Kernel versions.
- System uptime.
- Disk usage.
- Memory usage.

This is an introductory inventory script, not a complete security audit.

---

## 14. Common Function Errors

| Error | Explanation | Solution |
|---|---|---|
| Function not called | Function was defined but never executed | Call its name |
| Missing braces | Function body is incomplete | Check `{` and `}` |
| Incorrect argument | Wrong positional parameter used | Check `$1`, `$2`, etc. |
| Global variable overwritten | Variable was not declared local | Use `local` |
| Wrong return value | Text was returned using `return` | Use output capture |
| Function defined too late | Called before its definition in normal script flow | Define it before calling it |

### Debugging

Check syntax:

```bash
bash -n system-toolkit.sh
```

Trace execution:

```bash
bash -x system-toolkit.sh
```

---

## 15. Lesson Summary

In this lesson, we learned:

- Functions are reusable blocks of code.
- Functions are defined using a name followed by parentheses and braces.
- Functions execute when called.
- Functions can receive arguments.
- `$1`, `$2`, and `$@` access function arguments.
- `local` creates function-local variables.
- `return` provides an exit status.
- Command substitution captures text output.
- Functions can contain loops and conditions.
- Functions make large scripts easier to organize.

### Important Syntax

| Syntax | Purpose |
|---|---|
| `function_name() { }` | Defines a function |
| `function_name` | Calls a function |
| `$1` | First function argument |
| `$2` | Second function argument |
| `$@` | All function arguments |
| `$#` | Number of arguments |
| `local` | Declares a local variable |
| `return 0` | Returns success status |
| `return 1` | Returns a non-zero status |
| `$(function_name)` | Captures function output |

---

## 16. Knowledge Check

Answer these questions in your notebook:

1. What is a function in Bash?
2. Why are functions useful?
3. How do you define a function?
4. How do you call a function?
5. How can you pass arguments to a function?
6. What is the difference between local and global variables?
7. What does `return 0` mean?
8. Why is `echo` useful when capturing a function's output?
9. Can a function contain loops and conditional statements?
10. How can functions improve a cybersecurity automation toolkit?

### Practical Assessment

Successfully create and execute:

1. `greeting.sh`
2. `student-info.sh`
3. `calculator.sh`
4. `file-inspector.sh`
5. `system-toolkit.sh`

**Lesson 09 is complete when all five scripts work correctly.**
