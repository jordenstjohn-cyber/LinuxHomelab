# Networking

Documentation for the network infrastructure in the homelab.


## Current Network Components

| Device | Role | Status |
|---|---|---|
| HP EliteDesk 800 G5 | Proxmox Host | Active |
| MikroTik hEX RB750Gr3 | Router | Active |
| Raspberry Pi | Pi-hole DNS | Active |
| Acer W6 | Wi-Fi | Active |


## Network Architecture

Internet
   │
   ▼
MikroTik RB750Gr3
   │
   ├── Pi-hole
   │
   ├── Proxmox
   │
   └── Acer W6
          │
          └── Wi-Fi clients
          
The final architecture is still being refined as routing, DHCP, DNS, and wireless responsibilities are consolidated.


## Topics

### MikroTik

RouterOS configuration, routing, DHCP, NAT, and troubleshooting.

Read MikroTik documentation →⁠￼

### Acer W6

Wi-Fi and router/access-point configuration.

Read W6 documentation →⁠￼

### IP Addressing

Documentation of the addressing scheme used throughout the lab.

Read IP addressing documentation →⁠￼

⸻

## Skills Practiced

* IPv4 addressing
* DHCP
* DNS
* NAT
* Routing
* Default gateways
* Static IPs
* Wireless networking
* Network troubleshooting

