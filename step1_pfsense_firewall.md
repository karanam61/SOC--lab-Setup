# Step 1 – pfSense Firewall Setup

pfSense is the firewall for the lab. It handles DHCP, NAT, and DNS forwarding so the internal network can talk to the internet safely.  

## VM Network Settings
- WAN → VMware NAT (VMnet8)  
- LAN → VMware Host-only (VMnet1)  
- (Optional) DMZ → another Host-only network  

## pfSense Config
- LAN IP: `192.168.1.1/24`  
- DHCP range: `192.168.1.4 – 192.168.1.124`  
- Outbound NAT: **Automatic**  
- LAN firewall rule: Allow LAN net → Any  

## Validation
- From pfSense shell: `ping 8.8.8.8`  
- From LAN host:  
  - `ping 192.168.1.1` (gateway)  
  - `ping 8.8.8.8` (internet routing works)  
  - `nslookup google.com` (DNS resolution works)  

## Common Problems
- Can ping gateway but not internet → NAT not automatic, or LAN firewall rule missing.  
- WAN stuck at 0.0.0.0 → pfSense WAN NIC not mapped to NAT network.  
- DHCP not working → pfSense powered off, or LAN NIC mapped wrong.  

## SOC Relevance
pfSense is the **perimeter**. Every packet crosses it. SOC analysts use firewall logs to detect scans, brute-force attempts, and suspicious outbound traffic.
