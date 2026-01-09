## Capstone Project: Full VAPT Cycle
# Overview

The capstone project represents the culmination of the Vulnerability Assessment and Penetration Testing (VAPT) process by integrating all previously executed phases into a single, structured security assessment. This phase demonstrates the ability to perform end-to-end security testing, from initial information gathering to impact analysis and reporting, following industry-recognized methodologies.

The objective of the capstone was to simulate a real-world penetration testing engagement within a controlled laboratory environment and to validate the effectiveness of security controls through practical exploitation and analysis.

# Scope and Target Environment

The assessment was conducted in a controlled virtual lab environment consisting of:

Attacker Machine: Kali Linux

Target Machine: Vulnerable Linux system

Network Configuration: Isolated internal network

The scope included network services, exposed applications, and system-level components visible to an unauthenticated attacker.

# Methodology Followed

The capstone project followed a structured methodology aligned with:

NIST SP 800-115 – Technical Guide to Information Security Testing

PTES (Penetration Testing Execution Standard)

OWASP Testing Principles

The testing lifecycle consisted of the following phases:

Reconnaissance

Vulnerability Scanning

Scanning and Enumeration

Exploitation

Post-Exploitation

Reporting and Remediation Analysis

# Phase-Wise Execution Summary
# Reconnaissance

Passive reconnaissance techniques were used to collect publicly available information using OSINT tools such as WHOIS, Shodan, and Maltego. This phase helped identify exposed services, infrastructure metadata, and domain-related information without directly interacting with the target system.

# Vulnerability Scanning

Automated vulnerability scanning was performed using tools such as Nmap and Trivy to identify exposed services, vulnerable packages, and known CVEs. Vulnerabilities were categorized based on severity, and critical findings were prioritized for further validation.

# Scanning and Enumeration

Active scanning was conducted to enumerate open ports and running services. Service version detection allowed mapping of exposed components to known vulnerabilities. Enumeration provided a clear attack surface for exploitation planning.

# Exploitation

Based on scanning results, a critical Samba vulnerability was identified and exploited using the Metasploit Framework. The exploitation successfully resulted in a reverse shell with root-level privileges, confirming a complete system compromise.

# Post-Exploitation

Post-exploitation activities verified the level of access obtained and assessed the impact on system security. Root access was confirmed, demonstrating full control over the target system and highlighting the severity of the identified vulnerability.

# Key Findings Summary
| Phase                  | Outcome                                      |
| ---------------------- | -------------------------------------------- |
| Reconnaissance         | Public asset and service discovery           |
| Vulnerability Scanning | Critical and high-risk CVEs identified       |
| Enumeration            | Open ports and vulnerable services confirmed |
| Exploitation           | Successful root-level access achieved        |
| Post-Exploitation      | Complete system compromise validated         |

# Security Impact Assessment

The capstone project revealed that:

Outdated and misconfigured services can lead to full system compromise

Automated vulnerability scanning alone is insufficient without exploitation

Critical vulnerabilities pose severe risks to confidentiality, integrity, and availability

Lack of timely patching significantly increases attack success probability

# Remediation Recommendations

Based on the assessment, the following remediation actions are recommended:

Patch and update all vulnerable services immediately

Disable unnecessary network services

Restrict service access using firewall rules

Conduct regular vulnerability assessments

Perform periodic penetration testing

Monitor exposed assets using OSINT tools

## Risk Rating and Business Impact Analysis

The vulnerabilities identified during the assessment were evaluated not only based on technical severity but also on their potential business impact. A successful exploitation of critical vulnerabilities could directly affect business operations, data security, and system availability.

### Business Impact
- **Confidentiality:** Sensitive system and user data can be accessed or exfiltrated.
- **Integrity:** Attackers can modify system files, configurations, and application logic.
- **Availability:** Critical services can be disrupted, leading to downtime and service outages.

### Overall Risk Rating
Considering the successful root-level exploitation, the overall risk rating for the target system is classified as **Critical**. Immediate remediation is required to prevent potential real-world attacks.

## CVSS-Based Risk Justification

The exploited vulnerability was classified as Critical based on CVSS principles:

- **Attack Vector:** Network
- **Attack Complexity:** Low
- **Privileges Required:** None
- **User Interaction:** None
- **Impact:** Complete compromise of confidentiality, integrity, and availability

These factors justify the high CVSS score and demonstrate why exploitation resulted in full system compromise.

## Detection and Logging Gaps

During the assessment, no visible alerting or blocking mechanisms were observed while scanning and exploitation were performed. This indicates potential gaps in monitoring and intrusion detection.

### Observations
- No evidence of IDS/IPS blocking exploit attempts
- No visible rate limiting on exposed services
- Exploitation did not trigger service termination

### Recommendation
Implement centralized logging, intrusion detection systems, and real-time alerting to improve visibility and early attack detection.

## Mitigation Mapping

| Attack Phase | Weakness | Recommended Mitigation |
|-------------|---------|------------------------|
| Reconnaissance | Public service exposure | Reduce attack surface |
| Scanning | Outdated services | Regular patch management |
| Exploitation | Samba vulnerability | Disable or upgrade service |
| Post-Exploitation | Excessive privileges | Principle of least privilege |

## Assessment Limitations

- Testing was limited to a lab environment
- No authenticated scans were performed
- Web application logic testing was out of scope
- Lateral movement was not tested

Despite these limitations, the assessment successfully demonstrated critical security weaknesses.

## Ethical and Legal Disclaimer

All testing activities were conducted in a controlled lab environment with explicit authorization. No real-world systems were targeted, and no data was harmed. This project was performed strictly for educational and learning purposes.

# Skills Demonstrated

This capstone project strengthened practical skills in:

Vulnerability assessment and prioritization

OSINT-based reconnaissance

Network and service enumeration

Exploit selection and execution

Post-exploitation impact analysis

Professional security reporting

## Future Improvements

- Perform authenticated vulnerability scans
- Include web application exploitation
- Test lateral movement scenarios
- Integrate SIEM log analysis
- Automate reporting and evidence collection

# Conclusion

The capstone project successfully demonstrated a complete VAPT lifecycle in a real-world-simulated environment. By combining automated scanning with manual exploitation and validation, the assessment highlighted critical security weaknesses and their potential impact. The project emphasizes the importance of proactive security testing, timely patch management, and continuous monitoring to reduce organizational risk.
