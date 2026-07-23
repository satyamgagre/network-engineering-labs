# Project Overview

This mini project focuses on the design and implementation of a secure, scalable, and efficient enterprise network for **Apex Financial Services**, a medium-sized organization specializing in **banking and insurance services**. The company operates from a **three-storey building**, with each floor containing multiple departments that require reliable network connectivity for communication and resource sharing.

As a **Junior Network Engineer**, the objective of this project is to design the network topology, perform IP address planning, implement VLAN segmentation, configure Layer 2 switching features, and document the complete network implementation using industry-standard networking practices.

The network topology and implementation are completed entirely in **Cisco Packet Tracer**, providing a realistic simulation of an enterprise network environment.

The network follows the **Hierarchical Network Design Model**, consisting of **Core**, **Distribution**, and **Access** layers, while incorporating redundant links to improve network reliability and availability. As specified in the project requirements, configuration is performed on the **Access Layer switches** and **end devices**.

Each department is assigned a dedicated **VLAN** and a unique **IP subnet**. VLANs are created and managed using **VTP (VLAN Trunking Protocol)** with **VTP Pruning** enabled to allow only the required VLANs, including VLAN 1, across trunk links. **EtherChannel** is configured between switches using the **LACP (Link Aggregation Control Protocol)** to provide higher bandwidth and link redundancy.

IP addressing is planned using **Variable Length Subnet Masking (VLSM)** based on the number of hosts required by each department. Starting from the base network **192.168.100.0/24**, appropriate subnet masks, usable IP address ranges, and broadcast addresses are calculated for every VLAN.

Basic switch configurations are implemented on all networking devices, including:

* Hostname configuration
* Console and VTY passwords
* Login banner messages
* Disabling DNS lookup
* Console EXEC timeout of 3 minutes
* Logging synchronous

All end devices, including PCs, printers, and servers, are configured with appropriate static IP addresses based on the subnetting plan. Switch ports are secured using **Port Security** with **Sticky MAC Address learning** and **Shutdown** as the violation mode to prevent unauthorized network access.

Finally, network connectivity is verified by testing communication between devices within the same VLAN and across different VLANs to validate the network design and implementation.

## Department Layout

### First Floor

| Department | PCs | Printers | Servers |
| ---------- | --: | -------: | ------: |
| ICT        |  20 |        4 |       2 |
| Research   |  20 |        4 |       1 |
| Electrical |  10 |        2 |       1 |

### Second Floor

| Department | PCs | Printers | Servers |
| ---------- | --: | -------: | ------: |
| Marketing  |  10 |        2 |       1 |
| Accounting |  10 |        2 |       1 |
| Finance    |  10 |        2 |       1 |

### Third Floor

| Department          | PCs | Printers | Servers |
| ------------------- | --: | -------: | ------: |
| Logistics and Store |  10 |        2 |       1 |
| Customer Care       |  10 |        4 |       1 |

## Project Requirements

* Design the enterprise network topology.
* Implement the complete network using **Cisco Packet Tracer**.
* Follow the **Hierarchical Network Design Model** (Core, Distribution, and Access layers).
* Include redundant links for improved network availability.
* Configure the Access Layer switches and end devices.
* Perform basic switch configuration:

  * Hostnames
  * Console and VTY passwords
  * Banner messages
  * Disable DNS lookup
  * EXEC timeout (3 minutes)
  * Logging synchronous
* Create a separate VLAN for every department.
* Configure **VTP** and enable **VTP Pruning**.
* Configure **EtherChannel** using **LACP**.
* Perform VLSM subnetting using the base network **192.168.100.0/24**.
* Assign a unique subnet to each VLAN.
* Configure all PCs, printers, and servers with appropriate IP addresses.
* Configure **Port Security** using Sticky MAC addresses with **Shutdown** violation mode.
* Test communication between devices within the same VLAN and across different VLANs.
* Document the complete network design, implementation, configuration, and testing process.
