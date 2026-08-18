# Module 4: Command Line

# 1. Windows Command Line



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

# Navigating the Filesystem via Command Line

##  Finding My Bearings and Exploring Directories
When working strictly in the terminal, it is easy to lose track of where I am. 
* **`cd` (Check Location):** Typing `cd` with no parameters acts like a compass—it outputs my exact current directory path (e.g., `C:\Users\strategos`).
* **`dir` (List Contents):** This is the equivalent of opening a folder in the GUI to see what is inside. It lists all standard files and child directories. 
  * I use `dir /a` to force it to show hidden and system files that normally wouldn't appear.
  * I use `dir /s` to recursively display files in the current folder *and* all subfolders beneath it.
* **`tree` (Visual Map):** Sometimes a flat list isn't enough. Typing `tree` draws a visual, hierarchical map of the current directory and all subdirectories, which is incredible for understanding the structure of an unfamiliar machine.

##  Moving Around and Managing Folders
Navigating through the system requires the `cd` (Change Directory) command.
* **`cd <target>`:** Moves me into a specific child directory (e.g., `cd Documents`).
* **`cd ..`:** Moves me exactly one level *up* in the directory tree (from `C:\Users\strategos` back to `C:\Users`).

Creating and destroying folders is equally straightforward:
* **`mkdir <name>`:** Makes a new directory.
* **`rmdir <name>`:** Removes an existing directory. *(Note: By default, the folder must be empty for this to work).*

##  Interacting With Files
I cannot double-click a text file to open it in Notepad when working in a remote CLI session. Instead, I have to interact with the file contents directly in the terminal.
* **`type <filename>`:** This command dumps the entire contents of a text file directly onto the screen. It is perfect for quickly reading small config files or logs.
* **`more <filename>`:** If the file is massive, using `type` will just flood the screen. `more` opens the file one terminal-page at a time, allowing me to page through it using the Spacebar.

Managing the files themselves requires three core commands:
* **`copy <source> <destination>`:** Duplicates a file to a new location.
* **`move <source> <destination>`:** Moves a file from one location to another (this is also how I rename files in the CLI by just moving them to a new filename).
* **`del` or `erase <filename>`:** Permanently deletes a file.

##  The Power of Wildcards (`*`)
I quickly realized that typing out individual file names is tedious. The asterisk (`*`) acts as a wildcard, meaning "anything." 
For example, if I want to move every single text file in my current directory to a backup folder, I don't have to move them one by one. I can just execute `move *.txt C:\backup_files`.

# Managing Windows Processes via CLI

##  Ditching the GUI Task Manager
I have always relied on the classic `Ctrl + Shift + Esc` to bring up the Windows Task Manager whenever an application freezes or when I want to see what is eating up my RAM. However, managing a remote server through an SSH shell requires a completely different, CLI-driven approach. 

To see everything running on the system, I use the `tasklist` command. It essentially dumps the entire Task Manager "Processes" tab straight into my terminal. The output includes crucial details like the Image Name (the executable), the Process ID (PID), Session details, and Memory Usage for every active task.

##  Filtering the Noise
Running a raw `tasklist` command is overwhelming because a standard Windows environment runs dozens of background services simultaneously. To make it manageable and actually useful for troubleshooting, I have to filter the output.

* **Checking the Manual:** Running `tasklist /?` shows all the available filtering options.
* **Targeting a Specific Process:** If I am hunting down all running instances of the SSH daemon, I use the `/FI` (Filter) switch like this:
  
  `tasklist /FI "imagename eq sshd.exe"`
  
This exact syntax tells the command prompt to filter and return only the processes where the image name is strictly equal (`eq`) to `sshd.exe`. It instantly cuts out the noise and isolates the exact PIDs I need to look at.

##  The Kill Switch (`taskkill`)
Once I have used `tasklist` to hunt down the Process ID (PID) of a frozen, rogue, or malicious program, I need a way to forcefully terminate it without clicking an "End Task" button.

* **The Command:** I use `taskkill /PID <target_pid>`
* **Example Execution:** If the problematic process I found in the previous step has a PID of 4567, I simply execute `taskkill /PID 4567` to kill it immediately.

# Wrapping Up: Extra Utilities & Key Takeaways

##  The "Good to Know" System Commands
While this module focused heavily on practical daily commands for navigation and networking, the Windows CLI has some heavy-duty administrative tools built in. I am keeping a note of these three commands because they are absolute lifesavers when a system starts acting up or failing:

* **`chkdsk` (Check Disk):** This command scans the file system and physical disk volumes to find structural errors or bad sectors. It is the go-to tool when a hard drive starts failing or corrupting data.
* **`driverquery`:** Just like the name implies, it dumps a complete list of all installed device drivers. This is incredibly useful for auditing a system or figuring out exactly which driver is causing hardware conflicts without digging through the GUI Device Manager.
* **`sfc /scannow` (System File Checker):** If the Windows operating system itself feels broken or corrupted, this command scans all core system files, verifies their integrity, and automatically attempts to repair them if possible.

##  My Golden Rules for the Command Line
If there is anything I am taking away from this module, it boils down to how I handle getting stuck and how I handle terminal output:

* **The Universal Help Switch (`/?`):** Memorizing every command parameter is **almost** impossible. :) Knowing that I can append `/?` to almost *any* Windows command to instantly pull up its manual and syntax examples is the ultimate cheat code.
* **The Dual Nature of `more`:** I have learned to use `more` in two distinct, powerful ways:
   **Reading Files:** `more file.txt` allows me to open and read lengthy text files directly in the terminal, paging through them safely without flooding the screen.
   **Taming Output (Piping):** `[any_command] | more` takes a massive output (like a `netstat` or `driverquery` dump) and forces it to display page-by-page. 

## Next Steps in My Journey
Now that I have a solid grip on the legacy `cmd.exe` environment, it is time to graduate to the modern, significantly more powerful scripting engine: **Windows PowerShell**.

# Managing System Power via CLI

## Taking Control of Shutdowns and Restarts
As a final touch on basic Windows CLI administration, I learned how to manage system power states directly from the prompt. This is incredibly useful when managing a remote server via SSH, where I don't have access to the physical power button or the GUI Start menu.

* **`shutdown /s`:** This is the standard command to completely shut down the system.
* **`shutdown /r`:** This command is used to reboot (restart) the machine. The `/r` flag simply stands for restart. 



* **`shutdown /a`:** This is essentially the panic button. If a shutdown or restart is triggered (which usually gives a brief warning countdown before executing), I can quickly type `shutdown /a` to **abort** the scheduled shutdown and keep the system online.

# 2. Windows PowerShell

# Introduction to PowerShell

## Stepping Up from the Command Prompt
After getting comfortable with the legacy `cmd.exe` environment, diving into PowerShell feels like stepping into the future. I learned that PowerShell isn't just a command-line shell—it is a full-blown cross-platform task automation solution, a scripting language, and a configuration management framework all rolled into one. 

Unlike the older command prompt, which is strictly tied to Windows, PowerShell was expanded to support macOS and Linux (known as PowerShell Core), making it an incredibly versatile tool to learn for modern IT and cybersecurity.

## The History: Why Was PowerShell Created?
The backstory of PowerShell actually helped me understand its architecture a lot better. 
* **The Problem:** In the early 2000s, traditional tools like `cmd.exe` and batch scripts simply couldn't handle the complex administrative tasks required by modern enterprise Windows environments.
* **The Unix vs. Windows Dilemma:** A Microsoft engineer named Jeffrey Snover realized a fundamental difference between operating systems: Unix treats everything as plain text files, whereas Windows relies heavily on structured data and APIs. Because of this, simply porting Unix command-line tools over to Windows was clunky and inefficient.
* **The Solution:** Snover decided to build an entirely new, object-oriented shell backed by the power of the .NET framework. It officially released in 2006, and by 2016, Microsoft open-sourced it as a cross-platform tool.

## The Secret Sauce: Objects vs. Text
The absolute biggest "aha!" moment for me in this module was understanding **The Power in PowerShell**: the concept of Objects. 

In traditional command shells (like Linux bash or Windows `cmd.exe`), commands output plain text. If I want to extract specific data from that output, I have to rely on complex text-parsing tools (like `grep` or `awk`) to slice and dice strings of text.

PowerShell does **not** output plain text. Instead, its commands (called **cmdlets**) output **Objects**. 

### What is an Object?
In programming, an object is a data structure that contains two things:
1. **Properties (Data/Characteristics):** e.g., A car's *Color*, *Model*, or *Fuel Level*. In PowerShell, a file object's properties would be its *Name*, *Size*, or *Creation Date*.
2. **Methods (Actions):** e.g., A car's *Drive()* or *HonkHorn()* function. In PowerShell, a process object might have a *Kill()* or *Restart()* method.

**Why this matters:** Because PowerShell passes full objects down the pipeline instead of raw text, I don't have to parse strings. I can directly manipulate the data, filter by specific properties, and execute methods on the results seamlessly. It is infinitely more powerful and flexible than string manipulation.



# My First Steps into PowerShell

##  Firing Up the Shell
Before I could even start hacking away at PowerShell, I had to figure out how to launch the thing. In this lab, I connected to the target VM (`10.113.155.83`) using an SSH connection. But honestly, if I were just sitting at a normal Windows machine, there are a bunch of ways I could open it locally:
* **Start Menu:** Just search for `powershell`.
* **Run Dialog:** Hit `Win + R`, type `powershell`, and smash Enter.
* **File Explorer:** Typing `powershell` right into a folder's address bar. (I love this one because it drops you straight into that directory).
* **Task Manager:** Going to **File > Run new task**.

Since I was SSH'd into a legacy `cmd.exe` terminal, I just typed `powershell` and hit Enter. The prompt flipped to `PS C:\Users\captain>`, letting me know I was finally in the right environment.

##  The "Verb-Noun" Rule (Making Life Easy)
The coolest thing I realized about PowerShell is that I don't have to memorize a bunch of cryptic garbage commands. It uses **cmdlets** (pronounced "command-lets") that follow a dead-simple **Verb-Noun** naming convention. 

The Verb is what I want to do, and the Noun is what I want to do it to. It makes guessing commands incredibly intuitive:
* `Get-Content`: Gets the text inside a file.
* `Set-Location`: Changes my current directory.

##  The Holy Trinity of Discovery Cmdlets
I quickly learned I don't need to know every cmdlet by heart. I just need these three built-in tools to figure out everything else on the fly:

### `Get-Command`
This is basically my master directory. It lists every cmdlet, function, and alias available. Since the output is huge, I can filter it. For example, to only see functions, I run:
`Get-Command -CommandType "Function"`

### `Get-Help`
This is my built-in cheat sheet. If I don't know how a cmdlet works, I just ask for help. I can even slap flags on it to get exactly what I need:
* `Get-Help Get-Date -examples` (Just show me how to use it in the real world).
* `Get-Help Get-Date -detailed` 
* `Get-Help Get-Date -full`
* `Get-Help Get-Date -online` (Opens the official Microsoft docs directly in my browser).

### `Get-Alias`
To save my muscle memory from my Linux and CMD days, PowerShell uses aliases (shortcuts). Running `Get-Alias` showed me how my old commands map to PowerShell cmdlets. 
* Typing `dir` actually runs `Get-ChildItem` in the background.
* Typing `cd` actually runs `Set-Location`.

##  Upgrading My Arsenal (Downloading Modules)
PowerShell isn't just limited to what comes in the box. I learned I can hunt down and install new modules (like plugins) from online repos like the PowerShell Gallery. *(Note: My lab VM didn't have internet, so I couldn't test it live, but the syntax is essential for real-world scenarios).*

* **Hunting for tools:** If I want to find a module but only know part of the name, I use wildcards (`*`):
  `Find-Module -Name "PowerShell*"`
    
* **Installing them:** Once I find the one I want, I just install it:
  `Install-Module -Name "PowerShellGet"`
      
  *(Note to self: If the module is from an untrusted repo, PowerShell throws a warning prompt. I just type `Y` to accept and force it through).*

  # Advanced PowerShell Notes: File Management, Piping & System Recon

##  Ditching CMD for Universal File Management
In legacy CMD, navigating and modifying files required a bunch of fragmented commands (`dir`, `cd`, `mkdir`, `del`). I learned that PowerShell simplifies this by treating files and folders uniformly as "Items."

* **`Get-ChildItem`**: The modern replacement for `dir` or `ls`. It lists contents in my current directory or a specific path via `-Path`.
* **`Set-Location`**: My new `cd`. Used to change my working directory.
* **`New-Item`**: Creates both files and folders. I just specify the path and use `-ItemType "Directory"` or `-ItemType "File"`.
* **`Remove-Item`**: Deletes items natively, completely replacing both `rmdir` and `del`.
* **`Copy-Item` / `Move-Item`**: Duplicates or moves items around. Moving an item to a new name is also how I rename files here.
* **`Get-Content`**: Reads text files (like `type` or `cat`). Since PowerShell is object-oriented, it actually returns the file's lines as an array of string objects, not just raw text!

##  Unlocking the Pipeline (`|`) and Filtering
This is where PowerShell blows standard CLI out of the water. Instead of piping raw text, the `|` symbol passes **entire objects** (data + properties + methods) from one cmdlet to the next.

* **`Sort-Object`**: Sorts my piped objects by a specific property (e.g., `Get-ChildItem | Sort-Object Length`).
* **`Where-Object`**: Filters objects based on conditions. For example, `Where-Object -Property "Extension" -eq ".txt"`.
  * *Crucial Operators I need to memorize:* 
    * `-eq` (equal) / `-ne` (not equal)
    * `-gt` (greater than) / `-ge` (greater or equal)
    * `-lt` (less than) / `-le` (less or equal)
    * `-like` (pattern matching using wildcards like `*`)
* **`Select-Object`**: Cleans up output by selecting specific properties (like just `Name` and `Length`) or limiting results (e.g., `-First 1` to find the largest file).
* **`Select-String`**: PowerShell's `grep`. It searches for text patterns or full Regex inside files!

##  System Recon & Dynamic Monitoring
As someone studying Blue/Red teaming, these built-in system cmdlets are goldmines for reconnaissance and incident response.

### Static System Info
* **`Get-ComputerInfo`**: Dumps a massive snapshot of the OS build, hardware specs, and BIOS details.
* **`Get-LocalUser`**: Lists all local accounts and shows if they are enabled/disabled.
* **`Get-NetIPConfiguration`**: Shows active network interfaces, IP addresses, DNS, and gateways.
* **`Get-NetIPAddress`**: A much deeper dive showing *all* IPs on the machine (including inactive ones or IPv6 link-local addresses).

### Dynamic Monitoring
* **`Get-Process`**: Dumps running processes, memory/CPU usage, and PIDs.
* **`Get-Service`**: Checks the status (`Running`/`Stopped`) of all system services (crucial for hunting persistence).
* **`Get-NetTCPConnection`**: Displays active network connections and ports. Perfect for spotting C2 backdoors.
* **`Get-FileHash`**: Generates a cryptographic hash (like SHA256) of a file to check for tampering or to analyze malware.

##  Hunting Alternate Data Streams (ADS)
I learned that NTFS files have a default data stream (`:$DATA`), but attackers can hide malicious payloads in hidden Alternate Data Streams.
* I can hunt for these by running: `Get-Item -Path "C:\file.txt" -Stream *`
* If I see an extra stream name attached to the file (like `housinginfo`), it means extra hidden data is riding along with that file.

##  Scripting & Remote Execution
PowerShell scripts (`.ps1` files) act like automated to-do lists, making them essential for SysAdmins, Blue Teamers (automating log analysis/malware reverse engineering), and Red Teamers (system enumeration/bypassing defenses).

The ultimate tool for remote administration is **`Invoke-Command`**, which lets me run commands or scripts on remote machines over the network.
* **Executing a local script on a remote server:**
  ```powershell
  Invoke-Command -FilePath c:\scripts\test.ps1 -ComputerName Server01

  Invoke-Command -ComputerName Server01 -Credential Domain01\User01 -ScriptBlock { Get-Culture }

# 3. Linux Shells




##  The CLI Mindset: Cooking in the Kitchen
The introduction to this module uses a perfect analogy: using a GUI is like sitting in a restaurant and ordering from a menu—the waiter (the OS) does the work, but I am limited to what is explicitly offered on that menu. 

Using the CLI (Command Line Interface) means walking directly into the kitchen and cooking the meal myself. It requires more knowledge, but it gives me absolute power, efficiency, and control over the Linux system. 

##  My Core Linux Arsenal
Before diving into scripts, I need to make sure my basic navigation muscle memory is sharp. By default, opening a terminal drops me into my home directory.
* **`pwd` (Print Working Directory):** Tells me exactly where I am in the filesystem (e.g., `/home/user`).
* **`cd <directory>` (Change Directory):** Moves me around the filesystem (e.g., `cd Desktop`).
* **`ls`:** Lists the contents of my current directory.
* **`cat <filename>`:** Dumps the text contents of a file directly onto the screen.
* **`grep <pattern> <filename>`:** My absolute favorite command. It searches massive files for a specific keyword and extracts just the lines I need (e.g., `grep THM dictionary.txt`).

##  Exploring Different Shells
The "terminal" is just the visual window; the *Shell* is the actual engine processing my commands. 
* To see my current shell: `echo $SHELL`
* To list all installed shells: `cat /etc/shells`
* To permanently change my default shell: `chsh -s /path/to/shell`

### The Big Three Shells:
1. **Bash (Bourne Again Shell):** The default on almost every Linux distribution. It has rock-solid scripting capabilities, basic tab-completion, and command history (using the up/down arrows).
2. **Fish (Friendly Interactive Shell):** Built purely for user-friendliness. It features auto-spell correction, incredible syntax highlighting (colors change based on command roles or errors), and advanced tab-completion based on history.
3. **Zsh (Z Shell):** The modern powerhouse. It combines the reliability of Bash with the features of Fish. It supports intense customization (like the `oh-my-zsh` framework) and massive plugin support, though heavy customization can slow it down slightly.

##  Shell Scripting 101
Scripting is just writing a to-do list of commands for the computer to execute automatically. Instead of typing repetitive commands manually, I write them once in a `.sh` file.

### The Setup
1. **Create the file:** `nano script.sh`
2. **The Shebang:** The absolute first line of *any* script must be `#!/bin/bash`. This tells the OS exactly which interpreter to use to run the code.
3. **Execution Permissions:** Before running the script, I *must* make it executable using `chmod +x script.sh`.
4. **Running it:** I execute it by typing `./script.sh` (the `./` forces the shell to look in my *current* directory rather than searching global system paths).

### Variables & User Input
I can store data in variables to reuse later, and I can prompt the user for input using the `read` command.
```bash
#!/bin/bash
echo "Hey, what’s your name?"
read name
echo "Welcome, $name"



#!/bin/bash
# This prints numbers 1 through 10
for i in {1..10}; do
    echo $i
done

#!/bin/bash
echo "Please enter your name first:"
read name

if [ "$name" = "Stewart" ]; then
        echo "Welcome Stewart! Here is the secret: THM_Script"
else
        echo "Sorry! You are not authorized to access the secret."
fi


#!/bin/bash 

# Defining the variables
username=""
companyname=""
pin=""

# Defining the loop (giving the user 3 prompts)
for i in {1..3}; do
        if [ "$i" -eq 1 ]; then
                echo "Enter your Username:"
                read username
        elif [ "$i" -eq 2 ]; then
                echo "Enter your Company name:"
                read companyname
        else
                echo "Enter your PIN:"
                read pin
        fi
done

# Checking if the user entered the correct details using AND (&&)
if [ "$username" = "John" ] && [ "$companyname" = "Tryhackme" ] && [ "$pin" = "7385" ]; then
        echo "Authentication Successful. You can now access your locker, John."
else
        echo "Authentication Denied!!"
fi
