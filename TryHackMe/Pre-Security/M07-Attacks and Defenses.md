# Module 7: Attacks and Defense

## 1. The CIA Triad


**Objective:**
I took my first step directly into the core of cybersecurity by learning what we are actually trying to protect. I explored the three foundational pillars of security, known as the CIA Triad, and learned how to use them as a mindset for evaluating real-world security incidents.

### What is the CIA Triad?
Everything in cybersecurity revolves around protecting digital data and systems. Being secure means ensuring specific conditions for that data. These conditions are known as the **CIA Triad**: Confidentiality, Integrity, and Availability. Whether defending systems or acting as an attacker, my goal is to either preserve or break these three pillars.

### Confidentiality
Confidentiality ensures that sensitive digital information can *only* be accessed by authorized individuals. 
*   If this is breached, it leads to data exposure, privacy violations, and financial loss (e.g., someone intercepting my login credentials on a public coffee shop Wi-Fi).
*   To ensure confidentiality in the digital world, professionals use processes like encryption and strict access controls.

| Situation | Confidentiality Achieved? |
| :--- | :--- |
| My Gmail credentials are written on sticky notes on my office table. | No |
| Internal company documents are available to the employees who need them. | Yes |
| One of my personal, private documents is available on the internet. | No |

### Integrity
Integrity ensures that data is not modified without permission by unauthorized individuals.
*   Without integrity, data cannot be trusted. Unauthorized changes can have dangerous consequences (e.g., someone modifying a bank transfer transaction before it completes to change the receiving account).
*   Various security techniques are used to ensure data remains completely unaltered in transit and at rest.

| Situation | Integrity Achieved? |
| :--- | :--- |
| Data was changed through an authorized approval process. | Yes |
| Attendance records were changed *after* being officially locked by the teacher. | No |
| The price of an order was modified by a user just before checkout. | No |

### Availability
Availability ensures that data and services are actually accessible to authorized users exactly when they are needed.
*   If a service goes down, businesses lose money and users are stranded. For example, if a website receives too many requests at once (like in a Denial of Service attack), it crashes. No data is leaked or modified, but the system is entirely unusable.
*   To maintain availability, websites implement alternative power sources, backup servers, and traffic management systems to block excessive requests.

| Situation | Availability Achieved? |
| :--- | :--- |
| Critical services were disrupted by the installation of a new software update. | No |
| A company's website went offline during peak business hours. | No |
| All systems are fully accessible to employees during their working hours. | Yes |

### The Security Mindset
I learned that the CIA Triad is not just a set of definitions; it is the fundamental *security mindset* of a cybersecurity professional. When a security incident occurs, I must immediately assess the impact by asking:
*   **Confidentiality:** Was sensitive data exposed to unauthorized individuals?
*   **Integrity:** Was any data modified without permission?
*   **Availability:** Were systems or services unavailable to users when they needed them?

## 2. Cryptography Concepts

**Objective:**
I explored how cryptography protects confidentiality and integrity in the real world. I learned the fundamental differences between symmetric and asymmetric encryption, how keys and algorithms function together, and how they secure everyday web browsing through HTTPS.

### Core Terminology
Before diving into encryption types, I established the basic pattern of cryptography: `plaintext + encryption algorithm + key = ciphertext`.
*   **Plaintext:** A message I can read normally (e.g., `HELLO`).
*   **Ciphertext:** A scrambled version that looks like random nonsense and is not supposed to make sense without the key (e.g., `KHOOR`).
*   **Key:** The secret ingredient that controls how the scrambling and unscrambling work.
*   **Algorithm:** The public set of steps that explain how to use the key on the message. Security comes from keeping the key secret, not the algorithm.

### Symmetric Encryption
Symmetric encryption uses the exact same key to both encrypt (lock) and decrypt (unlock) the message.
*   It is incredibly fast and efficient, making it perfect for encrypting large amounts of data like files or network traffic.
*   **The Key Distribution Problem:** The major flaw is that both the sender and receiver need a copy of the exact same key. If they send the key over the internet in plain view, an eavesdropper can grab it and decrypt all future messages.

### Asymmetric Encryption
To solve the key distribution problem, asymmetric encryption uses two mathematically linked keys instead of one.
*   **Public Key:** A key that anyone can know and use. If I encrypt something with a public key, only the corresponding private key can decrypt it.
*   **Private Key:** A key that only one person keeps absolutely secret.
*   This eliminates the need to securely share a secret key beforehand. I can share my public key openly (like a mail slot), and anyone can drop a message inside, but only I possess the private key (the door key) to open and read it.

### HTTPS and Digital Certificates
Real-world systems, like the HTTPS protocol powering secure websites, use a hybrid approach to get the best of both worlds:
*   My browser requests the website's public key, which is wrapped in a **Certificate**.
*   A trusted **Certificate Authority (CA)** digitally signs this certificate to prove the website is legitimate.
*   My browser and the website use asymmetric encryption to securely agree on a shared symmetric key.
*   They then switch to fast symmetric encryption using that shared secret for the remainder of the session.

### Encryption Comparison

| Feature | Symmetric Encryption | Asymmetric Encryption |
| :--- | :--- | :--- |
| **Number of keys** | One key for both encrypting and decrypting. | Two keys: public and private. |
| **Key sharing** | Both people need the same secret key. | Public key can be shared openly. |
| **Speed** | Very fast. | Slower (used for small amounts of data). |
| **Main use** | Encrypting bulk data (files, network traffic). | Sharing keys securely and digital certificates. |

## 3. Become a Hacker


**Objective:**
I explored the fundamentals of offensive security, learning how ethical hackers proactively test systems to identify weaknesses before malicious actors can exploit them. I gained hands-on experience using industry-standard tools to chain vulnerabilities together, proving that small flaws can lead to significant access when combined.

### Core Offensive Security Terms
*   **Red Teaming:** A structured, authorized attack methodology that simulates a real adversary to test defenses.
*   **Penetration Test:** A structured assessment where an authorized tester attempts to exploit vulnerabilities to understand real-world risk.
*   **Vulnerability:** A weakness or flaw in a system, application, or configuration.
*   **Exploit:** A technique used to take advantage of a vulnerability.
*   **Scope:** The exact boundaries of what is allowed to be tested during an engagement.
*   **Permission:** The critical rule separating ethical hackers from malicious actors. All testing must be controlled, legal, and within the defined scope.

### The Hacker Mindset
To test a system effectively, I learned to look beyond whether a feature simply "works" and instead ask how it might be misused.
*   **Test the unexpected:** Try actions and inputs the developers didn't consider.
*   **Chain small weaknesses:** A tiny flaw (like a hidden login page) may be harmless alone, but dangerous when combined with another flaw (like weak passwords).
*   **Think like an adversary:** Approach targets methodically, looking for exposed sensitive functionality, user data, and administrative features.

### Hands-On Exercise: Finding Hidden Pages
My first goal in the simulated assessment was to identify any hidden pages on the target web server that shouldn't be publicly accessible. 

*   **Manual Testing:** I started by manually appending potential directories (like `/sitemap`, `/login`, `/admin`) to the URL to see if they returned an Error 404 (Not Found) or a valid page.
*   **Automated Enumeration (Gobuster):** Because manual testing is too slow for large targets, I used a command-line tool called **Gobuster** to automate the process.
`gobuster dir --url http://www.onlineshop.thm/ -w /usr/share/wordlists/dirbuster/directory-list.txt`


    *   `gobuster dir`: Specifies directory enumeration mode.
    *   `--url http://www.onlineshop.thm/`: Sets the target website.
    *   `-w /usr/share/wordlists/dirbuster/directory-list.txt`: Specifies the wordlist Gobuster will use to rapidly guess directory names.

### Hands-On Exercise: Bruteforcing Credentials
After finding the hidden login page, the next step was to chain that vulnerability by attempting to bypass the authentication.

*   **Dictionary Attack:** This is a technique that relies on a predefined list of possible passwords (a wordlist) rather than guessing randomly.
*   **Automated Attacking (Hydra):** I used **Hydra**, a powerful password-testing tool, to automate login attempts.
  
`hydra -l admin -P passlist.txt www.onlineshop.thm http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V` 
    *   `hydra -l admin`: Sets the target username as "admin".
    *   `-P passlist.txt`: Specifies the list of passwords to try.
    *   `www.onlineshop.thm`: Sets the target website.
    *   `http-post-form`: Tells Hydra exactly how the login form submits data.
    *   `-V`: Enables verbose output so I could watch every attempt in real-time.

### Potential Career Paths in Offensive Security
Completing this module highlighted several roles where these skills are used professionally:
*   **Penetration Tester / Ethical Hacker:** Focuses on safely exploring vulnerabilities within a defined scope.
*   **Vulnerability Researcher:** Identifies and validates undiscovered weaknesses in software and hardware.
*   **Red Team Operator:** Simulates real-world adversaries to thoroughly test an organization's detection and response capabilities.

## 4. Become a Defender


**Objective:**
I explored the fundamentals of defensive security, focusing on what needs to be protected and how to implement security measures to prevent, detect, and mitigate attacks. I learned how defenders (the Blue Team) approach securing client infrastructure by maintaining visibility, understanding attacker paths, and applying layered defenses.

### What is Defensive Security?
Defensive Security focuses on understanding what needs to be protected and implementing security measures to prevent, detect, and mitigate the impact of potential attacks. The goal is to ensure systems remain available and protected, aligning with the CIA Triad (Confidentiality, Integrity, Availability). 

While ethical hackers (the Red Team) proactively test systems by breaking into them, defenders (the Blue Team) work to gain visibility into those systems, identify weak points, and prepare to respond when incidents occur.

### Foundational Security Concepts
I learned that defenders organize their work around a set of foundational concepts that apply across nearly all environments:
*   **Prevention:** Putting security controls in place to stop attacks before they happen (e.g., firewalls, antivirus, patching).
*   **Detection:** Monitoring systems and networks to identify suspicious or malicious activity through logs, alerts, and security tools.
*   **Mitigation:** Taking action during an incident to limit damage (e.g., blocking traffic, isolating systems, disabling compromised accounts).
*   **Analysis:** Investigating what happened, how it happened, and which systems were affected by reviewing logs and evidence.
*   **Response and Improvement:** Recovering from the incident and improving defenses to reduce the risk of future attacks.

### Mapping the Environment (The City Analogy)
Before you can protect anything, you need clear visibility into what exists. Defenders are not responsible for protecting the entire internet, only the systems belonging to their organization or client (their "Scope"). 

To understand this, I mapped a client's infrastructure using a city analogy:
| System Component | Purpose | City Analogy | Defensive Focus |
| :--- | :--- | :--- | :--- |
| **Employee Devices** | Where users work and access company resources. | Homes | Protect via Antivirus and regular updates. |
| **Web Server** | Hosts websites or applications accessed by users. | Shop/Public buildings | Only allow safe traffic and use secure communication. |
| **Mail Server** | Sends and receives email for the organization. | Post office | Use spam filters and scan attachments. |
| **Firewall** | Controls what traffic is allowed in or out. | City gate | Use firewall rules to block known troublemakers and control access. |
| **The Internet** | External networks not controlled by the organization. | Outside the city walls | Restrict inbound traffic and monitor for suspicious activity. |

### The Defender Mindset
A successful defender must understand how attackers operate. Attackers rarely target a single system; instead, they compromise one asset and pivot to the next, building an interconnected attack chain. 

To counter this, defenders apply key principles:
*   **Threat anticipation:** Review systems and ask "What if?" to imagine realistic paths an attacker may take.
*   **Attack awareness:** Study common attack chains and frameworks, as attacks typically follow recognizable stages.
*   **Risk prioritization:** Identify high-value systems, because not every part of the system carries equal risk.
*   **Continuous adaptation:** Understand that threats evolve and defense is not a one-time setup.

### Potential Career Paths in Defensive Security
Completing this module highlighted several roles where defensive skills are used professionally:
*   **Security Operations Center (SOC) Analyst:** Monitors networks and systems to detect and investigate suspicious activity.
*   **Threat Intelligence Analyst:** Researches current threats, attackers, and trends to help prepare against potential attacks.
*   **Digital Forensics & Incident Response (DFIR):** Investigates security incidents to understand how an attack happened and contains the threat to restore systems.

--FINISHED PRE-SECURITY--
