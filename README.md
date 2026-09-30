# cyberlab

A three-node Proxmox cybersecurity homelab designed for
blue-team and purple-team experimentation.

## Objectives

- Build an isolated Active Directory environment
- Generate realistic endpoint and network telemetry
- Centralize security events using Wazuh
- Detect and investigate simulated attacks
- Develop custom detection rules
- Practice SIEM investigation workflows

## Infrastructure

- Optiplex 3050
- Optiplex 3070
- Optiplex 5090
- Cisco 8 port 1 gig switch
- Omada 2.5G 5 port switch

### Physical
- 3-node Dell OptiPlex Proxmox cluster
- Dedicated 2.5 GbE backend network
- Separate 1 GbE management network

### Security Environment
- OPNsense firewall/router
- Suricata IDS
- Windows Server 2025 Domain Controller
- Windows 11 workstation
- Kali Linux attacker
- Wazuh SIEM/XDR
- Sysmon endpoint telemetry

## Network Architecture

Management: 192.168.1.0/24
Cluster backend: 10.10.10.0/24
Cyber range: 10.20.0.0/24
