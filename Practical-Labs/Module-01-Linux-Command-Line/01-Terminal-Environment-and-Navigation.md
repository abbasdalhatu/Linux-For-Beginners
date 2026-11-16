
# PRACTICAL LABS
# MODULE 01 – LINUX COMMAND-LINE MASTERY

## LAB 01: TERMINAL ENVIRONMENT AND NAVIGATION

---

## 1. Lab Objectives

In this lab, you will practice navigating a Linux environment using the terminal.

By the end, you should be able to:

1. Identify your current working directory.
2. Navigate between directories.
3. Understand absolute and relative paths.
4. List files and directories.
5. Identify hidden files.
6. Navigate Linux system directories.
7. Use terminal help facilities.

## 2. Lab Environment

- Operating system: Kali Linux
- Environment: VMware virtual machine
- Interface: Linux terminal
- Practice directory: `~/practical-labs/module01/lab01`

All exercises will be performed inside your own Kali VM.

## 3. Exercise 1: Identify Your Current Location

Run:

```bash
pwd
```

Explanation:

`pwd` means Print Working Directory.

It displays the directory where your terminal is currently operating.

Example:

```text
/home/kali
```

Your result may be different.

Now run:

```bash
whoami
hostname
date
```

Record your results.

## 4. Exercise 2: Inspect Your Home Directory

Run:

```bash
ls
```

This displays visible files and directories.

Now run:

```bash
ls -l
```

This displays a detailed listing, including permissions, ownership, size and modification time.

Display hidden files:

```bash
ls -a
```

Display hidden and visible files with details:

```bash
ls -lah
```

Important:

Linux filenames beginning with a dot (`.`) are hidden by default in ordinary directory listings.

Examples:

- `.bashrc`
- `.profile`
- `.config`

## 5. Exercise 3: Create Your Practice Environment

Create your dedicated lab directory:

```bash
mkdir -p ~/practical-labs/module01/lab01
```

Navigate into it:

```bash
cd ~/practical-labs/module01/lab01
```

Verify:

```bash
pwd
```

Expected path:

```text
/home/kali/practical-labs/module01/lab01
```

Your username or home directory may differ.

## 6. Exercise 4: Navigate Between Directories

Practice these commands:

### Move one directory backward

```bash
cd ..
```

### Return to your home directory

```bash
cd ~
```

### Move to the root directory

```bash
cd /
```

### Inspect the root directory

```bash
ls -lah
```

### Enter the temporary directory

```bash
cd /tmp
```

### Return to your practice directory

```bash
cd ~/practical-labs/module01/lab01
```

Verify your location after each navigation command.

## 7. Exercise 5: Understand Absolute and Relative Paths

An absolute path begins from the root directory.

Example:

```bash
/home/kali/practical-labs/module01/lab01
```

A relative path is interpreted from your current working directory.

Example:

```bash
cd module01
```

This works only if the current directory contains a directory named `module01`.

Practice:

```bash
cd ~
cd practical-labs
pwd
cd module01
pwd
cd lab01
pwd
```

Observe how the current location changes.

## 8. Exercise 6: Explore Important Linux Directories

Inspect these directories:

```bash
ls /
ls /home
ls /etc
ls /var
ls /tmp
ls /usr
ls /dev
```

Understand their purposes:

| Directory | Purpose |
|---|---|
| `/` | Root of the filesystem |
| `/home` | Ordinary users' home directories |
| `/etc` | System configuration files |
| `/var` | Variable data, including logs |
| `/tmp` | Temporary files |
| `/usr` | User-space programs and resources |
| `/dev` | Device interfaces |

These commands inspect the directories. Do not modify system files.

## 9. Exercise 7: Use Command Help

Linux provides built-in documentation.

Run:

```bash
ls --help
```

Read the manual:

```bash
man ls
```

To leave the manual, press:

```text
q
```

Try:

```bash
man pwd
man cd
```

Note: `cd` is a shell built-in, so its documentation is normally available through:

```bash
help cd
```

## 10. Exercise 8: Terminal Navigation Challenge

Complete these tasks without copying the commands from earlier exercises.

1. Display your current location.
2. Return to your home directory.
3. Display all hidden and visible files.
4. Navigate to `/etc`.
5. Display the first entries in `/etc`.
6. Return to your home directory.
7. Enter your Lab 01 directory.
8. Confirm your final location.

Useful command options:

```bash
ls -lah
ls -l
pwd
cd
```

## 11. Knowledge Check

Answer in your notebook:

1. What does `pwd` mean?
2. What is the difference between `ls` and `ls -a`?
3. What is an absolute path?
4. What is a relative path?
5. What does `cd ..` do?
6. What does `cd ~` do?
7. What is the purpose of `/etc`?
8. Why are some Linux files hidden?
9. What is the purpose of `man`?
10. Why should you verify your location before running administrative commands?

## 12. Lab Completion Checklist

- [ ] Identified the current directory.
- [ ] Identified the current user.
- [ ] Inspected the home directory.
- [ ] Displayed hidden files.
- [ ] Created the Lab 01 directory.
- [ ] Practiced directory navigation.
- [ ] Compared absolute and relative paths.
- [ ] Inspected important Linux directories.
- [ ] Used `man` and `--help`.
- [ ] Completed the navigation challenge.

**END OF LAB 01**
