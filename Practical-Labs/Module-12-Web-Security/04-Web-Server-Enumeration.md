
# MODULE 12 – WEB SECURITY

# LESSON 04: WEB SERVER ENUMERATION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what web server enumeration means.
2. Identify common web server technologies.
3. Inspect HTTP response headers.
4. Identify server software and versions when disclosed.
5. Understand common web server misconfigurations.
6. Use curl and browser developer tools for basic enumeration.
7. Document enumeration findings professionally.

---

## 2. What Is Web Server Enumeration?

Web server enumeration is the process of collecting information about a web server to understand how it is configured and what technologies it uses.

During an authorized security assessment, an analyst may investigate:

- Web server software.
- Server version information.
- HTTP response headers.
- Supported HTTP methods.
- Redirect behavior.
- TLS configuration.
- Default pages and exposed information.
- Potentially unsafe configurations.

### Simple Example

Imagine visiting a website and discovering that its response headers disclose:

```http
Server: Apache/2.4.58
Content-Type: text/html
```

This reveals that the server may be using Apache HTTP Server version 2.4.58.

This information can help a security analyst identify which configurations and software updates should be reviewed.

**Important:** A disclosed server version does not automatically mean that the server is vulnerable.

---

## 3. Common Web Server Technologies

### A. Apache HTTP Server

Apache is a widely used open-source web server.

Common characteristics:

- Supports Linux and Windows.
- Uses configuration files.
- Supports modules.
- Commonly serves PHP applications.

### B. Nginx

Nginx is commonly used as:

- A web server.
- A reverse proxy.
- A load balancer.
- A caching layer.

It is popular for handling large numbers of simultaneous connections.

### C. Microsoft IIS

Internet Information Services (IIS) is Microsoft's web server platform.

Common characteristics:

- Runs on Windows Server.
- Integrates with Microsoft technologies.
- Supports ASP.NET applications.

### D. Caddy

Caddy is a modern web server known for its automatic HTTPS capabilities.

---

## 4. Understanding HTTP Response Headers

HTTP response headers provide information about how a server responds to a request.

Example:

```http
HTTP/1.1 200 OK
Server: nginx
Content-Type: text/html
Content-Length: 1024
Connection: keep-alive
```

Important headers:

| Header | Purpose |
|---|---|
| Server | May disclose server software |
| Content-Type | Identifies the response content type |
| Content-Length | Indicates response size |
| Location | Provides a redirect destination |
| Set-Cookie | Sets browser cookies |
| Strict-Transport-Security | Enforces HTTPS in supporting browsers |
| X-Content-Type-Options | Helps prevent MIME-type sniffing |
| Content-Security-Policy | Restricts permitted content sources |
| X-Frame-Options | Controls whether a page can be embedded in frames |

Not every server discloses every header.

---

## 5. Practical Lab Environment

All practical activities in this lesson will use your own Kali environment and a local test server.

We will not scan public websites or systems belonging to other people.

### Step 1: Create a Working Directory

Open your Kali terminal:

```bash
mkdir -p ~/practical-labs/module12/lab04
cd ~/practical-labs/module12/lab04
```

Confirm your location:

```bash
pwd
```

### Step 2: Create a Test Web Page

Create a simple HTML page:

```bash
nano index.html
```

Enter:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Web Enumeration Lab</title>
</head>
<body>
    <h1>Web Security Practical Lab</h1>
    <p>This is my local web server.</p>
</body>
</html>
```

Save the file.

### Step 3: Start a Local HTTP Server

Run:

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

Your server is now listening on:

```text
http://127.0.0.1:8000
```

Keep this terminal open.

Open another terminal for the remaining activities.

---

## 6. Practical Lab: Inspect HTTP Headers

Use curl:

```bash
curl -I http://127.0.0.1:8000
```

The `-I` option requests response headers without downloading the page body.

You should see a response similar to:

```http
HTTP/1.0 200 OK
Server: SimpleHTTP/0.6 Python/3.x
Date: ...
Content-type: text/html
Content-Length: ...
```

### Understand the Results

- `200 OK`: The request succeeded.
- `Server`: Identifies the server software disclosed by the response.
- `Content-type`: Identifies the content format.
- `Content-Length`: Indicates the response size.

The exact values may differ depending on your Python version.

### Save the Results

```bash
curl -I http://127.0.0.1:8000 > headers.txt
```

View the saved information:

```bash
cat headers.txt
```

---

## 7. Practical Lab: Inspect the Full HTTP Response

Run:

```bash
curl -i http://127.0.0.1:8000
```

Difference between the commands:

| Command | Function |
|---|---|
| `curl -I` | Retrieves response headers |
| `curl -i` | Displays headers and response body |
| `curl -v` | Displays detailed request and connection information |

Try:

```bash
curl -v http://127.0.0.1:8000
```

Observe:

1. The HTTP request method.
2. The requested URL.
3. The response status.
4. The response headers.
5. The server's response body.

Do not share sensitive information if you later use verbose output with authenticated systems.

---

## 8. Practical Lab: Test HTTP Methods

HTTP methods describe the action requested from a server.

Common methods:

| Method | Purpose |
|---|---|
| GET | Retrieve information |
| HEAD | Retrieve response headers |
| POST | Submit information |
| PUT | Create or replace a resource |
| PATCH | Partially modify a resource |
| DELETE | Delete a resource |
| OPTIONS | Ask which methods are supported |

For this lesson, use only the safe `OPTIONS` request against your local server:

```bash
curl -i -X OPTIONS http://127.0.0.1:8000/
```

Observe the response.

**Important:** A server's response to OPTIONS is not necessarily a complete or reliable list of every method it supports. Never test destructive methods against systems without explicit authorization.

---

## 9. Practical Lab: Check Redirect Behavior

Use this command:

```bash
curl -I http://127.0.0.1:8000
```

Our local Python server normally responds directly without redirecting.

Now understand how redirects work.

A redirect response may look like:

```http
HTTP/1.1 301 Moved Permanently
Location: https://example.local/
```

Common redirect status codes:

- `301`: Permanent redirect.
- `302`: Temporary redirect.
- `307`: Temporary redirect that preserves the request method.
- `308`: Permanent redirect that preserves the request method.

Do not assume a redirect is safe simply because it uses HTTPS. Review the destination and certificate.

---

## 10. Common Web Server Misconfigurations

A web server may expose unnecessary information or be configured insecurely.

### A. Server Version Disclosure

Example:

```http
Server: Apache/2.4.58
```

Risk:

An attacker may use the disclosed version to research potential weaknesses.

Defensive recommendation:

- Keep server software updated.
- Minimize unnecessary version disclosure.
- Maintain an accurate software inventory.

### B. Directory Listing

A directory listing may reveal files and folders when no default page exists and directory browsing is enabled.

Potentially exposed information includes:

- Backup files.
- Configuration files.
- Application archives.
- Internal documentation.

Defensive recommendation:

Disable directory browsing unless it is explicitly required.

### C. Missing Security Headers

Missing security headers may reduce browser-side protection.

Defensive recommendation:

Review and configure suitable headers such as:

- Content-Security-Policy.
- X-Content-Type-Options.
- Strict-Transport-Security.
- Frame protection.

### D. Unnecessary Services

A web server may expose services that are not needed.

Defensive recommendation:

Disable unnecessary services and restrict administrative interfaces.

---

## 11. Practical Lab: Build an Enumeration Report

Create a report:

```bash
nano enumeration-report.txt
```

Use this template:

```text
WEB SERVER ENUMERATION REPORT

Target:
127.0.0.1:8000

Assessment Type:
Local authorized lab

Date:
[Enter date]

1. SERVER IDENTIFICATION
Server software:
Server version:

2. HTTP RESPONSE
Status code:
Content type:
Content length:

3. HTTP METHODS
OPTIONS response:

4. SECURITY OBSERVATIONS
Observation 1:
Observation 2:

5. RECOMMENDATIONS
Recommendation 1:
Recommendation 2:

6. CONCLUSION
Summary of findings:
```

Complete the report using the actual information collected from your local server.

Save it.

---

## 12. Important Commands Reference

| Command | Purpose |
|---|---|
| `curl -I URL` | Inspect response headers |
| `curl -i URL` | Display headers and body |
| `curl -v URL` | Display detailed request information |
| `curl -X OPTIONS URL` | Send an OPTIONS request |
| `curl -L URL` | Follow redirects |
| `curl -o file URL` | Save response body to a file |
| `cat headers.txt` | Display saved headers |
| `nano file.txt` | Edit a text file |

For this module, replace `URL` with your own local lab address unless you have explicit authorization to assess another system.

---

## 13. Knowledge Check

Answer these questions in your own notes.

1. What is web server enumeration?
2. What is the difference between Apache and Nginx?
3. What information can the Server HTTP header disclose?
4. What does HTTP status code 200 mean?
5. What is the purpose of the Content-Type header?
6. What is directory listing?
7. Why should unnecessary server version information be minimized?
8. What is the purpose of the OPTIONS method?
9. Name four useful HTTP security headers.
10. Why should enumeration be performed only on authorized systems?

---

## 14. Lesson Completion Checklist

Before marking this lesson complete, confirm that you have:

- [ ] Created the lab directory.
- [ ] Created the HTML test page.
- [ ] Started the local Python HTTP server.
- [ ] Inspected HTTP response headers.
- [ ] Saved the header information.
- [ ] Inspected the complete HTTP response.
- [ ] Tested the OPTIONS method.
- [ ] Studied redirect behavior.
- [ ] Created the enumeration report.
- [ ] Completed the knowledge check.

---

## 15. Final Understanding

Web server enumeration is an important part of web security assessment.

It helps security professionals understand the technologies, configurations, and information exposed by a web server.

However, identifying a technology or version is only the beginning. A proper security assessment must validate findings, understand their impact, and recommend practical defensive improvements.

**Professional rule: Enumerate only systems you own or have explicit permission to assess.**
