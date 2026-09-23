# IP Services

## Overview

This section contains hands-on Cisco Packet Tracer labs covering common **CCNA-level IP Services** used in enterprise networks.

The labs focus on addressing, automatic host configuration, name resolution, address translation, application services, and basic client-server communication.

## Labs Completed

| # | Lab | Key Concepts | Status |
|---|---|---|---|
| 01 | [DHCP](01-DHCP/) | DHCP Server, Dynamic IP Addressing, Address Pool | ✅ Completed |
| 02 | [DHCP Relay](02-DHCP-Relay/) | DHCP Relay, `ip helper-address`, Inter-network DHCP | ✅ Completed |
| 03 | [DNS](03-DNS/) | DNS Server, Name Resolution, A Records | ✅ Completed |
| 04 | [NAT](04-NAT/) | Network Address Translation, Inside/Outside, Static NAT | ✅ Completed |
| 05 | [PAT](05-PAT/) | Port Address Translation, Overload, Shared Public IP | ✅ Completed |
| 06 | [Web Server – IIS](06-Web-Server-IIS/) | HTTP, DNS, Web Server, Client Access | ✅ Completed |
| 07 | [FTP Server](07-FTP-Server/) | FTP, User Authentication, File Transfer | ✅ Completed |
| 08 | [Email Server](08-Email-Server/) | SMTP, POP3, Email Accounts, Mail Delivery | ✅ Completed |

## Lab Coverage

### 1. DHCP

Configured a DHCP service to automatically provide IP addressing information to network clients.

**Covered:**
- DHCP address pool
- Network and subnet mask
- Default gateway
- Automatic host addressing
- DHCP verification

### 2. DHCP Relay

Configured DHCP Relay to allow clients in a different network to obtain addressing information from a centralized DHCP server.

**Covered:**
- `ip helper-address`
- DHCP client-server communication across networks
- Inter-network DHCP forwarding
- Verification

### 3. DNS

Configured a DNS server for hostname-to-IP address resolution.

**Covered:**
- DNS service
- A records
- Hostname resolution
- Client DNS configuration
- Name-based connectivity testing

### 4. NAT

Configured Network Address Translation to translate private addressing to a different address space.

**Covered:**
- Inside and outside interfaces
- Static NAT
- Translation verification
- Connectivity testing

### 5. PAT

Configured Port Address Translation using address overload so multiple internal hosts can share a translated address.

**Covered:**
- PAT / NAT overload
- Inside local and inside global addresses
- Translation table verification
- Connectivity testing

### 6. Web Server

Configured a Packet Tracer HTTP server and used DNS-based access from client PCs.

**Covered:**
- HTTP service
- DNS integration
- Web server IP configuration
- Browser-based testing
- Client-server communication

> Note: This Packet Tracer lab simulates web-server functionality; it is not a real Microsoft IIS deployment.

### 7. FTP Server

Configured an FTP server with authenticated user access and verified file transfer from network clients.

**Covered:**
- FTP service
- Username/password authentication
- FTP login
- Directory access
- File upload
- Client-side verification

### 8. Email Server

Configured an email server with SMTP and POP3 services and verified end-to-end email communication between two clients.

**Covered:**
- SMTP
- POP3
- Email domain
- User accounts
- Email client configuration
- Send/receive verification

## Overall Network Skills Practiced

- IPv4 addressing
- Default gateway configuration
- DHCP
- DHCP Relay
- DNS
- NAT
- PAT
- HTTP
- FTP
- SMTP
- POP3
- Client-server communication
- Network service verification
- Connectivity testing

## Verification Approach

Each individual lab includes its own topology, configuration, and verification screenshots along with the corresponding Packet Tracer `.pkt` file.

Testing was performed using appropriate Packet Tracer tools such as:

```text
ping
show ip interface brief
show ip nat translations
show ip nat statistics
ipconfig
nslookup
FTP client commands
Web Browser
Email client
```

The exact commands and verification steps are documented inside each individual lab README.

## Software Used

- Cisco Packet Tracer

## Status

**All IP Services labs in this section are completed and documented.**

## Author

**Mohamed Ashik**

Networking Labs Portfolio  
GitHub: `mohamedashik-cpu/networking-labs`
