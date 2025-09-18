# 🧩 TryHackMe — File & Hash Threat Intelligence

## 📚 Overview

This room explores **indicators of suspicious files** and techniques to identify and investigate potential threats. The focus is on **heuristic filename indicators** and **file hash analysis** for cybersecurity professionals.

---

## 🚩 Heuristic Filename Indicators

### 1. **Double Extensions**
**Example**: `invoice.pdf.exe`

- **❌ Problem**: Windows hides known file extensions by default, tricking users into believing malicious files are safe
- **✅ Prevention**: Disable "Hide extensions for known file types" in File Explorer settings

### 2. **System Binary Impersonation**
**Example**: `svchost.exe`

- **❌ Problem**: Attackers create files mimicking legitimate system processes
- **✅ Prevention**:
  - Don't rely solely on filenames for legitimacy verification
  - Verify file paths and locations
  - Implement allowlists for legitimate paths using **Windows Defender Application Control (WDAC)**

### 3. **High-Entropy Strings**
**Example**: `jnkaf77t1.exe`

- **❌ Problem**: Random filenames used to evade antivirus detection
- **✅ Prevention**:
  - Be suspicious of randomly generated filenames
  - Conduct regular **phishing awareness training** for employees

### 4. **Masquerading**
**Example**: `backup-2300.exe`

- **❌ Problem**: Users assume benign-looking names are safe
- **✅ Prevention**:
  - Monitor files executed from unusual locations (e.g., TEMP folders)
  - Check parent processes of newly created files
  - Include in employee security awareness training

---

## 🔐 File Hashing Techniques

### Cross-Platform Hash Commands

| Platform | Command | Example |
|----------|---------|----------|
| **PowerShell** | `Get-FileHash` | `Get-FileHash -LiteralPath "<file_path>" -Algorithm SHA256` |
| **CMD** | `certutil` | `certutil -hashfile "<filename.ext>" SHA256` |
| **Linux** | `sha256sum` | `sha256sum "<file_path>"` |

### 📋 Best Practices for File Hashing

- **Store hashes in lowercase** to avoid discrepancies
- **Hash what matters** for your investigation:
  - If malware is inside a ZIP file, hash both the archive AND the extracted binary
- **Document hash sources** and maintain chain of custody

---

## 🛠️ Threat Intelligence Tools

### **MalwareBazaar**
- Platform similar to VirusTotal
- Features complex syntax and analytical dashboard
- Useful for malware sample analysis

### **Multi-Source Correlation**
- **Never rely on single sources** for threat intelligence
- Cross-reference findings across multiple platforms
- **VirusTotal + MITRE ATT&CK**: Correlate known malware with MITRE techniques for detailed reporting

---

## 🎯 Skills Practiced

✅ **Technical Skills**:
- Identifying heuristic indicators of suspicious files
- Calculating SHA256 hashes across PowerShell, CMD, and Linux
- Threat intelligence correlation across multiple platforms

✅ **Security Awareness**:
- Employee training considerations
- Phishing recognition techniques
- File legitimacy assessment

---

## 🔑 Key Takeaways

> **🎯 Critical Assessment Factors**: File path, filename, and execution location are essential when determining file legitimacy.

> **🔍 Intelligence Gathering**: Combining multiple threat intelligence sources provides comprehensive threat analysis.

> **👥 Human Factor**: Employee training on phishing and suspicious file recognition is fundamental to organizational security posture.

---

## 📊 Quick Reference Card

### Suspicious File Indicators Checklist
- [ ] Double file extensions (.pdf.exe, .doc.scr)
- [ ] System process names in unusual locations
- [ ] High-entropy/random filenames
- [ ] Benign names in suspicious contexts
- [ ] Execution from TEMP/Downloads folders
- [ ] Unusual parent processes

### Hash Verification Workflow
1. **Calculate** file hash using appropriate platform tool
2. **Store** hash in lowercase format
3. **Cross-reference** with threat intelligence databases
4. **Document** findings and sources
5. **Correlate** with MITRE ATT&CK techniques if applicable
