# Module 9: Offensive Security Tooling

# 1. Hydra


I recently learned how to use **Hydra**, a fast and highly flexible network logon brute-forcer. It allows penetration testers to automate the process of guessing login credentials across a network to test authentication strength.

Here are my notes and a command cheat sheet for using Hydra to crack services like SSH, FTP, and Web Forms.

---

## 1. What is Hydra?
Hydra is a brute-force password cracking tool designed to attack online authentication services. 
* **Supported Protocols:** It supports a massive list of protocols, including SSH, FTP, HTTP/HTTPS (GET and POST), RDP, SMB, MySQL, Telnet, VNC, and many more.
* **Purpose:** It highlights the importance of strong, complex passwords. Default credentials (like `admin:password`) or simple passwords found in common wordlists will be cracked in seconds using this tool.

---

## 2. Basic Command Structure
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

## 3. Exploitation Examples

### FTP Brute Force
If I want to brute-force an FTP server using a known username (`user`) and a wordlist (`passlist.txt`):
```bash
hydra -l user -P passlist.txt ftp://<MACHINE_IP>
