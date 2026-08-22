# 01 - Static Routing

## 📖 Overview
This lab demonstrates the fundamental configuration, operation, and verification of **IPv4 Static Routing** in a multi-router Cisco network environment. Static routing involves manually configuring routes in the router's routing table to reach destination networks that are not directly connected. 

In this lab, three Cisco routers (**R1**, **R2**, and **R3**) interconnect two isolated Local Area Networks (**LAN 1** and **LAN 2**). Point-to-point WAN links utilize power-efficient `/30` subnets, and manual static routes are defined using next-hop IP addresses to establish complete end-to-end bidirectional communication between end devices.

---

## 🎯 Objectives
* Design and implement a multi-router topology connecting distinct subnets.
* Apply efficient IPv4 addressing, using `/24` subnets for LANs and `/30` subnets for point-to-point WAN links.
* Configure static routes on edge and intermediate routers using the next-hop IPv4 addressing method.
* Verify interface operational states (`up/up`) and inspect routing tables for static route entries (`S`).
* Validate end-to-end ICMP reachability (`ping`) and trace hop-by-hop packet forwarding paths (`tracert`).
* Analyze the forwarding decisions made by routers at each step of the transit path.

---

## 🖥️ Devices Used

| Device Type | Device Model | Quantity | Role / Description |
|---|---|---|---|
| **Router** | Cisco 2911 / 1941 | 3 | Core & Edge Routing Devices (R1, R2, R3) |
| **End Device** | Generic PC | 2 | End Hosts for LAN 1 (PC1) and LAN 2 (PC2) |
| **Cabling** | Copper Straight-Through / Cross-Over | 4 | Physical media connections for LAN & WAN links |

---

## 🌐 Network Topology
![Network Topology](01-topology.png)

---

## 🔌 Port Mapping

| Source Device | Source Interface | Connected Device | Connected Interface | Link Type |
|---|---|---|---|---|
| **PC1** | FastEthernet0 | **R1** | FastEthernet0/0 | Local LAN Link (LAN 1) |
| **R1** | FastEthernet0/1 | **R2** | FastEthernet0/0 | Point-to-Point WAN Link |
| **R2** | FastEthernet0/1 | **R3** | FastEthernet0/0 | Point-to-Point WAN Link |
| **R3** | FastEthernet0/1 | **PC2** | FastEthernet0 | Local LAN Link (LAN 2) |

---

## 🛣️ Routing & Subnet Allocation

| Subnet Network | Subnet Mask | CIDR | Purpose / Scope | Designated Interfaces |
|---|---|---|---|---|
| `192.168.10.0` | `255.255.255.0` | `/24` | LAN 1 (PC1 Subnet) | R1 Fa0/0, PC1 NIC |
| `10.0.12.0` | `255.255.255.252` | `/30` | WAN Link: R1 ↔ R2 | R1 Fa0/1, R2 Fa0/0 |
| `10.0.23.0` | `255.255.255.252` | `/30` | WAN Link: R2 ↔ R3 | R2 Fa0/1, R3 Fa0/0 |
| `192.168.20.0` | `255.255.255.0` | `/24` | LAN 2 (PC2 Subnet) | R3 Fa0/1, PC2 NIC |

---

## 🌐 IP Addressing Table

| Device | Interface | IPv4 Address | Subnet Mask | Default Gateway | Connected Network / Role |
|---|---|---|---|---|---|
| **R1** | FastEthernet0/0 | `192.168.10.1` | `255.255.255.0` | N/A | LAN 1 Gateway |
| **R1** | FastEthernet0/1 | `10.0.12.1` | `255.255.255.252` | N/A | WAN to R2 |
| **R2** | FastEthernet0/0 | `10.0.12.2` | `255.255.255.252` | N/A | WAN to R1 |
| **R2** | FastEthernet0/1 | `10.0.23.1` | `255.255.255.252` | N/A | WAN to R3 |
| **R3** | FastEthernet0/0 | `10.0.23.2` | `255.255.255.252` | N/A | WAN to R2 |
| **R3** | FastEthernet0/1 | `192.168.20.1` | `255.255.255.0` | N/A | LAN 2 Gateway |
| **PC1** | FastEthernet0 | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` | Host on LAN 1 |
| **PC2** | FastEthernet0 | `192.168.20.10` | `255.255.255.0` | `192.168.20.1` | Host on LAN 2 |

---

## ⚙️ Configuration

### 1. Router R1 Configuration
```text
Router> enable
Router# configure terminal
Router(config)# hostname R1

! Configure LAN 1 Interface
R1(config)# interface FastEthernet0/0
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit

! Configure WAN Link to R2
R1(config)# interface FastEthernet0/1
R1(config-if)# ip address 10.0.12.1 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# exit

! Configure Static Route to Remote LAN 2 via R2 next-hop
R1(config)# ip route 192.168.20.0 255.255.255.0 10.0.12.2
R1(config)# end
R1# copy running-config startup-config
2. Router R2 ConfigurationPlaintextRouter> enable
Router# configure terminal
Router(config)# hostname R2

! Configure WAN Link to R1
R2(config)# interface FastEthernet0/0
R2(config-if)# ip address 10.0.12.2 255.255.255.252
R2(config-if)# no shutdown
R2(config-if)# exit

! Configure WAN Link to R3
R2(config)# interface FastEthernet0/1
R2(config-if)# ip address 10.0.23.1 255.255.255.252
R2(config-if)# no shutdown
R2(config-if)# exit

! Configure Static Route to LAN 1 via R1 next-hop
R2(config)# ip route 192.168.10.0 255.255.255.0 10.0.12.1

! Configure Static Route to LAN 2 via R3 next-hop
R2(config)# ip route 192.168.20.0 255.255.255.0 10.0.23.2
R2(config)# end
R2# copy running-config startup-config
3. Router R3 ConfigurationPlaintextRouter> enable
Router# configure terminal
Router(config)# hostname R3

! Configure WAN Link to R2
R3(config)# interface FastEthernet0/0
R3(config-if)# ip address 10.0.23.2 255.255.255.252
R3(config-if)# no shutdown
R3(config-if)# exit

! Configure LAN 2 Interface
R3(config)# interface FastEthernet0/1
R3(config-if)# ip address 192.168.20.1 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# exit

! Configure Static Route to Remote LAN 1 via R2 next-hop
R3(config)# ip route 192.168.10.0 255.255.255.0 10.0.23.1
R3(config)# end
R3# copy running-config startup-config
4. End Host IP ConfigurationPC1: Configure IPv4 Address 192.168.10.10, Subnet Mask 255.255.255.0, and Default Gateway 192.168.10.1.PC2: Configure IPv4 Address 192.168.20.10, Subnet Mask 255.255.255.0, and Default Gateway 192.168.20.1.🔍 Verification1. Interface Operational StatusVerify that all configured interfaces are administratively enabled and operational.PlaintextR1# show ip interface brief
Expected Output:FastEthernet0/0 shows IP 192.168.10.1, Status: up, Protocol: up.FastEthernet0/1 shows IP 10.0.12.1, Status: up, Protocol: up.2. Routing Table InspectionVerify that statically configured routes have been successfully installed into the IPv4 routing table.PlaintextR1# show ip route
Expected Output:Directly connected networks appear with the C code (192.168.10.0/24 and 10.0.12.0/30).The static route appears with the S code pointing to the remote subnet:S    192.168.20.0/24 [1/0] via 10.0.12.2🧪 Connectivity TestingTest 1: End-to-End ICMP Connectivity (PC1 → PC2)Execute a ping from PC1 (192.168.10.10) across all three routers to PC2 (192.168.20.10).PlaintextC:\> ping 192.168.20.10
Expected Result:PlaintextPinging 192.168.20.10 with 32 bytes of data:
Reply from 192.168.20.10: bytes=32 time<1ms TTL=125
Reply from 192.168.20.10: bytes=32 time<1ms TTL=125
Reply from 192.168.20.10: bytes=32 time<1ms TTL=125
Reply from 192.168.20.10: bytes=32 time<1ms TTL=125

Ping statistics for 192.168.20.10:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
(Note: The initial packet may show a timeout while ARP resolution takes place across intermediate segments).Test 2: Hop-by-Hop Path Trace (PC1 → PC2)Execute a traceroute from PC1 to verify the sequential router hops taken by the packet.PlaintextC:\> tracert 192.168.20.10
Expected Result:PlaintextTracing route to 192.168.20.10 over a maximum of 30 hops:

  1   <1 ms   <1 ms   <1 ms   192.168.10.1  (R1 LAN Gateway)
  2   <1 ms   <1 ms   <1 ms   10.0.12.2     (R2 Ingress Interface)
  3   <1 ms   <1 ms   <1 ms   10.0.23.2     (R3 Ingress Interface)
  4   <1 ms   <1 ms   <1 ms   192.168.20.10 (PC2 Destination)

Trace complete.
🔄 Traffic FlowPlaintextPC1 (192.168.10.10)
   │
   ▼ [1. Encapsulates frame with R1 Fa0/0 MAC as destination]
R1 (Gateway: 192.168.10.1)
   │
   ▼ [2. Matches 192.168.20.0/24 in routing table; forwards to next-hop 10.0.12.2]
R2 (Intermediate: 10.0.12.2 / 10.0.23.1)
   │
   ▼ [3. Matches 192.168.20.0/24 in routing table; forwards to next-hop 10.0.23.2]
R3 (Gateway: 10.0.23.2 / 192.168.20.1)
   │
   ▼ [4. Identifies destination on directly connected interface Fa0/1; forwards to PC2]
PC2 (192.168.20.10)
Detailed Routing Decisions:Source Host Generation: PC1 inspects the destination IP (192.168.20.10), determines it is on a different subnet based on its local subnet mask (/24), and forwards the packet to its configured Default Gateway (192.168.10.1 on R1).Ingress Router R1: R1 de-encapsulates the frame and performs a longest-prefix match lookup for 192.168.20.10. It finds the static route 192.168.20.0/24 with next-hop 10.0.12.2. R1 encapsulates the packet into a new Layer 2 frame and transmits it out FastEthernet0/1.Transit Router R2: R2 receives the frame on Fa0/0, inspects the IP header, consults its routing table, finds the static route for 192.168.20.0/24 pointing to 10.0.23.2, re-encapsulates the frame, and transmits it via FastEthernet0/1.Egress Router R3: R3 receives the frame on Fa0/0, checks its routing table, and finds that 192.168.20.0/24 is directly connected on FastEthernet0/1. R3 resolves PC2's MAC address via ARP (if not cached) and delivers the packet directly to PC2.Return Path Processing: For the ICMP Echo Reply, the exact reverse process occurs, relying on the static routes configured on R3 (192.168.10.0/24 via 10.0.23.1) and R2 (192.168.10.0/24 via 10.0.12.1).📸 Verification ScreenshotsFile NameDescriptionProven Result01-topology.pngCisco Packet Tracer logical workspaceComplete 3-router topology with operational green link indicators02-ip-addressing.pngCLI output of show ip interface brief on R1Confirms interface IPv4 assignments and up/up operational state03-routing-table.pngCLI output of show ip route on R1Displays connected (C) and statically injected (S) routing entries04-connectivity-test.pngCommand prompt on PC1 executing ping to PC2Verifies 100% bidirectional ICMP packet delivery05-traceroute-test.pngCommand prompt on PC1 executing tracertValidates sequential packet traversal through intermediate hops📚 Concepts CoveredStatic Routing Architecture: Manual path definition with granular administrative control.Administrative Distance (AD): Static routes hold a default Administrative Distance of 1, making them preferred over dynamic protocols like OSPF (110) or RIP (120).Next-Hop vs. Exit Interface: Utilizing next-hop IP addresses (ip route <dest-net> <mask> <next-hop-ip>) to enforce correct Layer 2 frame rewrite and recursion resolution.Variable Length Subnet Masking (VLSM): Applying /30 subnets on point-to-point links to avoid address waste on non-broadcast transit segments.Symmetric vs. Asymmetric Routing: Ensuring reciprocal routes exist on every intermediate hop so return traffic is not dropped.🎓 Learning OutcomeConfigured and validated static route entries across multiple routing hops in Cisco IOS.Demonstrated understanding of how routers make independent forwarding decisions at each hop using their routing tables.Mastered point-to-point WAN link addressing and conservation using /30 subnet masks.Developed systematic troubleshooting skills by combining show ip route, ping, and tracert to pinpoint forwarding and return-path issues.🛠️ Software UsedCisco Packet Tracer (v8.x / v9.x)👨‍💻 AuthorMohamed AshikAspiring Network Engineer | CCNA (200-301) StudentGitHub: https://github.com/mohamedashik-cpu
