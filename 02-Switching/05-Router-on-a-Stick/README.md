# Inter-VLAN Routing using Router-on-a-Stick

## 📖 Overview

In this lab, Inter-VLAN Routing is configured using a Cisco router and a Layer 2 switch.

VLAN 10 and VLAN 20 are created to separate the network into different broadcast domains. Access ports are assigned to the respective VLANs, and a trunk link is configured between the switch and router.

Router-on-a-Stick is implemented using router subinterfaces with 802.1Q encapsulation to enable communication between VLAN 10 and VLAN 20.

---

## 🎯 Objectives

* Create VLAN 10 and VLAN 20.
* Assign switch ports to the appropriate VLANs.
* Configure access ports.
* Configure a trunk link between the switch and router.
* Configure router subinterfaces.
* Configure 802.1Q encapsulation.
* Configure default gateways for each VLAN.
* Enable communication between different VLANs.
* Verify VLAN configuration.
* Verify trunk configuration.
* Verify routing and connectivity.

---

## 🖥️ Devices Used

* 1 × Cisco Router
* 1 × Cisco Layer 2 Switch
* 4 × PCs
* Cisco Packet Tracer

---

## 🌐 Network Topology

The following topology was created in Cisco Packet Tracer:

![Router-on-a-Stick Network Topology](./topology.png)

---

## 🔌 Port Mapping

| Switch Port | Connected Device | VLAN / Purpose |
| ----------- | ---------------- | -------------- |
| Fa0/1       | PC0              | VLAN 10        |
| Fa0/2       | PC1              | VLAN 10        |
| Fa0/3       | PC2              | VLAN 20        |
| Fa0/4       | PC3              | VLAN 20        |
| Fa0/5       | Router Fa0/0     | Trunk          |

---

## 📊 VLAN Configuration

| VLAN ID | VLAN Name | Network         | Default Gateway |
| ------- | --------- | --------------- | --------------- |
| 10      | HR        | 192.168.10.0/24 | 192.168.10.1    |
| 20      | SALES     | 192.168.20.0/24 | 192.168.20.1    |

---

## 🌐 IP Addressing

| Device | VLAN | IP Address    | Subnet Mask   | Default Gateway |
| ------ | ---: | ------------- | ------------- | --------------- |
| PC0    |   10 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1    |
| PC1    |   10 | 192.168.10.20 | 255.255.255.0 | 192.168.10.1    |
| PC2    |   20 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1    |
| PC3    |   20 | 192.168.20.20 | 255.255.255.0 | 192.168.20.1    |

---

# ⚙️ Configuration

## 1. Configure VLANs on SW0

```bash
enable
configure terminal

hostname SW0

vlan 10
name HR
exit

vlan 20
name SALES
exit
```

### Configure VLAN 10

```bash
interface range fa0/1 - 2
switchport mode access
switchport access vlan 10
exit
```

### Configure VLAN 20

```bash
interface range fa0/3 - 4
switchport mode access
switchport access vlan 20
exit
```

### Configure Trunk to Router

```bash
interface fa0/5
switchport mode trunk
exit
```

---

## 2. Configure Router-on-a-Stick

Router interface connected to SW0:

```text
Router Fa0/0 ↔ SW0 Fa0/5
```

### Enable Physical Interface

```bash
enable
configure terminal

interface fa0/0
no shutdown
exit
```

### VLAN 10 Subinterface

```bash
interface fa0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit
```

### VLAN 20 Subinterface

```bash
interface fa0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit
```

### Save Configuration

```bash
end
copy running-config startup-config
```

---

# 🔍 Verification

## Verify VLANs

On SW0:

```bash
show vlan brief
```

Expected:

```text
VLAN 10 → Fa0/1, Fa0/2
VLAN 20 → Fa0/3, Fa0/4
```

### VLAN Verification Screenshot

![VLAN Verification](./show-vlan-brief.png)

---

## Verify Trunk Port

```bash
show interfaces trunk
```

Expected:

```text
Fa0/5 → Trunking
802.1Q
```

VLAN 10 and VLAN 20 should be allowed and active on the trunk.

### Trunk Verification Screenshot

![Trunk Verification](./show-interfaces-trunk.png)

---

## Verify Router Interfaces

On the router:

```bash
show ip interface brief
```

Expected:

```text
FastEthernet0/0      up/up
FastEthernet0/0.10   192.168.10.1   up/up
FastEthernet0/0.20   192.168.20.1   up/up
```

### Router Interface Verification Screenshot

![Router Interface Verification](./show-ip-interface-brief.png)

---

## Verify Routing Table

```bash
show ip route
```

Expected connected networks:

```text
C 192.168.10.0/24
C 192.168.20.0/24
```

### Routing Table Screenshot

![Routing Table](./show-ip-route.png)

---

# 🧪 Connectivity Testing

## VLAN 10 Gateway Test

From PC0:

```bash
ping 192.168.10.1
```

Expected:

```text
Successful
```

---

## VLAN 20 Gateway Test

From PC2:

```bash
ping 192.168.20.1
```

Expected:

```text
Successful
```

---

## Same-VLAN Communication

### PC0 → PC1

From PC0:

```bash
ping 192.168.10.20
```

PC0 and PC1 belong to VLAN 10.

Expected:

```text
Successful
```

### PC2 → PC3

From PC2:

```bash
ping 192.168.20.20
```

PC2 and PC3 belong to VLAN 20.

Expected:

```text
Successful
```

### Same-VLAN Connectivity Screenshot

![Same-VLAN Connectivity](./same-vlan-ping.png)

---

## Inter-VLAN Communication

### PC0 → PC2

From PC0:

```bash
ping 192.168.20.10
```

PC0 belongs to VLAN 10 and PC2 belongs to VLAN 20.

Expected:

```text
Successful
```

This confirms that traffic is successfully routed between VLAN 10 and VLAN 20 through the router.

### Inter-VLAN Connectivity Screenshot

![Inter-VLAN Connectivity](./inter-vlan-ping.png)

---

## 🛣️ Traceroute Test

From PC0:

```bash
tracert 192.168.20.10
```

The traffic should pass through the router before reaching the destination in VLAN 20.

### Traceroute Screenshot

![Traceroute Verification](./traceroute.png)

---

# 🔄 Traffic Flow

```text
PC0
192.168.10.10
     |
   VLAN 10
     |
    SW0
     |
   Trunk
     |
 Router Fa0/0
     |
+----------------------+
| Router Subinterfaces |
|                      |
| Fa0/0.10 → VLAN 10   |
| Fa0/0.20 → VLAN 20   |
+----------------------+
     |
   Routing
     |
   VLAN 20
     |
    SW0
     |
   PC2
192.168.20.10
```

---

# 📸 Screenshots

## Network Topology

![Router-on-a-Stick Topology](./topology.png)

## VLAN Verification

![VLAN Verification](./show-vlan-brief.png)

## Trunk Verification

![Trunk Verification](./show-interfaces-trunk.png)

## Router Interface Verification

![Router Interface Verification](./show-ip-interface-brief.png)

## Routing Table

![Routing Table](./show-ip-route.png)

## Same-VLAN Connectivity

![Same-VLAN Connectivity](./same-vlan-ping.png)

## Inter-VLAN Connectivity

![Inter-VLAN Connectivity](./inter-vlan-ping.png)

## Traceroute

![Traceroute Verification](./traceroute.png)

---

# 📚 Concepts Covered

* VLAN
* Access Port
* Trunk Port
* 802.1Q Encapsulation
* Inter-VLAN Routing
* Router-on-a-Stick
* Router Subinterfaces
* Default Gateway
* Broadcast Domains
* Layer 2 Switching
* Layer 3 Routing
* Connectivity Testing
* Network Troubleshooting

---

# 🎓 Learning Outcome

This lab helped me understand how communication between different VLANs is achieved using a Layer 3 router.

I configured VLANs and access ports on a Layer 2 switch, configured a trunk link between the switch and router, created router subinterfaces using 802.1Q encapsulation, configured VLAN-specific default gateways, and verified end-to-end connectivity between different VLANs using ping and traceroute.

---

## 🛠️ Software Used

* Cisco Packet Tracer 9.x

---

## 👨‍💻 Author

**Mohamed Ashik**

Aspiring Network Engineer | CCNA (200-301)

GitHub: https://github.com/mohamedashik-cpu
