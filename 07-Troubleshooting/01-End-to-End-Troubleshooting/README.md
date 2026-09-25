# End-to-End Troubleshooting

## Overview

This lab demonstrates a structured end-to-end network troubleshooting workflow using Cisco Packet Tracer. An intentional interface failure was introduced, diagnosed, corrected, and verified through end-to-end connectivity testing.

## Objective

- Apply a structured troubleshooting methodology
- Identify an interface-level connectivity issue
- Use CLI verification commands to isolate the fault
- Restore the affected interface
- Verify end-to-end connectivity
- Save the corrected configuration

## Topology

~~~
PC0
 |
SW1
 |
R1
 |
SW2
 |
PC1
~~~

## Devices Used

- Cisco 2911 Router — R1
- Cisco 2960 Switch — SW1
- Cisco 2960 Switch — SW2
- PC0
- PC1

## IP Addressing

| Device | Interface | IP Address | Subnet Mask | Gateway |
|---|---|---|---|---|
| R1 | G0/0 | 192.168.150.1 | 255.255.255.0 | — |
| R1 | G0/1 | 192.168.160.1 | 255.255.255.0 | — |
| PC0 | Fa0 | 192.168.150.10 | 255.255.255.0 | 192.168.150.1 |
| PC1 | Fa0 | 192.168.160.10 | 255.255.255.0 | 192.168.160.1 |

## Troubleshooting Scenario

The R1 G0/1 interface was intentionally disabled:

~~~
interface gigabitEthernet 0/1
shutdown
~~~

After the fault was introduced, PC0 could no longer reach PC1.

## Troubleshooting Process

### 1. Test Connectivity

~~~
ping 192.168.160.10
~~~

Result: **Ping failed**

### 2. Identify the Fault

~~~
show ip interface brief
~~~

R1 G0/1 was found in an **administratively down** state.

### 3. Fix the Fault

~~~
interface gigabitEthernet 0/1
no shutdown
~~~

### 4. Verify Interface Status

~~~
show ip interface brief
~~~

R1 G0/0 and G0/1 returned to **up/up**.

### 5. Verify End-to-End Connectivity

~~~
ping 192.168.160.10
~~~

Result: **Ping successful**

### 6. Save Configuration

~~~
copy running-config startup-config
~~~

## Troubleshooting Methodology

**Test → Identify → Isolate → Fix → Verify → Save**

## Screenshots

- 01-topology.png
- 02-baseline-connectivity.png
- 03-interface-verification.png
- 04-fault-created.png
- 05-fault-identification.png
- 06-interface-restored.png
- 07-end-to-end-ping-success.png
- 08-final-verification.png

## Packet Tracer File

- 01-End-to-End-Troubleshooting.pkt

## Concepts Learned

- End-to-end connectivity testing
- Interface status verification
- Administrative shutdown
- show ip interface brief
- Ping-based troubleshooting
- Fault isolation
- Configuration persistence

## Status

✅ Completed and verified

## Author

**Mohamed Ashik**
