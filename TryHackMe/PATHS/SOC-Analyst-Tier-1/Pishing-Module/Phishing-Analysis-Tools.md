# Phishing Email Analysis: Key Techniques from TryHackMe

## Introduction
This document summarizes the key takeaways and phishing techniques observed in the [Phishing Emails in Action](https://tryhackme.com/room/phishingemailsinaction) room on TryHackMe.  
The purpose of this analysis is to deconstruct real-world phishing emails to better understand the tactics, techniques, and procedures (TTPs) used by threat actors.  

This serves as a personal reference and learning repository for identifying and mitigating phishing threats.

---

## Phishing Techniques Identified

Phishing attacks are rarely one-dimensional. Attackers layer multiple techniques to create a convincing and effective lure. Below is a breakdown of common tactics highlighted in the room.

### 1. Urgency and Fear Tactics
- **What it is:** Creates a false sense of urgency (e.g., *“Your account has been suspended”*).  
- **Analysis:** Exploits human psychology, pushing victims to act impulsively.

### 2. Spoofed Email Address
- **What it is:** Forged `From` field to mimic trusted senders (banks, PayPal, DHL, etc.).  
- **Analysis:** Builds false trust; most users don’t inspect headers.

### 3. Link Manipulation
- **What it is:** Displayed URL looks legitimate, but underlying link points to a malicious site.  
- **Analysis:** Camouflages phishing domains; mitigated by hovering over links.

### 4. Credential Harvesting
- **What it is:** Redirects to fake login pages to steal usernames and passwords.  
- **Analysis:** Enables account takeover, financial fraud, or further attacks.

### 5. Brand Impersonation via HTML/CSS
- **What it is:** Emails crafted to perfectly mimic a trusted brand’s design.  
- **Analysis:** Lowers suspicion and increases effectiveness of other techniques.

### 6. Malicious Attachments
- **What it is:** Files such as `.html`, `.zip`, or macro-enabled Office docs deliver malware or phishing pages.  
- **Analysis:** Exploits user curiosity and bypasses URL filters.

### 7. URL Shortening Services
- **What it is:** Links masked with `bit.ly`, `tinyurl.com`, etc.  
- **Analysis:** Hides suspicious domains and evades filters.

### 8. Recipient in BCC
- **What it is:** Victims are hidden in the `BCC` field instead of `To`.  
- **Analysis:** Indicates a mass-targeted, impersonal campaign.

### 9. Pixel Tracking
- **What it is:** Hidden 1x1 pixel image that loads from attacker’s server when opened.  
- **Analysis:** Confirms active email addresses and measures campaign effectiveness.

### 10. Poor Grammar and Typos
- **What it is:** Awkward phrasing or errors uncommon in official communications.  
- **Analysis:** Sometimes intentional — weeds out more skeptical users while bypassing filters.

---

## Conclusion
Recognizing phishing emails requires spotting **combinations of red flags**, not just one.  
For SOC Analysts and cybersecurity professionals, deconstructing these techniques is a core skill.  

Each tactic — spoofed addresses, urgency, manipulated links, or malicious attachments — is a piece of a larger puzzle.  
By identifying these individual pieces, we can better protect organizations, educate users, and respond effectively to phishing threats.
