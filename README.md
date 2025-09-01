# SOC Home Lab Setup

This repository documents my SOC home lab build. The goal is to simulate an enterprise network with a firewall, Active Directory, a workstation, endpoint telemetry, and community-driven detection.  

## Steps

- [Step 1 – pfSense Firewall Setup](./step1_pfsense_firewall.md)
- [Step 2 – Active Directory Domain Controller](./step2_active_directory.md)
- [Step 3 – Windows Workstation](./step3_workstation.md)
- [Step 4 – Sysmon for Endpoint Telemetry](./step4_sysmon.md)
- [Step 5 – CrowdSec for Detection and Blocking](./step5_crowdsec.md)

## Why This Lab?

This setup provides:
- **pfSense** → firewall and perimeter logs  
- **Active Directory** → identity backbone  
- **Workstation** → endpoint simulation  
- **Sysmon** → detailed endpoint telemetry  
- **CrowdSec** → community-powered detection & automated response  

Together these create a realistic SOC playground to practice monitoring, detection, and response.
