---
---


# Module 11 – Lesson 06: Command-Line Arguments

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain command-line arguments.
2. Pass information to Bash scripts when executing them.
3. Understand positional parameters.
4. Use `$1`, `$2`, and other special parameters.
5. Count arguments using `$#`.
6. Access all arguments using `$@` and `$*`.
7. Validate command-line arguments.
8. Build practical scripts that accept external input.

---

## 2. What Are Command-Line Arguments?

Command-line arguments are values supplied to a program when it is executed.

They allow users to provide information without the program asking questions interactively.

For example:

```bash
bash welcome.sh Abbas
```

Here:
- `bash` executes the script.
- `welcome.sh` is the script filename.
- `Abbas` is an argument passed to the script.

The script can access this value using `$1`.

### Simple Explanation

Imagine a script that welcomes users.

Without arguments, it might ask:

```text
Enter your name:
```

With command-line arguments, you can provide the name immediately:

```bash
./welcome.sh Abbas
```

The script receives the name automatically.

---

## 3. Understanding Positional Parameters

Bash uses special variables called positional parameters to access arguments.

| Parameter | Meaning |
|---|---|
| `$0` | Name or path used to execute the script |
| `$1` | First argument |
| `$2` | Second argument |
| `$3` | Third argument |
| `$4` | Fourth argument |
| `$#` | Number of positional arguments |
| `$@` | All positional arguments as separate words |
| `$*` | All positional arguments combined into one word when quoted |

### Example

Create:

```bash
nano arguments.sh
```

Enter:

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Third argument: $3"
```

Make it executable:

```bash
chmod +x arguments.sh
```

Execute:

```bash
./arguments.sh Abbas Computer Science
```

Expected output:

```text
Script name: ./arguments.sh
First argument: Abbas
Second argument: Computer
Third argument: Science
```

Notice that Bash separates arguments using spaces unless quoting groups them together.

---

## 4. Understanding $0

The `$0` parameter represents the name or path used to invoke the script.

Example:

```bash
#!/bin/bash

echo "You executed: $0"
```

Run:

```bash
./arguments.sh
```

The script may display:

```text
You executed: ./arguments.sh
```

If you execute the same script using:

```bash
bash arguments.sh
```

The displayed script name will normally be:

```text
arguments.sh
```

The exact value depends on how the script is invoked.

---

## 5. Understanding $1, $2, and $3

These parameters represent individual arguments.

Example:

```bash
#!/bin/bash

name="$1"
course="$2"
level="$3"

echo "Name: $name"
echo "Course: $course"
echo "Level: $level"
```

Execute:

```bash
bash student.sh Abbas "Computer Science" 300
```

Output:

```text
Name: Abbas
Course: Computer Science
Level: 300
```

### Important: Quotation Marks

Notice:

```bash
"Computer Science"
```

The quotation marks keep the words together as one argument.

Without quotes:

```bash
bash student.sh Abbas Computer Science 300
```

Bash interprets `Computer` as `$2`, `Science` as `$3`, and `300` as `$4`.

---

## 6. Understanding $# – Counting Arguments

The `$#` parameter tells you how many positional arguments were supplied.

Example:

```bash
#!/bin/bash

echo "Number of arguments: $#"
```

Execute:

```bash
bash arguments.sh Abbas Computer 300
```

Output:

```text
Number of arguments: 3
```

This is useful when checking whether a user supplied all required information.

---

## 7. Understanding $@ and $*

Both `$@` and `$*` refer to all positional arguments, but they behave differently when quoted.

### Using $@

When quoted as `"$@"`, each original argument remains a separate word.

Example:

```bash
#!/bin/bash

for item in "$@"
do
    echo "Argument: $item"
done
```

Execute:

```bash
bash list.sh Abbas "Computer Science" 300
```

Output:

```text
Argument: Abbas
Argument: Computer Science
Argument: 300
```

### Using $*

When quoted as `"$*"`, all positional arguments are combined into a single word, separated by the first character of the `IFS` variable (normally a space).

Example:

```bash
#!/bin/bash

printf '<%s>\n' "$*"
```

Execute:

```bash
bash list.sh Abbas "Computer Science" 300
```

Output:

```text
<Abbas Computer Science 300>
```

### Important Difference

| Syntax | Behaviour |
|---|---|
| `"$@"` | Preserves each argument separately |
| `"$*"` | Combines all arguments into one word |
| `$@` | Expands arguments without preserving each one safely in all contexts |
| `$*` | Expands arguments as a combined word list |

**Professional rule:** Use `"$@"` when passing all received arguments to another command or function.

---

## 8. Using Command-Line Arguments in Calculations

Arguments can supply numbers to a script.

Example:

```bash
#!/bin/bash

num1="$1"
num2="$2"

result=$((num1 + num2))

echo "First number: $num1"
echo "Second number: $num2"
echo "Sum: $result"
```

Execute:

```bash
bash calculator.sh 20 10
```

Output:

```text
First number: 20
Second number: 10
Sum: 30
```

**Important:** Validate numeric input before using it in arithmetic. Bash arithmetic should not be treated as a safe parser for arbitrary user input.

---

## 9. Checking Whether Arguments Were Supplied

A script should check whether required arguments are present.

Example:

```bash
#!/bin/bash

if [[ $# -eq 0 ]]; then
    echo "Error: No arguments supplied."
    echo "Usage: $0 <name>"
    exit 1
fi

echo "Welcome, $1"
```

Execute without arguments:

```bash
bash welcome.sh
```

Expected output:

```text
Error: No arguments supplied.
Usage: bash welcome.sh <name>
```

Execute with an argument:

```bash
bash welcome.sh Abbas
```

Expected output:

```text
Welcome, Abbas
```

### Understanding exit

```bash
exit 1
```

Stops the script and returns a non-zero status to indicate failure.

We will explore exit codes and error handling in Lesson 12.

---

## 10. Validating the Number of Arguments

Suppose a script requires exactly two arguments.

Example:

```bash
#!/bin/bash

if [[ $# -ne 2 ]]; then
    echo "Usage: $0 <first_number> <second_number>"
    exit 1
fi

echo "First number: $1"
echo "Second number: $2"
```

The `-ne` operator means "not equal".

Run with two arguments:

```bash
bash check.sh 10 20
```

Run with one argument:

```bash
bash check.sh 10
```

The second execution displays the usage instructions.

---

## 11. Using Shift

The `shift` command moves positional parameters to the left.

Example:

```bash
#!/bin/bash

echo "First: $1"
shift
echo "New first: $1"
```

Execute:

```bash
bash shift.sh Abbas Computer
```

Output:

```text
First: Abbas
New first: Computer
```

Originally:
- `$1` = Abbas
- `$2` = Computer

After `shift`:
- `$1` = Computer
- The original second argument becomes the first.

`shift` is useful when processing multiple arguments one at a time.

---

## 12. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson06
cd ~/module11-lab/lesson06
```

### Step 2: Create an Argument Information Script

```bash
nano argument-info.sh
```

Enter:

```bash
#!/bin/bash

echo "========== ARGUMENT INFORMATION =========="

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Third argument: $3"

echo "Total arguments: $#"

echo "All arguments:"
printf 'Argument: %s\n' "$@"

echo "==========================================="
```

Save and execute:

```bash
chmod +x argument-info.sh
./argument-info.sh Abbas "Computer Science" 300
```

### Step 3: Test Different Argument Counts

Try:

```bash
./argument-info.sh Abbas
```

Then:

```bash
./argument-info.sh Abbas Computer 300 Extra
```

Observe how the output changes.

---

## 13. Practical Challenge: Create a Command-Line Calculator

Create:

```bash
nano advanced-calculator.sh
```

Enter:

```bash
#!/bin/bash

if [[ $# -ne 3 ]]; then
    echo "Usage: $0 <number1> <operator> <number2>"
    echo "Example: $0 20 + 10"
    exit 1
fi

num1="$1"
operator="$2"
num2="$3"

if [[ ! "$num1" =~ ^-?[0-9]+$ || ! "$num2" =~ ^-?[0-9]+$ ]]; then
    echo "Error: Please provide valid integers."
    exit 1
fi

case "$operator" in
    +)
        result=$((num1 + num2))
        ;;
    -)
        result=$((num1 - num2))
        ;;
    x|\*)
        result=$((num1 * num2))
        ;;
    /)
        if (( num2 == 0 )); then
            echo "Error: Division by zero is not allowed."
            exit 1
        fi
        result=$((num1 / num2))
        ;;
    *)
        echo "Error: Unsupported operator."
        exit 1
        ;;
esac

echo "========== CALCULATOR =========="
echo "First number: $num1"
echo "Operator: $operator"
echo "Second number: $num2"
echo "Result: $result"
echo "================================"
```

Make it executable:

```bash
chmod +x advanced-calculator.sh
```

Test addition:

```bash
./advanced-calculator.sh 20 + 10
```

Test subtraction:

```bash
./advanced-calculator.sh 20 - 10
```

Test multiplication:

```bash
./advanced-calculator.sh 20 x 10
```

Test division:

```bash
./advanced-calculator.sh 20 / 5
```

Test division by zero:

```bash
./advanced-calculator.sh 20 / 0
```

The script should reject division by zero.

---

## 14. Cybersecurity Application

Command-line arguments are useful when building security automation tools.

Imagine a defensive log-analysis tool:

```bash
./log-checker.sh /var/log/auth.log
```

The script receives the log file path through `$1`.

Another example:

```bash
./network-report.sh eth0
```

The script receives a network interface name.

However, never assume command-line arguments are trustworthy.

Security best practices include:

- Validate argument counts.
- Validate expected input formats.
- Quote variable expansions.
- Avoid executing supplied input as code.
- Check file paths before processing them.
- Use only authorized files and systems.

---

## 15. Lesson Summary

In this lesson, we learned:

1. Command-line arguments provide information when a script starts.
2. `$0` identifies how the script was invoked.
3. `$1`, `$2`, and `$3` access individual arguments.
4. `$#` counts positional arguments.
5. `"$@"` preserves arguments individually.
6. `"$*"` combines arguments into one word.
7. `shift` moves positional parameters.
8. Argument validation helps prevent incorrect input.
9. Arguments can be used in calculations and automation tools.

### Important Commands and Variables

| Syntax | Purpose |
|---|---|
| `$0` | Script invocation name |
| `$1` | First argument |
| `$2` | Second argument |
| `$#` | Argument count |
| `"$@"` | All arguments separately |
| `"$*"` | All arguments combined |
| `shift` | Move positional parameters |
| `exit 1` | Stop with a failure status |

---

## 16. Knowledge Check

Answer these questions in your notebook:

1. What are command-line arguments?
2. What is the purpose of `$0`?
3. Explain the difference between `$1` and `$2`.
4. What does `$#` represent?
5. What is the difference between `"$@"` and `"$*"`?
6. Why should arguments be validated?
7. What does the `shift` command do?
8. How can a script check whether exactly two arguments were supplied?
9. Why should variable expansions generally be quoted?
10. Give two examples of command-line arguments in cybersecurity tools.

### Practical Assessment

Successfully create and execute:

1. `argument-info.sh`
2. `advanced-calculator.sh`

Test multiple argument combinations, including missing arguments and division by zero.

**Lesson 06 is complete when both scripts work correctly.**
