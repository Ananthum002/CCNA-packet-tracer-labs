## Static Route Syntax

```bash
ip route <destination-network> <subnet-mask> <next-hop-ip>
```

### Example

```bash
ip route 192.168.1.24 255.255.255.248 192.168.1.18
```

---

## Default Route Syntax

```bash
ip route 0.0.0.0 0.0.0.0 <next-hop-ip>
```

### Example

```bash
ip route 0.0.0.0 0.0.0.0 192.168.1.10
```

---

## Configuration

### R1 (Default Route)

```bash
ip route 0.0.0.0 0.0.0.0 192.168.1.10
```

### R2 (Static Routes)

```bash
ip route 192.168.1.0 255.255.255.248 192.168.1.9
ip route 192.168.1.24 255.255.255.248 192.168.1.18
```

### R3 (Default Route)

```bash
ip route 0.0.0.0 0.0.0.0 192.168.1.17
```

## Verification Commands

```bash
show ip route
show running-config
show ip interface brief
ping <destination-ip>
traceroute <destination-ip>
```
