# Network Architecture

```text
                                        INTERNET
                                            │
                                            │
                                     VMnet0 (WAN)
                                            │
                                            │
                              WAN - DHCP (Bridged Network)
                                            │
                                   +-----------------+
                                   |    pfSense      |
                                   | Perimeter FW    |
                                   +-----------------+
                                  │        │
                 LAN              │        │               DMZ
        VMnet2 192.168.10.0/24    │        │      VMnet3 192.168.20.0/24
                                  │        │
                                  │        │
                                  │        └──────────────┐
                                  │                       │
                                  │                 Ubuntu Server
                                  │                 Apache + DVWA
                                  │
                                  │
                           OPNsense Firewall
                         WAN:192.168.10.2
                         LAN:192.168.30.1
                                  │
                                  │
                   VMnet4 192.168.30.0/24
                                  │
         ┌───────────────┬───────────────┬───────────────┐
         │               │               │               │
   Windows 11      Kali Linux     Wazuh Server     Future Expansion
     Employee        Attacker          SIEM
```

## 3. Network Zones

### Zone 1 - Internet (WAN)
- **Purpose**: Represents the public Internet.
- **Network**: VMnet0
- **Type**: Bridged
- **Connected Devices**: pfSense WAN
- **IP Assignment**: DHCP from your home router.

### Zone 2 - Enterprise LAN
- **Purpose**: Represents the internal corporate network.
- **Subnet**: `192.168.10.0/24`
- **Gateway**: `192.168.10.1`
- **Firewall**: pfSense
- **Connected Devices**: pfSense LAN, OPNsense WAN
- **Allowed Traffic**: Internet access, Internal routing, DMZ access, Firewall management
- **Blocked Traffic**: Direct external access

### Zone 3 - DMZ
- **Purpose**: Public-facing servers.
- **Subnet**: `192.168.20.0/24`
- **Gateway**: `192.168.20.1`
- **Connected Devices**: Ubuntu Server
- **Services**: Apache, PHP, MariaDB, DVWA
- **Allowed Access**: HTTP, HTTPS, SSH (Admin only)
- **Blocked**: Access to Management Network

### Zone 4 - Management Network
- **Purpose**: Secure internal management network.
- **Subnet**: `192.168.30.0/24`
- **Gateway**: `192.168.30.1`
- **Connected Devices**: Windows 11, Kali Linux, Wazuh, OPNsense LAN
- **Purpose**: Administration, Security monitoring, Attack simulation

## 4. VMware Virtual Network Design

| VMnet | Type | Subnet | Gateway | DHCP |
|-------|------|--------|---------|------|
| VMnet0 | Bridged | Home Network | Home Router | Yes |
| VMnet2 | Host-only | 192.168.10.0/24 | 192.168.10.1 | No |
| VMnet3 | Host-only | 192.168.20.0/24 | 192.168.20.1 | No |
| VMnet4 | Host-only | 192.168.30.0/24 | 192.168.30.1 | No |

*VMware DHCP will be disabled on VMnet2, VMnet3, and VMnet4. pfSense will provide DHCP where needed.*

## 5. Virtual Machine Inventory

| VM Name | Purpose | CPU | RAM | Disk |
|---------|---------|-----|-----|------|
| FW-PFSENSE | Perimeter Firewall | 2 | 1 GB | 8 GB |
| FW-OPNSENSE | Internal Firewall | 2 | 2 GB | 8 GB |
| SRV-UBUNTU | Web Server + DVWA | 2 | 2 GB | 20 GB |
| SRV-WAZUH | SIEM | 2 | 4 GB | 30 GB |
| WIN11-CLIENT | Employee Workstation | 2 | 4 GB | 20 GB |
| KALI-ATTACK | Attack Machine | 2 | 2 GB | 20 GB |

**Total Allocated RAM**: 15 GB
*Actual usage will be lower because not all VMs need to run simultaneously.*

## 6. IP Address Plan

| Device | Interface | Address |
|--------|-----------|---------|
| pfSense | WAN | DHCP |
| pfSense | LAN | 192.168.10.1 |
| pfSense | DMZ | 192.168.20.1 |
| OPNsense | WAN | 192.168.10.2 |
| OPNsense | LAN | 192.168.30.1 |
| Ubuntu | eth0 | 192.168.20.10 |
| Wazuh | eth0 | 192.168.30.10 |
| Windows 11 | eth0 | DHCP |
| Kali | eth0 | DHCP |

## 7. Data Flow

**Internet Browsing**
```text
Windows
   │
OPNsense
   │
pfSense
   │
Internet
```

**Public Website Access**
```text
Internet
   │
pfSense
   │
Ubuntu
```

**Attack Simulation**
```text
Kali
 │
 ├── SQL Injection
 ├── Nmap
 ├── Hydra
 ├── Metasploit
 └── XSS
         │
         ▼
       Ubuntu
```

**Alert Flow**
```text
Suricata
    │
    ▼
  Wazuh
    │
    ▼
Dashboard
```

**Log Collection**
```text
pfSense
   │
OPNsense
   │
Ubuntu
   │
Windows
   │
Suricata
   │
   ─────────────► Wazuh
```

## 8. Security Layers

```text
Layer 1: Internet
       ↓
Layer 2: pfSense Firewall
       ↓
Layer 3: DMZ
       ↓
Layer 4: OPNsense Firewall
       ↓
Layer 5: Enterprise Network
       ↓
Layer 6: Suricata IDS/IPS
       ↓
Layer 7: Wazuh SIEM
       ↓
Layer 8: Security Analyst
```
