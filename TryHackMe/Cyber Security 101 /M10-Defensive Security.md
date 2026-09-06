# Module 10: Defensive Security

# 1. SOC Fundamentals

I recently completed a module on the fundamentals of a Security Operations Center (SOC). Here are my notes on how a SOC operates, its core pillars, and the defensive technologies utilized to protect organizational networks.

###  The Core Focus: Detection and Response
*   **Detection:** Identifying system vulnerabilities, unauthorized access, security policy violations, and network intrusions.
*   **Response:** Supporting incident response teams by minimizing impact and performing root-cause analysis on detected threats.

###  The First Pillar: People
*   **Level 1 Analysts:** Serve as first responders who perform basic alert triage to determine if a detection is genuinely harmful.
*   **Level 2 Analysts:** Conduct deeper investigations by correlating data from multiple sources to perform a proper analysis.
*   **Level 3 Analysts:** Proactively hunt for threat indicators and handle critical incidents requiring containment, eradication, and recovery.
*   **Engineers & Management:** Security Engineers configure tools, Detection Engineers build alert logic, and the SOC Manager oversees processes and communicates with the CISO.

###  The Second Pillar: Process
*   **Alert Triage:** The initial analysis of an alert to determine severity and priority by answering the "5 Ws" (What, When, Where, Who, Why).
*   **Reporting:** Escalating validated threats to higher-level analysts via ticketing systems, ensuring reports include thorough analysis and evidence.
*   **Forensics & Incident Response:** Analyzing system or network artifacts during critical incidents to determine the exact root cause.

###  The Third Pillar: Technology
*   **SIEM (Security Information and Event Management):** Centralizes logs from various network devices, correlating data against detection rules to identify suspicious activity (Detection only).
*   **EDR (Endpoint Detection and Response):** Provides real-time and historical visibility at the device level, allowing analysts to investigate and execute automated responses.
*   **Firewall:** Acts as a network barrier to filter unauthorized traffic and block suspicious activity before it reaches the internal network.


# 2. Digital Forensics Fundamentals



I recently completed a module on Digital Forensics, learning how law enforcement investigates cybercrimes—like analyzing a bank robber's laptop or mobile phone for digital maps, documents, and chat logs. Here is my comprehensive guide covering the investigative process, secure evidence acquisition, and metadata analysis.

##  The NIST Forensics Process & Domains
I learned that the National Institute of Standards and Technology (NIST) defines a strict four-phase framework for every investigation:

*   **Collection:** Securely extracting data from devices (computers, USBs, cameras) without tampering with the original evidence.
*   **Examination:** Filtering massive datasets to extract only the data of interest for a specific timeframe or user account.
*   **Analysis:** Correlating the filtered evidence to build a chronological timeline of the incident and draw conclusions.
*   **Reporting:** Presenting detailed findings, methodologies, and executive summaries to law enforcement or executive management.
*   **Domains:** Digital investigations span across Computer, Mobile, Network (traffic logs), Database, Cloud, and Email forensics.

##  Secure Evidence Acquisition
Acquiring evidence is a highly critical job that must follow strict legal and technical procedures to be valid.

*   **Proper Authorization:** Securing legal approval before data collection is essential so the private evidence remains admissible in court.
*   **Chain of Custody:** A formal document tracking the evidence description, collectors, storage location, and access history to prove integrity and reliability.
*   **Write Blockers:** Essential hardware or software tools used during extraction to prevent the forensic workstation from accidentally altering original file timestamps.

##  Windows Forensics
When investigating personal computers and laptops, I learned to capture two distinct types of forensic images.

*   **Memory Image (Volatile):** Data inside RAM (running processes, network connections) that is completely lost upon system shutdown. This must be captured first using **DumpIt** or analyzed with **Volatility**.
*   **Disk Image (Non-Volatile):** Permanent storage data (documents, media, browsing history) acquired and analyzed using tools like **FTK Imager** or **Autopsy**.

##  Metadata Command Cheat Sheet
Digital activities always leave traces. I use specific command-line tools to extract hidden metadata from files and images.

| Tool | Function | Example Command |
| :--- | :--- | :--- |
| **pdfinfo** | Extracts document creation dates, authors, and the exact software used to generate the file. | `pdfinfo DOCUMENT.pdf` |
| **exiftool** | Extracts embedded Exchangeable Image File Format (EXIF) data, including camera settings and GPS coordinates that can be mapped online. | `exiftool IMAGE.jpg` |


# 3. Incident Response Fundamentals

# Incident Response: Planning & Processes

I recently completed a module focusing on Incident Response, an essential part of defensive security. I learned how organizations prepare for, identify, and mitigate cybersecurity incidents. Here are my notes on incident classification, response frameworks, and playbooks.

###  Incidents and Severity
*   **True Positives vs. False Positives:** Security solutions generate alerts based on events. A *true positive* indicates actual harmful activity (like a phishing attack), while a *false positive* is a benign action mistakenly flagged as harmful (like a scheduled backup).
*   **Severity Levels:** True positives are classified as *incidents* and assigned a severity level (Critical, High, Medium, Low) based on their potential impact to help prioritize the response.

###  Common Types of Incidents
*   **Malware Infections:** The most frequent incidents, caused by malicious programs designed to damage networks or systems.
*   **Security Breaches:** Occur when unauthorized individuals gain access to confidential data.
*   **Data Leaks:** The exposure of confidential information to unauthorized entities. Unlike breaches, these can be caused unintentionally by human error or misconfiguration.
*   **Insider Attacks:** Threats originating from within the organization, such as a disgruntled employee intentionally causing damage.
*   **Denial of Service (DoS):** Attacks designed to make a system or network unavailable to legitimate users by flooding it with false requests.

###  Incident Response Frameworks
To structure their response, organizations rely on established frameworks like SANS and NIST.

**SANS Framework (PICERL):**
1.  **Preparation:** Building necessary resources (teams, plans, tools) and training employees before an incident occurs.
2.  **Identification:** Monitoring for abnormal behavior to detect incidents.
3.  **Containment:** Minimizing the attack's impact (e.g., isolating a compromised host).
4.  **Eradication:** Removing the threat from the environment entirely.
5.  **Recovery:** Restoring affected systems from backups and ensuring they are safe to use.
6.  **Lessons Learned:** Documenting gaps and improving future responses through post-incident reviews.

**NIST Framework (4 Phases):**
The NIST framework simplifies this into four phases:
1.  Preparation
2.  Detection and Analysis (equivalent to SANS Identification)
3.  Containment, Eradication, and Recovery (combined)
4.  Post-Incident Activity (equivalent to SANS Lessons Learned)

###  The Incident Response Plan
This is the formal document, approved by senior management, that details the organization's approach to handling incidents. It includes roles, methodologies, communication strategies (including with law enforcement), and escalation paths.

###  Playbooks and Runbooks
*   **Playbooks:** Step-by-step guidelines for handling specific types of incidents comprehensively. For example, a phishing playbook might include steps like notifying stakeholders, analyzing the email and attachments, isolating infected systems, and blocking the sender.
*   **Runbooks:** Detailed execution steps for specific tasks within the incident response process, which can vary based on the available tools and resources.

###  Tools for Detection and Response
*   **SIEM:** Centralizes and correlates logs to identify incidents.
*   **AV (Antivirus):** Scans for and detects known malicious programs.
*   **EDR (Endpoint Detection and Response):** Deployed on endpoints to protect against advanced threats and can actively contain and eradicate them.


# 4. Logs Fundamentals

# Intro to Log Analysis: Event Viewer & Linux CLI

I recently completed a module focusing on the importance of log analysis in defensive security. I learned that logs are digital footprints left behind by both normal and malicious activities, making them crucial for incident investigation, security monitoring, and troubleshooting. 

Here are my repository notes covering the different types of logs, how to analyze Windows Event Logs, and how to parse Web Server Access Logs using Linux command-line tools.

---

##  Common Log Categories
To make investigations manageable, systems segregate logs into multiple categories based on the type of information they provide. 

| Log Type | Primary Usage | Example Events |
| :--- | :--- | :--- |
| **System** | Troubleshooting OS running issues. | System startup/shutdown, driver loading events. |
| **Security** | Detecting and investigating incidents. | Authentication, authorization, and policy changes. |
| **Application** | Events related to specific applications. | Application errors, updates, and user interactions. |
| **Audit** | Compliance requirements and security monitoring. | Data access and policy enforcement events. |
| **Network** | Monitoring incoming and outgoing traffic. | Network connection and firewall logs. |
| **Access** | Detailing access to different resources. | Webserver, database, and API access logs. |

---

##  Windows Event Logs & Event Viewer
I learned that Windows logs many of its activities into specific files, primarily categorized as Application, System, and Security. Security logs are the most important for cyber investigations, logging all authentication and account changes.

Windows provides a built-in GUI utility called **Event Viewer** to search and filter these logs. Every logged activity is assigned a unique identifier known as an Event ID.

### Crucial Windows Event IDs for Investigations:
*   **4624:** A user account successfully logged in.
*   **4625:** A user account failed to log in.
*   **4634:** A user account successfully logged off.
*   **4720:** A user account was created.
*   **4722:** A user account was enabled.
*   **4724:** An attempt was made to reset an account’s password.
*   **4725:** A user account was disabled.
*   **4726:** A user account was deleted.

> **Tip:** In Event Viewer, I can use the "Filter Current Log" feature to input specific Event IDs (like `4624`) to quickly hunt for successful logins without manually scrolling through thousands of entries.

---

##  Web Server Access Logs (Linux CLI)
When users interact with a website, requests are stored in a log file on the web server (e.g., Apache logs are often found at `/var/log/apache2/access.log`). I learned that these logs contain valuable data, including the requester's IP address, the timestamp, the HTTP method (e.g., GET), the requested URL, the status code, and the User-Agent string.

Because web logs are typically plain text, I can analyze them manually using standard Linux command-line utilities.

### Command-Line Cheat Sheet for Log Analysis:

*   **`cat`:** Displays the contents of a text file. It is also useful for combining multiple rotated log files into one.
    ```bash
    # Combine two access logs into a single file
    cat access1.log access2.log > combined_access.log
    ```
*   **`grep`:** Searches for specific strings or patterns inside a log file. This is highly effective for filtering logs by a specific suspicious IP address.
    ```bash
    # Search for all requests made by a specific IP
    grep "192.168.1.1" access.log
    ```
*   **`less`:** Allows for viewing large log files one page at a time to prevent overwhelming the terminal. 
    ```bash
    # Open the log file for paginated viewing
    less access.log
    ```
    *Inside `less`, I can press `/` followed by a string to search, `n` to move to the next occurrence, and `N` to move to the previous occurrence.*
