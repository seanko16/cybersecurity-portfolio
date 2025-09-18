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
- Cross-reference hashes against multiple threat intelligence sources

## Threat Intelligence Tools

### MalwareBazaar

- Community-driven malware sample repository
- Provides advanced search capabilities and analytical dashboard
- Supports hash-based IOC correlation and family classification

### Multi-Source Correlation

- Correlate findings across VirusTotal, MalwareBazaar, and proprietary threat feeds
- Map identified threats to MITRE ATT&CK framework techniques
- Validate threat intelligence through multiple independent sources

## Technical Skills Applied

- Heuristic analysis of suspicious file indicators
- Cross-platform hash calculation (SHA256)
- Threat intelligence correlation and validation
- IOC enrichment through multiple data sources

## Key Findings

**File Legitimacy Assessment**: File path, execution location, and digital signatures are more reliable indicators than filename analysis alone.

**Intelligence Integration**: Multi-source threat intelligence correlation provides comprehensive threat context and reduces false positives.

**Detection Engineering**: Filename heuristics should complement, not replace, behavioral analysis and signature-based detection methods.
