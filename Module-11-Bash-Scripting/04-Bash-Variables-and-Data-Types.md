---
---


# Module 11 – Lesson 04: Bash Variables and Data Types

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define variables in Bash.
2. Create and assign values to variables.
3. Access variable values.
4. Understand local and environment variables.
5. Use special Bash variables.
6. Perform basic arithmetic operations.
7. Understand how Bash handles data types.
8. Use variables in practical automation scripts.

---

## 2. What Is a Variable?

A **variable** is a named storage location used to hold a value that a program can access and manipulate.

In Bash scripting, variables allow us to store information and reuse it throughout a script.

For example:

```bash
name="Abbas"
```

Here:
- `name` is the variable name.
- `Abbas` is the value stored in the variable.

To display the value:

```bash
echo "$name"
```

Output:

```text
Abbas
```

### Simple Explanation

Think of a variable as a labelled container.

The label is the variable name, and the information inside the container is its value.

---

## 3. Creating Variables in Bash

The basic syntax is:

```bash
variable_name="value"
```

Examples:

```bash
name="Abbas"
age=21
course="Computer Science"
university="MAAUN"
```

Display the values:

```bash
echo "$name"
echo "$age"
echo "$course"
echo "$university"
```

### Important Rule

Do not put spaces around the assignment operator `=`.

Correct:

```bash
name="Abbas"
```

Incorrect:

```bash
name = "Abbas"
```

Bash will interpret the incorrect example as a command rather than a variable assignment.

---

## 4. Accessing Variable Values

Use the dollar sign `$` to access a variable's value.

Example:

```bash
name="Abbas"

echo "$name"
```

Output:

```text
Abbas
```

You can also use braces:

```bash
echo "${name}"
```

Both forms display the variable's value.

### Why Use Curly Braces?

Curly braces make the variable boundary clear.

Example:

```bash
name="Abbas"

echo "${name} is learning Bash"
```

Output:

```text
Abbas is learning Bash
```

Without braces, Bash may interpret adjacent characters as part of the variable name.

---

## 5. Rules for Naming Variables

Follow these rules:

1. Variable names can contain letters.
2. Variable names can contain numbers, but cannot begin with a number.
3. Underscores are allowed.
4. Spaces are not allowed.
5. Bash variable names are case-sensitive.
6. Avoid special characters in variable names.

Examples:

| Variable | Valid? |
|---|---|
| `name` | Yes |
| `student_name` | Yes |
| `user123` | Yes |
| `123user` | No |
| `student name` | No |
| `UserName` | Yes |
| `username` | Yes |

### Case Sensitivity

```bash
name="Abbas"
Name="Muhammad"

echo "$name"
echo "$Name"
```

These are two different variables.

---

## 6. Understanding Data Types in Bash

Unlike languages such as C, Java, and Python, Bash primarily treats ordinary variables as strings.

Example:

```bash
age="21"
name="Abbas"
```

Both values are stored as text by default.

However, Bash supports arithmetic operations on integer values.

### Common Value Categories

| Value | Example |
|---|---|
| String | `"Abbas"` |
| Integer | `21` |
| Decimal-looking text | `"3.14"` |
| Boolean-style value | `true` or `false` |

**Important:** Bash does not have native general-purpose Boolean or floating-point variable types like many programming languages.

Boolean-style values are usually represented using strings or command exit statuses.

---

## 7. Using Variables in Messages

You can combine variables with text.

Example:

```bash
name="Abbas"
course="Computer Science"

echo "My name is $name"
echo "I study $course"
```

Output:

```text
My name is Abbas
I study Computer Science
```

You can also use braces:

```bash
echo "Welcome, ${name}. Your course is ${course}."
```

---

## 8. Changing Variable Values

Variables can be reassigned.

Example:

```bash
name="Abbas"

echo "$name"

name="Muhammad"

echo "$name"
```

Output:

```text
Abbas
Muhammad
```

The second assignment replaces the previous value.

---

## 9. Local Variables and Environment Variables

There are two important variable concepts in Bash.

### A. Shell Variables

A normal assignment creates a shell variable.

Example:

```bash
student="Abbas"
```

The variable is available in the current shell.

### B. Environment Variables

Environment variables are passed to child processes.

Use `export`:

```bash
export student="Abbas"
```

Now child processes started by this shell can inherit the variable.

### Practical Example

Create a variable:

```bash
course="Computer Science"
```

Display it:

```bash
echo "$course"
```

Start a new Bash process:

```bash
bash -c 'echo "$course"'
```

The new process normally displays nothing because `course` was not exported.

Now export it:

```bash
export course
```

Run again:

```bash
bash -c 'echo "$course"'
```

Output:

```text
Computer Science
```

### Remove a Variable

```bash
unset course
```

This removes the variable from the current shell environment.

---

## 10. Special Bash Variables

Bash provides special variables that contain useful information.

| Variable | Meaning |
|---|---|
| `$0` | Name or path used to invoke the script |
| `$1` | First positional argument |
| `$2` | Second positional argument |
| `$#` | Number of positional arguments |
| `$@` | All positional arguments |
| `$?` | Exit status of the previous command |
| `$$` | Process ID of the current shell |
| `$!` | Process ID of the most recently started background job |

We will study command-line arguments in greater detail in Lesson 06.

### Example

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Number of arguments: $#"
```

Save it as `arguments.sh`.

Execute:

```bash
bash arguments.sh Abbas Computer
```

Expected output will include:

```text
First argument: Abbas
Second argument: Computer
Number of arguments: 2
```

---

## 11. Performing Arithmetic in Bash

Bash supports integer arithmetic.

The most common method is arithmetic expansion:

```bash
$((expression))
```

### Addition

```bash
a=10
b=5

result=$((a + b))

echo "$result"
```

Output:

```text
15
```

### Subtraction

```bash
result=$((a - b))
echo "$result"
```

### Multiplication

```bash
result=$((a * b))
echo "$result"
```

### Division

```bash
result=$((a / b))
echo "$result"
```

Bash integer division discards the fractional part.

For example:

```bash
echo $((10 / 3))
```

Output:

```text
3
```

### Modulus

The modulus operator returns the remainder.

```bash
echo $((10 % 3))
```

Output:

```text
1
```

### Arithmetic Operators

| Operator | Operation |
|---|---|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Integer division |
| `%` | Remainder |
| `**` | Exponentiation |

Example:

```bash
echo $((2 ** 3))
```

Output:

```text
8
```

---

## 12. Using the let Command

Bash also supports arithmetic using `let`.

Example:

```bash
a=10
b=5

let result=a+b

echo "$result"
```

Output:

```text
15
```

However, arithmetic expansion using `$((...))` is generally easier to read and is commonly preferred.

---

## 13. Read-Only Variables

You can prevent a variable from being reassigned using `readonly`.

Example:

```bash
readonly university="MAAUN"

echo "$university"
```

Attempting to change it:

```bash
university="Another University"
```

Bash will report an error.

Read-only variables are useful for values that should remain constant throughout a script.

---

## 14. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson04
cd ~/module11-lab/lesson04
```

### Step 2: Create a Variable Practice Script

```bash
nano variables.sh
```

Enter:

```bash
#!/bin/bash

name="Abbas"
course="Computer Science"
university="MAAUN"
level="300"

echo "========== STUDENT PROFILE =========="

echo "Name: $name"
echo "Course: $course"
echo "University: $university"
echo "Level: $level"

echo "====================================="
```

Save the file.

Make it executable:

```bash
chmod +x variables.sh
```

Execute:

```bash
./variables.sh
```

### Step 3: Create an Arithmetic Script

```bash
nano calculator.sh
```

Enter:

```bash
#!/bin/bash

num1=20
num2=5

addition=$((num1 + num2))
subtraction=$((num1 - num2))
multiplication=$((num1 * num2))
division=$((num1 / num2))
remainder=$((num1 % num2))

echo "========== CALCULATOR =========="

echo "First number: $num1"
echo "Second number: $num2"

echo "Addition: $addition"
echo "Subtraction: $subtraction"
echo "Multiplication: $multiplication"
echo "Division: $division"
echo "Remainder: $remainder"

echo "================================"
```

Make it executable and run:

```bash
chmod +x calculator.sh
./calculator.sh
```

Expected results:

```text
Addition: 25
Subtraction: 15
Multiplication: 100
Division: 4
Remainder: 0
```

---

## 15. Practical Challenge: Create a System Profile Script

Create:

```bash
nano system-profile.sh
```

Enter:

```bash
#!/bin/bash

username=$(whoami)
hostname_value=$(hostname)
current_directory=$(pwd)
current_date=$(date)

system_name="Kali Linux"
purpose="System Information Collection"

echo "========== SYSTEM PROFILE =========="

echo "System: $system_name"
echo "Purpose: $purpose"

echo "Username: $username"
echo "Hostname: $hostname_value"
echo "Directory: $current_directory"
echo "Date: $current_date"

echo "===================================="
echo "PROFILE GENERATED SUCCESSFULLY"
```

Notice this syntax:

```bash
username=$(whoami)
```

It stores the output of a command inside a variable. This is called **command substitution**.

Make the script executable:

```bash
chmod +x system-profile.sh
```

Run:

```bash
./system-profile.sh
```

---

## 16. Lesson Summary

In this lesson, we learned:

- Variables store values.
- Variables are created using assignment.
- `$variable` accesses a variable's value.
- `${variable}` clearly identifies a variable boundary.
- Bash treats ordinary variables primarily as strings.
- Bash supports integer arithmetic.
- `export` makes variables available to child processes.
- `unset` removes variables.
- `readonly` prevents reassignment.
- Special variables provide information about scripts and processes.
- Command substitution stores command output in variables.

### Important Commands and Syntax

| Syntax | Purpose |
|---|---|
| `name="Abbas"` | Create a variable |
| `echo "$name"` | Display a variable |
| `export name` | Export a variable |
| `unset name` | Remove a variable |
| `readonly name="value"` | Create a read-only variable |
| `$((a+b))` | Perform arithmetic |
| `$(command)` | Capture command output |
| `$?` | Read previous command's exit status |
| `$#` | Count positional arguments |

---

## 17. Knowledge Check

Answer these questions in your notebook:

1. What is a variable?
2. How do you create a variable in Bash?
3. Why should spaces not be placed around `=` during assignment?
4. What is the difference between `$name` and `${name}`?
5. Does Bash have native floating-point variables?
6. What does the `export` command do?
7. What does `unset` do?
8. What is the purpose of `readonly`?
9. Explain the difference between `$1` and `$#`.
10. What is command substitution?

### Practical Assessment

Create and execute all three scripts:

1. `variables.sh`
2. `calculator.sh`
3. `system-profile.sh`

Make sure the calculator displays the correct results and the system profile collects information using command substitution.

**Lesson 04 is complete when all three scripts execute successfully.**
       