# Module 6: Cryptography 

# 1. Cryptography Basics

## Cryptography Fundamentals
I learned that cryptography is the backbone of digital trust, ensuring three main pillars of secure communication:
* **Confidentiality:** No unauthorized party can read the data.
* **Integrity:** The data cannot be altered in transit without detection.
* **Authenticity:** We can verify the identity of the communicating parties.

Cryptography is heavily regulated depending on the industry. For instance, handling credit cards requires compliance with **PCI DSS** (encrypting data at rest and in transit), while medical records are governed by **HIPAA/HITECH** (US) or **GDPR** (EU).

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
  * **DES:** Legacy standard, 56-bit key (broken in 1999).
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
