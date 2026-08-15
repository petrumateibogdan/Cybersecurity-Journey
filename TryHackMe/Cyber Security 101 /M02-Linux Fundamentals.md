# Module 2: Linux Fundamentals

 ## 1. Linux Fundaments part 1

## Overview of Linux
* **Linux vs. Windows:** Linux is significantly more lightweight compared to operating systems like Windows.
* **Daily Usage:** Linux powers everyday technologies in various forms, including:
  * Websites you visit
  * Car entertainment and control panels
  * Point of Sale (PoS) systems such as checkout tills and registers
  * Critical infrastructures like traffic light controllers and industrial sensors
  * Phones and small computing devices

---

##  Deploying a Linux Machine
* Rooms often provide a browser-based Ubuntu Linux machine that can be accessed using the "Start Machine" or "Start Lab Machine" buttons.
* The management card displays essential information such as the IP address and expiry timer. 
* Always remember to click "Terminate" once you are finished with the room.

---

##  The Linux Terminal & Basic Commands
* Cybersecurity workflows rely heavily on the command line interface (CLI) rather than a mouse, from running hacking tools to tracking attackers.
* **Command:** An instruction given to the computer to perform a specific task.
* **`whoami`:** Tells you your current username on the system, which is critical when changing users to verify permissions.
* **`echo`:** Outputs specific text provided to it (e.g., `echo TryHackMe` or multiple words wrapped in quotes like `echo "hello world"`).
* **Pro Tip:** Press the up and down arrow keys on your keyboard in the terminal to scroll through previously entered commands.

---

##  File Navigation & Searching
Essential commands for navigating the file system without a mouse include:
* **`ls`:** List what is in the current folder. (Note: Folders typically display as blue on these systems).
* **`cd`:** Change directory—move into a specified folder.
* **`cat`:** Show the contents of a file.
* **`pwd`:** Print working directory—shows "where am I?".

Efficient searching commands:
* **`find`:** Search for files by their name (e.g., `find -name passwords.txt`).
* **`grep`:** Searches inside files for specific text (stands for *global regular expression print*). For example, running `grep "THM" access.log` searches through log files to extract matching lines like a flag (`THM{...}`).

---

##  Operators and Redirection
Special characters can combine commands or handle output routing:
* **`&`:** Runs the command in the background without waiting for it to finish. Useful for long-running processes.
* **`&&`:** Runs both commands sequentially, waiting for the first command to finish before executing the next.
* **`>` (Redirection):** Takes the output of a command and sends it to a file, overwriting any existing content. For example, `echo "TryHackMe" > thm` creates or overwrites the file `thm`.
* **`>>` (Append Redirection):** Adds output to the bottom of an existing file without replacing previous text. For example, using `>>` with the text `"thm"` appends it, which can be verified using `cat thm`.


## 2. Linux Fundamentals part 2



## Overview
This module transitions from in-browser terminals to a critical real-world skill: connecting to and controlling remote Linux machines. It expands on basic commands by introducing flags and arguments, advances filesystem manipulation, breaks down file permissions, and explores essential root directories.

---

##  Secure Shell (SSH)
Secure Shell (SSH) is an encrypted protocol used to securely connect to and interact with the command line of a remote Linux machine.
* **Encryption:** By using cryptography, any human-readable input sent over the network is encrypted during transit and decrypted once it reaches the remote machine.
* **Syntax:** Connecting requires an IP address and valid credentials.
  `ssh username@IP_address`
  *(Example: `ssh tryhackme@10.114.173.7`)*
* **Security Feature:** When typing a password in an SSH login prompt, there is absolutely no visible feedback (no characters or asterisks will appear). The terminal is still capturing the input; simply type the password and press Enter.

---

##  Flags, Switches, and the Manual
Commands perform a default behavior that can be extended using arguments known as flags or switches, typically identified by a hyphen (`-` or `--`).
* **Hidden Files (`ls -a`):** By default, `ls` hides files and folders starting with a period (`.`). Using the `-a` (or `--all`) flag reveals them.
* **The `--help` Option:** Most commands accept a `--help` flag to list possible options, descriptions, and examples.
* **The Manual (`man`):** The `man` command provides the formatted manual page for a command. It is the built-in documentation for system commands and applications.
  `man ls`
  
---

##  Advanced Filesystem Commands
Interacting with the filesystem goes beyond basic navigation. These commands are essential for file and directory manipulation:
* `touch <filename>`: Creates a blank file. (Content must be added later using tools like `echo` or `nano`).
* `mkdir <directory>`: Creates a new folder (make directory).
* `rm <filename>`: Removes (deletes) a file.
  * To delete a directory and its contents, the recursive switch is required: `rm -R <directory>`.
* `cp <existing_file> <new_file>`: Copies the entire contents of an existing file into a new file.
* `mv <file> <destination>`: Moves a file or folder. It is also the command used to **rename** files by simply moving them to a new name (e.g., `mv note2 note3`).
* `file <filename>`: Determines the actual type and purpose of a file based on its contents, rather than relying on file extensions (like `.txt`), which can be misleading.

---

##  Users, Groups, and Permissions
File access in Linux is highly granular. You can determine characteristics and permissions using `ls -lh` (long, human-readable format). 

### Switching Users
The `su` (substitute user) command allows transitioning between accounts if you know the target user's password.
* `su user2`: Switches to user2 but keeps you in the current directory.
* `su -l user2`: The `--login` switch starts a shell that mimics an actual login. It drops you into the new user's home directory and inherits their environment variables.
* **Sudo:** Stands for "superuser do," allowing authorized users to run commands with root privileges.

### Permission Structure
Permissions determine who can **read (`r`)**, **write (`w`)**, or **execute (`x`)** a file. They are divided into three groups of three characters (e.g., `rwxrwxrwx`):
1. **First 3:** Owner permissions
2. **Next 3:** Group permissions
3. **Last 3:** Others (everyone else)

### Numeric Permissions
Permissions are frequently managed using numeric values with the `chmod` command. Each permission type has a specific value:
* **Read (r)** = 4
* **Write (w)** = 2
* **Execute (x)** = 1

**Calculating the value:** Add the numbers for each group. 
* `rwx` = 4 + 2 + 1 = 7.
* `r-x` = 4 + 0 + 1 = 5.

**Common Numeric Examples:**
* `777` (`rwxrwxrwx`): Everyone has full access.
* `755` (`rwxr-xr-x`): Owner can do everything; Group and Others can only read and execute.
* `644` (`rw-r--r--`): Owner can read/write; Group and Others can only read.
* `700` (`rwx------`): Only the owner has access.
* `750` (`rwxr-x---`): Owner has full access, Group can read/execute, Others have no access.

---

##  Important Root Directories
Understanding the Linux root directory structure is essential for locating system configurations, user data, and temporary files.
* `/etc`: (Etcetera) Stores vital system files and configurations used by the OS. 
  * `sudoers`: Lists users/groups permitted to run commands as root.
  * `passwd` and `shadow`: Store user information and their passwords (encrypted in SHA-512 format).
* `/var`: (Variable Data) Stores data frequently accessed or written by system services, such as application log files (`/var/log`) and databases.
* `/root`: The dedicated home directory for the "root" system user (distinct from `/home/root`).
* `/tmp`: (Temporary) A volatile directory used to store data temporarily. Its contents are cleared when the computer restarts. 
  * **Pentesting Note:** Any user can write to `/tmp` by default, making it an ideal location to store enumeration scripts when first gaining access to a machine.

 ## 3. Linux Fundamentals Part 3

# Network Transfers, Processes, and System Management

## Overview
This section covers essential Linux administration skills: transferring files across networks, managing background processes, scheduling automated tasks, handling software packages, and reviewing system logs.

---

## 1. Transferring Files
### Downloading with `wget`
* `wget` allows you to download files from the web via HTTP, similar to accessing a file in a browser.
* **Syntax:** `wget <URL>`
  * *Example:* `wget https://assets.tryhackme.com/myfile.txt`

### Secure Copy (`scp`)
* `scp` copies files securely between two computers using the SSH protocol (providing both authentication and encryption).
* **Syntax Structure:** `scp <SOURCE> <DESTINATION>`
* **Local to Remote:**
  `scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt`
* **Remote to Local:**
  `scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt`

### Serving Files via Python
* Ubuntu comes pre-packaged with Python 3, which includes a lightweight module called `http.server`.
* Running `python3 -m http.server` instantly turns the current directory into a web server, allowing other computers to download your files using commands like `curl` or `wget`.
* By default, it serves files on port 8000. *(Example download: `wget http://10.114.145.150:8000/file`)*

---

## 2. Managing Processes
Processes are running programs managed by the kernel. Each is assigned an incremental Process ID (PID).
* **Viewing Processes:**
  * `ps`: Lists running processes for the current user's session.
  * `ps aux`: Shows a detailed list of all processes, including those run by other users and system processes.
  * `top`: Provides real-time, refreshing statistics about system processes.
* **Terminating Processes:**
  * Use the `kill <PID>` command to terminate a process (e.g., `kill 1337`).
  * *Signals:*
    * `SIGTERM`: Kills the process but allows it to clean up first.
    * `SIGKILL`: Kills the process instantly with no cleanup.
    * `SIGSTOP`: Suspends/stops the process.
* **Process Origins (systemd):** On Ubuntu, `systemd` (PID 1) initializes the system. Any program started afterward runs as a child process of `systemd`.
* **Managing Services:** Use `systemctl [option] [service]` to control services (e.g., `systemctl start apache2`). Options include start, stop, enable, disable, and status.

### Backgrounding and Foregrounding
* Commands typically run in the **foreground**, blocking the terminal until finished.
* **Backgrounding:** Append the `&` operator to a command (e.g., `echo "Hi THM" &`) to run it in the background. If a script is already running, press `Ctrl + Z` to suspend/background it.
* **Foregrounding:** Use the `fg` command to bring a backgrounded process back to the terminal focus.

---

## 3. Automation with Cron
The `cron` process facilitates scheduled tasks using special files called `crontabs`.
* **Editing:** Use `crontab -e` to edit the current user's crontab file.
* **Syntax:** Crontabs require 6 values: `MIN` `HOUR` `DOM (Day of Month)` `MON` `DOW (Day of Week)` `CMD`.
  * *Example:* `0 */12 * * * cp -R /home/cmnatic/Documents /var/backups/` (Backs up the Documents folder every 12 hours).
  * An asterisk (`*`) acts as a wildcard, meaning "every" or "any."

---

## 4. Software Package Management
Software on Ubuntu is managed via `apt` (Advanced Package Tool), which securely retrieves tools from community and vendor repositories.
* **Repositories:** A registry of available software. You can add third-party repositories using `add-apt-repository`.
* **GPG Keys (Gnu Privacy Guard):** Ensure the integrity and authenticity of downloaded software. If a repository's GPG key doesn't match, the system will reject the download.
* **Updating and Installing:**
  * `apt update`: Updates your system's list of available packages.
  * `apt install <software_name>`: Installs the specified package.
  * `apt remove <software_name>`: Uninstalls the software.

---

## 5. System Logs
Located in `/var/log`, these files contain vital diagnostic and security information for the OS and running services.
* **Common Service Logs:**
  * Apache2 (Web Server): Contains an `access log` (records all requests) and an `error log`.
  * `fail2ban`: Monitors for intrusion attempts like brute-force attacks.
  * UFW (Uncomplicated Firewall): Logs firewall activity.
* Logs are crucial for developers diagnosing performance issues and administrators investigating suspicious activity or authentication attempts.
 

 
