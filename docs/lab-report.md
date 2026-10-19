# FortiGate Firewall Lab - Detailed Report

**Author:** Manoj Kumar V M  
**Date:** October 2026  
**Lab Environment:** Virtual (VirtualBox / VMware)

---

## 1. Introduction

This lab demonstrates the deployment and configuration of a Fortinet FortiGate virtual firewall. The primary goals were to implement basic network segmentation using firewall policies, enable NAT for outbound traffic, and practice log analysis for detecting suspicious activity such as port scans.

---

## 2. Lab Setup

### Virtual Machines
- **FortiGate-VM** (Evaluation License)
- **Windows 10/11 Client** (LAN zone – 10.0.0.10)
- **Kali Linux** (External/Test client – 192.168.1.50)

### Network Configuration
- WAN Interface (port1): 192.168.1.1/24
- LAN Interface (port2): 10.0.0.1/24

---

## 3. Configuration Steps

1. Deployed FortiGate-VM and completed initial setup via console/GUI.
2. Configured interfaces and assigned them to correct zones (WAN / LAN).
3. Created address objects for the LAN subnet.
4. Created three firewall policies:
   - LAN → WAN Allow (with NAT)
   - WAN → LAN Deny
   - Limited Management Access
5. Enabled full session logging on all policies.

---

## 4. Testing Methodology

### Legitimate Traffic
- Browsed websites (HTTP/HTTPS) from Windows client
- Performed DNS lookups
- Pinged external IPs

### Suspicious Traffic
- Executed Nmap SYN scan from Kali Linux against the FortiGate LAN IP:
  ```bash
  nmap -sS -p 1-1000 10.0.0.1
  ```

---

## 5. Results

- All legitimate outbound traffic was allowed and correctly NATed.
- All inbound connection attempts from the external Kali machine were denied.
- Port scan activity was clearly visible in the Forward Traffic logs with multiple Deny actions in a short time window.
- Logging provided full visibility into source/destination IPs, ports, and policy hits.

---

## 6. Conclusion & Learnings

This lab provided practical experience in:
- FortiGate policy creation and NAT
- Traffic log analysis
- Identifying reconnaissance activity (port scans)
- Understanding the importance of default-deny posture

**Next Steps:** Explore IPS profiles, Application Control, and log forwarding to a SIEM.

---

## 7. Screenshots

> Place the following screenshots in the `/screenshots` folder and reference them here:

1. FortiGate Dashboard / System Information
2. Interface Configuration
3. Firewall Policy list (showing the 3 policies)
4. Policy 1 edit page (showing NAT enabled)
5. Forward Traffic log – Allowed sessions
6. Forward Traffic log – Blocked port scan
7. Nmap scan output from Kali

---

**End of Report**
