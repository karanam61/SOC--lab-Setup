# Step 5 – CrowdSec (Community-Driven Detection & Blocking)

Sysmon collects raw telemetry but does not determine what is malicious. **CrowdSec** adds the detection and response layer by analyzing logs for attack patterns and optionally blocking them. Think of it as a modern Fail2Ban powered by global community intelligence.

---

## Objective
- Install CrowdSec on the Domain Controller and/or workstation.  
- Configure it to parse Windows Security and Sysmon logs.  
- Deploy a bouncer to block malicious IPs automatically.  
- Provide both **detection** and **active defense** in the SOC lab.

---

## What is CrowdSec?
- **Detection Engine** → Reads logs and applies scenarios (rulesets) to identify brute-force, scans, and other abnormal behaviors.  
- **Crowd Intelligence** → Shares detected attacker IPs with a global network for community defense.  
- **Bouncers** → Enforce blocking through Windows Firewall, pfSense, or reverse proxies.  

---

## Installation on Windows

### Step 1: Download the MSI
Download the latest installer from the official site:  
[https://www.crowdsec.net/](https://www.crowdsec.net/)

### Step 2: Run the Installer
- Installs CrowdSec as a **Windows Service**.  
- Default install path: `C:\Program Files\CrowdSec\`  
- Log directory: `C:\Program Files\CrowdSec\log\`

### Step 3: Verify Installation
Run in PowerShell:
```powershell
Get-Service crowdsec*
You should see the CrowdSec service running.
```


### Step 4: Install a Bouncer
- By default, CrowdSec only detects. To actively block attacks, install a bouncer.
- For Windows, use the Windows Firewall Bouncer:
- https://github.com/crowdsecurity/cs-windows-firewall-bouncer

### How It Works in the Lab
- On AD/DC → Parses Security + Sysmon logs to detect brute-force, privilege escalation, and account abuse.
- On pfSense (optional) → Parses firewall logs to detect port scans and brute-force attempts.
- On Workstations → Monitors application/system logs for local abuse.
- Malicious IPs can be blocked automatically and shared with the global CrowdSec network.

## SOC Relevance
- With CrowdSec in place, the SOC lab evolves from passive to active defense:
- pfSense → network visibility
- AD/DC → identity visibility
- Sysmon → endpoint telemetry
- CrowdSec → intelligent detection and blocking
