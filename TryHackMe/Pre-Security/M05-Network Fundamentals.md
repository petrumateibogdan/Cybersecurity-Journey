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


    ## 3.OSI Model (Open Systems Interconnection Model)


**Objective:** 
I tackled the OSI (Open Systems Interconnection) Model. It is basically the universal rulebook for how devices communicate over a network. By following this model, completely different devices can understand each other. I learned that as data travels through these layers, specific information is added in a process called **encapsulation**.

### The 7 Layers (Top to Bottom)
I wanted a quick visual reference for my notes, so here is a breakdown of the layers from 7 down to 1. 

```yaml
# THE OSI MODEL
Layer_7: "Application"   # What I interact with (Browsers, GUI, Email)
Layer_6: "Presentation"  # The Translator (Data formatting, HTTPS Encryption)
Layer_5: "Session"       # The Connection (Maintains sessions, Checkpoints)
Layer_4: "Transport"     # The Delivery (TCP vs. UDP)
Layer_3: "Network"       # The GPS (Routing, IP Addresses, OSPF/RIP)
Layer_2: "Data Link"     # The Hardware Identity (MAC Addresses, NICs)
Layer_1: "Physical"      # The Real World (Cables, Electrical Signals, 1s & 0s)
```

---

### Layer 7: Application
This is the layer I am most familiar with because it is what the user actually sees. It provides the Graphical User Interface (GUI) for software like web browsers, email clients, and FTP tools (like FileZilla). It also handles protocols like DNS, which translates website names into IP addresses.

### Layer 6: Presentation
Because different software developers write code differently, this layer acts as a translator. It ensures that no matter what email client I use, the contents of the email display correctly. Crucially, this is also where security features like data encryption (HTTPS) happen.

### Layer 5: Session
Once data is formatted, this layer creates and maintains the connection (the "session") with the target computer. 
*   It handles closing the connection if it times out.
*   It creates "checkpoints." If data is lost, I don't have to restart the whole download; it just resumes from the last checkpoint, saving bandwidth.
*   Data can only travel across its own unique session.

### Layer 4: Transport (TCP vs. UDP)
This layer decides exactly how the data is delivered. It uses one of two protocols: TCP or UDP.

#### TCP (Transmission Control Protocol)
TCP guarantees delivery. It reserves a constant connection and constantly checks for errors to make sure all the small chunks of data arrive and are reassembled in the perfect order. 

| Advantages of TCP | Disadvantages of TCP |
| :--- | :--- |
| Guarantees 100% accuracy of data. | Requires a reliable connection; if one chunk fails, the whole file is useless. |
| Synchronizes devices to prevent data flooding. | Slower, because reserving the connection can bottleneck devices. |
| Built-in error checking. | Too much processing overhead for quick tasks. |

> **Use Case:** File sharing, internet browsing, and emails (situations where having "half a file" is useless).

#### UDP (User Datagram Protocol)
UDP just throws the data at the target and hopes for the best. There is no error checking and no guaranteed delivery. 

| Advantages of UDP | Disadvantages of UDP |
| :--- | :--- |
| Significantly faster than TCP. | Doesn't care if the data actually arrives. |
| Leaves the application to decide packet speed (flexible for devs). | No reserved connection; terrible experience if the network is unstable. |

> **Use Case:** Video streaming (where a dropped pixel is fine) and small network requests like ARP or DHCP.

### Layer 3: Network
This is where routing and IP addresses (like `192.168.1.100`) live. Layer 3 devices, like Routers, figure out the absolute best path to send data chunks. They decide this based on:
1.  **Shortest Path:** Fewest devices to hop across.
2.  **Reliability:** Avoiding paths where packets were lost before.
3.  **Speed:** Prioritizing faster physical cables (like fiber over copper).

### Layer 2: Data Link
This layer focuses on physical addressing. It takes the data from Layer 3 (which has the IP address) and attaches the **MAC Address**. Every computer has a Network Interface Card (NIC) with a MAC address permanently burned into it by the manufacturer. While it can be spoofed, it is the physical identity used to send data across the local network.

### Layer 1: Physical
The absolute lowest layer. This is the tangible hardware: ethernet cables, electrical signals, and raw binary data (1s and 0s).


## 4. Packets & Frames


**Objective:** 
 I explored how data is actually packaged for travel, the specific rules (TCP vs. UDP) that govern that travel, and how ports ensure data reaches the correct application on a machine.

###  Packets vs. Frames (The Mail Analogy)
I learned that when data travels over a network, it doesn't go all at once (which would cause massive bottlenecks). It gets chopped up into small pieces. There is a crucial difference in what we call these pieces depending on where they are in the OSI model:
*   **Packet (Layer 3 - Network):** This contains the actual data (payload) along with an IP header. 
*   **Frame (Layer 2 - Data Link):** This encapsulates the packet and adds the physical MAC addresses. 

> **The Analogy:** I like to think of it like mailing a letter. The **frame** is the envelope, which moves the contents to another place. The **packet** is the letter inside. Once the envelope (frame) is opened and stripped away, the recipient reads the letter (packet) to know what to do next.

###  TCP (Transmission Control Protocol)
TCP is a connection-based protocol. It is incredibly reliable because it establishes a strict connection between two devices before sending anything, and it guarantees delivery.

#### The TCP/IP Model
I noted that TCP/IP has its own simplified, 4-layer version of the OSI model:
1.  Application
2.  Transport
3.  Internet
4.  Network Interface

#### The Three-Way Handshake
Because TCP guarantees delivery, it has to set up a connection first using a process called the Three-Way Handshake. I broke down the sequence of messages used to establish and close this connection:

| Step | Message | Description |
| :--- | :--- | :--- |
| 1 | **SYN** | Client sends this to initiate a connection and synchronize sequence numbers. |
| 2 | **SYN/ACK** | Server acknowledges the attempt and sends its own synchronization number. |
| 3 | **ACK** | Client acknowledges the server's response. The connection is now established! |
| 4 | **DATA** | The actual bytes of the file are transmitted. |
| 5 | **FIN** | Used to cleanly and properly close the connection when finished. |
| *Fail* | **RST** | Abruptly kills all communication if there is a severe error. |

*Note: TCP packets contain specific headers, such as Source/Destination IP, Source/Destination Port, a Checksum for integrity, and a Sequence Number to ensure chunks are reassembled in the exact right order.*

###  UDP (User Datagram Protocol)
I compared TCP with its counterpart, UDP. UDP is completely stateless. There is no Three-Way Handshake, no synchronization, and no guarantee that the data actually arrives. 

| Advantages of UDP | Disadvantages of UDP |
| :--- | :--- |
| Incredibly fast compared to TCP. | Does not care if data is dropped or lost. |
| Gives software developers flexibility over packet speed. | No continuous connection; bad for unstable networks. |

> **Use Case:** Because it lacks complex headers and error checking, UDP is perfect for video streaming or voice chat, where losing a single packet just means a dropped pixel or a millisecond audio stutter, rather than stopping the entire stream to fix an error.

### 4. Ports (The Harbour)
Ports are numerical values (ranging from `0` to `65535`) that act as specific docking stations for data. 
*   **The Analogy:** Think of a massive harbour. A giant cruise liner cannot dock at a tiny fishing port. Ports enforce what kind of traffic can "park" where, so a computer knows exactly which application should handle the incoming data.

To prevent chaos, the industry agreed on standard **Common Ports** (ranging from 0 to 1024). I documented the most critical ones for cybersecurity:

| Protocol | Port | Description |
| :--- | :--- | :--- |
| **FTP** | 21 | File Transfer Protocol (Downloading/sharing files). |
| **SSH** | 22 | Secure Shell (Secure, text-based login to manage a remote system). |
| **HTTP** | 80 | Powers the web! Used by browsers to download unencrypted web pages. |
| **HTTPS** | 443 | The exact same as HTTP, but securely encrypted. |
| **SMB** | 445 | Server Message Block (Sharing files and devices, like printers, locally). |
| **RDP** | 3389 | Remote Desktop Protocol (Securely logging into a system with a visual GUI desktop). |


## 5. Extending Your Network


###  Port Forwarding
I learned that without port forwarding, applications (like a locally hosted web server) are completely invisible to the outside Internet; they only exist on the local intranet. 
*   **What it does:** It opens specific ports on a router to allow outside, public Internet traffic to reach a specific device on the private network.
*   **The distinction:** It is easy to confuse this with a firewall. Port forwarding *opens* the door, while a firewall determines *who* is allowed to walk through it.

###  Firewalls (Network Border Security)
A firewall acts as border security, inspecting packets to permit or deny traffic based on where it came from, where it is going, the port it is using, and the protocol (TCP/UDP). I compared the two main categories:

| Firewall Type | Description | Pros & Cons |
| :--- | :--- | :--- |
| **Stateful** | Inspects the *entire* connection behavior, not just one packet. | **Pros:** Smart and dynamic. If a connection turns bad, it blocks the whole device. <br> **Cons:** Consumes a lot of system resources. |
| **Stateless** | Uses a static, hard-coded set of rules to check individual packets. | **Pros:** Uses very few resources. Great for absorbing massive DDoS attacks. <br> **Cons:** Very "dumb." If a packet doesn't perfectly match a rule, the firewall is useless. |

###  VPNs (Virtual Private Networks)
A VPN creates a dedicated, encrypted path (a "tunnel") over the Internet, allowing devices on completely separate networks to communicate securely as if they were in the same room.

**Why use a VPN?**
*   **Geographical Connection:** Allows businesses to link multiple remote offices to one central server.
*   **Privacy:** Encrypts data, making it unreadable to anyone trying to sniff the network (especially crucial on public Wi-Fi).
*   **Anonymity:** Hides traffic from ISPs, allowing activists or journalists to bypass censorship (though it relies heavily on the VPN provider not keeping logs!).

**VPN Technologies I Explored:**
*   **PPP:** Uses a private key and public certificate for authentication and encryption.
*   **PPTP (Point-to-Point Tunneling Protocol)**  :  Very easy to set up and widely supported, but uses weak encryption.
*   **IPSec:** Encrypts data using the existing IP framework. It is difficult to configure, but boasts incredibly strong encryption.

###  Routers vs. Switches
I finalized my notes by comparing the two most important pieces of networking hardware.

#### Routers (Layer 3)
*   Their primary job is to connect *different* networks together.
*   They decide the optimal path for data to travel based on the shortest route, the most reliable path, and the fastest physical medium (like fiber vs. copper).

#### Switches (Layer 2 & Layer 3)
*   Their job is to connect multiple specific devices *within the same* network using Ethernet cables.
*   **Layer 2 Switches:** Forward frames strictly using physical MAC addresses.
*   **Layer 3 Switches:** More advanced; they can route IP packets just like a router.
*   **VLANs (Virtual Local Area Networks):** I learned that switches can use VLAN technology to virtually segregate a network. For example, the Sales team and Accounting team can plug into the exact same physical switch, but a VLAN ensures they cannot view each other's private data.
    
