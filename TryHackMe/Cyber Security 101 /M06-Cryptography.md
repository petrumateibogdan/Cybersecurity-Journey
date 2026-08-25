# Module 6: Cryptography

# 1. Cryptography Basics

## Cryptography Fundamentals
I learned that cryptography is the backbone of digital trust, ensuring three main pillars of secure communication:
* **Confidentiality:** No unauthorized party can read the data.
* **Integrity:** The data cannot be altered in transit without detection.
* **Authenticity:** We can verify the identity of the communicating parties.

Cryptography is heavily regulated depending on the industry. For instance, handling credit cards requires compliance with **PCI DSS (Payment Card Industry Data Security Standard) ** (encrypting data at rest and in transit), while medical records are governed by **HIPAA/HITECH** (US) or **GDPR** (EU).

---

## Core Terminology
To understand cryptographic algorithms, I need to know these foundational terms:
* **Plaintext:** The original, readable data (e.g., passwords, emails, files).
* **Ciphertext:** The scrambled, unreadable output after encryption.
* **Cipher:** The mathematical algorithm used to convert plaintext into ciphertext and back.
* **Key:** A secret string of bits used by the cipher. While the cipher algorithm is usually public, the key must remain a closely guarded secret.
* **Encryption:** The process of converting plaintext into ciphertext using a cipher and a key.
* **Decryption:** The reverse process of converting ciphertext back into plaintext.

---

## Historical Ciphers: The Caesar Cipher
The Caesar Cipher (1900 BCE) is a classic example of substitution. It encrypts data by shifting each letter of the plaintext by a certain number (the key). 
* **Example:** Shifting "TRYHACKME" right by a key of 3 results in "WUBKDFNPH".
* **Weakness:** By modern standards, this is completely insecure. Since there are only 26 letters in the English alphabet, there are only **25 possible keys**. An attacker can easily brute-force the ciphertext by trying all 25 shifts until the plaintext makes sense.

---

## Symmetric vs. Asymmetric Encryption

###  Symmetric Encryption (Private Key)
Symmetric encryption uses the **same key** to both encrypt and decrypt the data.
* **Pros:** Very fast and efficient for encrypting large amounts of data.
* **Cons:** The "Key Distribution Problem" — sharing the secret key securely over an insecure channel is difficult. If an attacker intercepts the key, the encryption is compromised.
* **Standard Ciphers:**
  * **DES (DATA ENCRYPTION STANDARD ):** Legacy standard, 56-bit key (broken in 1999).
  * **3DES:** DES applied three times with a 168-bit key (deprecated).
  * **AES (Advanced Encryption Standard):** The current global standard. Uses 128, 192, or 256-bit keys.

###  Asymmetric Encryption (Public Key)
Asymmetric encryption uses a mathematically linked **key pair**:
* **Public Key:** Shared openly with everyone, used to *encrypt* data.
* **Private Key:** Kept strictly secret, used to *decrypt* data.
* **Pros:** Solves the key distribution problem. Anyone can send me a secure message using my public key, but only I can read it with my private key.
* **Cons:** Much slower and requires significantly larger key sizes compared to symmetric encryption.
* **Standard Ciphers:**
  * **RSA:** Minimum recommended key size is 2048-bit (though 3072 or 4096-bit are common).
  * **Diffie-Hellman:** Used for secure key exchange.
  * **ECC (Elliptic Curve Cryptography):** Highly efficient. A 256-bit ECC key provides the same security as a 3072-bit RSA key.

---

## Cryptography Mathematics
Modern cryptography relies heavily on specific mathematical properties that are easy to compute in one direction but virtually impossible to reverse without the key.

### XOR Operation (Exclusive OR)
XOR is a binary logical operation that compares two bits: it returns `1` if the bits are different, and `0` if they are the same.
* **Truth Table:**
  * `0 ⊕ 0 = 0`
  * `0 ⊕ 1 = 1`
  * `1 ⊕ 0 = 1`
  * `1 ⊕ 1 = 0`
* **Cryptographic Properties:**
  * A value XORed with itself is zero: $A \oplus A = 0$
  * A value XORed with zero is itself: $A \oplus 0 = A$
* **Application:** XOR acts as a basic symmetric cipher. If $P$ is plaintext and $K$ is the key:
  * Encryption: $C = P \oplus K$
  * Decryption: $P = C \oplus K$

### Modulo Operation (`%` or `mod`)
The modulo operator returns the remainder of a division operation. For example:
* $25 \pmod 5 = 0$ (25 / 5 = 5, remainder 0)
* $23 \pmod 6 = 5$ (23 / 6 = 3, remainder 5)

**Why it matters:** Modulo is a "one-way" function. It is not reversible. If I know that $X \pmod 5 = 4$, there is an infinite number of values $X$ could be (4, 9, 14, 19...). This irreversible property is foundational to building asymmetric ciphers like RSA.

# 2. Public Key Cryptography Basics


## The Pillars of Secure Communication
I learned that secure communication relies on a combination of cryptography types to satisfy four core requirements:
* **Confidentiality:** Preventing unauthorized parties from eavesdropping (primarily handled by *Symmetric Encryption*).
* **Integrity:** Ensuring the data hasn't been altered in transit.
* **Authenticity:** Verifying the message genuinely comes from the claimed source.
* **Authentication:** Confirming the identity of the person/server you are talking to (these last three are primarily handled by *Asymmetric/Public Key Cryptography*).

Because asymmetric encryption is slow, it is heavily used to securely negotiate a shared secret key over an insecure channel. Once the secret key is established, the two parties switch to faster symmetric encryption to protect the actual data. 

**The Lock & Box Analogy:** If I want to send you a secret code securely, I ask you for an open padlock (your Public Key). You keep the physical key to that lock (your Private Key). I put the secret code in a box, snap your padlock shut, and send it back. Now, only you can open the box to read the code. 

---

## RSA (Rivest-Shamir-Adleman)
RSA is based on the mathematical difficulty of **factoring large numbers**. Multiplying two massive prime numbers is easy for a computer, but finding those original prime factors from the result is virtually impossible.

**CTF Cheat Sheet for RSA:**
If I encounter an RSA cryptography challenge in a CTF, I need to look for these variables:
* `p` and `q`: The large prime numbers.
* `n`: The product of `p` and `q`.
* **Public Key:** Made up of `n` and `e`.
* **Private Key:** Made up of `n` and `d`.
* `m`: The original message (plaintext).
* `c`: The encrypted message (ciphertext).

*Note:* Great tools for breaking RSA challenges include **RsaCtfTool** and **rsatool**.

---

## Diffie-Hellman Key Exchange
While RSA is often used for digital signatures and authentication, Diffie-Hellman (DH) is a brilliant method for two parties to establish a shared symmetric key over an insecure channel *without* ever transmitting the key itself. 

**How it works (simplified):**
1. Alice and Bob agree publicly on a prime number `p` and a generator `g`.
2. Alice picks a secret private key `a`. Bob picks a secret private key `b`.
3. Alice calculates her public key `A` (using `a`, `p`, and `g`). Bob calculates his public key `B` (using `b`, `p`, and `g`). 
4. They swap public keys `A` and `B` over the open internet.
5. Alice combines her private key `a` with Bob's public key `B` to get the shared secret. Bob does the exact same with his private key `b` and Alice's public key `A`. The math ensures they both arrive at the exact same shared secret key!

---

## SSH (Secure Shell) & Keys
When connecting to a server via SSH, public key cryptography works in two directions:
1. **Server Authentication:** The client checks the server's public key fingerprint. If it changes unexpectedly, the SSH client throws a massive warning to prevent Man-in-the-Middle (MITM) attacks.
2. **Client Authentication:** Instead of using weak passwords, I can generate a cryptographic key pair to log into a server. 

**Common SSH Commands:**
* **Generate a modern, secure key:** `ssh-keygen -t ed25519`
* *(Other key algorithms include RSA, DSA, ECDSA, and hardware-backed keys like ECDSA-SK/Ed25519-SK).*
* **Private Key Security:** The private key (stored in `~/.ssh/id_ed25519`) must have strict permissions (`chmod 600`) and should never be shared. It can optionally be encrypted with a passphrase to protect against local theft.
* **Public Key:** The public key (`.pub`) is placed on the target server inside the `~/.ssh/authorized_keys` file to grant access.

*Pro-tip for Penetration Testing:* Dropping my public SSH key into a compromised user's `authorized_keys` file is an excellent way to upgrade an unstable reverse shell into a fully interactive, persistent SSH session.

---

## Digital Signatures & Certificates
**Digital Signatures** prove authenticity and integrity. I sign a file by encrypting its hash with my *Private Key*. Anyone can decrypt that hash with my *Public Key* and compare it to the file to guarantee it was authored by me and hasn't been tampered with.

**Certificates** prove identity (e.g., that a website is legitimately `example.com`). They rely on a **Chain of Trust**:
* My browser inherently trusts a list of **Root CAs** (Certificate Authorities).
* The web server provides a certificate signed by one of these Root CAs.
* Since the CA trusts the server, my browser trusts the server.
* *Note:* I can use services like **Let's Encrypt** to get free, legitimate TLS certificates for my own web servers.

---

## PGP / GPG (Pretty Good Privacy)
GPG (GNU Privacy Guard) is an open-source implementation of PGP. I use it primarily to encrypt, decrypt, and sign files or emails.

**Common GPG Commands:**
* **Generate a key pair:** `gpg --full-gen-key` (I usually select ECC for modern security/speed).
* **Import an existing key backup:** `gpg --import backup.key`
* **Decrypt a received file:** `gpg --decrypt confidential_message.gpg`

---

## Attack Terminology Reference
* **Cryptanalysis:** The mathematical study of breaking or bypassing cryptographic systems without knowing the key.
* **Brute-Force Attack:** Trying every single possible key or password combination until one works.
* **Dictionary Attack:** A targeted brute-force attack that tries words from a dictionary list (much faster than pure brute-force for cracking weak, human-made passwords).

# 3. Hashing Basics

## What is a Hash Function?
I learned that hashing is fundamentally different from encryption. A hash function takes an input of *any* size and computes a fixed-size string of characters called a **digest** or **hash value**. 
* **One-Way:** It is mathematically impractical to reverse a hash back into its original input. There is no "key" to decrypt a hash.
* **Avalanche Effect:** Changing even a single bit of the input data (like changing a "T" to a "U") completely changes the entire hash output.

**Common Hash Output Sizes:**
* **MD5:** 16 bytes (128 bits) -> typically displayed as 32 hexadecimal characters.
* **SHA-1:** 20 bytes (160 bits).
* **SHA-256:** 32 bytes (256 bits).

---

## Hash Collisions
A collision occurs when two different inputs produce the exact same hash output. Because the number of possible inputs is infinite but the hash output size is fixed, collisions are inevitable (this is known as the **pigeonhole principle**). 
* Algorithms like **MD5** and **SHA-1** are now considered insecure and deprecated for cryptographic purposes because attackers have figured out how to intentionally engineer collisions for them.

---

## Securing Passwords & Rainbow Tables
Storing passwords in plaintext, using deprecated encryption, or using raw, unsalted hashes (like LinkedIn did in 2012) are massive security risks. 
* **Rainbow Tables:** These are massive, precomputed lookup tables mapping plaintexts to their hash values. Attackers use them to instantly reverse unsalted hashes, trading storage space for cracking speed.
* **The Solution (Salting):** To defeat rainbow tables, modern systems add a **Salt** (a random, unique string of characters) to the password *before* hashing it (e.g., `hash(password + salt)`). This ensures that even if two users have the same password, their database hashes will be completely different.
* **Secure Algorithms:** Modern systems use slow, salt-handling algorithms like **Bcrypt, Scrypt, Argon2,** or **PBKDF2** to deter brute-force hardware attacks.

---

## Recognizing and Cracking Hashes

### Linux Hashes (`/etc/shadow`)
On Linux, passwords are stored in the `/etc/shadow` file in the format: `$prefix$options$salt$hash`. The prefix tells me exactly what algorithm was used:
* `$y$` = yescrypt (The modern standard, 256-bit hash size)
* `$6$` = SHA-512
* `$2b$ / $2y$` = Bcrypt
* `$1$` = MD5

### Windows Hashes (SAM File)
Windows hashes passwords using **NTLM** (a variant of MD4). Because NTLM and MD5 both output a 32-character hexadecimal string (numbers 0-9 and letters a-f), automated identifier tools often mix them up. I always have to rely on context: if I dumped it from a Windows SAM file via Mimikatz, it's NTLM. 

### Cracking with Hashcat
If I have permission to crack a hash, I use my GPU with **Hashcat** (or CPU with John the Ripper). The basic syntax I use for Hashcat is:
`hashcat -m <hash_type> -a <attack_mode> hashfile wordlist`
*Example (Cracking an NTLM hash using the rockyou wordlist):*
`hashcat -m 1000 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

---

## Data Integrity & HMAC
Aside from passwords, hashing is primarily used to guarantee file integrity. By comparing the SHA-256 hash of a file I downloaded against the hash posted on the developer's official website, I can guarantee the file wasn't tampered with by a man-in-the-middle.

**HMAC (Keyed-Hash Message Authentication Code):**
HMAC takes hashing a step further by combining a cryptographic hash function with a **secret key**. 
* The secret key proves **Authenticity** (I know exactly who sent it).
* The hash proves **Integrity** (The message hasn't been altered).

---

## The Golden Rule: Hashing vs Encoding vs Encryption
To avoid confusing these three distinct concepts in cyber security, I use this baseline:
1. **Hashing (e.g., SHA-256, Bcrypt):** One-way process. Creates a unique fingerprint of the data. Cannot be reversed. Used for integrity and password storage.
2. **Encoding (e.g., Base64, UTF-8):** Converts data into a different format for system compatibility. Easily reversible by anyone. **Provides ZERO confidentiality.**
3. **Encryption (e.g., AES, RSA):** Two-way process. Secures data confidentiality. Can only be reversed (decrypted) if you possess the correct cryptographic key.

# 4. John the Ripper: The Basics

# Hash Cracking with John the Ripper (Jumbo John)

I learned that hashing algorithms (MD4, MD5, SHA1, NTLM) are one-way functions. In computer science, generating a hash is a **"P" (Polynomial Time)** problem—meaning it's fast to compute. Reversing a hash is an **"NP" (Non-deterministic Polynomial Time)** problem, making it computationally infeasible.

Because I can't reverse a hash directly, I have to use a **dictionary attack**. This involves taking a massive list of common words (like the infamous `rockyou.txt` breached in 2009), hashing each one, and comparing it to my target hash until I find a match. My primary tool for this is **John the Ripper** (specifically the extended "Jumbo John" version). 

---

## 1. Basic Syntax & Hash Identification
While John has an auto-detect feature, it can be unreliable. I always prefer to identify the hash manually first, then feed the specific format to John.

* **Identifying a hash:** I use a Python tool called `hash-identifier`.
  * *Setup:* `wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py`
  * *Run:* `python3 hash-id.py`
* **Finding John's supported formats:** `john --list=formats` (I can pipe this to `grep` to filter).
* **Automated Crack (Not recommended):** `john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`
* **Format-Specific Crack (Recommended):** `john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`
  *(Note: Standard hashes in JTR usually require the `raw-` prefix).*

---

## 2. Cracking Windows Authentication Hashes (NTLM)
Modern Windows systems store user and service passwords in the **SAM** (Security Account Manager) database or the Active Directory **NTDS.dit** database using the **NTHash / NTLM** format. 
* Once I dump the SAM database (e.g., using Mimikatz), I can crack the hashes by setting the format flag to `nt`.
* **Command:** `john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt ntlm.txt`

---

## 3. Cracking Linux Hashes (`/etc/shadow`)
Linux stores hashed passwords in `/etc/shadow`, which is readable only by the root user. John cannot read the shadow file by itself; it requires contextual data from the `/etc/passwd` file.

* **Step 1: Unshadowing**
  I use John's built-in `unshadow` tool to combine the copies of `passwd` and `shadow` into a single, readable file:
  `unshadow local_passwd local_shadow > unshadowed.txt`
* **Step 2: Cracking**
  I feed the combined file into John. (The format is usually `sha512crypt`):
  `john --format=sha512crypt --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt`

---

## 4. Single Crack Mode & Word Mangling
If I don't want to use a massive wordlist, I can use **Single Crack Mode**. This mode uses **Word Mangling**—it takes the user's username and heuristically mutates it to guess the password (e.g., `Markus` -> `Markus1`, `MArkus`, `Markus!`). 

It also pulls data from the **GECOS field** in UNIX-like systems (which stores general user info like full names and phone numbers) to generate password guesses.
* **Formatting the Hash:** The text file *must* be prepended with the username: `username:hash` (e.g., `mike:1efee03cdcb96d90ad48ccc7b8666033`).
* **Command:** `john --single --format=raw-sha256 hashes.txt`

---

## 5. Custom Rules (Exploiting Password Predictability)
Many organizations enforce strict password complexity rules (Uppercase + Lowercase + Number + Symbol). Because humans are predictable, they usually capitalize the first letter and append a number and symbol at the end (e.g., `Polopassword1!`).

I can exploit this **password complexity predictability** by creating **Custom Rules** in the `john.conf` file (located in `/opt/john/john.conf` or `/etc/john/john.conf`). This automatically mutates my wordlist to match these exact patterns.

* **Syntax Example (`john.conf`):**
  ```text
  [List.Rules:THMRules]
  cAz"[0-9] [!£$%@]"

c: Capitalizes the first letter positionally.

Az: Appends characters to the end of the word.

A0: Prepends characters to the start of the word.

[0-9]: Includes a number (0-9).

[!£$%@]: Includes one of these specific symbols.

(Example: To just add all capital letters to the end of a word, I would use: Az"[A-Z]")

Using the Custom Rule:
john --wordlist=/usr/share/wordlists/rockyou.txt --rule=THMRules hash.txt

6. Cracking Protected Files (Zip, RAR, SSH Keys)
John's true versatility lies in its suite of conversion tools. I can extract the hash from protected files, output it to a text file, and crack it using standard JTR commands.

Password-Protected Zip Archives
Extract Hash: zip2john secure.zip > zip_hash.txt

Crack: john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt

Extract Contents: unzip secure.zip

Password-Protected RAR Archives (WinRAR)
Extract Hash: rar2john secure.rar > rar_hash.txt (Path may vary: e.g., /opt/john/rar2john)

Crack: john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt

Extract Contents: unrar e secure.rar

SSH Private Keys (id_rsa)
If an SSH private key is protected by a passphrase, I cannot use it to authenticate over SSH until I crack that passphrase.

Extract Hash: ssh2john id_rsa > id_rsa_hash.txt
(Note: On systems like the AttackBox/Kali, this is a Python script: python3 /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt or python /usr/share/john/ssh2john.py id_rsa > id_rsa_hash.txt)

Crack: john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
