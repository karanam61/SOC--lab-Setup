# Step 2 – Active Directory on Windows Server 2022

This step sets up the Domain Controller (DC) using **Windows Server 2022 Datacenter** and populates it with BadBlood to create a realistic enterprise environment.

---

## ⚠️ VMware Firmware Fix
Windows Server 2022 ISOs don’t always boot properly in VMware when using UEFI firmware.  
- Change VM firmware: **UEFI → Legacy BIOS**.  
- Otherwise you’ll hit errors like *“Cannot trust the license or source”*.  
- Once switched to BIOS, the ISO boots and installs normally.

---

## What is AD and a Domain Controller?
- **Active Directory (AD)** is Microsoft’s central directory service. It stores **users, groups, computers, and policies** in one place.  
- **Domain Controller (DC)** is the Windows Server that runs AD DS (Active Directory Domain Services). It enforces authentication and applies group policies.  

### Example
In a real company:  
- Every employee has an AD account.  
- Group policies define what users can do (password rules, drive mappings, allowed apps).  
- When an employee logs into a workstation, the login request is sent to the **Domain Controller**.  
- The DC checks AD, applies policies, and approves or denies access.  

In short:  
- **AD** = the *database of identity and rules*  
- **DC** = the *server that enforces them*  

---

## Server Config
- NIC: Host-only (VMnet1)  
- Static IP: `192.168.1.200`  
- Gateway: `192.168.1.1` (pfSense LAN)  
- DNS: itself (`192.168.1.200`)  

---

## AD DS Install
1. Rename the server → `SOC-AD1`  
2. In Server Manager → Add **Active Directory Domain Services** + **DNS**  
3. Promote to Domain Controller → create new forest: `soc.lab`  
4. NetBIOS name: `SOC`  
5. Set DSRM password → reboot  

---

## DNS Fixes
- NIC Preferred DNS = `127.0.0.1`  
- NIC Alternate DNS = `192.168.1.1`  
- In DNS Manager → Forwarders tab = add `8.8.8.8`, `1.1.1.1`  
- Restart DNS service  
- Test with:
  ```powershell
  nslookup google.com
  ping google.com

## Populate AD with BadBlood

BadBlood is a PowerShell script that populates AD with **users, groups, computers, and misconfigurations**, making the lab environment look like a real enterprise.

### Installation
```powershell
git clone https://github.com/davidprowe/BadBlood.git
cd BadBlood
Set-ExecutionPolicy Bypass -Scope Process -Force
.\Invoke-BadBlood.ps1

### SOC-Relevance

The Domain Controller is the identity backbone of an enterprise. Almost every attacker interacts with AD at some point.

With BadBlood, your lab generates realistic events for SOC practice:

Failed logons → brute-force and password spraying

Service account queries → Kerberoasting detection

Misconfigured accounts → AS-REP roasting detection

Group membership changes → privilege escalation or insider abuse

Instead of a sterile AD install, your lab now feels like a real corporate directory. SOC analysts can investigate identity-based attacks with realistic noise and complexity, just like they would in production.


