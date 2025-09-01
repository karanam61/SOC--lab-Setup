# Step 2 – Active Directory on Windows Server 2022

We set up the Domain Controller (DC) using **Windows Server 2022 Datacenter**.  
Active Directory (AD) is the backbone of enterprise identity and access management, so this step builds the identity layer of the lab.

---

## ⚠️ VMware Firmware Fix
Windows Server 2022 ISOs don’t always boot under UEFI in VMware.
- Change VM firmware: **UEFI → Legacy BIOS**.
- Otherwise you’ll get “Cannot trust the license or source” errors.
- After switching to BIOS, the ISO boots normally.

---

## What is AD and a Domain Controller?
- **Active Directory (AD)** is Microsoft’s directory service. It stores **users, groups, policies, and computers** in a central database.  
- **Domain Controller (DC)** is the Windows Server that runs AD DS (Active Directory Domain Services). It enforces the rules.  

### Example
Think of a company with 500 employees:
- Each user has an account in AD.
- Group policies control what employees can or can’t do (password length, software allowed, mapped drives).
- A workstation login isn’t checked against the local PC — it’s sent to the **Domain Controller**.  
- The DC says “yes” or “no” depending on AD credentials and policies.  

In short:  
AD = the **database of identity and rules**  
DC = the **server that enforces them**  

---

## Server Config
- NIC: Host-only (VMnet1)
- Static IP: `192.168.1.200`
- Gateway: `192.168.1.1` (pfSense LAN)
- DNS: itself (`192.168.1.200`)

---

## AD DS Install
1. Rename server → `SOC-AD1`
2. Add **Active Directory Domain Services** + **DNS** in Server Manager
3. Promote to DC → new forest: `soc.lab`
4. NetBIOS name: `SOC`
5. Set DSRM password → reboot

---

## DNS Fixes
- NIC Preferred DNS = `127.0.0.1`
- NIC Alternate DNS = `192.168.1.1`
- In DNS Manager → Forwarders = add `8.8.8.8`, `1.1.1.1`
- Restart DNS service
- Verify with:
  ```powershell
  nslookup google.com
  ping google.com

