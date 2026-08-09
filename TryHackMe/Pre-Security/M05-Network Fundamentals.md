# Module 5: Network Fundamentals

## 1. What is Networking?

**Objective:** 
Dive into the absolute basics of computer networking. I took detailed notes on how devices communicate, the history of the internet, and the core differences between IP and MAC addresses. I also got hands-on with some basic network manipulation in a simulated lab.

###  The Basics & A Little History
At its core, a **network** is simply a group of devices connected together. 
*   **ARPANET:** I learned that the first documented network was the ARPANET project in the late 1960s, funded by the US Defence Department. 
*   **The WWW:** The internet as we use it today (for storing and sharing information) didn't actually come to life until 1989 when Tim Berners-Lee invented the World Wide Web.

###  IP Addresses (The Logical Identity)
An IP (Internet Protocol) address acts as a way to identify a host on a network. It is made up of four sets of numbers called "octets". An important rule I learned is that while IP addresses can change from device to device, no two active devices can have the same IP address on the same network at the same time.

There are two main types:
*   **Private IP:** Used to identify a device among other devices on a local network (e.g., my PC talking to my phone on my home Wi-Fi).
*   **Public IP:** Used to identify the device on the actual Internet. This is assigned by the ISP (Internet Service Provider) and costs money. All devices on a home network share this one public IP when talking to the outside world.

#### IPv4 vs. IPv6
We are running out of IPv4 addresses! IPv4 uses a 32-bit system (`2^32`), capping out at about 4.29 billion addresses. With companies like Cisco estimating over 50 billion connected devices by the end of 2021, the world had to adapt. 
*   This led to **IPv6**, which supports `2^128` addresses (over 340 trillion-plus). 


###  MAC Addresses (The Physical Identity)
While IP addresses are logical and can change, a **MAC (Media Access Control)** address is physically tied to the device's network interface board at the factory. Think of it like a hardware serial number.
*   It is a 12-character hexadecimal number, separated by colons (e.g., `a4:c3:f0:85:ac:2d`).
*   The first six characters identify the manufacturer, and the last six are a unique identifier for that specific chip.

#### MAC Spoofing (Security Flaw)
Even though it's physically burned into the chip, I learned you can fake or **"spoof"** a MAC address in software. This breaks poorly designed security setups. For example, if a firewall only allows traffic from an Administrator's MAC address, a hacker could just spoof their MAC to match the admin's and bypass the firewall entirely. 

###  Practical Lab: Bypassing Captive Portals
I completed an interactive lab simulating a hotel's paid public Wi-Fi. 
*   **The Setup:** Bob hadn't paid for the Wi-Fi, so the router threw his packets in the trash. Alice *had* paid, so her packets went through to the TryHackMe website.
*   **The Exploit:** By changing (spoofing) Bob's MAC address to match Alice's, the router was tricked into thinking Bob's device was actually Alice's device, granting him free internet access.

###  The Ping Command (ICMP)
Finally, I explored `ping`, one of the most fundamental network troubleshooting tools.
*   It uses **ICMP (Internet Control Message Protocol)** to test if a connection between devices exists and is reliable.
*   It works by sending an "echo packet" to a target and waiting for an "echo reply". The time it takes for that round trip is measured in milliseconds.
*   **Syntax:** `ping [IP address or URL]` (e.g., `ping 8.8.8.8`).

## 2. Intro to LAN


###  Network Topologies
A topology is simply the physical design or look of the network. I looked at the three main types:

#### Star Topology
*   This is the most common design today, where every device connects individually to a central device, like a switch or a hub.
*   All data sent to any device must pass through this central hardware.
*   **The Pros:** It is incredibly reliable and highly scalable, meaning it is very easy to add more devices as the network grows.
*   **The Cons:** It is the most expensive option because it requires more cabling and dedicated hardware. As it scales, it requires more maintenance, making troubleshooting difficult. If the central switch fails, the entire network drops, though these devices are usually built to be robust.

#### Bus Topology
*   This setup relies on a single "backbone" cable, where devices branch off of it like leaves on a tree.
*   **The Pros:** It is very cost-efficient and easy to set up because it requires less equipment and cabling.
*   **The Cons:** It is highly prone to bottlenecking and slowing down if multiple devices request data at the same time, since everything shares one route. This also makes troubleshooting extremely difficult. It has a massive single point of failure: if the backbone cable breaks, no devices can send or receive data.

#### Ring (Token) Topology
*   Devices are connected directly to one another to form a continuous loop. 
*   Data travels in one direction around the loop until it hits its target destination. Devices will only forward someone else's data if they don't have their own data to send first.
*   **The Pros:** It requires very little cabling and no central hardware. Because data flows in one direction, troubleshooting faults is fairly easy, and it is less prone to bottlenecks compared to a bus topology.
*   **The Cons:** It is inefficient because data might have to travel through multiple devices to reach its goal. Like the bus topology, a single broken cable or failed device will break the entire network.

###  Networking Hardware
*   **Switches:** These are dedicated devices used to aggregate multiple connections (like computers and printers) using Ethernet. They typically have anywhere from 4 to 64 ports and are common in larger networks like businesses. Unlike older "hubs" that blast data to every port, switches are efficient: they remember which device is plugged into which port and send data *only* to the intended target, cutting down on traffic.
*   **Routers:** A router's job is to connect different networks together and pass data between them. It creates paths for data to travel successfully, a process literally called "routing". Connecting routers and switches together adds redundancy, so if one path drops, the data just takes another route.

###  Subnetting
Subnetting is the process of splitting a large network into smaller, miniature networks. I like to think of it like slicing a cake: there is limited space, and subnetting decides who gets which slice. Businesses use this to separate departments (like HR, Finance, and Accounting). 

It uses a "subnet mask" (a 32-bit number ranging from 0-255) to divide things up. I learned three crucial address types within a subnet:
*   **Network Address:** Identifies the start and existence of a network (e.g., `192.168.1.0`).
*   **Host Address:** Identifies a specific, individual device on that network (e.g., `192.168.1.100`).
*   **Default Gateway:** A special address (usually ending in `.1` or `.254`) assigned to the device that handles sending data to *other* networks.

**Why Subnet?** It offers efficiency, full control, and most importantly, security. For example, a café can use subnetting to keep their employee cash register network completely separate and secure from their public guest Wi-Fi.

###  Essential Protocols (ARP & DHCP)
*   **ARP (Address Resolution Protocol):** This is how devices map a physical MAC address to a logical IP address. A device broadcasts an "ARP Request" asking the network who owns a specific IP. The owner sends an "ARP Reply" with its MAC address, and the requesting device saves this mapping in a ledger called an ARP cache for the future.
*   **DHCP (Dynamic Host Configuration Protocol):** This is the server that automatically assigns IP addresses so we don't have to type them manually. I learned the four-step process:
    1.  **DHCP Discover:** A new device asks the network, "Are there any DHCP servers here?".
    2.  **DHCP Offer:** The server replies, "Yes, here is an IP address you can use.".
    3.  **DHCP Request:** The device says, "I accept this IP address.".
    4.  **DHCP ACK:** The server sends an acknowledgment, finalizing the process so the device can use the IP.
