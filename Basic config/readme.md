# Basic Configuration Lab

## Objective
Configure basic settings on Cisco router and switches across two networks.

## Topology
- 1 Router (Router0) - 1841
- 2 Switches (Switch0, Switch1) - 2960-24TT
- 7 PCs (PC0 - PC6)

## Configuration Applied
- Hostnames set on all devices
- IP addresses assigned to router interfaces and switch VLANs
- Default gateway set on all PCs and switches
- Running config saved to startup config

## Commands Used
Router> enable
Router# configure terminal
Router(config)# hostname Router0
Router(config)# interface fa0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
Router(config)# interface fa0/1
Router(config-if)# ip address 192.168.2.1 255.255.255.0
Router(config-if)# no shutdown
Router# write memory

## Expected Result
PCs on both sides can ping each other through Router0.
