# Enterprise Network Security Operations Platform

## Overview

This project demonstrates the design and implementation of an optimized enterprise-grade network security environment using multiple firewalls, IDS/IPS, SIEM, VLAN segmentation, attack simulation, and security automation. The architecture is lightweight and optimized for host systems with 16GB RAM and ~100GB of available storage.

## Architecture & VMs

The lab uses 6 core VMs instead of a larger deployment to stay within resource limits, while still teaching core enterprise security concepts.

| VM | Purpose | RAM | Disk |
|----|---------|-----|------|
| **pfSense** | Perimeter Firewall (WAN/LAN) | 1 GB | 8 GB |
| **Ubuntu Server** | Web Server + DVWA | 2 GB | 20 GB |
| **Kali Linux** | Attacker | 2 GB | 20 GB |
| **Wazuh Server** | SIEM | 4 GB | 30 GB |
| **Windows 11** | Employee PC + Wazuh Agent | 4 GB | 20 GB |
| **OPNsense** | Internal Firewall + Suricata | 2 GB | 8 GB |

### Diagram

```text
                    Internet
                        │
                  Host Computer
                        │
                VMware Workstation
                        │
        ┌────────────────────────────────┐
        │                                │
        │      pfSense Firewall          │
        │           (WAN/LAN)            │
        └───────────────┬────────────────┘
                        │
                Internal Enterprise LAN
                        │
        ┌─────────┬─────────┬────────────┐
        │         │         │            │
    OPNsense   Ubuntu    Windows11    Kali Linux
   (Suricata)   + DVWA     Employee     Attacker
        │
   Wazuh Server
```

## Features Demonstrated

- Enterprise firewall configuration & Rules
- NAT & Network segmentation (DMZ, VLANs)
- Suricata IDS/IPS
- Wazuh SIEM & Log analysis
- Attack simulation (Nmap, Hydra, Metasploit)
- Web App Vulnerabilities (SQLi, XSS, CSRF, Command Injection via DVWA)
- Python automation & Incident response
- Documentation

## Resource Constraints & Usage

- **Host PC max RAM:** 16 GB (Lab uses 13 to 15 GB when fully running)
- **Host PC max Storage:** 100 GB (Lab uses 60 to 80 GB with thin provisioning)
- **Hypervisor:** VMware Workstation Pro (Windows host)
- **Snapshots:** One snapshot per VM to conserve space.

## Status

🚧 Project in Progress
