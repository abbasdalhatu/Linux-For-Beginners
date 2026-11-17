---
---

# LESSON 09: FILE EXTENSIONS AND FILE NAMES

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

Every file in Linux has a name, but not every file needs an extension.

A file extension is the part of a filename that usually appears after the final dot.

Examples:

```text
notes.txt
photo.jpg
script.sh
document.pdf
program.py
```

File extensions help humans understand what type of content a file may contain.

However, Linux does not depend on file extensions to determine whether a file can be executed or what type of data it contains.

This is an important difference between Linux and the way many people use Windows.

## 2. Understanding File Names

A filename identifies a file or directory.

Examples:

```text
notes.txt
index.html
script.sh
.bashrc
README
```

Linux filenames are generally case-sensitive.

This means:

```text
notes.txt
Notes.txt
NOTES.txt
```

These can be three different filenames.

Linux also allows spaces and many special characters in filenames, although using simple names is usually easier.

Recommended naming style:

```text
linux-notes.txt
network-lab.sh
security-report.pdf
```

Avoid unnecessarily complicated filenames.

## 3. Understanding File Extensions

A file extension is commonly used to indicate the expected format or purpose of a file.

Common extensions include:

| Extension | Common Purpose |
|---|---|
| `.txt` | Plain text |
| `.pdf` | PDF document |
| `.jpg` | JPEG image |
| `.png` | PNG image |
| `.sh` | Shell script |
| `.py` | Python source code |
| `.html` | HTML document |
| `.css` | CSS stylesheet |
| `.js` | JavaScript source code |
| `.zip` | ZIP archive |
| `.tar` | TAR archive |
| `.gz` | Gzip-compressed data |
| `.conf` | Configuration file |

An extension is a naming convention. It does not guarantee the actual content of a file.

## 4. Linux Does Not Require File Extensions

In Linux, a file can exist without an extension.

Examples:

```text
README
LICENSE
Makefile
passwd
hostname
```

These are valid filenames.

You can create a file without an extension:

```bash
touch myfile
```

Create a file with an extension:

```bash
touch myfile.txt
```

Both are valid.

Linux identifies file types using filesystem metadata and other information, not simply the filename extension.

## 5. Case Sensitivity

Linux distinguishes uppercase and lowercase letters in filenames.

Create three files:

```bash
touch notes.txt
touch Notes.txt
touch NOTES.txt
```

List them:

```bash
ls
```

You should see three separate files.

This is important when navigating directories and writing scripts.

For example:

```bash
cat notes.txt
```

is not necessarily the same as:

```bash
cat Notes.txt
```

If the exact filename does not exist, Linux will return an error.

## 6. Files With Multiple Dots

A filename can contain multiple dots.

Examples:

```text
backup.tar.gz
project.final.pdf
archive.old.zip
```

In `backup.tar.gz`:

- `backup` is the main filename.
- `.tar` indicates a TAR archive format.
- `.gz` indicates gzip compression.

The final extension is not always enough to explain the complete file format.

You should inspect files when necessary.

## 7. Identifying the Actual File Type

Linux provides the `file` command to identify the type of data stored in a file.

Example:

```bash
file notes.txt
```

Possible output:

```text
notes.txt: ASCII text
```

Another example:

```bash
file /bin/ls
```

It may identify the program as an ELF executable for your system architecture.

The `file` command examines file contents and metadata to make its identification.

This is useful when the extension is missing or misleading.

## 8. File Extensions and Executable Permissions

A common misunderstanding is that a `.sh` extension automatically makes a file executable.

It does not.

For example:

```bash
touch script.sh
```

The file exists, but it is not automatically executable.

Inspect its permissions:

```bash
ls -l script.sh
```

You can add executable permission:

```bash
chmod u+x script.sh
```

Then inspect it again:

```bash
ls -l script.sh
```

The `x` permission indicates that the file has executable permission for the relevant user.

We will study Linux permissions in detail in Module 07.

## 9. Misleading File Extensions and Cybersecurity

File extensions are important during security investigations.

Imagine receiving a file named:

```text
invoice.pdf
```

The filename suggests that it is a PDF document.

However, the actual file might contain something completely different.

For example:

```text
invoice.pdf
```

could contain executable data despite its name.

Inspect suspicious files using:

```bash
file invoice.pdf
```

Other useful inspection commands include:

```bash
ls -l invoice.pdf
stat invoice.pdf
```

Security professionals should never trust a file solely because of its name or extension.

Do not execute unknown files simply to discover what they do.

## 10. Renaming Files

The `mv` command can rename files.

Example:

```bash
mv notes.txt linux-notes.txt
```

This changes the filename.

It does not automatically convert the file's content into another format.

For example:

```bash
mv notes.txt notes.pdf
```

does not transform a text file into a real PDF document.

It only changes the name.

## 11. Practical Laboratory

Perform these exercises in Kali Linux.

### Exercise 1: Create Files With Different Extensions

```bash
mkdir -p ~/extension-lab
cd ~/extension-lab

touch notes.txt
touch script.sh
touch program.py
touch webpage.html
touch README
```

List the files:

```bash
ls -l
```

### Exercise 2: Compare Uppercase and Lowercase

```bash
touch test.txt
touch Test.txt
touch TEST.txt
```

List them:

```bash
ls
```

Confirm that Linux treats them as different filenames.

### Exercise 3: Inspect File Types

```bash
file notes.txt
file script.sh
file program.py
file webpage.html
file README
```

Observe how the `file` command identifies each file.

### Exercise 4: Create a File With Multiple Extensions

```bash
touch backup.tar.gz
file backup.tar.gz
```

Notice that an empty file with this name is not automatically a real compressed TAR archive.

### Exercise 5: Rename a File

```bash
mv notes.txt linux-notes.txt
ls
```

Inspect the renamed file:

```bash
file linux-notes.txt
```

### Exercise 6: Understand Extensions and Executable Permissions

```bash
touch example.sh
ls -l example.sh
chmod u+x example.sh
ls -l example.sh
```

Observe the difference in permissions.

Remember: executable permission does not guarantee that a file contains a valid or safe program.

### Exercise 7: Clean Up

Return to your home directory:

```bash
cd ~
```

Remove the laboratory directory you created:

```bash
rm -r extension-lab
```

## 12. Knowledge Check

1. What is a file extension?
2. Does Linux require every file to have an extension?
3. Is Linux case-sensitive?
4. What is the difference between `notes.txt` and `Notes.txt`?
5. Which command identifies the actual content type of a file?
6. Does renaming a `.txt` file to `.pdf` convert it into a PDF?
7. Does the `.sh` extension automatically make a file executable?
8. What does `chmod u+x` do?
9. Why can misleading file extensions be dangerous?
10. What command can rename a file?

## 13. Lesson Summary

File extensions help users identify the expected purpose of files, but Linux does not rely on extensions to determine their actual contents.

We learned how to:

- Understand Linux filenames.
- Work with file extensions.
- Recognize case sensitivity.
- Inspect actual file types using `file`.
- Rename files using `mv`.
- Understand the difference between extensions and executable permissions.

**Next Lesson:** Linux Storage Devices.