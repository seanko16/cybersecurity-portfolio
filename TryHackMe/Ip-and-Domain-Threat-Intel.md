# IP and Domain Threat Intelligence
## Overview
During this room, I explored indicators of suspicious domains and IPs and learned how to enrich them for SOC triage. Key topics included **DNS record analysis, ASN and IP enrichment, and geolocation**. I also practiced investigating exposed services, certificates, and related infrastructure to gain a better understanding of system configurations, potential risks, and malicious activity patterns.


---

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

## Workflow for Domain Enrichment
1. **A/AAAA** → Resolve the domain and check IP reputation for suspicious or fast-flux hosting.  
2. **NS** → Confirm DNS provider legitimacy and note any unusual or recently changed nameservers.  
3. **MX + TXT (if email)** → Analyze SPF, DKIM, and DMARC records to assess spoofing or phishing risk.  
4. **SOA + TTL** → Check if the domain is fresh, unstable, or frequently changing, which may indicate malicious activity.  
5. **WHOIS** → Record registrar, creation date, and contact patterns to support a light ownership profile.  
6. **Summarize & log** → Combine findings to classify the domain as stable or suspicious, and recommend block, monitor, or close. 

---

## 🛡️ ASN & IP Enrichment Checklist

When investigating an IP or ASN, these are the key elements to check:

| Element | What to Check | Why It Matters | Tools |
|---------|---------------|----------------|-------|
| **ASN** | - Confirm ASN number <br>- Check organization controlling it <br>- Note origin and role | Identifies which network controls the IP range. <br>Helps spot clusters of malicious activity or infrastructure. | [bgp.he.net](https://bgp.he.net), [bgpview.io](https://bgpview.io), [ipinfo.io](https://ipinfo.io), `whois` |
| **RDAP Info** | - Netrange (IP block) <br>- Organization <br>- Abuse contacts | Provides ownership context and official contacts for reporting abuse. | [RDAP.org](https://rdap.org), `whois` |
| **Geolocation** | - Capture country from at least two sources <br>- Record mismatches | Reveals physical location of IP. <br>Mismatches can indicate proxies, VPNs, or suspicious hosting. | [iplocation.net](https://www.iplocation.net), [ipinfo.io](https://ipinfo.io) |
| **Reverse DNS (rDNS)** | - Check PTR records <br>- Identify hosting type (residential, ISP, cloud) | Reverse DNS can hint at type of hosting. <br>Useful for correlation, but don’t rely solely on it. | `nslookup -type=PTR <IP>`, [mxtoolbox.com](https://mxtoolbox.com/ReverseLookup.aspx) |
| **Internal Logs** | - Has this IP appeared in the last 30 days? <br>- Context of previous activity | Helps correlate with past incidents. <br>Detect recurring malicious behavior. | SIEM logs, internal databases |
| **Role Classification** | - Hosting, residential, CDN, or cloud <br>- Record reasoning | Determines if IP belongs to legitimate infrastructure or potentially attacker-controlled. | SIEM, ASN tools, geolocation sources |
| **Outreach Preparedness** | - Verify abuse contacts for ASN <br>- Prepare report if confirmed malicious | Enables responsible reporting and escalation to reduce future attacks. | RDAP.org, ASN databases, internal SOC templates |

---

## Services & Certificates Workflow
1. **Check Shodan/Censys banners** → Identify open services and misconfigurations.  
2. **Review TLS certificates** → Record issuer, SANs, and validity period.  
3. **Spot anomalies** → Multiple SANs, brand look-alikes, or bursts of issuance.  
4. **Pivot** → Use certificates or banners to find related infrastructure.  
5. **Assess blast radius** → Evaluate risk based on service type and ASN context (e.g., RDP on residential IP, self-signed TLS on small ranges, TLS with many SANs on CDN).

---

## Tools Used
| Tool | Purpose |
|------|---------|
| [RDAP.org](https://rdap.org) | IP ownership, netrange, ASN, and abuse contacts |
| [bgp.he.net](https://bgp.he.net) | ASN enrichment and organizational context |
| [iplocation.net](https://www.iplocation.net) | Cross-check IP geolocation from multiple sources |
| [Shodan](https://www.shodan.io/) | Identify open ports, running services, service banners, and geolocation of IPs |
| [crt.sh](https://crt.sh/) | Inspect TLS certificates, record issuer, SANs, and validity period |
| [Censys](https://censys.io/) | Explore certificates, banners, and pivot to related infrastructure for reconnaissance |

---
