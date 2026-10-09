# LinuxHomelab

A personal homelab built to develop practical skills in Linux administration, networking, virtualization, DNS/DHCP, and infrastructure troubleshooting. Control of my media with the ability to share and self host. To run a full-stack self-hosted ecommerce platform built from the ground up with no prior experience.


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

View documentation →⁠￼[Networking Documentation](networking/README.md)

⸻

### 2. Pi-hole DNS Server

Deployed Pi-hole on a dedicated Raspberry Pi.

Implemented:

* Static IP addressing
* Ethernet networking
* DNS filtering
* Client DNS configuration
* Linux network troubleshooting

View documentation →⁠￼[DNS Server Documentation](networking/PiHole.md)

⸻

### 3. Proxmox Virtualization Host

Installed Proxmox VE on an HP EliteDesk Mini to create a dedicated virtualization platform.

Why not complete:
* Running but far from optimized
* Need to secure before giving access the internet
* Will complete write up when stable and locked down

View documentation →⁠￼(coming soon)

⸻

### 4. Wordpress Self-Hosted Website

Set up a full stack for a online store

Implemented:

* Host WordPress on my own infrastructure and Debian OS
* Deploy WooCommerce for e-commerce functionality
* Practice Linux server administration
* Understand MySQL/MariaDB database configuration
* Practice managing a production-style application
* Keep infrastructure under my control

View documentation →⁠￼[Website Hosting](SelfHosting/Wordpress.md)

⸻

### 4. Self-Hosted Media Streaming Service

Run a smooth streaming service that can handle multiple users

Implemented:

* Learn how to deploy and maintain self-hosted applications on Linux.
* Understand the relationship between virtual machines, applications, and persistent storage.
* Build a private media streaming environment.
* Create a personal photo and video backup system.

View documentation →⁠￼[Self-Hosted Streaming](SelfHosting/Streaming.md)

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
* Cooling

View documentation →⁠￼Hardware write up coming soon

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
