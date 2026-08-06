## OverTheWire: Bandit - Level 0

**Objective:** 
Log into the game server using SSH over the command line.

**Commands Used:**
* `ssh`: To securely connect to a remote server over a specific port.

**Solution:**
1. I opened my local Windows terminal.
2. I constructed the SSH command to connect to the specific host and custom port (2220) provided by the game: `ssh -p 2220 bandit0@bandit.labs.overthewire.org`
3. I accepted the host key fingerprint and entered the initial password (`bandit0`).
4. I successfully established a remote connection and gained access to the Linux server, completing the first challenge.

## OverTheWire: Bandit - Level 0-> Level 1
