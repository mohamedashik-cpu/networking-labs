# 01 - Static Routing

## 📖 Overview
This lab demonstrates the fundamental configuration, operation, and verification of **IPv4 Static Routing** in a multi-router Cisco network environment. Static routing involves manually configuring routes in the router's routing table to reach destination networks that are not directly connected. 

In this lab, three Cisco routers (**R1**, **R2**, and **R3**) interconnect two isolated Local Area Networks (**LAN 1** and **LAN 2**). Point-to-point WAN links utilize power-efficient `/30` subnets, and manual static routes are defined using next-hop IP addresses to establish complete end-to-end bidirectional communication between end devices.

## 🎯 Objectives
* Design and implement a multi-router topology connecting distinct subnets.
* Apply efficient IPv4 addressing, using `/24` subnets for LANs and `/30` subnets for point-to-point WAN links.
* Configure static routes on edge and intermediate routers using the next-hop IPv4 addressing method.
* Verify interface operational states (`up/up`) and inspect routing tables for static route entries (`S`).
* Validate end-to-end ICMP reachability (`ping`) and trace hop-by-hop packet forwarding paths (`tracert`).
* Analyze the forwarding decisions made by routers at each step of the transit path.

## 🖥️ Devices Used

| Device Type | Device Model | Quantity | Role / Description |
|---|---|---|---|
| **Router** | Cisco 2911 / 1941 | 3 | Core & Edge Routing Devices (R1, R2, R3) |
| **End Device** | Generic PC | 2 | End Hosts for LAN 1 (PC1) and LAN 2 (PC2) |
| **Cabling** | Copper Straight-Through / Cross-Over | 4 | Physical media connections for LAN & WAN links |

## 🌐 Network Topology
![Network Topology](01-topology.png)

## 🔌 Port Mapping

| Source Device | Source Interface | Connected Device | Connected Interface | Link Type |
|---|---|---|---|---|
| **PC1** | FastEthernet0 | **R1** | FastEthernet0/0 | Local LAN Link (LAN 1) |
| **R1** | FastEthernet0/1 | **R2** | FastEthernet0/0 | Point-to-Point WAN Link |
| **R2** | FastEthernet0/1 | **R3** | FastEthernet0/0 | Point-to-Point WAN Link |
| **R3** | FastEthernet0/1 | **PC2** | FastEthernet0 | Local LAN Link (LAN 2) |

## 🌐 IP Addressing

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

## ⚙️ Configuration

### 1. Configure R1
Configure LAN/WAN interfaces and a static route to LAN 2 (`192.168.20.0/24`) via R2's next-hop IP.

````text
enable
configure terminal
hostname R1

interface FastEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

interface FastEthernet0/1
 ip address 10.0.12.1 255.255.255.252
 no shutdown
 exit

ip route 192.168.20.0 255.255.255.0 10.0.12.2
end
copy running-config startup-config
````

### 2. Configure R2
Configure WAN interfaces and static routes to both LAN 1 (`192.168.10.0/24`) and LAN 2 (`192.168.20.0/24`).

````text
enable
configure terminal
hostname R2

interface FastEthernet0/0
 ip address 10.0.12.2 255.255.255.252
 no shutdown
 exit

interface FastEthernet0/1
 ip address 10.0.23.1 255.255.255.252
 no shutdown
 exit

ip route 192.168.10.0 255.255.255.0 10.0.12.1
ip route 192.168.20.0 255.255.255.0 10.0.23.2
end
copy running-config startup-config
````

### 3. Configure R3
Configure LAN/WAN interfaces and a static route to LAN 1 (`192.168.10.0/24`) via R2's next-hop IP.

````text
enable
configure terminal
hostname R3

interface FastEthernet0/0
 ip address 10.0.23.2 255.255.255.252
 no shutdown
 exit

interface FastEthernet0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown
 exit

ip route 192.168.10.0 255.255.255.0 10.0.23.1
end
copy running-config startup-config
````

## 🔍 Verification

### Verify Interface Status
This command verifies that the physical interfaces are up and assigned the correct IP addresses.
````text
show ip interface brief
````
**Expected Result:**  
Both `FastEthernet0/0` and `FastEthernet0/1` should show `Status: up` and `Protocol: up`.

### Verify Routing Table
This command verifies that the static routes have been successfully injected into the router's routing table.
````text
show ip route
````
**Expected Result:**  
You should see directly connected networks marked with a `C` (or `L` for local IPs) and the static route marked with an `S`. Example for R1:
`S 192.168.20.0/24 [1/0] via 10.0.12.2`

## 🧪 Connectivity Testing

### PC1 → PC2
Test complete end-to-end connectivity across the network.
````text
ping 192.168.20.10
````
**Expected Result:**  
Successful. The first packet may drop (Request timed out) due to ARP resolution across the routers, but subsequent packets should receive a reply.

### PC1 → PC2 Path Trace
Verify the exact path the packets take to reach the destination.
````text
tracert 192.168.20.10
