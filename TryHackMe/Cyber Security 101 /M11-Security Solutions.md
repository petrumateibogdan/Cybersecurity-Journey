# Module 11: Security Solutions

# 1. Introduction to SIEM ( Security Information and Event Management system )

## SIEM Overview & Log Challenges
A SIEM (Security Information and Event Management) system is a core tool used by SOC analysts to centralize logs. Without a SIEM, analysts face significant challenges:
*   Network devices (host-centric and network-centric) generate hundreds of events per second across isolated devices.
*   Logging into each machine individually to analyze formats is highly inefficient and wastes time during investigations.
*   Isolated logs provide limited context, making it nearly impossible to manually detect complex attacks like lateral movement.

## Core SIEM Features
SIEM solutions solve these issues by providing advanced capabilities to enhance security operations.
*   **Centralization & Normalization:** SIEM pulls logs from all endpoints and converts various raw formats into one consistent, parsed format.
*   **Correlation:** It links activities from different sources (e.g., unusual VPN login plus outbound data) to reveal broader attack patterns.
*   **Real-Time Alerting:** SIEM triggers alerts and notifies analysts when specific conditions within its detection rules are met.
*   **Dashboards:** Analysts receive actionable insights and summaries through visual dashboards highlighting failed logins, ingested events, and triggered rules.

## Log Sources and Ingestion
Different operating systems log events differently (e.g., Windows uses Event Viewer, while Linux stores logs in `/var/log/`). SIEM ingests these logs using:
*   **Agents/Forwarders:** Lightweight tools installed on endpoints that capture and send data directly to the server.
*   **Syslog & Port-Forwarding:** Real-time protocol streams or dedicated listening ports utilized for receiving endpoint data.

## Detection Rules & Alert Investigation
Detection rules use logical expressions and normalized field-value pairs to successfully catch threats.
*   For example, if a Windows EventLog registers Event ID 104, a rule triggers an "Event Log Cleared" alert.
*   Analysts investigate triggered alerts via dashboards to determine if they are True Positives or False Positives.
*   Based on these findings, analysts may tune rules, contact asset owners, block IPs, or isolate infected hosts.


# 2. Firewall Fundamentals



# 3. IDS ( Intrusion Detection System )  Fundamentals 
