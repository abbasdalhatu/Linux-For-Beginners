---
---

# MODULE 06 – FILE AND DIRECTORY MANAGEMENT

## LESSON 06: COPYING FILES WITH CP

### 1. Introduction

Copying files is an important Linux file-management operation.

It allows users to duplicate information without removing the original file.

For example, you may need to:

- Create backups of important documents.
- Duplicate configuration files before editing them.
- Copy scripts into project directories.
- Preserve investigation evidence.
- Organise cybersecurity reports.

Linux provides the `cp` command for copying files and directories.

`cp` means **Copy**.

### 2. Basic Syntax

```bash
cp [OPTIONS] SOURCE DESTINATION
```

Explanation:

- `cp` is the command.
- `OPTIONS` modify its behaviour.
- `SOURCE` is the original file or directory.
- `DESTINATION` is where the copy will be placed.

### 3. Copying a Single File

Create a sample file:

```bash
echo "Linux file management practice" > notes.txt
```

Copy it:

```bash
cp notes.txt notes-backup.txt
```

Verify:

```bash
ls -l
```

You should see:

```text
notes.txt
notes-backup.txt
```

Display both files:

```bash
cat notes.txt
cat notes-backup.txt
```

Both should contain the same text.

The original file remains unchanged.

### 4. Copying a File into a Directory

Create a destination directory:

```bash
mkdir backups
```

Copy the file into it:

```bash
cp notes.txt backups/
```

Verify:

```bash
ls backups/
```

The copied file should be inside `backups`.

### 5. Copying Multiple Files

You can copy multiple files into one destination directory.

Example:

```bash
touch report.txt evidence.txt script.sh
```

Copy them:

```bash
cp report.txt evidence.txt script.sh backups/
```

Verify:

```bash
ls backups/
```

All three files should appear in the destination directory.

### 6. Copying Directories with `-r`

By default, `cp` does not copy directories recursively.

To copy a directory and its contents, use `-r`.

Example:

```bash
mkdir -p project/documents
touch project/documents/notes.txt
```

Copy the directory:

```bash
cp -r project project-backup
```

Verify:

```bash
ls -R project-backup
```

The `-r` option means recursive copying.

It copies the directory and its contents, including subdirectories.

### 7. Important `cp` Options

| Option | Purpose |
|---|---|
| `-r` | Copies directories recursively |
| `-i` | Prompts before overwriting a destination |
| `-n` | Does not overwrite an existing destination |
| `-v` | Displays copying activity |
| `-p` | Preserves file mode, ownership where permitted, and timestamps |
| `-a` | Archive mode; preserves attributes and copies recursively |

Examples:

**Interactive copying:**

```bash
cp -i notes.txt backups/
```

Linux may ask before replacing an existing destination file.

**Verbose copying:**

```bash
cp -v notes.txt backups/
```

**Copy without overwriting:**

```bash
cp -n notes.txt backups/
```

**Preserve file attributes:**

```bash
cp -p notes.txt backups/notes-preserved.txt
```

**Archive copying:**

```bash
cp -a project project-archive
```

Archive mode is useful when preserving directory structure and file metadata.

### 8. Copying Files Using Absolute Paths

Example:

```bash
cp /home/kali/notes.txt /home/kali/backups/
```

This uses complete paths beginning from `/`.

The source and destination must exist as required, and you must have suitable permissions.

### 9. Copying Files Using Relative Paths

Suppose your current directory is:

```text
/home/kali/module06-lab
```

You can execute:

```bash
cp notes.txt backups/
```

Linux copies the file relative to your current location.

Check your current directory with:

```bash
pwd
```

### 10. Understanding Overwriting

One important risk of `cp` is accidentally replacing an existing file.

For example:

```bash
cp new-report.txt report.txt
```

If `report.txt` already exists, its contents may be replaced.

Safer alternatives include:

```bash
cp -i new-report.txt report.txt
```

or:

```bash
cp -n new-report.txt report.txt
```

Remember that shell aliases can change default behaviour on some systems. Do not rely on an alias as your only protection against overwriting.

### 11. Practical Demonstration

Enter your lab:

```bash
cd ~/module06-lab
```

Create a practice directory:

```bash
mkdir -p cp-practice
cd cp-practice
```

Create a sample file:

```bash
echo "This is my original file." > original.txt
```

Create a copy:

```bash
cp original.txt copy.txt
```

Display both:

```bash
cat original.txt
cat copy.txt
```

Create a backup directory:

```bash
mkdir backups
```

Copy the file into it:

```bash
cp original.txt backups/
```

Create a directory structure:

```bash
mkdir -p project/documents
echo "Project notes" > project/documents/notes.txt
```

Copy the entire project:

```bash
cp -r project project-backup
```

Inspect the result:

```bash
ls -R
```

### 12. Cybersecurity Applications

**A. Configuration backups**

Copy configuration files before making authorised changes.

**B. Evidence preservation**

Create working copies of files so the original evidence remains untouched.

For formal forensic work, ordinary copying may not preserve all metadata or establish forensic integrity. Use approved forensic acquisition methods when required.

**C. Script management**

Copy reusable scripts into different project directories.

**D. Report organisation**

Maintain separate copies of reports for different investigations.

**E. Recovery preparation**

Create backups before making changes to important files.

### 13. Safety Rules

1. Confirm the source and destination before copying.
2. Use `cp -i` when you want overwrite confirmation.
3. Use `cp -n` when existing destination files must not be replaced.
4. Be careful when copying directories recursively.
5. Do not copy sensitive files into publicly accessible directories.
6. Preserve original investigation evidence.
7. Do not assume ordinary copying preserves every forensic property.

### 14. Knowledge Check

Answer these questions.

1. What does `cp` stand for?
2. Write the general syntax of the `cp` command.
3. How do you copy one file into another file?
4. How do you copy multiple files into a directory?
5. What does the `-r` option do?
6. What is the difference between `cp -i` and `cp -n`?
7. What is the purpose of `cp -v`?
8. What does archive mode (`-a`) provide?
9. Why is copying important when working with investigation evidence?
10. What could happen if you copy a file over an existing file?

### 15. Practical Exercises

Perform these exercises inside your Kali Linux VM.

Enter your lab:

```bash
cd ~/module06-lab
```

**Exercise 1:** Create a directory called `copy-practice`.

**Exercise 2:** Enter it and create a file named `original.txt`.

**Exercise 3:** Write this text into the file:

```text
Learning Linux file copying.
```

**Exercise 4:** Copy the file into `duplicate.txt`.

**Exercise 5:** Create a directory called `backup`.

**Exercise 6:** Copy both files into the `backup` directory.

**Exercise 7:** Create a nested directory structure:

```text
project/
└── reports/
    └── final.txt
```

**Exercise 8:** Copy the entire `project` directory into `project-backup` using `cp -r`.

**Exercise 9:** Use `ls -R` to inspect your directory structure.

**Exercise 10:** Explain the difference between copying a file and copying a directory recursively.

### 16. Lesson Summary

In this lesson, you learned:

- The purpose and syntax of `cp`.
- How to copy single and multiple files.
- How to copy files into directories.
- How to copy directories recursively.
- How to use `-r`, `-i`, `-n`, `-v`, `-p`, and `-a`.
- How to avoid accidental overwriting.
- How copying supports backups and cybersecurity investigations.

**Next Lesson:** Moving and Renaming Files with `mv`.

**End of Lesson 06.**