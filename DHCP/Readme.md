# DHCP (Dynamic Host Configuration Protocol) — Single Network 

## What is it?
A protocol that **automatically assigns IP addresses** (and other settings like default gateway, DNS) to hosts on a network, so you don't have to configure each device manually.

## When it's "single network"
The DHCP server and the clients are on the **same subnet/VLAN** — no router/relay agent is needed in between, since DHCP broadcasts can reach the server directly.

## The DORA Process (always asked)
| Step | Message | Direction |
|---|---|---|
| 1 | **D**iscover | Client broadcasts to find a DHCP server |
| 2 | **O**ffer | Server offers an available IP address |
| 3 | **R**equest | Client requests/confirms the offered IP |
| 4 | **A**cknowledge | Server confirms the lease (ACK) |

- Uses **UDP**: client → server on port **67**, server → client on port **68**
- All messages are **broadcast** (since the client has no IP yet)

## What DHCP Assigns
- IP address
- Subnet mask
- Default gateway
- DNS server(s)
- Lease time (how long the IP is valid before renewal is needed)

## Basic Configuration (Cisco IOS Router as DHCP Server)

```bash
ip dhcp excluded-address 192.168.1.1 192.168.1.10

ip dhcp pool LAN-POOL
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8
 lease 7
```

- `ip dhcp excluded-address` reserves addresses (like the gateway) so DHCP doesn't hand them out
- `lease 7` = lease time in days (default is 24 hours if not specified)

## Enabling a Client Interface to Use DHCP

```bash
interface GigabitEthernet0/1
 ip address dhcp
```

## Verify

```bash
show ip dhcp binding
show ip dhcp pool
show ip dhcp conflict
```

## Key Point for Single Network
Because client and server are in the **same broadcast domain**, no extra configuration (like an `ip helper-address`) is needed — that's only required when DHCP requests have to cross a router into a different subnet.

## One-Line Summary
DHCP automatically hands out IP addressing info to hosts using a 4-step broadcast process (DORA); in a single network, the server hears the broadcasts directly, so no relay is needed.
