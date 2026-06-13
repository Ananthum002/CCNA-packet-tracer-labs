# CCNA Lab - RIP Routing Configuration

## Overview

This Cisco Packet Tracer lab demonstrates the implementation of Routing Information Protocol (RIP) Version 2 in a multi-router network. 
RIP is a distance-vector routing protocol that automatically shares routing information between routers, enabling communication across different networks.

## Topology

PC0 ↔ Router1 ↔ Switch0 ↔ Router2  
                      ↕  
                   Router3  
                      ↕  
                   Switch1  
                      ↕  
                   Router4 ↔ PC1

## IP Addressing Scheme

| Device | Interface | IP Address |
|----------|-----------|------------|
| Router1 | Fa0/0 | 10.0.0.1/8 |
| Router1 | Fa0/1 | 20.0.0.1/8 |
| Router2 | Fa0/0 | 20.0.0.2/8 |
| Router2 | Fa0/1 | 30.0.0.1/8 |
| Router3 | Fa0/0 | 20.0.0.3/8 |
| Router3 | Fa0/1 | 30.0.0.2/8 |
| Router4 | Fa0/0 | 30.0.0.3/8 |
| Router4 | Fa0/1 | 40.0.0.1/8 |

## RIP Version 2 Configuration

### Router1

```bash
enable
configure terminal

router rip
version 2
network 10.0.0.0
network 20.0.0.0
no auto-summary

end
write memory
```

### Router2

```bash
enable
configure terminal

router rip
version 2
network 20.0.0.0
network 30.0.0.0
no auto-summary

end
write memory
```

### Router3

```bash
enable
configure terminal

router rip
version 2
network 20.0.0.0
network 30.0.0.0
no auto-summary

end
write memory
```

### Router4

```bash
enable
configure terminal

router rip
version 2
network 30.0.0.0
network 40.0.0.0
no auto-summary

end
write memory
```

## RIP Syntax

```bash
router rip
version 2
network <network-address>
no auto-summary
```

## Verification Commands

```bash
show ip route
show ip protocols
show ip rip database
show running-config
show ip interface brief
```

## Connectivity Testing

```bash
ping 40.0.0.1
ping 10.0.0.1
traceroute 40.0.0.1
```

## Skills Practiced

- Router Configuration
- IP Addressing
- RIP Version 2 Configuration
- Dynamic Routing
- Route Advertisement
- Routing Table Verification
- Connectivity Testing
- Network Troubleshooting

## Software Used

- Cisco Packet Tracer
- Cisco 1841 Routers
- Cisco 2960 Switches

## Author

Ananthu Mohan

CCNA Networking Lab Repository
