---
---


# Module 11 – Lesson 08: Loops in Bash

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain loops and their importance in programming.
2. Understand `for` loops.
3. Understand `while` loops.
4. Understand `until` loops.
5. Use `break` and `continue`.
6. Create nested loops.
7. Use loops with files and command output.
8. Build simple automation scripts using loops.

---

## 2. What Are Loops?

A loop is a programming structure that repeatedly executes a set of instructions while a condition is satisfied or for a specified number of iterations.

### Real-World Example

Imagine you need to display the numbers from 1 to 100.

Without a loop, you might write 100 separate print commands.

With a loop, you write one instruction that repeats automatically.

### Why Are Loops Important?

Loops help programmers:

- Automate repetitive tasks.
- Process multiple files.
- Monitor system information.
- Analyze log entries.
- Perform repeated calculations.
- Build efficient cybersecurity tools.

---

## 3. The for Loop

A `for` loop executes commands for each item in a list or sequence.

### Basic Syntax

```bash
for variable in list
do
    commands
done
```

Explanation:

- `for` starts the loop.
- `variable` stores the current item.
- `in` introduces the list.
- `do` starts the commands.
- `done` ends the loop.

### Example 1: Display Five Numbers

```bash
#!/bin/bash

for number in 1 2 3 4 5
do
    echo "Number: $number"
done
```

Output:

```text
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

### Example 2: Using a Range

Bash supports range expansion.

```bash
#!/bin/bash

for number in {1..10}
do
    echo "$number"
done
```

This displays numbers from 1 to 10.

### Example 3: Counting by Two

```bash
#!/bin/bash

for number in {2..20..2}
do
    echo "$number"
done
```

Output:

```text
2
4
6
8
10
12
14
16
18
20
```

The final number in the range specifies the step.

### Example 4: C-Style for Loop

Bash also supports a C-style loop.

```bash
#!/bin/bash

for ((i=1; i<=5; i++))
do
    echo "Iteration: $i"
done
```

Explanation:

- `i=1`: Initialize the counter.
- `i<=5`: Continue while the condition is true.
- `i++`: Increase the counter by one.

---

## 4. The while Loop

A `while` loop repeats commands as long as its condition remains true.

### Syntax

```bash
while [[ condition ]]
do
    commands
done
```

### Example 1: Counting from 1 to 5

```bash
#!/bin/bash

number=1

while [[ $number -le 5 ]]
do
    echo "Number: $number"
    ((number++))
done
```

Output:

```text
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

### How It Works

1. The variable starts at 1.
2. Bash checks whether the number is less than or equal to 5.
3. If true, the number is displayed.
4. The number increases by one.
5. Bash checks the condition again.
6. The loop stops when the condition becomes false.

**Important:** Always make sure your loop can eventually stop. Otherwise, you may create an infinite loop.

### Example 2: User Input Loop

```bash
#!/bin/bash

read -r -p "Enter a number (0 to stop): " number

while [[ "$number" != "0" ]]
do
    echo "You entered: $number"
    read -r -p "Enter another number (0 to stop): " number
done

echo "Program stopped."
```

---

## 5. The until Loop

An `until` loop is similar to a `while` loop, but its condition works in the opposite direction.

- `while` repeats while a condition is true.
- `until` repeats until a condition becomes true.

### Syntax

```bash
until [[ condition ]]
do
    commands
done
```

### Example

```bash
#!/bin/bash

number=1

until [[ $number -gt 5 ]]
do
    echo "Number: $number"
    ((number++))
done
```

Output:

```text
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

The loop stops when the number becomes greater than 5.

---

## 6. The break Statement

The `break` statement immediately terminates a loop.

### Example

```bash
#!/bin/bash

for number in {1..10}
do
    if [[ $number -eq 5 ]]; then
        break
    fi

    echo "$number"
done
```

Output:

```text
1
2
3
4
```

When the number reaches 5, the loop terminates.

### Practical Use

`break` is useful when:

- A search finds the required result.
- A user chooses to exit a menu.
- A program encounters a stopping condition.

---

## 7. The continue Statement

The `continue` statement skips the current iteration and moves to the next iteration.

### Example

```bash
#!/bin/bash

for number in {1..5}
do
    if [[ $number -eq 3 ]]; then
        continue
    fi

    echo "$number"
done
```

Output:

```text
1
2
4
5
```

The number 3 is skipped, but the loop continues.

### Difference Between break and continue

| Statement | Purpose |
|---|---|
| `break` | Stops the loop completely |
| `continue` | Skips the current iteration |

---

## 8. Nested Loops

A nested loop is a loop placed inside another loop.

### Example

```bash
#!/bin/bash

for outer in 1 2 3
do
    echo "Outer loop: $outer"

    for inner in A B C
    do
        echo "  Inner loop: $inner"
    done
done
```

Output:

```text
Outer loop: 1
  Inner loop: A
  Inner loop: B
  Inner loop: C
Outer loop: 2
  Inner loop: A
  Inner loop: B
  Inner loop: C
Outer loop: 3
  Inner loop: A
  Inner loop: B
  Inner loop: C
```

The inner loop runs completely for each iteration of the outer loop.

### Practical Applications

Nested loops can be used for:

- Processing multiple directories and files.
- Comparing multiple values.
- Creating tables.
- Processing structured data.

---

## 9. Looping Through Files

Loops can automate file processing.

### Example: Display Text Files

Create a few sample files:

```bash
touch report1.txt report2.txt report3.txt
```

Create a script:

```bash
#!/bin/bash

for file in *.txt
do
    if [[ -f "$file" ]]; then
        echo "Found file: $file"
    fi
done
```

This script displays regular files matching the `.txt` pattern in the current directory.

### Important Note

If no files match a wildcard pattern, Bash may leave the pattern unchanged. The `nullglob` shell option can prevent this.

Example:

```bash
shopt -s nullglob

for file in *.txt
do
    echo "Found file: $file"
done
```

---

## 10. Reading Files Line by Line

The `while` loop can read a file one line at a time.

### Example

Create a file:

```bash
nano students.txt
```

Enter:

```text
Abbas
Ahmad
Maryam
Fatima
```

Now create:

```bash
nano read-students.sh
```

Enter:

```bash
#!/bin/bash

while IFS= read -r student
do
    echo "Student: $student"
done < students.txt
```

Execute:

```bash
chmod +x read-students.sh
./read-students.sh
```

Explanation:

- `read -r` reads a line without treating backslashes specially.
- `IFS=` prevents leading and trailing whitespace from being removed.
- `student` stores the current line.
- `< students.txt` redirects the file into the loop.

This technique is useful for processing configuration files and logs.

---

## 11. Using Loops with Command Output

Loops can process the output of Linux commands.

### Example: Display Directory Information

```bash
#!/bin/bash

for directory in */
do
    if [[ -d "$directory" ]]; then
        echo "Directory: $directory"
    fi
done
```

This displays directories in the current working directory.

### Example: Check Several Commands

```bash
#!/bin/bash

for command_name in bash python3 git curl
do
    if command -v "$command_name" >/dev/null 2>&1; then
        echo "$command_name is installed."
    else
        echo "$command_name is not installed."
    fi
done
```

This checks whether each command is available in your PATH.

---

## 12. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson08
cd ~/module11-lab/lesson08
```

### Step 2: Create a for Loop Script

```bash
nano for-loop.sh
```

Enter:

```bash
#!/bin/bash

echo "Counting from 1 to 10"

for number in {1..10}
do
    echo "Number: $number"
done
```

Execute:

```bash
chmod +x for-loop.sh
./for-loop.sh
```

### Step 3: Create a while Loop Script

```bash
nano while-loop.sh
```

Enter:

```bash
#!/bin/bash

counter=1

while [[ $counter -le 5 ]]
do
    echo "Counter: $counter"
    ((counter++))
done

echo "Loop completed."
```

Execute:

```bash
chmod +x while-loop.sh
./while-loop.sh
```

### Step 4: Create an until Loop Script

```bash
nano until-loop.sh
```

Enter:

```bash
#!/bin/bash

counter=1

until [[ $counter -gt 5 ]]
do
    echo "Counter: $counter"
    ((counter++))
done

echo "Loop completed."
```

Execute:

```bash
chmod +x until-loop.sh
./until-loop.sh
```

### Step 5: Create a break and continue Script

```bash
nano loop-control.sh
```

Enter:

```bash
#!/bin/bash

for number in {1..10}
do
    if [[ $number -eq 3 ]]; then
        echo "Skipping 3"
        continue
    fi

    if [[ $number -eq 8 ]]; then
        echo "Stopping at 8"
        break
    fi

    echo "Number: $number"
done
```

Execute:

```bash
chmod +x loop-control.sh
./loop-control.sh
```

Observe how 3 is skipped and the loop stops at 8.

---

## 13. Practical Cybersecurity Project: File Extension Scanner

Build a simple defensive script that searches the current lab directory for files with selected extensions.

Create:

```bash
nano extension-scanner.sh
```

Enter:

```bash
#!/bin/bash

echo "========== FILE EXTENSION SCANNER =========="

for file in *.sh *.log *.txt
do
    if [[ -f "$file" ]]; then
        echo "File detected: $file"
    fi
done

echo "============================================"
echo "Scan completed."
```

Make it executable:

```bash
chmod +x extension-scanner.sh
```

Run:

```bash
./extension-scanner.sh
```

The script lists matching regular files in the current directory.

**Security application:** File enumeration is a basic technique used in defensive inventory tools. This script does not inspect file contents or determine whether a file is malicious.

---

## 14. Common Loop Errors

| Error | Explanation | Solution |
|---|---|---|
| Missing `done` | Loop is not properly closed | Add `done` |
| Infinite loop | Condition never becomes false | Update the counter or stopping condition |
| Incorrect variable | Wrong variable used | Check variable names |
| Wrong range | Incorrect range syntax | Review `{start..end}` |
| Missing spaces | Incorrect condition syntax | Use proper spacing inside `[[ ]]` |

### Debugging a Script

Check syntax:

```bash
bash -n script.sh
```

Run with execution tracing:

```bash
bash -x script.sh
```

---

## 15. Lesson Summary

In this lesson, we learned:

- Loops repeat instructions automatically.
- `for` processes items in a list or range.
- `while` repeats while a condition is true.
- `until` repeats until a condition becomes true.
- `break` terminates a loop.
- `continue` skips an iteration.
- Nested loops contain other loops.
- Loops can process files and command output.
- `while read` can process files line by line.

### Important Syntax

| Syntax | Purpose |
|---|---|
| `for` | Starts a for loop |
| `while` | Repeats while a condition is true |
| `until` | Repeats until a condition is true |
| `do` | Starts loop commands |
| `done` | Ends a loop |
| `break` | Stops a loop |
| `continue` | Skips an iteration |
| `((counter++))` | Increases a numeric counter |
| `{1..10}` | Generates a sequence |
| `IFS= read -r` | Safely reads a line |

---

## 16. Knowledge Check

Answer these questions in your notebook:

1. What is a loop?
2. Why are loops important in Bash scripting?
3. Explain the difference between `for` and `while`.
4. What is the purpose of an `until` loop?
5. Differentiate `break` and `continue`.
6. What is a nested loop?
7. How can Bash process files using loops?
8. What does `((counter++))` do?
9. Why should you avoid accidental infinite loops?
10. How can loops help automate defensive cybersecurity tasks?

### Practical Assessment

Successfully create and execute:

1. `for-loop.sh`
2. `while-loop.sh`
3. `until-loop.sh`
4. `loop-control.sh`
5. `extension-scanner.sh`

Also test reading a text file line by line.

**Lesson 08 is complete when all five scripts work correctly.**
