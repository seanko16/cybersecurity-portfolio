# TryHackMe File and Hash Threat Intelligence
## Overview
This room explores indicators of suspicious files and techniques to identify and investigate potential threats. The focus is on heuristic filename indicators and file hash analysis for cybersecurity professionals.
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
### Hash Analysis Best Practices
- Store hashes in lowercase format to ensure consistency across platforms
- Hash both compressed archives and extracted contents when analyzing packed malware
- Maintain chain of custody documentation for forensic integrity
## Safe File Execution and Sandbox Analysis
### Need for Sandbox Environment
- Execute suspicious files in isolated environments to observe behavior safely
- Dynamic analysis reveals capabilities not visible in static analysis
- Generates IOCs for threat hunting and attribution
### Hash-Based Sandbox Tools
- **Hash Lookup**: Submit file hashes to platforms like Hybrid-Analysis for existing reports
- **Intelligence Extraction**: Retrieve process relationships, C2 servers, network indicators, file modifications
- **IOC Generation**: Network IPs/domains, file hashes, registry keys, API usage patterns
### Common Sandbox Evasion Tactics
- **Environment Awareness**: VM detection, hardware fingerprinting
- **Anti-Debug/Sandbox**: Debugging detection, sleep delays, user interaction requirements
- **Limited Runtime**: Time-based triggers, conditional execution, multi-stage payloads
- **Encrypted/Obfuscated Traffic**: SSL/TLS C2, domain generation algorithms, protocol tunneling
- **Fileless/Living-off-the-Land**: Memory-only execution, PowerShell abuse, WMI exploitation
### Analyst Sandboxing Checklist
- **Pre-Execution**: Hash verification, system baseline, network isolation, snapshots
- **Execution Confirmation**: Process monitoring, file system changes, network activity
- **Runtime IOC Extraction**: Network indicators (IPs, domains), host indicators (files, registry), behavioral patterns
- **MITRE ATT&CK Mapping**: Map observed behaviors to tactics/techniques for threat hunting
