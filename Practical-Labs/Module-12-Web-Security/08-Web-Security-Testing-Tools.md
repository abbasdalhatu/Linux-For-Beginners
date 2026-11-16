
# MODULE 12 – WEB SECURITY

# LESSON 08: WEB SECURITY TESTING TOOLS

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain the purpose of web security testing tools.
2. Understand how Burp Suite works.
3. Use OWASP ZAP for basic web security analysis.
4. Inspect HTTP requests and responses using curl.
5. Use browser Developer Tools.
6. Identify web services using Nmap.
7. Understand Nikto and web server scanning.
8. Collect evidence and document security findings.

---

## 2. Introduction to Web Security Testing Tools

Web security testing tools help professionals inspect websites and applications for security weaknesses.

They can help identify:

- Insecure HTTP headers.
- Authentication weaknesses.
- Session management problems.
- Input validation problems.
- Exposed services.
- Server configuration weaknesses.
- Potentially vulnerable software.

### Categories of Web Security Tools

| Category | Purpose | Examples |
|---|---|---|
| Intercepting proxies | Inspect HTTP traffic | Burp Suite, ZAP |
| HTTP clients | Send and inspect requests | curl |
| Network scanners | Identify ports and services | Nmap |
| Web server scanners | Check common server weaknesses | Nikto |
| Browser inspection | Analyze web application behavior | Chrome DevTools |
| Vulnerability scanners | Identify potential security weaknesses | ZAP |

### Important Principle

A security tool does not automatically prove that a vulnerability exists.

A tool produces observations. A security analyst must investigate, validate, and interpret those observations.

---

## 3. Burp Suite

Burp Suite is a web application security testing platform developed by PortSwigger.

It is commonly used by penetration testers and application security engineers.

### What Does Burp Suite Do?

Burp Suite acts as an intermediary between a browser and a web server.

Normal communication:

```text
Browser  ---------------->  Web Server
```

Communication through Burp Suite:

```text
Browser  ---->  Burp Suite  ---->  Web Server
```

This allows a tester to inspect HTTP requests and responses.

### Important Burp Suite Components

| Component | Purpose |
|---|---|
| Proxy | Intercepts and inspects HTTP traffic |
| Repeater | Replays and modifies individual requests |
| Intruder | Automates customized request testing |
| Decoder | Encodes and decodes data |
| Comparer | Compares requests and responses |
| Scanner | Performs automated testing in supported editions |

### Example

Suppose you log in to a local web application.

The browser may send:

```http
POST /login HTTP/1.1
Host: 127.0.0.1:8000
Content-Type: application/x-www-form-urlencoded
```

Burp Proxy allows you to examine the request structure and understand how login information is transmitted.

### Community vs Professional Edition

Burp Suite Community Edition provides essential manual testing capabilities.

Burp Suite Professional includes additional capabilities, including automated vulnerability scanning.

Do not assume that every feature shown in professional tutorials is available in the Community Edition.

### When to Use Burp Suite

- Understanding login requests.
- Inspecting cookies.
- Reviewing HTTP headers.
- Examining application parameters.
- Manually investigating application behavior.

---

## 4. OWASP ZAP

OWASP ZAP stands for OWASP Zed Attack Proxy.

It is an open-source web application security testing tool.

### Main Features

- Intercepting proxy.
- Passive scanning.
- Active scanning.
- Traditional spidering.
- Client-side application exploration.
- Request and response inspection.
- Security alerts and reports.

### Passive Scanning

Passive scanning analyzes requests and responses without modifying them or deliberately sending attack payloads.

It may identify:

- Missing security headers.
- Cookie configuration issues.
- Information disclosure.
- Other observable weaknesses.

### Active Scanning

Active scanning sends additional requests intended to test for vulnerabilities.

It may:

- Submit unexpected input.
- Test application parameters.
- Trigger application errors.
- Interact with application functionality.

Active scanning can affect an application and its data.

Only perform it against deliberately prepared lab targets or systems you are explicitly authorized to test.

### Burp Suite vs OWASP ZAP

| Feature | Burp Suite | OWASP ZAP |
|---|---|---|
| Intercepting proxy | Yes | Yes |
| Request inspection | Yes | Yes |
| Manual testing | Yes | Yes |
| Passive analysis | Yes | Yes |
| Automated scanning | Edition-dependent | Available |
| Open source | No | Yes |

### Important Note

ZAP passive scanning is generally safe because it does not change requests or responses. However, active scanning must be carefully scoped and authorized.

---

## 5. curl – Command-Line HTTP Testing

curl is a command-line utility used to communicate with servers through HTTP and other network protocols.

It is useful for understanding web communication without opening a graphical browser.

### Important Commands

Check curl:

```bash
curl --version
```

Inspect response headers:

```bash
curl -I http://127.0.0.1:8000
```

Display headers and body:

```bash
curl -i http://127.0.0.1:8000
```

Display detailed connection information:

```bash
curl -v http://127.0.0.1:8000
```

Follow redirects:

```bash
curl -L http://127.0.0.1:8000
```

Save response content:

```bash
curl -o response.html http://127.0.0.1:8000
```

Save response headers:

```bash
curl -D response-headers.txt -o response.html http://127.0.0.1:8000
```

### Understanding the Options

| Option | Meaning |
|---|---|
| `-I` | Request headers only |
| `-i` | Include headers in output |
| `-v` | Verbose connection information |
| `-L` | Follow redirects |
| `-o` | Save response body |
| `-D` | Save response headers |

---

## 6. Browser Developer Tools

Modern browsers include built-in tools for examining web applications.

Chrome and Microsoft Edge both provide Developer Tools.

### Opening Developer Tools

1. Open Chrome or Edge.
2. Visit your local test application.
3. Press `F12`.
4. Select the Network tab.
5. Refresh the page.
6. Select a request.
7. Inspect its details.

### Important Panels

| Panel | Purpose |
|---|---|
| Elements | Inspect HTML and CSS |
| Console | View JavaScript messages and errors |
| Network | Inspect HTTP requests and responses |
| Application | Inspect cookies and browser storage |
| Security | Inspect connection security information |
| Sources | Inspect loaded application resources |

### What Can You Inspect?

In the Network panel, examine:

- Request URL.
- HTTP method.
- Status code.
- Request headers.
- Response headers.
- Response size.
- Timing information.

### Why This Matters

A tester can use browser Developer Tools to understand what happens when a user:

- Logs in.
- Submits a form.
- Loads a page.
- Changes account settings.
- Interacts with an application.

---

## 7. Nmap – Network and Web Service Identification

Nmap is a network discovery and security auditing tool.

It can identify:

- Open ports.
- Running services.
- Service versions.
- Host availability.
- Some operating system characteristics.

### Important Commands

Check installation:

```bash
nmap --version
```

Scan your local web server:

```bash
nmap -sT -p 8000 127.0.0.1
```

Perform service version detection:

```bash
nmap -sV -p 8000 127.0.0.1
```

Save results:

```bash
nmap -sV -p 8000 127.0.0.1 -oN nmap-results.txt
```

### Understanding Results

| Result | Meaning |
|---|---|
| open | A service is accepting connections |
| closed | The port is reachable but no service is listening |
| filtered | Filtering or network behavior prevents a clear determination |

### Important Options

- `-sT`: TCP connect scan.
- `-sV`: Service version detection.
- `-p`: Specify port.
- `-oN`: Save results in normal text format.

Service identification is not always exact. The information should be treated as evidence to investigate rather than guaranteed truth.

---

## 8. Nikto – Web Server Security Scanner

Nikto is an open-source web server assessment tool.

It checks for various potentially unsafe server conditions.

### What Can Nikto Identify?

- Default server files.
- Commonly exposed resources.
- Known configuration weaknesses.
- Outdated software indicators.
- Potentially dangerous server behaviors.

### Important Limitation

Nikto is not a complete web application security testing solution.

It may produce:

- Informational findings.
- False positives.
- Findings requiring manual validation.

A result from Nikto does not automatically establish that a server is exploitable.

For this lesson, understand its purpose and do not run broad scans against external websites.

---

## 9. Tool Selection Guide

| Security Task | Recommended Tool |
|---|---|
| Inspect HTTP headers | curl |
| Intercept browser requests | Burp Suite or ZAP |
| Replay an HTTP request | Burp Repeater |
| Identify open ports | Nmap |
| Identify web server weaknesses | Nikto |
| Inspect cookies | Browser Developer Tools |
| Examine network service versions | Nmap |
| Review passive web security alerts | OWASP ZAP |

A professional tester chooses tools according to the assessment objective rather than using every available tool.

---

# PRACTICAL LABS

## 10. Practical Lab 1 – Verify Your Tools

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module12/lab08
cd ~/practical-labs/module12/lab08
```

Check available tools:

```bash
which curl
which nmap
which zaproxy
which burpsuite
which nikto
```

Some tools may not be installed.

That is normal. Do not install unnecessary software just to complete this exercise.

Check curl:

```bash
curl --version
```

Check Nmap:

```bash
nmap --version
```

Create your inventory:

```bash
nano tool-inventory.txt
```

Enter:

```text
WEB SECURITY TOOL INVENTORY

1. curl
Installed:
Version:
Purpose:

2. Nmap
Installed:
Version:
Purpose:

3. Burp Suite
Installed:
Version:
Purpose:

4. OWASP ZAP
Installed:
Version:
Purpose:

5. Nikto
Installed:
Version:
Purpose:

6. Browser Developer Tools
Available:
Purpose:
```

Complete the inventory.

---

## 11. Practical Lab 2 – HTTP Inspection Using curl

Start your local Python server:

```bash
cd ~/practical-labs/module12/lab04
python3 -m http.server 8000 --bind 127.0.0.1
```

Keep this terminal open.

Open a second terminal.

Inspect the headers:

```bash
curl -I http://127.0.0.1:8000
```

Inspect the complete response:

```bash
curl -i http://127.0.0.1:8000
```

Save the response:

```bash
curl -i http://127.0.0.1:8000 > http-response.txt
```

Read it:

```bash
cat http-response.txt
```

### Answer These Questions

1. What HTTP status code was returned?
2. What server software was disclosed?
3. What content type was returned?
4. Was a security header present?
5. What is the difference between `curl -I` and `curl -i`?

---

## 12. Practical Lab 3 – Local Nmap Assessment

Ensure your Python server is still running.

In your second terminal, run:

```bash
nmap -sT -p 8000 127.0.0.1
```

Then:

```bash
nmap -sV -p 8000 127.0.0.1
```

Save your results:

```bash
nmap -sV -p 8000 127.0.0.1 -oN nmap-results.txt
```

Read the results:

```bash
cat nmap-results.txt
```

### Record the Following

```text
NMAP LOCAL ASSESSMENT

Target: 127.0.0.1
Port: 8000

Port State:
Service:
Version Information:

Observations:
```

### Questions

1. What does an open port indicate?
2. What does `-sV` do?
3. Why can service identification be inaccurate?
4. Why is the scan restricted to localhost?

---

## 13. Practical Lab 4 – Browser Network Inspection

1. Open Chrome or Edge.
2. Visit `http://127.0.0.1:8000`.
3. Press `F12`.
4. Open the Network panel.
5. Refresh the page.
6. Select the main document request.
7. Inspect its headers.

Record:

```text
BROWSER NETWORK INSPECTION

Request URL:

HTTP Method:

Status Code:

Request Headers:

Response Headers:

Content Type:

Observations:
```

### Compare Your Results

Compare the information from Developer Tools with the output from curl.

Do both tools show the same server response?

Explain any differences.

---

## 14. Practical Lab 5 – Understand Burp Suite and ZAP

This exercise focuses on understanding proxy operation without performing attacks.

### Part A: Understand the Proxy

Draw this communication flow in your notebook:

```text
Browser
   |
   v
Burp Suite / OWASP ZAP
   |
   v
Local Web Application
```

Explain the role of each component.

### Part B: Compare the Tools

Complete this table:

| Feature | Burp Suite | OWASP ZAP |
|---|---|---|
| Intercept HTTP requests | | |
| Inspect response headers | | |
| Modify requests manually | | |
| Passive scanning | | |
| Automated scanning | | |
| Open-source | | |

### Part C: Understand Scope

A testing scope defines which systems and resources are authorized for assessment.

For this course, your approved target is:

```text
127.0.0.1:8000
```

Do not add public websites or third-party systems to your testing scope.

---

## 15. Practical Lab 6 – Analyze Nikto Findings Conceptually

Imagine Nikto produces these fictional observations:

```text
1. Server version information disclosed.
2. Default test page detected.
3. Missing security header.
4. Potentially exposed backup file.
```

For each observation, identify:

- What the finding means.
- Whether it is confirmed or requires validation.
- What evidence should be collected.
- What defensive recommendation should be made.

Use this table:

| Observation | Validation Required? | Recommendation |
|---|---|---|
| Version disclosure | | |
| Default test page | | |
| Missing security header | | |
| Exposed backup file | | |

Remember: scanner observations are not automatically confirmed vulnerabilities.

---

## 16. Practical Lab 7 – Build a Web Security Tool Report

Create a report:

```bash
nano web-tools-report.txt
```

Use this template:

```text
WEB SECURITY TESTING TOOLS REPORT

1. ASSESSMENT INFORMATION

Assessment Type:
Authorized Local Lab

Target:
127.0.0.1:8000

Date:
[Enter date]

2. TOOLS USED

Tool 1:
Purpose:

Tool 2:
Purpose:

Tool 3:
Purpose:

3. HTTP INSPECTION

HTTP Status:
Server:
Content Type:
Security Headers:

4. NETWORK IDENTIFICATION

Target:
Port:
State:
Service:

5. BROWSER INSPECTION

Request Method:
Response Status:
Important Observations:

6. SECURITY OBSERVATIONS

Observation 1:
Observation 2:
Observation 3:

7. LIMITATIONS

What could not be tested?

8. RECOMMENDATIONS

Recommendation 1:
Recommendation 2:
Recommendation 3:

9. CONCLUSION

Summary:
```

Complete the report using your actual observations.

Do not invent findings.

---

## 17. Safe Testing Rules

Always follow these rules:

1. Test only systems you own or are explicitly authorized to assess.
2. Keep the lab target within the approved scope.
3. Prefer passive inspection before active testing.
4. Do not run destructive tests against production systems.
5. Do not expose real credentials or session tokens in reports.
6. Validate scanner findings manually.
7. Keep evidence accurate and reproducible.
8. Record tool versions and testing limitations.

---

## 18. Knowledge Check

Answer these questions in your notebook.

1. What are web security testing tools?
2. What is Burp Suite?
3. What does Burp Proxy do?
4. What is OWASP ZAP?
5. Differentiate passive and active scanning.
6. What is curl used for?
7. What does `curl -I` do?
8. What is Nmap?
9. What does Nmap's `-sV` option do?
10. What is Nikto?
11. Name four browser Developer Tools panels.
12. What is the purpose of an intercepting proxy?
13. Why can automated scanners produce false positives?
14. What is testing scope?
15. Why is evidence collection important?

---

## 19. Lesson Completion Checklist

- [ ] Understand web security testing tools.
- [ ] Study Burp Suite.
- [ ] Study OWASP ZAP.
- [ ] Learn curl.
- [ ] Study browser Developer Tools.
- [ ] Learn Nmap service identification.
- [ ] Understand Nikto.
- [ ] Complete Practical Lab 1.
- [ ] Complete Practical Lab 2.
- [ ] Complete Practical Lab 3.
- [ ] Complete Practical Lab 4.
- [ ] Complete Practical Lab 5.
- [ ] Complete Practical Lab 6.
- [ ] Complete Practical Lab 7.
- [ ] Complete the knowledge check.
- [ ] Save the final report.

---

## 20. Final Understanding

Web security testing tools help professionals understand how web applications communicate, identify potential weaknesses, and collect technical evidence.

However, tools do not replace professional judgment.

A capable security analyst must know:

- Which tool to use.
- Why the tool is appropriate.
- How to interpret its output.
- How to validate findings.
- How to document the evidence.
- How to recommend practical improvements.

**Remember: Tools discover clues. Professional analysis turns those clues into reliable security findings.**
