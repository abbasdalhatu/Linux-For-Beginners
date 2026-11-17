---
---

# Lesson 10: The `which`, `whereis`, and `type` Commands

## 1. Introduction

Linux provides thousands of commands and utilities.

When you execute a command, you may wonder:

- Where is the command stored?
- Is it a built-in shell command?
- Is it an external executable?
- Does it have a manual page?
- Can multiple versions of the same command exist?

Linux provides three useful commands for answering these questions:

1. `which`
2. `whereis`
3. `type`

Although they are related, they do not perform exactly the same task.

Understanding them is important for Linux administration, troubleshooting, scripting, and cybersecurity.

## 2. Understanding the `which` Command

The `which` command searches the directories listed in the `PATH` environment variable to locate executable commands.

### Basic Syntax

```bash
which [OPTION] COMMAND
```

Example:

```bash
which ls
```

Possible output:

```text
/usr/bin/ls
```

This tells you that the executable found for `ls` is located at `/usr/bin/ls`.

### Finding Other Commands

Try:

```bash
which bash
which python3
which grep
which mkdir
```

The output shows the executable paths found through your command search path.

**Important:** `which` is not a reliable way to identify shell builtins, aliases, or every possible command resolution rule. Use `type` for that purpose.

## 3. Understanding the PATH Variable

Before going further, you need to understand `PATH`.

`PATH` is an environment variable containing a list of directories that the shell searches when you enter a command without specifying its full path.

Display it:

```bash
echo "$PATH"
```

Example output:

```text
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Each directory is separated by a colon (`:`).

For example, when you execute:

```bash
ls
```

Bash searches directories in its command lookup process to find an executable named `ls`, unless another command resolution rule takes precedence.

You can inspect the search path one entry per line:

```bash
printf '%s\n' "$PATH" | tr ':' '\n'
```

This makes the directories easier to read.

## 4. Understanding the `whereis` Command

The `whereis` command locates binary executables, source files, and manual pages associated with a command.

### Basic Syntax

```bash
whereis [OPTION] COMMAND
```

Example:

```bash
whereis ls
```

Possible output:

```text
ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz
```

This may identify:

- The executable file.
- The manual page.
- Source files, if available.

### Searching for Other Commands

```bash
whereis bash
whereis python3
whereis grep
whereis ssh
```

The output depends on what is installed and where your distribution stores the relevant files.

### Understanding `whereis` Options

Common options include:

| Option | Purpose |
|---|---|
| `-b` | Search for binaries only |
| `-m` | Search for manual pages only |
| `-s` | Search for source files only |
| `-u` | Search for unusual entries |

Examples:

Find binary locations:

```bash
whereis -b bash
```

Find manual pages:

```bash
whereis -m ls
```

Find source files:

```bash
whereis -s bash
```

Source files may not be installed on your system.

## 5. Understanding the `type` Command

The `type` command identifies how the current shell interprets a command name.

This is particularly important in Bash because a command name may refer to:

- A shell builtin.
- An alias.
- A function.
- An external executable.

### Basic Syntax

```bash
type COMMAND
```

Example:

```bash
type cd
```

Possible output:

```text
cd is a shell builtin
```

This tells you that `cd` is implemented inside the shell.

Another example:

```bash
type ls
```

Possible output:

```text
ls is aliased to `ls --color=auto`
```

Or it may identify an executable, depending on your configuration.

### Checking Multiple Commands

```bash
type pwd
type cd
type echo
type ls
type grep
```

Observe the differences.

## 6. Important `type` Options

### 6.1 Display All Matches: `type -a`

```bash
type -a ls
```

This displays all matching definitions and executable locations known through the shell's command lookup.

It is useful when multiple versions or definitions exist.

### 6.2 Display Only the Command Path: `type -P`

```bash
type -P ls
```

This searches for an external executable through the command search path.

It does not report shell builtins as executable paths.

### 6.3 Display a Short Description: `type -t`

```bash
type -t cd
```

Possible output:

```text
builtin
```

Other possible results include:

- `alias`
- `function`
- `file`
- `keyword`

These identify how Bash classifies the command name.

## 7. Comparing `which`, `whereis`, and `type`

These commands answer different questions.

| Command | Main purpose |
|---|---|
| `which` | Find an executable through PATH |
| `whereis` | Find binary, source, and manual-page locations |
| `type` | Identify how the current shell resolves a command |

Example comparison:

```bash
which ls
whereis ls
type ls
```

You may receive three different types of information.

Remember:

- `which` focuses on executable lookup.
- `whereis` searches for related files in known locations.
- `type` explains how your shell interprets the command name.

## 8. Understanding Aliases, Functions, Builtins, and Executables

To understand `type`, you need to know four important concepts.

### 8.1 Alias

An alias is a shortcut for another command or command sequence.

Example:

```bash
alias ll='ls -lah'
```

Now execute:

```bash
ll
```

The shell expands the alias into its associated command.

Check it:

```bash
type ll
```

### 8.2 Shell Builtin

A builtin is implemented directly inside the shell.

Examples:

```bash
cd
echo
pwd
history
```

Check one:

```bash
type cd
```

### 8.3 Shell Function

A shell function is a named block of commands.

Example:

```bash
hello() {
    echo "Welcome to Linux"
}
```

Execute it:

```bash
hello
```

Check it:

```bash
type hello
```

### 8.4 External Executable

An external executable is a program stored in the filesystem.

Examples commonly include:

```bash
ls
grep
mkdir
```

Check one:

```bash
type mkdir
```

The shell may identify its executable path.

## 9. Practical Lab: Finding Linux Commands

Perform these exercises in your Kali Linux virtual machine.

### Step 1: Find the Location of `ls`

```bash
which ls
```

### Step 2: Find the Location of Bash

```bash
which bash
```

### Step 3: Find Python 3

```bash
which python3
```

### Step 4: Inspect Related Files for `grep`

```bash
whereis grep
```

### Step 5: Find Only Its Binary

```bash
whereis -b grep
```

### Step 6: Find Its Manual Page

```bash
whereis -m grep
```

### Step 7: Inspect Shell Builtins

```bash
type cd
type echo
type pwd
type history
```

### Step 8: Inspect an External Command

```bash
type ls
type grep
```

### Step 9: Display All Matches

```bash
type -a ls
```

### Step 10: Inspect PATH

```bash
echo "$PATH"
```

Then display each directory separately:

```bash
printf '%s\n' "$PATH" | tr ':' '\n'
```

## 10. Practical Lab: Create and Inspect an Alias

### Step 1: Create an Alias

```bash
alias ll='ls -lah'
```

### Step 2: Execute the Alias

```bash
ll
```

### Step 3: Inspect the Alias

```bash
type ll
```

### Step 4: Display All Definitions

```bash
type -a ll
```

### Step 5: Remove the Alias

```bash
unalias ll
```

### Step 6: Verify Its Removal

```bash
type ll
```

You should no longer see the alias definition.

This alias exists only in your current shell session unless you save it in a startup configuration file.

## 11. Practical Lab: Create and Inspect a Function

### Step 1: Define a Function

```bash
greeting() {
    echo "Linux for Ethical Hackers"
}
```

### Step 2: Execute It

```bash
greeting
```

### Step 3: Inspect It

```bash
type greeting
```

### Step 4: Remove It

```bash
unset -f greeting
```

### Step 5: Verify Removal

```bash
type greeting
```

This demonstrates how shell functions differ from external executables.

## 12. Common Mistakes

### Mistake 1: Assuming `which` Finds Every Command

Try:

```bash
which cd
```

You may receive no result because `cd` is normally a Bash builtin, not an external executable.

Use:

```bash
type cd
```

### Mistake 2: Confusing `whereis` with `which`

`whereis` can locate manual pages and other related files.

`which` primarily searches for executable commands through PATH.

### Mistake 3: Assuming a Command Is Always an External Program

A command name might refer to an alias, function, builtin, or executable.

Use:

```bash
type COMMAND
```

### Mistake 4: Ignoring PATH

If an executable is not in a directory searched through PATH, entering its name alone may fail.

You may need to specify its full path or adjust the environment appropriately.

## 13. Cybersecurity Importance

These commands are particularly useful during security assessments and system administration.

### 13.1 Detecting Unexpected Command Locations

```bash
which ssh
type -a ssh
```

These commands can help identify which executable or definition your shell may use.

### 13.2 Investigating PATH Manipulation

An unsafe PATH configuration may cause a shell to execute an unintended program.

Inspect:

```bash
echo "$PATH"
```

Pay attention to unexpected directories and writable locations.

### 13.3 Identifying Command Aliases

An alias can change how a familiar command behaves.

Inspect it using:

```bash
type -a ls
```

### 13.4 Understanding Shell Functions

Functions can override the expected behaviour of command names.

Inspect them with:

```bash
type COMMAND
```

### 13.5 Investigating Suspicious Executables

During an authorised investigation, command-location checks can help identify unusual executable paths.

However, a path alone does not prove that a program is malicious. Verify files using ownership, permissions, package information, hashes, and other evidence.

**Security rule:** Do not assume that a familiar command name always executes the program you expect.

## 14. Important Commands Summary

| Command | Purpose |
|---|---|
| `which ls` | Find executable through PATH |
| `whereis ls` | Find related binary, source, and manual locations |
| `whereis -b ls` | Find binary locations |
| `whereis -m ls` | Find manual pages |
| `type ls` | Identify how the shell resolves `ls` |
| `type -a ls` | Display all known matches |
| `type -P ls` | Display an external executable path |
| `type -t cd` | Display the command classification |
| `echo "$PATH"` | Display executable search directories |
| `alias ll='ls -lah'` | Create an alias |
| `unalias ll` | Remove an alias |
| `unset -f greeting` | Remove a shell function |

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is the main purpose of `which`?
2. What information can `whereis` locate?
3. What does the `type` command identify?
4. What is the purpose of the PATH variable?
5. Why might `which cd` return no result?
6. What is a shell builtin?
7. What is an alias?
8. What is the difference between an alias and a shell function?
9. What does `type -a` do?
10. Why is inspecting PATH important during a cybersecurity investigation?

## 16. Practical Assignment

Complete the following tasks in Kali Linux.

1. Locate the `ls` executable using `which`.
2. Locate the Bash executable.
3. Use `whereis` to inspect `grep`.
4. Find the manual-page location of `grep`.
5. Use `type` to inspect `cd`, `pwd`, `echo`, and `ls`.
6. Use `type -a` to inspect `ls`.
7. Display your PATH variable.
8. Print every PATH directory on a separate line.
9. Create an alias named `ll`.
10. Inspect the alias using `type`.
11. Create a function named `greeting`.
12. Inspect the function and then remove it.
13. Explain three ways these commands can help detect unexpected command behaviour.

## 17. Lesson Conclusion

The `which`, `whereis`, and `type` commands help you understand how Linux commands are located and interpreted.

You have learned how to:

- Find executable locations.
- Locate manual pages.
- Understand the PATH variable.
- Identify aliases.
- Identify shell builtins.
- Inspect shell functions.
- Investigate command resolution.

**Key lesson:** Before trusting a command, understand what your shell will actually execute.

---

**End of Lesson 10 — The `which`, `whereis`, and `type` Commands**