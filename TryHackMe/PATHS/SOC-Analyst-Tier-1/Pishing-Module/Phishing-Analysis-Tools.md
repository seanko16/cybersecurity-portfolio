# Mastering Phishing Analysis: A Practical Workflow Summary

This document outlines a systematic workflow for analyzing and responding to phishing threats, based on a hands-on lab focused on practical tooling. The core achievement was mastering a multi-stage investigative process, moving from initial email triage to in-depth malware detonation in a secure sandbox environment.

---

## A Structured Phishing Investigation Workflow 🕵️

A successful investigation follows a clear, repeatable process to ensure all evidence is collected safely and analyzed thoroughly. The methodology I practiced involves three primary stages.

### 1. Initial Triage: Header and Body Analysis
The first step is a static analysis of the email file itself. Using **PhishTool**, I learned to dissect email headers to uncover the true origin of a message and identify immediate red flags. This foundational stage involves extracting key indicators of compromise (IOCs), such as:

- The impersonated brand the attacker is using as a lure.  
- The actual sender's email address and originating IP address.  
- Suspicious domains of interest embedded in the email body.  

A crucial best practice reinforced here is the **defanging** of all URLs and IPs (`hxxp://` instead of `http://`, `127.0.0[.]1` instead of `127.0.0.1`). This prevents accidental interaction with malicious infrastructure during analysis and reporting.

---

### 2. Safe URL and Hyperlink Inspection
Phishing emails almost always contain a malicious link. I practiced extracting all URLs, paying special attention to shortened links (e.g., bit.ly) that obscure their final destination. The goal is to safely expand these links to identify the true phishing domain without ever visiting it directly.

---

### 3. Secure Attachment Analysis with Sandboxing
When an email includes an attachment, the investigation escalates to dynamic analysis. I utilized the **Any.Run** interactive sandbox to safely execute and observe malicious files. This process involves:

- **Hashing the Attachment:** Calculating the file's SHA256 hash to check its reputation against threat intelligence platforms like VirusTotal.  
- **Detonating the File:** Running the attachment in an isolated environment to observe its behavior in real time.  
- **Monitoring Malicious Activity:** While in the sandbox, I successfully identified malicious activity by:  
  - Detecting *Potentially Bad Traffic* to known malicious IP addresses and domains.  
  - Observing suspicious Windows processes being created by the malware.  
  - Pinpointing the specific vulnerability (CVE) the malware attempted to exploit.  

💡 The **Text Report** feature in Any.Run proved invaluable for quickly summarizing these critical forensic details into a clean, actionable format.

---

## Key Tooling Spotlight 🛠️
- **[PhishTool](https://phishtool.com):** An essential tool for initial analysis. It excels at parsing email headers, extracting artifacts, and organizing evidence in a structured manner.  
- **[Any.Run](https://any.run):** A powerful cloud-based sandbox for safe, dynamic malware analysis. It provides deep visibility into a file's behavior, network connections, and process activity.  

---

## Conclusion: From Evidence to Intelligence
This exercise solidified a comprehensive methodology that mirrors a real-world Security Operations Center (SOC) workflow. By combining static analysis in **PhishTool** with dynamic detonation in **Any.Run**, I can confidently deconstruct a phishing attempt, safely extract actionable intelligence (IOCs), and understand the full scope of a threat.  

This systematic approach is critical for accurate threat identification and effective incident response.

