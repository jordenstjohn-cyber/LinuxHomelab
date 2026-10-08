# Self-Hosted WordPress + WooCommerce

A self-hosted WordPress and WooCommerce deployment running on my personal Proxmox homelab.

The goal of this project was to build and operate a real e-commerce website using infrastructure that I control rather than relying entirely on managed WordPress hosting.

This project combines Linux server administration, virtualization, web hosting, databases, DNS, PHP, WordPress, WooCommerce, and basic infrastructure troubleshooting.


## Project Goals

The primary goals were:

* Host WordPress on my own infrastructure
* Deploy WooCommerce for e-commerce functionality
* Learn how a modern PHP web application is structured
* Practice Linux server administration
* Learn how web servers interact with PHP and databases
* Understand MySQL/MariaDB database configuration
* Practice managing a production-style application
* Keep infrastructure under my control
* Integrate the project into my existing Proxmox homelab

## Architecture

The initial architecture is:
                         Internet
                            │
                            ▼
                         Router
                            │
                            ▼
                       Proxmox Host
                            │
                            ▼
                     Debian Linux VM
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
              Apache               MariaDB
                 │                     │
                 │                     │
                 ▼                     │
             PHP-FPM / PHP             │
                 │                     │
                 └──────────┬──────────┘
                            │
                            ▼
                        WordPress
                            │
                            ▼
                       WooCommerce

The website runs inside a dedicated Linux virtual machine rather than directly on the Proxmox host.

This provides separation between the virtualization platform and the application.

## Infrastructure

### Proxmox

The WordPress server runs as a virtual machine on my existing Proxmox infrastructure.

Using a VM provides:

* Isolation from the Proxmox host
* Independent operating system
* Easier backups
* Easier migration
* Ability to experiment without affecting other services
* A realistic server administration environment

### Operating System
The web server VM uses Debian Linux.

The decision to use Debian was based on its:

* Stability
* Long-term support
* Large package ecosystem
* Extensive documentation
* Common use in server environments

The VM was created with a minimal installation rather than installing a desktop environment.

## Initial Server Setup

After installing Debian, the server was configured through the command line.

Initial administration included:

* Creating a non-root user
* Installing required packages
* Updating the operating system
* Configuring the web server
* Installing PHP
* Installing MariaDB
* Installing WordPress
* Configuring file permissions

The project was intentionally completed primarily through the Linux command line to gain experience with server administration.

## Web Stack

The deployment uses a traditional Linux web stack.

Linux
  │
  ├── Apache
  │
  ├── PHP
  │
  └── MariaDB
        │
        └── WordPress
              │
              └── WooCommerce

Each component has a specific responsibility.

### Linux

Provides the operating system and server environment.

### Apache

Handles incoming HTTP requests and serves the website.

### PHP

Runs the WordPress application.

### MariaDB

Stores WordPress data such as:

* Users
* Posts
* Pages
* WooCommerce products
* Orders
* Settings
* Plugin configuration

### WordPress

Provides the content management system.

### WooCommerce

Adds e-commerce functionality.

## Installing the Web Server

Apache was installed as the web server.

The service was enabled so that it would start automatically when the VM boots.

The general workflow was:

sudo apt update
sudo apt upgrade
sudo apt install apache2

Apache was then verified as a running system service.

## Installing PHP

WordPress requires PHP to execute the application.

PHP and the required extensions were installed through Debian’s package manager.

The installation required identifying the PHP extensions needed by WordPress and WooCommerce.

Examples include:

php
php-mysql
php-curl
php-gd
php-mbstring
php-xml
php-zip
php-intl

The exact package requirements may change depending on the Debian and PHP versions being used.

## Installing MariaDB

MariaDB was used as the database server.

The database server was installed with:

sudo apt install mariadb-server

The database service was then enabled and started.


## Database Configuration

A dedicated database was created for WordPress rather than allowing WordPress to use an administrative database account.

The basic database architecture is:

MariaDB
   │
   └── WordPress Database
          │
          ├── Users
          ├── Posts
          ├── Pages
          ├── Products
          ├── Orders
          └── Settings

A dedicated database user was created with permissions limited to the WordPress database.

This follows the principle of giving applications only the permissions they require.


## WordPress Deployment

WordPress was downloaded and extracted into the web server directory.

The application was deployed under:

/var/www/wordpress

The directory contains the WordPress application files.

The general structure is:

/var/www/wordpress/
│
├── wp-admin/
├── wp-content/
├── wp-includes/
├── wp-config.php
└── index.php


## File Ownership and Permissions

One of the first Linux administration challenges was understanding the relationship between:

* The Linux user
* The Apache web server
* File ownership
* File permissions

WordPress needs appropriate permissions to read and modify specific files and directories.

The web application directory was therefore configured with appropriate ownership and permissions for the web server.

This was an important practical lesson because a website can appear to be correctly installed while still failing because the web server cannot access its files.


## Apache Configuration

Apache was configured to serve the WordPress installation.

The website’s document root was configured to point to:

/var/www/wordpress

Apache configuration separates the website from the rest of the filesystem and determines how requests are handled.


## WordPress Configuration

WordPress requires database connection information.

The configuration connects WordPress to MariaDB using:

Database name
Database username
Database password
Database host

This information is stored in:

wp-config.php

Sensitive credentials are not stored in this GitHub repository.


## WooCommerce

After WordPress was operational, WooCommerce was installed as the e-commerce platform.

WooCommerce adds functionality including:

* Products
* Product categories
* Shopping cart
* Checkout
* Orders
* Customers
* Inventory
* Shipping
* Payment integrations

The resulting application stack is:

WordPress
    │
    └── WooCommerce
          │
          ├── Products
          ├── Cart
          ├── Checkout
          └── Orders


## Domain

The website uses a custom domain purchased separately from the server infrastructure.

The domain provider and hosting infrastructure are intentionally separated.

This means the domain can be moved to a different hosting provider in the future without rebuilding the domain identity.

⸻

## DNS

DNS connects the domain name to the public-facing infrastructure.

The conceptual flow is:

Customer
   │
   │ example.com
   ▼
DNS
   │
   ▼
Public IP
   │
   ▼
Router
   │
   ▼
Homelab
   │
   ▼
WordPress VM

The DNS configuration will ultimately point the website domain toward the public endpoint used to reach the server.


## Network Architecture

The WordPress VM exists behind the homelab’s network infrastructure.

The general architecture is:

                         Internet
                            │
                            ▼
                       Public DNS
                            │
                            ▼
                        Home WAN
                            │
                            ▼
                        MikroTik
                            │
                            ▼
                        Proxmox
                            │
                            ▼
                     WordPress VM
                            │
                   ┌────────┴────────┐
                   │                 │
                Apache            MariaDB
                   │                 │
                   └────────┬────────┘
                            │
                         WordPress
                            │
                       WooCommerce

This project therefore required understanding how application traffic moves through multiple infrastructure layers.


## Problems Encountered

Building the server manually resulted in several configuration problems.

These were useful because they provided practical Linux troubleshooting experience.

⸻

### WordPress Directory Issues

During installation, the WordPress archive was extracted into /var/www.

The expected directory was initially not present when commands were run.

Investigation showed that the WordPress directory had been created under:

/var/www/wordpress

This reinforced the importance of checking the filesystem before assuming a command succeeded.

Useful commands included:

ls
ls -la
ls -la /var/www

### File Ownership Problems

One issue involved attempting to assign ownership to a web-server account before the appropriate web server package/user existed.

This produced an error indicating that the expected web-server user was not available.

The problem was resolved by understanding that Linux service accounts are normally created when the corresponding service is installed.

This was an important lesson in understanding dependencies between Linux packages and system users.

⸻

### Database Configuration Problems

Another problem occurred while configuring the WordPress database.

WordPress reported that the expected database could not be found.

The issue required checking:

* Database name
* Database user
* Database permissions
* MariaDB status
* WordPress configuration

This demonstrated the importance of troubleshooting an application from the bottom of the stack upward.

Network
   ↓
Web Server
   ↓
PHP
   ↓
WordPress
   ↓
Database

## Troubleshooting Method

When something failed, I worked from observable symptoms rather than repeatedly reinstalling components.

The general troubleshooting process was:

1. Identify the error
        ↓
2. Determine which layer failed
        ↓
3. Check service status
        ↓
4. Check configuration
        ↓
5. Check filesystem
        ↓
6. Check permissions
        ↓
7. Test again
        ↓
8. Document the result

Useful Linux commands included:

systemctl status apache2
systemctl status mariadb

ls -la /var/www/wordpress

ps aux

ip addr
ip route

sudo journalctl -xe

## Security Considerations

Because this is intended to become an internet-accessible e-commerce website, security is an important part of the project.

Planned security measures include:

* HTTPS
* TLS certificates
* Secure SSH configuration
* Firewall configuration
* Regular operating system updates
* WordPress updates
* Plugin updates
* Database backups
* VM backups
* Least-privilege database users
* Strong administrator credentials
* Monitoring

Credentials and secrets will not be committed to GitHub.

⸻

## Backup Strategy

A self-hosted e-commerce website requires backups because the server is under my control.

Potential backup layers include:

WordPress
   │
   ├── Website files
   │
   ├── Database
   │
   └── Uploaded media
          │
          ▼
       Backups
          │
          ▼
   Separate storage

Future improvements will include automated backups and off-server backup storage.


## Availability

One of the challenges of self-hosting an e-commerce application from a homelab is availability.

If the physical server loses power or internet connectivity, the website becomes unavailable.

The homelab therefore needs to address:

* UPS protection
* Automatic server restart
* Internet availability
* Backup restoration
* Monitoring
* Disaster recovery

The existing homelab power project is intended to eventually provide UPS-backed infrastructure.

⸻

## Lessons Learned

This project provided hands-on experience with several infrastructure concepts.

### Linux

I learned how Linux services, users, permissions, packages, and filesystems interact.

### Web Servers

I learned how Apache receives requests and serves an application from the filesystem.

### PHP

I learned that WordPress is not simply a collection of static HTML files and requires a server-side runtime.

### Databases

I learned how an application depends on a separate database service and how credentials and permissions connect the two.

### Networking

I learned how DNS, routing, NAT, and port forwarding affect an internet-facing service.

### Virtualization

I learned how a web application can be isolated inside a virtual machine while still operating as part of a larger infrastructure environment.

### Troubleshooting

Most importantly, I learned to troubleshoot problems by identifying which layer of the stack is actually failing.

⸻

## Current Architecture

                         INTERNET
                            │
                            ▼
                           DNS
                            │
                            ▼
                         ROUTER
                            │
                            ▼
                       PROXMOX HOST
                            │
                            ▼
                    ┌───────────────┐
                    │ Debian VM     │
                    │               │
                    │ Apache        │
                    │ PHP           │
                    │ MariaDB       │
                    │ WordPress     │
                    │ WooCommerce   │
                    └───────────────┘

## Current Status

### Completed

* Created Proxmox VM
* Installed Debian
* Installed Apache
* Installed PHP
* Installed MariaDB
* Created WordPress database
* Downloaded WordPress
* Deployed WordPress under /var/www/wordpress
* Configured basic WordPress installation
* Installed WooCommerce
* Began configuring the website

### In Progress

* Finalize Apache configuration
* Configure domain DNS
* Configure HTTPS
* Configure firewall
* Configure secure external access
* Implement backups
* Implement monitoring
* Harden SSH
* Test disaster recovery

⸻

## Future Improvements

### Infrastructure

* Reverse proxy
* HTTPS automation
* Firewall
* Monitoring
* Centralized logging
* Automated backups
* UPS integration

### DevOps

* Git-based configuration
* Ansible
* Docker
* CI/CD
* Infrastructure as Code

### Reliability

* Off-site backups
* Recovery testing
* Monitoring and alerting
* Automatic service recovery
* Documented disaster recovery procedures

⸻

## Skills Demonstrated

### Linux

* Debian
* Package management
* Systemd
* SSH
* Filesystem management
* Users and permissions
* Service management
* Troubleshooting

### Web Infrastructure

* Apache
* PHP
* MariaDB
* WordPress
* WooCommerce

### Networking

* DNS
* TCP/IP
* NAT
* Port forwarding
* LAN/WAN architecture

### Virtualization

* Proxmox VE
* Virtual machines
* Server isolation

### Infrastructure

* Self-hosting
* Backup planning
* Availability planning
* Security considerations
* Infrastructure documentation

⸻

## Why I Built This

I wanted to understand what actually happens behind a website rather than treating hosting as a black box.

By self-hosting WordPress and WooCommerce, I am responsible for the entire application stack:

Hardware
   ↓
Power
   ↓
Network
   ↓
Virtualization
   ↓
Operating System
   ↓
Web Server
   ↓
PHP
   ↓
Database
   ↓
WordPress
   ↓
WooCommerce
   ↓
Website

This project is part of my broader homelab and is being used to develop practical Linux, networking, infrastructure, and DevOps skills.

