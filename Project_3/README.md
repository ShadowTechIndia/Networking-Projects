# Cisco Packet Tracer - 3 Floor Office Network Lab

## Network Requirements

### Devices

* 3 Routers
* 3 Switches
* 8 PCs
* 8 Printers
* 3 Access Points (1 AP per floor)

### Departments & VLANs

| Floor   | Department | VLAN    | Network        |
| ------- | ---------- | ------- | -------------- |
| Floor 1 | Reception  | VLAN 80 | 192.168.8.0/24 |
| Floor 1 | Store      | VLAN 70 | 192.168.7.0/24 |
| Floor 1 | Logistics  | VLAN 60 | 192.168.6.0/24 |
| Floor 2 | Sales      | VLAN 30 | 192.168.3.0/24 |
| Floor 2 | HR         | VLAN 40 | 192.168.4.0/24 |
| Floor 2 | Finance    | VLAN 50 | 192.168.5.0/24 |
| Floor 3 | IT         | VLAN 10 | 192.168.1.0/24 |
| Floor 3 | Admin      | VLAN 20 | 192.168.2.0/24 |

---

# Step 1 - Physical Connections

## Router Hardware

Install **HWIC-2T** modules on all routers.

## Cabling Rules

### Switch Connections

* Connect PCs, Printers, and APs to switches using Copper Straight-Through cables.
* Ensure all interfaces are enabled (`no shutdown`).

### Router Connections

Use Serial DCE/DTE cables.

#### Serial Links

| Connection | Network       |
| ---------- | ------------- |
| F1 ↔ F2    | 10.10.10.8/30 |
| F2 ↔ F3    | 10.10.10.0/30 |
| F1 ↔ F3    | 10.10.10.4/30 |

---

# Step 2 - VLAN Configuration

## Floor 1 Switch

```bash
enable
configure terminal

vlan 60
name Logistics

vlan 70
name Store

vlan 80
name Reception

interface range fa0/6-8
 switchport mode access
 switchport access vlan 60
 exit

interface range fa0/4-5
 switchport mode access
 switchport access vlan 70
 exit

interface range fa0/2-3
 switchport mode access
 switchport access vlan 80
 exit

interface fa0/1
 switchport mode trunk
 exit

do write
```

---

## Floor 2 Switch

```bash
enable
configure terminal

vlan 30
name Sales

vlan 40
name HR

vlan 50
name Finance

interface range fa0/6-8
 switchport mode access
 switchport access vlan 50
 exit

interface range fa0/4-5
 switchport mode access
 switchport access vlan 40
 exit

interface range fa0/2-3
 switchport mode access
 switchport access vlan 30
 exit

interface fa0/1
 switchport mode trunk
 exit

do write
```

---

## Floor 3 Switch

```bash
enable
configure terminal

vlan 10
name IT

vlan 20
name Admin

interface range fa0/4-6
 switchport mode access
 switchport access vlan 10
 exit

interface range fa0/2-3
 switchport mode access
 switchport access vlan 20
 exit

interface fa0/1
 switchport mode trunk
 exit

do write
```

---

# Step 3 - Serial Interface Configuration

## F1 Router

```bash
enable
configure terminal

interface se0/1/0
 ip address 10.10.10.6 255.255.255.252
 no shutdown
 exit

interface se0/1/1
 ip address 10.10.10.9 255.255.255.252
 no shutdown
 exit

do write
```

---

## F2 Router

```bash
enable
configure terminal

interface se0/2/1
 ip address 10.10.10.10 255.255.255.252
 clock rate 64000
 no shutdown
 exit

interface se0/2/0
 ip address 10.10.10.1 255.255.255.252
 no shutdown
 exit

do write
```

---

## F3 Router

```bash
enable
configure terminal

interface se0/2/0
 ip address 10.10.10.5 255.255.255.252
 clock rate 64000
 no shutdown
 exit

interface se0/2/1
 ip address 10.10.10.2 255.255.255.252
 clock rate 64000
 no shutdown
 exit

do write
```

---

# Step 4 - Router-on-a-Stick Configuration

## F1 Router

```bash
hostname F1-Router

interface gig0/0
 no shutdown

interface gig0/0.60
 encapsulation dot1Q 60
 ip address 192.168.6.1 255.255.255.0

interface gig0/0.70
 encapsulation dot1Q 70
 ip address 192.168.7.1 255.255.255.0

interface gig0/0.80
 encapsulation dot1Q 80
 ip address 192.168.8.1 255.255.255.0

do write
```

---

## F2 Router

```bash
hostname F2-Router

interface gig0/0
 no shutdown

interface gig0/0.30
 encapsulation dot1Q 30
 ip address 192.168.3.1 255.255.255.0

interface gig0/0.40
 encapsulation dot1Q 40
 ip address 192.168.4.1 255.255.255.0

interface gig0/0.50
 encapsulation dot1Q 50
 ip address 192.168.5.1 255.255.255.0

do write
```

---

## F3 Router

```bash
hostname F3-Router

interface gig0/0
 no shutdown

interface gig0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.1 255.255.255.0

interface gig0/0.20
 encapsulation dot1Q 20
 ip address 192.168.2.1 255.255.255.0

do write
```

---

# Step 5 - DHCP Configuration

## F1 Router

```bash
ip dhcp pool Reception
 network 192.168.8.0 255.255.255.0
 default-router 192.168.8.1
 dns-server 8.8.8.8

ip dhcp pool Store
 network 192.168.7.0 255.255.255.0
 default-router 192.168.7.1
 dns-server 8.8.8.8

ip dhcp pool Logistics
 network 192.168.6.0 255.255.255.0
 default-router 192.168.6.1
 dns-server 8.8.8.8
```

---

## F2 Router

```bash
ip dhcp pool Sales
 network 192.168.3.0 255.255.255.0
 default-router 192.168.3.1
 dns-server 8.8.8.8

ip dhcp pool HR
 network 192.168.4.0 255.255.255.0
 default-router 192.168.4.1
 dns-server 8.8.8.8

ip dhcp pool Finance
 network 192.168.5.0 255.255.255.0
 default-router 192.168.5.1
 dns-server 8.8.8.8
```

---

## F3 Router

```bash
ip dhcp pool IT
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8

ip dhcp pool Admin
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 8.8.8.8
```

---

# Step 6 - SSH Configuration

## Apply on All Routers

### F1 Router

```bash
ip domain-name router.com
username router1 password router1

crypto key generate rsa
1024

ip ssh version 2

line vty 0 15
 login local
 transport input ssh
```

### F2 Router

```bash
ip domain-name router.com
username router2 password router2

crypto key generate rsa
1024

ip ssh version 2

line vty 0 15
 login local
 transport input ssh
```

### F3 Router

```bash
ip domain-name router.com
username router3 password router3

crypto key generate rsa
1024

ip ssh version 2

line vty 0 15
 login local
 transport input ssh
```

---

# Step 7 - OSPF Routing

## F1 Router

```bash
router ospf 10

network 10.10.10.4 0.0.0.3 area 0
network 10.10.10.8 0.0.0.3 area 0

network 192.168.6.0 0.0.0.255 area 0
network 192.168.7.0 0.0.0.255 area 0
network 192.168.8.0 0.0.0.255 area 0
```

---

## F2 Router

```bash
router ospf 10

network 10.10.10.0 0.0.0.3 area 0
network 10.10.10.8 0.0.0.3 area 0

network 192.168.3.0 0.0.0.255 area 0
network 192.168.4.0 0.0.0.255 area 0
network 192.168.5.0 0.0.0.255 area 0
```

---

## F3 Router

```bash
router ospf 10

network 10.10.10.0 0.0.0.3 area 0
network 10.10.10.4 0.0.0.3 area 0

network 192.168.1.0 0.0.0.255 area 0
network 192.168.2.0 0.0.0.255 area 0
```

---

# Step 8 - Verification

## Verify DHCP

On every PC and Printer:

```text
Desktop > IP Configuration > DHCP
```

Verify that an IP address is assigned automatically.

---

## Verify OSPF Neighbors

```bash
show ip ospf neighbor
```

Expected: All routers should form OSPF adjacencies.

---

## Verify Routing Table

```bash
show ip route
```

Expected: All VLAN networks should be visible.

---

## Verify DHCP Leases

```bash
show ip dhcp binding
```

---

## Test Connectivity

From any PC:

```bash
ping 192.168.1.1
ping 192.168.2.1
ping 192.168.3.1
ping 192.168.4.1
ping 192.168.5.1
ping 192.168.6.1
ping 192.168.7.1
ping 192.168.8.1
```

Then test inter-floor communication:

```bash
ping <IP of PC on another VLAN>
```

If all pings are successful, the lab is completed successfully.

```

## Lab Completed

Features implemented:

- VLAN Segmentation
- Router-on-a-Stick
- DHCP
- OSPF Dynamic Routing
- SSH Remote Management
- Inter-VLAN Routing
- Multi-floor Enterprise Design
```
