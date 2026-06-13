## Static Route Syntax

### General Syntax

```cisco
ip route <destination-network> <subnet-mask> <next-hop-ip>
```
# Static Routing Lab

## Overview
This lab demonstrates static routing between three Cisco 1941 routers in Cisco Packet Tracer.

## Network Topology

R1 ---- R2 ---- R3

### IP Addressing

| Device | Interface | IP Address |
|----------|----------|------------|
| R1 | G0/0 | 10.0.0.1/8 |
| R1 | G0/1 | 20.0.0.1/8 |
| R2 | G0/0 | 20.0.0.2/8 |
| R2 | G0/1 | 30.0.0.1/8 |
| R3 | G0/0 | 30.0.0.2/8 |
| R3 | G0/1 | 40.0.0.1/8 |

## Router Configurations

### Router R1

```cisco
hostname R1

interface g0/0
 ip address 10.0.0.1 255.0.0.0
 no shutdown

interface g0/1
 ip address 20.0.0.1 255.0.0.0
 no shutdown

ip route 30.0.0.0 255.0.0.0 20.0.0.2
ip route 40.0.0.0 255.0.0.0 20.0.0.2
```

### Router R2

```cisco
hostname R2

interface g0/0
 ip address 20.0.0.2 255.0.0.0
 no shutdown

interface g0/1
 ip address 30.0.0.1 255.0.0.0
 no shutdown

ip route 10.0.0.0 255.0.0.0 20.0.0.1
ip route 40.0.0.0 255.0.0.0 30.0.0.2
```

### Router R3

```cisco
hostname R3

interface g0/0
 ip address 30.0.0.2 255.0.0.0
 no shutdown

interface g0/1
 ip address 40.0.0.1 255.0.0.0
 no shutdown

ip route 10.0.0.0 255.0.0.0 30.0.0.1
ip route 20.0.0.0 255.0.0.0 30.0.0.1
```

## Verification

```cisco
show ip route
ping
traceroute
```

## Skills Demonstrated

- Static Routing
- Router Configuration
- IPv4 Addressing
- Network Troubleshooting
- Connectivity Testing

## Files Included

- Static-Routing-Lab.pkt
- Topology.png
- README.md

## Author

Ananthu Mohan
