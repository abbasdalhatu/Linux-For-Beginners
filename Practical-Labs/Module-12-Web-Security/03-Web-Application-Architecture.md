
# MODULE 12 – WEB SECURITY

# LESSON 03: WEB APPLICATION ARCHITECTURE

## 1. Introduction

Web application architecture describes how the different components of a web application work together.

A modern web application usually contains:

- Frontend.
- Backend.
- Web server.
- Application server or runtime.
- Database.
- APIs.
- Authentication system.
- External services.

Understanding this architecture is important in cybersecurity because vulnerabilities can exist at different layers.

For example:

- A frontend may expose sensitive information.
- A backend may have broken authorization.
- A database may have weak access controls.
- An API may accept unauthorized requests.
- A web server may be incorrectly configured.

A security analyst must understand how these components communicate.

## 2. The Client-Server Architecture

Most web applications use a client-server model.

### Client

The client is the device or software requesting a service.

Examples:

- Google Chrome.
- Microsoft Edge.
- Firefox.
- Mobile applications.

### Server

The server receives requests, processes them, and returns responses.

Example:

A student opens a university portal. The browser requests the login page, and the server returns the page.

Architecture:

```text
+------------------+
| CLIENT           |
| Web Browser      |
+------------------+
         |
         | HTTP / HTTPS
         |
         v
+------------------+
| WEB SERVER       |
+------------------+
         |
         v
+------------------+
| BACKEND          |
| Application      |
+------------------+
         |
         v
+------------------+
| DATABASE         |
+------------------+
```

## 3. Frontend Architecture

The frontend is the part of the application users interact with.

It is commonly developed using three technologies.

### A. HTML

HTML stands for HyperText Markup Language.

It defines the structure of a web page.

Example:

```html
<h1>Student Portal</h1>
<p>Welcome to the university portal.</p>
```

### B. CSS

CSS stands for Cascading Style Sheets.

It controls the appearance of web pages.

Example:

```css
h1 {
    color: blue;
    font-size: 30px;
}
```

### C. JavaScript

JavaScript adds interactive functionality.

Example:

```javascript
alert("Welcome to the Student Portal");
```

### Frontend Security Considerations

Common security concerns include:

- Cross-Site Scripting.
- Sensitive information exposed in page source.
- Insecure storage of authentication tokens.
- Unsafe handling of user input.
- Clickjacking risks.
- Insecure third-party scripts.

Important:

Frontend validation improves usability, but it cannot replace backend validation.

An attacker can modify browser requests or bypass frontend controls.

## 4. Backend Architecture

The backend is responsible for processing requests and implementing application logic.

Common backend technologies include:

- Python.
- PHP.
- Java.
- Node.js.
- C#.
- Ruby.

Backend responsibilities include:

- Authentication.
- Authorization.
- Business logic.
- Input validation.
- Database communication.
- Session management.
- API processing.

### Example

A student submits login credentials.

The backend:

1. Receives the submitted credentials.
2. Validates the request.
3. Retrieves the relevant account record.
4. Verifies the password using a secure password-hashing process.
5. Creates an authenticated session if verification succeeds.
6. Returns an appropriate response.

The backend must never trust a user merely because the frontend displays a successful login message.

## 5. Database Architecture

A database stores and manages application information.

Examples:

- Student records.
- User accounts.
- Examination results.
- Product information.
- Transaction records.

Common database systems include:

| Database | Type |
|---|---|
| MySQL | Relational |
| PostgreSQL | Relational |
| SQLite | Relational |
| Microsoft SQL Server | Relational |
| MongoDB | Document-oriented |

### Example Database Table

Imagine a student management system.

| Student ID | Name | Department |
|---|---|---|
| 1001 | Ahmad | Computer Science |
| 1002 | Fatima | Information Technology |
| 1003 | Musa | Software Engineering |

### Database Security

Important controls include:

- Strong authentication.
- Least-privilege access.
- Parameterized queries.
- Encryption where appropriate.
- Secure backups.
- Audit logging.
- Network restrictions.

Never expose database credentials in frontend JavaScript or publicly accessible source code.

## 6. Web Server vs Application Server

These components perform different responsibilities, although modern deployments sometimes combine their functions.

### Web Server

A web server handles HTTP requests and responses.

Examples:

- Nginx.
- Apache HTTP Server.
- Microsoft IIS.

Typical responsibilities:

- Serving static files.
- Handling HTTP connections.
- TLS termination.
- Reverse proxying.
- Request routing.

### Application Server

An application server or runtime executes application logic.

Examples:

- Gunicorn for Python applications.
- Node.js runtime.
- PHP-FPM for PHP applications.
- Java application servers.

Typical responsibilities:

- Processing application requests.
- Executing business logic.
- Communicating with databases.
- Managing application operations.

## 7. Understanding APIs

API stands for Application Programming Interface.

An API allows different software components to communicate.

For example, a student mobile application may communicate with a university backend through an API.

Example:

```text
MOBILE APPLICATION
        |
        v
     API REQUEST
        |
        v
    BACKEND SERVER
        |
        v
      DATABASE
```

### Example API Endpoints

```text
GET    /api/students
GET    /api/students/1001
POST   /api/students
PUT    /api/students/1001
PATCH  /api/students/1001
DELETE /api/students/1001
```

These are illustrative endpoint designs.

### API Security

Important security controls include:

- Authentication.
- Authorization.
- Input validation.
- Rate limiting.
- Secure transport.
- Proper error handling.
- Logging and monitoring.

An API must verify permissions on the server side.

Knowing an endpoint URL or changing a record identifier must not grant unauthorized access.

## 8. Monolithic vs Microservices Architecture

### Monolithic Architecture

A monolithic application is built and deployed as one main application unit.

Example:

A university portal where authentication, student records, results, and registration are implemented in one application.

Advantages:

- Simpler deployment for smaller applications.
- Easier to understand initially.
- Fewer distributed components.

Disadvantages:

- Changes may affect the entire application.
- Scaling individual functions can be difficult.
- A weakness in one component may affect the wider application.

### Microservices Architecture

Microservices divide an application into smaller independent services.

Example:

```text
          UNIVERSITY PORTAL
                 |
       +---------+---------+
       |         |         |
       v         v         v
   AUTH SERVICE STUDENT  PAYMENT
                SERVICE   SERVICE
       |         |         |
       v         v         v
   AUTH DB    STUDENT DB PAYMENT DB
```

Advantages:

- Independent deployment.
- Independent scaling.
- Separation of responsibilities.

Disadvantages:

- More complex infrastructure.
- More network communication.
- More authentication and authorization boundaries.
- More complicated monitoring.

### Security Considerations

Microservices require secure communication between services.

A compromised service should not automatically gain unrestricted access to every other service.

## 9. Three-Tier Architecture

Three-tier architecture separates an application into three logical layers.

### Tier 1: Presentation Layer

Responsible for user interaction.

Example: HTML, CSS, and JavaScript interface.

### Tier 2: Application Layer

Responsible for processing application logic.

Example: Python or Node.js backend.

### Tier 3: Data Layer

Responsible for storing and retrieving information.

Example: PostgreSQL database.

Diagram:

```text
+-------------------------+
| PRESENTATION LAYER      |
| Browser / Frontend      |
+-------------------------+
             |
             v
+-------------------------+
| APPLICATION LAYER       |
| Backend / Business Logic|
+-------------------------+
             |
             v
+-------------------------+
| DATA LAYER              |
| Database                |
+-------------------------+
```

Separating these layers can improve maintainability and security when appropriate controls are implemented.

## 10. Web Application Security Boundaries

A security boundary separates components that have different levels of trust or access.

Examples:

- Internet and internal network.
- Browser and backend.
- Backend and database.
- Public API and administrative API.
- Application service and payment provider.

Security controls should be applied at these boundaries.

### Example

A student portal may contain:

- Public landing page.
- Student dashboard.
- Lecturer dashboard.
- Administrator dashboard.

Each area should enforce the correct authorization rules.

Hiding a dashboard link from a student is not sufficient. The backend must reject unauthorized requests.

## 11. Common Architecture-Level Vulnerabilities

### A. Insecure Direct Object Reference (IDOR)

IDOR occurs when an application allows users to access objects without properly checking whether they are authorized to access them.

Example:

A user changes a student record identifier in a request and accesses another student's information.

Prevention:

- Object-level authorization.
- Server-side permission checks.
- Secure API design.

### B. Excessive Database Privileges

An application database account has more permissions than it needs.

Potential impact:

A compromised application could have unnecessary ability to modify or delete database information.

Prevention:

- Least privilege.
- Separate database accounts.
- Restrict administrative permissions.

### C. Insecure API Design

An API may expose sensitive information or permit unauthorized operations.

Prevention:

- Authentication.
- Authorization.
- Input validation.
- Rate limiting.
- Secure error handling.

### D. Trusting Client-Side Data

A backend accepts important values directly from the browser without validating them.

Example:

A user changes a price or role value in a browser request.

Prevention:

- Validate all security-sensitive values on the server.
- Calculate important values on trusted backend systems.
- Enforce authorization independently of the interface.

### E. Poor Network Segmentation

Sensitive databases or internal services are unnecessarily exposed to public networks.

Prevention:

- Network segmentation.
- Firewall rules.
- Private service interfaces.
- Restricted administrative access.

## 12. Practical Lab 1 – Create a Web Architecture Diagram

Create your Module 12 workspace:

```bash
mkdir -p ~/practical-labs/module12/lab03
```

Create a diagram file:

```bash
nano ~/practical-labs/module12/lab03/architecture.txt
```

Enter:

```text
STUDENT WEB PORTAL

       STUDENT
          |
          v
     WEB BROWSER
          |
       HTTPS
          |
          v
      WEB SERVER
          |
          v
    BACKEND APPLICATION
          |
          v
       DATABASE
```

Save the file.

Your task:

Identify the responsibilities of each component.

## 13. Practical Lab 2 – Build a Simple Frontend

Navigate to the lab directory:

```bash
cd ~/practical-labs/module12/lab03
```

Create a file:

```bash
nano index.html
```

Enter:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>University Portal</title>
</head>
<body>

    <h1>University Student Portal</h1>

    <form>
        <label>Username:</label>
        <input type="text" name="username">

        <br><br>

        <label>Password:</label>
        <input type="password" name="password">

        <br><br>

        <button type="submit">Login</button>
    </form>

</body>
</html>
```

Save the file.

This is a frontend demonstration only. It does not authenticate users or communicate with a database.

## 14. Practical Lab 3 – Inspect the Frontend

Open the HTML file in your browser.

Alternatively:

```bash
python3 -m http.server 8001 --bind 127.0.0.1
```

Visit:

```text
http://127.0.0.1:8001
```

Observe:

- Page structure.
- Input fields.
- Password field.
- Login button.

Remember that the password field only hides characters visually. It does not securely transmit or authenticate a password by itself.

Stop the server with CTRL + C.

## 15. Practical Lab 4 – Understand API Communication

Create:

```bash
nano api-notes.md
```

Enter:

```markdown
# API Architecture Notes

## Example Endpoint
GET /api/students/1001

## Client
Student browser or application.

## API
Receives the HTTP request.

## Backend
Processes the request and checks permissions.

## Database
Retrieves the requested student record.

## Security Requirement
The backend must verify that the requesting user is authorized to access the record.
```

Save your file.

Answer:

1. What is an API?
2. What is an endpoint?
3. Why must the backend enforce authorization?
4. Why should a database not be publicly exposed?

## 16. Practical Lab 5 – Inspect Your Local Application

Start a local server:

```bash
cd ~/practical-labs/module12/lab03
```

```bash
python3 -m http.server 8001 --bind 127.0.0.1
```

Open another terminal:

```bash
curl -I http://127.0.0.1:8001
```

Inspect the response:

```bash
curl -v http://127.0.0.1:8001
```

Observe how the frontend is delivered from the local web server.

This simple server does not implement the backend, authentication, or database layers of a real university portal.

Stop the server after testing.

## 17. Practical Lab 6 – Create an Architecture Security Review

Create:

```bash
nano architecture-security-review.md
```

Use:

```markdown
# WEB APPLICATION ARCHITECTURE SECURITY REVIEW

## Application
University Student Portal Demonstration

## Frontend
Identify its responsibilities and security risks.

## Web Server
Identify its responsibilities and security risks.

## Backend
Identify its responsibilities and security risks.

## Database
Identify its responsibilities and security risks.

## API
Identify its responsibilities and security risks.

## Security Boundaries
List important trust boundaries.

## Recommendations
Suggest security controls for each component.

## Conclusion
Summarize your observations.
```

Save your work.

## 18. Knowledge Check

Answer these questions in your notebook.

1. What is web application architecture?
2. Explain the client-server model.
3. What are the responsibilities of the frontend?
4. What are the responsibilities of the backend?
5. What is the purpose of a database?
6. Differentiate a web server from an application server.
7. What is an API?
8. Explain three-tier architecture.
9. Differentiate monolithic and microservices architectures.
10. What is a security boundary?
11. Explain IDOR.
12. Why should database privileges follow the principle of least privilege?
13. Why is frontend validation insufficient for security?
14. Why must APIs enforce authorization?
15. Explain why network segmentation is important.

## 19. Lesson Completion Checklist

- [ ] Understand client-server architecture.
- [ ] Understand frontend architecture.
- [ ] Understand backend architecture.
- [ ] Understand database architecture.
- [ ] Understand web servers and application servers.
- [ ] Understand APIs and endpoints.
- [ ] Understand monolithic architecture.
- [ ] Understand microservices architecture.
- [ ] Understand three-tier architecture.
- [ ] Understand security boundaries.
- [ ] Create a web architecture diagram.
- [ ] Build a simple frontend.
- [ ] Inspect local HTTP communication.
- [ ] Complete the architecture security review.

**END OF LESSON 03**
