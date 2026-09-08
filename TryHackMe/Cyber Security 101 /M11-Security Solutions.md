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


# Intro to Firewalls: Concepts and Practical Commands

A firewall is designed to inspect a network's or digital device’s incoming and outgoing traffic, acting as a security guard to prevent unauthorized visitors from sneaking in. Firewalls allow or deny traffic based on maintained rules, and many modern firewalls go beyond rule-based filtering to offer extra protection functionalities.

---

## 1. Types of Firewalls
Different types of firewalls operate on different layers of the OSI model and serve unique purposes.

| Firewall Type | OSI Layer | Characteristics |
| :--- | :--- | :--- |
| **Stateless** | 3 & 4 | Relies on basic filtering and predetermined rules. Keeps no track of previous connections, making it efficient for high-speed networks. |
| **Stateful** | 3 & 4 | Keeps track of previous connections in a state table. Recognizes traffic by patterns and automatically allows or denies subsequent packets based on connection history. |
| **Proxy** | 7 | Acts as an intermediary, masking internal IPs to provide anonymity. Inspects the data inside packets, providing application control and content filtering options. |
| **Next-Generation (NGFW)** | 3 to 7 | Provides advanced threat protection with an intrusion prevention system (IPS). Identifies anomalies via heuristic analysis and decrypts/inspects SSL/TLS data packets. |

---

## 2. Firewall Rules and Components
Firewalls give you control over network traffic through built-in or customized rules. 

### Basic Rule Components
*   **Source address:** The IP address of the machine originating the traffic.
*   **Destination address:** The IP address of the machine receiving the data.
*   **Port:** The port number used for the traffic.
*   **Protocol:** The protocol utilized during communication.
*   **Action:** The step taken when traffic matches the rule (Allow, Deny, Forward).
*   **Direction:** Defines if the rule applies to incoming (Inbound), outgoing (Outbound), or internal (Forward) traffic.

### Types of Actions
*   **Allow:** Permits the specified traffic defined inside the rule.
*   **Deny:** Blocks the specified traffic, which is fundamental for denying malicious IP addresses and reducing the network's threat surface.
*   **Forward:** Redirects traffic to a different network segment, applicable to firewalls acting as gateways.

---

## 3. Windows Defender Firewall
Windows Defender is Microsoft's built-in firewall containing core functionalities for allowing/denying specific programs or creating custom rules.

### Network Profiles
Windows determines your current network using Network Location Awareness (NLA) and applies specific settings:
*   **Private networks:** Firewall configurations applied when connected to trusted home or work networks.
*   **Guest or public networks:** Configurations applied on untrusted networks like coffee shops, where you can block all incoming connections and only allow essential outgoing traffic.

### Custom Rules
Custom rules are created in the "Advanced Settings" menu to strictly control inbound and outbound traffic. 
*   **Example:** You can create an Outbound rule targeting the "TCP" protocol for "Specific ports" 80 and 443, and set the action to "Block the connection". This completely restricts the system from browsing standard HTTP/HTTPS websites.

---

##  Linux Built-in Firewalls
Linux operating systems utilize the **Netfilter** framework, which provides core functionalities like packet filtering, NAT, and connection tracking. 

### Netfilter Utilities
*   **iptables:** The most widely used utility in many Linux distributions for controlling network traffic via Netfilter.
*   **nftables:** The successor to iptables, featuring enhanced packet filtering and NAT capabilities.
*   **firewalld:** Operates on Netfilter with predefined rule sets and pre-built network zone configurations.
*   **ufw (Uncomplicated Firewall):** A beginner-friendly interface that eliminates complex syntax by converting easy commands into iptables rules.

### Essential UFW Command Cheat Sheet
*   **Check status:** `sudo ufw status`.
*   **Enable/Disable firewall:** `sudo ufw enable` or `sudo ufw disable`.
*   **Set default outgoing policy:** `sudo ufw default allow outgoing`.
*   **Deny specific incoming port (e.g., SSH):** `sudo ufw deny 22/tcp`.
*   **List rules with ID numbers:** `sudo ufw status numbered`.
*   **Delete a specific rule by ID:** `sudo ufw delete <rule_number>` (e.g., `sudo ufw delete 2`).


# 3. IDS ( Intrusion Detection System )  Fundamentals 
