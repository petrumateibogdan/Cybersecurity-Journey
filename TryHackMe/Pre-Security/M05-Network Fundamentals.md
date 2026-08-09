# Module 5: Network Fundamentals

## 1.What is Networking?

**Objective:** 
Dive into the absolute basics of computer networking. I took detailed notes on how devices communicate, the history of the internet, and the core differences between IP and MAC addresses. I also got hands-on with some basic network manipulation in a simulated lab.

### 1. The Basics & A Little History
At its core, a **network** is simply a group of devices connected together. 
*   **ARPANET:** I learned that the first documented network was the ARPANET project in the late 1960s, funded by the US Defence Department. 
*   **The WWW:** The internet as we use it today (for storing and sharing information) didn't actually come to life until 1989 when Tim Berners-Lee invented the World Wide Web.

### 2. IP Addresses (The Logical Identity)
An IP (Internet Protocol) address acts as a way to identify a host on a network. It is made up of four sets of numbers called "octets". An important rule I learned is that while IP addresses can change from device to device, no two active devices can have the same IP address on the same network at the same time.

There are two main types:
*   **Private IP:** Used to identify a device among other devices on a local network (e.g., my PC talking to my phone on my home Wi-Fi).
*   **Public IP:** Used to identify the device on the actual Internet. This is assigned by the ISP (Internet Service Provider) and costs money. All devices on a home network share this one public IP when talking to the outside world.

#### IPv4 vs. IPv6
We are running out of IPv4 addresses! IPv4 uses a 32-bit system (`2^32`), capping out at about 4.29 billion addresses. With companies like Cisco estimating over 50 billion connected devices by the end of 2021, the world had to adapt. 
*   This led to **IPv6**, which supports `2^128` addresses (over 340 trillion-plus). 
*   As shown in `image_de8248.png`, you can clearly see the difference in formatting between an older IPv4 address and a massive, hex-based IPv6 address.

### 3. MAC Addresses (The Physical Identity)
While IP addresses are logical and can change, a **MAC (Media Access Control)** address is physically tied to the device's network interface board at the factory. Think of it like a hardware serial number.
*   It is a 12-character hexadecimal number, separated by colons (e.g., `a4:c3:f0:85:ac:2d`).
*   The first six characters identify the manufacturer, and the last six are a unique identifier for that specific chip.

#### MAC Spoofing (Security Flaw)
Even though it's physically burned into the chip, I learned you can fake or **"spoof"** a MAC address in software. This breaks poorly designed security setups. For example, if a firewall only allows traffic from an Administrator's MAC address, a hacker could just spoof their MAC to match the admin's and bypass the firewall entirely. 

### 4. Practical Lab: Bypassing Captive Portals
I completed an interactive lab simulating a hotel's paid public Wi-Fi. 
*   **The Setup:** Bob hadn't paid for the Wi-Fi, so the router threw his packets in the trash. Alice *had* paid, so her packets went through to the TryHackMe website.
*   **The Exploit:** By changing (spoofing) Bob's MAC address to match Alice's, the router was tricked into thinking Bob's device was actually Alice's device, granting him free internet access.

### 5. The Ping Command (ICMP)
Finally, I explored `ping`, one of the most fundamental network troubleshooting tools.
*   It uses **ICMP (Internet Control Message Protocol)** to test if a connection between devices exists and is reliable.
*   It works by sending an "echo packet" to a target and waiting for an "echo reply". The time it takes for that round trip is measured in milliseconds.
*   **Syntax:** `ping [IP address or URL]` (e.g., `ping 8.8.8.8`).
