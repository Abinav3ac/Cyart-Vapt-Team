## API Security Testing Lab
# Overview

This lab focuses on API security assessment using manual and automated techniques. The objective is to identify authorization flaws, improper access control, and injection issues in API-like endpoints exposed by a vulnerable web application (DVWA).

# Objectives

Enumerate API-style endpoints

Identify Broken Object Level Authorization (BOLA)

Manipulate API parameters and tokens using Burp Suite

Validate injection issues using sqlmap

Document findings with severity and impact

# Lab Environment
| Component | Details                                |
| --------- | -------------------------------------- |
| Attacker  | Kali Linux                             |
| Target    | Metasploitable 2 (DVWA)                |
| Target IP | `192.168.56.101`                       |
| Tools     | Burp Suite, Postman (optional), sqlmap |

# Test Setup

Although DVWA is a web application, its endpoints behave similarly to APIs by:

Accepting parameters via GET/POST

Returning structured responses

Lacking proper authorization checks

Target Endpoints: 

/dvwa/vulnerabilities/sqli/

/dvwa/vulnerabilities/exec/

/dvwa/vulnerabilities/brute/

# API Testing Methodology
# 1️.Endpoint Enumeration

Endpoints were identified through:

Manual browsing

Burp Suite Proxy interception

Parameter discovery (id, cmd, session cookies)

Tools Used:

Burp Proxy

Browser Developer Tools

# 2️.Broken Object Level Authorization (BOLA)
Description

BOLA occurs when an application fails to verify whether a user is authorized to access a specific object or resource.

Exploitation

By modifying the id parameter in intercepted requests, unauthorized access to other users’ data was achieved.

Payload: 

id=1 OR 1=1

Result:

Retrieved multiple user records

No authorization validation enforced.

# 3️.Burp Suite – Manual API Manipulation

Requests were intercepted and modified using Burp Repeater.

Manipulations Performed

Changed object IDs

Replayed requests with stolen session cookies

Tested unauthorized access scenarios

Impact: 

Full data disclosure

Authentication bypass at the API logic layer

# 4️. Automated Validation with sqlmap

sqlmap was used to confirm injection behavior and backend response.

# Command Used:

sqlmap -u "http://192.168.56.101/dvwa/vulnerabilities/sqli/?id=1" \

--cookie="PHPSESSID=XXXXX; security=low" --batch

# Outcome

Confirmed SQL Injection

Identified backend database behavior

# Vulnerability Log (Required)
| Test ID | Vulnerability                            | Severity | Target Endpoint               |
| ------- | ---------------------------------------- | -------- | ----------------------------- |
| 008     | Broken Object Level Authorization (BOLA) | Critical | `/dvwa/vulnerabilities/sqli/` |

# Impact Analysis

Unauthorized access to sensitive user data

Lack of object-level authorization

Potential for mass data extraction via automation

High risk in real-world API environments

# Remediation Recommendations

Enforce strict object-level authorization checks.

Validate user ownership before returning resources.

Avoid relying on client-supplied object identifiers.

Implement proper access control middleware.

Log and monitor abnormal API access patterns.
