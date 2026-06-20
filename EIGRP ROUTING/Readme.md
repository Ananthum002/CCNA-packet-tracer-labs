# EIGRP Routing (Enhanced Interior Gateway Routing Protocol)

A reference guide to EIGRP — Cisco's advanced distance-vector routing protocol, known for fast convergence and flexible metric calculation.

## Overview

EIGRP is a Cisco-developed interior gateway protocol (IGP), originally proprietary but opened as an IETF informational standard (RFC 7868) in 2016. It's often described as a **hybrid** protocol: it behaves like a distance-vector protocol (advertising routes via neighbors) but uses link-state-like features such as neighbor discovery, topology tables, and partial/triggered updates for fast convergence.

EIGRP's defining feature is the **Diffusing Update Algorithm (DUAL)**, which guarantees loop-free paths and near-instant convergence by pre-computing backup routes.

## Key Concepts

| Concept | Description |
|---|---|
| Neighbor table | Tracks directly connected EIGRP routers discovered via Hello packets |
| Topology table | Stores all routes advertised by neighbors, not just the best one |
| Routing table | Contains only the best (successor) routes installed for forwarding |
| Successor | The current best, loop-free route to a destination |
| Feasible successor | A pre-computed backup route, guaranteed loop-free, used instantly if the successor fails |

## Metric Calculation

Unlike RIP's simple hop count, EIGRP computes a composite metric from multiple K-values (by default, only bandwidth and delay are used):

```
Metric = 256 * ((K1*BW + (K2*BW)/(256-load) + K3*delay) * (K5/(reliability+K4)))
```

With default K-values (K1=1, K3=1, K2=K4=K5=0), this simplifies to:

```
Metric = 256 * (BW + delay)
```

- **Bandwidth (BW)**: the minimum bandwidth along the path, in units of 10,000,000 / kbps
- **Delay**: the cumulative delay along the path, in tens of microseconds

Maximum hop count is 224, far higher than RIP's 15.

## How DUAL Works

1. Each router maintains a topology table of all paths advertised by neighbors for each destination.
2. The lowest-cost path becomes the **successor**; any backup route with a lower reported distance than the successor's feasible distance qualifies as a **feasible successor**.
3. If the successor fails, the router instantly switches to a feasible successor with no recomputation needed.
4. If no feasible successor exists, the router goes into **Active** state and queries neighbors to recompute the path — this is the only scenario with noticeable convergence delay.

## Basic Configuration (Cisco IOS)

### Classic mode

```bash
router eigrp 100
 network 192.168.1.0 0.0.0.255
 network 10.0.0.0 0.0.0.255
 no auto-summary
```

- `100` is the autonomous system (AS) number — must match across all routers in the same EIGRP domain
- `network` statements with wildcard masks specify which interfaces participate
- `no auto-summary` disables automatic summarization at classful boundaries

### Named mode (recommended on modern IOS/IOS-XE)

```bash
router eigrp NAMED-EIGRP
 address-family ipv4 unicast autonomous-system 100
  network 192.168.1.0 0.0.0.255
  network 10.0.0.0 0.0.0.255
  af-interface default
   no split-horizon
  exit-af-interface
 exit-address-family
```

### Enabling Authentication (MD5)

```bash
key chain EIGRP-KEYS
 key 1
  key-string mysecretkey

interface GigabitEthernet0/0
 ip authentication mode eigrp 100 md5
 ip authentication key-chain eigrp 100 EIGRP-KEYS
```

### Adjusting Bandwidth/Delay (affects metric)

```bash
interface GigabitEthernet0/0
 bandwidth 100000
 delay 100
```

### Verifying EIGRP

```bash
show ip eigrp neighbors
show ip eigrp topology
show ip route eigrp
show ip protocols
debug eigrp packets
```

## Loop Prevention

- **DUAL** mathematically guarantees loop-free paths at all times, even during transient network changes
- **Split horizon** is enabled by default (disable with caution, e.g. on hub-and-spoke Frame Relay/DMVPN topologies)
- **Feasibility condition** ensures a backup route is only accepted as a feasible successor if it can't possibly loop back through the current router

## Advantages

- Very fast convergence due to pre-computed feasible successors
- Composite metric accounts for bandwidth and delay, not just hop count
- Supports unequal-cost load balancing (`variance` command)
- Lower bandwidth overhead than link-state protocols — sends partial/triggered updates, not full periodic broadcasts
- Supports VLSM, route summarization at any bit boundary, and multiple address families (IPv4, IPv6, VPNv4)

## Limitations

- Historically Cisco-proprietary; while RFC 7868 opened the spec, real-world support outside Cisco is limited
- Can become complex in very large or multi-vendor networks
- Stuck-in-Active (SIA) issues can occur in large/poorly summarized topologies if a router fails to respond to queries in time
- Less predictable metric behavior than link-state protocols like OSPF when bandwidth/delay aren't tuned carefully

## EIGRP vs. Other IGPs

| Protocol | Type | Metric | Max Hops | Convergence |
|---|---|---|---|---|
| RIP | Distance-vector | Hop count | 15 | Slow |
| EIGRP | Advanced distance-vector | Bandwidth, delay (composite) | 224 | Fast |
| OSPF | Link-state | Cost (bandwidth-based) | None | Fast |

## When to Use EIGRP

EIGRP is well suited for medium to large Cisco-centric networks that need fast convergence, flexible summarization, and lower configuration complexity than OSPF, particularly in hub-and-spoke WAN designs (e.g. DMVPN). For multi-vendor environments, OSPF or IS-IS are typically preferred.

## References

- RFC 7868 — Cisco's Enhanced Interior Gateway Routing Protocol (EIGRP)
