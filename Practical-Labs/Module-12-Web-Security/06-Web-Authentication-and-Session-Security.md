
# MODULE 12 – WEB SECURITY

# LESSON 06: WEB AUTHENTICATION AND SESSION SECURITY

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain web authentication and authorization.
2. Understand password security.
3. Explain multi-factor authentication (MFA).
4. Understand HTTP cookies and sessions.
5. Identify common session security vulnerabilities.
6. Understand secure session management.
7. Inspect cookies using browser developer tools.
8. Apply basic session security principles.

---

## 2. What Is Web Authentication?

Authentication is the process of verifying the identity of a user, device, or application.

In simple terms, authentication answers:

**Who are you?**

For example, when you log in to your university portal using your username and password, the application attempts to verify your identity.

Common authentication methods include:

- Password authentication.
- One-time passwords (OTP).
- Multi-factor authentication.
- Biometric authentication.
- Security keys.
- Digital certificates.
- Single sign-on (SSO).

### Authentication Process

A typical login process:

1. The user opens a login page.
2. The user enters their credentials.
3. The browser sends the login request securely.
4. The server verifies the credentials.
5. If successful, the server establishes an authenticated session.
6. The user can access authorized resources.

---

## 3. Authentication vs Authorization

These concepts are related but different.

| Authentication | Authorization |
|---|---|
| Verifies identity | Determines permissions |
| Who are you? | What can you access? |
| Happens during login | Applies when accessing resources |
| Uses credentials or other identity factors | Uses roles, policies, and permissions |

### Practical Example

Imagine a university management system.

- A student logs in using their credentials. This is authentication.
- The student accesses their own academic results. This is authorization.
- A lecturer accesses assigned course records. This is authorization.
- An administrator manages user accounts. This is authorization.

A user being authenticated does not mean they should have access to everything.

---

## 4. Password Security

Passwords are one of the most common authentication methods.

Weak password practices can allow unauthorized access.

### Examples of Weak Passwords

```text
123456
password
admin123
qwerty
```

These passwords are predictable.

### Characteristics of Strong Passwords

A strong password should:

- Be sufficiently long.
- Be unique to each account.
- Avoid predictable personal information.
- Resist common guessing patterns.
- Be stored securely by the application.

A password manager can help users create and maintain unique passwords.

### Secure Password Storage

Applications should never store passwords in plaintext.

Instead, they should use password-hashing algorithms such as:

- Argon2id.
- bcrypt.
- scrypt.

These algorithms are designed to make password guessing more expensive.

A unique salt should also be used for each password.

**Important:** Ordinary fast hashes such as SHA-256 are not appropriate replacements for dedicated password-hashing algorithms.

---

## 5. Multi-Factor Authentication (MFA)

Multi-factor authentication requires more than one independent authentication factor.

The three common factor categories are:

| Factor | Description | Example |
|---|---|---|
| Something you know | Knowledge | Password or PIN |
| Something you have | Possession | Security key or authenticator device |
| Something you are | Inherence | Fingerprint or facial recognition |

### Example

A user logs in with:

1. A password.
2. A time-based one-time password from an authenticator application.

This provides two different authentication factors.

### Common MFA Methods

- Authenticator applications.
- Hardware security keys.
- Passkeys.
- SMS-based OTP.
- Push-based authentication.

Not all MFA methods provide equal protection. Phishing-resistant methods, such as appropriately implemented passkeys and hardware security keys, offer stronger protection against many phishing attacks.

### Why MFA Matters

Even if a password is stolen, an attacker may still be unable to access the account without the additional authentication factor.

---

## 6. What Is a Web Session?

HTTP is a stateless protocol.

This means each HTTP request is independent by default. The server does not automatically know that two requests came from the same authenticated user.

Web applications use sessions or other mechanisms to maintain user state.

### Simple Example

A user logs into a university portal.

The server verifies the login and creates an authenticated session.

The browser receives a session identifier, commonly through a cookie.

The browser sends that cookie with later requests so the server can associate those requests with the authenticated session.

### Session Lifecycle

A typical session follows this process:

1. Session creation.
2. Session identifier issued.
3. Browser stores the identifier.
4. Browser sends it with relevant requests.
5. Server validates the session.
6. Session expires or is terminated.

---

## 7. What Are HTTP Cookies?

Cookies are small pieces of data stored by a browser for a website.

They are commonly used for:

- Session management.
- User preferences.
- Shopping carts.
- Authentication state.
- Analytics.

Example:

```http
Set-Cookie: session_id=abc123; Path=/; HttpOnly; Secure; SameSite=Lax
```

This is an illustrative example, not a real authentication token.

### Important Cookie Attributes

| Attribute | Purpose |
|---|---|
| Secure | Sends the cookie only over secure HTTPS connections |
| HttpOnly | Prevents ordinary JavaScript from reading the cookie |
| SameSite | Controls when browsers send cookies in cross-site contexts |
| Path | Limits the URL paths for which the cookie is sent |
| Domain | Controls which hosts may receive the cookie |
| Max-Age | Specifies the cookie's lifetime in seconds |
| Expires | Specifies an expiration date |

### Secure Cookie Example

```http
Set-Cookie: session_id=randomvalue; Secure; HttpOnly; SameSite=Lax
```

A real application should generate unpredictable session identifiers and validate them securely on the server.

---

## 8. Common Session Security Vulnerabilities

### A. Session Hijacking

Session hijacking occurs when an unauthorized person obtains another user's valid session identifier and uses it to impersonate that user.

Possible causes include:

- Exposure of session identifiers.
- Insecure network communication.
- Cross-site scripting.
- Malware or compromised devices.
- Insecure cookie handling.

Prevention:

- Use HTTPS.
- Set Secure and HttpOnly cookie attributes.
- Use unpredictable session identifiers.
- Rotate session identifiers after authentication.
- Expire sessions appropriately.

### B. Session Fixation

Session fixation occurs when an application allows an attacker-controlled or previously known session identifier to remain valid after the victim authenticates.

Prevention:

- Generate a new session identifier after login.
- Invalidate the old session identifier.
- Use secure session-management frameworks.

### C. Session Expiration Problems

A session may remain valid longer than necessary.

This creates additional risk if the user's device is lost, stolen, or compromised.

Prevention:

- Apply reasonable idle timeouts.
- Set absolute session lifetimes.
- Provide logout functionality.
- Revoke sessions after sensitive account changes when appropriate.

### D. Insecure Cookie Configuration

Cookies may be exposed to unnecessary risks when security attributes are missing or incorrectly configured.

Prevention:

- Enable Secure for HTTPS-only cookies.
- Enable HttpOnly for session cookies.
- Configure SameSite appropriately.
- Avoid unnecessarily broad cookie domains.

---

## 9. Cross-Site Request Forgery (CSRF)

CSRF is a vulnerability in which a victim's authenticated browser is tricked into sending an unwanted request to a website where they are already logged in.

### Example

Imagine a university portal allows an authenticated user to change account details.

If the application does not adequately protect sensitive state-changing requests, an attacker may attempt to make the victim's browser submit an unwanted action.

### Prevention

- Use anti-CSRF tokens.
- Configure SameSite cookies appropriately.
- Validate request origins where appropriate.
- Require reauthentication for sensitive operations.
- Avoid using GET requests for state-changing actions.

**Important:** SameSite is useful protection, but it should not automatically replace all other CSRF defenses.

---

## 10. Practical Lab 1 – Inspect Browser Cookies

Use your own local web application.

First, ensure your local server is running:

```bash
cd ~/practical-labs/module12/lab04
python3 -m http.server 8000 --bind 127.0.0.1
```

Open this address in your browser:

```text
http://127.0.0.1:8000
```

### Open Developer Tools

In Chrome or Edge:

1. Press `F12`.
2. Open the Application tab.
3. Find the Storage section.
4. Select Cookies.
5. Inspect the cookies associated with your local site.

The basic Python HTTP server used in the earlier lesson does not create an authentication session, so you may see no cookies.

That is expected.

### Your Task

Record:

- Does the site create any cookies?
- Is there a session cookie?
- Which cookie security attributes can you observe?
- Why should authentication cookies be protected?

Do not create or share real authentication tokens in your report.

---

## 11. Practical Lab 2 – Understand Session Cookie Security

Study this illustrative cookie:

```http
Set-Cookie: session_id=example123; Path=/; Secure; HttpOnly; SameSite=Strict
```

Identify the following:

1. Cookie name.
2. Cookie value.
3. Secure attribute.
4. HttpOnly attribute.
5. SameSite attribute.
6. Path attribute.

Create a report:

```bash
mkdir -p ~/practical-labs/module12/lab06
cd ~/practical-labs/module12/lab06
nano cookie-analysis.txt
```

Use this template:

```text
SESSION COOKIE SECURITY ANALYSIS

Cookie Name:
session_id

Secure:
Present / Absent

HttpOnly:
Present / Absent

SameSite:
Value:

Path:
Value:

Security Observations:

Recommendations:
```

Complete the analysis.

---

## 12. Practical Lab 3 – Password Hashing Demonstration

This exercise demonstrates the difference between a password and its cryptographic hash.

Create a file:

```bash
nano password-demo.py
```

Enter:

```python
import hashlib

password = input("Enter a demonstration password: ")

password_hash = hashlib.sha256(
    password.encode()
).hexdigest()

print("Demonstration SHA-256 hash:")
print(password_hash)
```

Run:

```bash
python3 password-demo.py
```

Enter a test password that you do not use for any real account.

Observe the output.

Run the program again using the same input.

The resulting hash should be the same because SHA-256 is deterministic.

### Important Security Lesson

This is only a demonstration of hashing.

SHA-256 is not a suitable password-storage algorithm by itself because it is designed to be fast.

Production applications should use dedicated password-hashing algorithms such as Argon2id, bcrypt, or scrypt, with appropriate salts and parameters.

---

## 13. Practical Lab 4 – Design a Secure Login System

Imagine you are designing a login system for a university portal.

Write a security checklist covering:

1. Password storage.
2. Login rate limiting.
3. Multi-factor authentication.
4. Session identifier generation.
5. Cookie configuration.
6. Session expiration.
7. Logout functionality.
8. Authorization checks.
9. Failed-login monitoring.
10. Password recovery.

Your objective is to design a system that protects users even when some security controls fail.

Save your checklist:

```bash
nano secure-login-design.txt
```

---

## 14. Recommended Secure Authentication Practices

A secure web application should:

- Use HTTPS for authenticated communication.
- Store passwords using dedicated password-hashing algorithms.
- Support MFA.
- Apply rate limiting to authentication attempts.
- Use secure session identifiers.
- Rotate session identifiers after authentication.
- Protect cookies with appropriate attributes.
- Expire inactive sessions.
- Invalidate sessions during logout.
- Enforce authorization on the server.
- Monitor suspicious authentication activity.
- Protect password-reset processes.

---

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is web authentication?
2. Differentiate authentication from authorization.
3. What is multi-factor authentication?
4. Name the three authentication factor categories.
5. Why should passwords not be stored in plaintext?
6. What is a web session?
7. Why are cookies used in session management?
8. Explain the Secure cookie attribute.
9. What does HttpOnly protect against?
10. What is session hijacking?
11. What is session fixation?
12. What is CSRF?
13. Why should session identifiers be rotated after login?
14. Why is SHA-256 alone unsuitable for password storage?
15. What is the purpose of session expiration?

---

## 16. Lesson Completion Checklist

- [ ] Understand authentication.
- [ ] Understand authorization.
- [ ] Study password security.
- [ ] Understand MFA.
- [ ] Understand web sessions.
- [ ] Study HTTP cookies.
- [ ] Learn the major session vulnerabilities.
- [ ] Complete Practical Lab 1.
- [ ] Complete Practical Lab 2.
- [ ] Complete Practical Lab 3.
- [ ] Complete Practical Lab 4.
- [ ] Complete the knowledge check.

---

## 17. Final Understanding

Authentication establishes identity, while authorization determines what an authenticated identity is allowed to access.

Sessions help web applications maintain authenticated state across multiple HTTP requests.

However, poorly managed sessions and cookies can expose users to serious security risks.

A professional web security analyst must understand how authentication works, how sessions are maintained, and how to protect the credentials and tokens that keep users secure.

**Remember: Protect the identity, protect the session, and enforce authorization on every sensitive operation.**
