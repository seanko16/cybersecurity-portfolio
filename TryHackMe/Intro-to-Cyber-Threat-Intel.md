# TryHackMe — Intro To CTI

## What Have I Learned
- I've learned about the Threat Intel lifecycle - for example: what happends after we find some indicators we believe are IOC's?
We need to gather more intel about them and provide a clear report for the next step, that could be the IR/SOC T2 team for escalation.
The report can be much more related and easier to understand if we make the applicable adjustments so that other teams can make the right
choices quickly enough based on the security polices. risk, and impcat of the incident/event.
Also, the room introduced the Cyber Kill Chain by Lockheed Martin again to explain the initiative behind the attacks.

## Skills Practiced
- Skill 1: checked the MITRE DEFEND framework website - explaining about counter measures
- Skill 2: a little side Lab of triaging IOC's given by THM to capture the flag - easy
- Skill 3: Now I know a few more tools that can help my flagging of IOC's better such as: "AbuseIPDB"
- Skill 4: Step 3 of the lifecycle taught me that we should gather enough information from more than just "VirusTotal", but use more vendors for flagging IOC's.
malicous IP's, Domains, or Hashes and then correlate and deduplicate the IOC's, make a table with TLP's assesing the information, by:
Creating a file for the firewall/EDR teams to block the IOC's of the week/month/incident.
For example: Let's say we found enough informations about three IP's that are considerd malicous by two or more vendors, and if they showed up on our SIEM platforms
we can say we advise to block them as-fast-as-possible and we send it with information gathered by the MITRE ATT&CK framework as well.
- we do it by making a file named: Firewall_Blocklist.CSV and send it to the team.

## Takeaway 
I understand the better way of assesing IOC's. I now know how to act upon incidents/events that require more investigations.
I am excited to continue to the other rooms related to CTI.
