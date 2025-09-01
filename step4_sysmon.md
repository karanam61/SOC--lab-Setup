# Step 4 – Sysmon (System Monitor)

Sysmon is a tool from the Sysinternals suite that extends Windows logging. By default, Windows Event Logs are limited and noisy. Sysmon provides **high-fidelity telemetry** about processes, network connections, file changes, and registry modifications, which is crucial for SOC monitoring.

---

## Objective
- Install Sysmon on the Domain Controller and Workstation(s).
- Apply a solid configuration file to control what Sysmon logs.
- Generate events in **Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational**.
- Provide SOC analysts with detailed process and network activity data.

---

## Key Events Sysmon Collects
- **Event ID 1** → Process creation (with hashes, parent process, command line)  
- **Event ID 3** → Network connections (process to IP/port)  
- **Event ID 6/7** → Driver and DLL loads  
- **Event ID 11** → File creation  
- **Event ID 12–14** → Registry changes  
- **Event ID 22** → DNS queries  

This level of detail is missing from native Windows logs.

---

## Installation

### Step 1: Download Sysmon
Download from official Microsoft Sysinternals:  
[https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

Unzip and place `sysmon.exe` and `sysmon64.exe` in `C:\Tools\Sysmon\`.

### Step 2: Get a Configuration File
A well-maintained config by **SwiftOnSecurity** is recommended:  
[https://github.com/SwiftOnSecurity/sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config)

Download `sysmonconfig.xml` and save it in the same folder.

### Step 3: Install Sysmon
Run an elevated PowerShell window:
```powershell
cd C:\Tools\Sysmon
.\sysmon64.exe -accepteula -i sysmonconfig.xml
```



## Step 4: Verify Installation

- Open Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
- You should see Event ID 1 (process creation) as soon as you run any program.

### SOC Relevance

- Sysmon provides the granular telemetry SOC analysts depend on:
- Detecting Living off the Land attacks (e.g., powershell.exe spawning cmd.exe).
- Spotting malware persistence in registry Run keys.
- Tracking outbound network connections by process (useful for C2 detection).
- Monitoring DNS queries from unusual processes.




