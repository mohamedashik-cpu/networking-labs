# EtherChannel using LACP

## 📖 Overview

In this lab, EtherChannel is configured between two Cisco Layer 2 switches using LACP (Link Aggregation Control Protocol).

Multiple physical links are bundled together to form a single logical interface called a Port-Channel.

EtherChannel provides increased bandwidth, redundancy, and improved link utilization. LACP is used to dynamically negotiate and maintain the EtherChannel between the switches.

In this lab, four physical FastEthernet links are bundled into Port-Channel 1 and configured as a trunk link.

---

## 🎯 Objectives

- Understand EtherChannel.
- Understand Link Aggregation Control Protocol (LACP).
- Configure multiple physical links as one logical link.
- Configure LACP using active mode.
- Configure Port-Channel.
- Configure trunking over EtherChannel.
- Verify EtherChannel operation.
- Test network connectivity.
- Perform a physical link failure test.
- Verify redundancy and link recovery.

---

## 🖥️ Devices Used

- 2 × Cisco Catalyst 3560-24PS Switches
- 2 × PCs
- Cisco Packet Tracer

---

## 🌐 Network Topology

![EtherChannel Network Topology](01-topology.png)

---

## 🔌 Port Mapping

### SW0

| Switch Port | Connected Device | Purpose |
|---|---|---|
| Fa0/1 | PC0 | Access Port |
| Fa0/2 | SW1 Fa0/2 | EtherChannel |
| Fa0/3 | SW1 Fa0/3 | EtherChannel |
| Fa0/4 | SW1 Fa0/4 | EtherChannel |
| Fa0/5 | SW1 Fa0/5 | EtherChannel |

### SW1

| Switch Port | Connected Device | Purpose |
|---|---|---|
| Fa0/1 | PC1 | Access Port |
| Fa0/2 | SW0 Fa0/2 | EtherChannel |
| Fa0/3 | SW0 Fa0/3 | EtherChannel |
| Fa0/4 | SW0 Fa0/4 | EtherChannel |
| Fa0/5 | SW0 Fa0/5 | EtherChannel |

---

## 📊 VLAN Configuration

| VLAN ID | VLAN Name | Network |
|---|---|---|
| 10 | USERS | 192.168.10.0/24 |

---

## 🌐 IP Addressing

| Device | VLAN | IP Address | Subnet Mask |
|---|---:|---|---|
| PC0 | 10 | 192.168.10.10 | 255.255.255.0 |
| PC1 | 10 | 192.168.10.20 | 255.255.255.0 |

---

# ⚙️ Configuration

## 1. Configure VLAN 10 on SW0

```bash
enable
configure terminal

hostname SW0

vlan 10
name USERS
exit
