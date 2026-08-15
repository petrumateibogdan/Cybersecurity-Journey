# Module 3: Windows and AD ( Active Directory )  Fundamentals

## 1. Windows Fundamentals 1


# Windows Fundamentals Overview

## Overview
Windows is the dominant operating system in home and corporate environments, making it a primary target for malware. Its history spans from early versions like XP and Vista, up through Windows 10, Windows 11, and Windows Server 2025. This module covers the core components, file systems, permissions, and management utilities essential for understanding the Windows environment.

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
* **Key Features:** Supports files larger than 4GB, folder/file compression, and encryption (EFS).
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
