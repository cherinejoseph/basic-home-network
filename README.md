#  🏠 Basic Home Network Lab (Packet Tracer)

## Overview
This lab simulates a simple home LAN built using Cisco Packet Tracer:

- 1 x Cisco 1941 router (acting as default gateway + DHCP server)
- 1 x 2960 switch
- 2 x PCs receiving IP addresses via DHCP

The goal is to understand how a basic routed LAN works: default gateway, DHCP, and host-to-host connectivity.

## Objectives

- Build a functional home LAN using Cisco devices  
- Configure a router as:
  - Default gateway  
  - DHCP server  
  - DNS distributor  
- Connect multiple PCs through a switch  
- Validate end-to-end network connectivity  

## Topology

- Router `GigabitEthernet0/0` → Switch `GigabitEthernet0/1`
- Switch `FastEthernet0/2` → PC-Home1
- Switch `FastEthernet0/3` → PC-Home2

```text
[Router (1941)]
    Gi0/0: 192.168.1.254/24
           |
           |
[Switch (2960)]
     |             |
 Fa0/2          Fa0/3
  |               |
[PC-Home1]     [PC-Home2]
```
