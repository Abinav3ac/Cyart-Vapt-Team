## Network Protocol Attacks
# Overview

This lab demonstrates network-level attacks by performing Man-in-the-Middle (MitM) techniques against insecure network protocols. Using ARP spoofing and traffic interception, sensitive information such as credentials and authentication material was captured and analyzed.

# Objectives

Perform Man-in-the-Middle attacks

Exploit insecure network protocols

Intercept and analyze network traffic

Capture authentication data

# Attack Methodology
# 1️. Man-in-the-Middle via ARP Spoofing (Ettercap)

ARP spoofing was performed to position the attacker between the victim and the gateway.

Technique:

ARP cache poisoning

Traffic redirection through attacker's machine

Impact:

Full visibility into victim network traffic

Ability to intercept credentials and session data

# 2️.Traffic Interception & Analysis (Wireshark)

Wireshark was used to capture and analyze intercepted traffic.

# Observed Protocols

FTP

Telnet

HTTP

SMB / NetBIOS

# Findings

Plaintext credentials visible in FTP and Telnet sessions

Authentication exchanges observable over SMB

Sensitive data transmitted without encryption

# 3️.Authentication Material Capture (Responder)

Responder was deployed to listen for authentication requests and misconfigured name resolution traffic.

Captured Data:

NTLM authentication attempts

SMB / NetBIOS name resolution traffic

Credential hashes (where applicable)

Impact:

Credential harvesting

Potential for offline password cracking

Risk of lateral movement

# Attack Log (Required)
| Attack ID | Technique                        | Target IP      | Status  | Outcome                                  |
| --------- | -------------------------------- | -------------- | ------- | ---------------------------------------- |
| 015       | Man-in-the-Middle (ARP Spoofing) | 192.168.56.101 | Success | Credential / Authentication Interception |

# Impact Analysis

Loss of confidentiality of network traffic

Exposure of credentials and authentication material

Enables replay attacks and lateral movement

Demonstrates risks of unencrypted protocols

# Core Implementation

Enforce encrypted protocols (SSH, HTTPS, SFTP)

Disable legacy services (Telnet, FTP)

Implement network segmentation

Enable SMB signing

Deploy ARP spoofing detection and IDS

Educate users on insecure network usage
