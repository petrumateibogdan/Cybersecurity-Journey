## Module 4: Software Basics

### 1. Data Representation


**Objective:** 
Understand how numeric values are represented and stored in computer memory, alongside how computers process and encode color.

### Concepts I Learned
* **Decimal (Base-10):** I learned that this is the standard numbering system we use in everyday life.
* **Binary (Base-2):** I learned that computers operate fundamentally on two states, which are encoded strictly as 0 and 1.
* **Hexadecimal (Base-16):** I learned that a single hexadecimal digit (ranging from 0 to F) represents exactly 4 binary digits (bits).
* **Octal (Base-8):** I learned that every 3 bits can be grouped into one octal digit (ranging from 0 to 7), though I noted this system is less commonly encountered on modern computers.
* **Bits and Bytes:** I learned that a "bit" is short for binary digit (a single 0 or 1), and a "byte" (also referred to as an octet) consists of 8 bits.
* **Hex Colors (RGB):** I learned that colors on computer systems are represented as a combination of Red, Green, and Blue. By assigning exactly one byte (8 bits) to each of these three primary colors, it allows for more than 16 million possible color combinations.

### 2. Data Encoding

**Objective:** 
Understand how computers encode, store, and display text characters across different languages and systems, transitioning from early standards like ASCII to modern universal standards like Unicode.

### Concepts I Learned
* **ASCII (American Standard Code for Information Interchange):** I learned that computers need a standard dictionary to translate binary bits into readable text. ASCII, established in 1963, is a 7-bit standard defining 128 characters (0-127) that cover English letters, digits, and basic punctuation. For example, when a computer reads the hexadecimal value "54", it knows to display an uppercase "T".
* **Extended ASCII and ISO-8859 Series:** I learned that 7 bits (128 characters) were not enough for other European languages, so an 8th bit was utilized to add 128 more characters. To manage this, regional standards were created, such as ISO-8859-1 (Latin-1 for Western Europe) and ISO-8859-2 (Latin-2 for Central/Eastern Europe). I discovered a major flaw here: if a file is saved using Latin-1 but opened by someone using Latin-2, the characters will break and display incorrectly.
* **The Need for a Universal Standard:** I learned that languages like Arabic, Japanese (with thousands of Kanji), and Chinese require vastly more characters than 8-bit encoding can provide. The continuous addition of new symbols and emoticons made regional encoding chaotic and highly incompatible.
* **Unicode:** I learned that Unicode was created to fix this fragmentation. It is a universal character encoding standard that assigns a unique code point to every single character across all modern and historical writing systems (e.g., U+0041 for the Latin letter "A"). With nearly 157,000 characters, Unicode ensures that any text, in any language, displays correctly regardless of the system opening it.
* **UTF-8:** I learned that UTF-8 is the most common encoding on the modern web. It is highly efficient because it dynamically uses 1 to 4 bytes based on the character's complexity. Standard English characters take exactly 1 byte (ensuring perfect backward compatibility with original ASCII), while complex symbols or scripts scale up to 4 bytes.
* **UTF-16:** I learned that this encoding uses either 2 or 4 bytes per character. Most common characters (Latin, Cyrillic, Chinese) fit into 2 bytes, while rarer ancient scripts or modern symbols require a pair of 16-bit units (4 bytes).
* **UTF-32:** I learned that UTF-32 is the simplest but most wasteful method. It strictly assigns exactly 4 bytes to every single Unicode character, regardless of whether it is a simple letter or a complex symbol, taking up much more memory.
* **Character Representation:** I learned how specific symbols are processed by the computer. For instance, a simple smiley face symbol maps to the code point U+0001F60A, the Chinese character for "dragon" maps to U+9F8D, and a black knight chess piece maps to U+265E. The computer reads the raw binary of these codes to show the correct graphic on the screen.
