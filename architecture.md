## Proxmox Cluster

The environment consists of three Proxmox VE nodes.

| Node | Management | Backend |
|---|---|---|
| proxmox-3050 | Redacted | 10.10.10.11 | 
| proxmox-3070 | Redacted | 10.10.10.12 | 
| proxmox-5090 | Redacted | 10.10.10.13 | 

A dedicated 2.5 GbE network separates cluster/backend
traffic from the home management network.

Testing with iperf3 produced approximately 2.35 Gbps
between nodes.
