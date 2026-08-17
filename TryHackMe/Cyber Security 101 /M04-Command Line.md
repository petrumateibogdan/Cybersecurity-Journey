# Module 4: Command Line

## 1. Windows Command Line



## My Thoughts on GUI vs. CLI
It is easy to rely on a Graphical User Interface (GUI) because it is so intuitive—you can usually just poke around and figure out a new interface by clicking. However, I am realizing that mastering the Command-Line Interface (CLI) is an absolute necessity. 

While the CLI has a steeper learning curve, it is infinitely faster and more efficient once you get the hang of it. For example, instead of digging through multiple network menus with a mouse just to find my IP address, I can grab it instantly without my hands ever leaving the keyboard. If I need to check it again, I just rerun the same command. 

## Why I am Prioritizing the Command Line
Beyond just speed, there are a few massive advantages to getting comfortable in the terminal:
* **Lower Resource Usage:** CLIs strip away the graphics, meaning they require far fewer system resources. This is perfect for running on older hardware, devices with limited memory, or keeping my cloud computing bills as low as possible.
* **Automation:** Trying to automate mouse clicks in a GUI is a nightmare. In the CLI, I can just write a quick batch file or script to automate repetitive commands.
* **Remote Management:** Managing servers, routers, or IoT devices remotely via SSH is incredibly convenient through the CLI, especially if the network connection is slow or the target system has limited resources.

## My Learning Objectives for this Module
I am using this room to get completely comfortable with `cmd.exe`, the default command-line interpreter in Windows. By the time I finish, I need to be able to:
1. Display basic system information.
2. Check and troubleshoot network configurations.
3. Manage files and folders entirely from the prompt.
4. Check and manage running system processes.

---

## Lab Notes: Establishing the SSH Connection
*I started up the provided lab machine and connected to it using the AttackBox terminal.*

**The Connection Process:**
1. Opened the AttackBox terminal.
2. Initiated the connection using the provided credentials: `ssh user@10.113.169.55`.
3. Since it was my first time connecting to this specific target, the system prompted me to verify the host key. I had to explicitly type `yes` to trust the connection.
4. I entered the password (`Tryhackme123!`). 
   * *Self-Reminder:* Passwords never display on the screen when typing in an SSH prompt. Just type it out and hit Enter.


# System Information & Essential CLI Concepts

##  Understanding Environment Variables and the "Path"
Before I even started throwing commands at the terminal, I had to understand *how* Windows knows what to execute. When I type a command, Windows searches through specific directories to find the corresponding executable program. This list of directories is called the **Windows Path**.

* **The Concept:** If a program isn't located inside one of the directories listed in the Path, the command prompt will throw an error unless I type out the entire absolute directory path to the file.
* **The Command:** I can view my current environment variables by typing `set`. It dumps a massive list of configurations, but I specifically look for the line starting with `Path=` to see exactly where my operating system is allowed to look for executable files.

##  Profiling the Target System
Whenever I connect to a new machine (like via SSH), my first priority is profiling the environment to understand what I am working with.
* **`ver`:** This is a quick and dirty way to check the exact Operating System version. It outputs a single line, like `Microsoft Windows [Version 10.0.17763.1821]`.
* **`systeminfo`:** When I need the full picture, this is the command I use. It generates a comprehensive breakdown of the system, including the OS Name (e.g., Windows Server 2019 Datacenter), Build Type, processor architecture, and memory availability.

##  The Magic of "Piping" (`|`)
Terminals can easily get overwhelmed by walls of text. To control this, I learned a core command-line concept called **piping**.
* **What is Piping?** Represented by the vertical bar character (`|`), piping takes the output (stdout) of the command on the left and literally "pipes" it in as the input (stdin) for the command on the right. It is a brilliant way to chain tools together.
* **Using `| more`:** For example, running a command like `driverquery` floods the screen with hundreds of lines of driver information. If I run `driverquery | more` instead, the terminal pauses the output to fit my screen. I can then press the **Space bar** to read through it page by page. If I find what I need early, I just hit **CTRL + C** to kill the output and return to the prompt.

##  Terminal Housekeeping
Finally, a couple of basic utility commands to keep my workflow smooth:
* **`help`:** If I forget the syntax or parameters for a specific tool, typing `help <command>` pulls up the built-in documentation.
* **`cls`:** Standing for "clear screen", this wipes all previous outputs and text from my terminal. I use this constantly to keep my workspace clean and my mind focused on the next task.

  # Windows Network Configuration & Troubleshooting

##  Network Configuration (`ipconfig`)
I am used to opening the Windows Control Panel to check network settings, but the CLI makes it instantaneous.
* **`ipconfig`:** This command outputs my fundamental network details—specifically my current IPv4 address, Subnet Mask, and Default Gateway. 
* **`ipconfig /all`:** When I need the full picture, appending `/all` reveals deeper configurations. It shows me my physical MAC address, confirms if DHCP is enabled (and when my IP lease expires), and lists my configured DNS servers.

##  Network Troubleshooting Tools
When a machine can't reach the internet or a specific server, these are the primary diagnostic tools I use:
* **`ping <target>`:** This is my first step in network troubleshooting. It sends ICMP packets to the target and listens for a response. If I receive a reply, I know the target is reachable and my machine is properly connected. It also provides the round-trip time (latency) in milliseconds.
* **`tracert <target>`:** If a `ping` fails or is unusually slow, I use `tracert` (Trace Route). It maps the exact path a packet takes through the network, listing every router (hop) it hits until it reaches the target. It relies on the "Time-to-Live" (TTL) packet drop mechanism to force routers along the path to identify themselves.

##  Advanced Networking Commands
Understanding the physical routing is important, but dealing with domains and current active connections is where I spend most of my time during security assessments.
* **`nslookup <domain>`:** This command queries DNS to resolve a domain name (like `example.com`) into its IP address. 
  * *Tip:* By default, it uses my machine's configured DNS server. However, I can force it to query a specific name server by appending it to the command (e.g., `nslookup example.com 1.1.1.1`).
* **`netstat`:** This is incredibly useful for security profiling. Without any arguments, it lists all currently established connections (e.g., showing an active SSH connection bound to port 22).

### Combining `netstat` Flags (`netstat -abon`)
To get maximum visibility into what my system is doing on the network, I combine four specific flags:
* **`-a`:** Displays all active connections AND listening ports (ports waiting for a connection).
* **`-b`:** Shows the specific executable program (like `sshd.exe` or `svchost.exe`) associated with the port.
* **`-o`:** Reveals the Process ID (PID) associated with the connection.
* **`-n`:** Forces numerical output for IP addresses and port numbers (stopping it from trying to resolve hostnames, which speeds up the output).
* **The Result:** Running `netstat -abon` gives me a comprehensive list showing exactly which programs are communicating on the network, what ports they are using, and their specific PIDs.
