#  🏠 Basic Home Network Lab (Packet Tracer)

A simple home LAN built using Cisco Packet Tracer.
This project focuses on establishing a functional network using a router, switch, and two PCs, including troubleshooting and DHCP configuration.

This Home Network consists of:
- 1 x Cisco 1941 router (acting as default gateway + DHCP server)
- 1 x 2960 switch (Layer 2 Switch)
- 2 x PCs receiving IP addresses via DHCP

## Objectives

- Build a basic home Local Area Network (LAN) using Cisco Packet Tracer  
- Configure a router to operate as the default gateway and DHCP server  
- Connect end devices through a Layer 2 switch and verify physical link status  
- Enable router interfaces and troubleshoot red/amber link states  
- Provide automatic IP configuration to PCs using DHCP  
- Validate end-to-end connectivity between hosts and the router  

## Topology

- Router `GigabitEthernet0/0` → Switch `GigabitEthernet0/1`
- Switch `FastEthernet0/1` → PC-Home1
- Switch `FastEthernet0/2` → PC-Home2

```text
[Router (1941)]
    Gi0/0: 192.168.1.254/24
           |
           |
[Switch (2960)]
     |             |
 Fa0/1          Fa0/2
  |               |
[PC1]           [PC2]
```

### 1. Add Devices
      - 1 x Cisco 1941 router
      - 1 x Cisco 2960 switch
      - 2 x PCs 
      
### 2. Connect the Devices
**Use Copper Straight-Through cables:**

Switch ↔ Router
- Switch Gi0/1 → Router Gi0/0

Switch ↔ PCs
- Switch Fa0/1 → PC1 Fa0
- Switch Fa/02 → PC1 Fa0

At this point, all links should normally transition:
amber → green

However, the link between: Router Gi0/0  ↔  Switch Gi0/1, remained RED

<img width="267" height="313" alt="image" src="https://github.com/user-attachments/assets/e2477493-b99c-49ab-8c9c-4a17494ddcf1" />

--- 

### 2.1 TROUBLESHOOTING – Router ↔ Switch Link Red

#### Enable the Router Interface
- opened the router CLI and ran:

enable
configure terminal
interface gigabitEthernet0/0
no shutdown

<img width="1018" height="256" alt="image" src="https://github.com/user-attachments/assets/bd948fba-bfc4-4a95-ba2b-39a638a1db3f" />

Observed:

<img width="354" height="27" alt="image" src="https://github.com/user-attachments/assets/5777a3dd-8543-4081-9b16-244b3f7f86c2" />

**The link turned green, and the LAN became operational.**

---

### 3. Configure the Router (LAN + DHCP)

Here, I configured the router to act like a home gateway.

#### 3.1 Assign LAN IP to Router G0/0

```
interface GigabitEthernet0/0
 ip address 192.168.1.254 255.255.255.0
 no shutdown
```
This makes the router:
- The default gateway
- The Layer 3 device for the LAN
- The anchor of the 192.168.1.0/24 network

**The subnet mask `255.255.255.0` (`/24`) was chosen because it is the standard size for small LANs. It provides 254 usable host addresses, which is ideal for a home network.**

#### 3.2 Configure DHCP on the Router
Here, I configured the router to hand out IPs automatically.

```
ip dhcp excluded-address 192.168.1.254
ip dhcp pool HOME-LAN
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.254
 dns-server 8.8.8.8
```
DHCP behavior:

- Router gives out 192.168.1.x addresses
- Gateway is 192.168.1.254
- DNS resolves through 8.8.8.8

---

### Configure the PCs
On each PC:
```
1. Click the PC
2. Go to Desktop → IP Configuration
3. Select DHCP
```
<img width="1219" height="298" alt="image" src="https://github.com/user-attachments/assets/672827f7-b77a-479b-a0b3-a2eb83f83aeb" />

---
<img width="2854" height="1286" alt="image" src="https://github.com/user-attachments/assets/06958d48-0af4-4d91-8640-b7a0428f3083" />

Each PC automatically received:

- IP Address: 192.168.1.x
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.1.254
- DNS: 8.8.8.8

### 5. Testing & Validation

**Test 1 — PC1 → Ping the Router (Gateway)**
From PC1:
```
ping 192.168.1.254
```
<img width="652" height="286" alt="image" src="https://github.com/user-attachments/assets/3955fcf2-e9b0-4dba-a127-549fbf04a4a6" />

**The successful test confirms that the router is reachable, its LAN interface is active, and it is correctly configured as the default gateway.**

---
**Test 2 — PC1 → Ping PC2**

```
ping 192.168.1.2
```
<img width="1698" height="946" alt="image" src="https://github.com/user-attachments/assets/88dccecb-545b-425a-8d83-cbf1253d7c8a" />

**The successful test confirms that devices can communicate with each other through the switch without any issues.**

----

### Addtional Info 

- A Layer 2 switch was used in this home network setup because all devices operate within the same subnet. A Layer 2 switch forwards traffic based on MAC addresses, providing simple, fast, and efficient device-to-device connectivity without requiring any routing features.
- The subnet mask 255.255.255.0 (/24) was chosen because it is the standard size for small LANs. It provides 254 usable IP addresses, which is more than enough for a home network and keeps addressing simple and easy to manage.
It was ideal to use this subnet mask for the setup because a `/24` subnet keeps all devices in the same broadcast domain, allowing them to communicate easily without routing. It offers a balance of simplicity and scalability for typical home or small office environments.
The router handles DHCP because it is already acting as the default gateway for the network. Centralizing DHCP on the router ensures all devices receive consistent IP, gateway, and DNS settings automatically, just like a real home router.
