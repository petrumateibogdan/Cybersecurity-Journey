# Module 5: Networking

# 1. Networking Concepts

##  The OSI Model (The Theoretical Blueprint)
To understand how devices actually talk to each other, I first needed to grasp the **OSI (Open Systems Interconnection) Model**. It is a 7-layer theoretical framework. A great mnemonic to remember the layers from bottom to top (Layer 1 to Layer 7) is: **"Please Do Not Throw Spinach Pizza Away"**.

* **Layer 1: Physical Layer** - The actual cables (Ethernet, fiber optic) and radio waves (WiFi). Data is just 1s and 0s here.
* **Layer 2: Data Link Layer** - Communication on the *same* local network segment. Devices use **MAC addresses** (Media Access Control) to identify each other. 
* **Layer 3: Network Layer** - Communication *between* different networks. This is where routing and **IP Addresses** live (e.g., IPv4, ICMP, IPSec).
* **Layer 4: Transport Layer** - End-to-end communication between applications. This is where **TCP** and **UDP** port numbers are assigned.
* **Layer 5: Session Layer** - Establishes, maintains, and kills sessions between applications (e.g., NFS, RPC).
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

## 4. Transport Protocols: UDP vs. TCP
At Layer 4, the network uses port numbers (ranging from 1 to 65535) to figure out which exact application should receive the data. 

* **UDP (User Datagram Protocol):** Connectionless and fast. It just fires data at the target and doesn't care if it actually arrives. Think of it like standard snail mail with no tracking. Great for video streaming or gaming where speed beats reliability.
* **TCP (Transmission Control Protocol):** Connection-oriented and highly reliable. It guarantees delivery by acknowledging received data.
  * **The TCP 3-Way Handshake:** Before sending data, TCP establishes a connection:
    1. **SYN:** Client says "Hello, I want to connect."
    2. **SYN-ACK:** Server says "I hear you, let's connect."
    3. **ACK:** Client says "Great, acknowledging your reply."

## 5. Encapsulation (The Life of a Packet)
When I send a message, it travels down the OSI layers, getting "wrapped" in new headers at each step. This is **Encapsulation**.
1. **Application Data:** I type a search query.
2. **Segment (Transport Layer):** A TCP header (with source/dest ports) is added to the data.
3. **Packet (Network Layer):** An IP header (with source/dest IP addresses) is wrapped around the segment.
4. **Frame (Data Link Layer):** A MAC header and trailer are wrapped around the packet for physical delivery.
*(When the server receives the Frame, it strips these headers off one by one, reading it backward until it gets the original Application Data).*

## 6. Practical Action: Talking to Ports via Telnet
I learned I can use `telnet` to manually connect to open TCP ports and talk directly to the services running there.

* **Echo Server (Port 7):** Repeats whatever I type.
  ```bash
  telnet MACHINE_IP 7
  telnet MACHINE_IP 13
  telnet MACHINE_IP 80
GET / HTTP/1.1
Host: telnet.thm
# (Hit Enter twice to send the blank line ending the request)
To exit a stuck telnet session, I hit CTRL + ], then type quit
  
