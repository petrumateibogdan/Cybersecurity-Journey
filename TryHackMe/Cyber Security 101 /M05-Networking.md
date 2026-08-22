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



##  What I Learned About Wireshark
I learned that Wireshark is an open-source network packet analyzer used to sniff live traffic and inspect `.pcap` files. 
* **Crucial distinction:** It is **not** an Intrusion Detection System (IDS). It does not flag alerts or modify packets; it only reads them. Detecting anomalies relies entirely on my own investigation skills.

##  The GUI & Loading PCAPs
The main interface is split into five key sections:
1. **Toolbar:** Shortcuts for sniffing, filtering, and exporting.
2. **Display Filter Bar:** The main query bar for filtering traffic.
3. **Recent Files:** Quick access to recently opened PCAPs.
4. **Capture Filter and Interfaces:** Where I select the network interface (e.g., `eth0`, `wlan0`) to start sniffing.
5. **Status Bar:** Shows tool status and packet counts.

When I open a `.pcap` file, the data is split into three highly detailed panes:
* **Packet List Pane:** A summary of each packet (Source, Destination, Protocol). Clicking one loads it into the other panes.
* **Packet Details Pane:** The deep-dive protocol breakdown (this directly maps to the OSI layers).
* **Packet Bytes Pane:** The raw Hex and ASCII data. Highlighting a field in the Details pane highlights the exact hex bytes here.

##  Core Sniffing & File Management
* **Sniffing:** I use the blue "shark fin" button to start capturing, the red square to stop, and the green button to restart.
* **Merging PCAPs:** If I have multiple captures, I can combine them into one file via `File --> Merge`.
* **File Details:** I can check the file hash, capture time, and interface statistics via `Statistics --> Capture File Properties`.
* **Time Format:** By default, Wireshark shows "Seconds Since Beginning of Capture". For real-world analysis, I should always change this to **UTC** (`View --> Time Display Format`).

##  Packet Dissection & The OSI Model
I found it really helpful how the **Packet Details Pane** breaks down the packet exactly according to the OSI model. When I inspect an HTTP packet, I see:
1. **Frame (Layer 1):** Physical layer details (wire/interface info).
2. **Source [MAC] (Layer 2):** Data Link layer (Source & Destination MAC addresses).
3. **Source [IP] (Layer 3):** Network layer (IPv4/IPv6 addresses).
4. **Protocol (Layer 4):** Transport layer (TCP/UDP and Port numbers).
5. **Protocol Errors:** Reassembled TCP segments.
6. **Application Protocol (Layer 5+):** Details for HTTP, FTP, SMB, etc.
7. **Application Data:** The actual payload/content.

##  Navigation, Searching & Extracting
When dealing with massive PCAP files containing thousands of packets, I learned a few tricks to manage the noise:
* **Find Packets (`CTRL+F`):** I can search using Display Filters, Hex, String, or Regex. *Crucial note:* I must select the correct pane (List, Details, or Bytes) before searching, otherwise Wireshark won't find the data!
* **Marking:** I can right-click to "Mark" a packet. It turns black, making it easy to spot. This is **temporary** and vanishes when I close the file.
* **Commenting:** I can add written comments to packets. Unlike marking, these are **saved permanently** inside the PCAP file.
* **Exporting Packets:** I can isolate and save specific suspicious packets into a new, smaller PCAP file (`File --> Export Specified Packets`).
* **Exporting Objects:** If a file was transferred over the network, I can extract and save it! (`File --> Export Objects`). This works for HTTP, SMB, TFTP, IMF, and DICOM.

##  Packet Coloring & Expert Info
Wireshark color-codes traffic so I can spot protocols and anomalies instantly.
* **Coloring Rules:** I can set temporary rules for my current session, or permanent rules via `View --> Coloring Rules`.

**Expert Info:** Wireshark automatically flags suspicious protocol states (like checksum errors, deprecated protocols, or malformed packets). I can view these via `Analyze --> Expert Information`. 

**Expert Info Severities:**
| Severity | Color | What it means |
| :--- | :--- | :--- |
| **Chat** | Blue | Normal workflow. |
| **Note** | Cyan | Notable events (e.g., application error codes). |
| **Warn** | Yellow | Warnings like unusual error codes. |
| **Error** | Red | Major problems (e.g., malformed packets). |

##  Traffic Filtering (The Golden Rule)
The golden rule for Wireshark analysis is: **"If you can click on it, you can filter and copy it."** 

There are two main types of filters:
1. **Capture Filters:** Applied *before* capturing to only record specific traffic.
2. **Display Filters:** Applied *after* capturing to hide noise and view specific traffic.

### Quick GUI Filtering Tools:
* **Apply as Filter:** Right-click any field and instantly apply it as a display filter.
* **Prepare as Filter:** Builds the query in the search bar but waits for me to hit Enter (great for building complex `AND`/`OR` queries).
* **Conversation Filter:** Isolates a specific communication stream between two IP/Port pairs.
* **Colorize Conversation:** Highlights the linked conversation packets without filtering out the rest of the traffic.
* **Apply as Column:** Takes a deep protocol field and adds it as a visible column in the Packet List Pane for quick scanning.

### Reconstructing Data (Follow Stream)
If I want to read the raw application data (like intercepted passwords or raw HTML), I right-click a packet and select **Follow TCP/UDP/HTTP Stream**. 
* **Red text** = Data originating from the Client.
* **Blue text** = Data originating from the Server.
*(Note: Following a stream auto-applies a filter. I have to click the 'X' in the filter bar to see all traffic again).*

##  Essential Filter Syntax Cheat Sheet
To filter traffic manually using the Display Filter Bar:
* **By Protocol:** Just type the name (e.g., `http`, `arp`, `dhcp`, `ftp`, `smtp`, `dns`).
* **By TCP Port:** `tcp.port == 80`
* **By UDP Port:** `udp.port == 53`
* **By IP Address:** `ip.addr == 192.168.1.2`

# 6. Tcpdump: The Basics

# Network Traffic Analysis with Tcpdump

I just completed the Tcpdump room, which focuses on capturing and analyzing network traffic from the command line. While GUIs like Wireshark are great, `tcpdump` (built on the `libpcap` library for Unix and `winpcap` for Windows) is incredibly stable, fast, and essential for operating in headless environments.

Here are my complete notes and command references for my GitHub repository.

## 1. Basic Packet Capture Operations
To actually capture packets, I need to specify exactly what to listen to, where to write the data, and how to read it later.

*   **Specify the Interface:** I use `-i` to tell tcpdump which network card to listen on.
    *   `tcpdump -i any` (Listens on all available interfaces)
    *   `tcpdump -i eth0` (Listens strictly on the `eth0` interface)
    *   *(Note: I can use `ip a s` to list my available interfaces).*
*   **Save to a File:** I use `-w` to write the output to a `.pcap` file so I can analyze it later (or open it in Wireshark).
    *   `tcpdump -i eth0 -w capture.pcap` *(Note: This suppresses screen output while writing).*
*   **Read from a File:** I use `-r` to read packets from a saved `.pcap` file.
    *   `tcpdump -r capture.pcap`
*   **Limit Packet Count:** I use `-c` to stop capturing after a specific number of packets, so it doesn't run endlessly.
    *   `tcpdump -i eth0 -c 100` (Captures exactly 100 packets and stops).

## 2. Modifying Output and Verbosity
By default, tcpdump tries to resolve IP addresses to domain names and port numbers to service names (like changing port 80 to `http`). This slows things down and clutters the output.

*   **`-n`**: Do not resolve IP addresses. (Prints `93.184.215.14` instead of `example.com`).
*   **`-nn`**: Do not resolve IP addresses OR port numbers. (Prints port `80` instead of `http`).
*   **`-v`**: Verbose output (adds TTL, identification, total length, and IP options).
*   **`-vv`** or **`-vvv`**: Even more verbosity.

## 3. Basic Packet Filtering
Listening to a whole network is too noisy. I need to filter the traffic to find exactly what I'm looking for.

### Filtering by Host
I can filter traffic going to or coming from a specific machine.
*   `tcpdump host 192.168.1.10` (Any traffic involving this IP)
*   `tcpdump src host 192.168.1.10` (Only traffic *originating* from this IP)
*   `tcpdump dst host 192.168.1.10` (Only traffic *destined* to this IP)

### Filtering by Port
I can filter traffic targeting specific services (like DNS on port 53).
*   `tcpdump port 53` (Any traffic on port 53)
*   `tcpdump src port 80` (Traffic sent from port 80)
*   `tcpdump dst port 443` (Traffic sent to port 443)

### Filtering by Protocol
I can restrict the capture to a specific protocol by just typing its name:
*   Protocols: `ip`, `ip6`, `udp`, `tcp`, `icmp`, `arp`.
*   Example: `tcpdump -i eth0 icmp` (Captures only ping requests/replies).

### Logical Operators
I can combine filters using logical operators:
*   `and`: Both conditions must be true (e.g., `tcpdump host 1.1.1.1 and tcp`).
*   `or`: Either condition can be true (e.g., `tcpdump udp or icmp`).
*   `not`: The condition must be false (e.g., `tcpdump not tcp` will capture UDP, ICMP, ARP, etc.).

## 4. Advanced Filtering (Header Bytes & TCP Flags)
For deep packet inspection, I can filter based on packet size or even specific bits inside the protocol headers.

### Filtering by Size
*   `greater 15000`: Captures packets larger than or equal to 15000 bytes.
*   `less 64`: Captures packets smaller than or equal to 64 bytes.

### Filtering by Header Bytes
Tcpdump allows me to look at specific bytes within a protocol header using the syntax: `proto[expr:size]`
*   `proto`: The protocol (e.g., `ether`, `ip`, `tcp`, `udp`).
*   `expr`: The byte offset (starting at 0).
*   `size`: Number of bytes to look at (1, 2, or 4).

**Binary Bitwise Operations:**
To isolate specific bits, I use binary operators:
*   `&` (AND): Returns 1 only if both bits are 1.
*   `|` (OR): Returns 1 if either bit is 1.
*   `!` (NOT): Inverts the bit.

*Example:* `ether[0] & 1 != 0` isolates the first byte of the Ethernet header to check if the packet was sent to a multicast address.

### Filtering by TCP Flags
Instead of manually calculating byte offsets, tcpdump has built-in keywords for TCP flags:
*   `tcp-syn` (Synchronize)
*   `tcp-ack` (Acknowledge)
*   `tcp-fin` (Finish)
*   `tcp-rst` (Reset)
*   `tcp-push` (Push)

**Syntax Examples:**
*   Capture packets with *only* the SYN flag set: 
    `tcpdump "tcp[tcpflags] == tcp-syn"`
*   Capture packets with *at least* the SYN flag set (even if other flags are set): 
    `tcpdump "tcp[tcpflags] & tcp-syn != 0"`
*   Capture packets with *either* the SYN or ACK flags set: 
    `tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"`

## 5. Packet Formatting and Display
Sometimes I need to see the actual raw data inside the packets, not just the headers.

*   **`-q` (Quick output):** Prints very brief packet information (shorter output lines).
*   **`-e` (Ethernet/Link-level header):** Prints the MAC addresses of the source and destination. Essential for tracing Layer 2 interactions like ARP and DHCP.
*   **`-A` (ASCII):** Prints the payload of the packet in ASCII text. Great for reading unencrypted HTTP traffic or plaintext passwords.
*   **`-xx` (Hexadecimal):** Prints the packet payload in hex format. Necessary if the data is encrypted, compressed, or non-English.
*   **`-X` (Hex and ASCII):** The best of both worlds. Prints the data side-by-side in both Hex and ASCII formats.

---

## 6. Practical Lab Commands (TryHackMe Exercises)
Here are the exact commands I built to solve the lab questions:

**1. Count how many packets in `traffic.pcap` use the ICMP protocol:**
```bash
tcpdump -r traffic.pcap icmp | wc -l


 Find the IP address of the host that asked for the MAC address of 192.168.124.137:
tcpdump -r traffic.pcap arp

 Find the hostname (subdomain) in the first DNS query:
tcpdump -r traffic.pcap port 53 -c 1


Count how many packets have only the TCP Reset (RST) flag set:
tcpdump -r traffic.pcap "tcp[tcpflags] == tcp-rst" | wc -l

Find the IP address of the host that sent packets larger than 15000 bytes:
tcpdump -r traffic.pcap greater 15000 -n

Find the MAC address of the host that sent an ARP request:
tcpdump -er traffic.pcap ar
