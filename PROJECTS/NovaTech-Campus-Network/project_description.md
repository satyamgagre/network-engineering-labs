# 🎓 NovaTech University Campus Network Design

A complete enterprise campus network designed and implemented in **Cisco Packet Tracer** for **NovaTech University**. This project simulates a real-world multi-campus university network using industry-standard networking concepts such as VLANs, Inter-VLAN Routing, DHCP, RIPv2, Static Routing, and Router-on-a-Stick architecture.

The network connects two geographically separated campuses, enabling secure communication between departments, faculties, laboratories, and university servers while maintaining logical network segmentation and scalability.

---

## 📖 Project Overview

NovaTech University operates two campuses located approximately **20 miles apart**. The university consists of multiple academic faculties, administrative departments, student laboratories, and IT services that require reliable and secure network connectivity.

The objective of this project is to design, configure, and simulate a scalable enterprise network that supports:

- Departmental communication
- Inter-campus connectivity
- Centralized network services
- Dynamic IP allocation
- Secure VLAN segmentation
- Access to internal and external servers

The entire infrastructure has been implemented and tested using **Cisco Packet Tracer**.

---

# 🏫 Campus Layout

## Main Campus

The main campus consists of three buildings.

### Building A
- Administration
- Human Resources (HR)
- Finance
- Faculty of Business

### Building B
- Faculty of Engineering & Computing
- Faculty of Art & Design

### Building C
- Student Computer Laboratories
- IT Department
- Internal Web Server
- Internal FTP Server

---

## Branch Campus

The branch campus hosts the **Faculty of Health and Sciences**, divided into:

- Staff Department
- Student Laboratory

---

## Cloud Network

A separate cloud network hosts the university's **Email Server**, which is accessed through static routing.

---

# 🎯 Project Objectives

- Design a realistic enterprise campus network
- Implement VLAN-based network segmentation
- Configure Router-on-a-Stick for Inter-VLAN Routing
- Deploy Router-based DHCP services
- Configure Dynamic Routing using RIPv2
- Configure Static Routing to external cloud resources
- Provide end-to-end communication across both campuses
- Simulate a scalable and maintainable enterprise infrastructure

---

# 🛠 Technologies Used

- Cisco Packet Tracer
- Cisco 2911 Routers
- Cisco 3650 Layer-3 Switches
- Cisco 2960 Access Switches
- VLANs
- Router-on-a-Stick
- DHCP
- RIPv2
- Static Routing
- Serial WAN Links
- Web Server
- FTP Server
- Email Server

---

# 🌐 Network Features

- Multi-campus enterprise topology
- Ten independent VLANs
- Router-based DHCP
- Inter-VLAN Routing
- Dynamic Routing using RIPv2
- Static Routing to cloud network
- Internal Web and FTP services
- External Email Server connectivity
- Logical network segmentation
- Scalable hierarchical design

---

# 🗂 VLAN Structure

| VLAN | Department |
|------:|------------|
| 10 | Administration |
| 20 | Human Resources |
| 30 | Finance |
| 40 | Business |
| 50 | Engineering & Computing |
| 60 | Art & Design |
| 70 | Student Laboratories |
| 80 | IT Department |
| 90 | Branch Staff |
| 100 | Branch Student Laboratory |

---

# 🌍 IP Addressing Scheme

| Network | Subnet |
|----------|----------------|
| VLAN 10 | 192.168.10.0/24 |
| VLAN 20 | 192.168.20.0/24 |
| VLAN 30 | 192.168.30.0/24 |
| VLAN 40 | 192.168.40.0/24 |
| VLAN 50 | 192.168.50.0/24 |
| VLAN 60 | 192.168.60.0/24 |
| VLAN 70 | 192.168.70.0/24 |
| VLAN 80 | 192.168.80.0/24 |
| VLAN 90 | 192.168.90.0/24 |
| VLAN 100 | 192.168.100.0/24 |
| Main ↔ Branch WAN | 10.10.10.0/30 |
| Main ↔ Cloud WAN | 10.10.10.4/30 |
| Cloud Network | 20.0.0.0/30 |

---

# 📌 Key Networking Concepts Demonstrated

- Enterprise Campus Network Design
- VLAN Segmentation
- Layer-2 Switching
- Layer-3 Switching
- Router-on-a-Stick
- Inter-VLAN Routing
- DHCP Configuration
- RIPv2 Dynamic Routing
- Static Routing
- WAN Connectivity
- Network Services Deployment
- End-to-End Connectivity Testing

---

# 📂 Repository Structure

```
NovaTech-Campus-Network/
│
├── PROJECT-NovaTech-Campus-Network.pkt
├── project_description.md
├── README.md
└── project_topology.png
```

---

# 🚀 Learning Outcomes

This project demonstrates practical implementation of enterprise networking concepts commonly used in universities, corporate campuses, and medium-sized organizations.

Through this project, the following skills were applied:

- Enterprise Network Planning
- VLAN Design
- IP Addressing
- DHCP Deployment
- Dynamic Routing
- Static Routing
- Router and Switch Configuration
- Server Deployment
- Campus Network Architecture
- Network Troubleshooting
- Connectivity Verification

---

# 📷 Network Topology

> A complete logical topology of the NovaTech University Campus Network is included in this repository.

---

# 👨‍💻 Author

**Satyam Gagare**

**B.Tech Computer Science & Engineering (Cyber Forensics & Information Security)**

---

## ⭐ If you found this project helpful, consider giving the repository a star.
