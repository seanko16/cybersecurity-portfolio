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
### Importance of Controlled Environment Analysis
- **Impact Assessment**: Executing suspicious files in isolated environments allows analysts to observe malicious behavior without compromising production systems
- **Runtime Intelligence**: Dynamic analysis reveals capabilities not apparent through static analysis alone
- **IOC Generation**: Sandbox execution generates network indicators, file modifications, and registry changes for threat hunting
- **Attribution**: Behavioral patterns and TTPs observed during execution aid in threat actor identification

### Sandbox Tools and Hash-Based Intelligence
#### Hybrid-Analysis Platform Benefits
- **Hash Lookup**: Submit file hashes to retrieve existing analysis reports without file upload
- **Behavioral Analysis**: Automated execution reveals:
  - Related processes and parent-child relationships
  - Network communications and C2 server indicators
  - File system modifications and persistence mechanisms
  - Registry changes and configuration modifications
- **IOC Extraction**: Comprehensive indicators including:
  - IP addresses and domain names
  - File hashes and mutexes
  - Registry keys and values
  - Network signatures and traffic patterns
- **String Analysis**: Extracted strings reveal:
  - Hardcoded URLs and IP addresses
  - Cryptographic keys and configuration data
  - Debug information and developer artifacts
- **DLL Analysis**: Dynamic library loading patterns expose:
  - API function usage and capabilities
  - Injection techniques and evasion methods
  - Third-party dependencies and libraries

### Sandbox Evasion Tactics
#### Environment Awareness Checks
- **VM Detection**: Malware scans for virtualization artifacts:
  - Registry keys indicating VMware, VirtualBox, or Hyper-V
  - Hardware identifiers and MAC address patterns
  - System file paths and driver signatures
- **Analysis Tool Detection**: Checks for debugging and monitoring tools:
  - Process names of common analysis software
  - DLL injection detection mechanisms
  - Timing-based analysis tool identification

#### Anti-Debug and Anti-Sandbox Techniques
- **Debugging Detection**: API calls to identify attached debuggers
- **Sleep/Delay Tactics**: Extended dormancy periods to bypass time-limited analysis
- **User Interaction Requirements**: Malware requiring mouse clicks or keyboard input
- **Resource Monitoring**: Checks for adequate CPU, memory, and disk resources

#### Limited Execution Coverage
- **Conditional Execution**: Malware activating only under specific conditions:
  - Date/time-based triggers
  - Geographic location requirements
  - Domain membership validation
- **Multi-Stage Payloads**: Initial droppers requiring network connectivity for full functionality
- **Packer/Crypter Evasion**: Runtime unpacking that may not complete in sandbox timeframes

#### Encrypted and Obfuscated Communication
- **Traffic Encryption**: SSL/TLS encrypted C2 communications hiding true intentions
- **Domain Generation Algorithms (DGA)**: Dynamically generated domains evading static IOC detection
- **Protocol Tunneling**: Malicious traffic disguised within legitimate protocols
- **Steganography**: Data hidden within seemingly benign files or communications

#### Fileless and Living-off-the-Land (LotL) Malware
- **Memory-Only Execution**: Malware residing entirely in system memory
- **PowerShell Abuse**: Legitimate administrative tools used for malicious purposes
- **WMI Exploitation**: Windows Management Instrumentation for persistence and execution
- **Registry-Based Persistence**: Code stored in registry keys rather than files

### Security Analyst Sandboxing Workflow
#### Pre-Execution Validation
- **Hash Verification**: Confirm file integrity using cryptographic hashes
- **Baseline Documentation**: Record initial system state before execution
- **Network Isolation**: Ensure controlled network environment with monitoring capabilities
- **Snapshot Creation**: Establish rollback points for system recovery

#### Execution Confirmation
- **Process Monitoring**: Verify malware execution through:
  - Process creation events and command-line arguments
  - Parent-child process relationships
  - Memory allocation and injection activities
- **File System Changes**: Monitor for:
  - File creation, modification, and deletion events
  - Directory structure changes
  - Hidden file and alternate data stream creation
- **Network Activity**: Observe:
  - DNS queries and resolution patterns
  - TCP/UDP connection establishment
  - HTTP/HTTPS request patterns and user agents

#### Runtime IOC Extraction
- **Network Indicators**: Document discovered infrastructure:
  - IP addresses and port numbers
  - Domain names and URL patterns
  - SSL certificate fingerprints
- **Host Indicators**: Catalog system modifications:
  - Created/modified file paths and hashes
  - Registry key additions and modifications
  - Service installations and modifications
- **Behavioral Indicators**: Record observed techniques:
  - Persistence mechanisms employed
  - Privilege escalation attempts
  - Data exfiltration methods

#### MITRE ATT&CK Framework Mapping
- **Tactic Identification**: Map observed behaviors to ATT&CK tactics:
  - Initial Access, Execution, Persistence
  - Privilege Escalation, Defense Evasion
  - Credential Access, Discovery, Collection
  - Command and Control, Exfiltration, Impact
- **Technique Documentation**: Specify exact techniques observed:
  - T1055: Process Injection
  - T1059: Command and Scripting Interpreter
  - T1071: Application Layer Protocol
- **Sub-Technique Granularity**: Provide detailed sub-technique classification when applicable
- **Threat Hunt Integration**: Use ATT&CK mappings to develop targeted hunting queries for environment-specific detection
