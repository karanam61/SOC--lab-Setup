# Step 3 – Windows Workstation (Join to AD Domain)

This step adds a Windows 10/11 workstation to the **soc.lab** domain. In a SOC lab, the workstation simulates a normal employee machine that authenticates to AD, follows group policies, and generates endpoint activity for monitoring.

---

## Objective
- Connect a workstation VM to the same LAN as the Domain Controller.
- Configure networking to use the **Domain Controller’s DNS**.
- Join the workstation to the **soc.lab** domain.
- Validate domain login, name resolution, and internet access.
- Understand what logs and events this generates for SOC purposes.

---

## Prerequisites
- pfSense firewall up and running with:
  - LAN: `192.168.1.1/24`
  - NAT outbound set to Automatic
  - DHCP configured or manual IPs allowed
- Domain Controller (SOC-AD1) running Windows Server 2022 with:
  - IP: `192.168.1.200`
  - DNS service configured with forwarders
  - Domain/forest: `soc.lab` (NetBIOS: `SOC`)
- Workstation VM connected to **Host-only (VMnet1)** same as DC.

---

## Network Configuration

### Static IP (recommended)
- **IP:** `192.168.1.20`
- **Subnet mask:** `255.255.255.0`
- **Gateway:** `192.168.1.1` (pfSense LAN)
- **Preferred DNS:** `192.168.1.200` (Domain Controller)
- **Alternate DNS:** leave blank or use `192.168.1.1`

## Join the Domain
- GUI Method
- Right-click This PC → Properties
- Click Advanced system settings
- In System Properties, open the Computer Name tab
- Click Change…
- Select Domain, enter: soc.lab
- Provide domain admin credentials → e.g., SOC\Administrator
- If successful, you’ll see “Welcome to the soc.lab domain”
- Restart the workstation
