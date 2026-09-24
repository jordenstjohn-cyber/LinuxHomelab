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
* The default gateway determines where traffic leaves a network.
* Multiple routers can introduce additional NAT and routing layers.
* Network problems are easier to diagnose when each device has a clearly defined role.

⸻
