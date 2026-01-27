## VULNERABILITY ASSESSMENT AND PENETRATION TESTING (VAPT) REPORT

# 1. Executive Summary
This Vulnerability Assessment and Penetration Testing (VAPT) activity was conducted on an authorized TryHackMe target system to identify security weaknesses using open-source security tools. The assessment focused on network service enumeration, web server configuration analysis, and manual validation of identified findings.


The assessment identified multiple medium- and high-risk security issues, including exposed network services, missing HTTP security headers, service version disclosure, and a critical exposure of a backup archive file accessible without authentication. These weaknesses could allow an attacker to gain sensitive information and potentially compromise the target system.
# 1.1Scope and Authorization

·	Target Type: Authorized TryHackMe Vulnerable Machine

·	Target IP Address: 10.49.170.172

·	Testing Type: Black-box Assessment

·	Access Method: OpenVPN (TryHackMe VPN)

·	Authorization: Explicitly permitted for educational purposes under the TryHackMe platform

# 1.2Environment Setup
Attacker System:

·	Operating System: Kali Linux

·	Tools Environment: Default Kali Linux toolset

Target System:

·	Operating System: Linux-based server (Kernel 4.15 detected)

·	Web Server: nginx 1.24.0 (Ubuntu)

·	Network Services: SSH, HTTP

# 1.3Tools Used

| Tool        | Purpose                                                  |
| ----------- | -------------------------------------------------------- |
| Nmap        | Network discovery and service enumeration                |
| Nikto       | Web server vulnerability and misconfiguration scanning   |
| Nuclei      | Template-based vulnerability and configuration detection |
| curl        | Manual validation of exposed files and services          |
| Web Browser | Manual verification and observation                      |
| OpenVPN     | Secure access to authorized lab environment              |

# Methodology Followed
The assessment followed a standard VAPT lifecycle, aligned with industry best practices:
1.	Reconnaissance
2.	Service Enumeration
3.	Vulnerability Scanning
4.	Manual Validation
5.	Risk Classification
6.	Documentation and Reporting

All testing activities were conducted strictly within a controlled and authorized lab environment.

# 2. Reconnaissance and Enumeration

2.1 Nmap Scan Command Used:

nmap -sV -A 10.49.170.172

Observations

·	Host was reachable and active

·	Two open TCP ports were identified

·	SSH and HTTP services were exposed

·	Service version and OS information were disclosed

Identified Open Ports

| Port   | Service | Version                | Risk   |
| ------ | ------- | ---------------------- | ------ |
| 22/tcp | SSH     | OpenSSH 9.6p1 (Ubuntu) | Medium |
| 80/tcp | HTTP    | nginx 1.24.0 (Ubuntu)  | Medium |

# 2.2 Web Vulnerability Assessment (Nikto)
Nikto was used to identify common web server misconfigurations and insecure settings.

# Nikto Command Used:
nikto -h http://10.49.170.172

# Findings
·	Missing X-Frame-Options header

·	Missing X-Content-Type-Options header

·	nginx version disclosure through HTTP headers

·	Presence of predictable backup and archive file names

·	Potential information leakage through server headers

# 2.3 Automated Vulnerability Scanning (Nuclei)
Nuclei was executed to validate findings and identify known misconfigurations using updated templates.

# Command Used:
nuclei -u http://10.49.170.172

Results

·	Missing multiple HTTP security headers (CSP, HSTS, Referrer-Policy, Permissions-Policy)

·	SSH banner enumeration confirmed

·	nginx version disclosure confirmed

·	No critical CVE-based vulnerabilities detected via templates

# Manual Validation of Critical Finding

Exposed Backup Archive File

Manual validation was performed to confirm the existence of a potentially sensitive backup file.

Command Used:

curl -I http://10.49.170.172/backup.tar

# 2.4 Observation

·	The server returned an HTTP/1.1 200 OK response

·	This confirms the backup archive file is publicly accessible without authentication

Impact:

 Backup files may contain sensitive information such as credentials, configuration files, or database data, which could lead to full system compromise.

# 3. Risk Assessment (CVSS-Based)
| Factor                 | Value                       |
| ---------------------- | --------------------------- |
| Attack Vector          | Network                     |
| Attack Complexity      | Low                         |
| Privileges Required    | None                        |
| User Interaction       | None                        |
| Scope                  | Unchanged                   |
| Confidentiality Impact | High                        |
| Integrity Impact       | High                        |
| Availability Impact    | Low                         |
| **Overall Severity**   | **High (Approx. CVSS 8.6)** |

# 3.1 Summary of Findings

| ID   | Vulnerability                 | Severity     | Tool Used             | Status     |
| ---- | ----------------------------- | ------------ | --------------------- | ---------- |
| V-01 | SSH service exposed           | Medium       | Nmap                  | Identified |
| V-02 | nginx version disclosure      | Low          | Nmap / Nikto / Nuclei | Identified |
| V-03 | Missing HTTP security headers | Medium       | Nikto / Nuclei        | Identified |
| V-04 | Exposed backup archive file   | **Critical** | Nikto / curl          | Confirmed  |

# 3.2CVSS Justification 

Exposed Backup File

·	Attack Vector: Network

·	Privileges Required: None

·	User Interaction: None

·	Confidentiality Impact: High

·	Integrity Impact: High

·	Availability Impact: Low

# 3.3 Recommendations
1.	Remove backup files from web-accessible directories
2.	Store backups securely outside the web root
3.	Implement proper file permission restrictions
4.	Configure HTTP security headers (CSP, HSTS, X-Frame-Options)
5.	Harden SSH configuration (disable root login, key-based authentication)
6.	Minimize service version disclosure
7.	Regularly update and patch system services
8.	Perform periodic vulnerability assessments
   
# Conclusion
The assessment successfully identified several configuration weaknesses and one critical security issue related to exposed sensitive backup files. While automated tools highlighted misconfigurations, manual validation confirmed the most severe risk, emphasizing the importance of combining automated scanning with manual verification. Implementing the recommended mitigations will significantly strengthen the security posture of the target system.

# References
·	OWASP Top 10

·	NIST SP 800-115

·	Nmap Documentation

·	Nikto Documentation

·	Nuclei Documentation

























