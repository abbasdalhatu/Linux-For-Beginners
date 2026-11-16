
# BASH SCRIPTING — PRACTICAL EXERCISES

Welcome to the Bash Scripting practical exercises.

These exercises develop programming and automation skills using Bash.

**Difficulty:** Beginner to Advanced

## Exercise 1: Your First Bash Script

Create a file named `hello.sh`.

Write a script that displays:

```text
Welcome to Bash Scripting!
I am learning Linux automation.
```

Tasks:
1. Add the Bash shebang.
2. Use the echo command.
3. Save the script.
4. Make it executable.
5. Run the script.

## Exercise 2: Variables

Create a script that stores:
- Your name.
- Your course.
- Your learning level.

Display all three values.

Example:

```bash
#!/bin/bash

name="Student"
course="Computer Science"
level="Beginner"

echo "Name: $name"
echo "Course: $course"
echo "Level: $level"
```

Challenge: Modify the script to accept the values from the user.

## Exercise 3: User Input

Create a script that asks for a user's name and displays a welcome message.

Requirements:
1. Use `read`.
2. Store the input in a variable.
3. Display the result using `echo`.

## Exercise 4: Mathematical Operations

Create a calculator script that accepts two numbers.

It should perform:
- Addition.
- Subtraction.
- Multiplication.
- Division.

Challenge:
Handle division by zero safely.

## Exercise 5: Conditional Statements

Create a script that accepts a number and checks whether it is:
- Positive.
- Negative.
- Zero.

Challenge: Create another script that checks whether a number is even or odd.

## Exercise 6: Loops

Write a script that displays numbers from 1 to 10 using a `for` loop.

Then write another script that uses a `while` loop to display the same numbers.

Questions:
1. What is a loop?
2. What is the difference between `for` and `while`?
3. Why are loops useful in automation?

## Exercise 7: Functions

Create a function named `greet`.

The function should display:

```text
Welcome to Linux Administration!
```

Call the function from your script.

Challenge: Create a function that accepts a name as an argument.

## Exercise 8: File and Directory Automation

Write a script that:

1. Creates a directory called `MyBackup`.
2. Creates a file called `notes.txt`.
3. Writes a message into the file.
4. Copies the file into `MyBackup`.
5. Displays a completion message.

Use a temporary practice directory.

## Exercise 9: System Information Script

Create a script that displays:

- Current username.
- Hostname.
- Linux kernel version.
- System uptime.
- Memory usage.
- Disk usage.

Use appropriate Linux commands.

## Exercise 10: Intermediate Automation Challenge

Create a menu-driven Bash script with these options:

```text
===== LINUX HELPER =====

1. Display current user
2. Display system information
3. Display disk usage
4. Display memory usage
5. List current directory
6. Exit
```

Requirements:
1. Accept a menu selection.
2. Use `case` to process the selection.
3. Display the appropriate result.
4. Allow the user to return to the menu.
5. Exit when option 6 is selected.
6. Handle invalid selections.

## Exercise 11: Error Handling

Answer the following:

1. What is an exit status?
2. What does exit code 0 normally indicate?
3. What does `set -e` do?
4. What is the purpose of `if` statements in error handling?
5. Why should scripts validate user input?

Challenge: Write a script that checks whether a directory exists before attempting to use it.

## Exercise 12: Advanced Challenge

Create a simple backup automation script.

Requirements:
1. Ask the user for a source directory.
2. Check whether the directory exists.
3. Create a backup directory.
4. Copy the source contents into the backup directory.
5. Display whether the operation succeeded.
6. Handle errors safely.

Use sample directories and files. Do not overwrite important data.

## Knowledge Assessment

1. What is Bash?
2. What is a shell script?
3. What is the purpose of a shebang?
4. How are variables declared?
5. What does `read` do?
6. What is a conditional statement?
7. What is a loop?
8. What is a function?
9. What does `chmod +x` do?
10. Why is error handling important?

## Completion Checklist

- [ ] Write and execute a Bash script.
- [ ] Use variables.
- [ ] Accept user input.
- [ ] Perform arithmetic.
- [ ] Use conditional statements.
- [ ] Use loops.
- [ ] Create functions.
- [ ] Automate file operations.
- [ ] Display system information.
- [ ] Build a menu-driven script.
- [ ] Understand error handling.
- [ ] Complete the backup challenge.

**Learning objective:** Develop the ability to write Bash scripts that automate everyday Linux tasks.
