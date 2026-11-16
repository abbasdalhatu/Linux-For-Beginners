
# Module 11 – Lesson 11: File Operations and Testing

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand file operations in Bash.
2. Create, read, copy, move, and delete files.
3. Work with directories.
4. Append and overwrite file contents.
5. Check file properties and permissions.
6. Use redirection and file descriptors.
7. Test whether files and directories exist.
8. Build practical file-management automation scripts.

---

## 2. Introduction to File Operations

File operations are actions performed on files and directories.

Examples include:

- Creating files.
- Reading file contents.
- Writing information.
- Copying files.
- Moving files.
- Renaming files.
- Deleting files.
- Checking permissions.

### Why Are File Operations Important?

Linux administrators and cybersecurity professionals frequently work with files.

Examples:

- Managing configuration files.
- Reviewing system logs.
- Creating backups.
- Collecting security reports.
- Checking file permissions.
- Organizing investigation evidence.

Bash allows many of these tasks to be automated.

---

## 3. Creating Files

The `touch` command creates an empty file if it does not already exist.

### Example

```bash
touch report.txt
```

Check that it exists:

```bash
ls -l report.txt
```

### Creating Multiple Files

```bash
touch file1.txt file2.txt file3.txt
```

### Creating a File with Content

Use output redirection:

```bash
echo "Welcome to Linux" > welcome.txt
```

Read it:

```bash
cat welcome.txt
```

Output:

```text
Welcome to Linux
```

**Important:** `touch` also updates timestamps when the file already exists. It does not erase the file's contents.

---

## 4. Reading Files

Linux provides several commands for reading files.

| Command | Purpose |
|---|---|
| `cat` | Displays file contents |
| `less` | Reads files page by page |
| `more` | Displays content one page at a time |
| `head` | Displays the beginning |
| `tail` | Displays the end |
| `wc` | Counts lines, words, and bytes |

### Example 1: cat

```bash
cat report.txt
```

### Example 2: head

```bash
head report.txt
```

Display the first five lines:

```bash
head -n 5 report.txt
```

### Example 3: tail

```bash
tail report.txt
```

Display the last five lines:

```bash
tail -n 5 report.txt
```

### Example 4: less

```bash
less report.txt
```

Useful navigation keys:

- `Space`: Next page.
- `b`: Previous page.
- `/word`: Search.
- `q`: Quit.

### Example 5: wc

```bash
wc report.txt
```

Display only the number of lines:

```bash
wc -l report.txt
```

---

## 5. Writing and Appending to Files

Bash supports output redirection.

Two important operators are:

- `>` overwrites a file.
- `>>` appends to a file.

### A. Overwriting a File

```bash
echo "First line" > notes.txt
```

If the file already contains information, this replaces its contents.

### B. Appending to a File

```bash
echo "Second line" >> notes.txt
```

This adds information to the end without removing existing contents.

### Example

```bash
echo "Linux Fundamentals" > course.txt
echo "Bash Scripting" >> course.txt
echo "Cybersecurity" >> course.txt
```

Read the file:

```bash
cat course.txt
```

Output:

```text
Linux Fundamentals
Bash Scripting
Cybersecurity
```

---

## 6. Using printf to Write Files

The `printf` command provides more control over formatting.

Example:

```bash
printf "Name: Abbas\nCourse: Computer Science\nLevel: 3\n" > student.txt
```

Read the file:

```bash
cat student.txt
```

You can also append formatted information:

```bash
printf "Status: Active\n" >> student.txt
```

---

## 7. Copying Files

The `cp` command copies files and directories.

### Copy a File

```bash
cp report.txt report-backup.txt
```

### Copy to Another Directory

```bash
mkdir backups
cp report.txt backups/
```

### Copy Multiple Files

```bash
cp file1.txt file2.txt backups/
```

### Copy a Directory

Use `-r` for recursive copying:

```bash
cp -r documents documents-backup
```

**Important:** Check the destination before copying. Existing files can be overwritten.

---

## 8. Moving and Renaming Files

The `mv` command moves or renames files.

### Rename a File

```bash
mv report.txt final-report.txt
```

### Move a File into a Directory

```bash
mkdir archive
mv final-report.txt archive/
```

### Rename a Directory

```bash
mv archive old-archive
```

**Difference:**

- `cp` creates a copy.
- `mv` moves or renames the original.

---

## 9. Deleting Files and Directories

The `rm` command removes files.

### Delete a File

```bash
rm unwanted.txt
```

### Delete an Empty Directory

```bash
rmdir empty-folder
```

### Delete a Directory and Its Contents

```bash
rm -r old-folder
```

**Safety warning:** Deletion commands can permanently remove data. Always verify the target path before using `rm`. Never experiment with recursive deletion on system directories.

For this course, only delete files and directories you created inside your lesson lab.

---

## 10. Working with Directories

Bash can automate directory creation and management.

### Create a Directory

```bash
mkdir project
```

### Create Nested Directories

```bash
mkdir -p project/reports/2026
```

### List Directory Contents

```bash
ls -la project
```

### Check Current Directory

```bash
pwd
```

### Change Directory

```bash
cd project
```

### Return to Home Directory

```bash
cd ~
```

---

## 11. Checking File and Directory Properties

Bash provides conditional tests for inspecting paths.

| Test | Meaning |
|---|---|
| `-e` | Path exists |
| `-f` | Regular file |
| `-d` | Directory |
| `-r` | Readable |
| `-w` | Writable |
| `-x` | Executable |
| `-s` | File is non-empty |
| `-L` | Symbolic link |

### Example

```bash
#!/bin/bash

target="report.txt"

if [[ -f "$target" ]]; then
    echo "Regular file found."
else
    echo "File not found."
fi
```

### Checking Whether a File Is Empty

```bash
if [[ -s "report.txt" ]]; then
    echo "File contains data."
else
    echo "File is empty or unavailable."
fi
```

### Checking for a Symbolic Link

```bash
if [[ -L "shortcut" ]]; then
    echo "Symbolic link detected."
fi
```

---

## 12. File Permissions and Metadata

The `stat` command displays file information.

Example:

```bash
stat report.txt
```

It displays details such as:

- File size.
- File type.
- Permissions.
- Ownership.
- Access time.
- Modification time.
- Change time.

### Display Permissions

```bash
ls -l report.txt
```

Example:

```text
-rw-r--r-- 1 kali kali 25 Nov 15 10:00 report.txt
```

The first characters represent file type and permissions.

### File Size

```bash
du -h report.txt
```

### File Type

```bash
file report.txt
```

These commands are useful when inspecting files during security investigations.

---

## 13. Working with File Descriptors

Linux uses file descriptors to identify open input and output streams.

The three standard file descriptors are:

| Descriptor | Name | Purpose |
|---|---|---|
| `0` | stdin | Standard input |
| `1` | stdout | Standard output |
| `2` | stderr | Standard error |

### Redirect Standard Output

```bash
ls -l > output.txt
```

### Redirect Standard Error

```bash
ls /nonexistent-directory 2> error.txt
```

### Redirect Both Output and Errors

```bash
command > all-output.txt 2>&1
```

This sends standard output and standard error to the same file.

A shorter Bash form is:

```bash
command &> all-output.txt
```

### Discard Output

```bash
command > /dev/null 2>&1
```

This discards both standard output and standard error.

Use `/dev/null` only when you intentionally do not need the output.

---

## 14. Reading Files Line by Line

A `while` loop can process file contents one line at a time.

Example:

```bash
#!/bin/bash

while IFS= read -r line
do
    echo "Line: $line"
done < report.txt
```

This is useful for:

- Processing log files.
- Reading configuration data.
- Analyzing lists of filenames.
- Processing exported reports.

---

## 15. Comparing Files

Linux provides tools for comparing file contents.

### A. diff

The `diff` command shows differences between two files.

```bash
diff original.txt modified.txt
```

### B. cmp

The `cmp` command compares files byte by byte.

```bash
cmp original.txt backup.txt
```

If the files are identical, `cmp` normally produces no output and returns exit status 0.

### C. sha256sum

The `sha256sum` command calculates a SHA-256 hash.

```bash
sha256sum report.txt
```

Example output:

```text
<hash-value>  report.txt
```

A hash can help verify whether file contents have changed.

To compare two files:

```bash
sha256sum original.txt backup.txt
```

Matching SHA-256 hashes provide strong evidence that the files have identical contents, assuming the hash calculation is performed correctly.

---

## 16. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson11
cd ~/module11-lab/lesson11
```

### Step 2: Create a File

```bash
touch notes.txt
```

Add content:

```bash
echo "Linux Administration" > notes.txt
echo "Bash Scripting" >> notes.txt
echo "Cybersecurity" >> notes.txt
```

Read:

```bash
cat notes.txt
```

### Step 3: Copy the File

```bash
cp notes.txt notes-backup.txt
```

Verify:

```bash
ls -l
```

### Step 4: Rename the Copy

```bash
mv notes-backup.txt archive.txt
```

Verify:

```bash
ls -l
```

### Step 5: Create a Directory

```bash
mkdir reports
```

Copy a file into it:

```bash
cp notes.txt reports/
```

Verify:

```bash
ls -l reports/
```

### Step 6: Inspect File Properties

```bash
stat notes.txt
```

Check its type:

```bash
file notes.txt
```

Check its size:

```bash
wc -c notes.txt
```

### Step 7: Compare Files

```bash
cp notes.txt original.txt
```

Compare:

```bash
cmp notes.txt original.txt
```

Calculate hashes:

```bash
sha256sum notes.txt original.txt
```

The hashes should match.

---

## 17. Practical Cybersecurity Project: File Integrity Monitor

A file integrity monitor helps detect changes in files by comparing their cryptographic hashes.

For this introductory project, we will monitor files inside your lesson lab only.

### Step 1: Create a Sample File

```bash
echo "Important training configuration" > protected-config.txt
```

### Step 2: Create the Monitoring Script

```bash
nano integrity-check.sh
```

Enter:

```bash
#!/bin/bash

target="protected-config.txt"
baseline="protected-config.sha256"

if [[ ! -f "$target" ]]; then
    echo "ERROR: Target file does not exist."
    exit 1
fi

if [[ ! -f "$baseline" ]]; then
    echo "Creating initial integrity baseline..."

    sha256sum "$target" > "$baseline"

    echo "Baseline created."
    exit 0
fi

echo "Checking file integrity..."

if sha256sum --check "$baseline"; then
    echo "INTEGRITY STATUS: File unchanged."
else
    echo "WARNING: File integrity check failed."
fi
```

### Step 3: Execute the Script

```bash
chmod +x integrity-check.sh
./integrity-check.sh
```

The first execution creates a baseline.

### Step 4: Run It Again

```bash
./integrity-check.sh
```

It should report that the file is unchanged.

### Step 5: Modify the Sample File

```bash
echo "Unexpected modification" >> protected-config.txt
```

Run the monitor again:

```bash
./integrity-check.sh
```

It should report a failed integrity check.

### What Have You Learned?

You have created a basic file integrity monitor using:

- File existence tests.
- Conditional statements.
- SHA-256 hashing.
- Output redirection.
- Exit statuses.

**Security limitation:** A baseline stored beside the monitored file can also be modified by an attacker who controls that directory. Real integrity-monitoring systems protect baselines separately and use additional controls.

---

## 18. Common File Operation Errors

| Error | Explanation | Solution |
|---|---|---|
| Permission denied | Current user lacks required access | Inspect permissions |
| No such file | Path does not exist | Check the path |
| File overwritten | Used `>` instead of `>>` | Choose the correct redirection |
| Wrong destination | Incorrect copy or move path | Verify paths first |
| Cannot read file | File is not readable | Inspect access permissions |
| Unexpected deletion | Incorrect target supplied to `rm` | Verify targets before deletion |

### Debugging Commands

Check script syntax:

```bash
bash -n integrity-check.sh
```

Trace execution:

```bash
bash -x integrity-check.sh
```

---

## 19. Lesson Summary

In this lesson, we learned:

- `touch` creates empty files.
- `cat`, `less`, `head`, and `tail` read files.
- `>` overwrites file contents.
- `>>` appends file contents.
- `cp` copies files and directories.
- `mv` moves or renames files.
- `rm` removes files.
- `mkdir` creates directories.
- `stat` displays metadata.
- `diff` and `cmp` compare files.
- `sha256sum` calculates file hashes.
- File tests check existence and permissions.
- Redirection manages standard input and output.
- File integrity monitoring helps detect unauthorized changes.

### Important Commands

| Command | Purpose |
|---|---|
| `touch` | Create an empty file |
| `cat` | Display file contents |
| `less` | Read a file interactively |
| `head` | Display the first lines |
| `tail` | Display the last lines |
| `wc` | Count lines, words, and bytes |
| `cp` | Copy files |
| `mv` | Move or rename |
| `rm` | Remove files |
| `mkdir` | Create directories |
| `rmdir` | Remove empty directories |
| `stat` | Display metadata |
| `file` | Identify file type |
| `diff` | Compare file contents |
| `cmp` | Compare files byte by byte |
| `sha256sum` | Calculate SHA-256 hashes |

---

## 20. Knowledge Check

Answer these questions in your notebook:

1. What are file operations?
2. What is the difference between `>` and `>>`?
3. Differentiate `cp` and `mv`.
4. What does the `rm` command do?
5. What is the purpose of `stat`?
6. Explain standard input, standard output, and standard error.
7. What does `2>` redirect?
8. What is the purpose of `sha256sum`?
9. How can you check whether a file exists in Bash?
10. Explain how a file integrity monitor works.

### Practical Assessment

Successfully create and execute:

1. A file containing several lines of text.
2. A backup copy of that file.
3. A renamed copy.
4. A directory containing a copied file.
5. A file comparison using `cmp`.
6. A SHA-256 hash comparison.
7. The `integrity-check.sh` project.

**Lesson 11 is complete when all practical exercises work correctly.**
