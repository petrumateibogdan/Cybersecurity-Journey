## Module 3: Operating System Basics

## 1. Operating Systems: Introduction

In this section, I explored what an Operating System (OS) actually is and why it's the most critical piece of software on a computer. 

* **The OS as a Manager:** I learnt that the OS acts as the invisible middleman between the user, the applications, and the physical hardware. Without it, every single app would fight for direct control over the CPU and memory, causing immediate crashes.
* **The Airport Analogy:** A great way to visualize this is by thinking of the computer as a busy airport. 
  * **Hardware (CPU, RAM):** The physical runways and infrastructure.
  * **Applications (Browsers, Games):** The airplanes and passengers trying to take off and land.
  * **The OS:** The Air Traffic Control tower that schedules resources, manages the traffic, and ensures nothing crashes into each other.

### System Privilege Layers
To keep the system secure and stable, I learnt that the OS separates permissions into two distinct areas:
* **Kernel Space:** The highly secure, privileged "control tower" of the system. The kernel has 100% unrestricted access to the CPU, RAM, and physical hardware.
* **User Space:** The restricted, safe area where normal apps run. 
* **System Calls:** Applications in the user space are deliberately blocked from touching hardware. If an app wants to save a file, connect to Wi-Fi, or play a sound, it has to politely ask the kernel to do it on its behalf using a "system call."

### Core OS Duties
Behind the scenes, the OS is constantly juggling several main responsibilities:
* **Process Management:** Allocating CPU time and scheduling programs so multitasking feels seamless.
* **Memory Management:** Handing out RAM to active apps and keeping them isolated so one crashed app doesn't take down the others.
* **File System Management:** Organizing folders, paths, metadata, and file permissions.
* **User Management:** Handling passwords, authentication, and keeping one user's files private from another.
* **Device Management:** Loading drivers so that when I plug in a new mouse or printer, it works immediately.

### Built-in OS Security
I also realized that the OS is the first line of defense. Long before you install an antivirus, the OS is already protecting the system by enforcing login authentication, strictly controlling read/write permissions, isolating processes into their own boxes, and locking down critical system files.

### How Users Interact with the OS
I also learnt about the two primary ways a user can communicate with the operating system:
* **GUI (Graphical User Interface):** This is the visual method most people use daily. It relies on windows, icons, and menus that you navigate with a mouse (like a standard Windows desktop). It is designed to be highly intuitive and user-friendly.
* **CLI (Command-Line Interface):** This is the text-based method of interacting with the computer. Instead of clicking icons, you type specific text commands into a terminal. While it has a steeper learning curve, I learned that the command line is significantly faster, uses fewer system resources, and is an absolute necessity for cybersecurity and system administration.


## 2. Windows Basics

In this track, I went over the core components of the Windows operating system. Since I have been using Windows on my personal computers up until now, I was already familiar with almost all of these tools and features, but it was still a great refresher to see them formally defined from an IT perspective:

* **Desktop:** The primary workspace where shortcuts, files, and folders live.
* **Taskbar:** The control strip that holds open applications, system tools, and notifications.
* **Start Menu:** Accessed via the Windows logo, this is the main hub for launching apps, finding settings, and managing power options.
* **Search:** A quick utility for finding files, applications, or settings just by typing keywords.
* **File Explorer:** The built-in navigation tool used to manage and organize directories and files.
* **Windows Update:** The service that automatically keeps the OS, security features, and native apps patched and up to date.
* **Microsoft Store:** The official marketplace for installing trusted Windows applications.
* **Windows Settings:** The modern, centralized dashboard for configuring system, personalization, and security options.
* **Control Panel:** The legacy management interface used for deeper, more advanced system configurations.
* **Task Manager:** A real-time monitoring tool used to check system performance and manage running processes.
* **Windows Security:** The main dashboard for Windows' built-in antivirus and threat protection tools.
* **Windows Defender Firewall:** The native firewall that monitors and blocks unauthorized network traffic to protect the system.

## 3. Linux CLI (Command-line interface)  Basics (VERY IMPORTANT)!!

In this room, I learnt about the essential commands for the Linux terminal. Here are the commands I used and what they do:

* **`pwd`("where am I?"):** Print Working Directory = shows me the folder I'm currently in.
* **`ls`:** Lists files in the current folder.
* **`ls -l`:** Lists files in LONG FORMAT.
* **`ls -al`:** Lists all hidden files (files that start with a `.` dot). Linux hides them by default.
* **`cd <directory>`:** Change directory.
* **`cd ..`:** Go back one level.
* **`find ~ -name mission_brief.txt`:** Finds a file (`~` is home in Linux).
* **`cat`:** Reads the file.
* **`whoami`:** Shows who you're logged in as.
* **`uname -a` (Unix name):** Shows details about the OS and kernel version. (Just using `uname` shows only the OS name).
* **`df -h`:** Disk free in human-readable format. If I would have said only `df`, it would have been in bytes.
* **`cd /etc`:** Changes into the `/etc` directory to read a system file.
* **`cat os-release`:** Reads the OS release system file.
* **`cd ~ && clear`:** Clears all and starts from the beginning in the home directory.
* **`CTRL+C`:** Force stops a process if it is frozen.

## 4. Windows CLI Basics

## Windows CLI Basics

In this room, I learnt about the essential commands used in the Windows Command Prompt (cmd). Here is the list of commands and what they do:

* **`cd`:** Shows my current folder so I know where I am, and is also used to change into a different folder.
* **`dir`:** Shows the contents of the current directory.
* **`dir /a`:** Shows all files, including the hidden ones (the `/a` stands for attributes).
* **`cd folder_name`:** Changes the directory to a specific folder.
* **`cd ..`:** Goes back up one level.
* **`dir /s task.txt`:** Searches for a specific file (the `/s` tells Windows to search all subfolders).
* **`type task.txt`:** Reads the contents of a file and prints it to the screen.
* **`cls`:** Clears the screen.
* **`whoami`:** Prints the username of the account currently logged in.
* **`hostname`:** Shows the computer's name.
* **`systeminfo`:** Prints detailed information about the PC.
* **`ipconfig`:** Displays basic network configuration information.

## 5. ## Operating System Security

This was a very important and interesting lecture! In this room, I learnt about the core concepts of OS security, why it matters, and how attackers try to break in.

### Hardware vs. Operating System
* **Hardware:** The physical parts of the computer I can actually touch (CPU, RAM, motherboard). But hardware is useless on its own.
* **Operating System (OS):** The middleman sitting between the hardware and my apps (like Firefox or WhatsApp). The OS controls the hardware and allows programs to run according to specific rules. There are OSes for PCs (Windows, macOS), phones (Android, iOS), and servers (Linux, Windows Server).

### What We Are Protecting (The CIA Triad)
Because our devices hold highly sensitive data (passwords, private photos, work documents, banking info), the OS must enforce security to protect three main things:
* **Confidentiality:** Ensuring my secret files are only seen by the people I intend.
* **Integrity:** Ensuring nobody can secretly tamper with or alter my files.
* **Availability:** Ensuring my computer and files are accessible and ready to use whenever I need them.

### Three Major Security Weaknesses
I learnt that attackers generally target these three weaknesses:

1. **Authentication and Weak Passwords:** Authentication proves who I am (by something I know, am, or have). Because people use terrible, predictable passwords (like `123456`, `password`, or keyboard patterns like `qwerty`), attackers can easily guess them and steal data. Complex, unique passwords are a must.
2. **Weak File Permissions:** Good security relies on the "principle of least privilege" (users should only have access to what they absolutely need). Weak permissions ruin confidentiality (attackers can read my files) and integrity (attackers can modify my files).
3. **Malicious Programs:** Malware attacks the system in various ways. Trojans give attackers backdoor access. Ransomware completely destroys *availability* by encrypting all my files into gibberish and demanding money to give me the decryption password.

### Privilege Escalation
Once an attacker guesses a weak password and gets into a remote system, their next goal is to escalate their privileges to take full control. 
* On Linux, Android, and Apple, this unrestricted super-account is called **`root`**. 
* On Windows, it is called **`Administrator`**.

### Hands-On: Linux Security Commands
I actually practiced attacking a remote Linux machine and learned these essential terminal commands during the process:
* **`ssh USERNAME@10.113.183.60`:** Secure Shell. Used to remotely log into another computer over the network. (I noted that when typing the password over SSH, it is completely invisible—no stars or dots show up, but the system still receives it).
* **`whoami`:** Prints out the name of the user I am currently logged in as.
* **`ls`:** Lists all the non-hidden files in my current directory.
* **`cat FILENAME`:** Short for concatenate; it reads a text file and prints its contents directly to the screen.
* **`history`:** Prints out a list of all the previous commands I typed.
* **`su - johnny`:** Switch User. Allows me to switch my current terminal session to another user account (like johnny) if I know their password.
