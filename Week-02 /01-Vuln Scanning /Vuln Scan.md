## Vulnerability Scanning
# Overview

Vulnerability scanning is a core phase of Vulnerability Assessment and Penetration Testing (VAPT). It focuses on systematically identifying known security weaknesses in systems, services, operating systems, and installed software components. Vulnerability scanning relies primarily on automated tools that compare detected software versions and configurations against public vulnerability databases such as the National Vulnerability Database (NVD).

The objective of vulnerability scanning is not exploitation, but early detection and prioritization of security risks so that organizations can remediate them before attackers take advantage of them.

In this project, vulnerability scanning was conducted after initial reconnaissance and basic network enumeration, ensuring that the target system was reachable and active.

# Scope of Vulnerability Scanning

The vulnerability scanning phase in this project focused on:

Identifying exposed network services

Detecting installed software and libraries

Mapping discovered components to known CVEs

Classifying vulnerabilities based on severity

Prioritizing vulnerabilities for further validation

The scan was performed in an unauthenticated manner, simulating the visibility an external attacker would have.

# Tools Used for Vulnerability Scanning

The following tools were used:

Nmap – for identifying open ports and running services

Trivy – for filesystem-based vulnerability detection and CVE mapping

Each tool served a different purpose and complemented the overall scanning process.

# Network-Level Vulnerability Scanning Using Nmap

Before performing deep vulnerability scanning, network-level discovery was conducted to understand exposed services.

# Command Used
nmap -sV 10.48.139.64

# Purpose

Identify open ports

Detect running services and their versions

Create an initial attack surface map

# Results
| Port | Service | Version                |
| ---- | ------- | ---------------------- |
| 22   | SSH     | OpenSSH 9.6p1 (Ubuntu) |
| 80   | HTTP    | Nginx 1.24.0           |

SSH exposure increases the risk of brute-force or credential-based attacks if weak authentication is used.

HTTP exposure may allow attackers to test for misconfigurations, outdated components, or application-level vulnerabilities.

Version detection is critical because many vulnerabilities are version-specific.

This information was later used to correlate services with known vulnerabilities during further analysis.

# Filesystem Vulnerability Scanning Using Trivy

After identifying exposed services, a deeper vulnerability scan was performed at the system level using Trivy. Instead of scanning a container image, Trivy was used in filesystem mode to analyze installed packages and libraries.

# Command Used
trivy fs /usr --scanners vuln

# Purpose

Detect vulnerable libraries and dependencies

Map installed components to known CVEs

Identify severity and patch availability

# Trivy Scan Results Summary

The scan identified vulnerabilities across multiple components and dependency types.

Severity Distribution
| Severity | Count |
| -------- | ----- |
| Critical | 1     |
| High     | 3     |
| Medium   | 1     |
| Low      | 0     |
| Unknown  | 0     |

This indicates the presence of high-impact vulnerabilities that require immediate attention.

# Detailed Vulnerability Findings
Example 1: Critical Vulnerability
| Field       | Details                                           |
| ----------- | ------------------------------------------------- |
| Component   | Microsoft.AspNetCore.App Runtime                  |
| CVE ID      | CVE-2024-21386                                    |
| Severity    | Critical                                          |
| Description | Denial of Service vulnerability in SignalR server |
| Status      | Fixed version available                           |

# Impact:
A successful exploit could allow an attacker to crash services, affecting availability. In production environments, this could lead to service outages and denial of legitimate user access.

Example 2: High Severity Vulnerabilities
| Field       | Details                                           |
| ----------- | ------------------------------------------------- |
| Component   | Microsoft.AspNetCore.App Runtime                  |
| CVE ID      | CVE-2024-21386                                    |
| Severity    | Critical                                          |
| Description | Denial of Service vulnerability in SignalR server |
| Status      | Fixed version available                           |

# Impact:
These vulnerabilities can be exploited by sending specially crafted XML data, causing excessive CPU or memory usage and resulting in denial of service conditions.

# Vulnerability Prioritization

Vulnerabilities were prioritized using severity levels provided by Trivy, which are aligned with CVSS scoring principles.

| Priority | Criteria                           |
| -------- | ---------------------------------- |
| Critical | Immediate remediation required     |
| High     | High risk, fix as soon as possible |
| Medium   | Scheduled remediation              |
| Low      | Monitor and review                 |

# In this project:

Critical and High vulnerabilities were marked for further validation

Medium vulnerabilities were documented for remediation planning

# False Positive Considerations

# Automated vulnerability scanners can sometimes report false positives due to:

Incorrect version detection

Patched software reporting outdated versions

Vulnerabilities not exploitable in the current configuration

# To minimize false positives:

Version numbers were manually reviewed

Only confirmed vulnerabilities with reliable CVE references were included

Exploitation was not attempted during this phase

# Limitations of Vulnerability Scanning

While vulnerability scanning is effective, it has inherent limitations:

Can only detect known vulnerabilities

Cannot confirm exploitability

May miss logic or configuration flaws

Requires follow-up penetration testing for validation

This reinforces the need to combine vulnerability scanning with manual testing and exploitation phases.

# Outcome of Vulnerability Scanning Phase

The vulnerability scanning phase successfully:

Identified exposed services

Detected critical and high-severity vulnerabilities

Mapped vulnerabilities to CVEs

Provided a prioritized list for further testing

These findings directly informed the next phases of the project, including exploitation and post-exploitation activities.

