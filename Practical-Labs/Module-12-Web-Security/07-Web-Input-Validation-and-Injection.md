
# MODULE 12 – WEB SECURITY

# LESSON 07: WEB INPUT VALIDATION AND INJECTION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain input validation.
2. Understand input sanitization and output encoding.
3. Identify common injection vulnerabilities.
4. Explain SQL injection.
5. Understand command injection.
6. Understand Cross-Site Scripting (XSS).
7. Apply parameterized SQL queries.
8. Recognize secure input-handling practices.

---

## 2. What Is Input Validation?

Input validation is the process of checking information submitted to an application to determine whether it meets expected rules.

Applications receive input from:

- Login forms.
- Registration forms.
- Search boxes.
- File uploads.
- API requests.
- Contact forms.
- Payment forms.

An application should never automatically trust information received from a user.

### Example

Imagine a university registration form requiring a student matriculation number.

Expected input:

```text
MAAUN/CS/2026/001
```

Unexpected input might include:

```text
<script>alert(1)</script>
```

The application should validate input according to the expected format and reject inappropriate data.

### Why Input Validation Matters

It helps prevent:

- Injection vulnerabilities.
- Invalid records.
- Unexpected application behavior.
- Data corruption.
- Certain types of abuse.

---

## 3. Types of Input Validation

### A. Allowlist Validation

Allowlist validation accepts only information that meets predefined rules.

Example:

A student age field accepts integers between 16 and 100.

```python
age = int(input("Enter age: "))

if 16 <= age <= 100:
    print("Valid age")
else:
    print("Invalid age")
```

This example assumes the user enters an integer. A production application should also handle non-numeric input safely.

### B. Denylist Validation

Denylist validation attempts to reject known unwanted values or patterns.

Example:

An application blocks a list of prohibited file extensions.

However, denylisting alone is insufficient because attackers can use unexpected variations.

### C. Server-Side Validation

Server-side validation checks input on the application server.

This is essential because users can bypass browser-side validation.

### D. Client-Side Validation

Client-side validation checks input in the browser.

Examples include:

- Required fields.
- Email format.
- Minimum character length.
- Numeric ranges.

Client-side validation improves usability but must never replace server-side validation.

---

## 4. Validation vs Sanitization vs Output Encoding

These three concepts are related but different.

| Technique | Purpose |
|---|---|
| Validation | Checks whether input meets expected rules |
| Sanitization | Transforms or removes unwanted content where appropriate |
| Output encoding | Ensures data is safely represented in its output context |

### Example

Suppose a user submits:

```html
<script>alert("Hello")</script>
```

The application should not blindly insert this value into an HTML page.

Depending on the application's requirements, it may:

- Reject the input.
- Store it as plain text.
- Encode it before displaying it.

**Important:** HTML encoding, JavaScript encoding, URL encoding, and SQL parameterization solve different problems. They are not interchangeable.

---

## 5. What Is Injection?

Injection occurs when an application incorrectly treats untrusted input as part of an instruction, command, or query.

Instead of treating user input as data, the application allows that input to change how a system interprets an operation.

Common injection categories include:

1. SQL injection.
2. OS command injection.
3. Cross-Site Scripting.
4. LDAP injection.
5. Template injection.

Injection vulnerabilities can expose sensitive information, modify records, or compromise application behavior.

---

## 6. SQL Injection

### Meaning

SQL injection is a vulnerability that occurs when untrusted input is incorporated into a SQL query without safe separation between data and SQL instructions.

SQL is used to communicate with relational databases.

Common database systems include:

- MySQL.
- PostgreSQL.
- SQLite.
- Microsoft SQL Server.

### Vulnerable Example

Consider this Python code:

```python
query = "SELECT * FROM users WHERE username = '" + username + "'"
```

The application directly combines user input with a SQL query.

This is unsafe because the input may change the meaning of the query.

### Secure Example

Use parameterized queries:

```python
cursor.execute(
    "SELECT * FROM users WHERE username = ?",
    (username,)
)
```

The SQL statement and user-supplied value are handled separately.

### Prevention

- Use parameterized queries.
- Avoid SQL string concatenation with untrusted input.
- Apply least privilege to database accounts.
- Handle database errors safely.
- Review database access controls.
- Use secure database libraries.

---

## 7. Practical Lab 1 – Create a Secure SQLite Application

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module12/lab07
cd ~/practical-labs/module12/lab07
```

Create a Python file:

```bash
nano secure-database.py
```

Enter:

```python
import sqlite3

connection = sqlite3.connect("students.db")

connection.execute("""
CREATE TABLE IF NOT EXISTS students (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    department TEXT NOT NULL
)
""")

connection.execute(
    "INSERT INTO students (name, department) VALUES (?, ?)",
    ("Abbas", "Computer Science")
)

connection.commit()

student_name = input("Enter student name to search: ")

cursor = connection.execute(
    "SELECT * FROM students WHERE name = ?",
    (student_name,)
)

results = cursor.fetchall()

if results:
    for student in results:
        print(student)
else:
    print("No matching student found.")

connection.close()
```

Run:

```bash
python3 secure-database.py
```

Enter:

```text
Abbas
```

Observe the output.

Run the program again and enter a name that does not exist.

### Questions

1. Why is the question mark used?
2. Why is the input passed separately?
3. What vulnerability does parameterized querying help prevent?
4. Why should database permissions be restricted?

---

## 8. OS Command Injection

### Meaning

Command injection occurs when an application constructs an operating-system command using untrusted input in an unsafe way.

### Example of Unsafe Design

Imagine an application that takes user input and directly builds a shell command.

```python
import os

filename = input("Enter filename: ")

os.system("cat " + filename)
```

This is dangerous because user input is combined with a shell command.

The application may unintentionally execute additional instructions.

### Safer Approach

Avoid using a shell when it is unnecessary.

Use Python's subprocess module with an argument list:

```python
import subprocess

filename = input("Enter filename: ")

result = subprocess.run(
    ["cat", "--", filename],
    capture_output=True,
    text=True,
    check=False
)

print(result.stdout)
```

This example avoids shell interpretation, but a production application should also validate filenames and restrict access to an approved directory.

### Prevention

- Avoid shell execution where possible.
- Use argument arrays rather than shell command strings.
- Validate input.
- Restrict file and process permissions.
- Avoid running applications with unnecessary privileges.
- Handle errors safely.

---

## 9. Practical Lab 2 – Safe File Processing

Create a file:

```bash
nano safe-file-reader.py
```

Enter:

```python
from pathlib import Path

BASE_DIR = Path.cwd() / "documents"
BASE_DIR.mkdir(exist_ok=True)

filename = input("Enter document name: ")

allowed_files = {
    "notes.txt",
    "report.txt"
}

if filename not in allowed_files:
    print("Access denied: unsupported filename.")
else:
    file_path = BASE_DIR / filename

    if file_path.exists() and file_path.is_file():
        print(file_path.read_text(encoding="utf-8"))
    else:
        print("File not found.")
```

Create two test documents:

```bash
mkdir -p documents
echo "My security notes" > documents/notes.txt
echo "My practical report" > documents/report.txt
```

Run:

```bash
python3 safe-file-reader.py
```

Test:

```text
notes.txt
```

Then test:

```text
unknown.txt
```

Observe the different results.

### Questions

1. Why does the application use an allowlist?
2. Why does it restrict files to a predefined directory?
3. Why should user input not be trusted?
4. How does this design reduce risk?

---

## 10. Cross-Site Scripting (XSS)

### Meaning

Cross-Site Scripting is a web vulnerability in which untrusted content is interpreted as executable script in another user's browser.

XSS can affect:

- User accounts.
- Session security.
- Website content.
- User privacy.
- Application integrity.

### Three Common XSS Types

#### A. Reflected XSS

Malicious input is included in a response immediately after being submitted.

Example:

A search page reflects user-provided search text without proper output encoding.

#### B. Stored XSS

Untrusted content is stored by the application and later displayed to other users without safe handling.

Example:

A discussion forum stores a comment and displays it to visitors without proper encoding.

#### C. DOM-Based XSS

The vulnerability occurs in client-side JavaScript when unsafe data is inserted into the page's DOM.

Example:

A script reads untrusted URL information and inserts it into an HTML element using an unsafe method.

### Prevention

- Apply context-aware output encoding.
- Use safe DOM APIs.
- Avoid unsafe HTML insertion.
- Sanitize HTML when rich HTML content is intentionally supported.
- Implement an appropriate Content Security Policy.
- Validate data on the server.

---

## 11. Practical Lab 3 – Understand Safe HTML Output

Create a Python file:

```bash
nano safe-html.py
```

Enter:

```python
from html import escape

user_input = input("Enter a message: ")

safe_output = escape(user_input)

print("Original input:")
print(user_input)

print("\nHTML-encoded output:")
print(safe_output)
```

Run:

```bash
python3 safe-html.py
```

Enter this harmless test string:

```html
<script>alert("test")</script>
```

Observe the encoded result.

The HTML special characters should be represented safely.

### Understanding the Result

For example:

- `<` becomes `&lt;`
- `>` becomes `&gt;`
- `"` becomes `&quot;`

The browser can display the encoded text without treating it as an HTML script element when it is placed in the appropriate HTML text context.

Remember: output encoding must match the context in which data is inserted.

---

## 12. Other Injection Categories

### A. LDAP Injection

LDAP injection occurs when untrusted input changes the structure of LDAP directory queries.

LDAP is commonly used for directory services and identity management.

Prevention:

- Use safe LDAP APIs.
- Escape special characters correctly.
- Validate input.
- Apply appropriate access controls.

### B. Template Injection

Template injection occurs when untrusted input is interpreted as template instructions.

This can expose sensitive information or cause more serious consequences depending on the template engine and configuration.

Prevention:

- Never treat untrusted input as template source code.
- Use secure template APIs.
- Separate template logic from user data.
- Restrict template execution capabilities.

---

## 13. Practical Lab 4 – Identify Injection Risks

Create a file:

```bash
nano injection-review.txt
```

Enter these fictional application examples:

```text
1. A login application builds SQL statements by joining strings.
2. A file manager directly inserts user input into shell commands.
3. A forum displays stored comments without output encoding.
4. A directory application builds LDAP queries from raw input.
5. A web application treats user input as executable template instructions.
```

For each example, identify:

- The injection category.
- The insecure programming practice.
- The potential impact.
- One suitable defensive control.

Use this table:

| No. | Injection Type | Risk | Prevention |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |

---

## 14. Secure Input-Handling Principles

A secure application should follow these principles:

1. Treat all external input as untrusted.
2. Validate input on the server.
3. Prefer allowlist validation where appropriate.
4. Use parameterized database queries.
5. Avoid unnecessary shell execution.
6. Encode output according to its context.
7. Use safe APIs and libraries.
8. Apply least privilege.
9. Handle exceptions safely.
10. Test security controls regularly.

---

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is input validation?
2. Differentiate allowlist and denylist validation.
3. Why is client-side validation insufficient?
4. Explain validation, sanitization, and output encoding.
5. What is an injection vulnerability?
6. Explain SQL injection.
7. Why are parameterized queries important?
8. What is OS command injection?
9. Why should shell execution be avoided when unnecessary?
10. What is Cross-Site Scripting?
11. Differentiate reflected, stored, and DOM-based XSS.
12. What is LDAP injection?
13. What is template injection?
14. Why is least privilege important?
15. Why must all external input be treated as untrusted?

---

## 16. Lesson Completion Checklist

- [ ] Understand input validation.
- [ ] Study validation, sanitization, and output encoding.
- [ ] Understand SQL injection.
- [ ] Complete Practical Lab 1.
- [ ] Understand command injection.
- [ ] Complete Practical Lab 2.
- [ ] Study XSS.
- [ ] Complete Practical Lab 3.
- [ ] Study other injection categories.
- [ ] Complete Practical Lab 4.
- [ ] Complete the knowledge check.

---

## 17. Final Understanding

Input validation and secure data handling are fundamental parts of web application security.

Many serious application vulnerabilities occur because developers allow untrusted data to influence queries, commands, or browser execution.

A professional security engineer must understand how these vulnerabilities arise and how to prevent them through secure programming practices.

**Remember: User input is data, not an instruction. Keep the two separate.**
