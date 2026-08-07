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
4. Obtained the password for `bandit3`.

## OverTheWire: Bandit - Level 3 → Level 4

**Objective:** 
Find the password for `bandit4` stored in a hidden file inside the `inhere` directory.

**Commands Used:**
* `cd`: To change the working directory.
* `ls -a`: To list all files, including hidden ones (files prefixed with a dot `.`).
* `cat`: To read the contents of the hidden file.

**Solution:**
1. Logged into `bandit3` via SSH.
2. Navigated into the target directory: `cd inhere`
3. Ran `ls` (which showed nothing) and realized the file was hidden.
4. Used `ls -a` to view all files, revealing a hidden file named `.hidden` (or `...Hiding-From-You`).
5. Read the contents using `cat ...Hiding-From-You` to retrieve the password for `bandit4`.

## OverTheWire: Bandit - Level 4 → Level 5

**Objective:** 
Find the password for `bandit5` stored in the only human-readable (ASCII text) file inside the `inhere` directory.

**Commands Used:**
* `cd`: To change directory into `inhere`.
* `file`: To determine the type of data contained within files.
* `cat`: To read the contents of the identified text file. 
### Commands & Concepts Learned
* `file`: Inspects file headers to reveal true file types (e.g., ASCII text vs. binary data).
* `*` (Wildcard): Matches all files/directories in the current path.
* `./*`: Passes all files in the current folder to a command safely without triggering option flags on filenames starting with `-`.

**Solution:**
1. Logged into `bandit4` via SSH.
2. Navigated into the target directory: `cd inhere`
3. Ran `file ./*` to inspect every file in the directory at once using the wildcard `*`.
4. Spotted the only file marked as **`ASCII text`** (while the rest were raw binary `data`). // '
5. Read that specific file using `cat` (`cat ./-file07`) to retrieve the password for `bandit5`.


## OverTheWire: Bandit - Level 5 → Level 6

**Objective:** 
Find the password for `bandit6` stored in a file somewhere under the `inhere` directory that matches all of the following properties:
* Human-readable (ASCII text)
* Exactly 1033 bytes in size
* Not executable

**Commands Used:**
* `cd`: To navigate into directories (`cd inhere`).
* `find`: To search files based on criteria like size, type, and permissions.
* `cat`: To read the contents of the target file.

**Solution:**
1. Logged into `bandit5` via SSH.
2. Entered the directory: `cd inhere`
3. Ran `find` with exact filters to isolate the file:
   
   `find . -type f -size 1033c ! -executable` ( dot . = my current directory , type f = regular files, size 1033c, c = characters=bytes in linux, ! - executable NON)
   4. The command returned the target path and i read the file with  cat./-file 2
   
