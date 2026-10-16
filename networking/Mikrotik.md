# Fedora Linux & MikroTik Networking Lab

## Overview

This project documents the configuration, administration, and troubleshooting of a homelab network using a Fedora Linux ThinkPad T480s as the primary administration workstation and a MikroTik hEX RB750Gr3 as the network gateway.

The project combines Linux command-line administration with practical networking concepts, including IPv4 addressing, subnetting, routing, DHCP, DNS, and Network Address Translation (NAT).

The network integrates a MikroTik router, a Raspberry Pi running Pi-hole, an Acer Predator Connect W6, and a Proxmox virtualization environment.

The objective is to develop practical Linux and networking skills through hands-on infrastructure configuration, connectivity testing, and technical documentation.

## Objectives

* Administer network infrastructure from a Fedora Linux workstation.
* Establish secure administrative access to a MikroTik router.
* Configure and understand IPv4 addressing and subnetting.
* Investigate routing tables, default gateways, and NAT.
* Understand DHCP address allocation and DNS configuration.
* Integrate Pi-hole for network-wide DNS filtering.
* Troubleshoot connectivity between wired and wireless devices.
* Use Linux networking utilities to diagnose network problems.
* Document network architecture and configuration decisions.
* Build a foundation for advanced networking, Linux administration, and infrastructure automation.

## Hardware and Software

| Component | Platform | Role |
|-|-|-|
| ThinkPad T480s | Fedora Linux KDE | Network administration and diagnostics |
| MikroTik hEX RB750Gr3 | MikroTik RouterOS | Routing, DHCP, NAT, and LAN gateway |
| Raspberry Pi | Raspberry Pi OS | Pi-hole DNS filtering |
| Acer Predator Connect W6 | Router / Wi-Fi system | Wireless connectivity |
| Proxmox host | Proxmox VE | Virtualization and server workloads |
| Debian virtual machine | Debian Linux | Linux server administration and self-hosted applications |

## Network Architecture

                 Internet
                    |
                    v
             [Internet Gateway]
                    |
                    v
              MikroTik hEX
            [LAN Router / DHCP]
                    |
          +---------+----------+
          |         |          |
          v         v          v
       Pi-hole   Proxmox    Fedora Laptop
        [DNS]    [Virtual     [Network
                  Machines]   Admin]
                    |
                    v
              Debian Server
            [WordPress / WooCommerce]

### 1. Installing WinBox on Fedora

WinBox is MikroTik’s graphical administration utility for managing RouterOS devices.

Official download page: ⁠[MikroTik WinBox](https://mikrotik.com/winbox)

Download WinBox

1. Open the official MikroTik download page.
2. Locate the WinBox download.
3. Download the Linux version appropriate for your Fedora workstation and system architecture.
4. Save the downloaded file somewhere convenient, such as ~/Downloads.

Check the downloaded file

Open a terminal:

'''bash 
cd ~/Downloads
ls -lh'''




