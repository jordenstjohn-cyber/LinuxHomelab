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

```text
Internet
   │
   ▼
MikroTik RB750Gr3
   │
   ├── Pi-hole
   │
   ├── Proxmox
   │
   └── Acer Predator Connect W6
          │
          └── Wi-Fi Clients
```
          
The final architecture is still being refined as routing, DHCP, DNS, and wireless responsibilities are consolidated.


## Topics

### MikroTik

RouterOS configuration, routing, DHCP, NAT, and troubleshooting.

Read MikroTik documentation →⁠￼[MikroTik](https://help.mikrotik.com/docs/spaces/UM/pages/8978720/hEX)

### Acer W6

Wi-Fi and router/access-point configuration.

Read W6 documentation →⁠￼[Acer W6](https://global-download.acer.com/GDFiles/Document/User%20Manual/User%20Manual_Acer_1.0_A_A.pdf?acerid=638580771619453908&Step3=PREDATOR%20CONNECT%20W6X%20WI-FI%20GAMING%20ROUTER&OS=ALL&LC=en&BC=ACER&SC=PA_6)

### IP Addressing

Documentation of the addressing scheme used throughout the lab.

Read IP addressing documentation →⁠￼coming soon

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


# MikroTik hEX RB750Gr3

The MikroTik hEX RB750Gr3 was introduced as the primary routing platform for the homelab.

## Role

The intended responsibilities are:

* WAN connectivity
* Routing
* NAT
* DHCP
* LAN management
* Firewall

⸻

## Initial Configuration

The router was configured with:
WAN / ether1
192.168.X.XXX

LAN / bridge
192.168.XX.XX

The LAN DHCP network initially used:
Network: 192.168.XX.XX
Gateway: 192.168.XX.XX
DNS:     192.168.XX.XX

## Initial Architecture

```text
Internet
   │
   ▼
MikroTik
   │
   ▼
Acer W6
   │
   ▼
Clients
```

The W6 was initially operating in router mode.

⸻

## Troubleshooting

After introducing the MikroTik into the network, internet performance became significantly slower.

The troubleshooting process involved investigating:

* Multiple routers
* NAT
* DHCP
* Default gateways
* Network addressing
* Router vs AP mode

This became a practical exercise in understanding how multiple network devices can interact when they are both performing Layer 3 functions.

⸻

## Lessons Learned

* A router and access point have different responsibilities.
* DHCP determines more than just a client’s IP address.
