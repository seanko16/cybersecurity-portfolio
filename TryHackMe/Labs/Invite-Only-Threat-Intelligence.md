# TryHackMe — "Invite Only" Lab

## Status
**Completed**

---

## Overview
This write-up documents the investigation of a multi-stage malware infection from the TryHackMe "Intro to Malware Analysis" final lab. The objective was to trace the infection chain, identify malware family and tools used by the attacker, and answer the lab questions using public analysis platforms.

---

## Environment & Tools
Primary analysis tools used for the lab:
- VirusTotal  
- Hybrid Analysis

(These tools were sufficient for the lab questions; a real-world investigation would commonly include additional static and dynamic analysis tooling and sandboxing.)

---

## Findings (Investigation Q&A)

**Q1 — File name for the flagged SHA256 hash**  
**A:** `syshelpers.exe`

**Q2 — File type for the flagged SHA256 hash**  
**A:** Win32 EXE

**Q3 — Execution parents of the flagged hash (chronological, comma-separated)**  
**A:** `361G.IXT7, 1nsta1ler.exe`

**Q4 — Name of the file being dropped**  
**A:** `Aclient.exe`

**Q5 — Four malicious dropped files from the second hash (top → bottom, comma-separated)**  
**A:** `searchhost.exe, syshelpers.exe, nat.vbs, runsys.vbs`

**Q6 — Malware family linking files related to the flagged IP**  
**A:** `AsyncRAT`

**Q7 — Title of the original report mentioning these indicators**  
**A:** *From Trust to Threat: Hijacked Discord Invites Used for Multi-Stage Malware Delivery*

**Q8 — Tool used to steal cookies from Google Chrome**  
**A:** `ChromeKatz`

**Q9 — Phishing technique used by the attackers**  
**A:** `ClickFix`

**Q10 — Platform used to redirect users to malicious servers**  
**A:** `Discord`

---

## Key Learnings & New Concepts

### ChromeKatz — Cookie & Credential Theft
- **Purpose:** Steals cookies and credentials from Chromium-based browsers (e.g., Google Chrome).  
- **Capabilities:**
  - Credential dumping (saved usernames/passwords).  
  - Cookie theft (session hijacking, bypassing password + some 2FA scenarios).  
  - Techniques to bypass application-bound protections (e.g., ABE) by interacting with browser processes in memory.  
- **Components:**
  - *CookieKatz* — dumps cookies from browser memory.  
  - *CredentialKatz* — extracts stored credentials.  
  - *ElevationKatz* — attempts to obtain decryption material from elevation services.

### ClickFix — Phishing Methodology
- **Description:** A social-engineering pattern that creates urgency or a perceived problem to trick users into clicking a “fix” link.  
- **Typical flow:** Fake security alert → prompt to “Verify/Reset” via link → link leads to fraudulent login page that harvests credentials.

---

## Analysis Notes
- The malware chain included multiple stages and dropped payloads; correlating parent/child relationships and dropped filenames helped reconstruct the infection flow.  
- The flagged IP and associated sample metadata pointed to an `AsyncRAT` family linkage across related files.  
- The use of Discord as a redirect/orchestration vector and ClickFix social engineering highlights the importance of monitoring third-party collaboration platforms for abuse.  
- Credential theft via ChromeKatz underscores risk from browser-based session theft in targeted intrusion scenarios.

---

## Reflection
This lab was a practical application of the skills learned throughout the module. While VirusTotal and HybridAnalysis were sufficient to answer the required questions, a real-world scenario would likely involve a deeper investigation using a wider range of static and dynamic analysis tools. It was an effective exercise in tracing an infection chain and understanding attacker TTPs (Tactics, Techniques, and Procedures).

---

## References
- VirusTotal (file and network analysis)  
- Hybrid Analysis (sandbox reports)  
- Lab report: *From Trust to Threat: Hijacked Discord Invites Used for Multi-Stage Malware Delivery*
