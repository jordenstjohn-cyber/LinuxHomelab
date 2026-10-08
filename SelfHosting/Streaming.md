# Self-Hosted Media Services

## Overview

This project explores self-hosting personal media services within my homelab using Linux and virtualization.

The goal is to reduce reliance on third-party platforms by running services on hardware I control, with personal media stored on my own infrastructure.

The project focuses on two applications:

* Jellyfin — A self-hosted media server for organizing and streaming movies, television shows, music, and other personal media.
* Immich — A self-hosted photo and video management platform designed for backing up, organizing, and browsing personal media libraries.

Both services are intended to run within my Proxmox-based homelab.

## Project goals

* Learn how to deploy and maintain self-hosted applications on Linux.
* Understand the relationship between virtual machines, applications, and persistent storage.
* Build a private media streaming environment.
* Create a personal photo and video backup system.
* Develop practical skills in networking, storage management, permissions, and troubleshooting.
* Document infrastructure decisions and configuration for future maintenance.

⸻

## 1. Services

### Jellyfin

Jellyfin is an open-source media server that allows me to manage and stream my own media library.

It provides a centralized interface for browsing media and playing content on compatible clients.

Planned features include:

* Centralized media management.
* Movie and television libraries.
* Music library organization.
* Streaming to compatible devices.
* User accounts and access controls.
* Optional hardware-accelerated transcoding, depending on hardware support.

### Immich

Immich is an open-source, self-hosted photo and video management platform.

It provides a private alternative to commercial photo cloud services, allowing personal media to be uploaded to infrastructure I control.

Planned features include:

* Photo and video uploads.
* Mobile device backups.
* Timeline browsing.
* Album organization.
* User accounts.
* Search and media management.

Important: Self-hosting Immich does not automatically guarantee data safety. Independent backups are necessary to protect against disk failure, accidental deletion, and other data-loss events.


## 2. Infrastructure

The applications are part of a broader homelab built around Proxmox VE.

The intended architecture separates virtualization, application services, and media storage.

                    Internet
                       |
                  Home Router
                       |
                  MikroTik hEX
                       |
                   Local LAN
                       |
                  Proxmox VE
                       |
              +--------+--------+
              |                 |
         Jellyfin VM       Immich VM
              |                 |
         Media Library     Photo Library
              |                 |
              +--------+--------+
                       |
                 Persistent Storage


This is the intended logical architecture. The exact VM assignments, storage mounts, and network configuration should be updated as the deployment is finalized.

### Infrastructure design principles

- Separation of services

Running applications in separate virtual machines can help isolate dependencies, simplify troubleshooting, and make maintenance more manageable.

- Persistent storage

Media files should be stored in locations that remain available when application containers or services restart.

- Network accessibility

Applications should be accessible to authorized devices on the local network without exposing unnecessary services to the public internet.

- Backup and recovery

Application configuration, databases, and media files should have appropriate backup strategies.


## 3. Jellyfin Deployment

### Purpose

Jellyfin provides a centralized streaming interface for personal media.

Rather than relying on a subscription-based media server platform, the objective is to manage and stream locally stored content using software running on my own infrastructure.

### Deployment plan

1. Prepare a Linux environment for Jellyfin.
2. Install Jellyfin using the official installation method.
3. Create and configure the media directories.
4. Ensure the Jellyfin service can read the media files.
5. Complete the initial web-based setup wizard.
6. Create the required media libraries.
7. Test playback from a client device.
8. Evaluate transcoding requirements and performance.
9. Document the final configuration.

### Access

The default local web interface is typically available at:

http://<JELLYFIN_SERVER_IP>:8096

Replace the placeholder with the actual address assigned to the Jellyfin server.

### Storage considerations

Media storage should be separate from the application’s essential configuration where practical.

An example directory structure is:

/srv/media/
├── movies/
├── shows/
└── music/

These are proposed paths, not confirmed paths from the existing deployment.

The Jellyfin service account must have appropriate permissions to read the media directories.

If media is stored on a separate disk or network share, the storage must be mounted and available before Jellyfin attempts to access it.

### Performance considerations

Streaming performance depends on several factors:

* CPU capabilities.
* Network bandwidth.
* Client device compatibility.
* Video codec compatibility.
* Direct Play versus transcoding.
* Storage performance.

Direct Play allows compatible clients to play media without server-side transcoding.

Transcoding converts media into a format or bitrate that a client can handle. Hardware acceleration may improve performance if the server’s hardware and configuration support it.

### Official documentation

* Installation: https://jellyfin.org/docs/general/installation/
* Setup wizard: https://jellyfin.org/docs/general/post-install/setup-wizard/
* Hardware acceleration: https://jellyfin.org/docs/general/post-install/transcoding/hardware-acceleration/

## 4. Immich Deployment

### Purpose

Immich provides a self-hosted environment for personal photos and videos.

The objective is to have a centralized library that can receive uploads from personal devices while keeping control of the underlying storage and server infrastructure.

### Deployment plan

1. Prepare a suitable Linux environment.
2. Install Docker Engine and the Docker Compose plugin.
3. Create an Immich application directory.
4. Download the official Docker Compose file and example environment file.
5. Configure media storage and database locations.
6. Review the environment variables before starting the application.
7. Start the Immich services.
8. Complete the initial administrator registration.
9. Connect the mobile application.
10. Enable photo and video backups.
11. Verify that uploaded media is stored persistently.
12. Establish an independent backup procedure.

### Docker Compose

Immich’s official documentation recommends Docker Compose for deployment.

The general workflow is:

```Bash
mkdir -p ~/immich-app
cd ~/immich-app
```

Download the official Compose configuration:

```Bash
wget -O docker-compose.yml \
  https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml
```

Download the example environment file:

```Bash
wget -O .env \
  https://github.com/immich-app/immich/releases/latest/download/example.env
```

Edit the environment file:

```Bash
nano .env
```

Review the media upload location, database location, timezone, and other settings before starting the application.

Start the services:

```Bash
docker compose up -d
```

These commands describe the standard deployment workflow. They should be run on the intended Immich server, not automatically on the Proxmox host.

### Access

The default local web interface is typically available at:

http://<IMMICH_SERVER_IP>:2283

Replace the placeholder with the actual address of the server.

The first registered user becomes the administrator.

After completing the initial setup, the mobile application can connect to the server and back up selected photos and videos.

### Storage considerations

Immich uses persistent storage for uploaded media and application data.

A typical deployment separates the following:

| Data | Purpose |
|---|---|
| Upload directory | Stores uploaded photos and videos |
| PostgreSQL database | Stores metadata and application records |
| Application configuration | Stores deployment settings |
| Backup destination | Protects data against loss |

The PostgreSQL database should use suitable local storage rather than a network share, as recommended by Immich’s documentation.

The uploaded media and database should both be included in an appropriate backup plan.

A database backup alone is not a complete backup of an Immich library. The actual photos and videos must also be protected.

### Official documentation

* Installation: https://docs.immich.app/install/docker-compose/
* Requirements: https://docs.immich.app/install/requirements/
* Post-installation: https://docs.immich.app/install/post-install/

⸻

## 5. Networking

Both applications are intended to be accessible from devices on the local network.

The initial approach is to use local IP addresses and the applications’ default web ports.

| Application | Default Web Port |
|---|---|
| Jellyfin | 8096 |
| Immich | 2283 |

These ports identify the default web interfaces. Additional ports or protocols may be required for other functionality.

### Networking objectives

* Assign predictable IP addresses to the application servers.
* Verify local network connectivity.
* Ensure the required application ports are reachable.
* Use Pi-hole for DNS filtering where appropriate.
* Consider internal DNS names for easier access.
* Avoid exposing application administration interfaces directly to the public internet.

Remote access, if required, should be designed separately with appropriate authentication and security controls.

⸻

## 6. Storage and Backup Strategy

Storage reliability is an important part of a self-hosted media environment.

The objective is to keep media separate from ephemeral application state and ensure that files remain available after a restart or application update.

### Storage principles

* Use persistent storage for all important application data.
* Mount media storage consistently.
* Verify filesystem permissions.
* Monitor available disk capacity.
* Avoid storing the only copy of important media on the application VM.
* Maintain backups on a separate physical device or other independent destination.

### Backup priorities

Jellyfin

Protect important configuration and metadata. The original media files should also be backed up if they cannot be replaced.

Immich

Protect the uploaded media, PostgreSQL database, and necessary configuration.

Proxmox

Document the VM configuration and establish an appropriate VM backup strategy.

A Proxmox VM backup is useful, but it should not automatically be considered a substitute for a tested, independent backup of irreplaceable media.

⸻

## 7. Troubleshooting

Jellyfin cannot find media files

Check that the expected storage path exists:

```Bash
ls -lah /srv/media
```

Check the permissions of the relevant directories:

```Bash
ls -ld /srv/media
```

Verify that the media filesystem is mounted and that the Jellyfin service account has read access.

The path above is an example and should be replaced with the actual media directory.

### Jellyfin is slow during playback

Investigate:

* Whether the client can use Direct Play.
* Whether the server is transcoding.
* CPU utilization during playback.
* Network throughput.
* Storage performance.
* Hardware acceleration configuration.

### Immich is inaccessible

Check the running containers:

```Bash
docker compose ps
```

Review the application logs:

```Bash
docker compose logs --tail=100
```

Run these commands from the directory containing the Immich Compose file.

Check that the server is reachable and that the required web port is available.

### Immich uploads are failing

Check the following:

* Available disk space.
* The configured upload location.
* Directory permissions.
* Container health.
* Application logs.
* Network connectivity between the client and server.

### Storage is unavailable after a reboot

Inspect mounted filesystems:

```Bash
lsblk -f
```

Inspect active mounts:

```Bash
findmnt
```

Confirm that the required media storage is mounted before starting or troubleshooting the application.

⸻

## 8. Security

Self-hosting provides greater control over infrastructure but also introduces responsibility for securing and maintaining the services.

### Security practices for this project include:

* Keeping operating systems and applications updated.
* Using strong, unique passwords.
* Restricting administrative access.
* Avoiding unnecessary public port forwarding.
* Limiting filesystem permissions to what services require.
* Maintaining backups before major changes.
* Reviewing application logs when troubleshooting.
* Keeping database and application configuration files out of public GitHub repositories when they contain secrets.

Never commit passwords, API keys, authentication tokens, private environment files, or other credentials to a public repository.

For Immich, keep the .env file containing deployment-specific configuration private if it contains secrets.

⸻

## 9. Skills Developed

This project provides practical experience with:

### Linux administration

* Managing Linux services.
* Inspecting filesystems and mount points.
* Understanding filesystem permissions.
* Troubleshooting application logs.

### Virtualization

* Running services in a Proxmox environment.
* Separating workloads into virtual machines.
* Understanding resource allocation and service dependencies.

### Containers

* Deploying applications using Docker Compose.
* Managing container lifecycle.
* Inspecting service health and logs.
* Persisting application data outside disposable containers.

### Networking

* Understanding local IP addressing.
* Accessing self-hosted applications over a LAN.
* Troubleshooting ports and connectivity.
* Integrating services with a local DNS environment.

### Storage and reliability

* Separating application state from media storage.
* Understanding database and media backup requirements.
* Planning for disk failures and service recovery.

⸻

## 10. Future Improvements

### Potential future enhancements include:

* Finalize the Jellyfin and Immich VM configurations.
* Document the actual IP addresses and storage mounts.
* Configure reliable storage mounting after reboot.
* Add internal DNS records for both services.
* Evaluate hardware-accelerated video transcoding.
* Implement automated backups.
* Test restoring application configuration and media.
* Monitor disk usage and service availability.
* Document secure remote access if required.

⸻

## References

* Jellyfin: https://jellyfin.org/
* Jellyfin Documentation: https://jellyfin.org/docs/
* Immich: https://immich.app/
* Immich Documentation: https://docs.immich.app/
* Proxmox VE: https://www.proxmox.com/en/proxmox-virtual-environment/overview
* Docker Documentation: https://docs.docker.com/
