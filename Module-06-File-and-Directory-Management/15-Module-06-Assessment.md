---
---

# Module 06 – File and Directory Management

# Module 06 Assessment

## 1. Assessment Information

**Module:** 06 – File and Directory Management

**Assessment Type:** Theory and Practical

**Total Marks:** 100

**Pass Mark:** 50%

**Instructions:**

1. Attempt every question.
2. Use your Kali Linux virtual machine for practical exercises.
3. Write commands correctly.
4. Explain your answers clearly.
5. Do not perform practical exercises outside the designated practice directory.
6. Do not use destructive commands on important system files.

---

# PART A: THEORY QUESTIONS (30 MARKS)

Answer all questions.

Each question carries 3 marks.

### Question 1
What is file and directory management in Linux? Explain why it is important.

### Question 2
Explain the difference between absolute paths and relative paths. Give one example of each.

### Question 3
Identify and explain four types of files found in Linux.

### Question 4
What is the purpose of the following commands?

a. `mkdir`

b. `touch`

c. `cp`

d. `mv`

### Question 5
Explain the difference between `rm` and `rmdir`.

### Question 6
What is the purpose of the `cat` command? Explain the difference between `>` and `>>`.

### Question 7
Differentiate between `less` and `more`.

### Question 8
Explain the difference between `head` and `tail`. What does `tail -f` do?

### Question 9
What is file metadata? Explain the difference between access time, modification time, and change time.

### Question 10
Differentiate between hard links and symbolic links.

---

# PART B: COMMAND KNOWLEDGE (20 MARKS)

Write the correct Linux command for each task.

Each question carries 2 marks.

### Question 11
Create a directory called `CyberLab`.

### Question 12
Create an empty file called `report.txt`.

### Question 13
Copy `report.txt` into a directory called `Backups`.

### Question 14
Rename `report.txt` to `security-report.txt`.

### Question 15
Move `security-report.txt` into `Backups`.

### Question 16
Display the contents of `security-report.txt`.

### Question 17
Display the first five lines of `security.log`.

### Question 18
Display the last ten lines of `security.log`.

### Question 19
Display detailed metadata for `security-report.txt`.

### Question 20
Create a symbolic link called `report-link.txt` pointing to `security-report.txt`.

---

# PART C: PRACTICAL LABORATORY (30 MARKS)

Complete the following tasks in your Kali Linux virtual machine.

Each task carries 5 marks.

## Task 1: Directory Creation

Create this directory structure inside your home directory:

```text
Module06-Assessment/
├── Documents/
├── Backups/
├── Logs/
└── Evidence/
```

Verify the structure using an appropriate command.

## Task 2: File Creation

Inside `Documents/`, create:

```text
report.txt
notes.txt
security.txt
```

Put at least two lines of meaningful text inside each file.

## Task 3: Copying and Moving

1. Copy `report.txt` into `Backups/`.
2. Move `notes.txt` into `Evidence/`.
3. Rename `security.txt` to `security-report.txt`.
4. Move `security-report.txt` into `Evidence/`.

Verify the final locations.

## Task 4: Viewing File Contents

1. Display the contents of `report.txt`.
2. Display the first two lines of `report.txt`.
3. Display the last two lines of `report.txt`.
4. Open `report.txt` using `less`.
5. Exit using `q`.

## Task 5: Metadata and Links

1. Display metadata for `report.txt`.
2. Identify its file type.
3. Display its inode number.
4. Create a hard link called `report-hardlink.txt` pointing to `report.txt`.
5. Create a symbolic link called `report-symlink.txt` pointing to `report.txt`.
6. Compare their inode numbers.

## Task 6: Wildcards and Deletion

1. List all `.txt` files inside `Documents/`.
2. Copy all `.txt` files into `Backups/`.
3. Create a temporary file called `temporary.tmp`.
4. Remove it using interactive deletion.
5. Verify that the temporary file no longer exists.

---

# PART D: TROUBLESHOOTING AND SCENARIO ANALYSIS (20 MARKS)

Each question carries 5 marks.

### Question 21

A Linux administrator runs:

```bash
rmdir Projects
```

The terminal displays:

```text
rmdir: failed to remove 'Projects': Directory not empty
```

Explain why this happens and provide a suitable solution.

### Question 22

A user executes:

```bash
cp report.txt Backups/
```

But receives an error saying that the destination does not exist.

Explain the problem and provide two ways to solve it.

### Question 23

An administrator creates a symbolic link:

```bash
ln -s report.txt shortcut.txt
```

Later, the administrator deletes `report.txt`.

What happens when they attempt to open `shortcut.txt`? Explain why.

### Question 24

During a security investigation, an analyst finds a suspicious file named:

```text
invoice.pdf
```

However, the analyst suspects that it may actually be an executable file.

Which Linux command can help identify its real file type? Explain why relying only on the filename extension is dangerous.

---

# PART E: ADVANCED PRACTICAL CHALLENGE (BONUS – 10 MARKS)

This section is optional but recommended.

Create a directory called:

```text
Forensics-Lab/
```

Inside it, create:

```text
Forensics-Lab/
├── Evidence/
├── Reports/
├── Logs/
└── Backups/
```

Complete the following:

1. Create three evidence files.
2. Create a report describing a fictional security incident.
3. Copy the report into `Backups/`.
4. Create a hard link to one evidence file.
5. Create a symbolic link to another evidence file.
6. Display metadata for all evidence files.
7. Use `tail -f` to monitor a sample log file.
8. Add a new log entry from another terminal.
9. Verify that the new entry appears.
10. Organise all files into the appropriate directories.

Document the commands you used and explain what each one accomplished.

---

# ASSESSMENT SUBMISSION CHECKLIST

- [ ] Part A: Theory questions completed.
- [ ] Part B: Command knowledge completed.
- [ ] Part C: Practical laboratory completed.
- [ ] Part D: Troubleshooting questions completed.
- [ ] Part E: Bonus challenge attempted (optional).

## Assessment Grading

| Section | Marks |
|---|---:|
| Part A – Theory | 30 |
| Part B – Command Knowledge | 20 |
| Part C – Practical Laboratory | 30 |
| Part D – Troubleshooting | 20 |
| **Total** | **100** |
| Bonus Challenge | +10 |

## Performance Classification

| Score | Result |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Requires Improvement |

## Final Reflection

After completing the assessment, answer:

1. Which topic did you find easiest?
2. Which topic was most challenging?
3. Which Linux commands can you now use confidently?
4. How can file management skills help in ethical hacking?
5. What do you still need to practise?

---

**END OF MODULE 06 ASSESSMENT**

**Next Module:** Module 07 – Users and Permissions.