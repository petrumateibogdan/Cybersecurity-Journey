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

### 3. Python: Simple Demo

## TryHackMe: Python - Simple Demo

**Objective:** 
Analyze a basic Python program provided by TryHackMe. This was my first real exposure to Python, and I used a provided "Guess the Number" game script to understand how loops, user input, and conditional logic operate under the hood; python being a high-level, general purpose, programming language.

### The Code Provided by TryHackMe
```python
import random

secret = random.randint(1, 20)
tries = 0
guess = 0

print("I'm thinking of a number between 1 and 20")

while guess != secret:
    text = input("Take a guess: ")
    guess = int(text)
    
    tries = tries + 1

    if guess < 1 or guess > 20:
        print("That number is out of range. Try again.")
    elif guess < secret:
        print("Too low, try again.")
    elif guess > secret:
        print("Too high, try again.")
    else:
        print("You got it in", tries, "tries!")
```

### Concepts Analyzed
* **Importing Modules (`import random`):** Python has built-in toolkits you can bring into a script. Importing `random` gives the program the ability to generate the secret number using `random.randint(1, 20)`.
* **Variables:** The script sets up variables like `secret`, `tries`, and `guess` to act as storage containers. It updates these containers as the game progresses, like adding 1 to `tries` every time the loop runs.
* **While Loops (`while guess != secret:`):** This keeps a program running indefinitely. The `while` loop forces the indented code below it to repeat over and over until the user finally guesses the exact secret number.
* **Handling User Input (`input()` and `int()`):** A crucial detail about data types: when you use `input()` to ask the user for something, Python always captures it as raw text (a string). To do math comparisons, the text must be passed through `int()` to convert it into a solid integer.
* **Conditional Logic (`if` / `elif` / `else`):** This gives the program a brain to make decisions. 
  * **`if`:** Checks the very first condition to ensure the user did not type a number outside the 1 to 20 range.
  * **`elif`:** This simply stands for "else if". It allows the script to chain multiple checks together. If the first check fails, it moves to the `elif` to see if the guess was too low, and then to the next `elif` to see if it was too high.
  * **`else`:** This is the default fallback. If the guess is not out of bounds, not too low, and not too high, the only logical conclusion is that the user guessed correctly, triggering the win message.

### 4. JavaScript: Simple Demo

## TryHackMe: JavaScript - Simple Demo

**Objective:** 
Checking out a basic JavaScript version of the "Guess the Number" program. I wanted to see how JavaScript handles user input, loops, and logic compared to Python, and I also learned the basics of running JS outside of a normal web browser.

### Running JavaScript
Before jumping into the code, I noted that JavaScript can be executed in two main ways:
1. **The Web Browser:** I can run JS directly by pressing F12 and opening the Web Developer Tools console in any browser (like Firefox).
2. **Node.js:** This lets me run JavaScript files directly from my terminal/command line, which is what I used for this specific script.

### The Code I Analyzed
```javascript
import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";
const rl = readline.createInterface({ input, output });

try {
    const secret = Math.floor(Math.random() * (20)) + 1; // 1 <= secret <= 20
    let tries = 0;
    let guess = 0; // start with a value that cannot be the secret

    console.log("I'm thinking of a number between 1 and 20");

    // Repeat until I guess the secret number.
    while (guess !== secret) {
        const text = await rl.question("Take a guess: "); // returns text (a string)
        guess = parseInt(text, 10); // convert the text to a number

        tries = tries + 1; // add 1 try

        // Give a hint using if / else if / else.
        if (guess < 1 || guess > 20) {
            console.log("That number is out of range. Try again.");
        } else if (guess < secret) {
            console.log("Too low, try again.");
        } else if (guess > secret) {
            console.log("Too high, try again.");
        } else {
            console.log("You got it in", tries, "tries!");
        }
    }
} finally {
    rl.close();
}
```

### Concepts Analyzed
* **Importing Modules:** I noticed the script imports `readline` with a `/promises` flag. This basically tells the program to wait for me to type something without freezing the entire system. It also imports standard input and output (`stdin` and `stdout`) to handle reading and displaying text.
* **Variables (`const` vs `let`):** I learned that JavaScript uses specific keywords to declare variables depending on whether they change. 
  * I use `const` for things that stay the same (like the `secret` number or the raw user `text` input for that specific turn).
  * I use `let` for things that update over time (like my `tries` and `guess`).
* **Generating Random Numbers:** It doesn't have a simple random tool like Python. Instead, I saw that it uses `Math.random()` combined with `Math.floor()` to generate and round out the secret number.
* **Strict Inequality (`!==`):** In the loop `while (guess !== secret)`, I learned that the `!==` operator means "strictly not equal". JavaScript checks to make sure both the actual value AND the data type (number vs text) are not the same. This is much safer than just using `!=`.
* **Handling Input (`parseInt`):** Just like Python grabs input as a string, `rl.question()` captures my guess as raw text. I had to use `parseInt(text, 10)` to convert that text into a solid base-10 number so the script could actually do math with it.
* **Conditional Logic and Operators:** 
  * I learned that JavaScript actually lets you write `else if` as two separate words, unlike Python's `elif`.
  * To check multiple conditions at once, JS uses `||` as the "OR" operator (e.g., `guess < 1 || guess > 20`).
* **Cleanup (`rl.close()`):** Once I win, the script uses a `finally` block to close the `readline` interface. I learned this is important to clean up the memory and properly end the command line process.
