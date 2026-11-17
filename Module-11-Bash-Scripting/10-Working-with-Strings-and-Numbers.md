---
---


# Module 11 – Lesson 10: Working with Strings and Numbers

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand strings and numbers in Bash.
2. Create and manipulate string variables.
3. Measure string length.
4. Extract parts of strings.
5. Replace text inside strings.
6. Perform arithmetic operations.
7. Use integer and floating-point calculations.
8. Validate numeric input.
9. Build practical string and number processing scripts.

---

## 2. Understanding Strings in Bash

A string is a sequence of characters.

Examples:

```bash
name="Abbas"
course="Computer Science"
message="Welcome to Linux"
```

Bash uses variables to store strings.

### Displaying Strings

```bash
name="Abbas"

echo "$name"
```

Output:

```text
Abbas
```

### Combining Strings

You can combine strings by placing variables next to each other.

```bash
first="Abbas"
last="Muhammad"

fullname="$first $last"

echo "$fullname"
```

Output:

```text
Abbas Muhammad
```

This process is called string concatenation.

---

## 3. Single Quotes vs Double Quotes

Bash treats single and double quotes differently.

### Single Quotes

Single quotes preserve the literal characters inside them.

```bash
name="Abbas"

echo '$name'
```

Output:

```text
$name
```

### Double Quotes

Double quotes allow variable expansion.

```bash
name="Abbas"

echo "$name"
```

Output:

```text
Abbas
```

### Comparison

| Single Quotes | Double Quotes |
|---|---|
| Preserve literal text | Expand variables |
| Do not expand `$variable` | Expand `$variable` |
| Do not execute command substitutions | Allow command substitutions |

**Best practice:** Usually quote variables with double quotes when using them in commands.

---

## 4. Finding String Length

Bash provides a simple way to calculate the length of a string.

### Syntax

```bash
${#variable}
```

Example:

```bash
#!/bin/bash

name="Abbas"

echo "Name: $name"
echo "Length: ${#name}"
```

Output:

```text
Name: Abbas
Length: 5
```

### Practical Example

```bash
#!/bin/bash

read -r -p "Enter your username: " username

length=${#username}

echo "Username length: $length"

if [[ $length -lt 4 ]]; then
    echo "Username is too short."
else
    echo "Username length is acceptable."
fi
```

This demonstrates basic input validation.

---

## 5. Extracting Parts of a String

Bash supports substring expansion.

### Syntax

```bash
${variable:position:length}
```

- `position` identifies the starting position.
- `length` specifies how many characters to extract.

Positions start at zero.

Example:

```bash
#!/bin/bash

word="CYBERSECURITY"

echo "${word:0:5}"
echo "${word:5:4}"
```

Output:

```text
CYBER
SECU
```

### Extracting from the Beginning

```bash
text="Linux Administration"

echo "${text:0:5}"
```

Output:

```text
Linux
```

### Extracting from the End

A negative starting position counts from the end.

```bash
text="Cybersecurity"

echo "${text: -4}"
```

Output:

```text
rity
```

Notice the space before `-4`.

---

## 6. Replacing Text in Strings

Bash can replace matching text inside a variable.

### Replace the First Match

Syntax:

```bash
${variable/old/new}
```

Example:

```bash
message="I like Windows"

updated="${message/Windows/Linux}"

echo "$updated"
```

Output:

```text
I like Linux
```

### Replace All Matches

Syntax:

```bash
${variable//old/new}
```

Example:

```bash
message="red blue red green red"

updated="${message//red/black}"

echo "$updated"
```

Output:

```text
black blue black green black
```

---

## 7. Changing Letter Case

Bash supports case conversion.

### Convert to Uppercase

```bash
name="abbas"

echo "${name^^}"
```

Output:

```text
ABBAS
```

### Convert to Lowercase

```bash
name="LINUX"

echo "${name,,}"
```

Output:

```text
linux
```

### Capitalize the First Character

```bash
name="linux"

echo "${name^}"
```

Output:

```text
Linux
```

These operations are useful when standardizing user input.

---

## 8. Removing Parts of Strings

Bash supports parameter expansion for removing matching patterns.

### Remove the Shortest Match from the Beginning

```bash
filename="report.txt"

echo "${filename#*.}"
```

Output:

```text
txt
```

### Remove the Longest Match from the Beginning

```bash
path="/home/abbas/documents/report.txt"

echo "${path##*/}"
```

Output:

```text
report.txt
```

### Remove the Shortest Match from the End

```bash
filename="report.txt"

echo "${filename%.*}"
```

Output:

```text
report
```

### Remove the Longest Match from the End

```bash
filename="archive.backup.tar.gz"

echo "${filename%%.*}"
```

Output:

```text
archive
```

These operations are useful when extracting filenames and extensions.

---

## 9. Understanding Numbers in Bash

Bash supports integer arithmetic.

Common arithmetic operators include:

| Operator | Meaning |
|---|---|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Integer division |
| `%` | Modulus (remainder) |
| `**` | Exponentiation |

### Example

```bash
a=20
b=10

echo $((a + b))
echo $((a - b))
echo $((a * b))
echo $((a / b))
echo $((a % b))
echo $((a ** 2))
```

Output:

```text
30
10
200
2
0
400
```

### Understanding Modulus

Modulus returns the remainder after division.

Example:

```bash
echo $((10 % 3))
```

Output:

```text
1
```

Because 10 divided by 3 leaves a remainder of 1.

---

## 10. Integer Division

Bash integer division discards the fractional part.

Example:

```bash
echo $((10 / 3))
```

Output:

```text
3
```

It does not produce `3.3333`.

For floating-point calculations, use a suitable external tool.

---

## 11. Floating-Point Calculations with bc

The `bc` utility supports decimal arithmetic.

Check whether it is installed:

```bash
command -v bc
```

If it is not installed, you can install it in Kali:

```bash
sudo apt update
sudo apt install bc
```

### Example: Decimal Addition

```bash
echo "10.5 + 5.2" | bc
```

Output:

```text
15.7
```

### Example: Division

```bash
echo "10 / 3" | bc -l
```

The `-l` option loads the mathematical library and provides decimal precision.

### Example: Set Decimal Precision

```bash
echo "scale=2; 10 / 3" | bc
```

Output:

```text
3.33
```

---

## 12. Arithmetic Using Variables

You can perform calculations with variables.

Example:

```bash
#!/bin/bash

a=50
b=25

result=$((a + b))

echo "First number: $a"
echo "Second number: $b"
echo "Total: $result"
```

### Incrementing a Number

```bash
counter=1

((counter++))

echo "$counter"
```

Output:

```text
2
```

### Decrementing a Number

```bash
counter=10

((counter--))

echo "$counter"
```

Output:

```text
9
```

---

## 13. Validating Numeric Input

Never assume user input is a valid number.

Use Bash's regular-expression matching to validate integer input.

Example:

```bash
#!/bin/bash

read -r -p "Enter an integer: " number

if [[ "$number" =~ ^-?[0-9]+$ ]]; then
    echo "Valid integer: $number"
else
    echo "Invalid input."
fi
```

Explanation:

- `^` means beginning of the input.
- `-?` allows an optional minus sign.
- `[0-9]+` requires one or more digits.
- `$` means end of the input.

This accepts positive integers, negative integers, and zero.

### Valid Examples

```text
25
-10
0
1000
```

### Invalid Examples

```text
hello
12.5
abc123
```

This pattern validates integers, not decimal numbers.

---

## 14. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson10
cd ~/module11-lab/lesson10
```

### Step 2: Create a String Information Script

```bash
nano string-info.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter a word or sentence: " text

echo "Original: $text"
echo "Length: ${#text}"
echo "Uppercase: ${text^^}"
echo "Lowercase: ${text,,}"
echo "First five characters: ${text:0:5}"
```

Execute:

```bash
chmod +x string-info.sh
./string-info.sh
```

Test with:

```text
Linux Administration
Cybersecurity
Bash Scripting
```

### Step 3: Create a String Replacement Script

```bash
nano string-replace.sh
```

Enter:

```bash
#!/bin/bash

message="Linux is useful. Linux is powerful."

updated="${message//Linux/Bash}"

echo "Original: $message"
echo "Updated: $updated"
```

Execute:

```bash
chmod +x string-replace.sh
./string-replace.sh
```

### Step 4: Create a Calculator Script

```bash
nano number-calculator.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter first integer: " first
read -r -p "Enter second integer: " second

if [[ ! "$first" =~ ^-?[0-9]+$ ]] || [[ ! "$second" =~ ^-?[0-9]+$ ]]; then
    echo "Error: Both inputs must be integers."
    exit 1
fi

echo "Addition: $((first + second))"
echo "Subtraction: $((first - second))"
echo "Multiplication: $((first * second))"

if [[ $second -ne 0 ]]; then
    echo "Division: $((first / second))"
    echo "Remainder: $((first % second))"
else
    echo "Division is not possible because the second number is zero."
fi
```

Execute:

```bash
chmod +x number-calculator.sh
./number-calculator.sh
```

Test with:
- `20` and `5`
- `15` and `4`
- `10` and `0`
- `hello` and `5`

### Step 5: Create a Filename Information Script

```bash
nano filename-info.sh
```

Enter:

```bash
#!/bin/bash

read -r -p "Enter a filename: " filename

if [[ -z "$filename" ]]; then
    echo "Filename cannot be empty."
    exit 1
fi

basename="${filename##*/}"
extension="${basename##*.}"
name="${basename%.*}"

echo "Full path: $filename"
echo "Filename: $basename"
echo "Name without extension: $name"
echo "Extension: $extension"
```

Execute:

```bash
chmod +x filename-info.sh
./filename-info.sh
```

Try:

```text
/home/abbas/report.txt
```

Note: This is a simple demonstration. Filenames without extensions and filenames beginning with a dot need special handling in a production-quality filename parser.

---

## 15. Practical Cybersecurity Project: Log Keyword Counter

Security teams often examine logs for suspicious keywords.

Create:

```bash
nano log-counter.sh
```

Enter:

```bash
#!/bin/bash

logfile="$1"

if [[ -z "$logfile" ]]; then
    echo "Usage: $0 logfile"
    exit 1
fi

if [[ ! -f "$logfile" || ! -r "$logfile" ]]; then
    echo "Error: File does not exist or cannot be read."
    exit 1
fi

echo "========== LOG KEYWORD REPORT =========="

for keyword in ERROR WARNING FAILED
do
    count=$(grep -Fic -- "$keyword" "$logfile")

    echo "$keyword occurrences: $count"
done

echo "========================================"
```

Create a sample log:

```bash
nano sample.log
```

Enter:

```text
INFO: System started
WARNING: Disk usage is high
ERROR: Application failed to start
INFO: User logged in
FAILED: Authentication attempt
WARNING: Network connection unstable
ERROR: Service unavailable
```

Run:

```bash
chmod +x log-counter.sh
./log-counter.sh sample.log
```

Expected counts:

```text
ERROR occurrences: 2
WARNING occurrences: 2
FAILED occurrences: 1
```

### Cybersecurity Application

This demonstrates basic log analysis.

Security analysts use text processing to identify:
- Failed authentication attempts.
- Application errors.
- System warnings.
- Repeated suspicious events.

This is a basic keyword counter, not a complete intrusion detection system.

---

## 16. Common Errors

| Error | Explanation | Solution |
|---|---|---|
| Incorrect quotes | String expansion behaves unexpectedly | Use appropriate quotes |
| Unquoted variables | Spaces may cause problems | Use `"$variable"` |
| Integer division confusion | Decimal portion is discarded | Use `bc` |
| Invalid arithmetic input | Input is not an integer | Validate before calculating |
| Division by zero | Divisor is zero | Check the divisor |
| Incorrect substring position | String indexing starts at zero | Check positions |
| Unexpected replacement | Pattern matches more than expected | Review parameter expansion |

### Debugging Commands

Check syntax:

```bash
bash -n number-calculator.sh
```

Trace execution:

```bash
bash -x number-calculator.sh
```

---

## 17. Lesson Summary

In this lesson, we learned:

- Strings store text.
- Numbers can be used in arithmetic operations.
- Single and double quotes behave differently.
- `${#variable}` calculates string length.
- `${variable:start:length}` extracts a substring.
- `${variable/old/new}` replaces the first match.
- `${variable//old/new}` replaces all matches.
- `${variable^^}` converts text to uppercase.
- `${variable,,}` converts text to lowercase.
- `$(( ))` performs integer arithmetic.
- `bc` performs decimal calculations.
- Regular expressions can validate integer input.
- String processing is useful for log analysis and automation.

### Important Syntax

| Syntax | Purpose |
|---|---|
| `${#name}` | String length |
| `${name:0:5}` | Extract substring |
| `${name/old/new}` | Replace first match |
| `${name//old/new}` | Replace all matches |
| `${name^^}` | Uppercase |
| `${name,,}` | Lowercase |
| `${name#pattern}` | Remove shortest prefix match |
| `${name##pattern}` | Remove longest prefix match |
| `${name%pattern}` | Remove shortest suffix match |
| `${name%%pattern}` | Remove longest suffix match |
| `$((a + b))` | Integer addition |
| `$((a * b))` | Integer multiplication |
| `scale=2` | Set decimal precision in `bc` |

---

## 18. Knowledge Check

Answer these questions in your notebook:

1. What is a string?
2. Explain the difference between single and double quotes.
3. How do you calculate string length?
4. What does `${text:0:5}` do?
5. Differentiate `${text/old/new}` and `${text//old/new}`.
6. How do you convert a string to uppercase?
7. Why does Bash integer division discard decimal values?
8. What is the purpose of `bc`?
9. How can you validate integer input?
10. How can string processing help cybersecurity analysts?

### Practical Assessment

Successfully create and execute:

1. `string-info.sh`
2. `string-replace.sh`
3. `number-calculator.sh`
4. `filename-info.sh`
5. `log-counter.sh`

Test your scripts using valid and invalid inputs.

**Lesson 10 is complete when all five scripts work correctly.**
