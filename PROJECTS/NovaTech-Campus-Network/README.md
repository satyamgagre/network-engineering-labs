# NovaTech University Campus Network — Cisco Packet Tracer Enterprise Lab

![Cisco](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?style=flat-square&logo=cisco&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square)
![VLANs](https://img.shields.io/badge/VLANs-10-blue?style=flat-square)
![Routing](https://img.shields.io/badge/Routing-RIPv2-orange?style=flat-square)
![Campuses](https://img.shields.io/badge/Campuses-2-informational?style=flat-square)
![License](https://img.shields.io/badge/License-Educational%20Use-lightgrey?style=flat-square)

---

## 📖 Project Overview

This repository contains a complete **enterprise campus network design and implementation** for **NovaTech University**, built and simulated in **Cisco Packet Tracer**. NovaTech operates across **two physical campuses located roughly 20 miles apart**, connected through a **cloud router (transit/WAN simulation)** using dedicated **Serial DCE links**.

The design follows a hierarchical, department-segmented approach: every academic faculty and administrative department is placed on its own **VLAN and IP subnet**, with **Router-on-a-Stick inter-VLAN routing**, **router-based DHCP services**, and **RIPv2 dynamic routing** tying the whole enterprise together. A cloud-hosted **Email Server** simulates an external/ISP-facing service reachable through the transit router.

This README documents the project **exactly as configured**, step-by-step, in the order the network was built — from hardware setup through final connectivity testing — so that a beginner can reproduce the entire lab from scratch without needing any external video or reference material.


---

## 🎯 Project Objectives

- Design a complete enterprise campus network topology for a two-campus university.
- Build a scalable, conflict-free IP addressing scheme across 10 subnets.
- Segment departmental traffic using VLANs on access-layer switches.
- Implement **Router-on-a-Stick** inter-VLAN routing on Layer 3 gateways.
- Deploy **router-based DHCP** for automatic client addressing.
- Implement **RIPv2** for dynamic routing between campuses and the cloud router.
- Provide connectivity to an externally hosted (cloud) Email Server.
- Simulate, verify, and troubleshoot the full solution end-to-end in Packet Tracer.

---

## ✨ Project Features

| Feature | Description |
|---|---|
| Multi-building VLAN segmentation | 8 VLANs on the Main Campus, 2 VLANs on the Branch Campus |
| Router-on-a-Stick | Sub-interface based inter-VLAN routing on both campus routers |
| Centralized DHCP | Router-based DHCP pools for every departmental subnet |
| Dynamic Routing | RIPv2 across Main Campus, Branch Campus, and Cloud Router |
| WAN Simulation | Serial DCE/DTE links with manually configured clock rates |
| External Connectivity | Cloud-hosted Email Server reachable via the Cloud Router |
| Modular Switch Design | 2960-24TT access switches + 3650-24PS Layer 3 distribution switches |
| End-to-End Testing | DHCP verification and inter-VLAN/inter-campus ping testing |

---

## 🧰 Network Technologies Used

| Technology | Purpose |
|---|---|
| VLAN (802.1Q) | Logical segmentation of departmental traffic |
| Trunking (dot1Q) | Carrying multiple VLANs over a single switch-to-router/switch link |
| Router-on-a-Stick | Inter-VLAN routing using sub-interfaces |
| DHCP (IOS DHCP Server) | Automatic IP assignment to end devices |
| RIPv2 | Dynamic interior routing protocol |
| Serial WAN (DCE/DTE) | Point-to-point WAN links between routers |
| HWIC-2T Module | Adds serial interfaces to 2911 routers |

---

## 🏫 Project Scenario

NovaTech University is a large higher-education institution serving students, faculty, administrative staff, and IT personnel across two campuses. The university requires a secure, scalable network that allows every department to communicate internally and across campuses, while keeping departmental traffic logically isolated.

As the Network Engineer for this project, the following was designed and implemented:

- A **Main Campus** with three buildings (Administration/Business, Engineering/Creative, IT/Student Labs).
- A **Branch (Secondary) Campus** hosting the Faculty of Health and Sciences, split into Staff and Student floors.
- A **Cloud Router** acting as a transit point to an externally hosted Email Server.


> ⚠ **Warning:** Physical HWIC-2T serial modules must be added to all three 2911 routers (Main Campus, Cloud, Branch Campus) **before** cabling. This requires powering the router **off** before dragging the module in.

---

## 🏗 Campus Layout

### Main Campus

| Building | Departments / Faculties | Switch Model |
|---|---|---|
| Building A – Administration & Business | Admin, HR, Finance, Business | 2960-24TT (x4) |
| Building B – Engineering & Creative | Engineering & Computing (E&C), Art & Design (A&D) | 2960-24TT (x2) |
| Building C – IT Services & Student Labs | Student Labs, IT Dept, Web Server, FTP Server | 2960-24TT (x2) |

### Branch (Secondary) Campus

| Floor | Department | Switch Model |
|---|---|---|
| Staff Floor | Staff Department | 2960-24TT |
| Student Floor | Student Labs | 2960-24TT |

---

## 🖥 Device Inventory

| # | Device Name | Model | Role | Campus |
|---|---|---|---|---|
| 1 | Main Campus Router | Cisco 2911 (+HWIC-2T) | Gateway / Router-on-a-Stick / DHCP / RIP | Main |
| 2 | Cloud Router | Cisco 2911 (+HWIC-2T) | WAN Transit / ISP Simulation | Cloud |
| 3 | Branch Campus Router | Cisco 2911 (+HWIC-2T) | Gateway / Router-on-a-Stick / DHCP / RIP | Branch |
| 4 | Main Campus L3 Switch | Catalyst 3650-24PS | Distribution / Trunk Aggregation | Main |
| 5 | Branch Campus L3 Switch | Catalyst 3650-24PS | Distribution / Trunk Aggregation | Branch |
| 6 | Admin Switch | Catalyst 2960-24TT | Access – VLAN 10 | Main |
| 7 | HR Switch | Catalyst 2960-24TT | Access – VLAN 20 | Main |
| 8 | Finance Switch | Catalyst 2960-24TT | Access – VLAN 30 | Main |
| 9 | Business Switch | Catalyst 2960-24TT | Access – VLAN 40 | Main |
| 10 | E&C Switch | Catalyst 2960-24TT | Access – VLAN 50 | Main |
| 11 | A&D Switch | Catalyst 2960-24TT | Access – VLAN 60 | Main |
| 12 | Student Labs Switch (Main) | Catalyst 2960-24TT | Access – VLAN 70 | Main |
| 13 | IT Dept Switch | Catalyst 2960-24TT | Access – VLAN 80 | Main |
| 14 | Staff-Dept Switch | Catalyst 2960-24TT | Access – VLAN 90 | Branch |
| 15 | Student Labs Switch (Branch) | Catalyst 2960-24TT | Access – VLAN 100 | Branch |
| 16 | Email Server | Generic Server | External Cloud Service | Cloud |
| 17 | Web Server | Generic Server | Internal University Service | Main (Building C) |
| 18 | FTP Server | Generic Server | Internal University Service | Main (Building C) |

---

## 🔧 Hardware Requirements

- 3 × Cisco 2911 Routers
- 2 × Cisco Catalyst 3650-24PS Layer 3 Switches
- 10 × Cisco Catalyst 2960-24TT Layer 2 Switches
- 3 × HWIC-2T Serial WIC Modules
- Host PCs and Printers for each department
- 3 × Generic Servers (Email, Web, FTP)
- Copper Straight-Through cables
- Serial DCE/DTE cables (WAN links)

## 💻 Software Requirements

- Cisco Packet Tracer (8.x or later recommended)
- Cisco IOS images bundled with Packet Tracer's 2911 and 3650/2960 device templates


---
## 🗺 Network Topology

![NovaTech Network Topology](NovaTech-Campus-Network/PROJECT-TOPOLOGY.png)

---

## 🧮 IP Addressing Plan

### LAN Subnets

| Network / VLAN | Subnet | Usable Range | Gateway | Broadcast |
|---|---|---|---|---|
| VLAN 10 – Admin | 192.168.10.0/24 | .1 – .254 | 192.168.10.1 | 192.168.10.255 |
| VLAN 20 – HR | 192.168.20.0/24 | .1 – .254 | 192.168.20.1 | 192.168.20.255 |
| VLAN 30 – Finance | 192.168.30.0/24 | .1 – .254 | 192.168.30.1 | 192.168.30.255 |
| VLAN 40 – Business | 192.168.40.0/24 | .1 – .254 | 192.168.40.1 | 192.168.40.255 |
| VLAN 50 – E&C | 192.168.50.0/24 | .1 – .254 | 192.168.50.1 | 192.168.50.255 |
| VLAN 60 – A&D | 192.168.60.0/24 | .1 – .254 | 192.168.60.1 | 192.168.60.255 |
| VLAN 70 – Student Labs | 192.168.70.0/24 | .1 – .254 | 192.168.70.1 | 192.168.70.255 |
| VLAN 80 – IT Dept | 192.168.80.0/24 | .1 – .254 | 192.168.80.1 | 192.168.80.255 |
| VLAN 90 – Staff Dept | 192.168.90.0/24 | .1 – .254 | 192.168.90.1 | 192.168.90.255 |
| VLAN 100 – Student Labs (Branch) | 192.168.100.0/24 | .1 – .254 | 192.168.100.1 | 192.168.100.255 |

### WAN / Point-to-Point Links

| Link | Subnet | Device A | IP A | Device B | IP B | DCE (Clock Rate) |
|---|---|---|---|---|---|---|
| Main Campus ↔ Cloud Router | 10.10.10.4/30 | Main Campus Rtr (Se0/3/0) | 10.10.10.5 | Cloud Router (Se0/3/0) | 10.10.10.6 | Cloud Router |
| Main Campus ↔ Branch Campus | 10.10.10.0/30 | Main Campus Rtr (Se0/3/1) | 10.10.10.1 | Branch Campus Rtr (Se0/3/0) | 10.10.10.2 | Branch Campus Router |
| Cloud Router ↔ Email Server | 20.0.0.0/30 | Cloud Router (Gi0/0) | 20.0.0.1 | Email Server | 20.0.0.2 | N/A (Ethernet) |

> ⚠ **Warning:** The `/30` masks used here (`255.255.255.252`) allow only **2 usable host addresses**. Double-check IPs before applying — a typo will silently break the WAN link.

---

## 🏷 VLAN Allocation Table

| VLAN ID | Name | Building/Floor | Switch | Subnet |
|---|---|---|---|---|
| 10 | ADMIN | Building A | Admin Switch | 192.168.10.0/24 |
| 20 | HR | Building A | HR Switch | 192.168.20.0/24 |
| 30 | FINANCE | Building A | Finance Switch | 192.168.30.0/24 |
| 40 | BUSINESS | Building A | Business Switch | 192.168.40.0/24 |
| 50 | E&C | Building B | E&C Switch | 192.168.50.0/24 |
| 60 | A&D | Building B | A&D Switch | 192.168.60.0/24 |
| 70 | STUD-LABS | Building C | Student Labs Switch | 192.168.70.0/24 |
| 80 | IT-DEPT | Building C | IT Dept Switch | 192.168.80.0/24 |
| 90 | STAFF-DEPT | Branch – Staff Floor | Staff-Dept Switch | 192.168.90.0/24 |
| 100 | STUD-LB | Branch – Student Floor | Stud-LB Switch | 192.168.100.0/24 |

---

## 🔗 Router Interface Summary

| Router | Interface | Description | IP Address |
|---|---|---|---|
| Main Campus Router | Gi0/0 | Trunk to Main Campus L3 Switch | (sub-interfaces only) |
| Main Campus Router | Se0/3/0 | WAN to Cloud Router | 10.10.10.5/30 |
| Main Campus Router | Se0/3/1 | WAN to Branch Campus Router | 10.10.10.1/30 |
| Main Campus Router | Gi0/0.10–.80 | Router-on-a-Stick sub-interfaces | 192.168.10.1 – 192.168.80.1 |
| Cloud Router | Se0/3/0 | WAN to Main Campus Router (DCE) | 10.10.10.6/30 |
| Cloud Router | Gi0/0 | LAN to Email Server | 20.0.0.1/30 |
| Branch Campus Router | Se0/3/0 | WAN to Main Campus Router (DCE) | 10.10.10.2/30 |
| Branch Campus Router | Gi0/0 | Trunk to Branch Campus L3 Switch | (sub-interfaces only) |
| Branch Campus Router | Gi0/0.90 / .100 | Router-on-a-Stick sub-interfaces | 192.168.90.1 / 192.168.100.1 |

---

## 🖨 Server Addresses

| Server | Location | IP Address | Subnet Mask | Gateway |
|---|---|---|---|---|
| Email Server | Cloud (external) | 20.0.0.2 | 255.255.255.252 | 20.0.0.1 |
| Web Server | Building C, Main Campus | DHCP or static (IT-DEPT VLAN 80) | 255.255.255.0 | 192.168.80.1 |
| FTP Server | Building C, Main Campus | DHCP or static (IT-DEPT VLAN 80) | 255.255.255.0 | 192.168.80.1 |


---

## 🗂 Department Mapping

| Department/Faculty | Building | VLAN | Devices |
|---|---|---|---|
| Administration | A | 10 | PC + Printer |
| Human Resources | A | 20 | PC + Printer |
| Finance | A | 30 | PC + Printer |
| Business | A | 40 | PC + Printer |
| Engineering & Computing | B | 50 | PC + Printer |
| Art & Design | B | 60 | PC + Printer |
| Student Labs (Main) | C | 70 | PC + Printer |
| IT Department | C | 80 | PC only + Web/FTP servers |
| Staff Department | Branch | 90 | PC + Printer |
| Student Labs (Branch) | Branch | 100 | PC + Printer |

---

## 🧱 Technology Stack

| Layer | Technology |
|---|---|
| Access Layer | Cisco Catalyst 2960-24TT switches, VLAN access ports |
| Distribution Layer | Cisco Catalyst 3650-24PS Layer 3 switches, 802.1Q trunks |
| Core / WAN | Cisco 2911 Routers, HWIC-2T serial modules, Router-on-a-Stick |
| Services | IOS DHCP Server, RIPv2 |
| Simulation Tool | Cisco Packet Tracer |

---

## ⚙ Configuration Workflow

The network was built in the following order. Each stage below documents the exact commands used, an explanation of what they do, verification steps, expected results, and common troubleshooting tips.

### Prerequisites

- Cisco Packet Tracer installed and a new blank project created.
- Familiarity with basic Cisco IOS CLI navigation (`enable`, `configure terminal`, `interface`, `exit`).
- All devices (3 routers, 2 L3 switches, 10 access switches, PCs, printers, 3 servers) placed on the canvas per the [Device Inventory](#-device-inventory).

---

### 1. Hardware Setup

**Introduction:** Before any cabling, each 2911 router needs a serial WIC module installed, since the base 2911 chassis ships without serial ports.

#### 2. Installing HWIC-2T Modules

> ⚠ **Warning:** The router **must be powered off** before adding or removing physical modules. Adding a module to a running router in Packet Tracer will not work.

**Steps (repeated for Main Campus Router, Cloud Router, and Branch Campus Router):**

1. Click the router → go to the **Physical** tab.
2. Click the **power switch** to turn the router **off**.
3. Drag the **HWIC-2T** module from the module tray into a blank slot on the router.
4. Click the **power switch** again to turn the router back **on**.

**Explanation:** The HWIC-2T module adds two additional serial interfaces (`Serial0/3/0` and `Serial0/3/1`), which are required for the WAN links between Main Campus ↔ Cloud Router and Main Campus ↔ Branch Campus Router.

✅ **Verification:** Return to the **CLI** tab and run `show ip interface brief`. The new `Serial0/3/0` and `Serial0/3/1` interfaces should now appear in the output.

🛠 **Troubleshooting:** If the serial interfaces do not appear, confirm the router was fully powered off before the module was dragged in, and that the module snapped into a valid slot (green highlight in Packet Tracer).

---

### 3. Power On Devices

Turn on the **Main Campus L3 Switch** and **Branch Campus L3 Switch** by dragging and dropping the **AC power supply module** into each 3650's power slot (these switches ship without a power supply pre-installed in Packet Tracer).

✅ **Verification:** The switch's power LED turns green and the CLI becomes accessible.

---

### 4. Cable Connections

Connect devices using the following cable types:

| Connection | Cable Type |
|---|---|
| Router ↔ Router (WAN) | Serial DCE cable |
| Router ↔ L3 Switch | Copper Straight-Through (trunk) |
| L3 Switch ↔ Access Switch | Copper Straight-Through (trunk) |
| Access Switch ↔ PC/Printer | Copper Straight-Through |
| Cloud Router ↔ Email Server | Copper Straight-Through |

> 💡 **Note:** For the Serial DCE cable, the **DCE end** (the end that provides clock signaling) determines which router needs the `clock rate` command. Hover over each serial interface in Packet Tracer to identify which end shows the **clock icon** — that is the DCE side.

---

### 5. Enable Interfaces

**Introduction:** By default, router physical interfaces are administratively shut down. Before any addressing or routing can work, the interfaces used in this topology must be brought up on all three routers.

**Commands — Main Campus Router:**

```cisco
enable
conf t
interface gig0/0
no shutdown
exit

interface se0/3/0
no shutdown
exit

interface se0/3/1
no shutdown
exit
do wr
```

**Commands — Cloud Router:**

```cisco
enable
conf t
interface gig0/0
no shutdown
exit

interface se0/3/0
no shutdown
exit
do wr
```

**Commands — Branch Campus Router:**

```cisco
enable
conf t
interface gig0/0
no shutdown
exit

interface se0/3/0
no shutdown
exit
do wr
```

**Explanation:**
- `enable` — enters privileged EXEC mode.
- `conf t` — enters global configuration mode.
- `interface <name>` — selects the interface to configure.
- `no shutdown` — administratively enables the interface (interfaces are shut down by default).
- `do wr` — saves the running-config to the startup-config without leaving config mode.

✅ **Verification Commands:**

```cisco
show ip interface brief
show interfaces
```

**Expected Output:** All listed interfaces (`GigabitEthernet0/0`, `Serial0/3/0`, `Serial0/3/1`) should show status **up/up** once cabled and clock rates are set (serial links stay `down/down` until both ends are configured and clocked).

🛠 **Common Mistakes / Troubleshooting:**
- Forgetting `no shutdown` is the #1 cause of "interface is administratively down" errors.
- Serial interfaces will show `up/down` (line protocol down) until the DCE side has a `clock rate` configured — this is expected at this stage and resolved in the next step.

---

### 6. Configure Clock Rate

**Introduction:** On a serial WAN link, the **DCE** end must supply clocking for the **DTE** end to synchronize. Without this, the serial line stays down even after `no shutdown`.

**Commands — Cloud Router** *(DCE end of the Main Campus ↔ Cloud Router link)*:

```cisco
interface serial 0/3/0
clock rate 64000
exit
do wr
```

**Commands — Branch Campus Router** *(DCE end of the Main Campus ↔ Branch Campus link)*:

```cisco
interface serial 0/3/0
clock rate 64000
exit
do wr
```

**Explanation:** `clock rate 64000` sets a 64 kbps clock signal on the DCE interface. Only the DCE side needs this command — the DTE side derives timing automatically.

✅ **Verification Commands:**

```cisco
show controllers serial 0/3/0
show interfaces serial 0/3/0
```

**Expected Output:** `show controllers` should report `DCE` for the cable type. `show interfaces` should transition to **up/up (line protocol is up)** on both ends of each serial link.

🛠 **Troubleshooting:**
- If the line protocol stays down, verify the cable is a **Serial DCE** cable (not a plain serial cable) and that IP addresses (configured later) match on both ends of the link.
- Mismatched clock rates or missing `clock rate` on the DCE side are the most common causes of a down serial line.

---

### 7. Configure Access Switches

**Introduction:** Each departmental access switch is configured with its own VLAN, and **all** access ports are placed into that VLAN so every connected PC/printer lands on the correct subnet.

#### Admin Switch (VLAN 10)

```cisco
enable
conf t
vlan 10
name ADMIN
exit
interface range fastethernet 0/1-24
switchport mode access
switchport access vlan 10
exit
do wr
```

#### HR Switch (VLAN 20)

```cisco
enable
conf t
vlan 20
name HR
exit
interface range fastethernet 0/1-24
switchport mode access
switchport access vlan 20
exit
do wr
```

#### Finance Switch (VLAN 30)

```cisco
enable
conf t
vlan 30
name FINANCE
exit
interface range fastethernet 0/1-24
switchport mode access
switchport access vlan 30
exit
do wr
```

#### Business Switch (VLAN 40)

```cisco
enable
conf t
vlan 40
name BUSINESS
exit
interface range fastethernet 0/1-24
switchport mode access
switchport access vlan 40
exit
do wr
```

#### Engineering & Computing Switch (VLAN 50)

```cisco
enable
conf t
vlan 50
name E&C
exit
interface range fastethernet 0/1-24
switchport mode access
switchport access vlan 50
exit
do wr
```

#### Art & Design Switch (VLAN 60)

```cisco
enable
conf t
vlan 60
name A&D
exit
interface range fastethernet 0/1-24
switchport mode access
switchport access vlan 60
exit
do wr
```

#### Student Labs Switch – Main Campus (VLAN 70)

```cisco
enable
conf t
vlan 70
name STUD-LABS
exit
interface range fastethernet 0/1-24
switchport mode access
switchport access vlan 70
exit
do wr
```

#### IT Dept Switch (VLAN 80)

```cisco
enable
conf t
vlan 80
name IT-DEPT
exit
interface range fastethernet 0/1-24
switchport mode access
switchport access vlan 80
exit
do wr
```

#### Staff-Dept Switch – Branch Campus (VLAN 90)

```cisco
enable
conf t
vlan 90
name STAFF-DEPT
exit
interface range fastethernet 0/1-24
switchport mode access
switchport access vlan 90
exit
do wr
```

#### Student Labs Switch – Branch Campus (VLAN 100)

```cisco
enable
conf t
vlan 100
name STUD-LB
exit
interface range fastethernet 0/1-24
switchport mode access
switchport access vlan 100
exit
do wr
```

**Explanation:**
- `vlan <id>` / `name <name>` — creates the VLAN and gives it a friendly name.
- `interface range fastethernet 0/1-24` — selects all 24 FastEthernet ports in one command.
- `switchport mode access` — sets the port to access mode (single VLAN, connects to end devices).
- `switchport access vlan <id>` — assigns the VLAN to those ports.

✅ **Verification Commands:**

```cisco
show vlan brief
show interfaces status
show running-config
```

**Expected Output:** `show vlan brief` should list each VLAN with all `Fa0/1–0/24` ports assigned to it (except any port later converted to a trunk).

🛠 **Common Mistakes:**
- Forgetting `range` and trying to configure 24 ports one at a time.
- Configuring `switchport access vlan` **before** creating the VLAN — the VLAN must exist first (Packet Tracer usually auto-creates it, but best practice is VLAN-first).

🛠 **Troubleshooting:** If a port shows the wrong VLAN in `show vlan brief`, re-enter `switchport access vlan <id>` directly on that interface.

---

### 8. Configure Layer 3 Switches

**Introduction:** The Main Campus and Branch Campus 3650 Layer 3 switches act as **distribution switches** — they don't route between VLANs themselves in this design (that's handled by Router-on-a-Stick), but they must know about every VLAN and forward the correct VLAN to the correct downstream access switch or uplink.

**Commands — Main Campus L3 Switch:**

```cisco
enable
conf t
vlan 10
name ADMIN
exit

vlan 20
name HR
exit

vlan 30
name FINANCE
exit

vlan 40
name BUSINESS
exit
 
vlan 50
name E&C
exit

vlan 60
name A&D
exit

vlan 70
name STUD-LABS
exit

vlan 80
name IT-DEPT
exit
```

```cisco
interface GigabitEthernet1/0/2
switchport mode access
switchport access vlan 10
exit

interface GigabitEthernet1/0/3
switchport mode access
switchport access vlan 20
exit

interface GigabitEthernet1/0/4
switchport mode access
switchport access vlan 30
exit

interface GigabitEthernet1/0/5
switchport mode access
switchport access vlan 40
exit

interface GigabitEthernet1/0/6
switchport mode access
switchport access vlan 50
exit

interface GigabitEthernet1/0/7
switchport mode access
switchport access vlan 60
exit

interface GigabitEthernet1/0/8
switchport mode access
switchport access vlan 70
exit

interface GigabitEthernet1/0/9
switchport mode access
switchport access vlan 80
exit
do wr
```


**Commands — Branch Campus L3 Switch:**

```cisco
enable
conf t
vlan 90
name STAFF-DEPT
exit

vlan 100
name STUD-LB
exit
do wr

interface GigabitEthernet1/0/2
switchport mode access
switchport access vlan 90
exit

interface GigabitEthernet1/0/3
switchport mode access
switchport access vlan 100
exit
do wr
```

**Explanation:** Each `GigabitEthernet1/0/x` port on the L3 switch faces one downstream access switch, and is set to access mode carrying that department's single VLAN — the trunk to the router is configured separately in the next section.

✅ **Verification Commands:**

```cisco
show vlan brief
show ip interface brief
show interfaces status
```

**Expected Output:** All eight (Main) / two (Branch) VLANs appear in `show vlan brief`, each with its designated downstream port.

🛠 **Troubleshooting:** If a downstream access switch cannot reach its gateway, confirm the L3 switch port facing it matches the same VLAN configured on the access switch itself.

---

### 9. Configure Trunk Links

**Introduction:** The uplink from each Layer 3 switch to its corresponding router must carry **all VLANs** simultaneously (802.1Q trunk), since a single router interface (via sub-interfaces) handles every department.

**Commands — Branch Campus L3 Switch (uplink to Branch Campus Router):**

```cisco
enable
conf t
interface GigabitEthernet1/0/1
switchport trunk encapsulation dot1Q
switchport mode trunk
exit
do wr
```

**Commands — Main Campus L3 Switch (uplink to Main Campus Router):**

```cisco
enable
conf t
interface GigabitEthernet1/0/1
switchport trunk encapsulation dot1Q
switchport mode trunk
exit
do wr
```


**Explanation:**
- `switchport trunk encapsulation dot1Q` — explicitly sets the trunking encapsulation to IEEE 802.1Q (only needed on switches that support multiple encapsulation types, e.g. ISL).
- `switchport mode trunk` — converts the port into a trunk, allowing all VLANs to pass by default.

✅ **Verification Commands:**

```cisco
show interfaces trunk
show interfaces GigabitEthernet1/0/1 switchport
```

**Expected Output:** `show interfaces trunk` should list `Gi1/0/1` as the trunk port with all configured VLANs in the "allowed" and "active" VLAN list.

🛠 **Troubleshooting:**
- If `show interfaces trunk` returns nothing, confirm the port is not still set to `switchport mode access`.
- If only some VLANs pass, check `switchport trunk allowed vlan` has not been restricted (by default all VLANs are allowed).

---

### 10. Assign Router IP Addresses

**Introduction:** With Layer 2/3 switching in place, the routers' WAN-facing serial interfaces need IP addresses to establish point-to-point Layer 3 connectivity between campuses and the cloud.

**Commands — Main Campus Router:**

```cisco
enable
conf t
interface serial 0/3/0
ip address 10.10.10.5 255.255.255.252
exit
do wr

interface serial 0/3/1
ip address 10.10.10.1 255.255.255.252
exit
do wr
```

**Commands — Cloud Router:**

```cisco
enable
conf t
interface serial 0/3/0
ip address 10.10.10.6 255.255.255.252
exit

interface gigaethernet 0/0
ip address 20.0.0.1 255.255.255.252
exit
do wr
```


**Commands — Branch Campus Router:**

```cisco
enable
conf t
interface serial 0/3/0
ip address 10.10.10.2 255.255.255.252
exit
do wr
```

**Explanation:** `ip address <ip> <mask>` assigns a Layer 3 address to the interface. The `/30` mask (`255.255.255.252`) is standard practice for point-to-point WAN links since it wastes only 2 addresses per link.

✅ **Verification Commands:**

```cisco
show ip interface brief
show ip route connected
```

**Expected Output:** `show ip interface brief` shows each serial interface with the assigned IP and status `up/up`.

🛠 **Troubleshooting:** If two connected interfaces are on different subnets (e.g. a typo like `10.10.10.5` on one end and `10.10.20.6` on the other), the link will show `up/up` physically but fail to route — always double-check both ends belong to the same `/30`.

---

### 11. Assign IP Address to Email Server

**Introduction:** The Email Server is a standalone host attached directly to the Cloud Router, representing an external/ISP-hosted mail service.

**Configuration (Desktop → IP Configuration on the Email Server):**

| Field | Value |
|---|---|
| IP Address | 20.0.0.2 |
| Subnet Mask | 255.255.255.252 |
| Default Gateway | 20.0.0.1 |

**Verification:**

```cisco
ping 20.0.0.1
```

✅ **Expected Output:** Successful replies (`Reply from 20.0.0.1: bytes=32 time=...`) confirm the server can reach its gateway (the Cloud Router).

🛠 **Troubleshooting:** If the ping fails, confirm the Cloud Router's `GigabitEthernet0/0` is `no shutdown` and carries `20.0.0.1/30`, and that the server's cable is connected to the correct port.

---

### 11. Configure Router-on-a-Stick (Inter-VLAN Routing)

**Introduction:** Router-on-a-Stick uses a single physical router interface, split into **sub-interfaces** — one per VLAN — to route traffic between VLANs that arrive trunked from the Layer 3 switch.

#### 12. Inter-VLAN Routing on Branch Campus Router

```cisco
enable
conf t
interface gigaethernet 0/0.90
encapsulation dot1Q 90
ip address 192.168.90.1 255.255.255.0
exit

interface gigaethernet 0/0.100
encapsulation dot1Q 100
ip address 192.168.100.1 255.255.255.0
exit
do wr
```

#### Inter-VLAN Routing on Main Campus Router

```cisco
enable
conf t
interface gigaethernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface gigaethernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

interface gigaethernet 0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit

interface gigaethernet 0/0.40
encapsulation dot1Q 40
ip address 192.168.40.1 255.255.255.0
exit

interface gigaethernet 0/0.50
encapsulation dot1Q 50
ip address 192.168.50.1 255.255.255.0
exit

interface gigaethernet 0/0.60
encapsulation dot1Q 60
ip address 192.168.60.1 255.255.255.0
exit

interface gigaethernet 0/0.70
encapsulation dot1Q 70
ip address 192.168.70.1 255.255.255.0
exit

interface gigaethernet 0/0.80
encapsulation dot1Q 80
ip address 192.168.80.1 255.255.255.0
exit
do wr
```


**Explanation:**
- `interface GigabitEthernet0/0.<vlan-id>` — creates a logical sub-interface tied to physical `Gi0/0`.
- `encapsulation dot1Q <vlan-id>` — tags the sub-interface to a specific VLAN carried on the trunk.
- `ip address` — gives that VLAN its default gateway address on the router.

✅ **Verification Commands:**

```cisco
show ip interface brief
show vlan brief
show running-config interface gigabitEthernet0/0
```

**Expected Output:** Each sub-interface (`Gi0/0.10` … `Gi0/0.80` on Main Campus; `Gi0/0.90`, `Gi0/0.100` on Branch) shows `up/up` and the correct gateway IP.

🛠 **Common Mistakes:**
- Forgetting `encapsulation dot1Q <id>` before the `ip address` line — the sub-interface won't come up without it.
- Mismatching the VLAN ID between the switch trunk and the router sub-interface.

🛠 **Troubleshooting:** If a sub-interface stays down, confirm the **physical** parent interface (`Gi0/0`) itself is `no shutdown`, and that the switchport facing it is a trunk carrying that VLAN.

---

### 13. Configure DHCP

**Introduction:** Instead of static addressing, end-user PCs and printers obtain their IP configuration automatically from router-based DHCP pools — one pool per VLAN/subnet.

#### Branch Campus Router DHCP

```cisco
enable
conf t
service dhcp
ip dhcp pool staff-pool
network 192.168.90.0 255.255.255.0
default-router 192.168.90.1
dns-server 192.168.90.1
exit
do wr

ip dhcp pool stud-lab
network 192.168.100.0 255.255.255.0
default-router 192.168.100.1
dns-server 192.168.100.1
exit
do wr
```

#### Main Campus Router DHCP

```cisco
enable
conf t
service dhcp

ip dhcp pool ADMIN-POOL
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 192.168.10.1
exit

ip dhcp pool HR-POOL
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 192.168.20.1
exit

ip dhcp pool FINANCE-POOL
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 192.168.30.1
exit

ip dhcp pool BUSINESS-POOL
network 192.168.40.0 255.255.255.0
default-router 192.168.40.1
dns-server 192.168.40.1
exit

ip dhcp pool E&C-POOL
network 192.168.50.0 255.255.255.0
default-router 192.168.50.1
dns-server 192.168.50.1
exit

ip dhcp pool A&D-POOL
network 192.168.60.0 255.255.255.0
default-router 192.168.60.1
dns-server 192.168.60.1
exit

ip dhcp pool STUD-LAB-POOL
network 192.168.70.0 255.255.255.0
default-router 192.168.70.1
dns-server 192.168.70.1
exit

ip dhcp pool IT-DEPT-POOL
network 192.168.80.0 255.255.255.0
default-router 192.168.80.1
dns-server 192.168.80.1
exit
do wr
```

**Explanation:**
- `service dhcp` — globally enables the IOS DHCP server process.
- `ip dhcp pool <name>` — creates a named address pool.
- `network <subnet> <mask>` — defines the pool's address range.
- `default-router` — the gateway handed out to clients (matches the sub-interface IP).
- `dns-server` — DNS server address handed out to clients.

✅ **Verification Commands:**

```cisco
show ip dhcp pool
show ip dhcp binding
show running-config | section dhcp
```

**Expected Output:** `show ip dhcp binding` populates with client MAC-to-IP leases once PCs request addresses. `show ip dhcp pool` shows the number of addresses leased/available per pool.

🛠 **Common Mistakes:**
- Forgetting `service dhcp` — pools are configured but never activated.
- Pool `network` statement not matching the router sub-interface's actual subnet.
- Not excluding the gateway address with `ip dhcp excluded-address` (optional hardening step — not present in the original config, worth adding; see [Future Improvements](#-future-improvements)).

🛠 **Troubleshooting:** If a PC gets an `Automatic Private IP Addressing (APIPA)` address (169.254.x.x), verify the access switchport is in the correct VLAN, the trunk carries that VLAN, and the router sub-interface/DHCP pool for that VLAN are both correctly configured.

---

### 14. Configure PCs and Printers

**Introduction:** All host PCs and printers across both campuses are configured to obtain addressing automatically via DHCP rather than static IPs.

**Steps (repeated for every PC and printer on both campuses):**

1. Click the device → **Desktop** tab → **IP Configuration**.
2. Select **DHCP**.
3. Confirm the device receives an IP address, subnet mask, default gateway, and DNS server automatically.

✅ **Verification:** The IP Configuration window auto-populates with an address from the correct VLAN's DHCP pool (e.g. a PC on the Admin switch should receive an address in `192.168.10.0/24`).

🛠 **Troubleshooting:** If "DHCP request failed" appears, re-check VLAN assignment on the access port, the trunk configuration, and the DHCP pool for that subnet, in that order.

---

### 15. Configure Servers

**Introduction:** Three servers exist in this topology: the external Email Server (already configured in [Step 11](#11-assign-ip-address-to-email-server)), and the internal Web Server and FTP Server hosted in the IT Department (Building C, VLAN 80).

#### Web Server

- Connect to the **IT Dept Switch** (VLAN 80).
- Assign a static IP within `192.168.80.0/24` (e.g. `192.168.80.10`), gateway `192.168.80.1`.
- Enable the **HTTP** service under the Services tab (Packet Tracer default page can be used or customized).

#### FTP Server

- Connect to the **IT Dept Switch** (VLAN 80).
- Assign a static IP within `192.168.80.0/24` (e.g. `192.168.80.11`), gateway `192.168.80.1`.
- Enable the **FTP** service and create a username/password for file access.

#### Email Server

- Already configured in [Step 11](#11-assign-ip-address-to-email-server) with IP `20.0.0.2`, connected to the Cloud Router.
- Optionally enable the **EMAIL** service (SMTP/POP3) and create user mailboxes for testing mail delivery between departments.

✅ **Verification:** From a client PC, open a web browser and navigate to the Web Server's IP; use the FTP client in the PC's Desktop tab to connect to the FTP Server.

🛠 **Troubleshooting:** If services are unreachable, confirm the server's default gateway matches its VLAN's router sub-interface, and that the relevant service is toggled **On** under the server's Services tab.

---

### 16. Configure RIPv2 Routing

**Introduction:** RIPv2 dynamically advertises all directly connected networks between the three routers, so every subnet in the enterprise can reach every other subnet without manual static routes.

**Commands — Branch Campus Router:**

```cisco
enable
conf t
router rip
version 2
network 192.168.90.0
network 192.168.100.0
network 10.10.10.0
exit
do wr
```

**Commands — Main Campus Router:**

```cisco
enable
conf t
router rip
version 2
network 10.10.10.0 
network 10.10.10.4
network 192.168.10.0
network 192.168.20.0
network 192.168.30.0
network 192.168.40.0
network 192.168.50.0
network 192.168.60.0
network 192.168.70.0
network 192.168.80.0
exit
do wr
```

**Commands — Cloud Router:**

```cisco
enable
conf t
router rip
version 2
network 10.10.10.4
network 20.0.0.0
exit
do wr
```

**Explanation:**
- `router rip` — enters RIP configuration mode.
- `version 2` — forces RIPv2 (classless, supports VLSM, uses multicast 224.0.0.9 instead of RIPv1's broadcast).
- `network <classful-network>` — advertises the classful network containing that interface and enables RIP on any interface within it. Note that RIP only requires the **classful** network address (e.g. `192.168.10.0`), not the exact subnet with mask.

✅ **Verification Commands:**

```cisco
show ip protocols
show ip route
show ip route rip
debug ip rip
```

**Expected Output:** `show ip route` on each router should show **R** (RIP-learned) routes for every remote subnet not directly connected to that router — e.g. the Branch Campus Router should learn routes to `192.168.10.0/24` through `192.168.80.0/24` via the Main Campus Router.


🛠 **Troubleshooting:** If a remote subnet is missing from `show ip route`, confirm the **classful** network statement is present on **both** ends of the link and that the RIP neighbor relationship is up (`show ip protocols` lists the neighbor under "Routing for Networks" and "Passive Interface(s)" should NOT include the WAN-facing interfaces).

> ⚠ **Warning:** As noted in [Project Scenario](project_description.md), the original brief called for **static routing** to reach the Cloud Email Server. This implementation instead uses **RIPv2** end-to-end, which provides dynamic route exchange across the network. If strict adherence to the original project requirements is needed, replace the Cloud Router's RIPv2 configuration with a static route as described in the **Future Improvements** section.
---

### 18. Connectivity Testing

**Introduction:** With addressing, VLANs, trunking, inter-VLAN routing, DHCP, and RIPv2 all in place, the final step is to verify true end-to-end reachability.

**Test matrix — ping the following from a client PC's Command Prompt:**

```cisco
ping 192.168.10.1     ! Local VLAN gateway
ping 192.168.90.1     ! Cross-campus VLAN gateway
ping 20.0.0.2         ! External Email Server
ping <another department's PC IP>
```

✅ **Expected Output:** All pings return `Reply from <ip>: bytes=32 time=...ms TTL=...` with 0% loss.

---

### 19. Verification

Run the following commands on each router and switch to confirm the overall build:

```cisco
show ip interface brief
show vlan brief
show interfaces trunk
show ip route
show ip protocols
show running-config
show ip dhcp binding
show ip dhcp pool
show controllers serial 0/3/0
```

| Command | Confirms |
|---|---|
| `show ip interface brief` | All interfaces/sub-interfaces are `up/up` with correct IPs |
| `show vlan brief` | VLANs exist and correct ports are assigned |
| `show interfaces trunk` | Trunk ports are active and passing all VLANs |
| `show ip route` | RIP routes to every remote subnet are present |
| `show ip protocols` | RIPv2 is running with correct networks advertised |
| `show ip dhcp binding` | Clients have received leases |
| `show controllers serial 0/3/0` | Correct DCE/DTE cable end and clock rate |

---

### 20. Troubleshooting

| Symptom | Likely Cause | Fix |
|---|---|---|
| Serial link `down/down` | Interface not `no shutdown`, or missing `clock rate` on DCE end | Re-check Steps 5 & 6 |
| PC gets APIPA address | Wrong VLAN on access port, trunk not carrying VLAN, or DHCP pool misconfigured | Re-check Steps 7, 9, 13 |
| Two VLANs can't reach each other | Router sub-interface missing or `encapsulation dot1Q` wrong VLAN ID | Re-check Step 12 |
| Remote campus subnet missing from routing table | RIP `network` statement missing/incorrect, or `version 2` omitted | Re-check Step 16 |
| Trunk not passing VLANs | Port still in access mode, or `switchport mode trunk` not applied | Re-check Step 9 |
| Email Server unreachable | Cloud Router `Gi0/0` down, wrong subnet on server, or RIP not advertising `20.0.0.0` | Re-check Steps 5, 11, 16 |

---

### 21. Expected Results

At the conclusion of this project, the network should demonstrate:

- All 10 VLANs operational with correctly segmented departmental traffic.
- Full inter-VLAN connectivity across the Main Campus via Router-on-a-Stick.
- Full inter-campus connectivity between Main Campus and Branch Campus via RIPv2 over the WAN link.
- Automatic DHCP addressing for every department's PCs and printers.
- Reachability to the externally hosted Email Server from internal hosts.
- Reachable internal Web and FTP servers from any authorized department.

---

## 🎓 Learning Outcomes

- Practical experience designing a multi-building, multi-campus enterprise network.
- Hands-on configuration of VLANs, trunking, and Router-on-a-Stick inter-VLAN routing.
- Configuring and troubleshooting router-based DHCP services.
- Implementing and verifying RIPv2 dynamic routing across a WAN.
- Working with serial DCE/DTE WAN links and clock rate configuration.
- Developing structured, professional network documentation.

## 📚 Networking Concepts Covered

- VLAN segmentation & 802.1Q trunking
- Router-on-a-Stick inter-VLAN routing
- IOS DHCP server configuration
- RIPv2 dynamic routing (classful network advertisement, VLSM support)
- Point-to-point WAN links (Serial DCE/DTE, clock rate)
- IP subnetting and addressing plan design
- Structured cabling and physical hardware (HWIC-2T) installation

---

## 🚀 Future Improvements

- Replace the Cloud Router's RIPv2 configuration with **static/default routing** to fully match the original project brief's requirement for static routing to the external email service.
- Add `ip dhcp excluded-address` statements on each router to reserve gateway addresses out of the DHCP pool.
- Implement basic switch port security (`switchport port-security`) on access ports.
- Migrate from RIPv2 to **OSPF** for faster convergence and better scalability as the network grows.
- Add VLAN-based ACLs to restrict inter-departmental traffic where appropriate (e.g. restrict Student Labs from reaching Finance/HR subnets).
- Configure **SSH** and disable **Telnet** for secure remote management of routers and switches.
- Assign static IPs (rather than DHCP) to the Web and FTP servers with `ip dhcp excluded-address` protection.

---

## 👤 Author

**Satya**
B.Tech. Computer Science Engineering — Networking & Cybersecurity focus
GitHub: [github.com/satyamgagre](https://github.com/satyamgagre)

Project: *NovaTech University Campus Network — Cisco Packet Tracer Enterprise Lab*
