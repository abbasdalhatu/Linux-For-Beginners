---
---

# LESSON 02 – COMMAND SYNTAX AND STRUCTURE

## 1. Introduction

Linux commands follow specific structures and rules. Understanding these rules helps you execute commands correctly, troubleshoot errors, and combine commands effectively.

Command syntax refers to the correct arrangement of a command, its options, arguments, and other elements.

For example:

```bash
ls -la /home/kali
```

This command contains:
- `ls`: Command name.
- `-la`: Options.
- `/home/kali`: Argument.

Understanding command syntax is an essential Linux skill.

## 2. General Linux Command Structure

A typical Linux command follows this structure:

```text
command [options] [arguments]
```

The square brackets indicate optional elements. They are not typed literally.

### 2.1 Command

The command identifies the program or operation to execute.

Example:

```bash
date
```

### 2.2 Options

Options modify the behaviour of a command.

Example:

```bash
ls -l
```

The `-l` option displays information in long format.

### 2.3 Arguments

Arguments provide additional information to the command.

Example:

```bash
cat report.txt
```

Here, `report.txt` is the argument.

### 2.4 Combining Them

```bash
ls -la /home/kali
```

- Command: `ls`
- Options: `-l` and `-a`
- Argument: `/home/kali`

## 3. Short and Long Options

Linux commands commonly support short and long options.

### Short Options

Short options usually begin with one hyphen.

```bash
ls -l
```

### Long Options

Long options usually begin with two hyphens.

```bash
ls --all
```

### Combining Short Options

Some commands allow multiple short options to be combined.

```bash
ls -la
```

This is equivalent to:

```bash
ls -l -a
```

### Using Long Options Together

```bash
ls --all --human-readable
```

This displays hidden files and uses human-readable file sizes where applicable.

**Important:** Not every command supports both short and long options. Always consult its help or manual page.

## 4. Arguments and Their Order

Some commands accept multiple arguments.

Example:

```bash
mkdir documents images backups
```

This creates three directories.

Another example:

```bash
cp report.txt backup.txt
```

This copies `report.txt` to `backup.txt`.

For many commands, argument order matters.

For example:

```bash
mv old.txt new.txt
```

The first argument is the source, and the second is the destination.

Changing their order changes the operation.

## 5. Command Separators and Operators

Bash provides operators that control how commands execute.

### 5.1 Semicolon (;)

Runs commands sequentially, regardless of whether an earlier command succeeds.

```bash
pwd; whoami; hostname
```

### 5.2 AND Operator (&&)

Runs the next command only if the previous command succeeds.

```bash
mkdir practice && cd practice
```

If directory creation succeeds, the shell changes into it.

### 5.3 OR Operator (||)

Runs the next command only if the previous command fails.

```bash
cd /directory-that-does-not-exist || echo "Directory not found"
```

### 5.4 Pipe Operator (|)

Sends the standard output of one command to the standard input of another.

```bash
ls -l | grep ".txt"
```

This filters the directory listing for lines containing `.txt`.

Pipes will be studied in greater detail in later lessons.

## 6. Understanding Quoting in Command Syntax

Quoting helps control how Bash interprets text.

### Single Quotes

Single quotes preserve the literal meaning of almost everything inside them.

```bash
echo '$HOME'
```

Output:

```text
$HOME
```

### Double Quotes

Double quotes preserve most characters but allow variable expansion and command substitution.

```bash
echo "$HOME"
```

This displays the home directory path.

### Backslash

A backslash can escape a special character.

```bash
echo "\$HOME"
```

Output:

```text
$HOME
```

## 7. Command Case Sensitivity

Linux commands are generally case-sensitive.

For example:

```bash
whoami
```

is different from:

```bash
WhoAmI
```

The first is a valid command. The second normally produces a command-not-found error.

Options and arguments may also be case-sensitive depending on the command.

## 8. Using Multiple Options Correctly

Consider:

```bash
ls -lh /var/log
```

This combines:
- `-l`: Long listing.
- `-h`: Human-readable file sizes.
- `/var/log`: Directory argument.

Another example:

```bash
ps aux
```

This uses options to request a detailed process listing.

Some commands use options that do not follow the conventional hyphen format. Learn the syntax of each command instead of assuming every option behaves identically.

## 9. Understanding Exit Status

When a command finishes, it returns an exit status.

The most common convention is:

- `0`: Success.
- Non-zero: An error or another condition.

You can inspect the exit status using:

```bash
echo $?
```

Example:

```bash
pwd
echo $?
```

If `pwd` succeeds, the result should normally be:

```text
0
```

Now try:

```bash
cd /directory-that-does-not-exist
echo $?
```

The exit status should be non-zero.

Exit statuses are useful in Bash scripts because they allow scripts to detect failures and make decisions.

## 10. Practical Laboratory

Perform these exercises inside your Kali Linux virtual machine.

### Exercise 1 – Identify Command Components

Run:

```bash
ls -lah /home/kali
```

Identify:
1. The command.
2. The options.
3. The argument.

### Exercise 2 – Compare Options

Run:

```bash
ls
ls -l
ls -a
ls -la
```

Explain how the output changes.

### Exercise 3 – Practise Command Operators

Run:

```bash
pwd; whoami; hostname
```

Then:

```bash
mkdir -p ~/syntax-practice && cd ~/syntax-practice
```

Finally:

```bash
cd /directory-that-does-not-exist || echo "The directory could not be opened"
```

Explain the purpose of each operator.

### Exercise 4 – Practise Quoting

Run:

```bash
echo '$HOME'
echo "$HOME"
echo "\$HOME"
```

Record the three outputs and explain the differences.

### Exercise 5 – Check Exit Status

Run:

```bash
pwd
echo $?
```

Then:

```bash
cd /this-directory-does-not-exist
echo $?
```

Explain why the exit statuses differ.

## 11. Common Command Syntax Mistakes

| Mistake | Explanation |
|---|---|
| Misspelling a command | The shell may not find the command |
| Incorrect option | The command may reject the option |
| Missing argument | The command may lack required information |
| Wrong argument order | The operation may behave differently |
| Incorrect quotation | Bash may interpret text unexpectedly |
| Wrong path | The requested file or directory may not exist |
| Ignoring exit status | A failed command may go unnoticed |

## 12. Security Considerations

Understanding command syntax is also important for cybersecurity.

### Avoid Blindly Executing Commands

Never execute commands copied from unknown sources without understanding them.

### Be Careful with Root Privileges

Commands executed with `sudo` can make system-wide changes.

Understand what a command does before granting it elevated privileges.

### Understand Command Chaining

A command containing `;`, `&&`, or `||` may execute several operations.

Read the complete command before running it.

### Be Careful with Quoting

Incorrect quoting can cause unexpected argument splitting or unwanted command interpretation.

Use proper quoting when handling filenames, variables, and user input.

## 13. Lesson Summary

In this lesson, you learned:

1. The general structure of Linux commands.
2. The difference between commands, options, and arguments.
3. Short and long options.
4. How argument order affects commands.
5. The purpose of `;`, `&&`, `||`, and `|`.
6. How single quotes, double quotes, and backslashes work.
7. Why Linux commands are case-sensitive.
8. How to check command exit statuses.
9. Common command syntax errors.
10. Basic security practices when executing commands.

## 14. Knowledge Check

Answer these questions:

1. What is command syntax?
2. Write the general structure of a Linux command.
3. What is the difference between an option and an argument?
4. What is the difference between `-l` and `--all`?
5. Explain the purpose of `;`.
6. What happens when you use `&&`?
7. What happens when you use `||`?
8. What does the pipe operator do?
9. What is the difference between single and double quotes?
10. What does an exit status of `0` normally indicate?

## 15. Practical Assignment

Write down the commands and explain their components.

```bash
ls -lah /var/log
```

```bash
mkdir -p ~/linux-test && cd ~/linux-test
```

```bash
pwd; whoami; hostname
```

```bash
cd /missing-directory || echo "Operation failed"
```

```bash
echo "$HOME"
```

For each command, identify:
- Command name.
- Options, if present.
- Arguments, if present.
- Operators, if present.
- Expected behaviour.

**End of Lesson 02 – Command Syntax and Structure.**