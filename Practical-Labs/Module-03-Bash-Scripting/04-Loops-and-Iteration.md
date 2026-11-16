# PRACTICAL LABS
# MODULE 03 – BASH SCRIPTING CHALLENGES

## LAB 04: LOOPS AND ITERATION

### 1. Lab Overview

**Objective:** Learn how to repeat commands automatically using Bash loops.

Imagine you need to display numbers from 1 to 100.

You could write 100 separate `echo` commands, but that would be inefficient.

Instead, you can use a loop to repeat the same operation automatically.

Loops are important in Linux administration and cybersecurity because they help automate repetitive tasks.

Examples include:

- Checking multiple files.
- Processing log entries.
- Monitoring system information.
- Performing repeated calculations.
- Running checks against a list of authorized lab machines.

### 2. What Is a Loop?

A loop is a programming structure that repeats a set of instructions while a specified condition or iteration rule is satisfied.

Bash provides three commonly used loop structures:

| Loop | Purpose |
|---|---|
| `for` | Repeats commands over a sequence of values |
| `while` | Repeats commands while a condition is true |
| `until` | Repeats commands until a condition becomes true |

### 3. Preparing Your Lab

Create your working directory:

```bash
mkdir -p ~/practical-labs/module03/lab04
cd ~/practical-labs/module03/lab04
```

Confirm:

```bash
pwd
```

### 4. The for Loop

A `for` loop executes commands for each value in a list.

Example:

```bash
#!/bin/bash

for number in 1 2 3 4 5
do
    echo "Number: $number"
done
```

Execute:

```bash
bash for-loop.sh
```

Output:

```text
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

Explanation:

- `for` starts the loop.
- `number` is the loop variable.
- `in` introduces the list of values.
- `do` begins the commands.
- `done` closes the loop.

### 5. Using a Range

Bash supports brace expansion for simple numeric sequences.

Example:

```bash
#!/bin/bash

for number in {1..10}
do
    echo "$number"
done
```

This displays numbers from 1 to 10.

You can also count backwards:

```bash
#!/bin/bash

for number in {10..1}
do
    echo "$number"
done
```

### 6. Using C-Style for Loops

Bash supports a loop structure similar to C programming.

Syntax:

```bash
for (( initialization; condition; update ))
do
    commands
done
```

Example:

```bash
#!/bin/bash

for ((i=1; i<=5; i++))
do
    echo "Iteration: $i"
done
```

Understanding the three sections:

- `i=1`: initializes the counter.
- `i<=5`: continues while the condition is true.
- `i++`: increases the counter by one after each iteration.

This is especially useful if you already understand loops in C or other programming languages.

### 7. The while Loop

A `while` loop repeats commands as long as its condition remains true.

Example:

```bash
#!/bin/bash

counter=1

while [[ "$counter" -le 5 ]]
do
    echo "Counter: $counter"
    ((counter++))
done
```

The loop stops when `counter` becomes greater than 5.

**Important:** Always ensure that a while loop can eventually reach its stopping condition. Otherwise, it may run indefinitely.

### 8. The until Loop

An `until` loop works differently from `while`.

- `while` continues while a condition is true.
- `until` continues while a condition is false.

Example:

```bash
#!/bin/bash

counter=1

until [[ "$counter" -gt 5 ]]
do
    echo "Counter: $counter"
    ((counter++))
done
```

The loop stops when the condition becomes true.

### 9. Looping Through Files

Loops can process multiple files.

Create some sample files inside your lab directory:

```bash
touch file1.txt file2.txt file3.txt
```

Create:

```bash
nano file-loop.sh
```

Enter:

```bash
#!/bin/bash

for file in *.txt
do
    if [[ -f "$file" ]]; then
        echo "Found file: $file"
    fi
done
```

Execute:

```bash
bash file-loop.sh
```

The `*.txt` pattern matches filenames ending in `.txt` in the current directory.

The `-f` test ensures that the matched path is a regular file.

### 10. Reading a List of Values

You can use loops to process a list.

Example:

```bash
#!/bin/bash

users=("admin" "analyst" "student" "guest")

for user in "${users[@]}"
do
    echo "Username: $user"
done
```

Explanation:

- `users=(...)` creates an array.
- `"${users[@]}"` accesses all array elements.
- `user` stores each element during an iteration.

This example only displays names; it does not create or modify Linux accounts.

### 11. The break Statement

The `break` statement immediately terminates a loop.

Example:

```bash
#!/bin/bash

for number in {1..10}
do
    if [[ "$number" -eq 5 ]]; then
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

When the counter reaches 5, the loop stops.

### 12. The continue Statement

The `continue` statement skips the current iteration and moves to the next one.

Example:

```bash
#!/bin/bash

for number in {1..10}
do
    if [[ "$number" -eq 5 ]]; then
        continue
    fi

    echo "$number"
done
```

The number 5 will not be displayed.

The loop continues with 6, 7, 8, 9, and 10.

### 13. Nested Loops

A nested loop is a loop inside another loop.

Example:

```bash
#!/bin/bash

for row in {1..3}
do
    for column in {1..3}
    do
        echo "Row: $row | Column: $column"
    done
done
```

The inner loop completes all its iterations for every iteration of the outer loop.

Nested loops are useful when working with multiple groups of data.

### 14. Practical Exercise 1 – Number Counter

Create:

```text
number-counter.sh
```

Requirements:

1. Use a `for` loop.
2. Display numbers from 1 to 20.
3. Display a message after the loop finishes.

### 15. Practical Exercise 2 – Even Number Generator

Create:

```text
even-numbers.sh
```

Requirements:

- Generate numbers from 1 to 50.
- Display only even numbers.
- Use a conditional statement inside a loop.

Hint:

```bash
if (( number % 2 == 0 )); then
    echo "$number"
fi
```

### 16. Practical Exercise 3 – File Inventory

Create:

```text
file-inventory.sh
```

Requirements:

1. Loop through files in your current lab directory.
2. Display each regular file.
3. Display the total number of regular files found.

Hint: You can use a counter variable.

```bash
count=0
```

Increase it for each matching file:

```bash
((count++))
```

Remember to handle the case where no files match.

### 17. Practical Exercise 4 – Simple Countdown

Create:

```text
countdown.sh
```

Requirements:

- Start at 10.
- Count down to 1.
- Display `Finished!` when the loop ends.

Use a `while` loop.

### 18. Practical Exercise 5 – Authorized Lab File Checker

Create:

```text
lab-file-checker.sh
```

Requirements:

1. Use a `for` loop to examine these filenames:
   - `hello.sh`
   - `system-info.sh`
   - `variables.sh`
   - `input.sh`
2. Check whether each file exists in your current directory.
3. Display `FOUND` or `NOT FOUND`.
4. Do not execute the files.

This exercise demonstrates how loops can automate basic file checks.

### 19. Loop Troubleshooting

**Problem: Infinite loop**

A loop may continue indefinitely if its condition never becomes false.

Example of a faulty loop:

```bash
counter=1

while [[ "$counter" -le 5 ]]
do
    echo "$counter"
done
```

The counter never changes.

Correct version:

```bash
counter=1

while [[ "$counter" -le 5 ]]
do
    echo "$counter"
    ((counter++))
done
```

**Problem: Incorrect loop variable**

Make sure the variable is referenced correctly:

```bash
echo "$number"
```

**Problem: Unexpected filename expansion**

Quote variables when processing filenames:

```bash
echo "$file"
```

For commands that accept filenames, use:

```bash
ls -- "$file"
```

Quoting helps prevent spaces and special characters from being interpreted incorrectly.

### 20. Syntax Validation

Check your scripts:

```bash
bash -n file-loop.sh
bash -n number-counter.sh
bash -n even-numbers.sh
bash -n file-inventory.sh
bash -n countdown.sh
bash -n lab-file-checker.sh
```

Inspect your files:

```bash
ls -l
```

### 21. Cybersecurity Application

Loops are widely used in defensive security automation.

For example, a security analyst may need to inspect a collection of log files.

A simple example:

```bash
#!/bin/bash

for file in *.log
do
    if [[ -f "$file" ]]; then
        echo "Log file found: $file"
        wc -l "$file"
    fi
done
```

This script:

1. Finds matching `.log` files in the current directory.
2. Confirms each is a regular file.
3. Displays the filename.
4. Counts its lines.

You can safely test this using sample `.log` files created in your lab directory.

Other defensive uses include:

- Checking backup files.
- Inspecting file permissions.
- Processing security reports.
- Counting failed-login records in authorized log samples.
- Repeating health checks on systems you administer.

### 22. Knowledge Check

1. What is a loop?
2. What is the difference between `for` and `while`?
3. What does `until` do?
4. What is the purpose of `do` and `done`?
5. What does `break` do?
6. What does `continue` do?
7. What is a nested loop?
8. What does `((counter++))` do?
9. Why can an infinite loop be dangerous?
10. How can loops help automate defensive cybersecurity tasks?

### 23. Lab Completion Checklist

- [ ] Created the Lab 04 directory.
- [ ] Practised basic `for` loops.
- [ ] Used numeric ranges.
- [ ] Practised C-style loops.
- [ ] Used `while` loops.
- [ ] Used `until` loops.
- [ ] Processed multiple files.
- [ ] Worked with arrays.
- [ ] Used `break` and `continue`.
- [ ] Practised nested loops.
- [ ] Completed all five practical exercises.
- [ ] Checked script syntax.

### 24. Expected Outcome

By completing this lab, you should understand how to:

- Repeat commands automatically.
- Use `for`, `while`, and `until`.
- Process lists and files.
- Work with arrays.
- Control loop execution.
- Avoid infinite loops.
- Apply iteration to defensive security automation.

**Next Lab:** Functions and Arguments.