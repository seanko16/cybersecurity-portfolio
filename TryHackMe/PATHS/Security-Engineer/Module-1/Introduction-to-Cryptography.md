# Cryptography Fundamentals  

## Overview  
This document summarizes the key concepts learned from the **TryHackMe: Cryptography Fundamentals** room.  
It covers the basics of symmetric and asymmetric encryption, hashing, HMAC, and how these concepts are applied in HTTPS.  

---

## Symmetric Encryption 🔑  
A symmetric encryption algorithm uses the same key for encryption and decryption. Consequently, the communicating parties need to agree on a secret key before being able to exchange any messages.  

### Core Terminology  
- **Cryptographic Algorithm / Cipher** → Defines the encryption and decryption processes.  
- **Key** → Needed by the algorithm to convert plaintext into ciphertext and vice versa.  
- **Plaintext** → The original message we want to encrypt.  
- **Ciphertext** → The message in its encrypted form.  

### Key Stretching with PBKDF2  
- **PBKDF2 (Password-Based Key Derivation Function 2)** → An algorithm that stretches a password into a longer key to resist brute-force attacks.  
- **-iter NUMBER** → Controls how many times the stretching process is repeated. The more iterations, the harder it is for attackers to crack.  

### Tools  
- **GNU Privacy Guard (GPG)**  
- **OpenSSL Project**  
Both can be used via commands for encryption operations.  

---

## Asymmetric Encryption 🔓  
Asymmetric encryption works by using two keys:  
- **Public Key** → Shared openly, used to encrypt data.  
- **Private Key** → Kept secret, used to decrypt data.  

Example: If I want to send a message to Bob, I need Bob’s public key to encrypt the message. Only Bob, with his private key, can decrypt it. This greatly enhances confidentiality.  

- **Diffie-Hellman Key Exchange** → An asymmetric algorithm that allows the exchange of a secret over a public channel.  

---

## Hashing #️⃣  
A hash is unique to a file, and even if 1 bit changes the hash will change. The checksum is usually represented using hexadecimal digits.  

### Algorithms  
- **Still secure** → SHA224, SHA256, SHA384, SHA512, RIPEMD160  
- **Broken** → MD5 (can generate collisions, meaning different files with the same hash).  

➡️ Because MD5 is broken, I will not rely on it in my work or projects.  

### Password Hashing & Salting  
- Passwords should never be stored in plaintext.  
- **Hashing** → Passwords are run through a secure hashing algorithm.  
- **Salting** → A random value is appended to the password before hashing.  

This prevents attacks like **pass-the-hash** and the use of rainbow tables.  

---

## HMAC (Hash-based Message Authentication Code) ✍️  
HMAC uses a secret key and a hashing algorithm to generate a unique signature (or “tag”) for a piece of data.  

It ensures:  
- **Authenticity** → Proves the message came from someone with the secret key.  
- **Integrity** → Ensures the data has not been altered.  

### Command example  
```bash
$ hmac256 <key> <filename>
```

---

## HTTPS in Action 🌐  
When logging into a website over HTTPS:  

1. **Client requests server’s SSL/TLS certificate**  
2. **Server sends the certificate**  
3. **Client validates the certificate**  
   - A valid certificate is signed by a trusted third party.  
   - Signing means a hash of the certificate is encrypted with the private key of a Certificate Authority (CA).  
   - The client uses the CA’s public key to decrypt the hash and compare it with the certificate’s hash.  

4. **SSL/TLS Handshake**  
   - The client and server agree on a secret key and symmetric encryption algorithm.  

5. **Encrypted Session**  
   - From this point, all communication is encrypted with symmetric encryption.  

6. **Login**  
   - Credentials are sent through the encrypted channel.  
   - The server compares the submitted password against a **salted and hashed** password stored in its database.  

---
