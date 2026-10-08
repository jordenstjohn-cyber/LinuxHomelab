# LinuxHomelab

A personal homelab built to develop practical skills in Linux administration, networking, virtualization, DNS/DHCP, and infrastructure troubleshooting.  

This project started as a way to gain more control over my personal technology. I am documenting the lab as I build it, including both successful configurations and problems encountered.


## Project Goals

* Develop practical Linux administration skills
* Learn networking through hands-on configuration
* Understand routing, NAT, DHCP, and DNS
* Build and manage a Proxmox virtualization environment
* Deploy self-hosted media streaming
* Deploy self-hosted eCommerce website
* Practice infrastructure troubleshooting
* Learn storage and hardware architecture
* Document infrastructure using Git and GitHub
* Progress toward automation, containers, and DevOps


## Architecture

                         Internet
                            │
                            │
                    ┌───────▼────────┐
                    │    MikroTik    │
                    │   hEX RB750Gr3 │
                    │                │
                    │ Routing / DHCP │
                    │      / NAT     │
                    └───────┬────────┘
                            │
              ┌─────────────┼─────────────┐
              │             │             │
              ▼             ▼             ▼
         ┌───────────┐   ┌────────────┐  ┌──────────┐
         │RaspberryPi│   │G5 Elitedesk│  │ Acer W6  │
         │   PiHole  │   │  Proxmox   │  │ Wi-Fi    │
         │   DNS     │   │            │  │          │
         └───────────┘   └─┬──────────┘  └────┬─────┘
                           |                  │
            ┌──────────────┤              Wi-Fi Clients
            ▼              ▼
         ┌─────────┐   ┌─────────────────────┐  
         │ Media   │   │ Self-Hosted Website │  
         │ Jellyfin│   │   Debian Linux VM   │   
         │ Immich  │   │Wordpress/Woocommerce│  
         └─────────┘   └─────────────────────┘  




The architecture is still being refined. Some components have been deployed independently while the final routing, DHCP, and Wi-Fi architecture is being completed.


## Hardware

| Device | Role | Status |
|---|---|---|
| HP EliteDesk 800 G5 | Proxmox Host | Active |
| MikroTik hEX RB750Gr3 | Router | Active |
| Raspberry Pi | Pi-hole DNS | Active |
| Acer W6 | Wi-Fi | Active |


## Infrastructure Components

### Networking

* MikroTik RouterOS
* IPv4 addressing
* DHCP
* DNS
* NAT
* Routing
* Wi-Fi
* Network troubleshooting

### Services

* Pi-hole
* Network-wide DNS filtering

### Virtualization

* Proxmox VE
* Virtual machine infrastructure
* Storage planning

### Hardware

* HP EliteDesk
* Raspberry Pi
* ThinkPad T480s
* Storage expansion
* 10-inch rack
* DC power distribution


## Major Projects

### 1. MikroTik Network Deployment

Introduced a MikroTik hEX RB750Gr3 as the primary routing platform.

Learned and configured:

* RouterOS
* DHCP
* NAT
* LAN addressing
* WAN connectivity
* Routing

View documentation →⁠￼[Networking Documentation](Linuxhomelab/networking/readme.md)

⸻

### 2. Pi-hole DNS Server

Deployed Pi-hole on a dedicated Raspberry Pi.

Implemented:

* Static IP addressing
* Ethernet networking
* DNS filtering
* Client DNS configuration
* Linux network troubleshooting

View documentation →⁠￼[DNS Server Documentation](Linuxhomelab/Networking/pihole.md)

⸻

### 3. Proxmox Virtualization Host

Installed Proxmox VE on an HP EliteDesk Mini to create a dedicated virtualization platform.

View documentation →⁠￼(coming soon)

⸻

### 4. Wordpress Self-Hosted Website

Designed a compact rack infrastructure around a 10-inch form factor.

Implemented:

* Host WordPress on my own infrastructure and Debian OS
* Deploy WooCommerce for e-commerce functionality
* Practice Linux server administration
* Understand MySQL/MariaDB database configuration
* Practice managing a production-style application
* Keep infrastructure under my control

View documentation →⁠￼[Website Hosting](Linuxhomelab/selfhosting/wordpress.md)

⸻

### 5. Compact 10-Inch Rack

Designed a compact rack infrastructure around a 10-inch form factor.

Areas being investigated:

* Power distribution
* UPS
* DC voltage conversion
* Cable management
* Storage
* Device mounting

View documentation →⁠￼

⸻

## Troubleshooting

A major part of this project is documenting failures and the investigation process.

Examples include:

* Slow networking after introducing the MikroTik
* Multiple network routes on Linux
* Pi-hole connectivity problems
* DHCP architecture conflicts
* SSH connectivity issues
* Hardware expansion limitations

Troubleshooting documentation →⁠￼

⸻

## Skills Demonstrated

### Linux

* Linux administration
* SSH
* NetworkManager
* Static networking
* Routing tables
* System services
* Package management

### Networking

* IPv4
* DHCP
* DNS
* NAT
* Routing
* LAN/WAN
* Wireless networking
* Network troubleshooting

### Infrastructure

* Proxmox VE
* Hardware selection
* Storage architecture
* Power distribution
* Rack design

### Tools

* Git
* GitHub
* Linux CLI
* RouterOS CLI
* Proxmox

⸻

## Roadmap

* Build physical homelab
* Install Proxmox
* Deploy MikroTik
* Deploy Pi-hole
* Configure static Pi-hole networking
* Investigate Linux routing
* Begin 10-inch rack design
* Finalize router / AP architecture
* Finalize DHCP architecture
* Expand Proxmox storage
* Deploy additional services
* Introduce Docker
* Introduce Ansible
* Implement monitoring
* Begin Infrastructure as Code
* Build CI/CD projects
* Experiment with Kubernetes

⸻

## Why This Project Exists

This project began as a personal Linux and infrastructure learning environment.

Rather than learning infrastructure entirely through tutorials, I am using physical hardware to build, break, troubleshoot, document, and improve a functioning environment.

The long-term goal is to develop practical skills that can be transferred into infrastructure and DevOps roles.

⸻

Author

Jorden St. John

SaaS Sales → Linux / Infrastructure / DevOps

This repository documents the transition from customer-facing technology experience into hands-on infrastructure engineering.
