# Homelab Networking & Cybersecurity Expansion

## Project Overview

This project outlines the planned expansion of my existing homelab into a more segmented, observable, and security-focused network environment.

My current environment includes a MikroTik hEX RB750Gr3 router, a Raspberry Pi running Pi-hole for DNS filtering, and a Proxmox server hosting virtualized Linux services. I am using this environment to develop practical skills in networking, Linux system administration, infrastructure management, and self-hosted services.

The next phase will focus on network segmentation, intrusion detection, centralized logging, and security monitoring. These upgrades will provide opportunities to work with technologies commonly used in enterprise networking, infrastructure operations, and cybersecurity.

The goal is to build a reliable learning environment where I can implement configurations, observe network behavior, investigate security events, troubleshoot failures, and document the results.

## Project Objectives

* Implement network segmentation using VLANs.
* Separate infrastructure management interfaces from user devices and hosted services.
* Improve DNS visibility and network monitoring.
* Introduce network intrusion detection using Suricata.
* Centralize system logs and performance metrics.
* Deploy security information and event management (SIEM) capabilities using Wazuh.
* Develop practical incident investigation and troubleshooting skills.
* Document network architecture, configuration decisions, and test results in GitHub.

⸻

## 1. Network Segmentation with VLANs

### Objective

Implement logical network separation using IEEE 802.1Q VLANs and a managed Ethernet switch.

As the number of devices and services in my homelab increases, placing every device on the same network becomes less desirable. Network segmentation will allow me to separate infrastructure management, hosted services, trusted computers, IoT devices, and guest devices.

### Planned Architecture

| VLAN | Network | Purpose |
|-|-|-|
| VLAN 10 | Management | Proxmox, switch management, and infrastructure administration |
| VLAN 20 | Servers | Debian, WordPress, and other hosted services |
| VLAN 30 | Trusted Devices | Personal computers and trusted clients |
| VLAN 40 | IoT | Smart-home devices and other less-trusted equipment |
| VLAN 50 | Guest | Isolated guest access |

hese VLAN IDs are proposed design choices and may change during implementation.

### Hardware

* Existing MikroTik hEX RB750Gr3 router.
* A managed Ethernet switch supporting IEEE 802.1Q VLANs.
* Existing Proxmox server and connected devices.

### Implementation Plan

1. Review the existing network topology and IP addressing.
2. Confirm VLAN capabilities and configuration requirements on the MikroTik.
3. Select a managed switch that supports VLAN tagging, access ports, and trunk ports.
4. Create VLAN interfaces and configure the required switch ports.
5. Establish appropriate IP addressing and DHCP services for each network.
6. Implement firewall rules controlling communication between VLANs.
7. Verify connectivity, routing, DNS resolution, and isolation.
8. Document the final configuration and troubleshooting process.

### Security Considerations

Network segmentation provides a foundation for limiting unnecessary communication between devices.

For example, IoT devices should not have unrestricted access to the Proxmox management interface. Guest devices should not be able to access internal server administration interfaces.

Inter-VLAN communication will be explicitly controlled through firewall rules, allowing only the traffic required for each network’s intended purpose.

### Skills Developed

* VLAN configuration and IEEE 802.1Q tagging.
* Access ports and trunk ports.
* Layer 2 switching and Layer 3 routing.
* DHCP and IP addressing.
* Firewall policy design.
* Network isolation and troubleshooting.

⸻

## 2. Network Intrusion Detection with Suricata

### Objective

Introduce network intrusion detection to identify suspicious traffic patterns and investigate potential security events.

Suricata is an open-source network threat detection engine that supports intrusion detection, intrusion prevention, and network security monitoring.

I plan to begin with passive monitoring before considering inline prevention.

### Planned Implementation

Deploy Suricata on a suitable virtual machine or compatible firewall platform.

Where practical, a managed switch mirror port will provide the sensor with copies of network traffic for analysis.

The initial configuration will prioritize detection rather than automatic blocking.

### Implementation Plan

1. Deploy a suitable Linux VM for network monitoring.
2. Install Suricata and configure its network interface.
3. Configure appropriate detection rules and signature updates.
4. Provide traffic visibility through a supported monitoring interface or switch mirror port.
5. Review generated alerts and investigate their causes.
6. Tune detection rules to reduce false positives.
7. Document representative alerts, findings, and troubleshooting procedures.

### Expected Outcomes

This project will provide hands-on experience identifying suspicious network behavior and interpreting intrusion detection alerts.

It will also help demonstrate the limitations of passive monitoring: the sensor can only inspect traffic that it can actually observe.

Encrypted application traffic may limit visibility into payload contents, although network metadata and other observable characteristics can still provide useful information.

### Skills Developed

* Packet inspection and network traffic analysis.
* Intrusion detection systems.
* Signature-based detection.
* Alert investigation and false-positive analysis.
* Network monitoring architecture.
* Security event documentation.

⸻

## 3. DNS Filtering and Monitoring with Pi-hole

### Objective

Expand the existing Pi-hole deployment from a DNS filtering service into a documented network visibility and troubleshooting component.

Pi-hole currently provides a foundation for understanding DNS requests, filtering unwanted domains, and observing client activity.

Rather than immediately purchasing additional DNS hardware, I intend to improve the existing deployment and integrate it into the broader network design.

### Planned Improvements

* Maintain clear device names and DHCP reservations.
* Review DNS query patterns and identify unexpected requests.
* Document upstream DNS resolver configuration.
* Restrict access to the DNS service to appropriate internal networks.
* Investigate devices attempting to use unauthorized DNS resolvers.
* Integrate available DNS and system logs into centralized monitoring.

### Security Considerations

DNS filtering can reduce access to known unwanted or malicious domains, depending on the configured lists and policies.

However, DNS filtering is not a complete security solution. It does not replace endpoint protection, firewall rules, or network intrusion detection.

As VLANs are introduced, DNS access will need to be configured carefully so that approved clients can resolve domains without unnecessarily exposing the DNS service to untrusted networks.

### Skills Developed

* DNS resolution and troubleshooting.
* DHCP reservations and device identification.
* DNS filtering and upstream resolver configuration.
* Network policy enforcement.
* Log analysis and investigation of unexpected DNS activity.

⸻

## 4. Centralized Logging and Monitoring

### Objective

Create a centralized monitoring environment that provides visibility into the health and activity of multiple systems.

As the homelab grows, investigating each server independently becomes less efficient. Centralized logging will help consolidate information and make it easier to identify service failures, authentication problems, and resource constraints.

### Planned Architecture

                  Homelab Systems
                         |
          +--------------+--------------+
          |              |              |
       Proxmox         Debian         Pi-hole
          |              |              |
          +--------------+--------------+
                         |
                Log and Metrics
                   Collection
                         |
             +-----------+-----------+
             |                       |
           Loki                 Prometheus
          (Logs)                (Metrics)
             |                       |
             +-----------+-----------+
                         |
                      Grafana
                    Dashboards


This diagram represents a proposed architecture. Actual integrations will depend on the supported exporters, log collectors, and configuration of each system.

### Technology Stack

* Grafana: Visualization, dashboards, and monitoring interfaces.
* Prometheus: Metrics collection and time-series monitoring.
* Loki: Log aggregation and querying.
* System-specific exporters and log collectors: Collection of information from supported systems.

### Implementation Plan

1. Deploy the required monitoring services on Proxmox.
2. Configure metrics collection from selected Linux systems.
3. Configure log forwarding from supported services.
4. Build dashboards for resource utilization and service health.
5. Establish alerts for relevant failures and abnormal conditions.
6. Investigate authentication failures and recurring service errors.
7. Document the monitoring architecture and operational procedures.

### Example Monitoring Use Cases

* Identifying memory or CPU exhaustion.
* Monitoring disk capacity.
* Detecting repeated SSH authentication failures.
* Investigating WordPress or web-server errors.
* Tracking service availability.
* Identifying recurring network or DNS problems.

### Skills Developed

* Linux logging and system administration.
* Metrics collection and time-series monitoring.
* Log aggregation and querying.
* Dashboard creation.
* Alert configuration.
* Infrastructure troubleshooting.

⸻

## 5. Security Information and Event Management with Wazuh

### Objective

Deploy Wazuh to develop practical experience with security event collection, endpoint monitoring, and alert investigation.

Wazuh is an open-source security platform that supports endpoint security monitoring, log analysis, file integrity monitoring, and security alerting.

This project will build on the centralized monitoring environment while introducing workflows focused specifically on security events.

### Planned Implementation

Deploy the Wazuh components in a suitable environment and install agents on supported systems.

Initial monitoring targets may include the Debian server hosting WordPress and other compatible Linux systems.

### Implementation Plan

1. Evaluate available Proxmox resources and Wazuh deployment requirements.
2. Deploy Wazuh using a supported installation method.
3. Enroll supported Linux endpoints.
4. Configure relevant security monitoring policies.
5. Review alerts associated with authentication failures and configuration changes.
6. Investigate findings and distinguish expected behavior from potentially suspicious activity.
7. Document example investigations and remediation steps.

### Example Investigation Scenarios

#### Authentication monitoring

Investigate repeated failed SSH login attempts and determine whether the activity reflects normal administrative mistakes or potentially suspicious behavior.

#### File integrity monitoring

Monitor selected configuration files and investigate unexpected modifications.

#### Web server monitoring

Review available system and application logs for unusual requests, repeated errors, or other activity warranting investigation.

#### Incident documentation

Record the observed event, supporting evidence, investigation steps, findings, and recommended corrective actions.

### Skills Developed

* Security event analysis.
* Endpoint monitoring.
* File integrity monitoring.
* Authentication and access-control auditing.
* Alert triage.
* Incident investigation and documentation.

⸻

## 6. Advanced Network Security Monitoring with Security Onion

### Objective

Explore a more comprehensive network security monitoring platform after establishing the foundational monitoring and detection services.

Security Onion integrates multiple tools and capabilities for network security monitoring, intrusion detection, and threat hunting.

This will be a later-stage project because the platform’s resource requirements and traffic visibility requirements may exceed those of the initial upgrades.

### Planned Activities

* Evaluate hardware and virtualization requirements.
* Determine how monitored network traffic will reach the sensor.
* Explore network metadata using Zeek.
* Investigate Suricata alerts.
* Correlate network observations with other available security events.
* Document investigation workflows and findings.

### Expected Outcomes

This project will provide an opportunity to explore how security analysts use network telemetry to investigate suspicious activity.

It will also help demonstrate how network monitoring, endpoint information, and centralized logs can complement one another.

### Skills Developed

* Network security monitoring.
* Network metadata analysis.
* Threat hunting fundamentals.
* Correlation of security events.
* Security operations workflows.

### Implementation Note

Security Onion is an optional advanced phase, not a prerequisite for completing the earlier projects. Its deployment will depend on available hardware, traffic visibility, and the resources required by the rest of the homelab.

⸻

## 7. Reliability, Security, and Operational Practices

Security monitoring is most useful when supported by reliable infrastructure and documented procedures.

Throughout these projects, I intend to incorporate the following practices:

### Configuration Management

Document network configurations, service settings, firewall policies, and deployment procedures in GitHub.

Sensitive information such as credentials, private keys, API tokens, and production secrets will not be committed to the repository.

### Backups

Maintain backups of important configurations and service data. Where possible, store backups separately from the systems they protect and periodically test restoration procedures.

### Access Control

Limit administrative access to trusted devices and management networks. Apply least-privilege principles wherever practical.

### Monitoring and Alerting

Configure relevant alerts for service availability, resource exhaustion, and security events.

### Testing and Validation

Verify network connectivity, VLAN isolation, firewall behavior, and monitoring functionality after significant configuration changes.

### Change Documentation

Record major changes, their purpose, the expected result, and any problems encountered. This creates a reproducible record of the project’s development.

⸻

## 8. Proposed Implementation Roadmap

The upgrades will be implemented incrementally to avoid introducing unnecessary complexity or disrupting existing services.

| Phase | Project | Completion Criteria |
|-|-|-|
| 1 | Network segmentation | VLANs configured, firewall rules tested, and network isolation documented |
| 2 | DNS monitoring | Pi-hole configuration documented and DNS behavior verified |
| 3 | Centralized monitoring | Selected systems report metrics and logs to the monitoring environment |
| 4 | Intrusion detection | Suricata generates and records test alerts from observable traffic |
| 5 | Endpoint security monitoring | Wazuh collects events from supported endpoints and produces reviewable alerts |
| 6 | Advanced security monitoring | Security Onion evaluated and deployed if hardware and resources permit |

Each phase will produce documentation describing the architecture, configuration, testing methodology, results, and lessons learned.


## Conclusion

This expansion will build on my existing homelab and create a structured path toward more advanced networking and security projects.

By implementing each component incrementally, I intend to develop a deeper understanding of how real infrastructure is segmented, monitored, secured, and maintained.

The focus is not simply on deploying software or acquiring equipment. It is on developing practical skills through implementation, testing, troubleshooting, and technical documentation.
