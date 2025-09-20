# IP and Domain Threat Intelligence

## Overview
During this TryHackMe room, I learned how to investigate suspicious domains and IPs to support SOC triage. I explored **DNS records, ASN and IP enrichment, geolocation, exposed services, and SSL/TLS certificates**. The focus was on understanding system configurations, identifying potential risks, and detecting malicious activity patterns.

## Checklists

### Domain Triage DNS Checklist
| Record | What to Check | Why It Matters | Tools |
|--------|---------------|----------------|-------|
| **A / AAAA** | IP address, multiple IPs, rapid rotation | Maps domain to IPv4/IPv6; fast rotation may indicate fast-flux hosting; IP reputation reveals malicious hosting | `nslookup`, [dnschecker.org](https://dnschecker.org), [nslookup.io](https://nslookup.io), [VirusTotal](https://virustotal.com) |
| **NS** | DNS provider, unusual or new nameservers | Identifies domain control; unusual providers or changes may signal suspicious setups | `whois`, [dnsdumpster.com](https://dnsdumpster.com) |
| **MX** | Presence of mail servers | Domains with mail servers can be used for phishing | `nslookup -type=MX <domain>` |
| **TXT** | SPF, DKIM, DMARC, unusual strings | Prevents email spoofing; weak or missing rules increase phishing risk | `nslookup -type=TXT <domain>` |
| **SOA** | Primary DNS host, serial number | Shows primary DNS authority and frequency of DNS changes | `nslookup -type=SOA <domain>` |
| **TTL** | Low TTL values | Indicates frequent DNS changes; suspicious if not CDN | `nslookup`, DNS tools |

### ASN & IP Enrichment Checklist
| Element | What to Check | Why It Matters | Tools |
|---------|---------------|----------------|-------|
| **ASN** | ASN number, organization, origin | Identifies controlling network; helps spot clusters of malicious activity | [bgp.he.net](https://bgp.he.net), [bgpview.io](https://bgpview.io), [ipinfo.io](https://ipinfo.io), `whois` |
| **RDAP Info** | Netrange, organization, abuse contacts | Ownership context and reporting contacts | [RDAP.org](https://rdap.org), `whois` |
| **Geolocation** | Country, mismatches | Reveals physical location; mismatches indicate VPN/proxy | [iplocation.net](https://www.iplocation.net), [ipinfo.io](https://ipinfo.io) |
| **Reverse DNS (rDNS)** | PTR records, hosting type | Hints at hosting type (residential, cloud, ISP) | `nslookup -type=PTR <IP>`, [mxtoolbox.com](https://mxtoolbox.com/ReverseLookup.aspx) |
| **Internal Logs** | Past appearances, context | Detect recurring malicious activity | SIEM logs, internal databases |
| **Role Classification** | Hosting type, reasoning | Determines legitimacy of infrastructure | SIEM, ASN tools, geolocation sources |
| **Outreach Preparedness** | Abuse contacts, reporting | Enables responsible escalation | RDAP.org, ASN databases, SOC templates | 

## Overall Workflow (Beginning → End)
1. **Verify Indicator** – Confirm the domain/IP appears in your telemetry and is relevant to your environment.  
2. **Enrich Data** – Collect DNS records (A/AAAA, NS, MX, TXT, SOA, TTL), IP reputation, ASN, geolocation, service banners, SSL/TLS certificates, and historical data.  
3. **Analyze & Score** – Apply confidence scoring, correlate across data sources, and document rationale.  
4. **Decide Action** – Choose to block, monitor, or allow the indicator. Prefer precise controls, add expiry, and fully document decisions.  
5. **Hunt & Notify** – Search for related indicators, inform stakeholders, and create follow-up tasks.  

### Services & Certificates Workflow
1. Check **Shodan/Censys banners** → identify open services and misconfigurations  
2. Review **TLS certificates** → record issuer, SANs, validity period  
3. Spot **anomalies** → multiple SANs, brand look-alikes, bursts of issuance  
4. Pivot → find related infrastructure using certificates or banners  
5. Assess **blast radius** → evaluate risk based on service type and ASN context

---

## New Tools Discovered
- Cisco Talos  
- VirusTotal Passive DNS  
- Shodan & Censys for service enumeration  
- crt.sh for certificate inspection  
- bgp.he.net, bgpview.io, ipinfo.io, RDAP.org for ASN/IP enrichment  

## Skills Obtained
- Domain and IP reputation analysis  
- DNS record and ASN enrichment  
- IP geolocation and role classification  
- Investigating exposed services and certificates  
- Correlating multiple data sources to assess potential threats  

## Key Takeaways
- Learned to analyze **DNS records** to map domain ownership, mail configurations, and detect fast-flux or short-lived hosting.  
- Applied **ASN and IP enrichment** to understand network control, geolocation, hosting type, and identify clusters of suspicious activity.  
- Investigated **exposed services and SSL/TLS certificates** to detect anomalies such as unusual SANs and bursts of issuance.  
- Used multiple tools together—**Cisco Talos, VirusTotal, Shodan, Censys, crt.sh**—to build a comprehensive threat profile.  
- Developed the ability to **correlate data across sources**, classify domains/IPs by risk, and provide actionable intelligence in SOC environments.  

