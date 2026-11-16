
# MODULE 12 – WEB SECURITY

# LESSON 05: OWASP TOP 10

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain OWASP and its importance.
2. Understand the OWASP Top 10.
3. Identify common web application security risks.
4. Understand how vulnerabilities affect confidentiality, integrity, and availability.
5. Recognize insecure application design and implementation.
6. Perform basic defensive security checks in a local lab.
7. Recommend appropriate security controls.

---

## 2. What Is OWASP?

OWASP stands for **Open Worldwide Application Security Project**.

It is a nonprofit organization that works to improve software and application security.

OWASP provides:

- Security documentation.
- Testing guides.
- Developer resources.
- Security standards.
- Vulnerability awareness materials.
- Open-source security tools.

One of its most popular resources is the **OWASP Top 10**.

### What Is the OWASP Top 10?

The OWASP Top 10 is an awareness document highlighting major categories of web application security risks.

It helps developers, system administrators, penetration testers, and security analysts understand common application security problems.

The current published edition used in this lesson is OWASP Top 10:2025.

---

## 3. The OWASP Top 10 (2025)

The ten categories are:

| No. | Category | Meaning |
|---|---|---|
| A01 | Broken Access Control | Users can access resources they should not access |
| A02 | Security Misconfiguration | Systems are configured insecurely |
| A03 | Software Supply Chain Failures | Risks involving software components, dependencies, and build processes |
| A04 | Cryptographic Failures | Sensitive information is inadequately protected |
| A05 | Injection | Untrusted input is interpreted as commands or queries |
| A06 | Insecure Design | Security weaknesses exist in the application's design |
| A07 | Authentication Failures | Weaknesses in proving user identity |
| A08 | Software or Data Integrity Failures | Software or data cannot be trusted to remain authentic and untampered |
| A09 | Security Logging and Alerting Failures | Security events are not properly recorded or detected |
| A10 | Mishandling of Exceptional Conditions | Errors and unusual conditions are handled unsafely |

**Important:** These are broad risk categories, not individual vulnerabilities.

---

## 4. A01 – Broken Access Control

### Meaning

Access control determines what an authenticated user is allowed to do.

Broken access control occurs when an application fails to enforce those permissions correctly.

### Example

Imagine a university portal:

```text
/student/profile/1001
/student/profile/1002
```

Student 1001 should only access their own profile.

If student 1001 can access student 1002's private information simply by changing the identifier, the application may have an access control vulnerability.

### Risks

- Unauthorized access to personal information.
- Modification of another user's records.
- Unauthorized administrative actions.
- Exposure of confidential documents.

### Prevention

- Enforce authorization on the server.
- Apply least privilege.
- Verify ownership of requested resources.
- Deny access by default.
- Test authorization rules for every sensitive operation.

---

## 5. A02 – Security Misconfiguration

### Meaning

Security misconfiguration happens when an application, server, framework, or cloud service is configured insecurely.

### Examples

- Default passwords remain enabled.
- Unnecessary services are running.
- Debug mode is enabled in production.
- Directory listing is enabled unnecessarily.
- Sensitive files are publicly accessible.
- Security headers are missing.

### Prevention

- Use secure configuration baselines.
- Remove unnecessary services.
- Change default credentials.
- Disable production debug mode.
- Review server permissions.
- Regularly audit configurations.

### Practical Example

Inspect the response headers of your local lab:

```bash
curl -I http://127.0.0.1:8000
```

Record which security-related headers are present and which are absent.

Remember that a missing header is an observation requiring context, not automatically proof of a vulnerability.

---

## 6. A03 – Software Supply Chain Failures

### Meaning

Modern applications depend on many external components.

These may include:

- Third-party libraries.
- Frameworks.
- Packages.
- Plugins.
- Build tools.
- Container images.
- External software services.

A supply chain failure occurs when weaknesses in these components or their development and delivery processes compromise application security.

### Example

A developer uses an outdated library containing a publicly known security vulnerability.

If the application continues using that library without assessment or remediation, the application may inherit the weakness.

### Prevention

- Use trusted package repositories.
- Maintain a software inventory.
- Update dependencies.
- Review package maintainers and sources.
- Scan dependencies for known vulnerabilities.
- Protect build pipelines and software signing credentials.

### Useful Linux Commands

Check installed Debian packages:

```bash
dpkg -l
```

Check whether package updates are available:

```bash
sudo apt update
apt list --upgradable
```

Do not install unfamiliar packages simply because a security scanner recommends them.

---

## 7. A04 – Cryptographic Failures

### Meaning

Cryptographic failures occur when sensitive information is not properly protected using cryptography.

### Examples

- Passwords stored in plaintext.
- Weak encryption algorithms.
- Poor key management.
- Sensitive information transmitted without HTTPS.
- Hardcoded encryption keys.
- Inadequate password hashing.

### Example

Unsafe password storage:

```text
username: abbas
password: mypassword123
```

A secure application should never store users' passwords in readable plaintext.

Instead, it should use a suitable password-hashing algorithm such as Argon2id, scrypt, or bcrypt, with appropriate parameters.

### Prevention

- Use HTTPS.
- Use modern cryptographic algorithms.
- Hash passwords with dedicated password-hashing algorithms.
- Protect encryption keys.
- Avoid hardcoded secrets.
- Minimize the collection and storage of sensitive information.

### Key Difference

Encryption is designed to protect confidentiality and can be reversed using the appropriate key.

Password hashing is designed to be one-way and is used to verify passwords without storing the original password.

---

## 8. A05 – Injection

### Meaning

Injection occurs when untrusted input is interpreted as part of a command or query instead of being treated strictly as data.

Common examples include:

- SQL injection.
- Command injection.
- Cross-site scripting (XSS).
- LDAP injection.

### Example: SQL Injection Concept

Imagine an application constructing a database query using unsafe string concatenation.

```python
query = "SELECT * FROM users WHERE username = '" + username + "'"
```

The problem is that user input becomes part of the SQL command.

An attacker may manipulate the input to change the meaning of the query.

### Prevention

- Use parameterized SQL queries.
- Validate input.
- Apply context-appropriate output encoding.
- Avoid constructing operating-system commands from untrusted input.
- Use least-privilege database accounts.

### Safe Python Example

```python
import sqlite3

connection = sqlite3.connect("users.db")

username = input("Enter username: ")

cursor = connection.cursor()

cursor.execute(
    "SELECT * FROM users WHERE username = ?",
    (username,)
)

results = cursor.fetchall()

print(results)

connection.close()
```

Notice that the input is passed separately from the SQL statement.

This is called parameterized querying.

---

## 9. A06 – Insecure Design

### Meaning

Insecure design occurs when an application lacks adequate security controls at the architectural or design level.

It is different from a coding mistake.

A developer may write code correctly while the overall system design remains insecure.

### Example

A banking application allows unlimited money-transfer attempts without:

- Transaction limits.
- Additional verification.
- Fraud detection.
- Risk-based monitoring.

The problem may exist in the original design.

### Prevention

- Perform threat modeling.
- Define security requirements.
- Apply secure design principles.
- Identify abuse cases.
- Include authorization and verification in system architecture.
- Conduct security reviews before deployment.

---

## 10. A07 – Authentication Failures

### Meaning

Authentication verifies that a person or system is who it claims to be.

Authentication failures happen when identity verification is weak or incorrectly implemented.

### Examples

- Weak passwords.
- No protection against automated login attempts.
- Poor session management.
- Predictable password-reset tokens.
- Improper multi-factor authentication implementation.
- Insecure account recovery.

### Authentication vs Authorization

| Authentication | Authorization |
|---|---|
| Verifies identity | Determines permissions |
| Who are you? | What can you access? |
| Login process | Access-control process |

### Prevention

- Use strong password policies.
- Implement multi-factor authentication.
- Secure password-reset workflows.
- Apply rate limiting.
- Use secure session identifiers.
- Monitor suspicious login activity.

---

## 11. A08 – Software or Data Integrity Failures

### Meaning

Integrity means ensuring that software and data remain authentic and have not been modified without authorization.

### Examples

- Installing unverified software packages.
- Accepting untrusted software updates.
- Insecure deserialization.
- Tampered application files.
- Unprotected deployment pipelines.

### Prevention

- Verify software signatures.
- Use trusted software repositories.
- Protect CI/CD pipelines.
- Apply integrity checks.
- Restrict access to deployment systems.
- Validate data before processing.

### Practical Example: File Integrity

Create a test file:

```bash
echo "Original security file" > integrity-test.txt
```

Calculate its SHA-256 hash:

```bash
sha256sum integrity-test.txt
```

Modify the file:

```bash
echo "Modified content" >> integrity-test.txt
```

Calculate the hash again:

```bash
sha256sum integrity-test.txt
```

Compare the results.

The hash should change because the file content changed.

**Important:** A hash can help detect modification when compared against a trusted reference. A hash alone does not prove who created a file.

---

## 12. A09 – Security Logging and Alerting Failures

### Meaning

Applications should record important security events and generate alerts when suspicious activity occurs.

Logging and alerting failures happen when security events are not adequately recorded, monitored, or investigated.

### Examples

- Failed logins are not recorded.
- Administrative changes are not logged.
- Logs can be modified by unauthorized users.
- Alerts are not generated for suspicious activity.
- Sensitive information is unnecessarily written into logs.

### Prevention

- Record important security events.
- Protect log files.
- Synchronize system clocks.
- Monitor authentication failures.
- Configure useful alerts.
- Review logs regularly.
- Avoid recording passwords, access tokens, and other secrets.

### Useful Linux Commands

View recent system logs:

```bash
sudo journalctl -n 30
```

View authentication-related messages where available:

```bash
sudo journalctl | grep -i authentication
```

Search for failed login messages where available:

```bash
sudo journalctl | grep -i "failed"
```

Log formats and available messages vary between distributions.

---

## 13. A10 – Mishandling of Exceptional Conditions

### Meaning

Applications must handle unexpected situations safely.

Exceptional conditions include:

- Invalid input.
- Database failures.
- Network timeouts.
- Missing files.
- Unexpected data formats.
- Insufficient permissions.
- Resource exhaustion.

If an application handles these conditions incorrectly, it may expose sensitive information or enter an unsafe state.

### Example

A poorly designed application displays a detailed database error to an ordinary user.

The error might reveal:

- Database table names.
- Internal file paths.
- Software versions.
- SQL query details.
- Internal system architecture.

### Prevention

- Handle errors safely.
- Avoid exposing internal stack traces to users.
- Log useful diagnostic information securely.
- Validate application state.
- Fail safely.
- Test unexpected inputs and failure conditions.

---

## 14. Practical Lab 1 – Identify OWASP Risks

Create your working directory:

```bash
mkdir -p ~/practical-labs/module12/lab05
cd ~/practical-labs/module12/lab05
```

Create a file:

```bash
nano owasp-observations.txt
```

Enter the following fictional application observations:

```text
1. A student can view another student's private result.
2. A production server displays detailed debugging information.
3. An application uses an outdated vulnerable third-party library.
4. User passwords are stored in plaintext.
5. User input is directly inserted into SQL queries.
6. A payment application has no transaction limits.
7. Login attempts are not protected against automation.
8. An application accepts unverified software updates.
9. Failed administrative login attempts are not recorded.
10. Database errors reveal internal file paths.
```

Your task is to identify the OWASP category associated with each observation.

Use the OWASP Top 10 table in this lesson.

---

## 15. Practical Lab 2 – File Integrity Verification

Create a file:

```bash
echo "OWASP practical exercise" > security-note.txt
```

Calculate its hash:

```bash
sha256sum security-note.txt
```

Copy the file:

```bash
cp security-note.txt security-note-backup.txt
```

Compare the hashes:

```bash
sha256sum security-note.txt security-note-backup.txt
```

Both hashes should initially match.

Now modify the original:

```bash
echo "Additional content" >> security-note.txt
```

Calculate both hashes again:

```bash
sha256sum security-note.txt security-note-backup.txt
```

Observe the difference.

Record your findings in your notes.

---

## 16. Practical Lab 3 – Review a Secure Coding Example

Create a Python file:

```bash
nano secure-query.py
```

Enter:

```python
import sqlite3

connection = sqlite3.connect(":memory:")

connection.execute("""
CREATE TABLE students (
    id INTEGER PRIMARY KEY,
    name TEXT
)
""")

connection.execute(
    "INSERT INTO students (name) VALUES (?)",
    ("Abbas",)
)

student_name = input("Enter student name: ")

cursor = connection.execute(
    "SELECT * FROM students WHERE name = ?",
    (student_name,)
)

for student in cursor:
    print(student)

connection.close()
```

Run it:

```bash
python3 secure-query.py
```

Enter:

```text
Abbas
```

Observe the result.

### Questions

1. Why is the question mark used in the SQL query?
2. Why is the user input passed separately?
3. What type of injection risk does this approach help prevent?
4. Why should the database account have only the permissions it needs?

---

## 17. OWASP Top 10 Summary Table

| Category | Main Security Concern |
|---|---|
| A01 | Broken access control |
| A02 | Security misconfiguration |
| A03 | Software supply chain failures |
| A04 | Cryptographic failures |
| A05 | Injection |
| A06 | Insecure design |
| A07 | Authentication failures |
| A08 | Software or data integrity failures |
| A09 | Security logging and alerting failures |
| A10 | Mishandling exceptional conditions |

---

## 18. Knowledge Check

Answer these questions in your notebook.

1. What does OWASP stand for?
2. What is the purpose of the OWASP Top 10?
3. Explain broken access control with a practical example.
4. What is security misconfiguration?
5. What is a software supply chain failure?
6. Explain the difference between encryption and password hashing.
7. What is SQL injection?
8. What is insecure design?
9. Differentiate authentication from authorization.
10. What is the purpose of file integrity verification?
11. Why are security logs important?
12. What is meant by mishandling exceptional conditions?

---

## 19. Lesson Completion Checklist

- [ ] Understand OWASP and its purpose.
- [ ] Study all ten OWASP categories.
- [ ] Complete Practical Lab 1.
- [ ] Complete Practical Lab 2.
- [ ] Complete Practical Lab 3.
- [ ] Understand the secure SQL example.
- [ ] Complete the knowledge check.
- [ ] Save your practical observations.

---

## 20. Final Understanding

The OWASP Top 10 provides a foundation for understanding common web application security risks.

A professional security analyst must understand not only how vulnerabilities occur but also how to identify, validate, document, and prevent them.

Remember:

**Secure applications are not created by fixing vulnerabilities alone. They are created through secure design, secure coding, proper configuration, continuous monitoring, and regular testing.**
