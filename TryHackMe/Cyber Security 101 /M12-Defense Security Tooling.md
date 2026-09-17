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



Notes on CAPA (Common Analysis Platform for Artifacts), an open-source static analysis tool originally developed by the FireEye Mandiant team. Instead of manually reverse engineering a binary line by line, CAPA runs it against a huge library of rules describing known malicious behaviors and tells you what the file is *capable of doing* — network communication, file manipulation, process injection, evasion techniques, and more. It works on PE files, ELF binaries, .NET modules, raw shellcode, and even sandbox reports.

Basically: it encodes years of reverse engineering knowledge into rules, so you get a fast capability summary instead of starting from zero on every sample.

## 1. Command Line Usage

Run from PowerShell or Bash, pointing at the target binary.

**Basic syntax:**
```powershell
capa.exe .\cryptbot.bin
```

**Key flags:**

| Flag | Description | Example |
|---|---|---|
| `-h` / `--help` | Shows help message and available options | `capa -h` |
| `-v` / `--verbose` | Detailed verbose result document | `capa.exe -v .\cryptbot.bin` |
| `-vv` / `--vverbose` | Very verbose — shows exact rule matches | `capa.exe -vv .\cryptbot.bin` |
| `-j` | Output in JSON format (needed for Web Explorer) | `capa.exe -j -vv .\cryptbot.bin > output.json` |

## 2. Reading CAPA's Output

CAPA organizes what it finds into a few structured blocks, each mapped to a standardized framework:

### Basic Information
File hashes (MD5/SHA1/SHA256), analysis method (static), OS, architecture (e.g. i386), file format (e.g. pe).

### MITRE ATT&CK Mapping
Maps discovered capabilities to the MITRE ATT&CK framework so you can see the tactics/techniques in play.

Format: `ATT&CK Tactic :: ATT&CK Technique :: Sub-Technique [Identifier]`

Example: `DEFENSE EVASION :: Obfuscated Files or Information :: Indicator Removal from Tools [T1027.005]`

### MAEC (Malware Attribute Enumeration and Characterization)
A language for describing complex malware behavior at a higher level:
- **Launcher** — drops additional payloads, activates persistence, connects to C2, executes specific functions
- **Downloader** — fetches additional payloads/resources, pulls updates, retrieves config files

## 3. Malware Behavior Catalogue (MBC)

MBC complements MITRE ATT&CK but is purpose-built for malware analysis specifically — it catalogues malware *objectives* and behaviors.

Format: `OBJECTIVE :: Behavior :: Method [Identifier]`

| Component | Example | Meaning |
|---|---|---|
| Objective | DATA | Broad goal (checking strings, compressing, decoding, encoding data) |
| Behavior | Encode Data | Specific action (e.g. encoding via Base64 or XOR) |
| Method | Base64 | The exact sub-technique used |
| Identifier | [C0026.001] | Unique tag linking back to the MBC catalog |

**Common MBC objectives to know:**
- **Anti-Behavioral Analysis** — evading sandboxes/debuggers (e.g. Lab Machine Detection [B0009])
- **Anti-Static Analysis** — obstructing static analysis, e.g. code obfuscation (Executable Code Obfuscation [B0032])
- **Execution** — abusing command/script interpreters (Command and Scripting Interpreter [E1059])
- **Discovery** — enumerating files/directories to gather target info (File and Directory Discovery [E1083])

## 4. Capabilities and Namespaces

This is where CAPA lists exactly which rules matched, grouped logically:

- **Capability** — the specific rule name matched (e.g. "reference anti-VM strings targeting VMWare"), which maps directly to a `.yml` rule file (`reference-anti-vm-strings-targeting-vmware.yml`)
- **Top-Level Namespace (TLN)** — the broad category (anti-analysis, host-interaction, communication, data-manipulation, impact...)
- **Namespace** — a more specific sub-category within the TLN (e.g. `anti-vm/vm-detection`)
- **Nursery** — an exception TLN for rules that aren't fully polished yet, so they sit here instead of their "proper" category

**TLN examples:**

| TLN | Meaning |
|---|---|
| anti-analysis | Evasion behaviors — obfuscation, packing, anti-debugging, VM detection |
| communication | Data transmission, C2 communication, network behavior (HTTP, DNS, etc.) |
| data-manipulation | Altering data inside the executable — string encryption, XOR/Base64 encoding |
| host-interaction | Reading/writing/modifying files, registry keys, or processes on the host |
| impact | The actual damage potential — destruction, cryptomining, remote access |

## 5. CAPA Web Explorer (for very verbose output)

Running with `-vv` can produce thousands of lines in the terminal — way too much to read raw. The Web Explorer turns that JSON dump into something actually browsable.

**Workflow:**
1. Generate the JSON report:
```powershell
   capa.exe -j -vv .\cryptbot.bin > cryptbot_vv.json
```
2. Open CAPA Web Explorer (online version, or a local HTML copy).
3. Click **"Upload from local"** (bottom left) and select the `.json` file.
4. Click into individual capabilities to see the exact YAML rule logic and the precise regex strings that matched inside the binary. There's a global search box to filter through everything quickly.

### Example — Anti-VM rule logic

Clicking into "reference anti-VM strings targeting VMWare" shows exactly which strings inside the binary triggered the match — confirming the malware is actively searching for VMware artifacts to detect (and evade) a sandbox:

```yaml
features:
    - or:
      - string: /VMWare/i
      - string: /VMTools/i
      - string: /SOFTWARE\\VMware, Inc\.\\VMware Tools/i
      - string: /vmnet\.sys/i
      - string: /vmmouse\.sys/i
      - string: /vmtoolsd\.exe/i
```

### Example — Persistence rule logic

Inspecting "schedule task via schtasks" shows the rule requires a process-creation event paired with specific scheduling command strings:

```yaml
features:
    - and:
      - match: host-interaction/process/create
      - or:
        - and:
          - string: /schtasks/i
          - string: /\/create /i
        - string: /Register-ScheduledTask /i
```

## Takeaway

What clicked for me: CAPA isn't guessing behavior from vibes — every single capability it reports traces back to an actual YAML rule made of concrete string/regex matches or API/process-creation patterns found inside the binary. The Web Explorer is what makes that traceable — instead of just trusting a label like "anti-VM detection," you can click straight through to the exact strings that earned it that label. That's a big part of why CAPA is useful for triage: it doesn't replace manual reverse engineering, but it tells you *where to look first* by mapping raw binary behavior onto frameworks (ATT&CK, MBC) that already have a shared vocabulary across the industry.

## Quick reference

| Task | Command |
|---|---|
| Basic scan | `capa.exe .\file.bin` |
| Verbose | `capa.exe -v .\file.bin` |
| Very verbose | `capa.exe -vv .\file.bin` |
| JSON output for Web Explorer | `capa.exe -j -vv .\file.bin > output.json` |
| Help | `capa -h` |

