# File and Hash Threat Intelligence
## Overview
During this room, I explored indicators of suspicious files and learned how to identify and investigate potential threats. Key topics included heuristic filename indicators, file hashing, threat intelligence correlation, and safe analysis of malicious files.
## Heuristic Filename Indicators
### Double Extensions
Example: `invoice.pdf.exe`
- **Problem**: Windows hides known file extensions by default, enabling social engineering attacks where malicious executables masquerade as documents
- **Mitigation**: Disable "Hide extensions for known file types" in File Explorer settings to reveal true file types
### System Binary Impersonation
Example: `svchost.exe`
- **Problem**: Threat actors create files mimicking legitimate system processes to evade detection
- **Mitigation**:
  - Verify file paths and digital signatures rather than relying on filenames
  - Implement application control policies using Windows Defender Application Control (WDAC)
  - Monitor process execution from non-standard locations
### High-Entropy Strings
Example: `jnkaf77t1.exe`
- **Problem**: Randomly generated filenames used to evade signature-based detection
- **Analysis**: High entropy in filenames may indicate programmatically generated malware
- **Detection**: Implement entropy analysis in security monitoring systems
### Masquerading
Example: `backup-2300.exe`
- **Problem**: Benign-appearing names used to bypass user scrutiny
- **Detection**:
  - Monitor file execution from temporary directories
  - Analyze parent processes of suspicious executables
  - Correlate filename patterns with known threat campaigns
## File Hashing Techniques
### Cross-Platform Hash Commands
| Platform | Command | Example |
|----------|---------|----------|
| PowerShell | `Get-FileHash` | `Get-FileHash -LiteralPath "<file_path>" -Algorithm SHA256` |
| CMD | `certutil` | `certutil -hashfile "<filename.ext>" SHA256` |
| Linux | `sha256sum` | `sha256sum "<file_path>"` |

## Best Practices
- Store hashes in lowercase to avoid discrepancies.
- Hash what matters: For example, if malware is inside a ZIP file, hash both the archive and its extracted binary.
- To truly understand the impact of a malicious file, execute it safely in a controlled environment (sandbox or isolated VM) to prevent accidental infection.

## Sandbox Analysis
I learned that using sandbox tools, such as **Hybrid-Analysis**, can provide valuable intelligence about a suspicious file:
- By submitting the file hash, you can extract related processes and see all system interactions.
- Sandboxes reveal Indicators of Compromise (IOCs), such as:
  - C2 server connections
  - Related DLLs loaded
  - Suspicious strings/files
  - Network/registry modifications
- This approach lets analysts gain deeper insight without risking the host system.

## Sandbox Evasion Techniques
Malware often tries to detect if it is running in a sandbox to avoid analysis. Common techniques include:
- Delaying execution until real user activity is detected
- Checking for virtual environments/analysis tools
- Obfuscating network communication to hide C2 traffic
- Awareness of these helps improve threat analysis

## Threat Intelligence Tools
- **MalwareBazaar**: Advanced platform/dashboard for analyzing malware samples.
- **Hybrid-Analysis**: Sandbox for dynamic malware analysis and extracting IOCs.
- **Correlating multiple sources**: Important to use more than one source for thorough analysis.
- **VirusTotal & MITRE**: VirusTotal links known malware to MITRE ATT&CK techniques for actionable reports.

## Skills Practiced
- Identifying heuristic indicators of suspicious files
- Calculating SHA256 hashes in PowerShell, CMD, Linux
- Correlating threat intel across platforms
- Using sandbox tools for safe malware analysis
- Promoting security awareness for employees

## Takeaways

- **Validate and hash files early:** Always confirm you have the exact binaries and generate hashes to use in investigations and threat intelligence sources.  
- **Check file paths and filenames:** Look for unusual storage locations, trusted software directories, double extensions, high-entropy names, or system impersonation — these often indicate suspicious files.  
- **Use controlled environments:** Execute suspicious files safely in a sandbox or isolated VM to observe behavior, extract network, persistence, and other IOCs, and confirm intent.  
- **Leverage multiple intelligence sources:** Correlate information from platforms like VirusTotal, MalwareBazaar, and Hybrid-Analysis for a fuller picture.  
- **Document and report findings:** Summarize indicators by type, describe observed behavior, and provide clear, evidence-backed recommendations.  
- **Employee awareness matters:** Regular phishing and suspicious file training is critical to improving overall security.  
- **Be mindful of sandbox evasion:** Malware may try to detect analysis environments; awareness of these techniques improves the accuracy of your investigation.
