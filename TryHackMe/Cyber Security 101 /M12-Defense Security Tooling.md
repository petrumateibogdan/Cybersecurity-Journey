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
