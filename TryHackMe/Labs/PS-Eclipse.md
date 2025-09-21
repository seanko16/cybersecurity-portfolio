````markdown
# PS Eclipse — Room Summary & Triage Notes

## Overview
This room walked through triaging a Windows endpoint after a suspicious binary was observed.  
Key learning points: where files commonly land, how attackers download & run code (often via PowerShell), how persistence is achieved (scheduled tasks / services / registry), and how to extract IOCs (files, domains, IPs, ransom notes, replaced wallpapers). I also practiced decoding obfuscated commands (base64) to reveal hidden URLs and actions.

---

## Key Findings (example IOCs from the lab)
> These IOCs are defanged for safe sharing.

- **Downloaded binary:** `OUTSTANDING_GUTTER.exe`  
- **Download location (example):** `hxxp[:]//886e-181-215-214-32[.]ngrok[.]io`  
- **Downloader executable:** `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`  
- **Scheduled task used to run with elevated privileges:**  
  ```text
  "C:\Windows\system32\schtasks.exe" /Create /TN OUTSTANDING_GUTTER.exe /TR C:\Windows\Temp\OUTSTANDING_GUTTER.exe /SC ON
````

* **Scheduled task run as:**

  ```text
  NT AUTHORITY\SYSTEM; "C:\Windows\system32\schtasks.exe" /Run /TN OUTSTANDING_GUTTER.exe
  ```
* **C2 connection observed:** `hxxp[:]//9030-181-215-214-32[.]ngrok[.]io`
* **Downloaded PowerShell script filename:** `script.ps1`
* **Likely actual malicious script name:** `BlackSun.ps1`
* **Ransom note saved:** `C:\Users\keegan\Downloads\vasg6b0wmw029hd\BlackSun_README.txt`
* **Wallpaper image saved (indicator):** `C:\Users\Public\Pictures\blacksun.jpg`

---

## Practical Triage Checklist (short & actionable)

1. **Search common download locations**

   * `%TEMP%`, `C:\Windows\Temp`, `C:\Users\<user>\Downloads`, user `Startup` and `Run` folders.
2. **Find the process that downloaded or executed the artifact**

   * Investigate processes around the event timestamp (PowerShell, `cmd.exe`, `bitsadmin`, `certutil`, `rundll32`).
3. **Decode obfuscated commands**

   * Base64-encoded PowerShell or other encoded commands often contain real URLs/IPs — decode and inspect them.
4. **Check persistence mechanisms**

   * Scheduled tasks: `schtasks /Query /FO LIST /V` or PowerShell `Get-ScheduledTask`
   * Services: `sc query` / PowerShell `Get-Service`
   * Registry Run keys: `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`, `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
5. **Collect network IOCs**

   * Use `netstat -ano`, proxy logs, or network capture to find which IPs/domains have the most outbound requests. Prioritize those for enrichment.
6. **Ransomware artifacts**

   * Search for `*_README.txt`, ransom notes, renamed files, or replaced wallpaper images in `C:\Users\Public\Pictures` or user `Pictures`.
7. **Record & preserve evidence**

   * Take screenshots, export relevant registry keys, scheduled task XML, PowerShell command history, and process memory if policy allows.

---

## Useful Defensive Commands & Patterns

> (For incident response / investigation)

* Find files (PowerShell):

```powershell
Get-ChildItem -Path C:\Users\*\Downloads,C:\Windows\Temp -Recurse -ErrorAction SilentlyContinue |
  Where-Object { $_.Name -match "OUTSTANDING|BlackSun|script" }
```

* List scheduled tasks (to find tasks created by attackers):

```powershell
Get-ScheduledTask | Where-Object { $_.TaskName -match "OUTSTANDING|Update|Windows*" }
```

* Query running services and suspicious ones:

```powershell
Get-Service | Where-Object { $_.Status -eq "Running" -and $_.Name -match "svc|updater|update" }
```

* Check recent PowerShell command history (if enabled / preserved):

```powershell
Get-Content (Join-Path $env:APPDATA 'Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt') -Tail 200
```

* Active network connections for suspicious IPs:

```cmd
netstat -ano | findstr :80
```

* Simple registry queries (Windows):

```cmd
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Run"
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
```

---

## Lessons Learned / Best Practices

* **Look in the usual places first.** Attackers commonly drop files in `Temp`, `Downloads`, startup locations and user `Run` registry keys.
* **PowerShell is often the downloader & escalation vector.** Always inspect encoded PowerShell commands — they hide real URLs/IPs.
* **Persistence is commonly scheduled tasks or services.** Search for `schtasks`, `sc.exe`, and unusual service names.
* **Use network request counts to identify likely C2 IPs.** The IP with the most repeated outbound requests is a strong candidate for the C2.
* **Ransomware IOCs include README files and replaced desktop images.** Search for README files and unusual images in the Pictures folder.
* **Correlate with external enrichment.** After extracting IPs/domains, enrich via VirusTotal, Censys, crt.sh, RDAP, or ASN lookups to get more context (certs, previous sightings, hosting).

---

## Next Steps / Improvement Ideas

* Automate common searches (scheduled tasks, Run keys, Temp folders) in a small PowerShell triage script.
* Preserve memory or images of suspicious processes when allowed by policy.
* Add decoded command snippets (defanged) to the incident file so escalation teams see exact C2 addresses used.

---

## TL;DR (one-liner)

Attackers commonly download and run binaries via PowerShell, persist by creating scheduled tasks or services, and leave clear IOCs (download paths, scheduled task commands, ransom README, wallpaper image). Focus triage on those locations, decode any obfuscated commands, and prioritize network hosts with the most outbound connections for enrichment.

```
```

