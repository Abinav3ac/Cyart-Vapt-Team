## Capstone Project – Full VAPT Cycle
Activities
# Tools

Kali Linux – attacker platform

Nmap – reconnaissance and service enumeration

Burp Suite – web application testing and session analysis

sqlmap – automated SQL injection validation

Metasploit Framework – exploitation and post-exploitation

Wireshark – network traffic capture and forensic evidence

SHA256 utilities – evidence integrity verification

# Tasks

Perform a complete VAPT lifecycle on a vulnerable target

Identify, exploit, and chain vulnerabilities

Demonstrate real-world attack progression

Collect and preserve technical evidence

Produce structured, actionable reporting

# Brief
Capstone Objective

The objective of this capstone project is to simulate a real-world penetration test from start to finish against a deliberately vulnerable system.
The engagement follows a traditional PTES-style workflow, covering:

Reconnaissance → Enumeration → Exploitation → Post-Exploitation → Evidence → Reporting


All activities were performed in a controlled lab environment for educational purposes.

# Phase 1 – Reconnaissance & Enumeration
Tool Used

Nmap

Command Executed:
nmap -sC -sV -oA meta_scan 192.168.56.101

# Key Discoveries

FTP (vsftpd 2.3.4)

HTTP (Apache + DVWA)

Telnet, SSH, SMB

UnrealIRCd

MySQL, PostgreSQL

Multiple legacy and misconfigured services

This phase revealed a large attack surface with several high-risk entry points.

# Phase 2 – Web Application Testing
Target

DVWA (Damn Vulnerable Web Application)

# Vulnerabilities Identified

SQL Injection (Critical)

Reflected XSS (Medium)

Weak session management

# Impact

Database interaction possible via SQLi

Session cookies exposed via XSS

Authentication trust abused

This phase enabled initial access and privilege abuse.

# Phase 3 – Exploitation
# Exploit 1 – FTP Backdoor (vsftpd 2.3.4)

Tool: Metasploit

use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.56.101
run


Result:

Remote shell spawned

Root access obtained immediately

# Exploit 2 – UnrealIRCd Backdoor

Tool: Metasploit

use exploit/unix/irc/unreal_ircd_3281_backdoor
run


Result:

Second independent root shell

Confirms systemic insecurity

# Phase 4 – Post-Exploitation
Actions Performed

Privilege validation (whoami, id)

User enumeration (/etc/passwd)

Credential exposure (/etc/shadow)

Network/service enumeration (netstat, ip a)

Outcome

Full administrative control

Sensitive data exposure

No further escalation required

# Phase 5 – Evidence Collection & Forensics
Tools Used

Wireshark

script

sha256sum

Evidence Collected

FTP traffic (credentials & banners in clear text)

HTTP traffic (sessions, requests, responses)

Command execution logs

Cryptographic hashes for integrity

Sample Evidence Log
| Timestamp        | Target IP      | Vulnerability | PTES Phase        |
| ---------------- | -------------- | ------------- | ----------------- |
| 2026-01-16 11:20 | 192.168.56.101 | SQL Injection | Exploitation      |
| 2026-01-16 11:35 | 192.168.56.101 | XSS           | Exploitation      |
| 2026-01-16 12:10 | 192.168.56.101 | FTP RCE       | Post-Exploitation |

# Phase 6 – Risk & Impact Assessment
CIA Triad Impact
| Category        | Status  |
| --------------- | ------- |
| Confidentiality | Broken  |
| Integrity       | Broken  |
| Availability    | At Risk |

Overall Risk

Critical

Attack complexity was low, while impact was severe, making this environment highly exploitable.

# Phase 7 – Remediation & Verification
Recommendations

Patch or remove vulnerable services

Enforce secure coding practices

Implement HTTPS and encrypted protocols

Harden authentication and session handling

Apply least-privilege principles

Conduct regular vulnerability scans

Verification

A rescan should be conducted after remediation to confirm:

Services are patched or disabled

Vulnerabilities are no longer exploitable

# Executive Summary

The penetration test revealed multiple critical security weaknesses that allow an attacker to fully compromise the system. Initial web application flaws enabled unauthorized access, which was escalated into complete system control through vulnerable services. Sensitive data, user credentials, and network communications were exposed, posing serious business and operational risks. Immediate remediation is required, including patching outdated services, securing authentication mechanisms, and enforcing encryption. Without corrective action, the system remains highly vulnerable to real-world attacks.

# Conclusion

This capstone project demonstrates how individual vulnerabilities, when combined, can result in total system compromise.
The assessment highlights the importance of defense-in-depth, regular patching, and secure development practices.

The full VAPT lifecycle was successfully executed and documented with verifiable evidence.
