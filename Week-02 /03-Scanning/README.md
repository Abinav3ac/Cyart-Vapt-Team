## Scanning (Enumeration)
# Overview

Scanning, also known as enumeration, is the phase that follows reconnaissance in a penetration testing lifecycle. While reconnaissance focuses on gathering high-level information, scanning involves active interaction with the target to identify live hosts, open ports, running services, and detailed service configurations.

The purpose of scanning is to transform reconnaissance data into actionable technical intelligence. This phase provides attackers and defenders with a clear understanding of the target’s exposed services and potential entry points.

In this project, scanning was performed after confirming target availability and network connectivity.

# Objectives of the Scanning Phase

The primary objectives of scanning were:

Identify open and accessible ports

Enumerate running services and versions

Detect misconfigured or outdated services

Validate information collected during reconnaissance

Prepare data for exploitation

# Tools Used for Scanning

Nmap – Network and service enumeration

Metasploit Auxiliary Modules – Service verification and banner checks

# Network and Service Enumeration Using Nmap
Purpose

Nmap was used to actively probe the target system and identify services running on exposed ports. Service and version detection helps map known vulnerabilities to specific components.

# Command Used
nmap -sV 10.48.139.64

# Results
| Port | State | Service | Version       |
| ---- | ----- | ------- | ------------- |
| 22   | Open  | SSH     | OpenSSH 9.6p1 |
| 80   | Open  | HTTP    | nginx 1.24.0  |

# Analysis

Open SSH services are commonly targeted for credential-based attacks.

HTTP services expose a broader attack surface, including misconfigurations and vulnerable modules.

Version information allows mapping to known CVEs and exploit databases.

# Enumeration of Network Services
# SSH Enumeration

SSH was identified as running on port 22

Authentication was not tested during scanning to avoid brute-force activity

The service version was documented for vulnerability correlation

# HTTP Enumeration

HTTP service on port 80 was identified

Server banner information was collected

Web application testing was deferred to later phases

# Validation Using Metasploit Auxiliary Modules
 Purpose

Metasploit auxiliary scanners were used to verify service availability and confirm scanning results from Nmap.

# Activities Performed

Confirmed service responsiveness

Verified network reachability

Cross-checked port accessibility

This step helped reduce false positives and ensured scan accuracy.

Scanning Result Summary
| Category            | Findings            |
| ------------------- | ------------------- |
| Live Hosts          | 1                   |
| Open Ports          | 2                   |
| Identified Services | SSH, HTTP           |
| Critical Services   | SSH (22), HTTP (80) |

# False Positive Handling
During scanning, care was taken to:

Cross-verify results using multiple tools

Avoid assumptions based solely on banners

Confirm that detected ports were actually reachable

This ensured reliability and accuracy of scanning results.

# Limitations of the Scanning Phase

Scanning reveals what is exposed, not how exploitable it is

Cannot identify internal vulnerabilities without authentication

Application-level flaws require deeper testing

Some services may be filtered or hidden by firewalls

# Outcome of the Scanning Phase

The scanning phase successfully:

Identified exposed network services

Enumerated service versions

Validated reconnaissance data

Provided a clear attack surface for exploitation

The results of this phase directly informed the exploitation strategy used in later stages of the assessment.
