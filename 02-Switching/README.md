# 02 - Switching

## 📖 Overview

This section contains hands-on Cisco Packet Tracer labs covering fundamental and intermediate Layer 2 switching concepts required for CCNA preparation and practical networking.

Each lab includes practical configuration, verification, screenshots, Packet Tracer files, and troubleshooting practice.

The labs focus on VLANs, trunking, Inter-VLAN Routing, Router-on-a-Stick, Spanning Tree Protocol, and EtherChannel.

---

## 🎯 Learning Objectives

Through these labs, I am practicing:

- Basic Cisco switch configuration
- VLAN creation and management
- Access port configuration
- Trunk port configuration
- 802.1Q trunking
- Inter-VLAN Routing
- Router-on-a-Stick
- Spanning Tree Protocol (STP)
- STP port roles
- EtherChannel
- LACP
- Port-Channel
- Link redundancy
- Network troubleshooting
- Cisco IOS verification commands

---

## 🧪 Completed Labs

| # | Lab | Topics Covered | Status |
|---|-----|----------------|--------|
| 01 | [Basic Switch Configuration](./01-Basic-Switch/) | Switch CLI, hostname, basic configuration, verification | ✅ Completed |
| 02 | [VLAN](./02-VLAN/) | VLAN creation, VLAN assignment, access ports | ✅ Completed |
| 03 | [Trunk](./03-Trunk/) | Trunk ports, 802.1Q, VLAN traffic between switches | ✅ Completed |
| 04 | [Inter-VLAN Routing](./04-Inter-VLAN-Routing/) | VLAN routing, default gateway, inter-VLAN communication | ✅ Completed |
| 05 | [Router-on-a-Stick](./05-Router-on-a-Stick/) | Router subinterfaces, 802.1Q, inter-VLAN routing | ✅ Completed |
| 06 | [STP](./06-STP/) | Spanning Tree Protocol, root bridge, port roles, loop prevention | ✅ Completed |
| 07 | [EtherChannel](./07-EtherChannel/) | LACP, Port-Channel, link aggregation, redundancy | ✅ Completed |

---

## 🗂️ Lab Structure

Each practical lab contains, where applicable:

```text
README.md
Packet-Tracer-File.pkt
topology.png
configuration screenshots
verification screenshots
connectivity test screenshots
```

The README files document the objective, topology, port mapping, IP addressing, configuration, verification, connectivity testing, concepts covered, and learning outcome.

---

## 🛠️ Tools Used

- Cisco Packet Tracer 9.x
- Cisco IOS CLI
- GitHub

---

## 📚 Core Concepts Practiced

### Layer 2 Switching

- Basic switch configuration
- MAC address learning
- Access ports
- Trunk ports
- VLANs
- Broadcast domains
- 802.1Q

### VLAN & Routing

- VLAN creation
- VLAN assignment
- Inter-VLAN Routing
- Default gateways
- Router-on-a-Stick
- Router subinterfaces

### Spanning Tree Protocol

- STP
- Root Bridge
- Root Port
- Designated Port
- Alternate / Blocking Port
- Layer 2 loop prevention
- STP verification

### EtherChannel

- EtherChannel
- LACP
- Link Aggregation Control Protocol
- LACP Active Mode
- Port-Channel
- Layer 2 EtherChannel
- Link aggregation
- Link redundancy
- Increased bandwidth
- Link failure recovery

---

## 🔍 Common Verification Commands

### VLAN Verification

```bash
show vlan brief
```

### Trunk Verification

```bash
show interfaces trunk
```

### Interface Status

```bash
show interfaces status
```

### MAC Address Table

```bash
show mac address-table
```

### STP Verification

```bash
show spanning-tree
show spanning-tree vlan 10
```

### EtherChannel Verification

```bash
show etherchannel summary
show interfaces port-channel 1
```

### Router Interface Verification

```bash
show ip interface brief
```

### Routing Table

```bash
show ip route
```

### Configuration Verification

```bash
show running-config
show startup-config
```

---

## 🧪 Connectivity Testing

Connectivity is verified throughout the switching labs using commands such as:

```bash
ping <destination-ip>
```

Examples:

```bash
ping 192.168.10.10
ping 192.168.10.20
ping 192.168.20.10
```

Successful ping results are used to verify VLAN connectivity, trunk operation, Inter-VLAN Routing, and EtherChannel connectivity.

---

## 🛠️ Troubleshooting Practice

The switching labs also provide practical troubleshooting experience.

Common troubleshooting steps include:

1. Check interface status.
2. Verify VLAN assignments.
3. Verify trunk configuration.
4. Check MAC address learning.
5. Verify STP status.
6. Verify EtherChannel status.
7. Check router interface status.
8. Test connectivity using ping.
9. Check Cisco IOS configuration.

Common commands used during troubleshooting:

```bash
show interfaces status
show vlan brief
show interfaces trunk
show mac address-table
show spanning-tree
show etherchannel summary
show ip interface brief
show running-config
```

---

## 🎓 Learning Outcome

These switching labs provide hands-on practice with Cisco Layer 2 switching technologies.

I practiced configuring VLANs, access ports, trunk links, Inter-VLAN Routing, Router-on-a-Stick, STP, and EtherChannel using Cisco IOS commands.

I also learned how to verify network configurations, troubleshoot connectivity issues, understand Layer 2 redundancy, and test network behavior using Cisco Packet Tracer.

These labs strengthen my practical switching knowledge as part of my CCNA 200-301 preparation.

---

## 🚀 Upcoming Topics

The next section of my CCNA practical portfolio will cover routing concepts, including:

- Static Routing
- Default Routing
- Dynamic Routing
- RIP
- OSPF
- DHCP
- DHCP Relay
- NAT
- ACL
- WAN Technologies
- Network Troubleshooting

---

## 👨‍💻 Author

**Mohamed Ashik**

Aspiring Network Engineer | CCNA (200-301) Student

GitHub: [mohamedashik-cpu](https://github.com/mohamedashik-cpu)
