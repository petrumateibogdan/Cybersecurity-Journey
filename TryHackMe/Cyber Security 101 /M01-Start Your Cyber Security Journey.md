# Module 1: Start Your Cyber Security Journey

## 1. Search Skills


## Overview
Knowing where and how to search is just as important as knowing what to search for—whether hunting down an exploit, trying to understand a tool, or tracking a threat actor. Effective use of internet resources is a critical skill in cybersecurity.
---

## Shodan
Shodan continuously scans the internet for networking equipment, industrial control systems, traffic cameras, and public network connections.
* **Apache Web Server:** Apache is the most widely used, free, open-source web server software developed and maintained by the Apache Software Foundation. Searching a version like `apache 2.4.1` returns servers advertising that version in their HTTP headers, useful during penetration tests and vulnerability assessments.
* **Query Filters:**
  * `country:` - Restricts results to a specific country code (e.g., `country:IE`).
  * `port:` - Filters by a specific port number or range (e.g., `port:22`).
  * `org:` - Scopes results to a named organization or ASN Identifier (e.g., `AS7224` for Amazon Web Services).
  * `hostname:` - Matches against a specific hostname or domain (e.g., `hostname:fakebank.thm`).
    
---

## VirusTotal
* Collates results from over 70 antivirus engines and website scanners into a single interface.
* Users can submit files, URLs, domains, or file hashes to check if engines flag them as malicious.
* A popular resource in the blue teaming community for obtaining a general consensus on suspicious files/links and gathering threat intelligence.

---

## CVE (Common Vulnerabilities and Exposures) & CVSS
* **CVE Program:** Acts as the industry's universal dictionary of known vulnerabilities, using the format `CVE-YEAR-NUMBER` (e.g., `CVE-2025-55182`). Major vulnerabilities often receive monikers like Heartbleed, React2Shell, and Log4Shell.
* **CVSS (Common Vulnerability Scoring System):** Scores vulnerabilities based on:
  * *Impact:* What damage can this vulnerability lead to?
  * *Complexity:* Is the vulnerability easy to exploit or not?
  * *Availability:* How likely is it that someone can exploit this?
* **ExploitDB & PoCs:** Websites like ExploitDB compile vulnerability info alongside Proof of Concepts (PoCs)—scripts capable of demonstrating the vulnerability.
  
---

## MAN (MANual)
* **Official Documentation:** Always the most reliable and up-to-date first stop when troubleshooting or learning tool usage.
* **Linux Man Pages:** Built-in terminal documentation accessible via `man <command>` for Linux commands and cybersecurity tooling.
* **Netcat (`nc`):** A utility used for arbitrary TCP and UDP connections, sending UDP packets, listening on ports, and port scanning. Common uses include simple TCP proxies, script-based HTTP clients/servers, network daemon testing, and SSH ProxyCommands.
  
---

## GitHub for Threat Intelligence
* Used to stay updated on latest threats, publishing PoC code, exploitation tools, and technical reports.
* Searching CVE identifiers directly (e.g., `CVE-2026-1337`) reveals repositories containing PoC code, scanner scripts, or detailed analyses.
* **Caution:** Not all PoCs are reliable; some are incomplete, flawed, or malicious. Always verify code before execution.
