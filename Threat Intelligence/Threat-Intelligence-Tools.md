# Threat Intelligence Tools

## Overview
This room introduces the different **types of threat intelligence** and key **tools for investigation**. I learned how to classify intelligence, use OSINT platforms, and conduct hands-on investigations using multiple tools.

---

## Types of Intelligence

- **Strategic Intelligence**  
  Data that impacts high-level decisions such as **policy or finance**.

- **Tactical Intelligence**  
  Data mapped to **MITRE ATT&CK**, explaining how attacks are initiated or carried out.

- **Technical Intelligence**  
  **Indicators of Compromise (IOCs)** such as file hashes, IP addresses, and domains.

- **Operational Intelligence**  
  Insights about the **intentions, objectives, or targeted activity** of an adversary against the organization.

---

## Investigation Tools

### 🔍 urlscan.io
- Maps out the provided URL to display:  
  - Website front page preview  
  - Forwarded URLs  
  - HTTP connections  
  - Indicators and links  
- Helps SOC analysts visualize and understand suspicious URLs.

---

### Abuse.ch
- A platform that integrates multiple malware and threat tracking services:  
  - **MalwareBazaar**  
  - **Feodo Tracker**  
  - **SSL Blacklist**  
  - **URLhaus**  
  - **ThreatFox**  
- Saves time in investigations by consolidating critical resources into one portal.  
- Added to my personal **SOC Tools folder** in the browser.

---

### ✉️ PhishTool
- A forensic analysis platform for **phishing email investigations**.  
- Core features for SOC Analysts:  
  - **Email analysis:** Attachments, headers, URLs, origin  
  - **OSINT integration:** Incorporates TTPs for better triage and alerting  
  - **Classification & reporting** for efficient workflow  

**Breakdown of Data Provided:**
- **Headers:** Routing info (source, destination, originating IP, DNS, timestamps)  
- **Received Lines:** SMTP traversal details for traceability  
- **X-Headers:** Metadata added by mail servers  
- **Security:** SPF, DKIM, DMARC results  
- **Attachments:** Lists all files sent with the email  
- **Message URLs:** Extracted external URLs  

---

### 🌐 Cisco Talos
- Cisco’s **cyber threat intelligence division**.  
- Collects massive amounts of telemetry from Cisco products and converts it into **actionable intelligence**.  
- Goals: Early threat detection, context enrichment, and advanced protection.

---

## Practical Application
At the end of this room, I received **three investigation challenges**:  
- Analyze three suspicious emails  
- Use multiple tools to enrich and investigate indicators:  
  - **VirusTotal**  
  - **Malware Analysis Platforms**  
  - **WHO.IS**  
  - **RDAP.org**  
  - **Cisco Talos**
