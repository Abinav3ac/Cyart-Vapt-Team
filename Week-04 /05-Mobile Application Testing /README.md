## Mobile Application Testing
# Overview

This lab focuses on mobile application security assessment through static analysis of an Android application. Using MobSF, the InsecureBank v2 APK was analyzed to identify insecure storage practices, hardcoded secrets, excessive permissions, and insecure application configurations that could lead to data compromise.

# Objectives

Perform static security analysis of an Android APK

Identify insecure storage mechanisms

Detect hardcoded secrets and weak configurations

Analyze application permissions and manifest risks

Document vulnerabilities with severity and impact

# Lab Environment
| Component    | Details                      |
| ------------ | ---------------------------- |
| Analyst      | Kali Linux                   |
| Target App   | InsecureBank v2              |
| Package Name | `com.android.insecurebankv2` |
| APK File     | `InsecureBankv2.apk`         |
| Tool Used    | MobSF (Static Analysis)      |

# Static Analysis Methodology (MobSF)

The APK was uploaded to MobSF and analyzed using:

Manifest inspection

Code analysis

Resource inspection

Permission analysis

Hardcoded secret detection

The analysis focused on OWASP Mobile Top 10 risks, particularly insecure data storage.

# Vulnerability Log (Required)
| Test ID | Vulnerability    | Severity | Target App         |
| ------- | ---------------- | -------- | ------------------ |
| 016     | Insecure Storage | High     | InsecureBankv2.apk |

# Detailed Findings & Severity Table
# 🔴 High Severity Issues
| Issue                      | Description                                                  | Impact                         |
| -------------------------- | ------------------------------------------------------------ | ------------------------------ |
| Insecure Data Storage      | Sensitive data stored insecurely within application files    | Credential theft, data leakage |
| Hardcoded Secrets          | Credentials and sensitive strings embedded in code/resources | Easy reverse engineering       |
| `android:debuggable=true`  | Application can be debugged in production                    | Runtime manipulation           |
| `android:allowBackup=true` | Application data can be extracted via ADB backup             | Data exfiltration              |

# 🟠 Medium Severity Issues
| Issue                    | Description                                     | Impact                        |
| ------------------------ | ----------------------------------------------- | ----------------------------- |
| Exported Activities      | Multiple activities exported without protection | Component abuse               |
| StrandHogg Vulnerability | App vulnerable to task hijacking                | UI spoofing, credential theft |
| Weak Cryptographic Usage | Use of outdated or weak crypto APIs             | Data compromise               |

# 🟡 Low Severity Issues
| Issue                 | Description                                 | Impact                   |
| --------------------- | ------------------------------------------- | ------------------------ |
| Excessive Permissions | Unnecessary dangerous permissions requested | Increased attack surface |
| Tracker Presence      | Third-party tracking components detected    | Privacy concerns         |

# Permission Analysis Summary

The application requests several dangerous permissions, including:

WRITE_EXTERNAL_STORAGE

READ_CONTACTS

SEND_SMS

GET_ACCOUNTS

These permissions, when combined with insecure storage, significantly increase the risk of sensitive data exposure.

# Security Score Summary (MobSF)
| Metric            | Value    |
| ----------------- | -------- |
| Security Score    | 28 / 100 |
| Trackers Detected | 3        |
| Risk Level        | High     |
| Analysis Type     | Static   |

# Impact Analysis

Sensitive banking data can be extracted from local storage.

Credentials can be recovered through reverse engineering.

Attackers can abuse exported components.

High risk of data leakage in real-world deployment

# Remediation Recommendations

Avoid storing sensitive data in plaintext.

Remove hardcoded credentials from code.

Disable debuggable and allowBackup flags

Apply proper encryption for local storage.

Minimize application permissions

Secure exported components with permission checks

# Scope Declaration (IMPORTANT)

This lab focused exclusively on static analysis using MobSF.
Dynamic analysis (Frida) and IPC testing (Drozer) were considered out of scope due to tooling and environment constraints.
