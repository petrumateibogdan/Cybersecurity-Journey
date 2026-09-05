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

## 3. Windows Forensics
When investigating personal computers and laptops, I learned to capture two distinct types of forensic images.

*   **Memory Image (Volatile):** Data inside RAM (running processes, network connections) that is completely lost upon system shutdown. This must be captured first using **DumpIt** or analyzed with **Volatility**.
*   **Disk Image (Non-Volatile):** Permanent storage data (documents, media, browsing history) acquired and analyzed using tools like **FTK Imager** or **Autopsy**.

## 4. Metadata Command Cheat Sheet
Digital activities always leave traces. I use specific command-line tools to extract hidden metadata from files and images.

| Tool | Function | Example Command |
| :--- | :--- | :--- |
| **pdfinfo** | Extracts document creation dates, authors, and the exact software used to generate the file. | `pdfinfo DOCUMENT.pdf` |
| **exiftool** | Extracts embedded Exchangeable Image File Format (EXIF) data, including camera settings and GPS coordinates that can be mapped online. | `exiftool IMAGE.jpg` |
