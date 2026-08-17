# Module 4: Command Line

## 1. Windows Command Line



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
