# Module 06 – File and Directory Management

## Lesson 14: Wildcards and File Patterns

### 1. Learning Objectives

By the end of this lesson, you should be able to:

- Understand Linux wildcards.
- Use the asterisk (`*`) wildcard.
- Use the question mark (`?`) wildcard.
- Use character sets with square brackets.
- Match multiple filenames using patterns.
- Combine wildcards with Linux commands.
- Use wildcards safely during system administration and cybersecurity tasks.

### 2. Introduction to Wildcards

Imagine you have 100 files in a directory.

You want to list all files ending in `.txt`.

You could type every filename individually, but that would take a long time.

Linux provides **wildcards** to match filenames based on patterns.

Wildcards are special characters that represent one or more characters in filenames.

They make file management faster and easier.

### 3. Understanding the Three Main Wildcards

Linux commonly uses three filename pattern characters:

| Wildcard | Meaning |
|---|---|
| `*` | Matches zero or more characters. |
| `?` | Matches exactly one character. |
| `[ ]` | Matches one character from a specified set or range. |

Let's understand each one.

### 4. The Asterisk (`*`) Wildcard

The asterisk matches zero or more characters.

Example:

```bash
ls *.txt
```

This lists files ending in `.txt`.

Suppose your directory contains:

```text
notes.txt
report.txt
image.png
script.sh
security.txt
```

Running:

```bash
ls *.txt
```

Displays:

```text
notes.txt
report.txt
security.txt
```

Another example:

```bash
ls report*
```

This matches filenames beginning with `report`.

Possible results:

```text
report.txt
report.pdf
report-backup.txt
```

### 5. The Question Mark (`?`) Wildcard

The question mark matches exactly one character.

Example:

```bash
ls file?.txt
```

This can match:

```text
file1.txt
file2.txt
fileA.txt
```

It does not match:

```text
file10.txt
file.txt
```

Why?

Because `?` represents exactly one character.

Another example:

```bash
ls ???.txt
```

This matches `.txt` filenames with exactly three characters before the extension.

### 6. Character Sets (`[ ]`)

Square brackets match one character from a specified set.

Example:

```bash
ls file[123].txt
```

This matches:

```text
file1.txt
file2.txt
file3.txt
```

It does not match `file4.txt`.

You can also specify a range.

Example:

```bash
ls file[1-5].txt
```

This matches filenames with one digit from 1 to 5 in that position.

Another example:

```bash
ls report[a-c].txt
```

This matches:

```text
reporta.txt
reportb.txt
reportc.txt
```

### 7. Using Wildcards with Linux Commands

Wildcards can be combined with many Linux commands.

#### A. Listing files

```bash
ls *.txt
```

Lists text files.

#### B. Copying files

```bash
cp *.txt Backups/
```

Copies matching text files into the `Backups` directory.

#### C. Moving files

```bash
mv *.pdf Documents/
```

Moves matching PDF files into `Documents`.

#### D. Removing files

```bash
rm *.tmp
```

Removes files ending in `.tmp` from the current directory.

**Warning:** Always inspect wildcard matches before using destructive commands such as `rm`.

### 8. Combining Wildcards

You can combine wildcard characters.

Example:

```bash
ls report*.txt
```

This matches filenames that begin with `report` and end with `.txt`.

Another example:

```bash
ls file?.*
```

This matches filenames beginning with `file`, followed by exactly one character, then a dot and an extension.

### 9. Understanding Hidden Files

Linux filenames beginning with a dot (`.`) are hidden by default.

Example:

```text
.config
.bashrc
.profile
```

The command:

```bash
ls
```

normally does not display hidden files.

To display them:

```bash
ls -a
```

The pattern:

```bash
ls .*
```

matches hidden names beginning with a dot, but may also match the special entries `.` and `..`.

For a safer way to list hidden files in the current directory, use:

```bash
ls -ld .[!.]*
```

This avoids matching `.` and `..` and includes hidden names with at least two characters.

### 10. Important: Wildcards Are Not Regular Expressions

Wildcards and regular expressions are different.

For example:

- `*.txt` is a shell wildcard pattern.
- `.*\.txt` is a regular expression pattern in many regex tools.

Do not confuse them.

Wildcards are commonly expanded by the shell before the command receives its arguments.

### 11. Practical Linux Lab

We will create several files and practise matching them.

#### Step 1: Create the practice directory

```bash
mkdir -p ~/module06-lab/wildcards
cd ~/module06-lab/wildcards
```

#### Step 2: Create sample files

```bash
touch notes.txt report.txt security.txt image.png photo.jpg script.sh
touch file1.txt file2.txt file3.txt file10.txt
touch report1.pdf report2.pdf report3.pdf
touch temporary.tmp backup.tmp
```

#### Step 3: List all text files

```bash
ls *.txt
```

Identify the matching files.

#### Step 4: List files beginning with `report`

```bash
ls report*
```

#### Step 5: Practise the question mark

```bash
ls file?.txt
```

Observe that `file10.txt` is not matched.

#### Step 6: Practise character sets

```bash
ls file[1-3].txt
```

Then:

```bash
ls report[1-3].pdf
```

#### Step 7: Practise copying with wildcards

Create a destination directory:

```bash
mkdir TextBackups
```

Copy all text files:

```bash
cp *.txt TextBackups/
```

Verify:

```bash
ls TextBackups/
```

#### Step 8: Practise moving with wildcards

Create another directory:

```bash
mkdir PDFReports
```

Move the PDF files:

```bash
mv *.pdf PDFReports/
```

Verify:

```bash
ls PDFReports/
```

#### Step 9: Practise hidden files

Create a hidden file:

```bash
touch .secret-notes
```

List ordinary files:

```bash
ls
```

Now display hidden files:

```bash
ls -a
```

You should see `.secret-notes`.

#### Step 10: Practise safe wildcard deletion

First list the temporary files:

```bash
ls *.tmp
```

Then remove them interactively:

```bash
rm -i *.tmp
```

Confirm each deletion.

Finally, inspect your directory:

```bash
ls -la
```

### 12. Cybersecurity Applications

Wildcards are useful in cybersecurity and system administration.

**A. Searching for Log Files**

```bash
ls /var/log/*.log
```

Lists matching log files, if present.

**B. Managing Investigation Evidence**

```bash
cp *.txt EvidenceBackup/
```

Copies matching text evidence files into a backup directory.

**C. Finding Temporary Files**

```bash
find . -name "*.tmp"
```

This uses `find` rather than shell wildcard expansion to search recursively for matching filenames.

**D. Managing Configuration Files**

```bash
ls /etc/*.conf
```

Lists matching configuration files directly inside `/etc`.

**E. Security Caution**

A wildcard can match more files than expected.

Before deleting, moving, or changing permissions on multiple files, inspect the matches first.

### 13. Common Mistakes

| Mistake | Explanation |
|---|---|
| Confusing `*` and `?` | `*` matches zero or more characters; `?` matches exactly one. |
| Forgetting file extensions | A pattern may not match the intended filenames. |
| Ignoring hidden files | Ordinary `ls` does not show them by default. |
| Using destructive wildcards carelessly | Many files may be affected. |
| Confusing wildcards with regular expressions | They use different pattern rules. |

### 14. Knowledge Check

Answer these questions:

1. What is a wildcard in Linux?
2. What does `*` represent?
3. What does `?` represent?
4. What is the purpose of square brackets `[ ]`?
5. What does `ls *.txt` display?
6. What does `ls file?.txt` match?
7. How do you display hidden files?
8. Why should you inspect wildcard matches before using `rm`?
9. What is the difference between wildcards and regular expressions?
10. Which command can recursively search for files matching a filename pattern?

### 15. Lesson Summary

In this lesson, you learned:

- Wildcards match filenames using patterns.
- `*` matches zero or more characters.
- `?` matches exactly one character.
- `[ ]` matches characters from a specified set or range.
- Wildcards can be used with `ls`, `cp`, `mv`, and `rm`.
- Hidden files begin with a dot.
- `ls -a` displays hidden files.
- Wildcards must be used carefully with destructive commands.

**Next Lesson:** Lesson 15 – Module 06 Assessment.