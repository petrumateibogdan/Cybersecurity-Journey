# Module 12: Defense Security Tooling

# 1. CyberChef: The Basics


CyberChef is a simple, intuitive web-based application designed to help with various "cyber" operation tasks within your web browser. Think of it as a Swiss Army knife for data—a toolbox designed for specific tasks ranging from simple encodings (like XOR or Base64) to complex operations (like AES encryption or RSA decryption). CyberChef operates on **recipes**, which are a series of operations executed in order.

---

## The Four Core Areas of CyberChef

CyberChef consists of four primary areas, each serving a specific function in your data processing workflow.

###  The Operations Area
This is a comprehensive repository of all the diverse operations CyberChef can perform, meticulously categorized for convenient access. You can use the search feature to locate specific operations quickly. Hovering over an operation provides a sample, description, and a link to Wikipedia for further reading.

**Common Operations Examples:**
| Operation | Description | Example |
| :--- | :--- | :--- |
| **From Morse Code** | Translates Morse Code into alphanumeric characters. | `- .... .-. . .- - ...` becomes `THREATS` |
| **URL Encode** | Encodes problematic characters into percent-encoding for URIs/URLs. | `https://tryhackme.com` becomes `https%3A%2F%2Ftryhackme%2Ecom` |
| **To Base64** | Encodes raw data into an ASCII Base64 string. | `This is fun!` becomes `VGhpcyBpcyBmdW4h` |
| **To Hex** | Converts the input string to hexadecimal bytes. | `This Hex conversion is awesome!` becomes `54 68 69...` |
| **To Decimal** | Converts the input data to an ordinal integer array. | `This Decimal conversion...` becomes `84 104 105...` |
| **ROT13** | A Caesar substitution cipher rotating characters by 13. | `Digital Forensics...` becomes `Qvtvgny Sberafvpf...` |

###  The Recipe Area
Considered the heart of the tool. Here, you can select, arrange, and fine-tune operations. You drag operations into this area and define their precise arguments and options.

**Key Features:**
* **Save recipe:** Save selected operations for future use.
* **Load recipe:** Load previously saved recipes.
* **Clear Recipe:** Clear the chosen operations during usage.
* **BAKE! Button:** Processes the data using the current recipe setup.
* **Auto Bake:** Automatically processes the data as you modify the recipe without needing to manually click "BAKE!".

###  The Input Area
A user-friendly space to input text or files by pasting, typing, or dragging.

**Key Features:**
* **Add a new input tab:** Create a new tab to work with different values simultaneously.
* **Open folder as input:** Upload an entire folder.
* **Open file as input:** Upload a specific file.
* **Clear input and output:** Clears the current input values and their corresponding output.
* **Reset pane layout:** Restores the interface to default window sizes.

###  The Output Area
The visual space showcasing the results of your data processing and manipulations.

**Key Features:**
* **Save output to file:** Save the results as a `.dat` file.
* **Copy raw output to clipboard:** Quickly copy results for use in other applications or documents.
* **Replace input with output:** Overwrite your initial input data with the processed results.
* **Maximise output pane:** Expands the output view.

---

## The CyberChef Thought Process

Before diving in, it is helpful to follow a structured four-step thought process:

1. **Set a Clear Objective:** Define what you want to accomplish (e.g., "I found a gibberish string and want to decode it to find a hidden message").
2. **Input Data:** Paste or upload your target data into the Input Area.
3. **Select Operations:** Choose the appropriate tools for the job. If you suspect encryption, search for categories like *Encryption/Encoding* and try operations like `ROT13`, `Base64`, or `Base85`.
4. **Check Output:** Evaluate the result. Did you achieve your objective? If not, adjust your operations and try again.

---

## Common Operation Categories

### Extractors
Used to pull specific, formatted data from large blocks of text.

| Operation | Description |
| :--- | :--- |
| **Extract IP addresses** | Extracts all valid IPv4 and IPv6 addresses. |
| **Extract URLs** | Extracts Uniform Resource Locators (URLs). *Note: Protocols like HTTP/FTP are required to reduce false positives.* |
| **Extract email addresses** | Extracts all strings formatted as `anything@domain.com`. |

### Date and Time
Used for timestamp conversions.

| Operation | Description |
| :--- | :--- |
| **From UNIX Timestamp** | Converts a UNIX timestamp (seconds since Jan 1, 1970 UTC) to a readable datetime string. |
| **To UNIX Timestamp** | Parses a UTC datetime string and returns the corresponding UNIX timestamp. |

### Data Format (Base Encodings)
Base encoding transforms binary data into text-based representations using specific character sets (ASCII).

| Operation | Description | Example |
| :--- | :--- | :--- |
| **From Base64** | Decodes data from an ASCII Base64 string back into raw format. | `V2VsY29tZ...` becomes `Welcome...` |
| **URL Decode** | Converts percent-encoded characters back to their raw values. | `%3A%2F%2F` becomes `://` |
| **From Base85** | Decodes arbitrary byte data using a highly efficient notation. | `BOu!rD]j7BEbo7` becomes `hello world` |
| **From Base58** | Decodes data removing misread characters (l, I, 0, O) for human readability. | `AXLU7qR` becomes `Thm58` |
| **To Base62** | Encodes data using a restricted, computer/human-friendly symbol set. | `Thm62` becomes `6NiRkOY` |

---

## Deep Dive: Manual Base64 Encoding

To understand how CyberChef works under the hood, here is how you manually convert the string **"THM"** into Base64.

**Reference ASCII Decimal/Binary Values:**
*   **T** = 84 (Decimal) = `01010100` (Binary)
*   **H** = 72 (Decimal) = `01001000` (Binary)
*   **M** = 77 (Decimal) = `01001101` (Binary)

**Step 1: Convert to Binary and Merge**
Concatenate the binary values into a single 24-character string:
`010101000100100001001101`

**Step 2: Divide and Convert to Decimal**
Separate the 24-character string into blocks of 6 bits, then convert each 6-bit block back into a decimal number:

| 6-Bit Binary | Decimal (Base 10) |
| :--- | :--- |
| 010101 | 21 |
| 000100 | 4 |
| 100001 | 33 |
| 001101 | 13 |

**Step 3: Convert to Base64**
Map the resulting decimal numbers to the standard Base64 Index Table.

| Decimal Index | Base64 Character |
| :--- | :--- |
| 21 | **V** |
| 4 | **E** |
| 33 | **h** |
| 13 | **N** |

**Result:** The string **"THM"** encoded in Base64 is **`VEhN`**.

---

## Quick Reference: Common URL Percent-Encodings
When working with the **URL Decode** operation, it is helpful to recognize common UTF-8 percent-encoded characters:

| Symbol | Percent-Encoded |
| :--- | :--- |
| `:` | `%3A` |
| `/` | `%2F` |
| `.` | `%2E` |
| `=` | `%3D` |
| `#` | `%23` |


# 2. CAPA: The Basics

CAPA: Automated Static Malware AnalysisCAPA (Common Analysis Platform for Artifacts) is an automated static analysis tool developed by the FireEye Mandiant team. It is designed to identify the capabilities present in executable files—such as Portable Executables (PE), ELF binaries, .NET modules, shellcode, and sandbox reports—by analyzing the file against a set of rules describing common malicious behaviors.By encapsulating years of reverse engineering knowledge, CAPA allows security professionals to quickly determine a program's capabilities (e.g., network communication, file manipulation, process injection, evasion) without manually reverse engineering the code.1. Command Line Usage and ParametersCAPA is executed via the command line (PowerShell or Bash) by pointing the tool to the target binary.Basic SyntaxPowerShellcapa.exe .\cryptbot.bin

Essential ParametersParameterDescriptionExample Syntax-h or --helpDisplays the help message and available options.capa -h-v or --verboseEnables a detailed verbose result document.capa.exe -v .\cryptbot.bin-vv or --vverboseEnables a very verbose document showing exact rule matches.capa.exe -vv .\cryptbot.bin-jOutputs the results in JSON format (ideal for Web Explorer).capa.exe -j -vv .\cryptbot.bin > output.json2. Dissecting the CAPA OutputCAPA categorizes its findings into highly structured blocks mapping the binary's behavior to standardized cybersecurity frameworks.Basic InformationDisplays foundational file details including cryptographic hashes (MD5, SHA1, SHA256), the analysis method (static), the operating system, the architecture (e.g., i386), and the file format (e.g., pe).MITRE ATT&CK MappingCAPA maps the file's discovered capabilities to the MITRE ATT&CK adversary playbook, helping defenders understand the tactics and techniques being utilized.Format: ATT&CK Tactic :: ATT&CK Technique :: Sub-Technique [Identifier]Example: DEFENSE EVASION :: Obfuscated Files or Information :: Indicator Removal from Tools [T1027.005]MAEC (Malware Attribute Enumeration and Characterization)MAEC is a specialized language used to encode and communicate complex details concerning malware.Launcher: The file exhibits behaviors such as dropping additional payloads, activating persistence mechanisms, connecting to C2 servers, or executing specific functions.Downloader: The file fetches additional payloads/resources from the internet, pulls updates, or retrieves configuration files.3. Malware Behavior Catalogue (MBC)MBC serves as a catalog of malware objectives and behaviors that complements the MITRE ATT&CK framework, tailored specifically for malware analysis and characterization.Format: OBJECTIVE :: Behavior :: Method [Identifier]ComponentExampleExplanationObjectiveDATABroad goals (e.g., checking strings, compressing, decoding, or encoding data).Behavior / Micro-BehaviorEncode DataSpecific actions or low-level actions (e.g., encoding data using Base64 or XOR).MethodBase64Sub-technique indicating how the behavior is achieved.Identifier[C0026.001]The unique tag linking to the MBC catalog.Common MBC ObjectivesAnti-Behavioral Analysis: Attempts to avoid detection by hindering sandboxes or debuggers (e.g., Lab Machine Detection [B0009]).Anti-Static Analysis: Obstructs static analysis to conceal intentions (e.g., Executable Code Obfuscation [B0032]).Execution: Exploits command and script interpreters (e.g., Command and Scripting Interpreter [E1059]).Discovery: Enumerates files and directories to gather target information (e.g., File and Directory Discovery [E1083]).4. Capabilities and NamespacesThis block outlines the specific rules triggered by the binary and groups them logically.Capability: The specific rule name that was matched (e.g., reference anti-VM strings targeting VMWare). This translates directly to the underlying .yml rule file name (e.g., reference-anti-vm-strings-targeting-vmware.yml).Top-Level Namespace (TLN): The broad category of the rule (e.g., anti-analysis, host-interaction, communication, data-manipulation).Namespace: The specific sub-category within the TLN (e.g., anti-vm/vm-detection).Nursery (Exception): Rules that are not yet fully polished are placed in the nursery TLN rather than their expected logical TLN.Top-Level Namespace (TLN) ExamplesTLNExplanationanti-analysisRules designed to detect evasion behaviors like obfuscation, packing, anti-debugging, and VM detection.communicationRules pertaining to data transmission, C2 communications, and network behaviors (HTTP, DNS, etc.).data-manipulationBehaviors altering data within executables, such as string encryption or data encoding (XOR, Base64).host-interactionBehaviors involving the host system (reading, writing, modifying files, registries, or processes).impactPotential consequences or harm the malware can cause (destruction, cryptocurrency mining, remote access).5. CAPA Web Explorer (Very Verbose Analysis)When utilizing the -vv (very verbose) parameter, CAPA generates extensive output explaining exactly why a rule matched. Because terminal output can easily exceed thousands of lines, analysts use the CAPA Web Explorer for visual analysis.Workflow for Web ExplorerGenerate a JSON report:PowerShellcapa.exe -j -vv .\cryptbot.bin > cryptbot_vv.json
Open CAPA Web Explorer: Access the online CAPA Explorer or use a local HTML copy.Upload the Report: Click "Upload from local" in the bottom left and select your cryptbot_vv.json file.Analyze Rule Logic: Click on specific capabilities to view the underlying YAML rule and the exact regex strings matched within the binary. Use the global search box to filter results efficiently.Example: Rule Matching Logic (Anti-VM)If you click on the capability reference anti-VM strings targeting VMWare inside the Web Explorer, you will see the exact strings CAPA found inside the binary's code that triggered the alert. This confirms that the malware is actively searching for VMware artifacts to evade sandboxes.YAML  features:
    - or:
      - string: /VMWare/i
      - string: /VMTools/i
      - string: /SOFTWARE\\VMware, Inc\.\\VMware Tools/i
      - string: /vmnet\.sys/i
      - string: /vmmouse\.sys/i
      - string: /vmtoolsd\.exe/i
Example: Rule Matching Logic (Persistence)If you inspect the capability schedule task via schtasks, the Web Explorer reveals the logic requiring specific process creation commands paired with task scheduling strings:YAML  features:
    - and:
      - match: host-interaction/process/create
      - or:
        - and:
          - string: /schtasks/i
          - string: /\/create /i
        - string: /Register-ScheduledTask /i

