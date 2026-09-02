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


Virtual Host Enumeration (vhost m
Virtual hosts are different websites hosted on the exact same server and IP address. I learned that unlike dns mode (which does DNS lookups), vhost mode finds virtual hosts by sending web requests and modifying the Host: header in the HTTP request.

Useful Flags for vhost Mode:
-u: Specifies the base URL or IP address.

--domain: Appends a domain to each wordlist entry to form a valid hostname.

--append-domain: Appends the base domain to each word in the wordlist, preventing the tool from just requesting www or blog without the root domain.

--exclude-length: Filters out false positives by excluding responses of a certain byte length (e.g., filtering out default 404 error pages that share the exact same size).


gobuster vhost -u "[http://10.114.164.57](http://10.114.164.57)" --domain example.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320
