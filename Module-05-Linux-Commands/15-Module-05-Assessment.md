# MODULE 05 – LINUX COMMANDS
## Final Module Assessment

**Course:** Linux for Ethical Hackers  
**Module:** 05 – Linux Commands  
**Total Marks:** 100  
**Pass Mark:** 50%  
**Suggested Time:** 90–120 minutes

---

## ASSESSMENT INSTRUCTIONS

1. Answer every question.
2. Write command answers carefully, paying attention to spelling, options, and syntax.
3. Complete practical tasks inside your Kali Linux virtual machine.
4. Use a dedicated assessment directory. Do not perform practical tasks on important system directories.
5. Where a command is unavailable, explain the limitation and provide an appropriate alternative.
6. Record your answers and practical outputs in this file or a separate submission document.

**Important:** Only practise cybersecurity activities on systems you own or have explicit permission to test.

---

# SECTION A – MULTIPLE-CHOICE QUESTIONS
**20 Questions × 1 Mark = 20 Marks**

Choose the correct answer.

**1.** Which command displays your current working directory?

A. `ls`  
B. `pwd`  
C. `cd`  
D. `whoami`

**2.** What does the `-l` option do when used with `ls`?

A. Lists hidden files only  
B. Displays files in long format  
C. Deletes files  
D. Changes directory

**3.** Which command moves you to your home directory?

A. `cd /`  
B. `cd ..`  
C. `cd ~`  
D. `pwd`

**4.** Which command displays documentation for another command?

A. `man`  
B. `clear`  
C. `echo`  
D. `history`

**5.** What does `mkdir` do?

A. Creates a directory  
B. Displays a directory  
C. Searches files  
D. Prints text

**6.** Which command is commonly used to display text in the terminal?

A. `find`  
B. `echo`  
C. `sort`  
D. `wc`

**7.** Which command clears the visible terminal screen?

A. `history`  
B. `clear`  
C. `printf`  
D. `type`

**8.** Which command displays previously entered shell commands?

A. `history`  
B. `whereis`  
C. `locate`  
D. `grep`

**9.** What does the `which` command normally identify?

A. The current user  
B. The location of an executable found through PATH  
C. The current directory  
D. The Linux kernel version

**10.** Which command searches for files and directories based on specified conditions?

A. `find`  
B. `echo`  
C. `uniq`  
D. `clear`

**11.** Which command searches text for matching patterns?

A. `sort`  
B. `grep`  
C. `wc`  
D. `pwd`

**12.** Which command sorts lines of text?

A. `uniq`  
B. `grep`  
C. `sort`  
D. `tee`

**13.** What does `wc -l` count?

A. Characters  
B. Words  
C. Lines  
D. Directories

**14.** Which symbol sends standard output to a file and overwrites its existing contents?

A. `>>`  
B. `>`  
C. `|`  
D. `<`

**15.** Which symbol appends standard output to an existing file?

A. `>>`  
B. `>`  
C. `2>`  
D. `&&`

**16.** What is the purpose of a pipe (`|`)?

A. Deletes a file  
B. Sends one command's standard output to another command's standard input  
C. Changes the current user  
D. Runs a command as administrator

**17.** Which file descriptor represents standard error?

A. 0  
B. 1  
C. 2  
D. 3

**18.** What does `&&` mean when chaining commands?

A. Run the next command only if the previous command succeeds  
B. Run the next command only if the previous command fails  
C. Run both commands in separate terminals  
D. Stop all processes

**19.** Which command can display output on the terminal while also writing it to a file?

A. `cat`  
B. `tee`  
C. `clear`  
D. `which`

**20.** Which symbol runs a command in the background?

A. `&`  
B. `|`  
C. `>`  
D. `;`

---

# SECTION B – SHORT-ANSWER QUESTIONS
**10 Questions × 2 Marks = 20 Marks**

Answer in your own words.

**1.** What is a Linux command?

**2.** Explain the general structure of a Linux command. Identify the command, options, and arguments.

**3.** Explain the difference between `man` and `info`.

**4.** State the difference between `pwd` and `ls`.

**5.** Explain the difference between `which`, `whereis`, and `type`.

**6.** What is the difference between `find` and `locate`?

**7.** Explain the difference between `grep` and `sort`.

**8.** What is the difference between standard output (stdout) and standard error (stderr)?

**9.** Explain the difference between `>` and `>>`.

**10.** What is command chaining? Give two examples of operators used for chaining commands.

---

# SECTION C – PRACTICAL COMMAND TASKS
**5 Tasks × 5 Marks = 25 Marks**

Perform these tasks in your Kali Linux terminal.

First, create a dedicated assessment directory:

`mkdir -p ~/module05-assessment`

Move into it:

`cd ~/module05-assessment`

### Task 1 – Directory Navigation (5 Marks)

1. Display your current working directory.
2. Create a directory named `practice`.
3. Enter the `practice` directory.
4. Display its absolute path.
5. Return to your assessment directory.

Record the commands you used.

### Task 2 – File Creation and Text Output (5 Marks)

Inside `~/module05-assessment`, create a file named `students.txt`.

Insert these names into the file, one per line:

- Amina
- Ibrahim
- Yusuf
- Fatima
- Maryam

Display the contents of the file.

Record the commands you used.

### Task 3 – Searching and Filtering (5 Marks)

Using `students.txt`:

1. Search for the name `Yusuf` using `grep`.
2. Search for the name `Amina`.
3. Display only the matching line for `Maryam`.
4. Count the total number of lines in the file.

Record your commands and their outputs.

### Task 4 – Sorting and Counting (5 Marks)

Create a file named `numbers.txt` containing:

```text
40
10
30
20
10
50
30
```

Then:

1. Sort the numbers.
2. Display unique lines.
3. Count the number of lines.
4. Count the number of words.
5. Count the number of bytes.

Record your commands and outputs.

### Task 5 – Command Discovery and Help (5 Marks)

Use your terminal to:

1. Find the executable path of `grep` using `which`.
2. Identify how the shell interprets `cd` using `type`.
3. Display information about `find` using `man`.
4. Check whether `locate` is available.
5. Explain what you would do if `locate` is not installed.

Record your commands and observations.

---

# SECTION D – COMMAND ANALYSIS AND PIPELINE CHALLENGES
**20 Marks**

### Question 1 – Understanding a Pipeline (5 Marks)

Study this command:

`cat students.txt | sort | uniq`

Answer:

1. What does the first command do?
2. What does `sort` do?
3. What does `uniq` do?
4. Explain how the pipe connects these commands.
5. What limitation should you remember when using `uniq` without sorting first?

### Question 2 – Redirection Analysis (7 Marks)

Study the following commands:

```bash
echo "Linux Practice" > practice.txt
echo "Ethical Hacking" >> practice.txt
cat practice.txt
```

Answer:

1. What happens in the first command?
2. What happens in the second command?
3. Why is `>>` used instead of `>` in the second command?
4. What does the third command display?
5. What would happen if the first command were executed again with different text?

### Question 3 – Build a Pipeline (8 Marks)

Assume you have a file named `access.log` containing several lines of log information.

Write commands to:

1. Search the file for lines containing the word `failed`.
2. Sort the matching lines.
3. Count how many matching lines exist.
4. Save the matching lines to a file named `failed-events.txt`.
5. Display the matching lines on the terminal while also saving them to a file named `review.txt`.

Explain the purpose of each command and operator you use.

You do not need to create or access real system logs. This is a command-writing exercise using a hypothetical file.

---

# SECTION E – CYBERSECURITY SCENARIO
**15 Marks**

### Scenario: Basic Security Log Investigation

You are a junior cybersecurity analyst working in an authorised Linux lab.

Your supervisor gives you a sample log file named `security.log`. The file contains records of successful and failed login attempts.

You are asked to investigate the file using basic Linux commands.

Answer the following:

**1.** Which command would you use to search for failed login attempts? Write the command. (3 Marks)

**2.** How would you count the number of lines containing failed login attempts? Write the command. (3 Marks)

**3.** How would you save the matching lines into a separate file without modifying the original log? (3 Marks)

**4.** Why is it important not to modify the original log file during an investigation? (3 Marks)

**5.** Explain how basic Linux commands can help a cybersecurity analyst investigate suspicious activity. (3 Marks)

---

# MARKING SUMMARY

| Section | Description | Marks |
|---|---|---:|
| A | Multiple-choice questions | 20 |
| B | Short-answer questions | 20 |
| C | Practical command tasks | 25 |
| D | Command analysis and pipelines | 20 |
| E | Cybersecurity scenario | 15 |
| **TOTAL** | | **100** |

## PERFORMANCE GUIDE

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| 0–49 | Needs Improvement |

**Official Module Pass Mark: 50/100**

---

# SUBMISSION CHECKLIST

- [ ] Completed all 20 multiple-choice questions.
- [ ] Answered all 10 short-answer questions.
- [ ] Completed all 5 practical tasks.
- [ ] Answered all 3 command analysis questions.
- [ ] Completed the cybersecurity scenario.
- [ ] Checked command syntax and spelling.
- [ ] Saved my assessment answers.

## FINAL REFLECTION

After completing this assessment, answer:

1. Which Linux command do you understand best?
2. Which command or concept was most difficult?
3. How can Linux commands help you in ethical hacking?
4. What topic do you need to practise more?

---

**END OF MODULE 05 ASSESSMENT**

*Complete the assessment independently. Submit your answers and practical command outputs for marking and feedback.*