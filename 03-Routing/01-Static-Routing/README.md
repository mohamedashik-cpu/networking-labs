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
````
**Expected Result:**  
The trace should hit three routing hops before the destination:
1. `192.168.10.1` (R1 Gateway)
2. `10.0.12.2` (R2 Ingress)
3. `10.0.23.2` (R3 Ingress)
4. `192.168.20.10` (PC2 Destination)

## 🔄 Traffic Flow
When PC1 pings PC2, the following sequence occurs:
1. **PC1** compares the destination IP (`192.168.20.10`) to its own subnet. Since it is remote, PC1 forwards the frame to its Default Gateway (**R1**).
2. **R1** receives the packet, checks its routing table, and finds a static route (`S`) for `192.168.20.0/24` pointing to the next-hop `10.0.12.2`. It forwards the packet out `Fa0/1`.
3. **R2** receives the packet, checks its routing table, and matches the static route for `192.168.20.0/24` pointing to next-hop `10.0.23.2`. It forwards the packet out `Fa0/1`.
4. **R3** receives the packet. Its routing table shows that `192.168.20.0/24` is a directly connected network (`C`) on `Fa0/1`. R3 sends an ARP broadcast on LAN 2 (if not already cached) to find PC2's MAC address and delivers the packet directly to **PC2**.
5. The return traffic (ICMP Echo Reply) follows the exact reverse path, relying on the static routes configured for the `192.168.10.0/24` network on R3 and R2.

## 📸 Verification Screenshots
1. `![Network Topology](01-topology.png)` → Complete Packet Tracer topology.
2. `![IP Addressing](02-ip-addressing.png)` → `show ip interface brief` proving interface status.
3. `![Routing Table](03-routing-table.png)` → `show ip route` highlighting the static `S` routes.
4. `![Connectivity Test](04-connectivity-test.png)` → Successful end-to-end ping from PC1 to PC2.
5. `![Traceroute Test](05-traceroute-test.png)` → Successful traceroute showing path traversal.

## 📚 Concepts Covered
* **Static Routing:** Manually defining paths to remote networks.
* **Next-Hop Addressing:** Using the IP address of the adjacent router to direct traffic.
* **Administrative Distance (AD):** Static routes have an AD of 1, making them highly trusted by the router.
* **Point-to-Point WAN Subnetting:** Utilizing `/30` subnet masks to conserve IP addresses on links connecting only two routers.
* **Bidirectional Routing:** Understanding that routers require explicit routes for both the forward path *and* the return path to establish successful communication.

## 🎓 Learning Outcome
By completing this lab, I practically demonstrated the ability to configure static IPv4 routes using next-hop IP addresses across a multi-router topology. I successfully verified the routing tables, ensured bidirectional traffic flow, and validated end-to-end connectivity using standard Cisco IOS verification and troubleshooting commands. 

## 💡 Interview Questions

1. **What is Static Routing?**
   Static routing is a method where a network administrator manually configures routes in a router's routing table to specify exactly how packets should reach a destination network.

2. **What is the default Administrative Distance (AD) of a static route?**
   The default Administrative Distance for a static route using a next-hop IP or exit interface is 1.

3. **What is the difference between specifying a next-hop IP versus an exit interface?**
   A next-hop IP tells the router the exact IP of the neighboring device to send traffic to (requiring a recursive lookup). An exit interface simply pushes the traffic out a specific port, assuming the destination is directly attached (which can cause ARP issues on broadcast multi-access networks like Ethernet).

4. **Why did we use a `/30` subnet mask on the WAN links?**
   A `/30` mask (`255.255.255.252`) provides exactly two usable host IP addresses. This is the most efficient use of IP space for a point-to-point link between two routers, preventing wasted addresses.

5. **If PC1 can ping R2, but cannot ping PC2, what is the most likely routing issue?**
   Either R2 is missing a route to LAN 2 (`192.168.20.0/24`), or R3 is missing a return route to LAN 1 (`192.168.10.0/24`). Traffic must know how to get there *and* how to get back.

6. **How do you verify if a static route is correctly installed?**
   Use the `show ip route` command. If the route is active and the next-hop interface is "up," it will appear in the routing table marked with the letter `S`.

7. **What happens to a static route if the interface connecting to the next-hop goes down?**
   The router will automatically remove the static route from the routing table because the next-hop IP is no longer reachable.

8. **What is a Floating Static Route?**
   A floating static route is a backup route configured with a manually raised Administrative Distance (e.g., higher than a primary dynamic route's AD). It only enters the routing table if the primary route fails.

9. **In the command `ip route 192.168.20.0 255.255.255.0 10.0.12.2`, what does the last IP address represent?**
   `10.0.12.2` represents the Next-Hop IP address (the IP address of the adjacent router's receiving interface).

10. **Why is dynamic routing preferred over static routing in large enterprise networks?**
    Static routing lacks scalability and automatic failover. In a large network, manually updating routes for every topology change is prone to human error and administrative overhead. Dynamic protocols (like OSPF or EIGRP) adapt to changes and calculate paths automatically.

## 💻 Software Used
* Cisco Packet Tracer

## 👨‍💻 Author
**Mohamed Ashik**  
Aspiring Network Engineer | CCNA (200-301) Student  
GitHub: [https://github.com/mohamedashik-cpu](https://github.com/mohamedashik-cpu)
