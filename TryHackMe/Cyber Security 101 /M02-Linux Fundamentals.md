# Module 2: Linux Fundamentals

## Overview of Linux
* **Linux vs. Windows:** Linux is significantly more lightweight compared to operating systems like Windows.
* **Daily Usage:** Linux powers everyday technologies in various forms, including:
  * Websites you visit
  * Car entertainment and control panels
  * Point of Sale (PoS) systems such as checkout tills and registers
  * Critical infrastructures like traffic light controllers and industrial sensors
  * Phones and small computing devices

---

## 2. Deploying a Linux Machine
* Rooms often provide a browser-based Ubuntu Linux machine that can be accessed using the "Start Machine" or "Start Lab Machine" buttons.
* The management card displays essential information such as the IP address and expiry timer. 
* Always remember to click "Terminate" once you are finished with the room.

---

## 3. The Linux Terminal & Basic Commands
* Cybersecurity workflows rely heavily on the command line interface (CLI) rather than a mouse, from running hacking tools to tracking attackers.
* **Command:** An instruction given to the computer to perform a specific task.
* **`whoami`:** Tells you your current username on the system, which is critical when changing users to verify permissions.
* **`echo`:** Outputs specific text provided to it (e.g., `echo TryHackMe` or multiple words wrapped in quotes like `echo "hello world"`).
* **Pro Tip:** Press the up and down arrow keys on your keyboard in the terminal to scroll through previously entered commands.

---

## 4. File Navigation & Searching
Essential commands for navigating the file system without a mouse include:
* **`ls`:** List what is in the current folder. (Note: Folders typically display as blue on these systems).
* **`cd`:** Change directory—move into a specified folder.
* **`cat`:** Show the contents of a file.
* **`pwd`:** Print working directory—shows "where am I?".

Efficient searching commands:
* **`find`:** Search for files by their name (e.g., `find -name passwords.txt`).
* **`grep`:** Searches inside files for specific text (stands for *global regular expression print*). For example, running `grep "THM" access.log` searches through log files to extract matching lines like a flag (`THM{...}`).

---

## 5. Operators and Redirection
Special characters can combine commands or handle output routing:
* **`&`:** Runs the command in the background without waiting for it to finish. Useful for long-running processes.
* **`&&`:** Runs both commands sequentially, waiting for the first command to finish before executing the next.
* **`>` (Redirection):** Takes the output of a command and sends it to a file, overwriting any existing content. For example, `echo "TryHackMe" > thm` creates or overwrites the file `thm`.
* **`>>` (Append Redirection):** Adds output to the bottom of an existing file without replacing previous text. For example, using `>>` with the text `"thm"` appends it, which can be verified using `cat thm`.
