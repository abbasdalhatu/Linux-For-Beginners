
# BASH SCRIPTING FUNDAMENTALS

## 1. What Is Bash?

Bash stands for Bourne Again Shell.

It is a command-line interpreter used on Linux and other Unix-like systems.

Bash allows users to:
- Execute commands.
- Automate repetitive tasks.
- Manage files and directories.
- Work with system processes.
- Create simple administration tools.

## 2. What Is a Bash Script?

A Bash script is a text file containing commands that Bash executes in sequence.

Bash scripts usually use the `.sh` extension.

Example:

```bash
#!/bin/bash

echo "Hello, Linux!"
```

## 3. The Shebang

The first line of many Bash scripts is:

```bash
#!/bin/bash
```

This tells the operating system to use Bash to interpret the script.

## 4. Creating a Bash Script

Create a file:

```bash
nano hello.sh
```

Enter:

```bash
#!/bin/bash

echo "Welcome to Bash scripting!"
```

Save the file.

## 5. Making a Script Executable

Run:

```bash
chmod +x hello.sh
```

Execute it:

```bash
./hello.sh
```

## 6. Variables

Variables store information.

Example:

```bash
#!/bin/bash

name="Abbas"

echo "Hello, $name"
```

Important:
- Do not put spaces around `=`.
- Use `$` to access a variable's value.
- Variable names are case-sensitive.

## 7. Taking User Input

The `read` command accepts input from a user.

Example:

```bash
#!/bin/bash

echo "Enter your name:"
read name

echo "Welcome, $name"
```

## 8. Comments

Comments explain what a script does.

Example:

```bash
# This is a comment

echo "Learning Bash"
```

Bash ignores lines beginning with `#`, except special interpreter lines such as the shebang.

## 9. Basic Mathematical Operations

Use arithmetic expansion:

```bash
#!/bin/bash

a=10
b=5

echo $((a + b))
echo $((a - b))
echo $((a * b))
echo $((a / b))
```

## 10. Conditions

Conditions allow a script to make decisions.

Example:

```bash
#!/bin/bash

read -p "Enter your age: " age

if [ "$age" -ge 18 ]; then
    echo "You are an adult."
else
    echo "You are under 18."
fi
```

## 11. Loops

Loops repeat commands.

### For Loop

```bash
#!/bin/bash

for number in 1 2 3 4 5
do
    echo "$number"
done
```

### While Loop

```bash
#!/bin/bash

number=1

while [ "$number" -le 5 ]
do
    echo "$number"
    number=$((number + 1))
done
```

## 12. Functions

Functions organize reusable commands.

Example:

```bash
#!/bin/bash

greet() {
    echo "Welcome to Bash!"
}

greet
```

## 13. Script Permissions and Safety

- Review a script before executing it.
- Use appropriate file permissions.
- Avoid running unknown scripts as root.
- Back up important files before automation.
- Test scripts in a safe environment.

## 14. Summary

Bash scripting helps Linux users automate tasks and manage systems efficiently.

Important concepts include:
- Variables.
- User input.
- Comments.
- Arithmetic.
- Conditions.
- Loops.
- Functions.
- Script permissions.
