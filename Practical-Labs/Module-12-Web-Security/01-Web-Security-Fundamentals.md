
# MODULE 12 – WEB SECURITY

# LESSON 01: WEB SECURITY FUNDAMENTALS

## 1. Introduction to Web Security

Web security is the practice of protecting websites, web applications, web servers, and their users from cyber threats.

Almost every organization uses web applications to provide services.

Examples include:

- Online banking systems.
- University portals.
- E-commerce websites.
- Social media platforms.
- Healthcare applications.
- Cloud management dashboards.
- Online payment systems.

These applications process information such as usernames, passwords, financial records, personal information, and business data.

If a web application is poorly secured, attackers may exploit its weaknesses to access information, modify records, or disrupt services.

Web security helps prevent these attacks.

## 2. What Is a Website?

A website is a collection of related web pages accessible through a web browser.

Examples:

- Google.
- Wikipedia.
- University websites.
- Online shopping platforms.

A website may contain static pages or dynamic content.

### Static Website

A static website displays predefined content.

Example:

A simple HTML page containing a university's contact information.

### Dynamic Website

A dynamic website generates or changes content based on user interactions, application logic, or database information.

Examples:

- Student login portals.
- Online banking dashboards.
- Shopping carts.
- Social media feeds.

Dynamic applications generally have more complex security requirements because they process user input and interact with backend services.

## 3. What Is a Web Application?

A web application is software accessed through a web browser or another HTTP-capable client.

Examples include:

- Gmail.
- Online banking applications.
- Student management systems.
- Content management systems.
- Online booking platforms.

A web application typically contains three major components.

### A. Frontend

The frontend is the part users interact with.

Common technologies:

- HTML – defines page structure.
- CSS – controls presentation and styling.
- JavaScript – provides interactive behavior.

### B. Backend

The backend processes requests and implements application logic.

Common technologies:

- Python.
- PHP.
- Java.
- Node.js.
- C#.

### C. Database

The database stores application information.

Examples:

- MySQL.
- PostgreSQL.
- MongoDB.
- Microsoft SQL Server.

## 4. How a Web Application Works

Consider a student logging into a university portal.

The process:

1. The student opens the portal in a browser.
2. The browser sends an HTTP or HTTPS request.
3. The web server receives the request.
4. The backend processes the login information.
5. The application checks the credentials against its authentication system.
6. The application returns a response.
7. The browser displays the result.

Simplified architecture:

```text
USER
 |
 v
WEB BROWSER
 |
 | HTTP / HTTPS
 v
WEB SERVER
 |
 v
BACKEND APPLICATION
 |
 v
DATABASE
 |
 v
RESPONSE TO USER
```

Every component needs appropriate security controls.

For example:

- The browser should communicate securely.
- The web server should be properly configured.
- The backend should validate user input.
- The database should restrict unauthorized access.

## 5. What Is the Attack Surface?

An attack surface is the collection of points through which a system could potentially be attacked.

A web application's attack surface may include:

- Login pages.
- Registration forms.
- Search fields.
- File uploads.
- API endpoints.
- Administrative dashboards.
- Password reset functions.
- Server configuration.
- Third-party components.

### Example

Imagine an online university portal containing:

```text
/login
/register
/student-dashboard
/admin
/search
/upload
/api/students
```

Each endpoint may have different security requirements.

For example:

- `/login` requires secure authentication.
- `/admin` requires strong access control.
- `/upload` requires file validation.
- `/api/students` requires proper authorization.

An endpoint is not automatically vulnerable simply because it exists. It must be assessed within its application context.

## 6. The CIA Triad in Web Security

The CIA Triad is a fundamental information security model.

CIA stands for:

### A. Confidentiality

Ensures that information is accessible only to authorized users.

Example:

A student should not be able to view another student's private academic records.

Security controls:

- Encryption.
- Authentication.
- Access control.
- Secure session management.

### B. Integrity

Ensures that information remains accurate and is not modified without authorization.

Example:

A student should not be able to change their examination score without proper authorization.

Security controls:

- Input validation.
- Authorization checks.
- Audit logging.
- Data integrity mechanisms.

### C. Availability

Ensures that systems and information remain accessible when required.

Example:

Students should be able to access the university portal during registration.

Security controls:

- Backups.
- Redundancy.
- Monitoring.
- Capacity management.
- Denial-of-service protection.

## 7. Common Web Security Threats

### 7.1 SQL Injection

SQL Injection occurs when untrusted input is improperly included in database queries.

Potential consequences include:

- Unauthorized data access.
- Data modification.
- Authentication bypass.
- Database compromise.

Prevention:

- Parameterized queries.
- Safe database APIs.
- Input validation.
- Least-privilege database accounts.

### 7.2 Cross-Site Scripting (XSS)

Cross-Site Scripting occurs when an application improperly handles untrusted content that is interpreted as executable script in another user's browser.

Potential consequences:

- Unauthorized actions in a user's browser context.
- Exposure of sensitive page information.
- Manipulation of page content.

Prevention:

- Context-aware output encoding.
- Safe templating.
- Input handling.
- Content Security Policy (CSP).

### 7.3 Cross-Site Request Forgery (CSRF)

CSRF occurs when a victim's authenticated browser is tricked into submitting an unwanted request to an application.

Prevention:

- CSRF tokens.
- Appropriate SameSite cookie settings.
- Origin and request validation.
- Reauthentication for sensitive actions.

### 7.4 Broken Authentication

Broken authentication refers to weaknesses in how an application verifies users and manages authentication sessions.

Examples:

- Weak password policies.
- Poor session handling.
- Insecure password reset processes.
- Missing multi-factor authentication where appropriate.

Prevention:

- Secure password hashing.
- Multi-factor authentication.
- Secure session management.
- Rate limiting.
- Strong recovery procedures.

### 7.5 Broken Access Control

Broken access control occurs when users can perform actions or access resources beyond their authorized permissions.

Example:

A student changes a record identifier in a URL and gains access to another student's record.

Prevention:

- Server-side authorization checks.
- Role-based access control.
- Object-level permission checks.
- Deny-by-default policies.

### 7.6 Security Misconfiguration

Security misconfiguration occurs when systems are configured insecurely.

Examples:

- Default passwords.
- Unnecessary services.
- Exposed administrative interfaces.
- Debug mode enabled in production.
- Unnecessary directory listing.

Prevention:

- Secure configuration baselines.
- Regular updates.
- Configuration reviews.
- Removing unnecessary services.
- Secure deployment procedures.

## 8. Authentication vs Authorization

These two concepts are related but different.

### Authentication

Authentication answers:

"Who are you?"

Example:

A student enters a username and password to log into the portal.

### Authorization

Authorization answers:

"What are you allowed to do?"

Example:

A student can view their results, but only an authorized lecturer can submit examination scores.

| Authentication | Authorization |
|---|---|
| Verifies identity | Determines permissions |
| Happens during identity verification | Applies to access decisions |
| Uses credentials or other authenticators | Uses roles, policies and permissions |
| Example: login | Example: access an admin page |

Remember:

Authentication does not automatically mean a user is authorized to access every resource.

## 9. HTTP and HTTPS

HTTP stands for Hypertext Transfer Protocol.

It is used for communication between web clients and servers.

HTTPS is HTTP protected using TLS.

HTTPS provides:

- Encryption.
- Integrity protection.
- Server authentication through certificates.

HTTP generally sends application data without TLS protection, while HTTPS protects the connection using TLS.

HTTPS does not guarantee that a website is trustworthy or free from vulnerabilities.

A malicious website can also use HTTPS.

## 10. OWASP and Web Application Security

OWASP stands for Open Worldwide Application Security Project.

It is a nonprofit organization that provides resources for improving application security.

One important OWASP resource is the OWASP Top 10.

It highlights major categories of web application security risks.

Examples include:

- Broken Access Control.
- Security Misconfiguration.
- Software Supply Chain Failures.
- Cryptographic Failures.
- Injection.

The OWASP Top 10 is an awareness document, not a complete vulnerability testing checklist.

You will study it in detail in Lesson 05.

## 11. Security Testing Principles

When assessing a web application, follow a structured process.

### Step 1: Define Scope

Identify the application and systems you are authorized to assess.

### Step 2: Understand the Application

Identify:

- Main functions.
- User roles.
- Login processes.
- Application endpoints.
- Data handled by the application.

### Step 3: Identify Security Controls

Review:

- Authentication.
- Authorization.
- Input validation.
- Session management.
- Encryption.
- Logging.

### Step 4: Test Safely

Use authorized test accounts and non-destructive techniques.

### Step 5: Document Findings

Record:

- Observation.
- Evidence.
- Impact.
- Severity.
- Recommendation.

### Step 6: Verify Remediation

Confirm whether the recommended security fix works.

## 12. Practical Lab 1 – Inspect Your Web Environment

Open Kali Linux.

Check whether Python is installed:

```bash
python3 --version
```

Check whether curl is installed:

```bash
curl --version
```

Check whether a browser is available:

```bash
which firefox
```

Check your network interfaces:

```bash
ip addr
```

These commands help identify the tools and environment available for web security practice.

## 13. Practical Lab 2 – Create a Local Web Security Workspace

Create a dedicated directory:

```bash
mkdir -p ~/practical-labs/module12/lab01
```

Enter the directory:

```bash
cd ~/practical-labs/module12/lab01
```

Create three files:

```bash
touch index.html
touch security-notes.md
touch observations.txt
```

Verify:

```bash
ls -la
```

Expected structure:

```text
lab01/
├── index.html
├── security-notes.md
└── observations.txt
```

## 14. Practical Lab 3 – Create a Basic HTML Page

Open the HTML file:

```bash
nano index.html
```

Enter:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Web Security Lab</title>
</head>
<body>

    <h1>Web Security Fundamentals</h1>

    <p>This is my first local web security laboratory.</p>

    <h2>Security Objectives</h2>

    <ul>
        <li>Confidentiality</li>
        <li>Integrity</li>
        <li>Availability</li>
    </ul>

</body>
</html>
```

Save:

- Press CTRL + O.
- Press ENTER.
- Press CTRL + X.

## 15. Practical Lab 4 – Start a Local Web Server

Inside your lab directory, run:

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

This starts a simple local HTTP server.

Important:

- `8000` is the port number.
- `127.0.0.1` means the loopback interface.
- The server is accessible only from the local machine through that interface.

Open another Kali terminal and run:

```bash
curl -I http://127.0.0.1:8000
```

You should receive an HTTP response containing a status code such as:

```text
HTTP/1.0 200 OK
```

This confirms that your local web server responded successfully.

Stop the server using:

```text
CTRL + C
```

## 16. Practical Lab 5 – Inspect HTTP Response Headers

Start the local server again:

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

From another terminal:

```bash
curl -I http://127.0.0.1:8000
```

Inspect the response headers.

Look for:

- HTTP status.
- Server header.
- Content type.
- Other headers returned by the server.

Record your observations:

```bash
nano observations.txt
```

Answer:

1. Did the server return a successful response?
2. Which HTTP version was displayed?
3. What server information was disclosed?
4. Why should unnecessary server information disclosure be reviewed?
5. Does a successful HTTP response prove that the application is secure?

Stop the server when finished.

## 17. Practical Lab 6 – Document Your First Web Security Observation

Open:

```bash
nano security-notes.md
```

Enter:

```markdown
# Web Security Lab 01

## Application
Local Python HTTP Server

## Address
127.0.0.1:8000

## Assessment Scope
Local machine only.

## Observation
Document the HTTP response and headers.

## Potential Security Considerations
Explain any information disclosed by the server.

## Risk
Do not assign a vulnerability severity without sufficient evidence.

## Recommendation
Describe possible improvements appropriate to the application.

## Conclusion
Summarize what you learned.
```

Save the document.

## 18. Knowledge Check

Answer these questions in your notebook.

1. What is web security?
2. Differentiate a static website from a dynamic website.
3. What are the three major components of a typical web application?
4. Explain how a browser communicates with a web server.
5. What is an attack surface?
6. Explain the CIA Triad.
7. What is SQL Injection?
8. What is Cross-Site Scripting?
9. Differentiate authentication from authorization.
10. What is the difference between HTTP and HTTPS?
11. What does OWASP stand for?
12. Why is server-side input validation important?
13. What is security misconfiguration?
14. Why should web security assessments have a clearly defined scope?
15. Does HTTPS guarantee that a website is safe? Explain.

## 19. Lesson Completion Checklist

- [ ] Understand web security fundamentals.
- [ ] Understand website and web application architecture.
- [ ] Understand frontend, backend and database components.
- [ ] Understand the attack surface.
- [ ] Understand the CIA Triad.
- [ ] Identify common web application threats.
- [ ] Understand authentication and authorization.
- [ ] Understand HTTP and HTTPS.
- [ ] Create a local web security workspace.
- [ ] Build a basic HTML page.
- [ ] Start a local HTTP server.
- [ ] Inspect HTTP response headers.
- [ ] Document your observations.

**END OF LESSON 01**
