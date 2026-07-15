# 📡 LAB 75 - Wireless LAN Controller (Method 3) Configuration using Cisco Packet Tracer

## 📖 Overview

This lab demonstrates an **Enterprise Wireless LAN (WLC) deployment** using a **Cisco WLC-3504**, **Cisco 3702i Lightweight Access Points**, **multiple VLANs**, **Router-on-a-Stick**, **DHCP Relay (IP Helper Address)**, **802.1Q Trunking**, and **FlexConnect Local Switching**.

Unlike the previous WLC labs, this implementation introduces complete **department-based wireless segmentation** where each SSID is mapped to its own VLAN and subnet while being centrally managed through the Wireless LAN Controller.

> 💡 **This lab builds on the concepts from [LAB 63 - Wireless LAN Controller (Method 1)](../LAB63/README.md) and [LAB 74 - Wireless LAN Controller (Method 2)](../LAB74/README.md).**  
> This method introduces **multiple VLANs, Router-on-a-Stick, DHCP Relay (IP Helper Address), WLC Interfaces, FlexConnect Local Switching, and enterprise wireless segmentation.**

---

# 🖥️ Devices Used

| Device | Quantity |
|---------|---------:|
| Cisco Router 2911 | 1 |
| Cisco Switch 2960-24TT | 1 |
| Cisco WLC-3504 | 1 |
| Cisco 3702i Lightweight AP | 4 |
| DHCP Server | 1 |
| Management PC | 1 |
| Tablet PCs | 4 |

---

# 🗺️ Network Topology

| Department | VLAN | Network |
|------------|------|----------------|
| IT | VLAN 10 | 192.168.10.0/24 |
| HR | VLAN 20 | 192.168.20.0/24 |
| FIN | VLAN 30 | 192.168.30.0/24 |
| Management | VLAN 50 | 192.168.50.0/24 |
| DHCP Server | - | 192.168.1.0/24 |

---

# ✨ Features

- 🌐 Router-on-a-Stick
- 🔀 Multiple VLANs
- 🏢 Department-based Wireless Networks
- 📡 Cisco WLC-3504 Configuration
- 📶 Cisco 3702i Lightweight AP Deployment
- 🔒 WPA2-Personal Security
- 📋 DHCP Relay (IP Helper Address)
- ⚡ FlexConnect Local Switching
- 📱 Multiple Wireless Clients
- 🖧 802.1Q Trunking
- 🎯 Enterprise Wireless Network Design

---

# 📍 IP Addressing

## Router

| Interface | IP Address |
|-----------|------------|
| G0/0 | 192.168.1.1/24 |
| G0/1.10 | 192.168.10.1/24 |
| G0/1.20 | 192.168.20.1/24 |
| G0/1.30 | 192.168.30.1/24 |
| G0/1.50 | 192.168.50.1/24 |

---

## DHCP Server

| Parameter | Value |
|-----------|-------|
| IP Address | 192.168.1.5 |
| Gateway | 192.168.1.1 |
| DNS | 192.168.1.5 |

---

## Wireless LAN Controller

| Parameter | Value |
|-----------|-------|
| IP Address | 192.168.50.5 |
| Gateway | 192.168.50.1 |
| Management VLAN | 50 |

---

## Management PC

| Parameter | Value |
|-----------|-------|
| IP Address | 192.168.50.6 |
| Gateway | 192.168.50.1 |

---

# 🔀 VLAN Configuration

| VLAN | Name |
|------|------|
| 10 | IT |
| 20 | HR |
| 30 | FIN |
| 50 | Management |

All switch ports connected to:

- Router
- WLC
- Access Points

were configured as **802.1Q trunk ports**.

---

# 📋 DHCP Configuration

The DHCP server provides separate address pools for:

- ✅ ITPOOL
- ✅ HRPOOL
- ✅ FINPOOL
- ✅ MGTPOOL

Each pool contains:

- Default Gateway
- DNS Server
- Start IP Address
- Subnet Mask
- WLC Address

---

# 📡 Wireless Networks

| Profile | SSID | VLAN |
|----------|------|------|
| IT-WIFI | IT | 10 |
| HR-WIFI | HR | 20 |
| FIN-WIFI | FIN | 30 |
| MGT-WIFI | MGT | 50 |

Security

- WPA2-Personal
- PSK Authentication

Password

```
12345678
```

---

# ⚙️ Configuration Summary

## Step 1

- Configure VLANs
- Configure trunk ports
- Set native VLAN

---

## Step 2

Configure Router-on-a-Stick

- Dot1Q Subinterfaces
- VLAN Interfaces
- Default Gateways
- IP Helper Address

---

## Step 3

Configure DHCP Server

- Four DHCP Pools
- DNS
- Gateway
- WLC Address

---

## Step 4

Configure WLC Management Interface

- Management IP
- Gateway
- DNS
- VLAN 50

---

## Step 5

Create WLC Interfaces

- VLAN10
- VLAN20
- VLAN30

Assign

- VLAN ID
- Gateway
- DHCP Server
- IP Address

---

## Step 6

Create WLANs

- IT
- HR
- FIN
- MGT

Enable

- WPA2
- PSK

---

## Step 7

Configure FlexConnect

Enable

- ✅ Local Switching
- ✅ Local Authentication

---

## Step 8

Configure Lightweight APs

- Power On
- DHCP Mode
- Join WLC Automatically

---

## Step 9

Connect Wireless Clients

Configure each Tablet PC with the appropriate SSID.

| Device | SSID |
|--------|------|
| Tablet PC0 | IT |
| Tablet PC1 | HR |
| Tablet PC2 | FIN |
| Tablet PC3 | MGT |

---

# 🧪 Verification

✔ Ping Management PC → WLC

✔ Verify WLC GUI Access

✔ Verify AP Registration

✔ Verify Controller Interfaces

✔ Verify WLAN Status

✔ Verify DHCP Address Assignment

✔ Verify Wireless Client Connectivity

✔ Verify Correct VLAN Mapping

✔ Verify FlexConnect Operation

---

# 🎯 Skills Learned

- Cisco WLC-3504 Administration
- Cisco Lightweight Access Points
- Enterprise Wireless LAN Deployment
- Router-on-a-Stick
- 802.1Q Trunking
- VLAN Segmentation
- DHCP Relay (IP Helper Address)
- WPA2 Wireless Security
- Cisco WLAN Profiles
- FlexConnect Local Switching
- Enterprise Wireless Design

---

# 🛠️ Technologies Used

- Cisco Packet Tracer
- Cisco IOS
- Cisco WLC-3504
- Cisco 3702i Lightweight AP
- DHCP
- VLAN
- 802.1Q
- Router-on-a-Stick
- FlexConnect
- WPA2-Personal

---

# 🏁 Result

✅ Successfully configured a Cisco WLC-3504 to centrally manage multiple Lightweight Access Points.

✅ Implemented department-based wireless segmentation using VLANs.

✅ Configured Router-on-a-Stick with 802.1Q subinterfaces.

✅ Implemented DHCP Relay (IP Helper Address) for all wireless VLANs.

✅ Created multiple secure WLANs mapped to individual VLANs.

✅ Enabled FlexConnect Local Switching for enterprise wireless deployment.

✅ Verified successful AP registration, DHCP assignment, and wireless connectivity across all departments.
