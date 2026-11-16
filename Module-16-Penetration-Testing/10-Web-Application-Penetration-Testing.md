
# MODULE 16 – LESSON 10
# WEB APPLICATION PENETRATION TESTING

## 1. Introduction

A web application is software that runs on a web server and can be accessed through a web browser or an API.

Examples include:
- Online banking systems.
- University portals.
- E-commerce websites.
- Social media platforms.
- Online registration systems.
- Content management systems.

Web application penetration testing is the authorized process of evaluating a web application's security to identify weaknesses that could expose information, allow unauthorized access, or disrupt normal operations.

### Simple Explanation

Imagine a university student portal.

Students can:
- Log in.
- Register courses.
- Check results.
- Update personal information.

A security professional examines whether these functions are properly protected.

For example:
- Can a student access another student's results?
- Are passwords handled securely?
- Can unauthorized users access administrative pages?
- Is sensitive information exposed?

The goal is to identify and help fix weaknesses before malicious attackers exploit them.

## 2. How Web Applications Work

A typical web application involves three main components.

### A. Client

The client is usually the user's browser.

Examples:
- Google Chrome.
- Microsoft Edge.
- Mozilla Firefox.

The client sends requests and displays responses.

### B. Web Server

The web server receives HTTP requests and returns responses.

Examples:
- Apache HTTP Server.
- Nginx.
- Microsoft IIS.

### C. Database

The database stores application information.

Examples:
- MySQL.
- PostgreSQL.
- Microsoft SQL Server.
- MongoDB.

### Basic Communication Flow

1. A user opens a website.
2. The browser sends an HTTP request.
3. The web server receives the request.
4. Application logic processes it.
5. The database may be queried.
6. The server returns an HTTP response.
7. The browser displays the result.

## 3. Understanding HTTP and HTTPS

HTTP stands for Hypertext Transfer Protocol.

It is the communication protocol used by web applications.

HTTPS is HTTP protected by TLS encryption.

### Common HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve information |
| POST | Submit information |
| PUT | Create or replace a resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |
| HEAD | Retrieve response headers |

Not every application supports every method.

### HTTP Status Codes

| Status Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 301 | Moved Permanently |
| 302 | Found / Redirect |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |
| 503 | Service Unavailable |

Understanding these responses helps security professionals interpret application behavior.

## 4. Understanding the OWASP Top 10

OWASP stands for Open Worldwide Application Security Project.

It provides security guidance, tools, and educational resources for application security.

The OWASP Top 10:2021 contains ten major web application risk categories.

| Category | Description |
|---|---|
| A01: Broken Access Control | Users can access resources they should not access |
| A02: Cryptographic Failures | Sensitive data is inadequately protected |
| A03: Injection | Untrusted input is interpreted as commands or queries |
| A04: Insecure Design | Security weaknesses exist in application design |
| A05: Security Misconfiguration | Insecure application or infrastructure settings |
| A06: Vulnerable and Outdated Components | Software dependencies contain known weaknesses |
| A07: Identification and Authentication Failures | Authentication mechanisms are weak |
| A08: Software and Data Integrity Failures | Software or data integrity is not adequately protected |
| A09: Security Logging and Monitoring Failures | Security events are inadequately recorded or monitored |
| A10: Server-Side Request Forgery (SSRF) | A server can be induced to make unintended requests |

OWASP publishes updated editions, so always identify the edition being used.

## 5. Common Web Application Vulnerabilities

### A. SQL Injection

SQL injection occurs when untrusted input is improperly included in a database query.

It can potentially allow an attacker to:
- Access unauthorized database records.
- Modify information.
- Bypass application controls.
- Expose sensitive data.

**Defensive measures:**
- Use parameterized queries.
- Validate input.
- Apply least-privilege database permissions.
- Avoid constructing SQL queries through unsafe string concatenation.

### B. Cross-Site Scripting (XSS)

XSS occurs when untrusted content is handled in a way that allows malicious scripts to execute in another user's browser.

Common categories include:
- Stored XSS.
- Reflected XSS.
- DOM-based XSS.

**Defensive measures:**
- Context-aware output encoding.
- Safe HTML sanitization.
- Content Security Policy.
- Secure application development practices.

### C. Broken Access Control

Broken access control occurs when an application fails to enforce appropriate permissions.

Example:

A student should only view their own academic results.

If the application allows the student to view another student's records, access control may be broken.

**Defensive measures:**
- Enforce authorization on the server.
- Apply least privilege.
- Check ownership of requested resources.
- Test permissions for every sensitive operation.

### D. Authentication Failures

Authentication weaknesses can allow unauthorized users to gain access.

Examples:
- Weak password policies.
- Insecure session handling.
- Poor account recovery processes.
- Missing protection against repeated login attempts.

**Defensive measures:**
- Use strong authentication.
- Enable multifactor authentication where appropriate.
- Secure session management.
- Apply rate limiting.
- Protect account recovery processes.

### E. Security Misconfiguration

This occurs when systems or applications are configured insecurely.

Examples:
- Default credentials.
- Debug mode enabled in production.
- Unnecessary services.
- Publicly accessible administrative interfaces.
- Exposed sensitive configuration files.

**Defensive measures:**
- Use secure configuration baselines.
- Remove unnecessary features.
- Disable production debugging.
- Review server permissions.
- Keep software updated.

## 6. Understanding Cookies and Sessions

### Cookies

Cookies are small pieces of data stored by a browser.

Web applications commonly use cookies to maintain session information and user preferences.

### Sessions

A session represents a user's interaction with an application over a period of time.

A session identifier may be stored in a cookie.

### Important Cookie Security Attributes

| Attribute | Purpose |
|---|---|
| Secure | Sends the cookie over secure HTTPS connections |
| HttpOnly | Prevents ordinary JavaScript access to the cookie |
| SameSite | Controls cross-site cookie sending behavior |
| Path | Restricts the URL paths for which a cookie is sent |
| Domain | Defines the cookie's domain scope |

Sensitive session cookies should be protected carefully.

## 7. Web Application Testing Methodology

A structured web application assessment may follow these stages.

### Stage 1: Define Scope

Identify:
- Authorized website or application.
- Permitted testing accounts.
- Allowed testing techniques.
- Testing schedule.
- Prohibited actions.

### Stage 2: Application Discovery

Understand the application's:
- Main pages.
- Login process.
- User roles.
- API endpoints.
- Input fields.
- Important functions.

### Stage 3: Authentication Assessment

Review:
- Login security.
- Password handling.
- Account recovery.
- Multifactor authentication.
- Session management.

### Stage 4: Authorization Assessment

Check whether users can access only the resources they are permitted to access.

### Stage 5: Input Validation Assessment

Review how the application handles user input.

### Stage 6: Configuration Assessment

Inspect:
- Security headers.
- TLS configuration.
- Error handling.
- Server configuration.
- Software dependencies.

### Stage 7: Reporting

Document:
- Findings.
- Evidence.
- Impact.
- Severity.
- Recommendations.

## 8. Common Web Application Security Tools

| Tool | Purpose |
|---|---|
| Burp Suite | Web application security testing |
| OWASP ZAP | Web application security testing |
| Nikto | Web server security assessment |
| curl | HTTP requests and response inspection |
| Browser Developer Tools | Inspect browser requests and responses |
| Nmap | Authorized network and service discovery |
| SSL Labs | Public TLS configuration assessment |

Tools should be used only against applications within your authorized testing scope.

## 9. Practical Lab: Inspecting a Web Application

### Lab Objective

Learn how HTTP requests and responses work using a documentation website.

This exercise does not require attacking or exploiting a web application.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module16-lab/lesson10
cd ~/module16-lab/lesson10
```

### Step 2: Inspect an HTTP Response

```bash
curl -I https://example.com
```

Record:
- HTTP status.
- Server header, if provided.
- Content-Type.
- Other response headers.

### Step 3: Inspect the Full HTTP Response

```bash
curl -v https://example.com
```

Observe:
- DNS connection information.
- TLS connection information.
- Request headers.
- Response headers.
- HTTP status.

Be careful when sharing verbose output because it may include connection details.

### Step 4: Inspect Supported HTTP Methods

For the documentation domain:

```bash
curl -i -X OPTIONS https://example.com
```

Observe the response.

Do not assume that a method is supported simply because it appears in an HTTP specification. Interpret the actual server response.

### Step 5: Inspect TLS Certificate Information

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null
```

Observe:
- Certificate subject.
- Certificate issuer.
- Certificate validity.
- TLS connection information.

### Step 6: Inspect Your Browser

Open Google Chrome or Microsoft Edge.

1. Visit `https://example.com`.
2. Press `F12`.
3. Open the Network tab.
4. Refresh the page.
5. Select the main document request.
6. Review the request and response headers.
7. Identify the HTTP status code.

This introduces you to browser-based web traffic analysis.

## 10. Practical Exercise: Web Security Review

Imagine that you are reviewing a university student portal.

The portal has:
- Student login.
- Course registration.
- Results checking.
- Profile management.
- Administrator dashboard.

For each function, identify the security controls that should exist.

| Function | Security Questions |
|---|---|
| Login | Are passwords securely handled? |
| Course registration | Can unauthorized users modify registrations? |
| Results checking | Can students view only their own results? |
| Profile management | Can users modify only permitted information? |
| Administrator dashboard | Is access restricted to authorized administrators? |

Write five potential security risks and propose a defensive control for each.

Do not test a real university portal without explicit permission.

## 11. Create a Web Application Assessment Report

Create your report:

```bash
nano web-application-assessment-report.txt
```

Use this structure:

```text
MODULE 16 - LESSON 10
WEB APPLICATION PENETRATION TESTING REPORT

1. APPLICATION INFORMATION

Application Name:
Application Type:
Testing Date:
Authorized Scope:

2. APPLICATION COMPONENTS

Client:
Web Server:
Database:
Authentication Method:

3. HTTP OBSERVATIONS

HTTP Status:
Response Headers:
TLS Information:

4. SECURITY REVIEW

Authentication:
Authorization:
Session Management:
Input Validation:
Security Configuration:

5. POTENTIAL FINDINGS

Finding 1:
Description:
Evidence:
Impact:
Severity:

Finding 2:
Description:
Evidence:
Impact:
Severity:

6. RECOMMENDATIONS

Recommendation 1:
Recommendation 2:
Recommendation 3:

7. CONCLUSION

Overall Assessment:
Important Observations:
Recommended Next Steps:
```

Use actual observations and clearly label hypothetical findings.

## 12. Common Web Application Testing Mistakes

1. Testing websites without permission.
2. Assuming that HTTPS means an application is completely secure.
3. Ignoring authorization checks.
4. Treating every HTTP error as a vulnerability.
5. Ignoring session security.
6. Performing destructive tests without approval.
7. Reporting assumptions as confirmed vulnerabilities.
8. Failing to document evidence.

## 13. Defensive Security Recommendations

Organizations should:

1. Use secure authentication.
2. Enforce server-side authorization.
3. Protect sensitive information.
4. Use parameterized database queries.
5. Validate and safely handle input.
6. Configure secure cookies.
7. Implement appropriate security headers.
8. Keep application dependencies updated.
9. Monitor security events.
10. Perform regular authorized security assessments.

## 14. Knowledge Check

Answer these questions in your notebook.

1. What is a web application?
2. What is web application penetration testing?
3. Explain the roles of a client, web server, and database.
4. What is the difference between HTTP and HTTPS?
5. What does GET do?
6. What does POST do?
7. What does OWASP stand for?
8. What is SQL injection?
9. What is cross-site scripting?
10. Explain broken access control.
11. What is a session?
12. What is the purpose of the HttpOnly cookie attribute?
13. What is the purpose of the Secure cookie attribute?
14. Name five web application security testing tools.
15. Why must web application testing be authorized?

## 15. Practical Thinking Questions

1. Why is HTTPS not enough to guarantee application security?
2. How could broken access control affect a university student portal?
3. Why should database queries use parameterized statements?
4. What could happen if session cookies are exposed?
5. Why should administrators disable debugging in production environments?
6. How does multifactor authentication improve account security?
7. Why should web applications validate user input?
8. Why is server-side authorization necessary even when a website hides restricted buttons?

## 16. Lesson Summary

In this lesson, you learned:

- Web application fundamentals.
- Client-server architecture.
- HTTP and HTTPS.
- HTTP methods and status codes.
- OWASP Top 10.
- SQL injection.
- Cross-site scripting.
- Broken access control.
- Authentication failures.
- Cookies and sessions.
- Web application testing methodology.
- Common web security tools.
- HTTP and TLS inspection.
- Web application assessment reporting.

### Key Takeaway

A secure web application must protect user information, enforce authorization, handle input safely, manage sessions securely, and maintain appropriate server configurations.

Web application penetration testing helps organizations identify weaknesses before attackers exploit them.

**NEXT LESSON: MODULE 16 – LESSON 11: PASSWORD SECURITY ASSESSMENT**
