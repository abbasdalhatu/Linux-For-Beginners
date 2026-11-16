
# LINUX BASICS — PRACTICAL EXERCISES

Welcome to the Linux Basics practical exercises.

These activities help learners develop confidence using the Linux terminal.

**Difficulty:** Beginner

## Exercise 1: Understanding the Terminal

Answer the following questions:

1. What is Linux?
2. What is a Linux terminal?
3. What is a shell?
4. What is the difference between a command and a script?
5. What is the purpose of the root user?

## Exercise 2: Basic Commands

Run the following commands in a Linux terminal:

```bash
pwd
whoami
hostname
date
uname -a
```

Tasks:

1. Identify your current directory.
2. Find your current username.
3. Display your computer's hostname.
4. Display the current date and time.
5. Identify your Linux kernel version.

## Exercise 3: Directory Management

Practise these commands:

```bash
mkdir LinuxPractice
cd LinuxPractice
mkdir Documents Projects Backups
pwd
ls
```

Tasks:

1. Create a directory called LinuxPractice.
2. Enter the directory.
3. Create three additional directories.
4. Display your current location.
5. List all directories.

## Exercise 4: File Management

Run:

```bash
touch notes.txt
touch information.txt
ls -l
```

Tasks:

1. Create two empty text files.
2. Rename notes.txt to linux-notes.txt.
3. Copy linux-notes.txt into the Backups directory.
4. Move information.txt into Documents.
5. Display the directory contents.

## Exercise 5: Reading Files

Create a text file:

```bash
echo "Linux is an operating system." > introduction.txt
```

Practise:

```bash
cat introduction.txt
less introduction.txt
head introduction.txt
tail introduction.txt
```

Tasks:

1. Display the file contents.
2. Open the file using less.
3. Display its first lines.
4. Display its last lines.

## Exercise 6: File Permissions

Create a script:

```bash
touch practice.sh
ls -l practice.sh
```

Change its permissions:

```bash
chmod 755 practice.sh
ls -l practice.sh
```

Tasks:

1. Identify the original permissions.
2. Change the permissions to 755.
3. Explain what 755 means.
4. Identify which users can execute the file.

## Exercise 7: Finding Files

Practise:

```bash
find . -name "*.txt"
```

Tasks:

1. Find all text files in the current directory.
2. Find a file named introduction.txt.
3. Explain what the find command does.

## Exercise 8: Beginner Challenge

Create the following directory structure:

```text
LinuxPractice/
├── Documents/
│   ├── notes.txt
│   └── introduction.txt
├── Projects/
│   └── project.txt
└── Backups/
    └── backup.txt
```

Requirements:

1. Create all directories.
2. Create all files.
3. Add a short sentence to each file.
4. Display the complete directory structure.
5. Confirm that all files exist.

## Exercise 9: Knowledge Assessment

Answer these questions:

1. Which command displays the current directory?
2. Which command lists files?
3. Which command creates a directory?
4. Which command copies files?
5. Which command renames files?
6. Which command deletes a file?
7. Which command changes permissions?
8. What does `pwd` mean?
9. What is the purpose of `chmod`?
10. What is the difference between an absolute path and a relative path?

## Completion Checklist

- [ ] Understand basic Linux commands.
- [ ] Navigate directories.
- [ ] Create and manage files.
- [ ] Read file contents.
- [ ] Understand basic permissions.
- [ ] Locate files using find.
- [ ] Complete the beginner challenge.
- [ ] Answer the knowledge assessment.

**Learning objective:** Become comfortable navigating Linux and managing basic files and directories.
