Production-Grade Private Cloud from Bare Metal

  A Full-Lifecycle IaaS Deployment Using Canonical MicroCloud on Cisco UCS Infrastructure

![Platform](https://img.shields.io/badge/Platform-Cisco%20UCS%20C220%20M5-00BCEB?logo=cisco&logoColor=white)
![OS](https://img.shields.io/badge/OS-Ubuntu%2024.04%20LTS-E95420?logo=ubuntu&logoColor=white)
![Cloud](https://img.shields.io/badge/Cloud-Canonical%20MicroCloud-772953?logo=canonical&logoColor=white)
![Storage](https://img.shields.io/badge/Storage-Ceph-EF5C55?logo=ceph&logoColor=white)
![Monitoring](https://img.shields.io/badge/Monitoring-Prometheus%20%2B%20Grafana-E6522C?logo=prometheus&logoColor=white)
![Status](https://img.shields.io/badge/Status-Operational-1E8449)

> A three-node, high-availability private cloud built entirely from bare metal — delivering compute, distributed storage, and software-defined networking, with full real-time observability and secure remote access.

 Overview

> **[ Insert screenshot: Mission Control Grafana dashboard — live ]**

This project transforms three bare-metal **Cisco UCS C220 M5** rack servers into a fully functional **Infrastructure-as-a-Service (IaaS)** platform using **Canonical MicroCloud** (LXD + MicroCeph + MicroOVN). It provides the same core capabilities as a public cloud — on-demand virtual machines and containers, resilient distributed storage, and isolated virtual networks — running entirely on self-hosted, open-source infrastructure.

I built this end-to-end: from RAID controller configuration and OS installation, through cluster formation and distributed storage, to a custom monitoring stack and secure remote operation. It doubles as my hands-on preparation for the **Certified OpenStack Administrator (COA)** certification and my learning path into Canonical's cloud ecosystem.

---

 Architecture

┌─────────────────────────────────────────┐
                │        Private Cloud Cluster            │
                │      (Canonical MicroCloud 2.1.3)       │
                └─────────────────────────────────────────┘
                                  │
      ┌───────────────────────────┼───────────────────────────┐
      │                           │                           │
┌─────▼─────┐               ┌─────▼─────┐               ┌─────▼─────┐
│   SVR01   │               │   SVR02   │               │   SVR03   │
│  Leader   │◄─────────────►│   Voter   │◄─────────────►│   Voter   │
│           │               │           │               │           │
│ Prometheus│               │  Grafana  │               │           │
│  9 disks* │               │  9 OSDs   │               │  8 OSDs   │
└───────────┘               └───────────┘               └───────────┘
      │                           │                           │
      └───────────────────────────┴───────────────────────────┘
                LXD (Compute) · Ceph (Storage) · OVN (Network)
                          17 OSDs · ~19 TiB · HEALTH_OK

* SVR01 storage expansion tracked as a planned enhancement

---

 Technology Stack

| Layer | Technology | Role |
|-------|-----------|------|
| **Orchestration** | Canonical MicroCloud | Single-command cluster management |
| **Compute** | LXD | Containers & virtual machines |
| **Distributed Storage** | MicroCeph (Ceph) | Replicated block storage across nodes |
| **Networking** | MicroOVN (OVN) | Software-defined tenant networking |
| **Operating System** | Ubuntu Server 24.04 LTS | Base platform |
| **Metrics** | Prometheus + Node Exporter | Time-series metrics collection |
| **Visualization** | Grafana | Custom real-time dashboards |
| **Remote Access** | Tailscale (WireGuard) | Secure zero-config mesh VPN |

---

 What This Project Demonstrates

- **Bare-metal to cloud**: full lifecycle deployment with no pre-existing virtualization
- **Enterprise hardware**: Cisco CIMC, RAID/JBOD configuration, controller-level troubleshooting
- **Distributed systems**: Ceph storage cluster with replication and quorum management
- **Software-defined networking**: OVN virtual networks, isolation, NAT, and forwarding
- **High availability**: workloads and data survive single-node failure
- **Observability**: custom multi-node monitoring dashboard with dynamic thresholds
- **Infrastructure automation**: systemd-managed services with auto-recovery on reboot
- **Secure operations**: encrypted remote management without public internet exposure
- **Systematic troubleshooting**: 16 documented real-world issues with root-cause analysis

---

 Key Challenges Solved

A few of the real problems I diagnosed and resolved during this build (full details in the [Troubleshooting Guide](troubleshooting-guide.pdf)):

| Challenge | Resolution |
|-----------|-----------|
| RAID controller invisible to OS despite healthy status in management console | Diagnosed a data-plane vs. management-plane split; resolved with physical card reseat |
| Cluster unreachable remotely after reboot | Traced to disabled IP forwarding on the gateway node; made persistent |
| Monitoring stack lost data on every restart | Converted manual processes to systemd services + persistent container networking + auto-start |
| Containers couldn't reach the internet on the virtual network | Added a dedicated NAT bridge network as a secondary interface |
| Storage database wouldn't start | Identified quorum requirement — cluster needs a majority of nodes online |

---

## 📊 Monitoring Dashboard

The cluster is monitored through a custom **"Mission Control"** Grafana dashboard featuring:

- **Fleet-wide rollups** — aggregate CPU, memory, and network across all nodes
- **Live cluster heartbeat** — real-time CPU pulse per node
- **Per-node vitals** — CPU, RAM, disk, and load gauges with dynamic thresholds
- **Throughput trends** — network receive/transmit and memory pressure over time

The importable dashboard definition is included: [`mission-control-dashboard.json`](mission-control-dashboard.json)

> **[ Insert screenshot: dashboard heartbeat + fleet panels ]**

---

## 📚 Documentation

This repository includes complete, reproducible documentation:

- 📖 **[Project Report](project-report.pdf)** — full architecture and 7-phase deployment, reproducible on comparable hardware
- 🔧 **[Troubleshooting Guide](troubleshooting-guide.pdf)** — 16 real issues with symptoms, root causes, and fixes

---

 Project Goals

This was a self-directed project with three aims:

1. **Master private-cloud infrastructure** hands-on, from hardware to observability
2. **Prepare for the Certified OpenStack Administrator (COA)** exam — the concepts (compute, networking, block storage, identity) map directly onto OpenStack
3. **Build toward a cloud engineering role at Canonical**, working with the Ubuntu ecosystem I used throughout

---

 Skills Applied

`Cloud Infrastructure` · `Canonical MicroCloud` · `LXD` · `Ceph` · `OVN` · `Ubuntu Server` · `Cisco UCS` · `CIMC` · `KVM/Virtualization` · `Prometheus` · `Grafana` · `Linux System Administration` · `Netplan` · `systemd` · `Distributed Storage` · `Software-Defined Networking` · `High Availability` · `Tailscale/WireGuard` · `Troubleshooting`

---

 Contact

**Horus Ngu Djomou Sielatshom** — Network Technician
Building toward Cloud Engineering · Pursuing COA Certification
 GitHub: [github.com/kinghorus12](https://github.com/kinghorus12)

Built with open-source tools · Documented for reproducibility 
