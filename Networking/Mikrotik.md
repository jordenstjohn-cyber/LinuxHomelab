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

```bash 
cd ~/Downloads
ls -lh
```

Identify the WinBox file you downloaded.

If it is an AppImage, make it executable:

```bash
chmod +x ./winbox*.AppImage
```

Then launch it:

```Bash
./winbox*.AppImage
```

The wildcard is convenient if only one matching AppImage exists in the directory. If the downloaded file has a different name or format, use its actual filename and follow the instructions provided by MikroTik.

Note: WinBox’s Linux packaging can change. If the download is a different format, do not assume the AppImage commands apply.

### Fedora compatibility troubleshooting

If WinBox fails to launch:

1. Check the downloaded file’s format and architecture.
2. Read any error messages printed in the terminal.
3. Confirm that the file has executable permissions if required.
4. Check whether the application requires additional system libraries.

Avoid installing dependencies or running downloaded files with sudo unless there is a specific, understood reason.

## 2. Connecting to the MikroTik Router

WinBox can connect using an IP address or, when the router is discoverable on the local network, its MAC address.

1. Connect the Fedora workstation to the appropriate LAN.
2. Open WinBox.
3. Locate the router in the Neighbors list, or enter its management address.
4. Enter your RouterOS username and password.
5. Connect and confirm that the RouterOS interface loads.

Use a trusted local connection for initial administration. MAC-based access is useful for local discovery and recovery, but it does not replace properly configured IP connectivity or firewall rules.

### Security practices

* Use a strong, unique RouterOS administrator password.
* Avoid exposing the WinBox management service directly to the internet.
* Restrict management access to trusted devices or networks.
* Keep RouterOS updated after reviewing the release and upgrade requirements.
* Do not commit passwords, private keys, tokens, or unredacted configuration exports to GitHub.

## 3. RouterOS Command-Line Administration

RouterOS can also be administered through its terminal interface in WinBox or through SSH when SSH access is enabled and permitted by the router’s configuration.

Useful read-only commands include:

| Command | Purpose |
|-|-|
| /system resource print | Inspect router resources |
| /system identity print | Display the router’s configured identity |
| /system package print | Review installed RouterOS packages |
| /interface print | Inspect network interfaces |
| /ip address print | Review configured IP addresses |
| /ip route print | Inspect routing-table entries |
| /ip dhcp-server print | Review DHCP server configuration |
| /ip dns print | Inspect DNS configuration |
| /ip firewall filter print | Review firewall filter rules |
| /ip firewall nat print | Review NAT rules |

These commands help build familiarity with RouterOS and provide a starting point for troubleshooting.

Privacy: Do not publish raw command output without reviewing it. Outputs can reveal internal addresses, hostnames, network structure, and security configuration.

## 4. Fedora Network Diagnostics

Fedora provides useful tools for diagnosing connectivity from the administration workstation.

Inspect local network configuration

```bash
ip address
```

Display the routing table:

```bash
ip route
```

Inspect DNS configuration:

```bash
resolvectl status
```

If resolvectl is unavailable or not managing DNS on the system, inspect the active NetworkManager connection and its DNS settings instead.

### Test connectivity

Test connectivity to your configured gateway using its actual address:

```bash
ping -c 4 <ROUTER_LAN_IP>
```

Test DNS resolution:

```bash
getent hosts example.com
```

Test HTTPS connectivity:

```bash
curl -I --connect-timeout 5 https://example.com
```

Replace <ROUTER_LAN_IP> with the router’s LAN address on your local system. Keep the actual value out of the public repository.

## 5. Pi-hole DNS Integration

Pi-hole provides DNS filtering for devices on the local network.

The intended DNS flow is:

Client Device
     |
     v
MikroTik DHCP Configuration
     |
     v
Pi-hole DNS Resolver
     |
     v
Upstream DNS Resolver
     |
     v
Internet

The router’s DHCP configuration can distribute the Pi-hole address to clients as their DNS server.

When troubleshooting DNS, verify:

1. The client has a valid network configuration.
2. The client has the expected DNS server.
3. Pi-hole is running and listening for DNS requests.
4. The router’s DHCP settings are correct.
5. Pi-hole can reach its upstream resolver.
6. Firewall rules allow the required DNS traffic.

Useful Fedora checks include:

```bash
ip address
ip route
resolvectl status
getent hosts example.com
```

Do not change DHCP or DNS settings on a working network without first recording the existing configuration and planning how to recover if connectivity is lost.

6. IP Address Documentation

Actual IP addresses and network details are intentionally omitted from this public repository.

Use placeholders when documenting configuration:

| Device | Example identifier |
|-|-|
| MikroTik router | <ROUTER_LAN_IP> |
| Pi-hole | <PIHOLE_IP> |
| Proxmox host | <PROXMOX_IP> |
| Debian server | <DEBIAN_SERVER_IP> |
| Fedora workstation | <FEDORA_WORKSTATION_IP> |

The placeholders are documentation labels, not literal addresses to enter into RouterOS.

Keep a separate local inventory containing your real IP assignments and other private infrastructure details.

## 7. Troubleshooting Notes

WinBox cannot find the router

* Confirm the workstation is connected to the correct network.
* Check whether the router appears in WinBox’s Neighbors list.
* Verify that the relevant interface is enabled and connected.
* Try connecting using the router’s known LAN management address.
* Check whether firewall rules restrict management access.

Router is reachable, but internet access fails

* Inspect the routing table.
* Check the WAN interface’s connection status.
* Verify that a valid default route exists.
* Review NAT and firewall rules.
* Check whether the upstream gateway is reachable.

Devices cannot resolve domain names

* Inspect the client’s DNS settings.
* Confirm the DHCP server distributes the intended DNS resolver.
* Check Pi-hole’s service status and query logs.
* Test resolution from both the client and the Pi-hole host.

Changes cause loss of access

Before making configuration changes:

* Save a backup of the current router configuration.
* Record the settings being changed.
* Make one logical change at a time.
* Ensure you have a local recovery method.
* Avoid applying unfamiliar firewall or management-access rules remotely without a recovery plan.

## 8. Skills Demonstrated

This project demonstrates practical experience with:

* MikroTik RouterOS administration
* Network routing and DHCP fundamentals
* DNS configuration and troubleshooting
* WinBox and SSH-based administration
* Fedora Linux network diagnostics
* Firewall and NAT concepts
* Network documentation and topology diagrams
* Infrastructure troubleshooting
* Secure configuration management

## 9. Future Improvements

Planned additions to this networking project include:

* Documenting VLANs and network segmentation
* Creating a dedicated management network
* Improving DNS redundancy and recovery procedures
* Adding network monitoring and logging
* Automating configuration backups
* Documenting firewall rules and traffic flows
* Testing network failure and recovery scenarios
* Integrating networking documentation with the wider Proxmox homelab

Disclaimer

This repository documents a personal homelab for educational and professional development purposes. Configuration examples should be adapted and validated before use in another environment.
