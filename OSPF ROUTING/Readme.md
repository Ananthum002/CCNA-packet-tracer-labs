# OSPF (Open Shortest Path First)

## What is it?
An open-standard **link-state** routing protocol. Every router builds a full map of the network and calculates the shortest path itself using **SPF (Dijkstra's algorithm)**.

## Key Facts
- Metric: **cost** = Reference Bandwidth (default 100 Mbps) ÷ Interface Bandwidth → faster link = lower cost = preferred
- No hop count limit, scales to large networks
- Uses **areas** to organize the network — all areas must connect to **Area 0 (backbone)**
- Administrative Distance: **110**
- Open standard → works across all vendors (not just Cisco)

## Key Terms (commonly asked)
| Term | Meaning |
|---|---|
| LSA | Link-State Advertisement — info routers flood about their links |
| LSDB | Link-State Database — full topology map, same on every router in an area |
| DR / BDR | Designated Router / Backup — elected on Ethernet segments to reduce flooding |
| Router ID | Unique ID for each router, looks like an IP address |
| Area 0 | The backbone area; all other areas must connect to it |

## Neighbor States (just remember the last one)
Down → Init → 2-Way → ExStart → Exchange → Loading → **Full** (fully adjacent)

## Basic Configuration

```bash
router ospf 1
 router-id 1.1.1.1
 network 192.168.1.0 0.0.0.255 area 0
 network 10.0.0.0 0.0.0.255 area 0
```

## Verify

```bash
show ip ospf neighbor
show ip ospf database
show ip route ospf
```

## Pros / Cons
- ✅ Fast convergence
- ✅ Open standard, multi-vendor support
- ✅ Scales well using areas
- ❌ More complex to design/configure than RIP or EIGRP
- ❌ Higher CPU/memory use (keeps full topology map)

## One-Line Summary
OSPF = the most common enterprise IGP — every router builds a full map of the network and calculates the best path itself using cost (bandwidth-based).
