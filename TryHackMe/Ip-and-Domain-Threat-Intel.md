# IP and Domain Threat Intelligence
## Overview
During this room, I explored indicators of suspicious files and learned how to identify and investigate potential threats. Key topics included heuristic filename indicators, file hashing, threat intelligence correlation, and safe analysis of malicious files.

## 🛡️ Domain Triage DNS Checklist

When enriching a domain, these are the DNS records that matter most.  

| Record | What to Check | Why It Matters | Tools |
|--------|---------------|----------------|-------|
| **A / AAAA** | - IP address of the domain <br>- Multiple IPs or rapid rotation? <br>- Copy IP → check on VirusTotal | Maps domain to IPv4/IPv6. <br>Fast rotation may indicate fast-flux hosting. <br>IP reputation can reveal malicious hosting. | `nslookup`, [dnschecker.org](https://dnschecker.org), [nslookup.io](https://nslookup.io), [VirusTotal](https://virustotal.com) |
| **NS (Name Server)** | - Which provider manages DNS? <br>- Any unusual or unknown nameservers? <br>- Recent changes? | Identifies who controls the domain. <br>Unusual providers may signal shady setups. <br>Recent changes can indicate new infrastructure. | `whois`, [dnsdumpster.com](https://dnsdumpster.com) |
| **MX (Mail Exchange)** | - Does the domain have MX records? <br>- Which servers handle mail? | MX records mean the domain can send/receive email. <br>Attackers may set this up for phishing. | `nslookup -type=MX <domain>` |
| **TXT** | - Look for SPF, DKIM, DMARC rules <br>- Weak or missing SPF? <br>- Random/unusual strings? | SPF/DKIM/DMARC prevent email spoofing. <br>Weak or missing configs increase phishing risk. | `nslookup -type=TXT <domain>` |
| **SOA (Start of Authority)** | - Primary DNS host <br>- Serial number | Shows the primary DNS authority. <br>Serial numbers indicate frequency of DNS changes. | `nslookup -type=SOA <domain>` |
| **TTL (Time To Live)** | - Is TTL very low (seconds/minutes)? | Low TTL = frequent record changes. <br>Common for CDNs, suspicious in attacker setups. | `nslookup`, DNS tools |

---

## Workflow at The Job
1. **A/AAAA** → Resolve the domain and check IP reputation for suspicious or fast-flux hosting.  
2. **NS** → Confirm DNS provider legitimacy and note any unusual or recently changed nameservers.  
3. **MX + TXT (if email)** → Analyze SPF, DKIM, and DMARC records to assess spoofing or phishing risk.  
4. **SOA + TTL** → Check if the domain is fresh, unstable, or frequently changing, which may indicate malicious activity.  
5. **WHOIS** → Record registrar, creation date, and contact patterns to support a light ownership profile.  
6. **Summarize & log** → Combine findings to classify the domain as stable or suspicious, and recommend block, monitor, or close. 
