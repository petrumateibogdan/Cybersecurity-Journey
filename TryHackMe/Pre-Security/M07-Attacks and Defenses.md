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

