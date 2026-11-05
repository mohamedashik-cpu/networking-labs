# Enterprise Switching Network (Cisco Packet Tracer)
 
A multi-layer campus switching lab built in Cisco Packet Tracer 9.0.1: two Layer 3 core switches with HSRP gateways, three access switches, VLAN segmentation, trunking, an LACP EtherChannel between the cores, RSTP with a deliberate root and backup root, DHCP, port security and a routed uplink to an edge router.
 
All results below come from the live `02-Enterprise-Switching.pkt` project and the screenshots in this folder. Anything that was not tested is marked **Not tested**.
 
---
 
## Project Overview
 
The network has a core/access design. Each access switch connects to both core switches, the two cores are joined by a Port-channel, and an edge router attaches to each core over a routed point-to-point link. Six PCs, two per access switch, sit in three VLANs.
 
### Objectives
 
- Segment users into VLANs 10 (Sales), 20 (HR), 30 (IT) and 99 (Management)
- Trunk all switch-to-switch links with native VLAN 99 and an allowed-VLAN list
- Bundle the two core-to-core links into an LACP Port-channel
- Make CORE-SW1 the STP root and CORE-SW2 the backup root
- Provide redundant default gateways with HSRP
- Provide DHCP for each VLAN and secure the PC-facing access ports
- Connect the cores to an edge router with routed links and default routes
---
 
## Technologies Used
 
- Cisco Packet Tracer 9.0.1
- Cisco 3560-24PS Layer 3 switches (core), 2960-24TT switches (access), 2911 router, PC-PT hosts
- VLANs and 802.1Q trunking (native VLAN 99)
- LACP EtherChannel (Port-channel1)
- Rapid PVST+ (RSTP)
- HSRP version 2 with preempt
- Inter-VLAN routing with SVIs on the cores
- IOS DHCP pools on the cores
- Port security with sticky MAC learning
- Static routing
---
 
## Network Topology
 
![Network Topology](01-network-topology.png)
 
| Device | Model | Role |
|---|---|---|
| EDGE-R1 | 2911 | Edge router |
| CORE-SW1 | 3560-24PS | Core, STP root, HSRP active |
| CORE-SW2 | 3560-24PS | Core, STP backup root, HSRP standby |
| ACC-SW1 | 2960-24TT | Access for PC-A1, PC-A2 (VLAN 10) |
| ACC-SW2 | 2960-24TT | Access for PC-B1, PC-B2 (VLAN 20) |
| ACC-SW3 | 2960-24TT | Access for PC-C1, PC-C2 (VLAN 30) |
 
Links:
 
- CORE-SW1 and CORE-SW2: Fa0/23 and Fa0/24 on each core, bundled as Port-channel1
- Each core Fa0/1, Fa0/2 and Fa0/3 to the Gi0/1 (CORE-SW1) or Gi0/2 (CORE-SW2) uplink of ACC-SW1, ACC-SW2 and ACC-SW3
- EDGE-R1 Gi0/0 to CORE-SW1 Gi0/2, and EDGE-R1 Gi0/1 to CORE-SW2 Gi0/2
- Each PC to Fa0/1 or Fa0/2 of its access switch
---
 
## VLAN and IP Plan
 
| VLAN | Name | Subnet | HSRP gateway | CORE-SW1 SVI | CORE-SW2 SVI |
|---|---|---|---|---|---|
| 10 | Sales | 192.168.10.0/24 | 192.168.10.1 | 192.168.10.2 | 192.168.10.3 |
| 20 | HR | 192.168.20.0/24 | 192.168.20.1 | 192.168.20.2 | 192.168.20.3 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 | 192.168.30.2 | 192.168.30.3 |
| 99 | Management | none | none | none | none |
 
VLAN 99 exists and is the trunk native VLAN. It has no SVI or member ports, so no device has a management IP.
 
Routed links:
 
| Link | Subnet | EDGE-R1 | Core |
|---|---|---|---|
| EDGE-R1 Gi0/0 to CORE-SW1 Gi0/2 | 10.0.0.0/30 | 10.0.0.1 | 10.0.0.2 |
| EDGE-R1 Gi0/1 to CORE-SW2 Gi0/2 | 10.0.0.4/30 | 10.0.0.5 | 10.0.0.6 |
 
PC addresses are assigned by DHCP and have changed during testing. The addresses at the time of capture:
 
| PC | VLAN | Address |
|---|---|---|
| PC-A1 | 10 | 192.168.10.128 |
| PC-A2 | 10 | 192.168.10.11 |
| PC-B1 | 20 | 192.168.20.10 |
| PC-B2 | 20 | 192.168.20.11 |
| PC-C1 | 30 | 192.168.30.10 |
| PC-C2 | 30 | 192.168.30.11 |
 
---
 
## Implementation
 
### 1. VLANs
 
VLANs 10, 20, 30 and 99 exist on all five switches.
 
![VLAN Configuration, CORE-SW1](03-vlan-configuration.png)
![VLAN Configuration, CORE-SW2](03b-vlan-configuration-core-sw2.png)
 
### 2. Access Port Assignments
 
Fa0/1 and Fa0/2 are access ports in VLAN 10 on ACC-SW1, VLAN 20 on ACC-SW2 and VLAN 30 on ACC-SW3.
 
![Access Ports, ACC-SW1](04-access-port-assignments.png)
![Access Ports, ACC-SW2](04b-access-port-assignments-acc-sw2.png)
![Access Ports, ACC-SW3](04c-access-port-assignments-acc-sw3.png)
 
### 3. Trunking
 
Switch-to-switch links trunk 802.1Q with native VLAN 99 and VLANs 10, 20, 30 and 99 allowed. ACC-SW1 forwards on Gi0/1 only; Gi0/2 forwards no VLANs because it is the blocked redundant uplink. ACC-SW2 and ACC-SW3 trunks were not captured.
 
![Trunks, CORE-SW1](05-trunk-configuration.png)
![Trunks, CORE-SW2](05b-trunk-configuration-core-sw2.png)
![Trunks, ACC-SW1](05c-trunk-configuration-acc-sw1.png)
 
### 4. Spanning Tree
 
CORE-SW1 is the root bridge for VLANs 10, 20, 30 and 99 (priority 4096 plus the VLAN ID). CORE-SW2 is configured with priority 8192 and reaches the root through Port-channel1.
 
![STP Root, VLANs 10 and 20](06-spanning-tree-root.png)
![STP Root, VLANs 30 and 99](06b-spanning-tree-root-vlan30-99.png)
![STP, CORE-SW2](06c-spanning-tree-core-sw2-vlan10-99.png)
 
### 5. EtherChannel
 
Port-channel1 is bundled with LACP on both cores. Fa0/23 and Fa0/24 both show as bundled (P).
 
![EtherChannel, CORE-SW1](07-etherchannel-summary.png)
![EtherChannel, CORE-SW2](07b-etherchannel-summary-core-sw2.png)
![Port-channel1 Status](08-core-to-core-trunk.png)
 
### 6. HSRP
 
CORE-SW1 (priority 110) is Active and CORE-SW2 (priority 100) is Standby for groups 10, 20 and 30. The `show standby brief` output covers all three groups, so one screenshot per core is used for all three VLANs.
 
![HSRP, CORE-SW1](09-hsrp-vlan10.png)
![HSRP, CORE-SW2](09b-hsrp-core-sw2.png)
 
### 7. Routed Uplinks and Routing
 
EDGE-R1 Gi0/0 and Gi0/1 are up with the addresses above, and Gi0/2 on each core is a routed port. Each core has a default route to EDGE-R1. EDGE-R1 has a static `192.168.0.0/16` route through both cores and no default route.
 
![Router Interfaces](12-router-interface-status.png)
![Core Routed Links, CORE-SW1](13-core-routed-links.png)
![Core Routed Links, CORE-SW2](13b-core-routed-links-core-sw2.png)
![Routing Table, EDGE-R1](14-routing-tables.png)
![Routing Table, CORE-SW1](14b-routing-table-core-sw1.png)
![Routing Table, CORE-SW2](14c-routing-table-core-sw2.png)
 
### 8. DHCP
 
Pools SALES, HR and IT run on the cores with the HSRP address as the default router. The screenshot shows the lease bindings for the six PCs.
 
![DHCP Leases](15-dhcp-configuration.png)
 
### 9. PC Configuration
 
![PC-A1](16-pc-ip-configuration.png)
![PC-A2](16b-pc-ip-configuration-pc-a2.png)
![PC-B1](16c-pc-ip-configuration-pc-b1.png)
![PC-B2](16d-pc-ip-configuration-pc-b2.png)
![PC-C1](16e-pc-ip-configuration-pc-c1.png)
![PC-C2](16f-pc-ip-configuration-pc-c2.png)
 
### 10. Port Security
 
Fa0/1 and Fa0/2 on each access switch use sticky MAC learning, a maximum of 1 address and violation mode shutdown. Each port shows 1 secure MAC and 0 violations.
 
![Port Security, ACC-SW1](17-port-security.png)
![Port Security, ACC-SW2](17b-port-security-acc-sw2.png)
![Port Security, ACC-SW3](17c-port-security-acc-sw3.png)
 
---
 
## Verification Results
 
| Test | Result |
|---|---|
| VLANs 10, 20, 30, 99 present on all switches | Pass (03, 03b, 04 series) |
| Access ports in correct VLANs | Pass (04 series) |
| Trunks with native VLAN 99, VLANs 10/20/30/99 allowed | Pass on both cores and ACC-SW1; ACC-SW2 and ACC-SW3 not captured |
| CORE-SW1 STP root for VLANs 10, 20, 30, 99 | Pass (06, 06b) |
| Port-channel1 LACP, both members bundled | Pass (07, 07b, 08) |
| HSRP: CORE-SW1 Active, CORE-SW2 Standby, groups 10/20/30 | Pass (09, 09b) |
| HSRP failover when CORE-SW1 fails | Not tested |
| EDGE-R1 Gi0/0 and Gi0/1 up with IPs | Pass (12) |
| Core routed links and default routes | Pass (13, 14 series) |
| DHCP leases for all six PCs | Pass (15, 16 series) |
| Port security: 1 of 1 secure MAC, 0 violations | Pass (17 series) |
| Port security violation behaviour | Not tested |
| PC to its HSRP gateway, second run | Pass, 4/4 for all six PCs |
| PC to EDGE-R1 10.0.0.1, first run | Partial, 1 packet lost per PC (PC-A1 lost 2) |
| PC to EDGE-R1 10.0.0.1, second run | 5 of 6 PCs 4/4; PC-A1 3/4 (shown in 18); a third PC-A1 run was 4/4 |
| Cross-VLAN PC to PC pings | Not re-run for this evidence set |
 
The first ping to EDGE-R1 loses a packet on every PC even with ARP warmed on the gateway path. The cause was not determined.
 
![Connectivity Tests](18-connectivity-tests.png)
 
### Final Interface Status
 
![ACC-SW1](19-final-interface-status.png)
![ACC-SW2](19b-final-interface-status-acc-sw2.png)
![ACC-SW3](19c-final-interface-status-acc-sw3.png)
![CORE-SW1](19d-final-interface-status-core-sw1.png)
![CORE-SW2](19e-final-interface-status-core-sw2.png)
![EDGE-R1](19f-final-interface-status-edge-r1.png)
 
### Final Topology
 
![Final Topology](20-final-topology.png)
 
---
 
## Known Limitations
 
- Gi0/1 on both cores is down and unused (left from an earlier core-to-core link).
- EDGE-R1 Gi0/2 and Vlan1 are administratively down; the router has no default route or upstream link.
- Unused access ports (Fa0/3 to Fa0/24) remain in VLAN 1 without port security.
- VLAN 99 has no SVI, so no switch has a management IP, and all devices keep default IOS hostnames.
- DHCP lease addresses changed between captures.
---
 
## Project Evidence
 
`01-network-topology.png`, `03-vlan-configuration.png`, `03b-vlan-configuration-core-sw2.png`, `04-access-port-assignments.png`, `04b-access-port-assignments-acc-sw2.png`, `04c-access-port-assignments-acc-sw3.png`, `05-trunk-configuration.png`, `05b-trunk-configuration-core-sw2.png`, `05c-trunk-configuration-acc-sw1.png`, `06-spanning-tree-root.png`, `06b-spanning-tree-root-vlan30-99.png`, `06c-spanning-tree-core-sw2-vlan10-99.png`, `07-etherchannel-summary.png`, `07b-etherchannel-summary-core-sw2.png`, `08-core-to-core-trunk.png`, `09-hsrp-vlan10.png`, `09b-hsrp-core-sw2.png`, `12-router-interface-status.png`, `13-core-routed-links.png`, `13b-core-routed-links-core-sw2.png`, `14-routing-tables.png`, `14b-routing-table-core-sw1.png`, `14c-routing-table-core-sw2.png`, `15-dhcp-configuration.png`, `16-pc-ip-configuration.png` and `16b` to `16f`, `17-port-security.png`, `17b-port-security-acc-sw2.png`, `17c-port-security-acc-sw3.png`, `18-connectivity-tests.png`, `19-final-interface-status.png` and `19b` to `19f`, `20-final-topology.png`.
 
---
 
## Project Files
 
- `02-Enterprise-Switching.pkt` is the Packet Tracer 9.0.1 project. Open it in Packet Tracer 9.0.1 or later.
- `README.md` is this document, and the numbered PNG files above are the screenshots.
---
 
## Skills Demonstrated
 
- Designing a two-tier campus switching network with redundant uplinks
- VLAN segmentation, 802.1Q trunking and native VLAN hardening
- LACP EtherChannel, RSTP root placement and HSRP gateway redundancy
- Inter-VLAN routing with SVIs and routed uplinks with static routes
- DHCP services, sticky port security and verification with show commands and pings
## Conclusion
 
The network implements VLANs, trunks, an LACP core link, STP root and backup root, HSRP gateways, DHCP, port security and routed edge uplinks, and the captured evidence supports each of those. HSRP failover and violation handling were not tested, and a one-packet loss on the first ping to EDGE-R1 remains unexplained.
 
