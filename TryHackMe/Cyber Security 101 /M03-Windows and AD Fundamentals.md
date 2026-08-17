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

# Windows Security and System Protection

## Overview
This module explores built-in Windows features designed to keep systems secure and up-to-date. It covers how Microsoft delivers updates, the comprehensive protections offered by Windows Security (Defender), and data preservation features like Volume Shadow Copies.

---

##  Windows Update
Windows Update is a critical service provided by Microsoft to deliver security updates, feature enhancements, and patches for the OS and associated products like Microsoft Defender.
* **Patch Tuesday:** Updates are typically released on the 2nd Tuesday of each month. However, urgent or critical patches can be pushed out at any time via the update service.
* **Update Enforcement:** Historically, users would delay updates to avoid system reboots. Starting with Windows 10, Microsoft mandated updates; they can be postponed temporarily but can no longer be completely ignored. They will eventually install and force a reboot to ensure device security.
* **Access:** It can be found in the Settings menu or launched via the Run dialog/CMD using `control /name Microsoft.WindowsUpdate`.

---

##  Windows Security (Microsoft Defender)
Windows Security serves as the central hub for managing tools that protect your device and data. It uses color-coded status icons:
* **Green:** Device is protected; no actions needed.
* **Yellow:** Safety recommendation requires review.
* **Red:** A warning requiring immediate attention.

### A. Virus & Threat Protection
Divided into two main areas:
* **Current Threats:** Allows you to run different scans:
  * *Quick Scan:* Checks common threat locations.
  * *Full Scan:* Checks all files and running programs (can take over an hour).
  * *Custom Scan:* Allows checking specific files/locations.
  * *Threat History:* Displays quarantined threats (isolated and prevented from running) and allowed threats (items identified as threats but explicitly permitted by the user).
* **Manage Settings:**
  * *Real-time Protection:* Locates and stops malware from running. (Note: Often disabled in secure VMs to improve performance, but critical for personal devices).
  * *Controlled Folder Access:* Prevents unauthorized changes by unknown/malicious apps to protected files, folders, and memory areas. Must be manually enabled.
  * *Exclusions:* Allows specific files/folders to bypass scanning to prevent false positives. Should only be used if 100% certain the excluded item is safe.
* **On-Demand Scanning:** You can right-click any file or folder and select "Scan with Microsoft Defender."

### B. Firewall & Network Protection
A firewall controls what traffic is allowed to pass in and out of device ports, acting like a security guard checking IDs. Windows Firewall offers three profiles:
* **Domain:** Applies when a host can authenticate to a domain controller.
* **Private:** User-assigned profile for trusted home/private networks.
* **Public:** Default profile for untrusted networks like coffee shop Wi-Fi.
* **Management:** You can turn the firewall on/off, block all incoming connections, or specifically "Allow an app through firewall." (Advanced configuration shortcut: `WF.msc`).

### C. App & Browser Control
Features Microsoft Defender SmartScreen, which protects against phishing/malware websites and potentially malicious file downloads by checking for unrecognized apps on the web. It can be set to Warn, Block, or Off.
* **Exploit Protection:** Built-in safeguards to protect against specific attack vectors. (Default settings should rarely be changed).

### D. Device Security
* **Core Isolation (Memory Integrity):** Prevents attacks from inserting malicious code into high-security processes.
* **Trusted Platform Module (TPM):** A secure, hardware-based crypto-processor chip designed to carry out cryptographic operations and resist physical tampering.
* **BitLocker:** A drive encryption feature that integrates with the OS to prevent data theft from lost or stolen computers. 
  * It provides maximum protection when paired with a TPM chip (version 1.2 or later). 
  * On systems *without* a TPM chip, BitLocker requires a removable drive (like a USB) containing a **startup key** to unlock the drive.

---

##  Volume Shadow Copy Service (VSS)
The Volume Shadow Copy Service (VSS) coordinates the creation of consistent shadow copies (snapshots or point-in-time copies) of data to be backed up.
* **Storage Location:** Shadow copies are stored in the "System Volume Information" folder on each protected drive.
* **Capabilities:** If System Protection is turned on, you can create restore points, perform system restores, and manage existing restore points from the advanced system settings.
* **Security Threat:** Ransomware authors specifically write code to seek out and delete Volume Shadow Copies to prevent victims from recovering their data without paying the ransom. Therefore, offline or off-site backups are essential.


## 4. Active Directory (AD)  Basics 


# Overview
Active Directory (AD) is the central backbone of identity and access management in modern corporate Windows environments. It eliminates the need to configure standalone computers by centralizing user, machine, and policy management into a single, scalable domain managed by a Domain Controller.

---

##  Core Concepts
* **Windows Domain:** A centralized group of users and computers under the administration of a single business.
* **Domain Controller (DC):** The server that runs the Active Directory Domain Service (AD DS) and manages the network.
* **Active Directory Domain Service (AD DS):** Acts as a catalogue holding network "objects" like users, groups, machines, and printers.
* **Centralized Management:** AD allows IT to manage all user identities and deploy security policies across the entire network from a single repository.

---

##  Active Directory Objects
Objects within AD are generally categorized as "security principals," meaning they can be authenticated and assigned privileges.

### A. Users
* **People:** Represent human employees requiring network access.
* **Services:** Specific accounts used to run applications like IIS or MSSQL, operating with restricted privileges specific to their service.

### B. Machines
* Every computer joined to the domain receives a machine account object.
* Machine accounts act as local administrators on their respective computers.
* Machine account names end with a dollar sign (e.g., `DC01$`).
* Their passwords are automatically rotated and consist of 120 random characters.

### C. Security Groups
Used to assign access rights to resources (like files or printers) to entire groups of users, allowing members to inherit the group's privileges. Notable default groups include:
* **Domain Admins:** Have administrative privileges over the entire domain.
* **Server Operators:** Can administer Domain Controllers but cannot alter administrative group memberships.
* **Backup Operators:** Can access any file (bypassing permissions) to perform data backups.
* **Domain Users/Computers/Controllers:** Catch-all groups for all existing accounts, machines, and DCs in the domain.

---

##  Organizational Units (OUs) vs. Security Groups
While both classify users and computers, they serve distinct purposes:
* **Organizational Units (OUs):** Container objects used to classify users and machines to apply specific policies (GPOs). OUs often mimic a business's departmental structure. *Note: A user can only belong to one OU at a time.*
* **Security Groups:** Used explicitly to grant permissions over resources. A user can belong to multiple security groups.

### OU Management & Delegation
* **Accidental Deletion Protection:** By default, OUs cannot be deleted. To delete an OU, "Advanced Features" must be enabled in the View menu, and the protection checkbox must be disabled in the object's properties.
* **Delegation:** AD allows Domain Administrators to grant specific users control over specific OUs. For example, IT support can be delegated the right to reset passwords for the Sales department without needing full Domain Admin privileges.

---

## 4. Group Policy Objects (GPOs)
GPOs are collections of settings used to push configurations and security baselines to OUs.
* **Application:** GPOs can target users or computers. A GPO applied to an OU will also affect all its child OUs.
* **Distribution:** GPOs are synced to computers via a network share called `SYSVOL`, stored on the DC (`C:\Windows\SYSVOL\sysvol\`).
* **Forcing Updates:** While computers sync periodically, you can force an immediate GPO update by running `gpupdate /force` in PowerShell.

---


##  Domain Authentication Protocols
When authenticating to a service, the service verifies domain credentials against the Domain Controller.

### Kerberos (Default)
The modern, default authentication protocol using a ticketing system.
1. **TGT Request:** The user sends their username and an encrypted timestamp to the Key Distribution Center (KDC).
2. **TGT Issued:** The KDC returns a Ticket Granting Ticket (TGT) and a Session Key. (The TGT is encrypted with the `krbtgt` account hash).
3. **TGS Request:** To access a specific service, the user presents the TGT to request a Ticket Granting Service (TGS) ticket.
4. **TGS Issued:** The KDC sends back the TGS, encrypted with a key derived from the specific Service Owner Hash.
5. **Access:** The user presents the TGS to the service, which decrypts it using its own hash to authenticate the user.

### NetNTLM (Legacy)
A legacy challenge-response protocol kept for compatibility.
1. The client requests access from a server.
2. The server sends a random number (challenge) back to the client.
3. The client combines their NTLM password hash with the challenge and sends the response to the server.
4. The server forwards this to the DC.
5. The DC recalculates the expected response and compares it. If it matches, the DC tells the server the user is authenticated.
*Note: The user's password/hash is never transmitted over the network.*

---

##  Scaling: Trees and Forests
As companies grow, a single domain may no longer suffice.
* **Trees:** If a domain splits (e.g., `uk.thm.local` and `us.thm.local`), they can be joined into a Tree. They share the same namespace but can have independent DCs, policies, and local Domain Admins.
* **Forests:** The union of several trees with completely different namespaces (e.g., merging `thm.local` and `mht.local` after a corporate acquisition).
* **Enterprise Admins:** A specialized group granting administrative privileges over *all* domains within the entire enterprise tree/forest.

### Trust Relationships
Trusts connect domains within trees and forests, allowing them to authorize users from other domains.
* **One-Way Trust:** If Domain AAA trusts Domain BBB, a user from BBB can access resources on AAA (the access flows opposite to the trust direction).
* **Two-Way Trust:** Both domains mutually authorize users. (This is the default when joining domains under a tree/forest).
* *Note: A trust does not automatically grant access to everything; it simply enables the ability to authorize specific cross-domain access.*
