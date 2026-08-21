# Module 5: Networking

# 1. Networking Concepts

##  The OSI Model (The Theoretical Blueprint)
To understand how devices actually talk to each other, I first needed to grasp the **OSI (Open Systems Interconnection) Model**. It is a 7-layer theoretical framework. A great mnemonic to remember the layers from bottom to top (Layer 1 to Layer 7) is: **"Please Do Not Throw Spinach Pizza Away"**.

* **Layer 1: Physical Layer** - The actual cables (Ethernet, fiber optic) and radio waves (WiFi). Data is just 1s and 0s here.
* **Layer 2: Data Link Layer** - Communication on the *same* local network segment. Devices use **MAC addresses** (Media Access Control) to identify each other. 
* **Layer 3: Network Layer** - Communication *between* different networks. This is where routing and **IP Addresses** live (e.g., IPv4, ICMP, IPSec).
* **Layer 4: Transport Layer** - End-to-end communication between applications. This is where **TCP** and **UDP** port numbers are assigned.
* **Layer 5: Session Layer** - Establishes, maintains, and kills sessions between applications (e.g., NFS ( Network File System ) , RPC ( Remote Procedure Call) ).
* **Layer 6: Presentation Layer** - Data translation, compression, and encryption. It formats data so the application can read it (e.g., JPEG, MIME, Unicode).
* **Layer 7: Application Layer** - The layer closest to me, the user. This is where my web browser operates using protocols like HTTP, DNS, FTP, and SMTP.


##  The TCP/IP Model (The Practical Reality)
While the OSI model is great for learning, the real world largely runs on the **TCP/IP Model**. It condenses the 7 layers down into 4 (or sometimes 5, if counting physical):
* **Application Layer** (Combines OSI Layers 5, 6, and 7)
* **Transport Layer** (OSI Layer 4)
* **Internet Layer** (OSI Layer 3)
* **Link Layer** (OSI Layer 1 and 2)

##  IP Addressing and Subnets
Every device on a network needs a unique identifier—the **IP Address**. The standard IPv4 address is 32 bits long, divided into four octets (e.g., `192.168.66.89`).
* The very first address (e.g., `192.168.1.0`) is reserved as the **Network Address**.
* The very last address (e.g., `192.168.1.255`) is the **Broadcast Address** (sends data to everyone on the subnet).
* I can check my network config using `ipconfig` on Windows or `ip a s` on Linux. A subnet mask like `255.255.255.0` (or `/24`) means the first three numbers of the IP are locked to the network, and only the final number changes for individual devices.

### ⚠️ CRITICAL: The Private IP Address Ranges ⚠️
Most IP addresses are public (like a home postal address). However, there are specific ranges dedicated to **Private IP Addresses**—these are strictly for internal networks and cannot be routed directly over the public internet without Network Address Translation (NAT). **I absolutely must memorize these:**

* **`10.0.0.0` - `10.255.255.255`** (The `10.0.0.0/8` range)
* **`172.16.0.0` - `172.31.255.255`** (The `172.16.0.0/12` range)
* **`192.168.0.0` - `192.168.255.255`** (The `192.168.0.0/16` range)

##  Transport Protocols: UDP vs. TCP
At Layer 4, the network uses port numbers (ranging from 1 to 65535) to figure out which exact application should receive the data. 

* **UDP (User Datagram Protocol):** Connectionless and fast. It just fires data at the target and doesn't care if it actually arrives. Think of it like standard snail mail with no tracking. Great for video streaming or gaming where speed beats reliability.
* **TCP (Transmission Control Protocol):** Connection-oriented and highly reliable. It guarantees delivery by acknowledging received data.
  * **The TCP 3-Way Handshake:** Before sending data, TCP establishes a connection:
    1. **SYN:** Client says "Hello, I want to connect."
    2. **SYN-ACK:** Server says "I hear you, let's connect."
    3. **ACK:** Client says "Great, acknowledging your reply."

##  Encapsulation (The Life of a Packet)
When I send a message, it travels down the OSI layers, getting "wrapped" in new headers at each step. This is **Encapsulation**.
1. **Application Data:** I type a search query.
2. **Segment (Transport Layer):** A TCP header (with source/dest ports) is added to the data.
3. **Packet (Network Layer):** An IP header (with source/dest IP addresses) is wrapped around the segment.
4. **Frame (Data Link Layer):** A MAC header and trailer are wrapped around the packet for physical delivery.
*(When the server receives the Frame, it strips these headers off one by one, reading it backward until it gets the original Application Data).*

##  Practical Action: Talking to Ports via Telnet
I learned I can use `telnet` to manually connect to open TCP ports and talk directly to the services running there.

* **Echo Server (Port 7):** Repeats whatever I type.
  ```bash
  telnet MACHINE_IP 7
  
  Daytime Server (Port 13): Spits out the current date and immediately closes the connection.
  telnet MACHINE_IP 13
  
  HTTP Web Server (Port 80): I can manually request a webpage without a browser!
  telnet MACHINE_IP 80
GET / HTTP/1.1
Host: telnet.thm
# (Hit Enter twice to send the blank line ending the request)
To exit a stuck telnet session, I hit CTRL + ], then type quit


# 2. Networking Essentials

# Network Essentials: DHCP, ARP, ICMP, Routing, and NAT

##  How I Automatically Get My Network Config: DHCP
Whenever I connect to a new WiFi network, I don't have to manually configure my IP address, Subnet Mask, Default Gateway, or DNS server. This magic is handled by **DHCP (Dynamic Host Configuration Protocol)**. 

DHCP operates at the application layer using **UDP port 67 (Server)** and **UDP port 68 (Client)**. The entire automated negotiation process is easily remembered using the acronym **DORA**:
* **[D]iscover:** My device broadcasts a message (`DHCPDISCOVER`) asking, "Are there any DHCP servers out here?" (Source IP: `0.0.0.0`, Destination: `255.255.255.255`).
* **[O]ffer:** The DHCP server replies (`DHCPOFFER`) with a proposed IP address.
* **[R]equest:** My device replies (`DHCPREQUEST`) saying, "I accept this IP."
* **[A]cknowledge:** The server confirms (`DHCPACK`) the lease is officially mine.

##  Bridging the Gap Between IP and MAC: ARP
I learned that while I use IP addresses (Layer 3) to target machines across the internet, devices on the *same local network* must use MAC addresses (Layer 2) to physically communicate over Ethernet or WiFi. 

**ARP (Address Resolution Protocol)** acts as the translator between these two layers. 
* If my device knows a target's IP but not its MAC address, it broadcasts an **ARP Request**: *"Who has 192.168.1.5? Tell 192.168.1.10."*
* The target machine responds with an **ARP Reply**, providing its MAC address directly to my machine. From then on, they can exchange Layer 2 frames.

##  Network Diagnostics: ICMP
**Internet Control Message Protocol (ICMP)** is the backbone of error reporting and network diagnostics. It doesn't use ports; it uses "Types".

### `ping`
This command tests connectivity to a target.
* My machine sends an **ICMP Echo Request (Type 8)**.
* If the target is alive and firewalls permit it, it responds with an **ICMP Echo Reply (Type 0)**.

### `traceroute` (or `tracert` on Windows)
This command maps the exact path of routers my packet takes to reach a destination. 
* **How it works:** It manipulates the **TTL (Time-to-Live)** field in the IP header. TTL represents the maximum number of routers ("hops") a packet can pass through. 
* `traceroute` starts by sending a packet with a TTL of 1. The first router drops it and sends back an **ICMP Time Exceeded (Type 11)** message, revealing its IP. The command increments the TTL to 2, 3, 4, etc., mapping every single router along the way until the target is reached!

##  The Pathfinders: Routing Protocols
The internet is a web of millions of routers. To figure out the fastest or most efficient path for my data, routers communicate with each other using routing protocols:
* **OSPF (Open Shortest Path First):** Routers share network maps and calculate the most efficient path based on link states.
* **EIGRP (Enhanced Interior Gateway Routing Protocol) : ** A proprietary Cisco protocol that factors in bandwidth and delay.
* **RIP (Routing Information Protocol):** A simpler protocol for small networks that just counts the number of "hops".
* **BGP (Border Gateway Protocol):** The massive protocol that actually runs the backbone of the Internet, routing data between different ISPs.

##  Saving IPv4: Network Address Translation (NAT)
Since there are only about 4 billion IPv4 addresses, we would have run out years ago if every device needed a public IP. **NAT** solves this.

Instead of my phone, laptop, and smart TV all having public IPs, my home router gets *one* public IP. All my internal devices use Private IPs (like `192.168.x.x`). 
When I browse the web, the NAT router intercepts my traffic, replaces my internal Private IP with its external Public IP, and assigns the connection a random external port number. It keeps a **Translation Table** so that when the web server replies, the router knows exactly which internal device to forward the data back to.

---

### Challenge Question Answered
> **Question:** Assuming that the router has infinite processing power, approximately speaking, how many thousand simultaneous TCP connections can it maintain?

**Answer:** **65**

**Explanation:** 
When a router performs NAT (specifically PAT - Port Address Translation), it maps internal connections to its single public IP address using unique **source port numbers**. Because TCP (and UDP) port numbers are 16-bit values, the maximum possible number of ports is 65,535 ($2^{16} - 1$, excluding port 0). Therefore, the router has a hard limit of approximately 65,000 available ports to assign to outgoing connections at any given time. Once all 65,000 ports are mapped in the NAT translation table, the router cannot establish any new TCP connections until some of the existing ones are closed and their ports are freed up.



# 3. Networking Core Protocols



  ## Domain Resolution & Registration
I use the Domain Name System (DNS, Layer 7) to map domain names to IPs. It uses UDP port 53 by default, with TCP 53 as a fallback. It relies on specific records:
* **A Record:** Maps a hostname to an IPv4 address.
* **AAAA Record:** Maps a hostname to an IPv6 address.
* **CNAME Record:** Maps a domain to another domain name.
* **MX Record:** Specifies the mail server for a domain.

I can query DNS records and check public domain registration (WHOIS) directly from the command line:
* **Query DNS records:** `nslookup www.example.com`
* **Analyze DNS packets:** `tshark -r dns-query.pcapng -Nn`
* **Check domain registration:** `whois example.com` (Note: Privacy services can mask registrant contact info).

---

## Web & File Transfer Protocols
**HTTP/HTTPS** (TCP 80/443, and less commonly 8080/8443) retrieves web elements. **FTP** (TCP 21) transfers files efficiently. Note that FTP listens for commands on port 21, but data transfer happens over a separate connection.

I can interact with these services manually to troubleshoot or transfer files:
* **Manual HTTP GET Request:** `telnet <ip> 80` then type `GET / HTTP/1.1` and `Host: anything`. (Other methods include POST, PUT, and DELETE).
* **Connect to FTP:** `ftp <ip>`
* **Common FTP Commands:** `USER` (e.g., anonymous), `PASS`, `ls` (list files), `type ascii` (switch to text mode), `RETR` or `get` (download), and `STOR` (upload).

---

## Email Protocols
Sending and receiving emails involves distinct protocols. I can manually test mail servers by connecting to their respective ports via `telnet`:

* **SMTP (Sending - TCP 25):** `telnet <ip> 25`
  * *Workflow:* `HELO` or `EHLO client.thm` > `MAIL FROM: <user@client.thm>` > `RCPT TO: <strategos@server.thm>` > `DATA` > type message > end with a single `.` > `QUIT`.
* **POP3 (Receiving - TCP 110):** `telnet <ip> 110` (Downloads and typically deletes from the remote server to save storage).
  * *Workflow:* `USER <username>` > `PASS <password>` > `STAT` (check size/messages) > `LIST` > `RETR <msg_number>` > `DELE <msg_number>` > `QUIT`.
* **IMAP (Syncing - TCP 143):** `telnet <ip> 143` (Keeps messages on the server, syncing read, moved, and deleted statuses across multiple devices).
  * *Workflow:* `A LOGIN <username> <password>` > `B SELECT inbox` > `C FETCH 3 body[]`. (Other useful commands: `MOVE` and `COPY`). `D LOGOUT`.

---

## Quick Reference Summary
| Protocol | Transport | Port |
| :--- | :--- | :--- |
| **FTP** | TCP | 21 |
| **TELNET** | TCP | 23 |
| **SMTP** | TCP | 25 |
| **DNS** | UDP / TCP | 53 |
| **HTTP** | TCP | 80 |
| **POP3** | TCP | 110 |
| **IMAP** | TCP | 143 |
| **HTTPS**| TCP | 443 |


# 4. Networking Secure Protocols


# Networking Secure Protocols: Protecting the Web

##  Why We Needed "S" (The Problem with Plaintext)
In the previous modules, I learned how core protocols like HTTP, POP3, and TELNET function. The massive, glaring problem with them is that they were designed without any security. They operate in **plaintext**, meaning any adversary sitting on the network (using a packet sniffer like Wireshark in promiscuous mode) can read every password, email, and credit card number sent over the wire. They also lack **integrity** (an attacker can alter the data in transit) and **authenticity** (I have no proof the server I'm talking to is who they claim to be).

To fix this, the internet needed a cryptographic blanket to wrap around these protocols.

##  SSL and TLS (Transport Layer Security)
**TLS** (the modern successor to SSL) operates at the Transport Layer. It provides the confidentiality, integrity, and authenticity that early protocols lacked. 

### How it Works (Briefly):
1. A server generates a cryptographic public/private key pair.
2. The server submits a **CSR (Certificate Signing Request)** to a trusted **Certificate Authority (CA)**.
3. The CA verifies the server's identity and issues a digitally signed TLS certificate.
4. When my browser connects to the server, the server presents this certificate. Because my browser trusts the CA, it trusts the server. 
*(Note: Self-signed certificates exist, but browsers will warn users that no third-party CA has verified the identity).*

Once TLS is negotiated, all data sent between the client and server is completely encrypted. Without the private key, an attacker sniffing the traffic only sees gibberish.

##  Securing Core Protocols
Adding TLS to a protocol usually involves shifting it to a new port number and appending an "S" (for Secure) to the name.

### HTTPS (HTTP over TLS)
When making an HTTPS request, the browser first completes the standard TCP 3-way handshake. Then, it initiates a **TLS handshake** to negotiate encryption. Only after this is complete does the actual HTTP `GET` request get sent (hidden entirely inside the encrypted TLS tunnel).

### Master Port Number Cheat Sheet (Secure vs. Insecure)
I need to memorize how the core ports shift when TLS is applied:

| Protocol | Purpose | Insecure Port | Secure Protocol | Secure Port |
| :--- | :--- | :--- | :--- | :--- |
| **HTTP** | Web Browsing | 80 | **HTTPS** | 443 |
| **SMTP** | Sending Email | 25 | **SMTPS** | 465 (or 587) |
| **POP3** | Downloading Email | 110 | **POP3S** | 995 |
| **IMAP** | Syncing Email | 143 | **IMAPS** | 993 |

---

##  SSH and SFTP (Killing TELNET and FTP)

### SSH (Secure Shell)
TELNET (Port 23) was the original way to remotely access systems, but it sent entire sessions—including passwords—in cleartext. **SSH (Port 22)** was created to replace it. 
* It provides strong authentication (passwords, public/private keys, or 2FA).
* It provides full end-to-end encryption.
* It supports **X11 Forwarding**, allowing me to securely run graphical GUI applications from a remote Linux machine on my local screen.

### SFTP (SSH File Transfer Protocol)
FTP (Port 21) is also insecure. While FTPS (FTP over TLS on Port 990) exists, it is notoriously difficult to configure through firewalls because it requires separate control and data channels. 
**SFTP** solves this. It is a completely different protocol built directly into SSH. It runs over the same encrypted **Port 22** as SSH, making secure file transfers incredibly easy to set up and manage.

---

##  Virtual Private Networks (VPNs)
The internet was built to deliver packets reliably, not privately. A **VPN** fixes this by creating a secure, encrypted "tunnel" over the public internet.

* **Site-to-Site VPN:** Connects two physical locations (like a branch office to a headquarters) so all devices can securely share resources as if they were in the same building.
* **Remote Access VPN:** Connects a single user (like me sitting in a coffee shop) to a private corporate network.

When I connect to a commercial VPN to browse the web, my local ISP can only see an encrypted tunnel connecting to the VPN server's IP. Furthermore, the websites I visit see the VPN server's IP address rather than my real one, which is why VPNs can bypass geographical content restrictions.

---

##  Practical Lab: Decrypting TLS Traffic in Wireshark
I ran an incredible practical exercise in the lab to prove how TLS works. 
By default, Wireshark cannot read HTTPS traffic. However, browsers like Chromium can be forced to log the session's TLS encryption keys to a file using the flag:
`chromium --ssl-key-log-file=~/ssl-key.log`

By opening a `.pcapng` capture file in Wireshark and loading that exact `ssl-key.log` file into Wireshark's **Transport Layer Security Preferences**, Wireshark used the keys to decrypt the session on the fly. 
What was once total gibberish instantly turned back into cleartext HTTP `POST` requests, revealing the user's plaintext login credentials hiding inside the packet!


# 5. Wireshark: The Basics

Wireshark is an open-source, cross-platform network packet analyser tool capable of sniffing and investigating live traffic and inspecting packet captures (PCAP). It is commonly used as one of the best packet analysis tools. In this room, we will look at the basics of Wireshark and use it to perform fundamental packet analysis.

## Learning Objectives
* Navigate and configure Wireshark
* Inspect packets and discover information from the different layers of TCP/IP
* Apply display filters

## Prerequisites
* Networking Module

## Environment Setup
Press the **Start Lab Machine** button to start the lab machine. The machine will start in Split-Screen view. If it is not visible, use the blue **Show Split View** button at the top of the page.

There are two capture files given in the VM. You can use the `http1.pcapng` file to simulate the actions shown in the screenshots. Please note that you need to use the `Exercise.pcapng` file to answer the questions.

---

## Use Cases
Wireshark is one of the most potent traffic analyser tools available in the wild. There are multiple purposes for its use:
* Detecting and troubleshooting network problems, such as network load failure points and congestion.
* Detecting security anomalies, such as rogue hosts, abnormal port usage, and suspicious traffic.
* Investigating and learning protocol details, such as response codes and payload data.

> **Note:** Wireshark is not an Intrusion Detection System (IDS). It only allows analysts to discover and investigate the packets in depth. It also doesn't modify packets; it reads them. Hence, detecting any anomaly or network problem highly relies on the analyst's knowledge and investigation skills.

---

## GUI and Data
Wireshark GUI opens with a single all-in-one page, which helps users investigate the traffic in multiple ways. At first glance, five sections stand out.

| Section | Description |
| :--- | :--- |
| **Toolbar** | The main toolbar contains multiple menus and shortcuts for packet sniffing and processing, including filtering, sorting, summarising, exporting and merging. |
| **Display Filter Bar** | The main query and filtering section. |
| **Recent Files** | List of the recently investigated files. You can recall listed files with a double-click. |
| **Capture Filter and Interfaces** | Capture filters and available sniffing points (network interfaces). The network interface is the connection point between a computer and a network. The software connection (e.g., `lo`, `eth0` and `ens33`) enables networking hardware. |
| **Status Bar** | Tool status, profile and numeric packet information. |

*(The picture below shows Wireshark's main window. The sections explained in the table are highlighted. Now open Wireshark and follow along with the walkthrough.)*

---

## Loading PCAP Files
The empty interface only displays recently processed files. You can load a file by using the **File** menu, dragging and dropping the file, or double-clicking on the file to load a pcap.

Now, we can see the processed filename, detailed number of packets, and packet details. Packet details are shown in three different panes, which allow us to discover them in different formats. 

| Pane | Description |
| :--- | :--- |
| **Packet List Pane** | Summary of each packet (source and destination addresses, protocol, and packet info). You can click on the list to choose a packet for further investigation. Once you select a packet, the details will appear in the other panels. |
| **Packet Details Panel** | Detailed protocol breakdown of the selected packet. |
| **Packet Bytes Pane** | Hex and decoded ASCII representation of the selected packet. It highlights the packet field depending on the clicked section in the details pane. |

---

## Colouring Packets
Along with quick packet information, Wireshark also colours packets in order of different conditions and the protocol to spot anomalies and protocols in captures quickly (this explains why almost everything is green in typical HTTP captures). This glance at packet information can help track down exactly what you're looking for during analysis. You can create custom colour rules to spot events of interest by using display filters. 

Wireshark has two types of packet colouring methods: 
1. **Temporary rules:** Only available during a program session. Done with the "right-click menu" or **View --> Conversation Filter** menu.
2. **Permanent rules:** Saved under the preference file (profile) and available for the next program session. You can use the "right-click menu" or **View --> Coloring Rules** menu to create permanent colouring rules. 

The **Colourise Packet List** menu activates/deactivates the colouring rules.

---

## Traffic Sniffing
You can use the blue **shark button** to start network sniffing (capturing traffic), the red button will stop the sniffing, and the green button will restart the sniffing process. The status bar will also provide the used sniffing interface and the number of collected packets.

---

## Merge PCAP Files
Wireshark can combine two pcap files into one single file. You can use the **File --> Merge** menu path to merge a pcap with the processed one. When you choose the second file, Wireshark will show the total number of packets in the selected file. Once you click "open", it will merge the existing pcap file with the chosen one and create a new pcap file. Note that you need to save the "merged" pcap file before working on it. *(See GIF)*

---

## View File Details
Knowing the file details is helpful. Especially when working with multiple pcap files, sometimes you will need to know and recall the file details (File hash, capture time, capture file comments, interface and statistics) to identify the file, classify and prioritise it. You can view the details by following **Statistics --> Capture File Properties** or by clicking the **pcap icon located on the left bottom**.

---

## Packet Dissection
Packet dissection is also known as protocol dissection, which investigates packet details by decoding available protocols and fields. Wireshark supports a long list of protocols for dissection, and you can also write your dissection scripts. 

> **Note:** This section covers how Wireshark uses OSI layers to break down packets and how to use these layers for analysis. It is expected that you already have background knowledge of the OSI model and how it works. 

### Packet Details
You can click on a packet in the packet list pane to open its details (double-click will open details in a new window). Packets consist of 5 to 7 layers based on the OSI model. We will go over all of them in an HTTP packet from a sample capture. 

Each time you click a detail, it will highlight the corresponding part in the packet bytes pane. We can see seven distinct layers to the packet: frame/packet, source [MAC], source [IP], protocol, protocol errors, application protocol, and application data.

* **The Frame (Layer 1):** This will show you what frame/packet you are looking at and details specific to the Physical layer of the OSI model.
* **Source [MAC] (Layer 2):** This will show you the source and destination MAC Addresses; from the Data Link layer of the OSI model.
* **Source [IP] (Layer 3):** This will show you the source and destination IPv4 Addresses; from the Network layer of the OSI model.
* **Protocol (Layer 4):** This will show you details of the protocol used (UDP/TCP) and source and destination ports; from the Transport layer of the OSI model.
* **Protocol Errors:** This continuation of the 4th layer shows specific segments from TCP that needed to be reassembled.
* **Application Protocol (Layer 5):** This will show details specific to the protocol used, such as HTTP, FTP, and SMB. From the Application layer of the OSI model.
* **Application Data:** This extension of the 5th layer can show the application-specific data.

Wireshark calculates the number of investigated packets and assigns a unique number for each packet. This helps the analysis process for big captures and makes it easy to go back to a specific point of an event.

---

## Go to Packet
Packet numbers do not only help to count the total number of packets or make it easier to find/investigate specific packets. This feature not only navigates between packets up and down; it also provides in-frame packet tracking and finds the next packet in the particular part of the conversation. You can use the **Go** menu and toolbar to view specific packets.

---

## Find Packets
Apart from packet number, Wireshark can find packets by packet content. You can use the **Edit --> Find Packet** menu to make a search inside the packets for a particular event of interest. This helps analysts and administrators to find specific intrusion patterns or failure traces.

There are two crucial points in finding packets:
1. **Knowing the input type:** This functionality accepts four types of inputs (Display filter, Hex, String and Regex). String and regex searches are the most commonly used search types. Searches are case insensitive, but you can set the case sensitivity in your search by clicking the radio button.
2. **Choosing the search field:** You can conduct searches in the three panes (packet list, packet details, and packet bytes). If you try to find information available in the packet details pane and conduct the search in the packet list pane, Wireshark won't find it even if it exists.

---

## Mark Packets
Marking packets is another helpful functionality for analysts. You can find/point to a specific packet for further investigation by marking it. It helps analysts point to an event of interest or export particular packets from the capture. You can use the **Edit** or the **right-click** menu to mark/unmark packets.

Marked packets will be shown in black regardless of the original colour representing the connection type. Note that marked packet information is renewed every file session, so marked packets will be lost after closing the capture file.

---

## Packet Comments
Similar to packet marking, commenting is another helpful feature for analysts. You can add comments for particular packets that will help the further investigation or remind and point out important/suspicious points for other layer analysts. Unlike packet marking, the comments can stay within the capture file until the operator removes them.

---

## Export Packets
Capture files can contain thousands of packets in a single file. As mentioned earlier, Wireshark is not an IDS, so sometimes, it is necessary to separate specific packages from the file and dig deeper to resolve an incident. This functionality helps analysts share only the suspicious packages (decided scope). Thus redundant information is not included in the analysis process. You can use the **File** menu to export packets.

---

## Export Objects (Files)
Wireshark can extract files transferred through the wire. For a security analyst, it is vital to discover shared files and save them for further investigation. Exporting objects are available only for selected protocol's streams (DICOM, HTTP, IMF, SMB and TFTP).

---

## Time Display Format
Wireshark lists the packets as they are captured, so investigating the default flow is not always the best option. By default, Wireshark shows the time in "Seconds Since Beginning of Capture", the common usage is using the UTC Time Display Format for a better view. You can use the **View --> Time Display Format** menu to change the time display format.

---

## Expert Info
Wireshark also detects specific states of protocols to help analysts easily spot possible anomalies and problems. Note that these are only suggestions, and there is always a chance of having false positives/negatives. Expert info can provide a group of categories in three different severities:

| Severity | Colour | Info |
| :--- | :--- | :--- |
| **Chat** | Blue | Information on usual workflow. |
| **Note** | Cyan | Notable events like application error codes. |
| **Warn** | Yellow | Warnings like unusual error codes or problem statements. |
| **Error** | Red | Problems like malformed packets. |

Frequently encountered information groups are listed below:

| Group | Info |
| :--- | :--- |
| **Checksum** | Checksum errors |
| **Deprecated** | Deprecated protocol usage |
| **Comment** | Packet comment detection |
| **Malformed** | Malformed packet detection |

You can use the **lower left bottom section** in the status bar or **Analyse --> Expert Information** menu to view all available information entries via a dialogue box. It will show the packet number, summary, group protocol and total occurrence.

---

## Filters
Wireshark has a powerful filter engine that helps analysts to narrow down the traffic and focus on the event of interest. Wireshark has two types of filtering approaches: 
1. **Capture filters:** Used for "capturing" only the packets valid for the used filter. 
2. **Display filters:** Used for "viewing" the packets valid for the used filter. 

There is a golden rule for analysts who don't want to write queries for basic tasks: *"If you can click on it, you can filter and copy it"*.

### Apply as Filter
This is the most basic way of filtering traffic. While investigating a capture file, you can click on the field you want to filter and use the **right-click menu** or **Analyse --> Apply as Filter** menu to filter the specific value. Note that the number of total and displayed packets are always shown on the status bar.

### Conversation Filter
When you use the "Apply as a Filter" option, you will filter only a single entity of the packet. However, if you want to investigate a specific packet number and all linked packets by focusing on IP addresses and port numbers, the "Conversation Filter" option helps you view only the related packets. You can use the **right-click menu** or **Analyse --> Conversation Filter** menu to filter conversations.

### Colourise Conversation
This option is similar to the "Conversation Filter" with one difference. It highlights the linked packets without applying a display filter and decreasing the number of viewed packets. You can use the **right-click menu** or **View --> Colourise Conversation** menu to colourise a linked packet in a single click. (Use **View --> Colourise Conversation --> Reset Colourisation** to undo).

### Prepare as Filter
Similar to "Apply as Filter", this option helps analysts create display filters using the "right-click" menu. However, this model doesn't apply the filters immediately. It adds the required query to the pane and waits for the execution command (enter) or another chosen filtering option by using the ".. and/or.." logic.

### Apply as Column
You can use the **right-click menu** or **Analyse --> Apply as Column** menu to add columns to the packet list pane. Once you click on a value and apply it as a column, it will be visible on the packet list pane. This helps analysts examine the appearance of a specific value/field across the available packets.

### Follow Stream
Following the protocol streams helps analysts recreate the application-level data and understand the event of interest. It is possible to reconstruct streams to view unencrypted protocol data like usernames, passwords and other transferred data.

You can use the **right-click menu** or **Analyse --> Follow TCP/UDP/HTTP Stream** menu to follow traffic streams. Packets originating from the server are highlighted in blue, and those originating from the client are highlighted in red. Once you follow a stream, Wireshark automatically creates and applies the required filter. You will need to use the "X button" located on the right side of the display filter bar to remove it.

---

## Simple Display Filter Queries
The easiest way to filter quickly the huge amount of packets is by applying a display filter using the "Apply a display filter" bar. 

### Filter By Protocol Name or Port
There are two basic ways to filter based on a specific protocol:
1. **By protocol name:** Simply type in the protocol name (e.g., `http`, `arp`, `dhcp`, `ftp`, `smtp`, `pop`, `imap`) and hit enter. 
2. **By protocol port number:** Use the structure `tcp.port == <port number>` or `udp.port == <port number>`. For example, to see only HTTP packets, you would use the filter `tcp.port == 80`.

### Filter By IP
To filter for a specific IP, you can use the structure `ip.addr == <IP address>`. If you need to search for the IP 192.168.1.2, your filter would be `ip.addr == 192.168.1.2`


# 6. Tcpdump: The Basics

