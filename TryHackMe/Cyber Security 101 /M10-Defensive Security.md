# Module 10: Defensive Security

# 1. SOC Fundamentals



I recently completed a module on the fundamentals of a Security Operations Center (SOC). Here are my notes on how a SOC operates, its core pillars, and the defensive technologies utilized to protect organizational networks.

### 1. The Core Focus: Detection and Response
*   **Detection:** Identifying system vulnerabilities, unauthorized access, security policy violations, and network intrusions.
*   **Response:** Supporting incident response teams by minimizing impact and performing root-cause analysis on detected threats.

### 2. The First Pillar: People
*   **Level 1 Analysts:** Serve as first responders who perform basic alert triage to determine if a detection is genuinely harmful.
*   **Level 2 Analysts:** Conduct deeper investigations by correlating data from multiple sources to perform a proper analysis.
*   **Level 3 Analysts:** Proactively hunt for threat indicators and handle critical incidents requiring containment, eradication, and recovery.
*   **Engineers & Management:** Security Engineers configure tools, Detection Engineers build alert logic, and the SOC Manager oversees processes and communicates with the CISO.

### 3. The Second Pillar: Process
*   **Alert Triage:** The initial analysis of an alert to determine severity and priority by answering the "5 Ws" (What, When, Where, Who, Why).
*   **Reporting:** Escalating validated threats to higher-level analysts via ticketing systems, ensuring reports include thorough analysis and evidence.
*   **Forensics & Incident Response:** Analyzing system or network artifacts during critical incidents to determine the exact root cause.

### 4. The Third Pillar: Technology
*   **SIEM (Security Information and Event Management):** Centralizes logs from various network devices, correlating data against detection rules to identify suspicious activity (Detection only).
*   **EDR (Endpoint Detection and Response):** Provides real-time and historical visibility at the device level, allowing analysts to investigate and execute automated responses.
*   **Firewall:** Acts as a network barrier to filter unauthorized traffic and block suspicious activity before it reaches the internal network.
