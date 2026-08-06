## Module 2: Computer Fundamentals

### 1. Inside a Computer System

* **PC Components & Analogies:** 
  I learned the basics about each and every PC component in a computer system and their analogies with the human body: 
  * CPU with the brain
  * Graphics card with visual cortex
  * RAM with short memory
  * HDD and SSD with long memory
  * PSU with the heart
  * Motherboard with skeleton and nerves, etc. 

* **Practical Work:** 
  I studied the structure of the motherboard and placed virtually every component in its place.

* **Boot Sequence (Step-by-Step):** 
  Starting with pressing the power button, firmware starts (older **BIOS** (Basic Input/Output System) and **UEFI** (Unified Extensible Firmware Interface)), **POST** (Power-On Self Test), select boot device from the SSD or HDD, and then start the bootloader (OS into RAM -> UEFI gives control of the components to the OS).


### 2. Computer Types

* I learnt that there are multiple types of computers out there: **Laptops, Desktops, Workstations, Servers, Smartphones, Tablets, Embedded computers, IoT devices**.
* I also learnt about **redundant power reducing a single failure point**.
* I learnt about **thermal throttling**, a **laptop having a limited one because of the tight physical space, whereas a desktop has an efficient cooling system**.


### 3. Client-Server Basics

This was an interesting topic where I learnt about: 

* **Client and Server:** client always initiates the request via a browser requesting a webpage, and the server gives what what the client was looking for
* **Request and response:** clients send requests and servers responses. If it's unavailable an error response is returned.
* **Protocol:** How does a client and a server communicate(commands,language,structure,syntax,lingo)
* **Port:** Used to identify a specific service running on a system, similar to using different doors for different services( takeaway,delivery,restaurant)
* **DNS Domain Name Service):** Like GPS, website to server's location. Server's location is IP (Internet Protocol). So DNS corresponds to an IP address

* **HTTP methods:** GET,POST,PUT,DELETE,PATCH.HEAD,OPTIONS,CONNECT,TRACE (defined in RFC documents, Request for Comments)
* HTTP uses stateless traffic but it keeps track of past requests via cookies,tokens.
* WITH **GET** method you retrieve resources from web servers via a browser. (status code: '200 OK')

* **HTTP(S) = Hypertext Transfer Protocol(Secure)** = client server protocol used for the World Wide Web where requests are processed independently
* In the browser, **F12(inspect)** = opens Developer Tools to inspect,debug and analyze web pages.
* On the network tab I saw multiple GET requests. (200 OK and 404) and by clicking each one I saw: Scheme(which protocol : HTTP/ HTTPS),Host,Filename('/'=index.html),Address(IP address),Status(200 OK).

### 4. Virtualisation Basics

In this section, I learnt why virtualization is such a critical foundation in modern IT, both for maximizing hardware efficiency and for safely isolating environments.

### Key Terminology I Learnt:
* **Virtualization:** Enables a single physical computer to act like multiple separate computers at the same time.
* **Hypervisor:** The "manager" software that actually makes and runs these virtual computers.
* **Lab Machine (VM):** A complete virtual computer running inside a real one, with its own dedicated operating system.
* **Container:** A small, isolated box built for a single app that shares the same system as the host instead of needing a full OS.
* **Container Images:** Pre-packed recipes or templates used to create containers.
* **Network Ports:** Special numbered entry points that apps use to communicate over the network.

### The Key Benefits of Virtualization:
I also learnt that using virtualization provides massive advantages, including:
* Cost savings and much better resource usage
* A safe, isolated environment for cyber security testing
* Faster deployment and centralized management
* Scalability, flexibility, and portability

### 5. Cloud Computing Fundamentals


In this section, I learnt about the core concepts of cloud computing, how different organizations deploy it, and the different ways to consume cloud services.

### Cloud Benefits & Characteristics
* **Scalability:** The ability to easily grow or shrink resources.
* **On-demand self-service:** Getting resources exactly when needed without waiting.
* **Pay only for what you use:** Cost-efficient billing.
* **Security:** Built-in protection for data and infrastructure.
* **High availability:** Ensuring services stay online and accessible.
* **Global access:** Connecting from anywhere in the world.

### Types of Cloud Deployments
* **Public Cloud:** The most commonly used type (great for startups and general websites).
* **Private Cloud:** Highly secure, dedicated environments (used by banks, healthcare, and governments).
* **Hybrid Cloud:** A mix of both (often used by e-commerce platforms).

### Ways to Use Cloud Services (Service Models)
* **IaaS (Infrastructure as a Service):** Like an empty house. You have to choose the furniture, install the appliances, and handle the maintenance inside.
* **PaaS (Platform as a Service):** The basics are already set up. You can just move in and focus on living (building your apps).
* **SaaS (Software as a Service):** Everything is fully furnished, managed for you, and ready to use.

### Major Cloud Vendors
* **AWS (Amazon Web Services):** The current industry leader.
* **Other major players:** GCP (Google Cloud Platform), Alibaba Cloud, IBM Cloud, and Oracle Cloud.

### Basic Cloud Terminology
* **EC2 (Elastic Compute Cloud):** A virtual computer or server in the cloud.
* **Instance types (e.g., t2, t3, m5):** These dictate how powerful the virtual computer is.
