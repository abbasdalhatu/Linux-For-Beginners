# Lesson 08: The `echo` and `printf` Commands

## 1. Introduction

Linux provides commands for displaying text and formatted output in the terminal.

Two important commands are:

1. `echo`
2. `printf`

Both commands can display information, but they behave differently.

- `echo` is commonly used for simple text output.
- `printf` provides more precise control over formatting.

These commands are especially useful when writing Bash scripts, displaying messages, creating reports, and automating tasks.

## 2. Understanding the `echo` Command

The `echo` command displays text or variable values in the terminal.

### Basic Syntax

```bash
echo [OPTION] [STRING]
```

Example:

```bash
echo Hello World
```

Output:

```text
Hello World
```

The shell passes the words to `echo`, which displays them.

### Displaying a Message

```bash
echo Welcome to Linux
```

Output:

```text
Welcome to Linux
```

### Displaying Multiple Lines

You can use separate `echo` commands:

```bash
echo First line
echo Second line
echo Third line
```

Output:

```text
First line
Second line
Third line
```

Each command normally ends with a newline.

## 3. Using `echo` with Variables

A variable stores information that can be reused.

Example:

```bash
name="Kali Linux"
echo $name
```

Output:

```text
Kali Linux
```

You can also use braces:

```bash
echo ${name}
```

Braces help distinguish a variable name from surrounding text.

Example:

```bash
name="Abbas"
echo "Welcome, ${name}"
```

Output:

```text
Welcome, Abbas
```

### Displaying System Information

```bash
echo $USER
echo $HOME
echo $SHELL
```

These commands display the values of commonly used environment variables.

The exact output depends on your system and shell environment.

## 4. Important `echo` Options

### 4.1 The `-n` Option

By default, `echo` adds a newline after displaying text.

The `-n` option suppresses that newline.

```bash
echo -n "Enter your username: "
```

The terminal cursor remains on the same line.

This is useful when displaying prompts.

### 4.2 The `-e` Option

The `-e` option enables interpretation of certain backslash escape sequences in implementations that support it.

Example:

```bash
echo -e "First line\nSecond line"
```

Output:

```text
First line
Second line
```

Common escape sequences include:

| Escape sequence | Meaning |
|---|---|
| `\n` | Newline |
| `\t` | Horizontal tab |
| `\\` | Backslash |
| `\r` | Carriage return |
| `\b` | Backspace |

**Note:** `echo` behaviour can vary between shells and implementations. For predictable formatting, prefer `printf`.

## 5. Using `echo` to Display Special Characters

Example:

```bash
echo -e "Name:\tAbbas"
echo -e "Course:\tLinux for Ethical Hackers"
```

Output:

```text
Name:   Abbas
Course: Linux for Ethical Hackers
```

You can use escape sequences to make terminal output easier to read.

## 6. Introduction to `printf`

The `printf` command displays formatted output.

Unlike `echo`, it gives you more control over how values are displayed.

### Basic Syntax

```bash
printf FORMAT [ARGUMENTS]
```

Example:

```bash
printf "Hello World\n"
```

Output:

```text
Hello World
```

Notice that we explicitly included `\n`.

Unlike `echo`, `printf` does not automatically add a newline.

## 7. Understanding Format Specifiers

A format specifier tells `printf` how to display a value.

Common format specifiers include:

| Specifier | Meaning |
|---|---|
| `%s` | String |
| `%d` | Decimal integer |
| `%i` | Integer |
| `%f` | Floating-point number |
| `%c` | Single character |
| `%%` | Percent sign |

### 7.1 Displaying a String

```bash
printf "Name: %s\n" "Abbas"
```

Output:

```text
Name: Abbas
```

### 7.2 Displaying an Integer

```bash
printf "Age: %d\n" 21
```

Output:

```text
Age: 21
```

### 7.3 Displaying a Decimal Number

```bash
printf "Score: %.2f\n" 95.567
```

Output:

```text
Score: 95.57
```

The `.2` means two digits after the decimal point.

### 7.4 Displaying Multiple Values

```bash
printf "Name: %s | Age: %d | Score: %.2f\n" "Abbas" 21 95.567
```

Output:

```text
Name: Abbas | Age: 21 | Score: 95.57
```

## 8. Formatting Text with Width and Precision

`printf` allows you to control spacing and alignment.

### 8.1 Minimum Field Width

```bash
printf "%10s\n" "Linux"
```

This reserves at least ten character positions for the string.

### 8.2 Left Alignment

```bash
printf "%-10s|\n" "Linux"
```

The minus sign aligns the text to the left within the field.

### 8.3 Decimal Precision

```bash
printf "%.2f\n" 3.14159
```

Output:

```text
3.14
```

### 8.4 Combining Formatting Options

```bash
printf "%-15s %10s\n" "Name" "Score"
printf "%-15s %10d\n" "Abbas" 95
printf "%-15s %10d\n" "Student" 88
```

Output:

```text
Name                  Score
Abbas                    95
Student                  88
```

This is useful when generating simple reports.

## 9. Using `echo` and `printf` in Bash Scripts

These commands are commonly used in automation.

### Example 1: A Simple Greeting Script

Create a file:

```bash
nano greeting.sh
```

Enter:

```bash
#!/bin/bash

name="Abbas"

echo "Welcome to Linux for Ethical Hackers"
echo "Hello, $name"
```

Save the file.

Run:

```bash
bash greeting.sh
```

### Example 2: A Formatted Information Script

Create:

```bash
nano information.sh
```

Enter:

```bash
#!/bin/bash

name="Abbas"
course="Linux for Ethical Hackers"
level=3

printf "Student: %s\n" "$name"
printf "Course: %s\n" "$course"
printf "Level: %d\n" "$level"
```

Run:

```bash
bash information.sh
```

Expected output:

```text
Student: Abbas
Course: Linux for Ethical Hackers
Level: 3
```

## 10. Redirecting Output into Files

You can use `echo` and `printf` to write text into files using output redirection.

### 10.1 Using `echo`

```bash
echo "Linux is powerful" > notes.txt
```

The `>` operator creates the file or replaces its existing contents.

To append another line:

```bash
echo "Linux is useful for cybersecurity" >> notes.txt
```

The `>>` operator appends without replacing existing contents.

### 10.2 Using `printf`

```bash
printf "Name: Abbas\nCourse: Computer Science\n" > student.txt
```

Display the file:

```bash
cat student.txt
```

Expected output:

```text
Name: Abbas
Course: Computer Science
```

**Important:** Be careful with `>`. It can overwrite an existing file.

## 11. Practical Lab: Using `echo`

Perform these exercises in Kali Linux.

### Step 1: Display a Message

```bash
echo "Learning Linux Commands"
```

### Step 2: Display Your Username

```bash
echo $USER
```

### Step 3: Display Your Home Directory

```bash
echo $HOME
```

### Step 4: Display Multiple Lines

```bash
echo -e "Linux\nEthical Hacking\nCybersecurity"
```

### Step 5: Suppress the Newline

```bash
echo -n "Enter your name: "
```

Observe where the cursor remains.

### Step 6: Display Tab-Separated Information

```bash
echo -e "Name:\tAbbas\nCourse:\tComputer Science"
```

## 12. Practical Lab: Using `printf`

### Step 1: Display a String

```bash
printf "Welcome to Kali Linux\n"
```

### Step 2: Display an Integer

```bash
printf "Level: %d\n" 3
```

### Step 3: Display a Decimal

```bash
printf "Percentage: %.2f%%\n" 87.456
```

Expected output:

```text
Percentage: 87.46%
```

### Step 4: Display Multiple Values

```bash
printf "Name: %s | Level: %d\n" "Abbas" 3
```

### Step 5: Practise Alignment

```bash
printf "%-15s %10s\n" "Tool" "Category"
printf "%-15s %10s\n" "Nmap" "Networking"
printf "%-15s %10s\n" "Wireshark" "Analysis"
```

Observe how the output is aligned.

## 13. Practical Lab: Build a Small Security Information Script

Create a script:

```bash
nano system-info.sh
```

Enter:

```bash
#!/bin/bash

echo "================================"
echo "       SYSTEM INFORMATION"
echo "================================"

printf "Username: %s\n" "$USER"
printf "Home: %s\n" "$HOME"
printf "Shell: %s\n" "$SHELL"

echo "================================"
echo "Information collection complete."
```

Save and execute:

```bash
bash system-info.sh
```

Observe the output.

This is a simple example of how Bash scripts can display system information.

Do not assume that environment variables alone provide a complete or verified security assessment.

## 14. Common Mistakes

### Mistake 1: Forgetting the Newline in `printf`

Example:

```bash
printf "Hello"
printf "World"
```

Output:

```text
HelloWorld
```

Correct:

```bash
printf "Hello\n"
printf "World\n"
```

### Mistake 2: Using the Wrong Format Specifier

Example:

```bash
printf "%d\n" "Linux"
```

The `%d` specifier expects an integer, not a text string.

Use:

```bash
printf "%s\n" "Linux"
```

### Mistake 3: Accidentally Overwriting a File

This command replaces the contents of `notes.txt`:

```bash
echo "New information" > notes.txt
```

To add information without replacing existing contents, use:

```bash
echo "Additional information" >> notes.txt
```

### Mistake 4: Assuming `echo -e` Behaves Identically Everywhere

Different shells and implementations can interpret `echo` options differently.

Use `printf` when exact, predictable formatting is important.

## 15. Cybersecurity Importance

The `echo` and `printf` commands are useful in cybersecurity and system administration.

### 15.1 Displaying Script Status

```bash
echo "Starting security checks..."
```

### 15.2 Generating Reports

```bash
printf "Target: %s\nStatus: %s\n" "Lab-VM" "Completed"
```

### 15.3 Recording Information

```bash
echo "Investigation started" >> investigation.log
```

### 15.4 Creating Automation Messages

Scripts can use these commands to display progress, warnings, and completion messages.

### 15.5 Avoiding Unsafe Variable Handling

When displaying variable values, quote them:

```bash
echo "$USER"
printf "%s\n" "$HOME"
```

Quoting helps prevent unintended word splitting and pathname expansion by the shell.

Remember: output commands should not be used to expose passwords, private keys, access tokens, or other secrets.

## 16. Important Commands Summary

| Command | Purpose |
|---|---|
| `echo "Hello"` | Display text |
| `echo -n "Hello"` | Display text without a trailing newline |
| `echo -e "A\nB"` | Interpret supported escape sequences |
| `printf "Hello\n"` | Display formatted text |
| `printf "%s\n" "Linux"` | Display a string |
| `printf "%d\n" 10` | Display an integer |
| `printf "%.2f\n" 3.14159` | Display a decimal to two places |
| `echo "Text" > file.txt` | Write or replace file contents |
| `echo "Text" >> file.txt` | Append text to a file |

## 17. Knowledge Check

Answer these questions in your notebook.

1. What is the main purpose of the `echo` command?
2. What is the difference between `echo` and `printf`?
3. What does the `-n` option do?
4. What does `\n` represent?
5. What does `%s` mean in `printf`?
6. What does `%d` mean?
7. Why does `printf` require an explicit newline in many examples?
8. What is the difference between `>` and `>>`?
9. Why should variables usually be quoted when passed to output commands?
10. Why is `printf` useful when creating automated reports?

## 18. Practical Assignment

Complete the following tasks in Kali Linux.

1. Display a welcome message using `echo`.
2. Display your username and home directory.
3. Use `echo -e` to display three lines.
4. Use `echo -n` to display a prompt.
5. Use `printf` to display your name and course.
6. Display an integer and a decimal using appropriate format specifiers.
7. Create a file named `linux-notes.txt` using output redirection.
8. Append a second line without overwriting the first.
9. Create a Bash script that displays your username, home directory, and shell.
10. Use `printf` to create a small two-column report.
11. Explain three practical uses of `echo` and `printf` in cybersecurity.

## 19. Lesson Conclusion

The `echo` and `printf` commands help Linux users display and format information.

You have learned how to:

- Display text.
- Display environment variables.
- Use escape sequences.
- Format strings, integers, and decimals.
- Redirect output into files.
- Use output commands inside Bash scripts.

**Key lesson:** Use `echo` for simple messages and `printf` when you need precise formatting.

---

**End of Lesson 08 — The `echo` and `printf` Commands**