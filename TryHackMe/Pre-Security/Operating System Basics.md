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
## Windows Basics

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

