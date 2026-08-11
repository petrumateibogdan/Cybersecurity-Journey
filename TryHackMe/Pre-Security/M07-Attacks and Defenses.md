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
