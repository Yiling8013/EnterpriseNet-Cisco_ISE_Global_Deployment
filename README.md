# EnterpriseNet-Cisco_ISE_Global_Deployment

## Overview

This design illustrates a globally distributed Cisco Identity Services Engine (ISE) architecture deployed across three geographic regions: America, Europe/Africa, and Asia Pacific. The America data center serves as the central control hub, hosting the Administration (PAN), Monitoring (MnT), and pxGrid nodes to provide centralized policy management, logging, and ecosystem integration. Each regional data center deploys a scalable group of Policy Service Nodes (PSNs), locally integrated with Active Directory (AD) and Certificate Authority (CA) services to ensure low-latency authentication, authorization, and accounting (AAA) operations.

To optimize performance and resiliency, AAA traffic is distributed locally within each region through load balancers, minimizing cross-region dependencies and reducing authentication delays. The architecture supports high availability through redundant node roles and inter-region synchronization, ensuring continuous policy enforcement even during partial failures.

This repository includes topology diagrams, device configurations, and validation commands used to build and test the environment.

---

## Topology
![Lab Topology](./Topology/VXLAN_EVPN_Multisite_LAB1_Visio.png)
The Visio network diagram shown above illustrates the overall architecture of this lab environment.

---

# Cisco ISE Global Distributed Deployment IP Scheme

## 1. Global Management and Replication Networks

| Segment | Subnet | Purpose |
|--------|--------|---------|
| Global ISE Management | 10.10.0.0/24 | Management access for PAN, MnT, pxGrid, and PSN nodes |
| Global Replication Network | 10.10.1.0/24 | ISE node replication and synchronization |
| Global pxGrid Services | 10.10.2.0/24 | pxGrid service communication |
| Global Monitoring / Logging | 10.10.3.0/24 | Monitoring, syslog, and reporting traffic |

## 2. America Data Center

### 2.1 Core ISE Infrastructure

| Device / Service | IP Address | Subnet Mask | Purpose |
|------------------|------------|-------------|---------|
| ISE Primary PAN | 10.10.0.11 | /24 | Primary Administration Node |
| ISE Secondary PAN | 10.10.0.12 | /24 | Secondary Administration Node |
| ISE Primary MnT | 10.10.0.21 | /24 | Primary Monitoring Node |
| ISE pxGrid Node | 10.10.0.31 | /24 | pxGrid services |

### 2.2 America PSN Group

| Device / Service | IP Address | Subnet Mask | Purpose |
|------------------|------------|-------------|---------|
| America PSN-1 | 10.20.10.11 | /24 | Policy Service Node |
| America PSN-2 | 10.20.10.12 | /24 | Policy Service Node |
| America PSN-3 | 10.20.10.13 | /24 | Policy Service Node |
| America PSN-4 | 10.20.10.14 | /24 | Policy Service Node |
| America PSN VIP (Load Balancer) | 10.20.10.10 | /24 | AAA virtual IP for regional clients |

### 2.3 Identity and Trust Services

| Device / Service | IP Address | Subnet Mask | Purpose |
|------------------|------------|-------------|---------|
| America AD Server | 10.20.20.10 | /24 | Active Directory / LDAP / Kerberos |
| America CA Server | 10.20.20.20 | /24 | Certificate Authority / PKI |

## 3. Europe Africa Data Center

### 3.1 Europe Africa PSN Group

| Device / Service | IP Address | Subnet Mask | Purpose |
|------------------|------------|-------------|---------|
| Europe Africa PSN-1 | 10.30.10.11 | /24 | Policy Service Node |
| Europe Africa PSN-2 | 10.30.10.12 | /24 | Policy Service Node |
| Europe Africa PSN-3 | 10.30.10.13 | /24 | Policy Service Node |
| Europe Africa PSN-4 | 10.30.10.14 | /24 | Policy Service Node |
| Europe Africa PSN VIP (Load Balancer) | 10.30.10.10 | /24 | AAA virtual IP for regional clients |

### 3.2 Identity and Trust Services

| Device / Service | IP Address | Subnet Mask | Purpose |
|------------------|------------|-------------|---------|
| Europe Africa AD Server | 10.30.20.10 | /24 | Active Directory / LDAP / Kerberos |
| Europe Africa CA Server | 10.30.20.20 | /24 | Certificate Authority / PKI |

## 4. Asia Pacific Data Center

### 4.1 Asia Pacific PSN Group

| Device / Service | IP Address | Subnet Mask | Purpose |
|------------------|------------|-------------|---------|
| Asia Pacific PSN-1 | 10.40.10.11 | /24 | Policy Service Node |
| Asia Pacific PSN-2 | 10.40.10.12 | /24 | Policy Service Node |
| Asia Pacific PSN-3 | 10.40.10.13 | /24 | Policy Service Node |
| Asia Pacific PSN-4 | 10.40.10.14 | /24 | Policy Service Node |
| Asia Pacific PSN VIP (Load Balancer) | 10.40.10.10 | /24 | AAA virtual IP for regional clients |

### 4.2 Identity and Trust Services

| Device / Service | IP Address | Subnet Mask | Purpose |
|------------------|------------|-------------|---------|
| Asia Pacific AD Server | 10.40.20.10 | /24 | Active Directory / LDAP / Kerberos |
| Asia Pacific CA Server | 10.40.20.20 | /24 | Certificate Authority / PKI |

## 5. Regional AAA Client Access Networks

| Region | Subnet | Purpose |
|--------|--------|---------|
| America AAA Clients | 10.21.0.0/16 | Switches, WLCs, VPN gateways, and NADs sending AAA requests |
| Europe Africa AAA Clients | 10.31.0.0/16 | Switches, WLCs, VPN gateways, and NADs sending AAA requests |
| Asia Pacific AAA Clients | 10.41.0.0/16 | Switches, WLCs, VPN gateways, and NADs sending AAA requests |

## 6. Suggested DNS / FQDN Mapping

| FQDN | IP Address | Purpose |
|------|------------|---------|
| ise-pan.america.lab.local | 10.10.0.11 | Primary PAN |
| ise-span.america.lab.local | 10.10.0.12 | Secondary PAN |
| ise-mnt.america.lab.local | 10.10.0.21 | Monitoring node |
| ise-pxg.america.lab.local | 10.10.0.31 | pxGrid node |
| ise-psn-na.lab.local | 10.20.10.10 | America PSN VIP |
| ise-psn-emea.lab.local | 10.30.10.10 | Europe Africa PSN VIP |
| ise-psn-apac.lab.local | 10.40.10.10 | Asia Pacific PSN VIP |

## 7. Design Notes

- The America data center hosts the centralized PAN, MnT, and pxGrid services.
- Each region has a dedicated PSN group for local AAA processing.
- Load balancers provide a regional AAA virtual IP for authentication requests.
- AD and CA services are deployed regionally to reduce latency and dependency on long-distance WAN links.
- PSNs communicate with PAN for policy synchronization and with MnT for logging and monitoring.

---

## Device Configuration 
Please refer to project folder.
