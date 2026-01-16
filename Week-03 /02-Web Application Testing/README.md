## 02. Web Application Testing Lab
Activities
# Tools

Burp Suite – for manual web application testing, request interception, and session analysis

sqlmap – for automated SQL injection detection and exploitation

# Tasks

Test a vulnerable web application for OWASP Top 10 issues

Identify and validate web application vulnerabilities

Perform manual and automated testing

Document findings with severity and impact

# Brief
Test Setup

The target for this lab was DVWA (Damn Vulnerable Web Application) running on a local virtual machine.
DVWA is intentionally vulnerable and is commonly used to understand real-world web application security flaws.

Target Application: DVWA
Target URL: http://192.168.56.101/dvwa/
Security Level: Low

The objective was to test the application for common web vulnerabilities such as SQL Injection, Cross-Site Scripting (XSS), and authentication weaknesses.

Vulnerability Testing Log
| Test ID | Vulnerability | Severity | Target URL                     |
| ------- | ------------- | -------- | ------------------------------ |
| 001     | SQL Injection | Critical | `/dvwa/vulnerabilities/sqli/`  |
| 002     | Reflected XSS | Medium   | `/dvwa/vulnerabilities/xss_r/` |

# Manual Testing (Burp Suite)
Request Interception and Manipulation

Burp Suite was used to intercept HTTP requests between the browser and the DVWA application.

Key actions performed:

Captured login and form submission requests

Analyzed request parameters and cookies

Modified request values to test server-side validation

Replayed requests using Repeater

# Session Token Analysis

During XSS testing, JavaScript payloads were injected to extract session cookies:

<script>alert(document.cookie)</script>


# Result:

Application session token (PHPSESSID) was exposed

Cookie was accessible via JavaScript

Session lacked HttpOnly protection

This confirmed weak session management, allowing session hijacking.

# Automated Testing (sqlmap)
SQL Injection Testing

sqlmap was used to test input fields for SQL injection vulnerabilities.

Command Used:

sqlmap -u "http://192.168.56.101/dvwa/vulnerabilities/sqli/?id=1" \
--cookie="security=low; PHPSESSID=241ddec8f0893d80882073b765e5437" \
--batch --level=2 --risk=2

# Authentication & Input Validation Review

The following weaknesses were observed:

No server-side input validation

No prepared statements

Session tokens accessible via JavaScript

No CSRF protection

No rate limiting on requests

These issues significantly increase the risk of:

Data extraction

Account compromise

Privilege escalation

# Result:

SQL injection vulnerability confirmed

Backend database interaction possible

High risk of data extraction and manipulation

This vulnerability was classified as Critical due to direct database access.

# Identified Vulnerabilities

1. SQL Injection (Critical)

User input was not properly validated.

SQL queries are constructed dynamically

Allows database enumeration and data leakage

2. Reflected Cross-Site Scripting (Medium)

User input reflected without output encoding.

JavaScript execution inthe  victim’s browser

Enables session theft and client-side attacks

# Impact Analysis

Confidentiality: User sessions and database data exposed

Integrity: Data manipulation is possible via SQL injection

Availability: Application stability at risk due to injection attacks

# The following analysis was created and maintained during testing:

Test for SQL Injection using sqlmap

Check for XSS using manual payloads.

Verify session cookie security.y

Validate authentication and authorization mechanisms.

Review input handling and output encoding.

# Self-Curated Scripts (Optional)

Although not mandatory, simple custom payloads were tested manually to:

Bypass basic input filters.

Confirm vulnerability behavior

Understand server-side processing logic

This reinforces understanding beyond automated tools.
