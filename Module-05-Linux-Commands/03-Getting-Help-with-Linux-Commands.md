# LESSON 03 – GETTING HELP WITH LINUX COMMANDS

## 1. Introduction

Linux contains hundreds of commands and thousands of options. It is impossible to memorise everything.

Professional Linux users do not depend entirely on memory. They learn how to find accurate documentation whenever they need it.

Getting help with commands is an essential skill for:

- Linux system administration.
- Software development.
- Troubleshooting.
- Cybersecurity investigations.
- Ethical hacking.
- Bash scripting.

In this lesson, you will learn how to discover commands, understand their options, and find documentation directly from your Kali Linux terminal.

## 2. Why Is Command Help Important?

Imagine that you want to search for a file but cannot remember the available options of the `find` command.

Instead of guessing, you can request help:

```bash
find --help
```

The command displays information about its usage and available options.

Documentation helps you understand:

1. What a command does.
2. How to use it.
3. What options it supports.
4. What arguments it accepts.
5. What errors may occur.
6. How to use it safely.

## 3. The `--help` Option

Many Linux commands support the `--help` option.

General syntax:

```bash
command --help
```

Example:

```bash
ls --help
```

This displays information about the `ls` command.

Another example:

```bash
cp --help
```

This displays usage information for the `cp` command.

You can also try:

```bash
grep --help
```

### Understanding Help Output

Help information commonly includes:

- Usage: The general command structure.
- Options: Available command options.
- Arguments: Information the command expects.
- Description: What the command does.

**Important:** Not every command supports `--help`. Some commands have different help mechanisms.

## 4. The `man` Command

`man` stands for manual.

It opens the manual documentation for a Linux command.

General syntax:

```bash
man command
```

Example:

```bash
man ls
```

This opens the manual page for `ls`.

Another example:

```bash
man grep
```

This opens the documentation for `grep`.

### Navigating Manual Pages

When reading a manual page, you can use these keyboard controls:

| Key | Function |
|---|---|
| Arrow Up | Move up |
| Arrow Down | Move down |
| Space | Move down one page |
| `b` | Move back one page |
| `/word` | Search for a word |
| `n` | Find the next match |
| `q` | Exit the manual |

The `q` key is particularly important because it closes the manual viewer.

## 5. The `whatis` Command

The `whatis` command provides a short description of a command.

Example:

```bash
whatis ls
```

Possible output:

```text
ls (1) - list directory contents
```

Another example:

```bash
whatis pwd
```

It is useful when you want a quick explanation without opening a full manual page.

The information depends on the system's manual database.

## 6. The `apropos` Command

The `apropos` command searches manual-page descriptions for a keyword.

Example:

```bash
apropos network
```

This searches for manual entries related to networking.

Another example:

```bash
apropos password
```

This searches for commands and documentation related to passwords.

You can use it when you know what you want to accomplish but do not know the command name.

**Difference:**

- `whatis`: Explains a known command briefly.
- `apropos`: Helps discover commands related to a topic.

## 7. The `type` Command

The `type` command identifies how Bash interprets a command name.

Example:

```bash
type ls
```

Possible output:

```text
ls is aliased to `ls --color=auto'
```

Depending on your configuration, the output may differ.

Try:

```bash
type cd
type pwd
type echo
```

It can identify whether a command is:

- An alias.
- A shell keyword.
- A shell function.
- A built-in command.
- An external executable.

This is useful when investigating unexpected command behaviour.

## 8. The `help` Command

Bash has built-in commands that are part of the shell itself.

The `help` command provides documentation for Bash built-ins.

Example:

```bash
help cd
```

Another example:

```bash
help echo
```

You can display a list of available Bash built-ins with:

```bash
help
```

Why is this important?

Some commands, such as `cd`, are implemented inside Bash. They do not operate like ordinary external programs.

For example:

```bash
cd /home/kali
```

changes the working directory of the current shell.

## 9. The `info` Command

Some Linux software provides documentation through the GNU Info system.

General syntax:

```bash
info command
```

Example:

```bash
info coreutils
```

This opens documentation for GNU Coreutils.

The Info system supports navigating between sections and topics.

Useful keys include:

| Key | Function |
|---|---|
| `n` | Next node |
| `p` | Previous node |
| `u` | Move up |
| `q` | Quit |

Not every program provides Info documentation.

## 10. Understanding Documentation Sections

Manual pages are divided into numbered sections.

Common sections include:

| Section | Description |
|---|---|
| 1 | User commands |
| 2 | System calls |
| 3 | Library functions |
| 4 | Special files and devices |
| 5 | File formats and configuration |
| 6 | Games |
| 7 | Miscellaneous documentation |
| 8 | System administration commands |

For example:

```bash
man 1 printf
```

Opens the manual for the user-level `printf` command.

Another example:

```bash
man 3 printf
```

Opens documentation for the C library `printf()` function, if installed.

This distinction matters because the same name can refer to different things.

## 11. Finding Help for an Unknown Command

Suppose you want to find a command related to disk usage but do not know its name.

You could try:

```bash
apropos disk
```

Then inspect a result using:

```bash
man COMMAND_NAME
```

You can also use:

```bash
whatis COMMAND_NAME
```

This creates a simple documentation workflow:

1. Search for a topic.
2. Identify a suitable command.
3. Read its manual.
4. Examine its options.
5. Test it safely.

## 12. Practical Laboratory

Perform the following activities inside your Kali Linux VM.

### Exercise 1 – Explore Help Options

Run:

```bash
ls --help
```

Then:

```bash
grep --help
```

Identify at least five options from either help output.

### Exercise 2 – Read Manual Pages

Run:

```bash
man ls
```

Practise:

- Scrolling.
- Searching for a keyword.
- Moving to the next match.
- Exiting with `q`.

Repeat with:

```bash
man grep
```

### Exercise 3 – Use `whatis`

Run:

```bash
whatis ls
whatis pwd
whatis grep
whatis uname
```

Record the descriptions.

### Exercise 4 – Search with `apropos`

Run:

```bash
apropos file
apropos network
apropos password
```

Identify three commands you did not previously know.

### Exercise 5 – Investigate Command Types

Run:

```bash
type cd
type ls
type echo
type pwd
```

Record whether each is a built-in, alias, function, or external command on your system.

### Exercise 6 – Explore Bash Built-in Help

Run:

```bash
help cd
help export
help history
```

Explain what each command does.

## 13. Troubleshooting Missing Documentation

Sometimes you may receive:

```text
man: command not found
```

or:

```text
No manual entry for COMMAND
```

Possible reasons include:

- The documentation package is not installed.
- The manual database is incomplete.
- The command does not provide a manual page.
- The command name is incorrect.

On Kali Linux, manual documentation can be installed through APT when the required package is available.

For example:

```bash
sudo apt update
sudo apt install man-db manpages
```

Do not install packages unnecessarily if the documentation is already available.

## 14. Security Applications

Documentation is important in ethical hacking because tools may have options that can affect systems.

Before using a command:

1. Read its documentation.
2. Understand the options.
3. Understand the arguments.
4. Check whether it changes files or system settings.
5. Test it in your authorised lab.

Never assume that a command is harmless simply because it is short.

## 15. Lesson Summary

In this lesson, you learned:

1. Why Linux documentation is important.
2. How to use `--help`.
3. How to open manual pages with `man`.
4. How to get short descriptions with `whatis`.
5. How to discover commands with `apropos`.
6. How to identify command types with `type`.
7. How to access Bash built-in documentation with `help`.
8. How to use the GNU Info system.
9. How manual sections are organised.
10. How documentation supports safe cybersecurity practice.

## 16. Knowledge Check

Answer these questions:

1. Why is it important to understand how to get help in Linux?
2. What does the `--help` option do?
3. What does `man` stand for?
4. Which key exits a manual page?
5. What is the purpose of `whatis`?
6. What is the difference between `whatis` and `apropos`?
7. What information does `type` provide?
8. Why does Bash provide a separate `help` command?
9. What is the difference between manual section 1 and section 5?
10. What should you do before running an unfamiliar command with elevated privileges?

## 17. Practical Assignment

Complete the following without copying the explanations from this lesson.

1. Find help information for `mkdir`.
2. Open the manual for `chmod`.
3. Find a short description of `hostname`.
4. Search for commands related to processes using `apropos`.
5. Identify whether `cd` is a built-in command.
6. Open Bash help for `export`.

For every task, record:

- The command used.
- What information it returned.
- What you learned from the documentation.

**End of Lesson 03 – Getting Help with Linux Commands.**