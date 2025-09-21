# TryHackMe: PS Eclipse Walkthrough

## Challenge Summary
This room focused on analyzing a Windows endpoint compromised by malicious activity.  
The investigation included tracing the attacker’s steps: **initial infection vector, privilege escalation, persistence, network communication, and malware identification.**

---

## Key Learnings & Forensic Techniques

### 1. Initial Access & File Locations
Attackers typically drop malicious files in **common system directories** to avoid detection.  
Always check these locations first:

- `C:\Temp\`
- `C:\Users\<username>\`
- `Startup` folders (`Run`, `Startup`)

-> These paths are prime hunting grounds for suspicious executables and artifacts.

---

### 2. Command Obfuscation
Attackers often encode PowerShell commands (e.g., Base64) to mask their true purpose.  
**Decoding is critical** — it often reveals:

- Hidden **C2 IPs/domains**
- Full payload download paths
- Specific attacker actions (persistence, privilege escalation, etc.)

---

### 3. Privilege Escalation & Persistence
Persistence is usually achieved with **Windows native tools**:

- **`schtasks.exe`** → creates scheduled tasks (`/Create`, `/TN`, `/TR`) to rerun malware with **SYSTEM** privileges.  
- **`sc.exe`** → manages services; attackers may create malicious services.  
- **`powershell.exe`** → commonly abused to download & execute payloads.  
- **Registry Keys** → check `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` and  
  `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` for persistence.

-> Look for newly added tasks, services, or registry entries as **indicators of persistence**.

---

### 4. Command & Control (C2) Communication
Malware often communicates with external C2 servers. To investigate:

- Check **network logs** for **frequent requests** to specific IPs/domains.
- Enrich suspicious IPs via:
  - [VirusTotal](https://www.virustotal.com)
  - [Censys](https://censys.io)

-> High-volume outbound connections often point directly to the attacker’s infrastructure.

---

### 5. Ransomware Identification
Ransomware almost always leaves a **ransom note**. Common filenames include:

- `*_README.txt`
- `HOW_TO_RECOVER_YOUR_FILES.txt`

-> These files contain **instructions and threats**, and often reveal the **malware family name**.

---

### 6. Malware Identification
To confidently identify the malware:

1. Inspect the ransom note for names or references.  
2. Hash the suspicious binary/script (e.g., SHA-256).  
3. Submit to **threat intelligence sources** like VirusTotal.  

-> This provides attribution, context, and community insight into the malware’s capabilities.

---

## TL;DR
The **PS Eclipse** challenge demonstrated how attackers:  
- Drop files into common Windows directories  
- Use **PowerShell & obfuscation** for execution  
- Abuse **scheduled tasks, services, and registry keys** for persistence  
- Communicate with C2 servers over repeated outbound requests  
- Leave clear **ransomware IOCs** like README files & altered wallpapers  

---

## Windows Forensic Essentials Checklist

> ⚠️ **Note:** This checklist was AI-generated.

| Item | What to Check | Why it Matters |
|------|---------------|----------------|
| **Suspicious file locations** | `%TEMP%`, `Downloads`, `Startup` | Common drop zones for malware |
| **Processes & tasks** | Unusual processes, new scheduled tasks | Persistence & privilege escalation |
| **Registry autoruns** | `HKCU/HKLM\...\Run` keys | Classic persistence point |
| **PowerShell / command history** | Encoded or obfuscated commands | Reveals hidden payloads & C2 |
| **Network activity** | Strange IPs/domains, repeated connections | Signs of C2 traffic |
| **Ransom notes** | `README.txt`, `HOW_TO_RECOVER...` | Identifies ransomware family |
| **File hashes** | SHA256/MD5 of suspicious files | Use VirusTotal for quick intel |

✅ Quick, simple, and actionable.
