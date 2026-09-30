# Wazuh Deployment

## Architecture

Wazuh 4.14.8 virtual appliance
IP: 10.20.0.60
vCPU: 4
RAM: 8 GB

Monitored endpoints:
- W11-Attack
- DC01

<img width="1904" height="529" alt="Screen Shot 2026-09-30 at 17 05 21 PM" src="https://github.com/user-attachments/assets/51cf4a28-f958-431d-973c-55344dd99e6d" />


## Deployment

The official Wazuh OVA was imported into Proxmox.

During deployment, the appliance initially failed to
display correctly after GRUB.

Troubleshooting included:
- verifying the imported virtual disk
- verifying GRUB functionality
- changing the Proxmox CPU model
- changing VM machine compatibility settings
- testing Linux kernel console parameters

The appliance subsequently booted successfully.

## Windows Telemetry

Windows endpoints use:

Sysmon
    ->
Windows Event Log
    ->
Wazuh Agent
    ->
Wazuh Manager
    ->
Wazuh Dashboard
