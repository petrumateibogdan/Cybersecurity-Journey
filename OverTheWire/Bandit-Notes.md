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

**Objective:** 
Find the password for `bandit1` stored in a file named `readme` in the home directory.

**Commands Used:**
* `ls`: To list files in the current directory.
* `cat`: To read and display the contents of the `readme` file.
* `ssh`: To connect to the next level as `bandit1`.

**Solution:**
1. Logged into `bandit0` via SSH.
2. Ran `ls` to locate the `readme` file.
3. Executed `cat readme` to print the file contents, revealing the password for `bandit1`: `[REDACTED]`
4. Disconnected with `exit` and logged into `bandit1` using: `ssh -p 2220 bandit1@bandit.labs.overthewire.org`
