# MODULE 06 – FILE AND DIRECTORY MANAGEMENT

## LESSON 05: CREATING FILES WITH TOUCH

### 1. Introduction

Linux stores information in files. These files may contain text, scripts, configuration settings, application data, logs, and other information.

In the previous lesson, you learned how to create directories using `mkdir`.

Now, you will learn how to create files using the `touch` command.

The `touch` command is commonly used to:

- Create empty files.
- Update file timestamps.
- Create multiple files.
- Prepare files for scripts and projects.

### 2. What Is the `touch` Command?

`touch` is a Linux command used to update file timestamps.

If a file does not exist, `touch` creates an empty regular file.

Basic syntax:

```bash
touch [OPTIONS] FILE_NAME
```

Explanation:

- `touch` is the command.
- `OPTIONS` modify its behaviour.
- `FILE_NAME` specifies the file to create or update.

### 3. Creating a Single File

To create an empty file named `notes.txt`, run:

```bash
touch notes.txt
```

Verify that it exists:

```bash
ls
```

You should see:

```text
notes.txt
```

Check its file type:

```bash
file notes.txt
```

Possible output:

```text
notes.txt: empty
```

The file exists, but it contains no text.

### 4. Creating Multiple Files

You can create multiple files with one command.

Example:

```bash
touch notes.txt report.txt script.sh
```

This creates three empty files.

Verify:

```bash
ls
```

You should see:

```text
notes.txt
report.txt
script.sh
```

This is useful when preparing a project with several files.

### 5. Creating Files Inside a Directory

You can specify a directory path when creating a file.

Example:

```bash
touch documents/notes.txt
```

This creates `notes.txt` inside the `documents` directory, provided that the directory already exists.

Another example:

```bash
touch ~/module06-lab/report.txt
```

This creates a file named `report.txt` inside your Module 06 lab directory.

Remember: `touch` does not automatically create missing parent directories.

For example:

```bash
touch projects/linux/report.txt
```

will fail if the required directories do not exist.

Create them first:

```bash
mkdir -p projects/linux
touch projects/linux/report.txt
```

### 6. Creating Files Using Absolute Paths

An absolute path specifies the complete location of a file.

Example:

```bash
touch /home/kali/notes.txt
```

This creates the file inside `/home/kali`, assuming you have permission.

You can also use:

```bash
touch ~/notes.txt
```

The `~` symbol represents your home directory.

### 7. Creating Files Using Relative Paths

A relative path depends on your current working directory.

First, check your location:

```bash
pwd
```

Suppose the output is:

```text
/home/kali/module06-lab
```

Now run:

```bash
touch evidence.txt
```

The file will be created inside:

```text
/home/kali/module06-lab
```

### 8. Understanding File Timestamps

One of the important functions of `touch` is updating file timestamps.

Linux tracks several timestamps associated with files.

**A. Access Time (atime)**

Records when file contents were last accessed, subject to filesystem and mount options.

**B. Modification Time (mtime)**

Records when the file contents were last modified.

**C. Change Time (ctime)**

Records when the file's metadata or contents last changed.

The `touch` command can update timestamps without changing the actual contents of an existing file.

Inspect timestamps using:

```bash
stat notes.txt
```

You may see fields such as:

```text
Access:
Modify:
Change:
```

### 9. Understanding Important `touch` Options

| Option | Purpose |
|---|---|
| `-a` | Updates access time only |
| `-m` | Updates modification time only |
| `-c` | Does not create a missing file |
| `-t` | Sets timestamps using a specified timestamp format |
| `-r` | Uses timestamps from another reference file |

Examples:

Update only access time:

```bash
touch -a notes.txt
```

Update only modification time:

```bash
touch -m notes.txt
```

Avoid creating a missing file:

```bash
touch -c missing.txt
```

Use another file's timestamps:

```bash
touch -r notes.txt report.txt
```

Inspect the result:

```bash
stat notes.txt report.txt
```

### 10. Important Difference: `touch` and `echo`

Both commands can be used when working with files, but they serve different purposes.

**Using touch:**

```bash
touch notes.txt
```

Creates an empty file if it does not exist.

**Using echo with redirection:**

```bash
echo "Linux is powerful" > notes.txt
```

Writes text into the file and creates it if necessary.

Important:

If `notes.txt` already contains information, using `>` overwrites its contents.

To append text instead:

```bash
echo "Linux is useful for cybersecurity" >> notes.txt
```

The `>>` operator adds text without replacing the existing contents.

### 11. Practical Demonstration

Enter your Module 06 lab:

```bash
cd ~/module06-lab
```

Create a practice directory:

```bash
mkdir -p touch-practice
```

Enter it:

```bash
cd touch-practice
```

Create one empty file:

```bash
touch notes.txt
```

Create multiple files:

```bash
touch report.txt evidence.txt script.sh
```

List them:

```bash
ls -l
```

Check a file's type:

```bash
file notes.txt
```

Inspect its metadata:

```bash
stat notes.txt
```

Now write some text into the file:

```bash
echo "Linux file management practice" > notes.txt
```

Display its contents:

```bash
cat notes.txt
```

Append another line:

```bash
echo "Learning the touch command" >> notes.txt
```

Display the updated contents:

```bash
cat notes.txt
```

Inspect its timestamps again:

```bash
stat notes.txt
```

You should notice that the modification time has changed after writing to the file.

### 12. Cybersecurity Applications

The `touch` command can support different cybersecurity activities.

**A. Preparing investigation files**

Create empty files for notes, reports, and evidence documentation.

**B. Preparing scripts**

Create shell script files before writing Bash commands into them.

**C. Organising security projects**

Prepare files for network scanning results and investigation records.

**D. Understanding file metadata**

Learn how file timestamps can help analysts understand when files were accessed or modified.

**E. Lab preparation**

Create test files for practising commands such as `grep`, `find`, `sort`, and `wc`.

Important: Timestamps can be changed and are not reliable proof of when an event actually occurred. Investigators should preserve original evidence and use multiple sources when establishing a timeline.

### 13. Common Mistakes

**Mistake 1: Trying to create a file inside a missing directory**

```bash
touch projects/linux/notes.txt
```

If the parent directories do not exist, the command fails.

Solution:

```bash
mkdir -p projects/linux
touch projects/linux/notes.txt
```

**Mistake 2: Assuming `touch` writes text**

```bash
touch notes.txt
```

This creates an empty file when it does not exist. It does not write lesson content into it.

**Mistake 3: Assuming `touch` always creates a new file**

If a file already exists, `touch` normally updates its timestamps instead of replacing its contents.

### 14. Safety Rules

1. Confirm your current directory before creating files.
2. Avoid creating unnecessary files inside system directories.
3. Remember that `touch` updates timestamps on existing files.
4. Do not use file redirection carelessly because `>` can overwrite data.
5. Preserve original evidence files during cybersecurity investigations.

### 15. Knowledge Check

Answer these questions in your notebook.

1. What is the purpose of the `touch` command?
2. Write the general syntax of `touch`.
3. How do you create an empty file named `notes.txt`?
4. How can you create three files using one command?
5. Does `touch` automatically create missing parent directories?
6. What is the difference between absolute and relative file paths?
7. Explain the meaning of access time, modification time, and change time.
8. What is the purpose of `touch -c`?
9. Explain the difference between `touch` and `echo`.
10. Why are file timestamps useful during cybersecurity investigations?

### 16. Practical Exercises

Perform these exercises inside your Kali Linux VM.

**Exercise 1:** Enter your Module 06 lab.

```bash
cd ~/module06-lab
```

**Exercise 2:** Create a directory named `file-practice`.

**Exercise 3:** Enter the directory.

**Exercise 4:** Create an empty file named `notes.txt`.

**Exercise 5:** Create three additional files:

- `report.txt`
- `evidence.txt`
- `script.sh`

**Exercise 6:** Use `ls -l` to display all the files.

**Exercise 7:** Use `file` to identify the type of `script.sh`.

**Exercise 8:** Use `stat` to inspect `notes.txt`.

**Exercise 9:** Write the following text into `notes.txt`:

```text
Linux file management is important.
I am learning the touch command.
```

**Exercise 10:** Display the contents of `notes.txt`.

**Exercise 11:** Explain what happens when you execute `touch notes.txt` again after the file already exists.

### 17. Lesson Summary

In this lesson, you learned:

- What the `touch` command does.
- How to create single and multiple files.
- How to create files using absolute and relative paths.
- How to inspect file metadata with `stat`.
- The meaning of access, modification, and change timestamps.
- Important `touch` options.
- The difference between `touch` and `echo`.
- How file timestamps relate to cybersecurity investigations.

**Next Lesson:** Copying Files with `cp`.

**End of Lesson 05.**