# LESSON 03: ROOT DIRECTORY AND HOME DIRECTORY

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

Every Linux user interacts with the filesystem through directories.

Two important directories every Linux user must understand are:

1. Root directory (`/`)
2. Home directory (`/home/username`)

Although their names sound similar, they have completely different purposes.

Understanding them is important for navigating Linux, managing files, and protecting system resources.

## 2. What Is the Root Directory?

The root directory is the highest directory in the Linux filesystem hierarchy.

It is represented by:

```bash
/
```

Every directory and file in the standard Linux filesystem hierarchy exists beneath this directory.

Examples:

```text
/
/home
/etc
/var
/usr
/boot
/root
```

The root directory acts as the starting point for locating files and directories throughout the system.

### Display the root directory

Run:

```bash
ls /
```

This displays the directories and files located directly inside `/`.

### Display detailed information

```bash
ls -lah /
```

This displays information such as permissions, ownership, sizes, and hidden entries.

## 3. What Is the Home Directory?

A home directory is a personal directory assigned to a user account.

It provides a location where users can store their personal files and organize their work.

For example, if your Linux username is `kali`, your home directory will normally be:

```text
/home/kali
```

A home directory may contain:

```text
/home/kali/
├── Desktop/
├── Documents/
├── Downloads/
├── Music/
├── Pictures/
├── Public/
├── Templates/
└── Videos/
```

The exact directories available depend on your system configuration.

### Display your home directory

Run:

```bash
echo $HOME
```

This prints the home directory associated with your current account.

You can also run:

```bash
cd ~
pwd
```

The `~` symbol represents the current user's home directory.

## 4. Understanding the Root User

Linux has a special administrative account called `root`.

The root account has user ID (UID) 0 and has extensive control over the operating system.

The root user's home directory is:

```text
/root
```

Remember:

- `/` is the root directory of the entire filesystem.
- `/root` is the home directory of the root account.
- `/home/kali` is an example of a normal user's home directory.

These are three different concepts.

### Inspect the root user's home directory

Run:

```bash
sudo ls -la /root
```

You may be asked to enter your password.

The `sudo` command allows an authorized user to execute a command with elevated privileges.

## 5. Root Directory vs Home Directory

| Feature | Root Directory (`/`) | Home Directory (`/home/user`) |
|---|---|---|
| Purpose | Starting point of filesystem | Personal user storage |
| Contains | System and user directories | User files and folders |
| Access | Usually readable; modifications are restricted | Controlled by user permissions |
| Example | `/etc` | `/home/kali/Documents` |
| Importance | System organization | Personal file organization |

## 6. Understanding the Difference Through an Analogy

Imagine a university campus.

- The entire campus represents `/`.
- The different departments represent directories such as `/etc`, `/var`, and `/usr`.
- A student's personal room represents `/home/student`.
- The administrator's private office represents `/root`.

The campus contains everything, while a student's room is only one location inside it.

Similarly, `/` contains the entire filesystem hierarchy, while `/home/kali` contains the personal files of one user.

## 7. Important Commands

| Command | Purpose |
|---|---|
| `ls /` | Lists the root directory's contents |
| `ls /home` | Lists normal users' home directories |
| `pwd` | Displays the current working directory |
| `echo $HOME` | Displays the current user's home directory |
| `cd /` | Moves to the root directory |
| `cd ~` | Moves to the current user's home directory |
| `cd /root` | Attempts to move into the root user's home directory |
| `sudo ls -la /root` | Lists the root user's home directory with elevated privileges |

## 8. Practical Laboratory

Perform these exercises inside Kali Linux.

### Exercise 1: Display the Root Directory

```bash
ls /
```

Identify at least five directories.

### Exercise 2: Display Your Home Directory

```bash
echo $HOME
```

Write down the path displayed.

### Exercise 3: Navigate to the Root Directory

```bash
cd /
pwd
```

Confirm that your current directory is `/`.

### Exercise 4: Navigate to Your Home Directory

```bash
cd ~
pwd
```

Confirm that you have returned to your personal home directory.

### Exercise 5: Inspect Your Home Directory

```bash
ls -lah ~
```

Identify the files and directories displayed.

### Exercise 6: Inspect the Root User's Home Directory

```bash
sudo ls -la /root
```

Observe the contents without modifying anything.

### Exercise 7: Compare the Three Paths

Run:

```bash
pwd
echo $HOME
sudo ls -ld /root
```

Explain what each command tells you.

## 9. Knowledge Check

1. What is the root directory?
2. What symbol represents the root directory?
3. What is a home directory?
4. Where is the home directory of a normal user usually located?
5. What is the home directory of the root account?
6. What does the `~` symbol represent?
7. What is the difference between `/` and `/root`?
8. Which command displays your current working directory?
9. Which command displays your home directory?
10. Why should normal users avoid performing everyday activities as root?

## 10. Lesson Summary

The root directory `/` is the starting point of the Linux filesystem hierarchy.

A home directory is a personal storage location assigned to a user.

The root account has its own home directory at `/root`, while normal users usually have home directories under `/home`.

Understanding these differences helps Linux users navigate the system and manage files safely.

**Next Lesson:** Understanding Linux Paths.