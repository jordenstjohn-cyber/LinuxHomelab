# Pi-hole Homelab DNS Server

A self-hosted network-wide DNS filtering project using Pi-hole on a Raspberry Pi, integrated with a MikroTik hEX router and an Acer Predator Connect W6 Wi-Fi router.

This project is part of my homelab, where I’m developing practical Linux administration, networking, DNS, and infrastructure management skills.

## Project Overview

Pi-hole provides network-wide DNS filtering by blocking requests to domains associated with advertisements and tracking.

Rather than configuring filtering individually on every device, Pi-hole can serve as the DNS resolver for clients across the local network.

## Objectives

* Deploy Pi-hole on a Raspberry Pi.
* Configure DNS filtering for the home network.
* Integrate Pi-hole with a MikroTik hEX RB750Gr3 router.
* Connect the Acer Predator Connect W6 for Wi-Fi access.
* Learn how DHCP, DNS, gateways, and routing interact.
* Document configuration changes and troubleshooting procedures.
* Build practical Linux administration experience.

## Hardware and Software

| Component | Configuration | Role |
|---|---|---|
| Raspberry Pi | Raspberry Pi running Raspberry Pi OS | Pi-hole DNS filtering |
| Operating System | Raspberry Pi OS based on Debian 12 | Linux environment |
| DNS Software | Pi-hole | DNS filtering and resolution |
| Router | MikroTik hEX RB750Gr3 | Primary router and LAN gateway |
| Wi-Fi Router | Acer Predator Connect W6 | Wi-Fi connectivity |
| Virtualization Host | Proxmox VE | Hosts other homelab services |
| Administration Client | MacBook | Configuration and testing |

## Network Architecture

The intended network topology is:

```text
                         Internet
                            |
                            v
                    MikroTik hEX RB750Gr3
                       192.168.xxx.xxx
                            |
             +--------------+--------------+
             |              |              |
             v              v              v
          Pi-hole        Proxmox        Acer W6
       192.168.xxx.xxx                    Wi-Fi
             |                             |
             |                             v
             |                       Wi-Fi Clients
             |
             v
       DNS Filtering
```

The MikroTik acts as the primary router for the local network. Pi-hole provides DNS filtering, while the Acer W6 provides Wi-Fi connectivity.

Important: This diagram represents the intended design. Actual DNS behavior depends on the DHCP settings, DNS configuration, and operating mode of the routers.

## Network Configuration

The following addresses were used during configuration and troubleshooting.

| Device | IP Address | Purpose |
|---|---|---|
| MikroTik hEX | `192.168.8xx.xxx` | LAN gateway |
| Pi-hole | `192.168.8xx.xxx` | DNS server |
| Acer W6 | DHCP-assigned address | Wi-Fi connectivity |
| Proxmox | Not documented here | Virtualization host |

The Pi-hole previously used the address 192.168.7xx.xxx on a different network before being moved to the MikroTik LAN.

## DHCP and DNS

The intended configuration is for network clients to receive the appropriate network settings through DHCP and use Pi-hole for DNS resolution.
* LAN bridge address: 192.168.8xx.xxx/xx
* DHCP gateway: 192.168.8xx.xxx
* DHCP-provided DNS server: 192.168.8xx.xxx
* Dynamic upstream DNS servers previously included 192.168.xxx.xxx

The MikroTik’s upstream DNS configuration and the DNS server advertised to clients are separate settings.

The desired DNS flow is:

```text
Wi-Fi or Wired Client
          |
          v
     Pi-hole DNS
    192.168.88.207
          |
          v
   Upstream DNS Resolver
          |
          v
       Internet
```

## Pi-hole System Configuration

Pi-hole was running on Raspberry Pi OS based on Debian 12.

The following system details were identified during troubleshooting:

* Hostname: PiHole
* Network interface: eth0
* Network manager: NetworkManager
* Network management service dhcpcd: inactive
* Pi-hole FTL: running
* DNS listening: IPv4 and IPv6
* DNS ports: UDP 53 and TCP 53
* Blocking: enabled

NetworkManager was active, so network interface configuration should be managed consistently rather than mixing NetworkManager settings with an independent DHCP client configuration.

## Troubleshooting and Lessons Learned

### 1. Moving Pi-hole to a different subnet

Pi-hole originally used 192.168.7xx.xxx before being moved to the 192.168.8xx.xxx network.

After the change, the Pi-hole address was 192.168.8xx.xxx.

This highlighted the importance of updating DNS settings and checking gateway reachability when changing subnets.

### 2. Router DNS configuration

The MikroTik was configured to advertise Pi-hole as the DNS server through DHCP.

However, the Acer W6 and MikroTik configuration required further troubleshooting after the W6 was reset.

This demonstrated that a router can have internet access itself while downstream clients experience DNS or DHCP problems.

### 3. Diagnosing connectivity

Useful checks include:

* Confirming the Pi-hole has the expected IP address.
* Verifying the default gateway.
* Checking whether Pi-hole FTL is running.
* Confirming that DNS is listening on port 53.
* Testing IP connectivity separately from DNS resolution.
* Checking the DNS server advertised by DHCP.
* Reviewing MikroTik firewall, NAT, DHCP, and DNS settings.
* Confirming that the Acer W6 is operating in the intended network mode.

See Troubleshooting⁠(coming soon) for a more detailed checklist.

## Skills Demonstrated

This project provides hands-on experience with:

* Linux system administration
* Raspberry Pi OS and Debian
* DNS resolution and filtering
* DHCP configuration
* IP addressing and subnetting
* Router configuration
* Network troubleshooting
* Service management
* Infrastructure documentation

## Future Improvements

* Verify end-to-end DNS filtering for wired and wireless clients.
* Finalize the MikroTik and Acer W6 network configuration.
* Document upstream DNS and fallback behavior.
* Record firewall and NAT rules.
* Document Pi-hole configuration backups and recovery.
* Add screenshots of the Pi-hole dashboard.
* Add a network diagram showing the final, verified topology.

## Project Status

Status: Configured; network integration requires further validation.

Pi-hole was installed and running, with DNS listening and blocking enabled. Integration with the MikroTik and Acer W6 required further troubleshooting following a router reset.

This repository documents the implementation, the configuration investigated, and the troubleshooting process.

⸻

Part of my personal homelab project focused on Linux, networking, self-hosted services, and developing practical IT and DevOps skills.
