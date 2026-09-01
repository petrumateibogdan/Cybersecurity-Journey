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

# 2. 
