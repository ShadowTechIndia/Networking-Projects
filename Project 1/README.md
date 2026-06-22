# Basic Subnetting Lab

## Overview

This lab demonstrates how to create a small network using subnetting. The network is divided into two departments:

* Accounts Department
* Delivery Department

A router is used to connect both departments and allow communication between them.

---

# Network Requirements

## Devices

* 1 Router
* 2 Switches
* 4 PCs
* 2 Printers

## Switch Names

| Switch   | Hostname |
| -------- | -------- |
| Switch 1 | ACCOUNT  |
| Switch 2 | DELIVERY |

---

# Network Topology

## Accounts Department

* Router Gig0/0
* ACCOUNT Switch
* PC1
* PC2
* Printer1

## Delivery Department

* Router Gig0/1
* DELIVERY Switch
* PC3
* PC4
* Printer2

---

# Step 1: Connect All Devices

Use Copper Straight-Through cables to connect the devices.

## Accounts Department Connections

| Device A      | Device B       |
| ------------- | -------------- |
| Router Gig0/0 | ACCOUNT Switch |
| PC1           | ACCOUNT Switch |
| PC2           | ACCOUNT Switch |
| Printer1      | ACCOUNT Switch |

## Delivery Department Connections

| Device A      | Device B        |
| ------------- | --------------- |
| Router Gig0/1 | DELIVERY Switch |
| PC3           | DELIVERY Switch |
| PC4           | DELIVERY Switch |
| Printer2      | DELIVERY Switch |


# Step 2: Subnetting

## Network Address

```text
192.168.1.0/24
```

## Subnet Mask

```text
255.255.255.128
```

## CIDR Notation

```text
/25
```

The network is divided into two subnets.

| Department | Network Address  | Host Range                    | Broadcast Address |
| ---------- | ---------------- | ----------------------------- | ----------------- |
| Accounts   | 192.168.1.0/25   | 192.168.1.1 - 192.168.1.126   | 192.168.1.127     |
| Delivery   | 192.168.1.128/25 | 192.168.1.129 - 192.168.1.254 | 192.168.1.255     |

---

# Step 3: Configure Router Interfaces

Assign IP addresses to the router interfaces.

## Router Configuration

```bash
enable
configure terminal

interface gig0/0
ip address 192.168.1.1 255.255.255.128
no shutdown
exit

interface gig0/1
ip address 192.168.1.129 255.255.255.128
no shutdown
exit

do write
```

## Router Interface Summary

| Interface | IP Address    | Department |
| --------- | ------------- | ---------- |
| Gig0/0    | 192.168.1.1   | Accounts   |
| Gig0/1    | 192.168.1.129 | Delivery   |

---

# Step 4: Assign Static IP Addresses

Configure the following IP addresses manually on all PCs and printers.

## Accounts Department

| Device   | IP Address  | Subnet Mask     | Default Gateway |
| -------- | ----------- | --------------- | --------------- |
| PC1      | 192.168.1.2 | 255.255.255.128 | 192.168.1.1     |
| PC2      | 192.168.1.3 | 255.255.255.128 | 192.168.1.1     |
| Printer1 | 192.168.1.4 | 255.255.255.128 | 192.168.1.1     |

## Delivery Department

| Device   | IP Address    | Subnet Mask     | Default Gateway |
| -------- | ------------- | --------------- | --------------- |
| PC3      | 192.168.1.130 | 255.255.255.128 | 192.168.1.129   |
| PC4      | 192.168.1.131 | 255.255.255.128 | 192.168.1.129   |
| Printer2 | 192.168.1.132 | 255.255.255.128 | 192.168.1.129   |

---

# Step 5: Test Connectivity

Use the ping command to verify communication between devices.

## Test 1: Ping Within the Same Department

From PC1:

```bash
ping 192.168.1.3
```

Expected Result:

```text
Reply from 192.168.1.3
```

## Test 2: Ping Between Departments

From PC1:

```bash
ping 192.168.1.130
```

Expected Result:

```text
Reply from 192.168.1.130
```

## Test 3: Ping a Printer

From PC1:

```bash
ping 192.168.1.132
```

Expected Result:

```text
Reply from 192.168.1.132
```

---

# Verification Checklist

* [ ] All devices connected correctly
* [ ] ACCOUNT switch configured
* [ ] DELIVERY switch configured
* [ ] Router interfaces configured
* [ ] Static IP addresses assigned
* [ ] Default gateways configured
* [ ] Devices can ping within the same subnet
* [ ] Devices can ping across subnets
* [ ] Printers are reachable

---

# Conclusion

This project demonstrates:

* Network cabling and device connections
* Switch configuration
* Router configuration
* IPv4 subnetting using /25
* Static IP address assignment
* Inter-network communication
* Network troubleshooting using ping

If all ping tests are successful, the Basic Subnetting Lab has been completed successfully.
