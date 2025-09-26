# The Greenholt Phish - Phishing Email Analysis

## 🎯 Objective
The obejctive was to Investigate the suspicious Email that was sent to a worker in the organization. 
The goal was to **identify malicious indicators** by dissecting the email’s content, headers, and attachments to verify the sender’s authenticity and intent.

---

## Phase 1: Initial Triage & Content Analysis 🔍
- Examined the email’s visible content for **social engineering cues**.  
- Subject and From fields contained suspicious details (e.g., *“Transfer Reference Number”*)—a pretext to create false legitimacy.  
- This phase helps quickly spot **obvious red flags** before deeper investigation.  

---

## Phase 2: In-Depth Email Header Analysis 📧
- Conducted technical analysis of full headers to uncover the **true origin**.  
- Found a **mismatch** between `From` and `Reply-To` addresses → common phishing indicator.  
- Traced `Received` headers to identify the **originating IP**.  
- [MX Lookup](https://mxtoolbox.com/MXLookup.aspx) performed on the IP to determine owner legitimacy (legit mail provider vs. suspicious/unexpected source).  

---

## Phase 3: Sender Authentication Verification 🔑
- Queried **DNS records** for the `Return-Path` domain.  

  - **SPF (Sender Policy Framework):** Checked if originating IP was an authorized sender. A **fail/softfail** strongly indicated spoofing.  
  - **DMARC (Domain-based Message Authentication):** Queried to assess the sender’s enforcement policy (`none`, `quarantine`, `reject`) for authentication failures.  

✅ These checks together provided definitive evidence of the sender’s authenticity (or lack thereof).  

---

## Phase 4: Attachment Forensics 🗂️
- Safely analyzed the suspicious attachment.  

  - **Hashing & Reputation:** Calculated SHA256 hash → checked against threat intel platforms like [VirusTotal](https://www.virustotal.com/) to detect known malware.  
  - **File Type Identification:** Verified the **true file type** to detect deceptive extensions (e.g., `invoice.pdf.exe`). Such tricks often disguise malicious payloads as harmless files.
 



***

# Tier 1 SOC Analyst Checklist – Suspicious Email Analysis

A structured checklist for a Tier 1 SOC Analyst to use when analyzing a potentially malicious email.

---

## 📜 Guiding Principles
- **Golden Rule:** Never click links or open attachments from a suspicious email on your primary machine. Use isolated virtual machines (sandboxes) or designated analysis tools.  
- **Check Scope:** Before starting, check your security tools (SIEM, EDR) to see if other users received the same or similar emails.

---

## Phase 1: Email Header Analysis 📧
Focus on the email’s metadata to verify its origin and authenticity.

- [ ] **Check Sender vs. Reply-To** → Verify that the `From:`, `Reply-To:`, and `Return-Path:` (Envelope From) are consistent and logical. A mismatch is a red flag.  
- [ ] **Analyze Authentication Results**:  
  - **SPF:** Does it show a pass? (Softfail/Fail means the server isn’t authorized.)  
  - **DKIM:** Does it show a pass? (Fail means the email may have been altered.)  
  - **DMARC:** Does it show a pass? (Relies on SPF/DKIM and shows sender’s policy.)  
- [ ] **Trace the Origin IP** → Find the first `Received:` header (true originating IP).  
- [ ] **Investigate the IP** → Perform WHOIS & geolocation. Does it make sense for the sender? (e.g., a US bank email shouldn’t come from a residential IP abroad).

---

## Phase 2: Email Body & Content Analysis ✍️
Examine the visible message for common **social engineering** tactics.

- [ ] **Look for Urgency/Threats** → “Immediate action required”, “Account suspended”, etc.  
- [ ] **Check for Generic Salutations** → “Dear Customer” instead of recipient’s name.  
- [ ] **Verify Sender Legitimacy** → Name, signature, and job title match directory or past correspondence.  
- [ ] **Inspect for Poor Quality** → Spelling mistakes, bad grammar, low-quality logos.

---

## Phase 3: Link (URL) & Attachment Analysis 🔗
⚠️ The most critical phase — proceed with caution.

### Links
- [ ] **Hover to Reveal Destination** → Does the URL match the text and look legitimate?  
- [ ] **Scrutinize the Domain**:  
  - **Lookalike:** `go0gle.com` vs `google.com`, `microsft.com` vs `microsoft.com`.  
  - **Suspicious Subdomains:** `yourbank.secure-login.com` → true domain is `secure-login.com`.  
- [ ] **Analyze URL with a Tool** → Paste into [VirusTotal](https://www.virustotal.com/), [urlscan.io](https://urlscan.io), or your internal tool.

### Attachments
- [ ] **Check File Name/Type** → Suspicious extensions: `.html`, `.js`, `.zip`, double extensions like `invoice.pdf.exe`.  
- [ ] **Calculate File Hash** → In a sandbox, compute `SHA256`.  
- [ ] **Check Hash Reputation** → Submit hash to VirusTotal or other threat intel platforms.  
- [ ] **Sandbox Detonation (if needed)** → Execute in isolated sandbox and observe behavior (network, filesystem changes).

---

## Phase 4: Disposition & Action ✅
Classify the email and take appropriate next steps.

- [ ] **Classify the Email**:  
  - *Phishing* → Credential theft / malware delivery.  
  - *Spam* → Unsolicited, non-malicious.  
  - *Benign* → Safe/legitimate.  
- [ ] **Execute Remediation**:  
  - Delete malicious emails from all inboxes.  
  - Block sender email/domain.  
  - Block originating IP at firewall/mail gateway.  
  - Block malicious URLs or file hashes.  
- [ ] **Document & Escalate**:  
  - Record findings, screenshots, and IOCs in incident ticket.  
  - Escalate to Tier 2/3 or IR team if compromise suspected.  
  - Close ticket with final classification and actions taken.

---

## 🏁 Conclusion
This lab demonstrated a **multi-layered investigative methodology**:  
1. Content triage →  
2. Header inspection →  
3. Sender authentication →  
4. Attachment forensics.
