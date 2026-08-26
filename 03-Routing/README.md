# 🌐 Routing Labs

## 📖 Overview
This section contains hands-on **Cisco Routing labs** created using Cisco Packet Tracer. The labs progress from basic static routing concepts to dynamic routing protocols and advanced **route redistribution**.

The same practical lab approach is used throughout the section: build the topology, configure IP addressing, configure routing, verify routing tables and neighbors, test connectivity, and document the results with screenshots and Packet Tracer files.

## 🎯 Objectives
- Understand how routers forward IPv4 traffic between different networks.
- Configure and verify static and default routes.
- Configure dynamic routing protocols.
- Understand routing metrics and administrative distance.
- Verify routing tables and routing protocol neighbors.
- Compare RIP, OSPF, and EIGRP.
- Understand OSPF areas and multi-area routing.
- Understand route redistribution between different routing protocols.
- Troubleshoot routing and end-to-end connectivity.

## 🧪 Lab Index

| No. | Lab | Main Concepts | Status |
|---:|---|---|---|
| 01 | [Static Routing](01-Static-Routing/) | Static routes, next-hop routing | ✅ Complete |
| 02 | [Default Routing](02-Default-Routing/) | Default route, gateway of last resort | ✅ Complete |
| 03 | [RIP Version 1](03-RIP-Version-1/) | Distance vector, hop count, classful routing | ✅ Complete |
| 04 | [RIP Version 2](04-RIP-Version-2/) | Classless routing, VLSM, CIDR | ✅ Complete |
| 05 | [OSPF Single Area](05-OSPF-Single-Area/) | Link state, SPF, cost, Area 0 | ✅ Complete |
| 06 | [OSPF Multi-Area](06-OSPF-Multi-Area/) | OSPF areas, ABR, Area 0 | ✅ Complete |
| 07 | [EIGRP](07-EIGRP/) | DUAL, successor, feasible successor | ✅ Complete |
| 08 | [Route Redistribution](08-Route-Redistribution/) | OSPF ↔ EIGRP redistribution | ✅ Complete |

## 🗺️ Routing Learning Path

```text
Static Routing
      ↓
Default Routing
      ↓
RIP Version 1
      ↓
RIP Version 2
      ↓
OSPF Single Area
      ↓
OSPF Multi-Area
      ↓
EIGRP
      ↓
OSPF ↔ EIGRP Route Redistribution
```

## 📚 Topics Covered

### Static & Basic Routing
- Static routes
- Next-hop routes
- Exit-interface routes
- Default routes
- Gateway of last resort

### RIP
- RIP Version 1
- RIP Version 2
- Distance-vector routing
- Hop-count metric
- Maximum hop count
- Classful vs classless routing
- VLSM and CIDR support
- `network` command
- `no auto-summary`

### OSPF
- Link-state routing
- OSPF process ID
- Router ID
- OSPF neighbor adjacency
- SPF algorithm
- OSPF cost
- Area 0
- OSPF single-area design
- OSPF multi-area design
- Area Border Router (ABR)
- OSPF routing table verification

### EIGRP
- EIGRP Autonomous System
- Neighbor adjacency
- DUAL algorithm
- Successor
- Feasible successor
- Feasibility condition
- Composite metric
- Bandwidth and delay
- EIGRP topology table
- EIGRP route verification

### Route Redistribution
- Routing-domain boundaries
- OSPF → EIGRP redistribution
- EIGRP → OSPF redistribution
- Redistribution metrics
- External routes
- Redistribution verification
- Routing-loop considerations

## 🔧 Common Verification Commands

### Interface Verification
```text
show ip interface brief
```

### Routing Table
```text
show ip route
```

### Static / Default Routes
```text
show ip route static
show ip route 0.0.0.0
```

### RIP
```text
show ip protocols
show ip route rip
```

### OSPF
```text
show ip ospf neighbor
show ip ospf interface
show ip route ospf
show ip ospf database
```

### EIGRP
```text
show ip eigrp neighbors
show ip eigrp topology
show ip route eigrp
```

### Connectivity
```text
ping <destination-ip>
tracert <destination-ip>
```

## 🔄 Routing Protocol Comparison

| Feature | RIP v1 | RIP v2 | OSPF | EIGRP |
|---|---|---|---|---|
| Type | Distance Vector | Distance Vector | Link State | Advanced Distance Vector |
| Metric | Hop Count | Hop Count | Cost | Composite Metric |
| Algorithm | Bellman-Ford based | Bellman-Ford based | SPF / Dijkstra | DUAL |
| VLSM/CIDR | ❌ | ✅ | ✅ | ✅ |
| Scalability | Low | Low | High | High |
| Convergence | Slow | Slow | Fast | Fast |
| Route Code | `R` | `R` | `O` | `D` |
| Default AD | 120 | 120 | 110 | 90 Internal |

## 🧠 Key Concepts Learned
- Routing table selection
- Longest prefix match
- Administrative distance
- Routing metrics
- Static vs dynamic routing
- Distance-vector routing
- Link-state routing
- Dynamic neighbor formation
- SPF and DUAL algorithms
- OSPF areas and ABRs
- EIGRP successor and feasible successor
- Route redistribution
- End-to-end packet forwarding
- Ping and traceroute troubleshooting

## 🧪 Standard Lab Workflow

Each routing lab follows this workflow:

```text
1. Build Topology
       ↓
2. Configure IP Addresses
       ↓
3. Configure Routing Protocol
       ↓
4. Verify Interfaces
       ↓
5. Verify Neighbors / Routes
       ↓
6. Test Ping
       ↓
7. Test Traceroute
       ↓
8. Capture Screenshots
       ↓
9. Save .pkt File
       ↓
10. Document in README.md
```

## 📸 Documentation Standard

Each completed lab includes, where applicable:
- Network topology screenshot
- IP addressing screenshot
- Routing configuration screenshot
- Neighbor verification screenshot
- Routing table screenshot
- Connectivity test screenshot
- Traceroute screenshot
- Cisco Packet Tracer `.pkt` file
- Detailed `README.md`

## 💻 Software Used
- Cisco Packet Tracer
- PuTTY / SSH / Console CLI for command-line practice
- GitHub for lab documentation and version control

## 🎓 Learning Outcome
By completing these labs, I developed practical experience in configuring, verifying, and troubleshooting IPv4 routing on Cisco routers. The progression covers basic static routing, dynamic routing protocols, OSPF area design, EIGRP, and interoperability through route redistribution.

## 👨‍💻 Author
**Mohamed Ashik M**  
Aspiring Network Engineer | CCNA (200-301) Student  
GitHub: [mohamedashik-cpu](https://github.com/mohamedashik-cpu)

---

⭐ **This routing section is part of my hands-on Cisco networking lab portfolio.**
