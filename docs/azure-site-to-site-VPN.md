# Azure Site-to-Site VPN Integration

## 1. Project Overview

This project extended my existing Proxmox-based cybersecurity homelab into Microsoft Azure using an IKEv2/IPsec site-to-site VPN.
The objective was to establish secure communication between an isolated on-premises cybersecurity network and an Azure Virtual Network.
The initial plan was to deploy a secondary Active Directory domain controller in Azure, enabling hybrid-cloud identity management and Active Directory replication.
However, due to the recurring costs associated with Azure VPN Gateway, the project was limited to establishing and validating the VPN tunnel.

## Technologies Used

| Technology | Purpose |
|------------|---------|
| Microsoft Azure | Cloud infrastructure hosting the virtual network and VPN gateway |
| Azure Virtual Network (VNet) | Provides an isolated cloud network with the `10.30.0.0/16` address space |
| Azure VPN Gateway | Establishes an encrypted site-to-site VPN connection with the on-premises network |
| Azure Local Network Gateway | Defines the on-premises VPN endpoint and CyberLab address space |
| OPNsense | Acts as the on-premises firewall, router, and IPsec VPN endpoint |
| strongSwan | Handles IKEv2 negotiation, authentication, and IPsec security associations |
| Proxmox VE | Hosts the virtualized CyberLab infrastructure |
| IPsec Virtual Tunnel Interface (VTI) | Enables route-based communication between the on-premises and Azure networks |
| IKEv2/IPsec | Provides authenticated and encrypted communication between VPN endpoints |
| NAT Traversal (NAT-T) | Allows the IPsec connection to operate behind the Spectrum router's NAT |
| Active Directory Domain Services | Existing on-premises identity infrastructure targeted for future hybrid-cloud integration |

## Network Architecture

| Component | Network / Address | Role |
|-----------|-------------------|------|
| Spectrum Router | `192.168.1.0/24` | Provides Internet connectivity and NAT for the on-premises environment |
| Proxmox Cluster | `192.168.1.0/24` | Hosts the virtualized CyberLab infrastructure |
| OPNsense WAN | `192.168.1.93` | On-premises IPsec endpoint behind the Spectrum router |
| OPNsense LAN | `10.20.0.1` | Default gateway for the CyberLab network |
| CyberLab Network | `10.20.0.0/24` | Isolated network containing security tools and Windows systems |
| DC01 | `10.20.0.50` | Primary Active Directory domain controller for `cyberlab.test` |
| Kali Linux | `10.20.0.x` (DHCP) | Security testing and network connectivity validation |
| Azure Virtual Network | `10.30.0.0/16` | Cloud-side network connected through the VPN |
| Azure Server Subnet | `10.30.10.0/24` | Reserved for future Azure virtual machines |
| Azure Gateway Subnet | `10.30.255.0/27` | Dedicated subnet hosting Azure VPN Gateway infrastructure |
| Azure VPN Gateway | Public IP redacted | Cloud-side IKEv2/IPsec endpoint |
| OPNsense VTI | `10.111.1.1` | Local route-based IPsec tunnel interface |
| Azure VTI Next Hop | `10.111.1.2` | Configured remote tunnel gateway address |
| IPsec Static Route | `10.30.0.0/16 → AZURE_VPN_GW` | Routes Azure-bound traffic through the IPsec tunnel |

## VPN Configuration Summary

| Setting | Configuration |
|---------|---------------|
| VPN Architecture | Site-to-Site |
| VPN Protocol | IKEv2/IPsec |
| VPN Type | Route-Based |
| Authentication | Pre-Shared Key (PSK) |
| Azure Gateway SKU | VpnGw1AZ |
| Azure Region | East US 2 |
| On-Premises Firewall | OPNsense |
| IPsec Implementation | strongSwan |
| NAT Traversal | Enabled |
| Local Network | `10.20.0.0/24` |
| Remote Network | `10.30.0.0/16` |
| VTI Interface | `IPSEC10` |
| VTI Reqid | `10` |
| Local Tunnel Address | `10.111.1.1` |
| Configured Remote Tunnel Address | `10.111.1.2` |
| BGP | Disabled |
| Routing | Static |
| Static Route | `10.30.0.0/16` via `AZURE_VPN_GW` |
| Traffic Selectors | `0.0.0.0/0` (both directions) |
| IPsec Child Policies | Disabled (route-based configuration) |
