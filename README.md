# MikroTik Hotspot Voucher Infrastructure

A professional small-scale hotspot infrastructure project built using MikroTik devices with a separated Core Router and Dedicated Hotspot Router architecture.

This project demonstrates how to build a scalable WiFi voucher system using MikroTik RB760iGS as the core router and MikroTik RB951Ui-2HnD as the hotspot gateway.

---

# Project Objectives

This project was created to simulate a real-world hotspot deployment for environments such as:

- Coffee Shops
- Small Businesses
- Tourism Areas
- Farms and Outdoor Areas
- Guest Houses
- Small Community Networks

The main objective is to separate routing functions from hotspot services to make the network easier to manage, secure, and scale.

---

# Network Architecture

```
                           Internet
                               |
                               |
                     IndiHome ONT / Modem
                        192.168.100.1
                               |
                               |
                        ether1 (WAN)
                         RB760iGS
                       Core Router
                    192.168.100.144
                               |
                    Management Network
                        172.20.1.0/24
                               |
                  ether5 (PoE Out to RB951)
                               |
                        ether1 (UPLINK)
                       RB951Ui-2HnD
                    Hotspot Router / AP
                      Management IP
                        172.20.1.2
                               |
               --------------------------------
               |                              |
           LAN Ports                      WLAN 2.4 GHz
                                               |
                                       KEBON-HOTSPOT
                                               |
                                       172.20.10.0/24
                                               |
                                         WiFi Clients
```

---

# Network Addressing Plan

## WAN Network

| Device | Address |
|---|---|
| ISP Modem | 192.168.100.1 |
| RB760 WAN | DHCP Client (192.168.100.x) |

---

## Management Network

| Device | Address | Function |
|---|---|---|
| RB760iGS | 172.20.1.1 | Core Router |
| RB951Ui-2HnD | 172.20.1.2 | Hotspot Management |

---

## Hotspot Network

| Network | Description |
|---|---|
| 172.20.10.0/24 | Hotspot User Network |
| Gateway | 172.20.10.1 |
| DHCP Pool | 172.20.10.100 - 172.20.10.254 |

---

# Hardware Used

| Device | Role |
|---|---|
| MikroTik RB760iGS | Core Router |
| MikroTik RB951Ui-2HnD | Hotspot Router & Access Point |
| IndiHome ONT | Internet Source |

---

# Implemented Features

Current implementation:

## RB760iGS

- WAN DHCP Client
- Internet Gateway
- NAT Masquerade
- DNS Forwarding
- Management Network
- PoE Out to RB951

---

## RB951Ui-2HnD

- Dedicated Hotspot Gateway
- Wireless Access Point
- Captive Portal
- DHCP Server for Hotspot Users
- User Authentication
- Bandwidth Management

---

## Hotspot Policy

Current hotspot policy:

| Parameter | Value |
|---|---|
| SSID | KEBON-HOTSPOT |
| Voucher Type | Daily |
| Bandwidth | 2 Mbps Upload / 2 Mbps Download |
| Shared User | 1 Device |
| Authentication | Username & Password |
| Captive Portal | Enabled |

---

# Repository Structure

```
mikrotik-hotspot-voucher/
│
├── README.md
│
├── docs/
│   ├── 01-project-overview.md
│   ├── 02-network-topology.md
│   ├── 03-ip-addressing-plan.md
│   ├── 04-rb760-core-router-configuration.md
│   ├── 05-rb951-hotspot-router-configuration.md
│   ├── 06-hotspot-voucher-configuration.md
│   ├── 07-security-hardening.md
│   ├── 08-troubleshooting.md
│   └── 09-future-development.md
│
├── scripts/
│   ├── rb760-core.rsc
│   └── rb951-hotspot.rsc
│
├── backup/
│   ├── RB760-CORE.backup
│   └── RB951-HOTSPOT.backup
│
└── assets/
    ├── topology/
    ├── screenshots/
    └── hotspot-login/
```

---

# Documentation Guide

Each documentation contains:

- Network Objective
- Network Design
- Configuration using Winbox GUI
- Configuration using MikroTik Terminal (CLI)
- Verification Procedure
- Troubleshooting

This repository is designed not only as a configuration backup, but also as a learning resource for MikroTik network deployment.

---

# Current Status

| Feature | Status |
|---|---|
| Core Router Configuration | ✅ Completed |
| Internet Access | ✅ Completed |
| Management Network | ✅ Completed |
| RB951 PoE Uplink | ✅ Completed |
| Wireless Access Point | ✅ Completed |
| Hotspot DHCP | ✅ Completed |
| Captive Portal | ✅ Completed |
| Voucher Authentication | ✅ Completed |
| 2 Mbps User Limit | ✅ Completed |
| Single Device Login | ✅ Completed |
| Firewall Hardening | 🔄 Planned |
| Mikhmon Integration | 🔄 Planned |
| Website Integration | 🔄 Planned |
| VLAN Segmentation | 🔄 Future |

---

# Future Development

This project will continue to evolve with:

- Advanced Firewall Policy
- Management Access Restriction
- Mikhmon Integration
- Automated Voucher Management
- Internal Website Hosting
- Port Forwarding
- VLAN Implementation
- Network Monitoring

---

# License

This project is intended for learning, research, and small-scale network deployment references.
