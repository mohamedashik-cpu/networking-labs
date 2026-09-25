# 06 - Network Security

## Overview

This section contains practical Cisco Packet Tracer labs covering fundamental network security concepts and firewall basics.

The labs are built as a progressive learning path, starting with traffic filtering and Layer 2 security, followed by secure device access, device hardening, password protection, and firewall fundamentals.

All documented results are based on verified Packet Tracer testing.

## Security Labs

| No. | Lab | Main Concepts | Status |
|---|---|---|---|
| 01 | [Standard ACL](01-Standard-ACL/) | Standard ACL, source-based traffic filtering, ACL application | ✅ Completed |
| 02 | [Extended ACL](02-Extended-ACL/) | Extended ACL, protocol/port filtering, HTTP filtering, ICMP permit | ✅ Completed |
| 03 | [Port Security](03-Port-Security/) | Sticky MAC, maximum MAC addresses, violation/shutdown mode | ✅ Completed |
| 04 | [SSH](04-SSH/) | SSH v2, local authentication, RSA keys, secure remote access | ✅ Completed |
| 05 | [Device Hardening](05-Device-Hardening/) | Console/VTY security, login controls, banners, unused interface shutdown | ✅ Completed |
| 06 | [Password Security](06-Password-Security/) | Enable secret, password encryption, console and VTY authentication | ✅ Completed |
| 07 | [Firewall Basics](07-Firewall-Basics/) | Cisco ASA, security zones, routing, PAT, ACL, NAT verification | ✅ Completed |

## Learning Path

**ACL → Port Security → SSH → Device Hardening → Password Security → Firewall Basics**

This progression covers traffic-level controls and device-level security before introducing Cisco ASA firewall concepts.

## Key Concepts Covered

### Access Control
- Standard ACL
- Extended ACL
- Source-based filtering
- Protocol and port-based filtering
- Inbound and outbound ACL application
- ACL verification and hit counters

### Layer 2 Security
- Switch port security
- Sticky MAC addresses
- Maximum MAC address limits
- Port-security violation modes
- Secure-shutdown and recovery verification

### Secure Device Access
- SSH version 2
- Local username authentication
- RSA key generation
- VTY line security
- Telnet-based authentication testing in controlled lab scenarios

### Device Hardening
- Console and VTY password protection
- exec-timeout
- logging synchronous
- MOTD banner
- no ip domain-lookup
- Shutdown of unused interfaces
- Password encryption

### Password Security
- enable password
- enable secret
- service password-encryption
- Console authentication
- VTY authentication
- Running-configuration verification

### Firewall Fundamentals
- Cisco ASA 5505
- Inside and outside security zones
- ASA security levels
- ASA VLAN interfaces
- Default routing
- Dynamic PAT
- NAT translation verification
- Outside inbound ACL
- ACL hit-count verification
- Return routing

## Verification Approach

Each completed lab includes relevant verification and testing such as:

- show commands
- Ping tests
- Connectivity checks
- ACL hit counters
- MAC address and security violation checks
- SSH login verification
- Console/VTY authentication tests
- NAT translation verification
- Final configuration checks

Screenshots and Packet Tracer .pkt files are included inside the individual lab folders.

## Repository Structure

~~~text
06-Network-Security/
├── 01-Standard-ACL/
├── 02-Extended-ACL/
├── 03-Port-Security/
├── 04-SSH/
├── 05-Device-Hardening/
├── 06-Password-Security/
├── 07-Firewall-Basics/
└── README.md
~~~

## Tools Used

- Cisco Packet Tracer

## Learning Outcome

After completing these labs, the following practical skills were developed:

- Filtering network traffic using ACLs
- Securing switch access ports
- Configuring secure remote device access with SSH
- Applying basic Cisco device hardening
- Protecting privileged and remote-access passwords
- Understanding fundamental firewall security zones
- Configuring and verifying basic ASA NAT/PAT
- Applying and verifying firewall ACLs
- Using Cisco IOS and ASA verification commands for troubleshooting

## Status

**7 security labs completed and verified in Cisco Packet Tracer.**

## Author

**Mohamed Ashik**