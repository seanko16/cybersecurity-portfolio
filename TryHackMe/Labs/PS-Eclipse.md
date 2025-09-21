TryHackMe: PS Eclipse Walkthrough
📝 Challenge Summary
This document details the analysis of the TryHackMe PS Eclipse room, a forensic challenge focused on a Windows endpoint compromised by malicious activity. The investigation involved analyzing various system artifacts to understand the attacker's actions, including the initial infection vector, methods for privilege escalation and persistence, network communication, and the identification of the malware itself.

💡 Key Learnings & Forensic Techniques
1. Initial Access & File Locations
Attackers often download malicious files to common, less-monitored system directories to evade detection. When performing a forensic analysis, always prioritize checking locations like:

C:\Temp

C:\Users\<username>\

Startup folders (run, startup)

These locations are prime spots for discovering suspicious files or artifacts left by an attacker.

2. Command Obfuscation
Malicious commands, particularly those executed via PowerShell, are frequently encoded (e.g., using Base64) to hide their true intent. It's a critical step in any investigation to decode these commands. Decoded commands can reveal valuable information such as:

Hidden URLs or IP addresses for command-and-control (C2) servers.

The full path to downloaded payloads.

The specific actions the malware is configured to perform.

3. Privilege Escalation & Persistence
Attackers use built-in Windows utilities to achieve privilege escalation and persistence. A common technique is to create a scheduled task to run the malware with elevated privileges, ensuring it executes automatically on startup or at a specific time.

Key executables and registry locations to investigate for persistence mechanisms include:

schtasks.exe: Used to manage scheduled tasks. Look for command-line arguments like /Create, /TN (Task Name), and /TR (Task Run) to identify newly created tasks.

sc.exe: Manages Windows services. Attackers may create a new service to run their payload.

powershell.exe: A versatile tool used to download and execute malicious code.

Registry Keys: Check HKCU (HKEY_CURRENT_USER) and HKLM (HKEY_LOCAL_MACHINE) for newly added Run or RunOnce entries.

4. Command and Control (C2) Communication
To identify the remote server the malware communicates with, analyze network logs for unusual connections. Look for a high volume of requests to a specific IP address or domain. Once a suspicious IP is identified, use threat intelligence platforms like VirusTotal or Censys to gather more information, such as its reputation and historical use in malicious campaigns.

5. Ransomware Identification
When dealing with ransomware, always look for readme files left on the system. These files, commonly named _README.txt or HOW_TO_RECOVER_YOUR_FILES.txt, contain the ransom demand and instructions. The filename of the ransomware note can often provide a clue to the actual name of the malicious script or binary.

6. Malware Identification
To professionally name and document the malware, use the following methods:

Examine the ransom note for any clues or names.

Submit the file's hash (e.g., SHA-256) to public threat intelligence databases like VirusTotal. This will likely provide a confirmed name and additional context from the security community.
