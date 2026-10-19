# FortiGate Firewall Lab

**Hands-on Lab: Deploying & Configuring Fortinet FortiGate Virtual Firewall**

This repository documents a practical cybersecurity lab where I deployed a FortiGate-VM firewall, configured security policies with NAT, analyzed traffic logs, and detected suspicious activity (port scans).

---

##  Tools Used

| Tool                    | Purpose                              |
|-------------------------|--------------------------------------|
| Fortinet FortiGate-VM   | Virtual Next-Generation Firewall     |
| Kali Linux VM           | Attacker / Test Client               |
| Windows 10/11 VM        | Internal LAN Client                  |
| VirtualBox / VMware     | Virtualization Platform              |
| FortiGate CLI & GUI     | Configuration & Log Analysis         |

---

##  Lab Topology

```
                    Internet (WAN)
                          |
                    [ FortiGate-VM ]
                     /           \
               LAN Zone       DMZ (optional)
                  |               
          [ Windows Client ]     [ Kali Linux ]
```

**Zones:**
- **WAN** → External / Untrusted network
- **LAN** → Internal trusted network (Windows client)
- **Test Client** → Kali Linux used to generate legitimate and suspicious traffic

---

##  Objectives

1. Deploy FortiGate-VM in a virtual lab environment
2. Configure 3 firewall policies with NAT
3. Allow/block traffic between LAN and WAN zones
4. Analyze Forward Traffic logs
5. Detect suspicious activity (port scans)
6. Document findings and recommend rule improvements

---

##  Configuration Summary

### 1. Interface Configuration

| Interface | Zone  | IP Address       | Role          |
|-----------|-------|------------------|---------------|
| port1     | WAN   | 192.168.1.1/24   | External      |
| port2     | LAN   | 10.0.0.1/24      | Internal      |

### 2. Firewall Policies Created

#### Policy 1: LAN → WAN (Allow with NAT)
- **Source**: LAN zone / 10.0.0.0/24
- **Destination**: WAN / All
- **Service**: HTTP, HTTPS, DNS, PING
- **Action**: ACCEPT
- **NAT**: Enabled (Outgoing Interface Address)
- **Logging**: All Sessions

#### Policy 2: WAN → LAN (Block by default)
- **Source**: WAN
- **Destination**: LAN
- **Service**: ALL
- **Action**: DENY
- **Logging**: All Sessions
- **Purpose**: Prevent unsolicited inbound traffic

#### Policy 3: Specific Allow (Management / Testing)
- **Source**: Specific trusted IP (optional)
- **Destination**: FortiGate management
- **Service**: HTTPS, SSH, PING
- **Action**: ACCEPT
- **Logging**: All Sessions

> Full CLI configuration is available in [`configs/firewall-policies.conf`](configs/firewall-policies.conf)

---

##  Testing & Traffic Analysis

### Test Cases Performed

| Test Case                      | Expected Result          | Actual Result     | Status |
|--------------------------------|--------------------------|-------------------|--------|
| Ping from LAN to WAN           | Allowed                  | Allowed           | Pass   |
| HTTP/HTTPS browsing from LAN   | Allowed                  | Allowed           | Pass   |
| Inbound connection from WAN    | Blocked                  | Blocked           | Pass   |
| Port Scan from Kali (Nmap)     | Detected in logs         | Detected          | Pass   |
| DNS query from LAN             | Allowed                  | Allowed           | Pass   |

### Suspicious Activity Detection

- Generated a **port scan** from Kali Linux (`nmap -sS -p 1-1000 10.0.0.1`)
- FortiGate Forward Traffic logs clearly showed multiple blocked connection attempts
- Logs were filtered by source IP and action = Deny

Sample log entries are available in the [`logs/`](logs/) folder.

---

##  Key Findings

- Default deny policy effectively blocked unsolicited inbound traffic
- NAT was correctly applied on outbound connections
- Port scan attempts were successfully logged with source IP, destination ports, and action
- Logging "All Sessions" provided complete visibility for both allowed and denied traffic

---

##  Recommendations

1. Enable **IPS (Intrusion Prevention)** profile on the LAN → WAN policy for better threat detection
2. Create a dedicated **Geo-IP** or **Threat Intelligence** based deny policy
3. Enable **Application Control** to restrict high-risk applications
4. Configure **Local-in policies** for tighter management plane protection
5. Set up log forwarding to a SIEM (e.g., Splunk / FortiAnalyzer) for centralized monitoring
6. Implement **SSL Inspection** (careful with performance impact in lab)

---

##  Repository Structure

```
FortiGate-Firewall-Lab/
├── README.md
├── configs/
│   └── firewall-policies.conf      # CLI configuration
├── logs/
│   ├── allowed-traffic-sample.log
│   └── blocked-portscan.log
├── screenshots/                    # Add your screenshots here
│   └── (policy-config, logs, topology)
└── docs/
    └── lab-report.md               # Detailed lab report
```

---

##  How to Reproduce (High-Level)

1. Download FortiGate-VM (Fortinet support portal – evaluation license)
2. Deploy in VirtualBox/VMware with 2 network adapters (WAN + LAN)
3. Configure interfaces and static routes
4. Create the three policies as documented
5. Generate traffic from Windows (normal) and Kali (port scan)
6. Review **Log & Report → Forward Traffic**

---

##  Author

**Manoj Kumar V M**  
Cybersecurity Enthusiast | SOC Analyst Aspirant  

- GitHub: [github.com/manojkumarvm](https://github.com/manojkumarvm)
- LinkedIn: [www.linkedin.com/in/manoj-kumar-vm
]
- Email: manumanoj2422@gmail.com

---

**Note**: This is a personal lab project for learning purposes. All configurations were tested in a controlled virtual environment.
