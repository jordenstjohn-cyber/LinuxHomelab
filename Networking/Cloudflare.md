# WordPress High Availability, Cloudflare & Disaster Recovery

### Overview

This project explores how to improve the reliability, availability, and scalability of a self-hosted WordPress and WooCommerce website.

The primary website runs on a Debian virtual machine hosted on Proxmox VE within my homelab. While self-hosting provides control over the infrastructure, it introduces potential points of failure, including power outages, hardware failures, network interruptions, and unexpected traffic spikes.

To address these risks, this project develops a disaster recovery and high-availability architecture using Cloudflare, an independent cloud-hosted server, automated backups, and infrastructure monitoring.

Cloudflare provides the external traffic-management layer, while the primary and secondary servers provide the application infrastructure.

The long-term goal is to build a system that can recover from server failures, reduce downtime, and accommodate increases in website traffic.

### Objectives

* Protect WordPress and WooCommerce data through automated backups.
* Maintain independent local and offsite backup copies.
* Use Cloudflare for DNS, CDN caching, and traffic management.
* Configure health checks to detect unavailable application servers.
* Establish a secondary server outside the home network.
* Implement primary-to-secondary failover.
* Reduce the impact of power outages and hardware failures.
* Investigate performance monitoring and traffic-spike management.
* Document recovery procedures and test them regularly.

## 1. Current Infrastructure
   
                    Internet
                        |
                        v
                  Home Router
                        |
                        v
                   Proxmox VE
                        |
                        v
                  Debian Linux VM
                        |
                        v
                      Nginx
                        |
                        v
                   PHP / MariaDB
                        |
                        v
                WordPress / WooCommerce

The current deployment depends on a single primary host. If the host loses power, the virtual machine crashes, or the home internet connection fails, the website may become unavailable.

This project introduces additional infrastructure to reduce these dependencies.

## 2. Proposed High-Availability Architecture

The target architecture uses Cloudflare to manage incoming website traffic and an independent VPS as a secondary hosting environment.

The existing homelab includes a Proxmox VE host running a Debian virtual machine that hosts WordPress and WooCommerce.
                     
                       Customers
                           |
                           v
                       Cloudflare
                    DNS / CDN / WAF
                           |
                           v
                  Cloudflare Load Balancer
                           |
                   Health Check Monitoring
                           |
                 +---------+---------+
                 |                   |
                 v                   v
          PRIMARY ORIGIN       SECONDARY ORIGIN
          Home Proxmox VM      Offsite Cloud VPS
                 |                   |
                 v                   v
          WordPress / PHP      WordPress / PHP
                 |                   |
                 +---------+---------+
                           |
                           v
                  Database / Media
                 Synchronization Plan
                           |
                           v
                  Backup Infrastructure
                 Local + Encrypted Offsite

This diagram represents the target architecture. The secondary origin, load balancer, and synchronization mechanisms must be configured and tested before the system can provide reliable failover.

### Primary Origin

The primary origin is the existing WordPress installation running on the Proxmox host.

Responsibilities include:

* Serving website requests.
* Running WordPress and WooCommerce.
* Processing application and database operations.
* Storing current website content and uploaded files.
* Generating backups.
* Reporting application health.

### Secondary Origin

The secondary origin is a cloud-hosted VPS in a separate infrastructure environment.

Responsibilities include:

* Hosting a compatible WordPress environment.
* Maintaining the required application files and configuration.
* Receiving backups or synchronized data.
* Remaining available when the home server is offline.
* Supporting disaster recovery and planned maintenance.

The secondary server must have a sufficiently recent copy of the database, uploaded media, plugins, themes, and configuration.

A VPS running WordPress with an outdated database is not a complete backup of a live WooCommerce store.

### Cloudflare

Cloudflare provides the external traffic-management layer.

Depending on the configuration, it can provide:

* DNS management.
* CDN caching for eligible content.
* Web application firewall capabilities.
* Load balancing between configured origins.
* Health monitoring and origin failover.
* Traffic distribution across multiple healthy origins.

Cloudflare is not a replacement for the servers, databases, backups, or application synchronization. It manages requests between visitors and the infrastructure that serves the website.

## 3. Cloudflare Configuration

Cloudflare will sit between website visitors and the origin servers.

DNS and CDN

The website’s domain will use Cloudflare-managed DNS.

Cloudflare’s proxy and CDN can help reduce the amount of work performed by the origin server by serving eligible cached content from its edge network.

Potential benefits include:

* Faster delivery of static assets.
* Reduced origin bandwidth usage.
* Lower server load for eligible cached pages.
* Additional protection and traffic-management options.

Static assets such as images, CSS, and JavaScript are suitable starting points for caching.

WordPress HTML pages are not automatically cached in every situation. Cache rules must be designed carefully, especially for WooCommerce.

Shopping carts, checkout pages, account pages, logged-in sessions, and other personalized responses must not be cached indiscriminately.

### Cloudflare Load Balancing

Cloudflare Load Balancing will monitor the health of the configured origins and direct requests according to the selected traffic-steering policy.

The intended configuration is an active-passive arrangement.

| Component | Role |
|-|-|
| Primary pool | Contains the home-hosted WordPress server |
| Secondary pool | Contains the offsite VPS |
| Health monitor | Checks whether the origin responds as expected |
| Traffic steering | Prefers the primary pool and uses the secondary when the primary is unhealthy |
| Fallback behavior | Defines where traffic goes if the normal pools become unhealthy |

With an active-passive configuration, traffic normally goes to the primary pool. When Cloudflare determines that the primary is unhealthy, traffic can be directed to the secondary pool.

When the primary becomes healthy again, traffic can return to it according to the configured failback behavior.

Health checks must be configured with appropriate timeouts, expected responses, and failure thresholds.

A successful health check should indicate that the application can serve requests, not merely that the machine is powered on.

### Important: Detecting Overload

A health check is not necessarily an overload detector.

A server may be overwhelmed by CPU usage, memory pressure, database contention, or too many concurrent requests while still responding successfully to a simple HTTP check.

To improve overload handling, this project will investigate:

* Application response-time monitoring.
* CPU and memory monitoring.
* Database performance metrics.
* Request queue and concurrency limits.
* An application health endpoint.
* Carefully configured health checks.
* Caching and request-rate controls.
* Load testing to establish realistic thresholds.

Cloudflare can route traffic away from an unhealthy origin, but detecting overload reliably requires suitable monitoring and health-check design.

If the objective is to distribute normal traffic across multiple servers, an active-active architecture may be more appropriate than failover alone.

## 4. Automated Backup Strategy

Backups protect against data loss and provide the foundation for disaster recovery.

Cloudflare Load Balancing cannot recover deleted files or restore a corrupted database. Independent backups are therefore essential.

Data to Protect

| Component | Data to Protect |
|-|-|
| WordPress | Application customizations, plugins, themes, and uploads |
| WordPress configuration | wp-config.php and relevant server configuration |
| MariaDB | Posts, pages, products, users, orders, and store settings |
| WooCommerce | Product information, order records, and related application data |
| Proxmox | VM configuration and recovery information |
| Infrastructure | Network documentation, deployment notes, and recovery procedures |

### Backup Architecture 

              Primary WordPress Server
                         |
                         v
                  Automated Backups
                         |
                 +-------+-------+
                 |               |
                 v               v
          Separate Local     Offsite Storage
             Storage          Encrypted Copy
                 |               |
                 +-------+-------+
                         |
                         v
                 Restore Testing


Recommended measures include:

* Automated database backups.
* Backups of uploaded media and configuration.
* Separate physical storage for local backup copies.
* Encrypted offsite backup storage.
* Multiple historical recovery points.
* Monitoring for backup failures.
* Regular restoration testing.

A backup stored only on the Proxmox host is not sufficient protection against the failure of that host.

## Suggested Schedule

| Task | Initial Schedule |
|-|-|
| Database backup | Daily |
| WordPress files backup | Daily |
| Offsite backup | Daily |
| Backup verification | After each backup |
| Restore testing | Monthly |
| Recovery procedure review | After significant changes |

As the store begins processing real orders, backup frequency should be reviewed against the amount of order and inventory data that could be lost.

## 5. Power Protection

A power outage can interrupt the Proxmox host, router, network equipment, and WordPress server.

A UPS can provide temporary battery power and allow systems to shut down cleanly.

                  Utility Power
                        |
                        v
                       UPS
                        |
                +-------+-------+
                |               |
                v               v
             Proxmox          Router
                |
                v
           Debian VM
                |
                v
         WordPress / MariaDB

### Planned Improvements

* Evaluate UPS capacity for the server and essential network equipment.
* Investigate Network UPS Tools (NUT).
* Configure monitoring for power and battery status.
* Implement graceful shutdown procedures.
* Test shutdown and recovery behavior.

A UPS reduces the risk of abrupt shutdowns but does not guarantee continuous website availability during a prolonged outage.

The offsite VPS remains independent of the home power supply and can continue serving traffic if the primary home server becomes unavailable, provided its application data is current.

## 6. WooCommerce Data Consistency

WooCommerce introduces additional complexity because the database changes as customers interact with the store.

Examples include:

* Orders being placed.
* Inventory levels changing.
* Customer information being updated.
* Payment and order statuses changing.
* Products being created or modified.

A secondary server must not simply be treated as an independent copy of the primary.

### Initial Recovery Model

The first implementation will use a controlled recovery process:

1. Back up the primary WordPress database and files.
2. Transfer protected copies to the secondary environment.
3. Verify that the secondary environment can run the website.
4. Test restoration and application behavior.
5. Document how the secondary server would be promoted.
6. Prevent the original server from accepting conflicting writes during recovery.
7. Define a safe process for returning to the primary environment.

### Future Synchronization

More advanced implementations may investigate database replication, shared storage, or another controlled synchronization mechanism.

These approaches require careful design.

Database replication alone does not automatically synchronize WordPress uploads, plugin files, configuration, or other application state.

Before a secondary server accepts real customer orders, the system must have a reliable method of maintaining a single consistent production data set.

The initial objective is a tested disaster recovery process rather than an unsafe automatic promotion of an outdated database.

## 7. Handling Traffic Spikes

Traffic spikes can overwhelm the web server, PHP workers, database, or network connection.

Failover may help when the primary server is genuinely unhealthy, but it does not automatically solve every capacity problem.

### First Line of Defense: Caching

Cloudflare’s CDN can reduce origin load by caching eligible content.

Additional measures include:

* WordPress page caching for suitable anonymous visitors.
* Browser caching for static assets.
* Image optimization.
* Object caching where appropriate.
* PHP and database performance tuning.
* Rate limiting where appropriate.

WooCommerce sessions, checkout, account pages, and personalized content require specific caching exclusions.

### Second Line of Defense: Additional Application Capacity

If a single server cannot handle the workload, a second application server can be introduced.

                     Customers
                         |
                         v
                     Cloudflare
                         |
                         v
                    Load Balancer
                         |
                 +-------+-------+
                 |               |
                 v               v
          WordPress Server 1  WordPress Server 2
                 |               |
                 +-------+-------+
                         |
                         v
                    MariaDB


In this architecture, both application servers serve traffic.

They must use compatible application files and configuration, and they must access a database and media storage arrangement designed for concurrent use.

The database can remain a bottleneck even if more application servers are added.

### Future Autoscaling

Automatic scaling could eventually be investigated using cloud infrastructure that can create additional application instances when demand increases.

This is more complex than basic Cloudflare failover.

It requires a deployment process, a suitable application image or configuration management system, shared or synchronized data, health checks, and infrastructure capacity.

For this project, caching and monitoring will be implemented before pursuing autoscaling.

## 8. Monitoring and Alerts

Monitoring helps identify failures before they result in prolonged downtime.

The project will investigate monitoring for:

* Website availability.
* HTTP response time.
* CPU and memory usage.
* Disk utilization.
* Database health.
* Backup completion.
* UPS status.
* Cloudflare origin health.
* Secondary-server availability.

Cloudflare health alerts can report changes in origin or pool health. Additional monitoring on the Linux hosts is needed to identify resource exhaustion and other performance problems.

Alerts should provide enough information to identify the affected component and begin recovery.

## 9. Recovery Objectives

Two metrics will be used to evaluate reliability.

Recovery Time Objective (RTO): The target maximum time required to restore service.

Recovery Point Objective (RPO): The maximum acceptable amount of recent data that could be lost.

Initial development targets:

| Metric | Illustrative Target |
|-|-|
| Recovery Time Objective | Within 4 hours |
| Recovery Point Objective | Within 24 hours |
| Database backup | Daily |
| Offsite backup | Daily |
| Restore testing | Monthly |

These are proposed targets, not measured or guaranteed results.

Cloudflare failover may redirect traffic more quickly than a manual recovery process, but it cannot guarantee the RTO if the secondary server is unavailable, misconfigured, or missing current data.

A production WooCommerce store may require a much shorter RPO to protect recent orders and inventory changes.

## 10. Implementation Roadmap

### Phase 1: Backups

* Document the existing WordPress installation.
* Automate database and file backups.
* Store backups independently of the primary host.
* Configure encrypted offsite storage.
* Test restoration on a separate environment.

### Phase 2: Power Protection

* Evaluate UPS requirements.
* Investigate NUT monitoring.
* Configure graceful shutdown.
* Test recovery after power returns.

### Phase 3: Cloudflare CDN and DNS

* Configure the website domain in Cloudflare.
* Review DNS and proxy settings.
* Enable appropriate caching for static content.
* Configure WooCommerce cache exclusions.
* Verify HTTPS and origin connectivity.

### Phase 4: Secondary Server

* Provision an offsite VPS.
* Install a compatible web-server and PHP environment.
* Prepare WordPress and its required configuration.
* Restore a copy of the database and files.
* Test the site independently of the primary server.

### Phase 5: Load Balancing and Failover

* Configure Cloudflare Load Balancing.
* Create primary and secondary origin pools.
* Configure health monitors.
* Implement active-passive traffic steering.
* Verify failover and failback behavior.
* Confirm that the secondary server has sufficiently current data.

### Phase 6: Monitoring and Performance

* Add host and application monitoring.
* Record baseline resource utilization.
* Investigate performance bottlenecks.
* Tune caching and database performance.
* Conduct controlled load tests.
* Evaluate whether active-active load balancing is justified.

### Phase 7: Advanced Scaling

* Investigate automated application deployment.
* Explore infrastructure automation.
* Evaluate database replication and storage synchronization.
* Consider cloud autoscaling where appropriate.
* Document and test failure scenarios.

## 11. Testing and Validation

The project will use controlled tests to verify that the architecture works as intended.

| Test | Expected Outcome |
|-|-|
| Restore database backup | Database can be imported and validated |
| Restore WordPress files | Required application files are available |
| Restore to a separate server | Website can run independently of the primary host |
| Stop the primary web service | Monitoring detects the application failure |
| Simulate a primary-origin outage | Cloudflare directs traffic according to the failover configuration |
| Restore the primary origin | Traffic returns according to the configured failback behavior |
| Test an overloaded application | Monitoring identifies degraded performance
| Test caching | Eligible content is served without unnecessary origin requests |
| Perform a load test | Resource bottlenecks and capacity limits are documented |
| Restore the primary after failover | Newer production data is preserved |

Testing should begin in a development environment. Failover tests must not allow the primary and secondary WooCommerce databases to accept conflicting production writes.

## 12. Skills Demonstrated

This project develops practical experience with:

* Linux server administration.
* Proxmox virtualization.
* WordPress and WooCommerce hosting.
* MariaDB backup and recovery.
* Cloudflare DNS and CDN configuration.
* Load balancing and health checks.
* Disaster recovery planning.
* Infrastructure monitoring.
* Network reliability.
* Web performance analysis.
* High-availability architecture.
* Backup automation.
* Recovery testing and technical documentation.

## Conclusion

This project extends a self-hosted WordPress deployment into a more resilient hosting architecture.

Cloudflare provides the external traffic-management layer, while independent backup storage and a secondary VPS reduce dependence on the primary homelab server.

The implementation will begin with reliable backups and restoration, followed by power protection, Cloudflare caching, a secondary hosting environment, and tested failover.

More advanced traffic distribution and autoscaling will be explored only after application data consistency and recovery procedures are reliable.

The objective is to develop a documented infrastructure environment in which failures can be detected, recovery can be tested, and reliability improvements can be measured.
