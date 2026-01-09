## Reconnaissance
# Overview

Reconnaissance is the initial phase of penetration testing where information about a target is collected to understand its external exposure and attack surface. The primary goal of reconnaissance is to gather as much intelligence as possible without actively exploiting the target. This phase helps attackers and defenders alike to identify publicly exposed assets, services, and relationships that may later be abused.

Reconnaissance can be classified into two main types:

Passive Reconnaissance – Information gathering without directly interacting with the target

Active Reconnaissance – Limited interaction with the target to discover additional details

In this project, reconnaissance was performed primarily using passive OSINT techniques to avoid alerting detection systems.

# Scope of Reconnaissance

The reconnaissance phase focused on:

Identifying publicly available domain information

Discovering exposed services on the internet

Mapping domain-related infrastructure

Understanding asset relationships

No exploitation or intrusive testing was performed during this phase.

# Tools Used for Reconnaissance

The following tools were used:

WHOIS – Domain registration and ownership details

Shodan – Internet-wide asset discovery and service exposure

Maltego – Visual OSINT and relationship mapping

Each tool provided a different layer of intelligence.

# WHOIS Enumeration
Purpose

WHOIS enumeration provides administrative and registration details about a domain. This information can reveal ownership, DNS infrastructure, and registrar details that may assist in targeted attacks or social engineering.

# Command Used
whois google.com

# Information Collected

Domain creation and expiration dates

Registrar information

Name server details

Domain status flags (transfer, update protection)

# Security Relevance

Registrar and DNS provider information can help attackers identify third-party dependencies.

Domain age indicates long-standing infrastructure, which may expose legacy configurations.

Domain protection statuses show whether the domain is secured against unauthorized changes.

# Shodan Reconnaissance
Purpose

Shodan is a search engine for internet-connected devices and services. Unlike traditional scanners, Shodan passively collects data by indexing banners and service metadata exposed on the internet.

# Activities Performed

Searched for public IPs and domains

Identified exposed services such as SSH and HTTP

Observed SSL/TLS and server banner information

Reviewed geolocation and organization metadata

# Observations

Publicly accessible SSH services were identified

Web servers exposing HTTP headers and version details

Metadata revealing hosting provider and location

# Security Impact

Shodan demonstrates how attackers can locate vulnerable systems without sending any packets to the target. Any service indexed by Shodan is already publicly exposed and potentially vulnerable to automated attacks.

# Maltego Asset Mapping
Purpose

Maltego is an OSINT tool used to visually map relationships between domains, IP addresses, DNS records, and other entities. It helps identify hidden connections that are not easily visible through manual analysis.

# Steps Performed

Created a new Maltego graph

Added a Domain entity (e.g., testphp.vulnweb.com)

Executed DNS-related transforms:

DNS Name – Interesting DNS Names

DNS from Domain

MX and NS record discovery

# Results

No additional DNS or subdomain entities were discovered

Limited OSINT exposure observed

# Analysis

The absence of additional results suggests:

Minimal DNS exposure

Restricted third-party integrations

Limited public asset footprint

This is an indicator of improved security posture from an OSINT perspective.

Asset Mapping Summary
| Tool    | Information Discovered                          |
| ------- | ----------------------------------------------- |
| WHOIS   | Domain ownership, registrar, DNS infrastructure |
| Shodan  | Exposed services, banners, metadata             |
| Maltego | DNS relationships and asset visibility          |

# Limitations of Reconnaissance

OSINT tools depend on publicly available data

Private infrastructure remains undiscovered

Results vary depending on third-party data sources

Passive recon cannot confirm vulnerabilities

Despite these limitations, reconnaissance provides critical intelligence that shapes later testing phases.

# Outcome of Reconnaissance Phase

The reconnaissance phase successfully:

Identified publicly available domain and infrastructure details

Discovered exposed internet-facing services

Mapped visible assets using OSINT tools

Reduced blind spots for subsequent scanning and exploitation phases

The information gathered during reconnaissance directly guided vulnerability scanning and exploitation planning.
