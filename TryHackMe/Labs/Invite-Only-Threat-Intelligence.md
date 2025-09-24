TryHackMe Lab: Multi-Stage Malware Analysis
This write-up documents the investigation process of a multi-stage malware attack as part of the final lab in the "Intro to Malware Analysis" module on TryHackMe. The lab involved analyzing several malicious files to understand the infection chain, the malware family involved, and the attacker's objectives.

The primary tools used for this investigation were VirusTotal and HybridAnalysis.

Investigation Questions & Answers
Below are the questions from the lab and the answers discovered during the analysis.

Q1: What is the name of the file identified with the flagged SHA256 hash?

A: syshelpers.exe

Q2: What is the file type associated with the flagged SHA256 hash?

A: Win32 EXE

Q3: What are the execution parents of the flagged hash? List the names chronologically, using a comma as a separator.

A: 361G.IXT7, 1nsta1ler.exe

Q4: What is the name of the file being dropped?

A: Aclient.exe

Q5: Research the second hash in question 3 and list the four malicious dropped files in the order they appear (from up to down), separated by commas.

A: searchhost.exe, syshelpers.exe, nat.vbs, runsys.vbs

Q6: Analyse the files related to the flagged IP. What is the malware family that links these files?

A: asyncrat

Q7: What is the title of the original report where these flagged indicators are mentioned?

A: From Trust to Threat: Hijacked Discord Invites Used for Multi-Stage Malware Delivery

Q8: Which tool did the attackers use to steal cookies from the Google Chrome browser?

A: ChromeKatz

Q9: Which phishing technique did the attackers use?

A: ClickFix

Q10: What is the name of the platform that was used to redirect a user to malicious servers?

A: Discord

Key Learnings & New Concepts
This lab introduced two significant concepts: a credential-stealing tool and a specific phishing methodology.

ChromeKatz: Cookie & Credential Theft
A tool used by attackers to steal cookies and credentials from Chromium-based browsers like Google Chrome.

Key Capabilities
Credential Dumping: Extracts saved usernames and passwords stored by the browser.

Cookie Stealing: Steals session cookies, which can be used to hijack active user sessions, bypassing password and 2FA requirements.

Bypassing Application-Bound Encryption (ABE): Specifically designed to circumvent Google's ABE security feature by interacting directly with the browser's processes in memory.

How It Works
ChromeKatz uses several components to access and decrypt sensitive data:

CookieKatz: Dumps cookies from the browser's process memory.

CredentialKatz: Extracts stored login credentials.

ElevationKatz: Obtains decryption keys from the browser's elevation service to decrypt the protected data.

"ClickFix" Phishing Technique
A social engineering method that preys on a person's instinct to resolve issues quickly. The attacker manufactures a sense of urgency or concern, prompting the victim to click a malicious link without thinking.

A common example is a Fake Security Alert:

The Lure: An email or pop-up warning of a "suspicious login" or "account issue."

The "Fix": The message insists the user click a link immediately to "Verify Your Account" or "Reset Your Password."

The Trap: The link directs to a counterfeit login page that harvests the user's credentials.

Reflection
This lab was a practical application of the skills learned throughout the module. While VirusTotal and HybridAnalysis were sufficient to answer the required questions, a real-world scenario would likely involve a deeper investigation using a wider range of static and dynamic analysis tools. It was an effective exercise in tracing an infection chain and understanding attacker TTPs (Tactics, Techniques, and Procedures).
