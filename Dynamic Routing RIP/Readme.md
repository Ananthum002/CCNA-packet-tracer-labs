# RIP Routing (Routing Information Protocol)

A reference guide to RIP — one of the oldest and simplest distance-vector routing protocols still used in small to mid-sized networks.

## Overview

RIP is an interior gateway protocol (IGP) that uses **hop count** as its sole metric to determine the best path between source and destination. Each hop along a path adds 1 to the route's metric, and any route with a hop count greater than **15** is considered unreachable — this caps RIP's usable network diameter at 15 hops.

## Versions

| Version | Description |
|---|---|
| RIPv1 | Classful routing, broadcasts updates (255.255.255.255), no support for VLSM/CIDR, no authentication |
| RIPv2 | Classless routing (supports VLSM/CIDR), multicasts updates (224.0.0.9), supports plain-text and MD5 authentication, includes next-hop and route tag fields |
| RIPng | RIPv2 adapted for IPv6, uses UDP port 521 and multicasts to FF02::9 |

## How It Works

RIP routers exchange their full routing tables with directly connected neighbors at regular intervals (every 30 seconds by default). Each router builds its table using the **Bellman-Ford algorithm**, always preferring the route with the lowest hop count.

### Key Timers

| Timer | Default | Purpose |
|---|---|---|
| Update timer | 30 sec | Interval between periodic routing table broadcasts/multicasts |
| Invalid timer | 180 sec | Time before a route is marked invalid if no update is received |
| Hold-down timer | 180 sec | Time the router waits before accepting a less favorable route, to prevent flapping |
| Flush timer | 240 sec | Time before an invalid route is removed from the routing table entirely |

### Loop Prevention

RIP uses several mechanisms to avoid routing loops:
- **Split horizon** — don't advertise a route back out the interface it was learned from
- **Split horizon with poison reverse** — advertise the route back out with a hop count of 16 (infinity)
- **Triggered updates** — send an immediate update when a route changes, rather than waiting for the next periodic interval
- **Hold-down timers** — suppress updates about a route for a period after it's marked unreachable

## Basic Configuration (Cisco IOS)

```bash
router rip
 version 2
 network 192.168.1.0
 network 10.0.0.0
 no auto-summary
```

- `version 2` enables RIPv2 (classless, supports VLSM)
- `network` statements specify which directly connected networks participate in RIP and are advertised
- `no auto-summary` disables automatic summarization at classful boundaries, important when using VLSM

### Enabling Authentication (RIPv2)

```bash
interface GigabitEthernet0/0
 ip rip authentication mode md5
 ip rip authentication key-chain RIP-KEYS

key chain RIP-KEYS
 key 1
  key-string mysecretkey
```

### Verifying RIP

```bash
show ip protocols
show ip route rip
debug ip rip
```

## Advantages

- Simple to configure and understand
- Low resource overhead, suitable for small networks
- Widely supported across vendors

## Limitations

- Maximum network diameter of 15 hops
- Slow convergence compared to modern protocols
- Hop count ignores bandwidth/latency, so it can pick suboptimal paths
- Periodic full-table broadcasts/multicasts consume bandwidth as the network grows

## RIP vs. Other IGPs

| Protocol | Type | Metric | Max Hops | Convergence |
|---|---|---|---|---|
| RIP | Distance-vector | Hop count | 15 | Slow |
| EIGRP | Advanced distance-vector | Bandwidth, delay, etc. | 224 | Fast |
| OSPF | Link-state | Cost (bandwidth-based) | None | Fast |

## When to Use RIP

RIP is best suited for small, simple networks with minimal topology changes, where ease of configuration outweighs the need for scalability or fast convergence. For larger or more dynamic networks, OSPF or EIGRP are generally preferred.

## References

- RFC 2453 — RIP Version 2
- RFC 2080 — RIPng for IPv6
