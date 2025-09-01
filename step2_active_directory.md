# Step 2 – Active Directory Domain Controller (Windows Server)

Windows Server runs **Active Directory Domain Services (AD DS)**. This becomes the **Domain Controller (DC)** for users, groups, and authentication.  

## Server Config
- NIC: Host-only (VMnet1)  
- Static IP: `192.168.1.200`  
- Gateway: `192.168.1.1` (pfSense LAN)  
- DNS: itself (`192.168.1.200`)  

## Installation
1. Rename server → `SOC-AD1`  
2. In Server Manager → Add AD DS + DNS roles  
3. Promote to DC → new forest: `soc.lab`  
4. NetBIOS name: `SOC`  
5. Set DSRM password → reboot  

## DNS Fixes
- NIC Preferred DNS = `127.0.0.1`  
- NIC Alternate DNS = `192.168.1.1` (pfSense)  
- DNS Manager → Forwarders = add `8.8.8.8`, `1.1.1.1`  
- Restart DNS service  

## Populate AD with BadBlood
```powershell
git clone https://github.com/davidprowe/BadBlood.git
cd BadBlood
Set-ExecutionPolicy Bypass -Scope Process -Force
.\Invoke-BadBlood.ps1

