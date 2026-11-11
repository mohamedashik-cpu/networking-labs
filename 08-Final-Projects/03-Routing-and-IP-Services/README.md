# Routing and IP Services (Cisco Packet Tracer)

A multi-router HQ-and-branches lab built in Cisco Packet Tracer 9.0.1. It demonstrates static and default routing, centralized DHCP with DHCP relay, internal DNS, HTTP service on Server-PT, and NAT/PAT working together.

Results below are based on the reported tests of `03-Routing-and-IP-Services.pkt`. Anything not tested or not independently confirmed is marked accordingly.

---

## Project Overview

HQ has a router, a LAN switch, two client PCs and a Server-PT that provides DHCP, DNS and HTTP. Two branch routers each have an access switch and two PCs. The branch routers connect to HQ over routed point-to-point links. HQ translates internal addresses with NAT/PAT toward a **simulated external test network** that contains one external test host.

`203.0.113.0/30` is a simulated external test network. It is not the Internet, and this lab has no Internet access. `www.external.test` is a lab-only hostname.

### Objectives

- Connect HQ and two branches with routed point-to-point WAN links
- Route between sites with static routes and branch default routes
- Provide centralized DHCP from HQ-SRV, with DHCP relay on the branch routers
- Provide internal DNS from HQ-SRV
- Serve web pages with the Packet Tracer Server-PT HTTP service
- Translate internal addresses with NAT/PAT overload toward the external test host

---

## Technologies Used

- Cisco Packet Tracer 9.0.1
- 2911 routers, 2960-24TT switches, PC-PT and Server-PT
- Static routing and default routing
- Router-on-a-stick (802.1Q sub-interfaces) on HQ-R1 Gi0/0
- Server-PT DHCP with `ip helper-address` relay
- Server-PT DNS and HTTP services (Packet Tracer's web server; not Microsoft IIS)
- NAT/PAT with an ACL and `overload`

---

## Network Topology

![Routing and IP Services topology](01-routing-topology.png)

| Device | Model | Role |
|---|---|---|
| HQ-R1 | 2911 | HQ router and NAT/PAT |
| BR1-R1, BR2-R1 | 2911 | Branch routers |
| HQ-SW1 | 2960-24TT | HQ switch (VLANs 10 and 100) |
| BR1-SW1, BR2-SW1 | 2960-24TT | Branch access switches (VLAN 1) |
| HQ-SRV | Server-PT | DHCP, DNS and HTTP |
| EXT-SRV | Server-PT | Simulated external test host (HTTP) |
| HQ-PC1, HQ-PC2, BR1-PC1, BR1-PC2, BR2-PC1, BR2-PC2 | PC-PT | Clients |

HQ-R1 uses its Gi0/0 port for two VLAN sub-interfaces: VLAN 10 for the HQ LAN and VLAN 100 for the external test network. No hardware module was used.

---

## Addressing Plan

| Network | Subnet | Addresses |
|---|---|---|
| HQ LAN (VLAN 10) | 10.10.1.0/24 | HQ-R1 Gi0/0.10 = 10.10.1.1; HQ-SRV = 10.10.1.10 (static) |
| Branch 1 LAN | 10.20.1.0/24 | BR1-R1 Gi0/0 = 10.20.1.1 |
| Branch 2 LAN | 10.30.1.0/24 | BR2-R1 Gi0/0 = 10.30.1.1 |
| WAN HQ to BR1 | 172.16.1.0/30 | HQ-R1 Gi0/1 = .1; BR1-R1 Gi0/1 = .2 |
| WAN HQ to BR2 | 172.16.2.0/30 | HQ-R1 Gi0/2 = .1; BR2-R1 Gi0/1 = .2 |
| External test (VLAN 100) | 203.0.113.0/30 | HQ-R1 Gi0/0.100 = .1 (NAT address); EXT-SRV = .2 (static) |

Each PC receives its address by DHCP. The scopes cover 10.10.1.100–10.10.1.199, 10.20.1.100–10.20.1.199 and 10.30.1.100–10.30.1.199.

---

## Implementation

### 1. HQ Switch VLANs

HQ-SW1 has VLAN 10 (HQ-LAN) and VLAN 100 (EXT-TEST). Fa0/1 is a trunk to HQ-R1 carrying both VLANs. Fa0/2–Fa0/4 are in VLAN 10, and Fa0/10 (EXT-SRV) is in VLAN 100. The branch switches use VLAN 1; their configurations were not captured.

![HQ switch VLANs and trunk](02-hq-switch-vlans-trunk.png)

### 2. Router Interfaces and Routing

HQ-R1 has static routes to 10.20.1.0/24 via 172.16.1.2 and 10.30.1.0/24 via 172.16.2.2. Each branch has a default route to HQ (172.16.1.1 or 172.16.2.1). HQ-R1 has no default route because EXT-SRV is directly connected.

![HQ-R1 interfaces](03-hq-r1-interfaces.png)
![HQ-R1 routing table](04-hq-r1-routing-table.png)
![BR1-R1 routing and relay](05-br1-r1-routing-and-relay.png)
![BR2-R1 routing and relay](05b-br2-r1-routing-and-relay.png)

### 3. DHCP and DHCP Relay

HQ-SRV serves three pools. The HQ pool is Packet Tracer's built-in `serverPool`, edited to the HQ settings; the other pools are `BR1-LAN` and `BR2-LAN`. Each pool provides its LAN's gateway and DNS server 10.10.1.10. BR1-R1 and BR2-R1 use `ip helper-address 10.10.1.10` on their LAN interfaces.

![HQ-SRV DHCP pools](10-hq-srv-dhcp-pools.png)
![DHCP client HQ-PC1](08-dhcp-client-hq-pc1.png)
![DHCP client BR1-PC1](08b-dhcp-client-br1-pc1.png)
![DHCP client BR2-PC1](08c-dhcp-client-br2-pc1.png)
![HQ-PC2 address](15-remaining-pc-addresses.png)
![BR1-PC2 address](15b-br1-pc2-address.png)
![BR2-PC2 address](15c-br2-pc2-address.png)

### 4. NAT/PAT

On HQ-R1, Gi0/0.10, Gi0/1 and Gi0/2 are NAT inside; Gi0/0.100 is NAT outside. ACL 1 permits the three LAN subnets, and `ip nat inside source list 1 interface GigabitEthernet0/0.100 overload` translates them to 203.0.113.1. Tested site-to-site traffic was not translated.

![NAT configuration](06-hq-r1-nat-config.png)
![NAT translations and statistics](07-hq-r1-nat-translations.png)

### 5. DNS and HTTP Services

HQ-SRV has DNS records `web.corp.local` → 10.10.1.10, `hq-r1.corp.local` → 10.10.1.1 and `www.external.test` → 203.0.113.2. HQ-SRV and EXT-SRV serve custom pages through the Server-PT HTTP service.

![HQ-SRV DNS records](13-hq-srv-dns-and-http.png)
![HQ-SRV HTTP service](13b-hq-srv-http-service.png)
![HQ-SRV IP configuration](14-server-ip-configurations.png)
![EXT-SRV IP configuration](14b-ext-srv-ip-configuration.png)

---

## Verification Results

| Test | Result |
|---|---|
| WAN links, interfaces and routes | Pass |
| DHCP on all three LANs, including relay at both branches | Pass |
| DNS for `web.corp.local` and `www.external.test` from one PC per LAN | Pass |
| HTTP by name and IP to HQ-SRV and EXT-SRV from one PC per LAN | Pass, with first-request delay caveat below |
| NAT/PAT from all three LANs to 203.0.113.1 | Pass |
| Site-to-site traffic not translated | Pass |
| HQ-PC1 pings to both branch PCs and EXT-SRV | Pass |
| Branch-to-HQ PC pings | Not independently confirmed from the available test report |
| Health check: no duplicate IPs | Pass |
| Translation timeout/expiry | Not tested |
| DNS and HTTP from HQ-PC2, BR1-PC2 and BR2-PC2 | Not tested |
| WAN link failure behaviour | Not tested |

![DNS tests, HQ-PC1](11-dns-tests.png)
![DNS tests, BR1-PC1](11b-dns-tests-br1-pc1.png)
![DNS tests, BR2-PC1](11c-dns-tests-br2-pc1.png)
![HTTP tests, HQ-PC1](12-http-access-tests.png)
![HTTP tests, BR1-PC1](12b-http-access-tests-br1-pc1.png)
![HTTP tests, BR2-PC1](12c-http-access-tests-br2-pc1.png)
![HQ custom HTTP page](12d-hq-custom-http-page.png)
![Connectivity tests, HQ-PC1](09-connectivity-tests-hq-pc1.png)
![Branch connectivity, BR1-PC1](16-branch-connectivity-tests.png)
![Branch connectivity, BR2-PC1](16b-br2-branch-connectivity-tests.png)

---

## Limitations and Unresolved Items

- **Branch HTTP first request:** A first HTTP request from a branch PC sometimes timed out after 30 seconds and passed on retry. This was reported for BR1-PC1 accessing HQ-SRV and BR2-PC1 accessing EXT-SRV. The cause is unknown.
- **DHCP leases:** Client lease addresses shifted between reads. No duplicate IPs were reported; the exact cause is unknown, so addresses should be treated as values observed at capture time.
- **Factory `serverPool`:** Packet Tracer recreated its default DHCP pool after deletion, and it issued leases without a usable gateway. The HQ scope therefore uses the edited built-in `serverPool`.
- **HQ-SRV page:** The first custom page write was not served and the default Packet Tracer page appeared. A later write worked; the cause of the first failure is unknown.
- **NAT counters:** NAT hit and miss counters differed between reads; the difference was not explained.
- **First-packet loss:** Some first pings (including a ping to 203.0.113.2) lost one packet. The cause was not confirmed.
- HQ-R1 Gi0/0 has no IP by design because its sub-interfaces carry the addresses. Some unused interfaces are shut down.
- This is a simulated external test network, not the Internet. Routing is static; there is no real Internet access.
- Branch switch configurations were not captured.
- Screenshot files were confirmed present and non-empty in the local screenshots folder, but their visual contents were not independently reviewed as part of this README check.

---

## Project Evidence

The following screenshot files were confirmed present and non-empty in the local screenshots folder:

`01-routing-topology.png`, `02-hq-switch-vlans-trunk.png`, `03-hq-r1-interfaces.png`, `04-hq-r1-routing-table.png`, `05-br1-r1-routing-and-relay.png`, `05b-br2-r1-routing-and-relay.png`, `06-hq-r1-nat-config.png`, `07-hq-r1-nat-translations.png`, `08-dhcp-client-hq-pc1.png`, `08b-dhcp-client-br1-pc1.png`, `08c-dhcp-client-br2-pc1.png`, `09-connectivity-tests-hq-pc1.png`, `10-hq-srv-dhcp-pools.png`, `11-dns-tests.png`, `11b-dns-tests-br1-pc1.png`, `11c-dns-tests-br2-pc1.png`, `12-http-access-tests.png`, `12b-http-access-tests-br1-pc1.png`, `12c-http-access-tests-br2-pc1.png`, `12d-hq-custom-http-page.png`, `13-hq-srv-dns-and-http.png`, `13b-hq-srv-http-service.png`, `14-server-ip-configurations.png`, `14b-ext-srv-ip-configuration.png`, `15-remaining-pc-addresses.png`, `15b-br1-pc2-address.png`, `15c-br2-pc2-address.png`, `16-branch-connectivity-tests.png`, `16b-br2-branch-connectivity-tests.png`.

Place these PNG files in the same GitHub project folder as this README so the relative image links above render correctly. The screenshots currently reside in `C:\Users\kingo\PacketTracerMCP\screenshots\` and are shared with another project, so upload only the listed files to this project's folder.

---

## Project Files

- `03-Routing-and-IP-Services.pkt` is the Cisco Packet Tracer 9.0.1 project. Open it in Packet Tracer 9.0.1 or later.
- `README.md` documents the lab. The PNG evidence files are listed above.

---

## Skills Demonstrated

- Multi-site IP planning with non-overlapping subnets
- Static and default routing with router-on-a-stick sub-interfaces
- Centralized DHCP with relay, internal DNS and HTTP services
- NAT/PAT with an ACL and overload, verified with translation tables
- Troubleshooting simulator behaviour and reporting results with limitations

## Conclusion

The lab demonstrates static and default routing, DHCP with relay, DNS, HTTP and NAT/PAT across three sites toward a simulated external test host. The core reported tests passed. First-request delays, shifting DHCP leases and untested scenarios are documented above.
