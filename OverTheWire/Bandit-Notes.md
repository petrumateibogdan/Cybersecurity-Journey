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
3. Executed `cat readme` to print the file contents, revealing the password for `bandit1`
4. Disconnected with `exit` and logged into `bandit1` using: `ssh -p 2220 bandit1@bandit.labs.overthewire.org`


## OverTheWire: Bandit - Level 1 → Level 2

**Objective:** 
Find the password for `bandit2` stored in a file named `-` located in the home directory.

**Commands Used:**
* `ls`: To list the files in the directory.
* `cat`: To read file contents using full or relative paths.

**Solution:**
1. Logged into `bandit1` via SSH.
2. Discovered a file named `-`. Running `cat -` directly failed because `-` is interpreted as standard input (stdin) rather than a filename.
3. Bypassed this by providing the absolute path to the file: `cat /home/bandit1/-` (Alternatively, using relative path `./`: `cat ./-`).
4. Obtained the password for `bandit2`.


## OverTheWire: Bandit - Level 2 → Level 3

**Objective:** 
Find the password for `bandit3` stored in a file named `--spaces in this filename--` in the home directory.

**Commands Used:**
* `ls`: To view the contents of the home directory.
* `cat`: To read the file using quotes or character escaping with relative paths.

**Solution:**
1. Logged into `bandit2` via SSH.
2. Ran `ls` to view the file `spaces in this filename`.
3. Handled spaces in the filename using multiple methods:
   * **Quotes:** `cat "--spaces in this filename--"`
   * **Tab Completion:** Typed `cat ./--s` and pressed `Tab` to automatically escape spaces with backslashes: `cat ./--spaces\ in\ this\ filename--`
4. Obtained the password for `bandit3`: `[REDACTED]`
