# TryHackMe - Phishing Prevention Room

## Overview
Welcome to the **Phishing Prevention** room! 📧 This document is a simple guide to the essential concepts of email security covered in this TryHackMe module.  
We'll break down how bad guys fake emails and explore the key technologies—**SPF**, **DKIM**, **DMARC**, and **S/MIME**—that we use to stop them. We'll also cover other defensive actions and tools that help protect users from malicious emails. Think of this as your friendly cheat sheet for understanding how to make email safer.

---

## Defensive Actions Against Phishing
To protect users from falling for malicious emails, defenders can use several tactics. Here are some of the most effective ones:

- **Email Authentication (SPF, DKIM, DMARC):** Technical standards that verify an email is from who it says it's from.  
- **SPAM Filters:** Automatically flag or block incoming emails based on sender reputation and content.  
- **Email Labels:** Add warnings to emails (e.g., `[EXTERNAL]`) to alert users the message is from an outside source.  
- **Blocklists:** Maintain lists of email addresses, domains, and URLs known for malicious activity and block them.  
- **Attachment Blocking:** Prevent users from receiving files with risky extensions (e.g., `.exe`, `.bat`).  
- **Attachment Sandboxing:** Open attachments in a safe, isolated environment (a "sandbox") to see if they behave maliciously before they reach the user.  
- **Security Awareness Training:** Run internal phishing campaigns and training sessions to teach users how to spot and report suspicious emails.

---

## SPF (Sender Policy Framework)
**SPF** is like a digital bouncer for your email domain — the first line of defense against email spoofing.

**What it does:** The owner of a domain (e.g., `mycompany.com`) publishes a public list of servers (by IP) that are authorized to send email on behalf of the domain (a DNS record).

**How it works:**  
- The receiving server checks the sender's IP address.  
- It looks up the domain's SPF record to see if the IP is authorized.

**The result:**  
- If the IP is on the list → **pass** ✅  
- If not → **fail** ❌ (server can mark the message as spam, block it, or escalate checks)

**Analogy:** SPF is like a guest list at a party — the security guard (receiving server) checks if the sender (sending server’s IP) is on the list.

---

## DKIM (DomainKeys Identified Mail)
**DKIM** is like a tamper-proof seal on a letter. It ensures the email’s content hasn't been changed in transit and confirms it was sent by the claimed domain.

**What it does:** Uses asymmetric encryption (public/private key pair) to add a digital signature to the email header.

**How it works:**  
1. The sending server creates a fingerprint (hash) of parts of the email and signs it with its **private key**.  
2. The signature is added to the email header.  
3. The receiving server retrieves the sender’s **public key** from DNS and verifies the signature and hash.

**The result:**  
- Valid signature → email is authentic and unchanged ✅  
- Invalid signature → DKIM check fails ❌

**Analogy:** The sender signs the letter with a unique secret signature (private key); the receiver verifies it using a public signature copy (public key).

---

## DMARC (Domain-based Message Authentication, Reporting, and Conformance)
**DMARC** is the policy layer that uses SPF and DKIM results to decide what to do with failing emails and to provide reporting to domain owners.

**What it does:** Acts on SPF/DKIM results and enforces a domain owner’s policy.

**How it works:**  
- DMARC checks whether an email passed SPF or DKIM and whether the domain in the `From:` header aligns with the domain that passed the checks.  
- Domain owners set a policy in DNS:

  - `p=none` → Monitor and report only.  
  - `p=quarantine` → Put failed emails into spam/quarantine.  
  - `p=reject` → Block failed emails outright.

**The result:** DMARC gives domain owners visibility (XML reports) and control over who can send mail on their behalf, enabling proactive spoofing mitigation.

---

## S/MIME (Secure/Multipurpose Internet Mail Extensions)
While SPF/DKIM/DMARC protect the **domain**, **S/MIME** protects the **message content** and verifies the **individual sender**.

**Two main features:**

- **Digital Signature (Authenticity & Integrity):**  
  - Sender signs the email with their **private key**.  
  - Receiver uses the sender’s public key (from their certificate) to verify the signature — proving the sender and that the message wasn’t altered.

- **Encryption (Confidentiality):**  
  - Sender uses the receiver’s public key to encrypt the email.  
  - Only the receiver’s private key can decrypt it — ensuring end-to-end confidentiality.

---

## Tools and Resources
During this room, the following tools and resources were highlighted for incident response and analysis:

- **Wireshark:** A powerful tool for analyzing network traffic. Useful to inspect SMTP packets and response codes to understand communication between email servers. The Wireshark SMTP Display Filter Reference is a handy resource for finding relevant fields.  
- **Phishing Playbook:** A structured Incident Response Playbook for handling phishing incidents — an excellent template for consistent response procedures.

---
