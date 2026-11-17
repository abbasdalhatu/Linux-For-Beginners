---
---

# Lesson 12: Shell Expansion and Quoting

## 1. Introduction

When you type a command in Linux, Bash does not always execute exactly what you typed.

Before executing a command, Bash processes certain expressions and replaces them with their corresponding values.

This process is called **shell expansion**.

For example:

```bash
name="Abbas"
echo "Hello $name"
```

Output:

```text
Hello Abbas
```

Bash replaced `$name` with the value stored in the variable.

Quoting controls how Bash interprets characters, spaces, variables, and special symbols.

Understanding expansion and quoting is essential for:
- Writing reliable Bash scripts.
- Working with filenames containing spaces.
- Processing command output.
- Automating Linux administration.
- Preventing command injection and unexpected command behavior.

In this lesson, we will cover:

1. What shell expansion means.
2. Parameter expansion.
3. Command substitution.
4. Arithmetic expansion.
5. Brace expansion.
6. Tilde expansion.
7. Filename expansion.
8. Single quotes.
9. Double quotes.
10. Escape characters.
11. Word splitting.
12. Practical cybersecurity applications.

## 2. What Is Shell Expansion?

Shell expansion is the process through which Bash transforms expressions into values before executing commands.

Example:

```bash
name="Abbas"
echo "My name is $name"
```

Bash expands `$name` into `Abbas`.

Another example:

```bash
echo "Today is $(date)"
```

Bash executes `date` and inserts its output into the command.

Shell expansion makes commands dynamic and flexible.

## 3. Types of Shell Expansion

Bash supports several types of expansion.

| Expansion | Purpose |
|---|---|
| Parameter expansion | Retrieves or manipulates variable values |
| Command substitution | Replaces a command with its output |
| Arithmetic expansion | Performs mathematical calculations |
| Brace expansion | Generates multiple strings |
| Tilde expansion | Expands `~` into a home directory |
| Filename expansion | Matches filenames using wildcard patterns |

Let's examine each one.

## 4. Parameter Expansion

Parameter expansion allows Bash to retrieve or manipulate the value of a variable.

The basic syntax is:

```bash
$variable
```

Example:

```bash
username="Abbas"
echo "$username"
```

Output:

```text
Abbas
```

You can also use braces:

```bash
echo "${username}"
```

Braces make the variable boundary clear.

### Why use braces?

Consider:

```bash
name="Abbas"
echo "$nameLinux"
```

Bash interprets `$nameLinux` as a different variable.

Instead, use:

```bash
echo "${name}Linux"
```

Output:

```text
AbbasLinux
```

### Useful parameter expansion examples

Get the length of a variable:

```bash
name="Abbas"
echo "${#name}"
```

Output:

```text
5
```

Provide a default value when a variable is unset or empty:

```bash
echo "${username:-Guest}"
```

Assign a default value when a variable is unset or empty:

```bash
echo "${username:=Guest}"
```

Remove a matching prefix:

```bash
file="report.txt"
echo "${file#*.}"
```

Output:

```text
txt
```

Parameter expansion is useful when processing filenames, paths, and configuration values.

## 5. Command Substitution

Command substitution allows you to execute a command and store or use its output.

There are two common forms.

### Method 1: Modern syntax

```bash
current_user=$(whoami)
```

Display:

```bash
echo "$current_user"
```

### Method 2: Older syntax

```bash
current_user=`whoami`
```

Both forms work in Bash, but the `$()` form is recommended.

### Practical examples

Get the current date:

```bash
today=$(date)
echo "$today"
```

Get the system hostname:

```bash
host=$(hostname)
echo "$host"
```

Get the kernel version:

```bash
kernel=$(uname -r)
echo "$kernel"
```

### Nested command substitution

The modern syntax can be nested:

```bash
echo "Kernel: $(uname -r)"
```

You can use command substitution in system-information scripts, log analysis, and administrative automation.

## 6. Arithmetic Expansion

Bash supports arithmetic expansion using:

```bash
$(( expression ))
```

Example:

```bash
echo $((10 + 5))
```

Output:

```text
15
```

### Arithmetic operations

| Operator | Meaning | Example |
|---|---|---|
| `+` | Addition | `10 + 5` |
| `-` | Subtraction | `10 - 5` |
| `*` | Multiplication | `10 * 5` |
| `/` | Division | `10 / 5` |
| `%` | Remainder | `10 % 3` |

Examples:

```bash
echo $((20 + 10))
echo $((20 - 10))
echo $((5 * 4))
echo $((20 / 5))
echo $((10 % 3))
```

### Using variables

```bash
first=20
second=10

result=$((first + second))

echo "$result"
```

Output:

```text
30
```

Bash arithmetic uses integers by default. For decimal calculations, additional tools such as `bc` may be used.

## 7. Brace Expansion

Brace expansion generates multiple strings from a single expression.

Example:

```bash
echo file{1,2,3}.txt
```

Output:

```text
file1.txt file2.txt file3.txt
```

### Creating multiple directories

```bash
mkdir project/{documents,scripts,logs,backups}
```

This creates four directories under `project`, provided the parent directory exists.

You can also use numeric sequences:

```bash
echo {1..5}
```

Output:

```text
1 2 3 4 5
```

Alphabetical sequences:

```bash
echo {a..e}
```

Output:

```text
a b c d e
```

Brace expansion is useful for creating directory structures and generating filenames.

## 8. Tilde Expansion

The tilde character `~` represents the current user's home directory.

Example:

```bash
echo ~
```

It may produce:

```text
/home/kali
```

Navigate to your home directory:

```bash
cd ~
```

You can also reference another user's home directory:

```bash
echo ~root
```

This normally displays the root user's home directory if the account exists.

Tilde expansion is useful when working with user files and configuration directories.

## 9. Filename Expansion (Globbing)

Filename expansion allows Bash to match filenames using wildcard characters.

The main wildcard characters are:

| Character | Meaning |
|---|---|
| `*` | Matches zero or more characters |
| `?` | Matches exactly one character |
| `[abc]` | Matches one listed character |
| `[a-z]` | Matches one character in a range |

### Example 1: Match all files

```bash
ls *
```

### Example 2: Match text files

```bash
ls *.txt
```

### Example 3: Match files beginning with report

```bash
ls report*
```

### Example 4: Match filenames containing one character after file

```bash
ls file?.txt
```

This could match:

```text
file1.txt
file2.txt
fileA.txt
```

### Important distinction

The `*` used in filename expansion is not the same as the multiplication operator used in arithmetic expansion.

For example:

```bash
ls *.txt
```

uses filename expansion, while:

```bash
echo $((5 * 4))
```

uses arithmetic expansion.

## 10. Understanding Quoting in Bash

Quoting controls how Bash interprets special characters.

There are three important mechanisms:

1. Single quotes.
2. Double quotes.
3. Backslash escaping.

Understanding their differences prevents many scripting errors.

## 11. Single Quotes

Single quotes preserve the literal meaning of almost every character inside them.

Example:

```bash
name="Abbas"
echo 'Hello $name'
```

Output:

```text
Hello $name
```

Bash does not expand `$name` inside single quotes.

Another example:

```bash
echo 'Today is $(date)'
```

Output:

```text
Today is $(date)
```

The command is not executed.

### When should you use single quotes?

Use single quotes when you want text treated literally.

For example:

```bash
echo 'Do not execute this expression: $(whoami)'
```

## 12. Double Quotes

Double quotes preserve most characters literally while still allowing certain expansions.

These include:
- Variable expansion.
- Command substitution.
- Arithmetic expansion.

Example:

```bash
name="Abbas"
echo "Hello $name"
```

Output:

```text
Hello Abbas
```

Command substitution also works:

```bash
echo "Current user: $(whoami)"
```

Arithmetic expansion works:

```bash
echo "Total: $((10 + 20))"
```

### Why are double quotes important?

Consider:

```bash
filename="my report.txt"
```

Without quotes:

```bash
cat $filename
```

Bash may interpret the expanded value as two separate words:

```text
my
report.txt
```

Instead, use:

```bash
cat "$filename"
```

Now Bash treats the filename as one argument.

**Important rule:** In most cases, quote variable expansions using double quotes unless you intentionally want word splitting or filename expansion.

## 13. Backslash Escaping

A backslash `\` can remove the special meaning of the next character.

Example:

```bash
echo \$HOME
```

Output:

```text
$HOME
```

Bash prints the literal text instead of expanding the variable.

Another example:

```bash
echo "This is a \"quoted\" word"
```

Output:

```text
This is a "quoted" word
```

You can also escape spaces in filenames:

```bash
touch my\ report.txt
```

This creates a file named:

```text
my report.txt
```

Using quotes is usually clearer:

```bash
touch "my report.txt"
```

## 14. Word Splitting

Word splitting occurs when Bash expands an unquoted value and separates it into multiple words according to the shell's Internal Field Separator (IFS).

Example:

```bash
files="one.txt two.txt"
```

Now:

```bash
printf '<%s>\n' $files
```

Output:

```text
<one.txt>
<two.txt>
```

Because the variable is unquoted, Bash splits the expanded value.

Now use double quotes:

```bash
printf '<%s>\n' "$files"
```

Output:

```text
<one.txt two.txt>
```

The entire value is passed as one argument.

### Why does this matter?

Suppose you have a filename:

```bash
file="important report.txt"
```

This is unsafe:

```bash
cat $file
```

Use:

```bash
cat "$file"
```

Quoting protects filenames and values containing spaces.

## 15. Understanding "$@" and "$*"

These special parameters represent command-line arguments.

However, their quoted behavior differs.

### Quoted "$@"

```bash
"$@"
```

Preserves each argument as a separate argument.

### Quoted "$*"

```bash
"$*"
```

Combines all positional parameters into one word, separated by the first character of IFS.

Example script:

```bash
#!/bin/bash

printf '<%s>\n' "$@"

echo "----------------"

printf '<%s>\n' "$*"
```

Run:

```bash
bash arguments.sh "Abbas Muhammad" Linux
```

The `"$@"` section preserves two arguments.

The `"$*"` section combines them into one argument.

This distinction is important when writing scripts that process user-supplied filenames or command-line parameters.

## 16. Practical Cybersecurity Applications

Shell expansion and quoting are important in cybersecurity automation.

### Example 1: Safely store a target address

```bash
target="192.0.2.10"

echo "Target: $target"
```

The address belongs to the documentation-only TEST-NET-1 range.

### Example 2: Store command output

```bash
kernel=$(uname -r)

echo "Detected kernel: $kernel"
```

### Example 3: Safely process a filename

```bash
log_file="security report.txt"

if [ -f "$log_file" ]; then
    echo "Report exists"
fi
```

### Example 4: Calculate a number of scanned hosts

```bash
hosts=10
completed=7

remaining=$((hosts - completed))

echo "Remaining: $remaining"
```

### Security warning: Command injection

Never assume that user input is safe.

For example, avoid constructing shell commands by inserting untrusted input into a string and executing it using `eval`.

Unsafe:

```bash
eval "some_command $user_input"
```

`eval` can cause text to be interpreted as shell syntax.

Prefer using quoted variables and passing arguments directly to commands.

## 17. Practical Lab: Shell Expansion and Quoting

### Lab objective

Practice different forms of shell expansion and understand how quoting affects command execution.

### Step 1: Create a working directory

```bash
mkdir -p ~/expansion-lab
cd ~/expansion-lab
```

### Step 2: Practice parameter expansion

```bash
name="Abbas"
echo "$name"
echo "${name}"
echo "${#name}"
```

### Step 3: Practice command substitution

```bash
username=$(whoami)
kernel=$(uname -r)

echo "User: $username"
echo "Kernel: $kernel"
```

### Step 4: Practice arithmetic expansion

```bash
a=25
b=5

echo $((a + b))
echo $((a - b))
echo $((a * b))
echo $((a / b))
```

### Step 5: Practice brace expansion

```bash
echo file{1,2,3,4}.txt
echo {1..10}
echo {a..e}
```

### Step 6: Practice tilde expansion

```bash
echo ~
cd ~
pwd
```

### Step 7: Practice filename expansion

Create some files:

```bash
touch report1.txt report2.txt report3.txt notes.md image.png
```

Display text files:

```bash
ls *.txt
```

Display Markdown files:

```bash
ls *.md
```

### Step 8: Practice single quotes

```bash
name="Abbas"

echo 'My name is $name'
```

Observe that `$name` is not expanded.

### Step 9: Practice double quotes

```bash
echo "My name is $name"
```

Observe the difference.

### Step 10: Practice escaped characters

```bash
echo \$HOME
echo "The command is: \"whoami\""
```

### Step 11: Practice filenames containing spaces

```bash
touch "security report.txt"
```

Inspect the file:

```bash
ls
```

Store the filename:

```bash
file="security report.txt"
```

Check it:

```bash
if [ -f "$file" ]; then
    echo "File exists"
fi
```

### Step 12: Compare word splitting

```bash
value="Linux Ethical Hacking"

printf '<%s>\n' $value
printf '<%s>\n' "$value"
```

Observe the difference.

## 18. Knowledge Check

Answer these questions in your notebook.

1. What is shell expansion?
2. What is parameter expansion?
3. What is command substitution?
4. What is arithmetic expansion?
5. What is brace expansion?
6. What does the tilde character represent?
7. What is filename expansion?
8. What is the difference between single quotes and double quotes?
9. What does a backslash do?
10. What is word splitting?
11. Why should variable expansions usually be quoted?
12. What is the difference between `"$@"` and `"$*"`?
13. What is command injection?
14. Why should untrusted input not be passed to `eval`?
15. How can quoting help prevent unexpected command behavior?

## 19. Practical Assignment

Create a Bash script named:

`expansion-practice.sh`

Your script must:

1. Store your name in a variable.
2. Display the variable using parameter expansion.
3. Display the number of characters in your name.
4. Display the current username using command substitution.
5. Display the current kernel version using command substitution.
6. Perform at least three arithmetic calculations.
7. Display a sequence using brace expansion.
8. Create and inspect a filename containing spaces.
9. Demonstrate the difference between single and double quotes.
10. Use comments to explain the important operations.

### Example structure

```bash
#!/bin/bash

# Shell expansion practice

name="Abbas"

echo "Name: ${name}"
echo "Name length: ${#name}"

username=$(whoami)
kernel=$(uname -r)

echo "Username: $username"
echo "Kernel: $kernel"

first=20
second=10

echo "Addition: $((first + second))"
echo "Subtraction: $((first - second))"
echo "Multiplication: $((first * second))"

echo file{1,2,3}.txt

echo 'Single quotes: $name'
echo "Double quotes: $name"
```

Complete the remaining requirements yourself.

## 20. Key Terms

| Term | Meaning |
|---|---|
| Shell expansion | Replacing expressions with values |
| Parameter expansion | Retrieving or manipulating variable values |
| Command substitution | Replacing a command with its output |
| Arithmetic expansion | Performing calculations |
| Brace expansion | Generating multiple strings |
| Tilde expansion | Expanding the home-directory symbol |
| Globbing | Matching filenames using patterns |
| Single quotes | Preserve literal text |
| Double quotes | Allow selected expansions |
| Backslash | Escapes the next character |
| Word splitting | Dividing unquoted expanded text into words |
| Command injection | Unintended command execution through unsafe input handling |

## 21. Exam Summary

Remember:

- Bash performs expansion before executing commands.
- `$variable` retrieves a variable's value.
- `${variable}` provides explicit parameter boundaries.
- `$(command)` performs command substitution.
- `$((expression))` performs arithmetic expansion.
- `{1..5}` generates a sequence.
- `~` represents a home directory.
- `*` and `?` are common filename wildcards.
- Single quotes prevent variable expansion.
- Double quotes allow variable and command substitution.
- Backslashes escape special characters.
- Unquoted expansions can cause word splitting and filename expansion.
- `"$@"` preserves individual positional parameters.
- Avoid unsafe use of `eval` with untrusted input.

## 22. Conclusion

Shell expansion and quoting are fundamental Bash concepts.

They explain how Linux interprets variables, commands, mathematical expressions, filenames, and special characters.

Mastering these concepts will help you build reliable scripts and avoid common security mistakes.

**Next Lesson: Lesson 13 — Input, Output, Redirection and Pipes.**