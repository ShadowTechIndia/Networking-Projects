# VLAN, Subnetting & Inter-VLAN Routing Lab

## Overview

This Cisco Packet Tracer lab demonstrates:

* VLAN Configuration
* IPv4 Subnetting (/26)
* Inter-VLAN Routing (Router-on-a-Stick)
* Wireless Networking
* Connectivity Testing

---

## Network Requirements

| Device       | Qty |
| ------------ | --- |
| Router       | 1   |
| Switch       | 1   |
| PC           | 3   |
| Printer      | 3   |
| Access Point | 3   |
| Laptop       | 3   |
| Smartphone   | 3   |

---

## Step 1: Device Connections

Use Copper Straight-Through cables.

| Port     | Device             |
| -------- | ------------------ |
| Fa0/1    | Router             |
| Fa0/2-4  | CS PC, Printer, AP |
| Fa0/5-7  | HR PC, Printer, AP |
| Fa0/8-10 | IT PC, Printer, AP |

Each AP serves:

* 1 Laptop
* 1 Smartphone

> Install a WPC300N wireless adapter on each laptop.

Enable any disabled interface:

```bash
configure terminal
interface <interface>
no shutdown
```

---

## Step 2: Subnetting Plan

Subnet Mask: `255.255.255.192`

CIDR: `/26`

| VLAN | Department | Network          | Gateway       | Broadcast     |
| ---- | ---------- | ---------------- | ------------- | ------------- |
| 10   | IT         | 192.168.1.0/26   | 192.168.1.1   | 192.168.1.63  |
| 20   | HR         | 192.168.1.64/26  | 192.168.1.65  | 192.168.1.127 |
| 30   | CS         | 192.168.1.128/26 | 192.168.1.129 | 192.168.1.191 |

---

## Step 3: Configure VLANs on Switch

Create VLANs:

```bash
enable
configure terminal

vlan 10
name IT

vlan 20
name HR

vlan 30
name CS
```

Assign VLANs:

```bash
interface range fa0/2-4
switchport mode access
switchport access vlan 30
exit

interface range fa0/5-7
switchport mode access
switchport access vlan 20
exit

interface range fa0/8-10
switchport mode access
switchport access vlan 10
exit
```

Configure trunk:

```bash
interface fa0/1
switchport mode trunk
exit

do write
```

---

## Step 4: Configure Router-on-a-Stick

Configure subinterfaces:

```bash
interface gig0/0.10
encapsulation dot1Q 10
ip address 192.168.1.1 255.255.255.192

interface gig0/0.20
encapsulation dot1Q 20
ip address 192.168.1.65 255.255.255.192

interface gig0/0.30
encapsulation dot1Q 30
ip address 192.168.1.129 255.255.255.192

do write
```

---

## Step 5: Configure End Devices

### VLAN 10 - IT

| Device       | IP Address  | Gateway     |
| ------------ | ----------- | ----------- |
| PC           | 192.168.1.2 | 192.168.1.1 |
| Printer      | 192.168.1.3 | 192.168.1.1 |
| Access point | ----------- | ----------- |

**AP Wi-Fi Settings**

* SSID: `Admin-WiFi`
* Security: WPA2-PSK
* Password: `admin@123`

---

### VLAN 20 - HR

| Device       | IP Address   | Gateway      |
| ------------ | ------------ | ------------ |
| PC           | 192.168.1.67 | 192.168.1.65 |
| Printer      | 192.168.1.66 | 192.168.1.65 |
| Access point | ------------ | ------------ |

**AP Wi-Fi Settings**

* SSID: `HR-WiFi`
* Security: WPA2-PSK
* Password: `hrdep@123`

---

### VLAN 30 - Customer Service

| Device       | IP Address    | Gateway       |
| ------------ | ------------- | ------------- |
| PC           | 192.168.1.131 | 192.168.1.129 |
| Printer      | 192.168.1.130 | 192.168.1.129 |
| Access point | ------------- | ------------- |

**AP Wi-Fi Settings**

* SSID: `CS-WiFi`
* Security: WPA2-PSK
* Password: `customer@123`

---

## Step 6: Connect Wireless Devices

| Department | SSID         | Password     |
| ---------- | ------------ | ------------ |
| IT         | Admin-WiFi   | admin@123    |
| HR         | HR-WiFi      | hrdep@123       |
| CS         | CS-WiFi      | customer@123 |

Connect:

* Laptop
* Smartphone

to their department's Wi-Fi network.

---

## Step 7: Connectivity Testing

Same VLAN:

```bash
ping 192.168.1.3
ping 192.168.1.66
ping 192.168.1.130
```

Inter-VLAN:

```bash
ping 192.168.1.67
ping 192.168.1.131
ping 192.168.1.2
```

Wireless Devices:

```bash
ping 192.168.1.2
ping 192.168.1.67
ping 192.168.1.131
```

Successful replies confirm proper VLAN communication and inter-VLAN routing.

---

## Verification Commands

### Switch

```bash
show vlan brief
show interfaces trunk
show running-config
```

### Router

```bash
show ip interface brief
show running-config
show ip route
```

---

## Expected Results

✅ VLANs created successfully

✅ Switch ports assigned correctly

✅ Trunk link operational

✅ Router subinterfaces configured

✅ Wireless devices connected

✅ Inter-VLAN routing working

✅ Successful ping between all departments

---

## Technologies Used

* Cisco Packet Tracer
* VLANs
* IEEE 802.1Q Trunking
* Router-on-a-Stick
* IPv4 Subnetting
* Wireless Networking
* Network Troubleshooting
