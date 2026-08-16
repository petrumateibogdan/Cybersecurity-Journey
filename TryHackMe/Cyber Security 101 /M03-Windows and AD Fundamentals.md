# Module 3: Windows and AD ( Active Directory )  Fundamentals

## 1. Windows Fundamentals 1


# Windows Fundamentals Overview

## Overview
Windows is the dominant operating system in home and corporate environments, making it a primary target for malware. Its history spans from **1985** early versions like XP and Vista, up through Windows 10, Windows 11, and Windows Server 2025. This module covers the core components, file systems, permissions, and management utilities essential for understanding the Windows environment.

---

##  The Windows GUI
The Windows Desktop interface is composed of several key components that facilitate user interaction:
* **The Desktop:** The main workspace for shortcuts to programs, folders, and files. Right-clicking allows for personalization (wallpaper, themes) and display settings (resolution, multi-monitor setups).
* **The Start Menu:** Divided into three main sections:
  * **Shortcuts:** Quick access to account settings, documents, pictures, and power options.
  * **App List:** An alphabetical list of recently added and installed applications.
  * **Tiles:** Customizable shortcuts for specific apps or utilities on the right side.
* **The Taskbar & Notification Area:** Displays open applications and pinned programs. The Notification Area (bottom right) shows the date, time, volume, network status, and background utilities.

---

##  NTFS (New Technology File System)
NTFS is the modern file system used by Windows, replacing older FAT16/32 formats.
* **Journaling:** NTFS can automatically repair files and folders on disk using a log file in the event of a system failure.
* **Key Features:** Supports files larger than 4GB, folder/file compression, and encryption (EFS). (Encrypting File System)
* **NTFS Permissions:** Access can be explicitly granted or denied using specific permissions:
  * Full control
  * Modify
  * Read & Execute
  * List folder contents
  * Read
  * Write
* **Alternate Data Streams (ADS):** A feature that allows files to contain more than one stream of data (every file has at least a `$DATA` stream). While Windows Explorer hides ADS, it is used legitimately (e.g., identifying files downloaded from the internet) but can also be abused by malware to hide malicious code.

---

## Core Directories & Environment Variables
* **The Windows Folder (`C:\Windows`):** Houses the core operating system files. Its location is mapped to the `%windir%` system environment variable.
* **System32 (`C:\Windows\System32`):** Contains the most critical files for the operating system. Modifying or deleting files here without extreme caution can render the OS completely inoperable.

---

##  User Accounts & Management
Local Windows systems typically utilize two main account types:
* **Administrator:** Has full privileges to make system-wide changes, add/remove users, and modify security settings.
* **Standard User:** Can only make changes to their specific profile files and cannot perform system-level changes (like installing global software).
* **User Profiles:** Located in `C:\Users\<username>`, created upon the initial login, and contain default folders like Desktop, Documents, and Downloads.
* **lusrmgr.msc:** The Local Users and Groups Management utility, used to assign users to specific groups, allowing them to inherit the group's specific permissions.

---

##  User Account Control (UAC)
UAC is a critical security feature designed to protect administrators from accidentally executing malicious actions.
* **Function:** When an administrator attempts to perform an operation requiring elevated privileges, UAC intercepts the request and prompts the user for confirmation.
* **Identification:** Programs that require elevated privileges to run or install are marked with a small shield icon.
* **Security Benefit:** Because malware runs in the context of the logged-in user, UAC prevents malicious code from automatically executing system-level changes without explicit human approval.

---

##  System Configuration & Monitoring
* **Settings vs. Control Panel:** Both are used to configure the system. The Settings menu (introduced in Windows 8) provides a modernized UI for primary configurations, while the legacy Control Panel handles more complex, deep-level system settings (like the "Programs and Features" menu to view installed applications).
* **Task Manager:** Accessed by right-clicking the taskbar, it provides real-time data on running applications, background processes, and system performance utilization (CPU, RAM, Disk, Network).


## 2. Windows Fundamentals 2


## Overview
This section explores the core utilities used by administrators and power users to configure, troubleshoot, and manage a Windows environment. These tools provide deep visibility into hardware performance, background services, network connections, and automated tasks.

---

##  System Configuration (MSConfig)
MSConfig is an advanced troubleshooting utility primarily used to diagnose startup issues. It requires local administrator rights to open.
* **General:** Select what devices and services load upon boot (Normal, Diagnostic, or Selective).
* **Boot:** Define various boot options for the operating system.
* **Services:** Lists all background services configured on the system (both running and stopped).
* **Startup:** On modern Windows (10/11), this redirects to Task Manager. On Windows Servers, user-level startup items are reliably managed via the Startup folder (accessible by running `shell:startup`).
* **Tools:** Provides a list of shortcuts to launch other system administration utilities.

---

##  Advanced System Settings
This menu provides control over system performance behavior and crash recovery.
* **Performance (Page File):** Windows uses a "page file" as virtual memory when physical RAM is full to prevent crashes. Here, you can view or modify the page file's size and the drive where it is stored.
* **Startup and Recovery:** Configures "crash dump" files, which are generated during a critical error (like a Blue Screen of Death). Administrators use these memory dumps (e.g., Kernel, Small, or Complete memory dumps) to investigate the cause of the crash.

---

##  User Account Control (UAC) Settings
UAC controls how Windows alerts you when applications or users attempt to make system-level changes. It has four security levels:
1. **Always notify:** Highest security; notifies for any change and dims the desktop (Secure Desktop).
2. **Notify for apps (Default):** Notifies only when apps try to make changes; desktop dims.
3. **Notify without dimming:** Same as default, but the screen does not dim.
4. **Never notify:** Lowest security; no warnings are issued for any changes.

---

##  Computer Management (`compmgmt.msc`)
A consolidated console containing three primary sections: System Tools, Storage, and Services and Applications.

### System Tools
* **Task Scheduler:** Automates tasks (running apps or scripts) at specified times, on login, or on recurring schedules. 
* **Event Viewer:** An audit trail of system activity used for troubleshooting and security investigations. Logs include Application, Security, Setup, System, and Forwarded Events. Event types range from Information to Critical Errors and Audit Success/Failures.
* **Shared Folders:** Displays all active network shares (including default admin shares like `C$` and `ADMIN$`), connected user sessions, and open files.
* **Performance Monitor (`perfmon`):** Views real-time or log-based hardware performance data.
* **Device Manager:** Views and configures attached hardware components.

### Storage & Services
* **Disk Management:** Advanced storage tasks, such as setting up drives, extending/shrinking partitions, and assigning drive letters.
* **Services:** Manages background applications. The "Startup type" determines how a service launches: **Automatic** (on boot), **Manual** (triggered by a user/process), or **Disabled**.

---

##  System Information (`msinfo32`)
A comprehensive view of your hardware, system components, and software environment. 
* Displays granular hardware specifications.
* **Environment Variables:** Stores data used by the OS and programs (e.g., `%windir%` points to the Windows installation directory). 

---

##  Resource Monitor (`resmon`)
An advanced utility providing granular, real-time, per-process usage statistics.
* **Tabs:** CPU, Memory, Disk, and Network.
* **Features:** Allows administrators to isolate specific applications to see their exact resource drain, analyze deadlocked processes, and force-close unresponsive applications.

---

##  The Command Prompt (`cmd`)
While the GUI is dominant, the command line remains a powerful tool for retrieving system and network information.
* `hostname`: Outputs the name of the computer.
* `whoami`: Outputs the name of the currently logged-in user.
* `ipconfig`: Displays the network address settings.
* `netstat`: Displays active TCP/IP network connections and protocol statistics. (Accepts parameters like `-a` or `-b`).
* `net`: Used to manage network resources (e.g., `net user`, `net share`).
* `/?`: Appending this to a command (e.g., `ipconfig /?`) opens the help manual detailing its syntax. (For `net`, use `net help`).
* `cls`: Clears the command prompt screen.

---

##  Windows Registry (`regedit`)
The central, hierarchical database that stores configurations for users, applications, and hardware devices. 
* It contains user profiles, property sheet settings, and installed application data.
* **Warning:** The registry is highly sensitive. Incorrectly editing keys or values can cause critical system failures.


## 3. Windows Fundamentals 3
