# DHCP to a Different Network (DHCP Relay)

## What is it?
When the **DHCP server is on a different subnet** than the client, the client's DHCP broadcast can't cross the router on its own — routers don't forward broadcasts by default. A **DHCP Relay Agent** is used to fix this.

## The Problem
- DORA messages start as **broadcasts** (client has no IP yet)
- Routers **block broadcasts** between subnets by default
- So if the DHCP server is in another subnet/VLAN, the client's request never reaches it

## The Fix: DHCP Relay (`ip helper-address`)
Configured on the **router interface facing the client's subnet** (the default gateway for that LAN). It:
1. Listens for the client's DHCP broadcast
2. Converts it into a **unicast** packet
3. Forwards it directly to the DHCP server's IP address

This makes the router act as a "relay" between the client and the remote DHCP server.

## Basic Configuration (Cisco IOS)

```bash
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip helper-address 10.0.0.5
```

- `192.168.1.1` = gateway for the client subnet (where the broadcast arrives)
- `10.0.0.5` = the DHCP server's IP, sitting in a different subnet

> Apply `ip helper-address` on **every interface** that connects to a client subnet needing relay to the remote DHCP server.

## What Else `ip helper-address` Forwards
By default, it doesn't just relay DHCP — it also forwards other broadcast services (TFTP, DNS, NetBIOS, etc.) unless restricted. For CCNA purposes, just know **DHCP relay is its main/most common use**.

## Verify

```bash
show ip interface GigabitEthernet0/0
show running-config interface GigabitEthernet0/0
show ip dhcp binding        ← run on the DHCP server
```

## Quick Comparison

| Scenario | Needs `ip helper-address`? |
|---|---|
| DHCP server on the same subnet as client | ❌ No |
| DHCP server on a different subnet | ✅ Yes |

## One-Line Summary
When the DHCP server is in a different subnet, the gateway router needs `ip helper-address` pointing to the server's IP — this converts the client's DHCP broadcast into a unicast packet so it can cross the network.
