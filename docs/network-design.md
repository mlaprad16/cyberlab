# Network Design

## Overview

The CyberLab network is designed to separate management, cluster,
and cybersecurity lab traffic while allowing virtual machines to
communicate across a three-node Proxmox cluster.

The environment uses three primary networks:

| Network | Subnet | Purpose |
|---|---|---|
| Management | 192.168.1.0/24 | Proxmox management and general LAN connectivity |
| Cluster Backend | 10.10.10.0/24 | Dedicated 2.5 GbE inter-node traffic |
| CyberLab | 10.20.0.0/24 | Isolated cybersecurity lab environment |

No public IP addresses are documented in this repository.

---

## Network Architecture

The physical Proxmox hosts have two primary network paths:

1. A 1 GbE connection to the management network.
2. A dedicated 2.5 GbE connection to the cluster backend network.

Virtual cybersecurity systems communicate through a Proxmox SDN
network spanning the cluster.

OPNsense provides routing and firewall services between the CyberLab
and the upstream network.

```text
                        Internet
                            |
                     [ISP / Home Router]
                            |
                    192.168.1.0/24
                     Management LAN
                            |
        +-------------------+-------------------+
        |                   |                   |
   Proxmox-3050        Proxmox-3070        Proxmox-5090
   192.168.1.172       192.168.1.67        192.168.1.43
        |                   |                   |
   10.10.10.11         10.10.10.12         10.10.10.13
        |                   |                   |
        +----------[2.5 GbE Switch]-------------+
                            |
                    10.10.10.0/24
                            |
                     Cluster Backend
                   Proxmox SDN / VXLAN
                            |
                     CyberLab Network
                      10.20.0.0/24
                            |
                        OPNsense
                       10.20.0.1
                            |
       +--------------------+--------------------+
       |                    |                    |
      DC01              W11-Attack             Wazuh
   10.20.0.50               DHCP            10.20.0.60
 Active Directory       Windows Target       SIEM / XDR
       |                                         |
       +--------------------+--------------------+
                            |
                           Kali
                           DHCP
                     Attack Simulation
