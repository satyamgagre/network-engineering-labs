

# DAY 7 – Remote Access Using Telnet Configuration

## Devices Used

* 1 × Cisco Router **2911**
* 1 × Cisco Switch **2960-24TT**
* 3 PCs

  * 1 Admin PC (Left Side)
  * 2 PCs (Right Side)

---

# IP Addressing

| Device   | Interface | IP Address    | Subnet Mask   | Gateway     |
| -------- | --------- | ------------- | ------------- | ----------- |
| Admin PC | NIC       | 192.168.1.10  | 255.255.255.0 | 192.168.1.1 |
| PC1      | NIC       | 192.168.2.10  | 255.255.255.0 | 192.168.2.1 |
| PC2      | NIC       | 192.168.2.20  | 255.255.255.0 | 192.168.2.1 |
| Router   | G0/0      | 192.168.1.1   | 255.255.255.0 | —           |
| Router   | G0/1      | 192.168.2.1   | 255.255.255.0 | —           |
| Switch   | VLAN 1    | 192.168.2.254 | 255.255.255.0 | 192.168.2.1 |

---

# Router Configuration

```
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
```

---

# Switch Management IP Configuration

```
enable
configure terminal

interface vlan 1
ip address 192.168.2.254 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.2.1
```

---

# Configure Hostname, Domain, Username

```
hostname S1
ip domain-name cisco.com
username admin password 123
```

---

# Configure Telnet Access

```
line vty 0 15
login local
transport input telnet
exit
```

---

# Save Configuration

```
do write memory
```

or

```
copy running-config startup-config
```

---

# Connectivity Test

From **Admin PC (Left Side)**

```
ping 192.168.2.254
```

If successful, proceed with Telnet.

---

# Remote Access Using Telnet

```
telnet 192.168.2.254
```

Login credentials:

```
Username: admin
Password: 123
```

You will now get **remote CLI access to the switch**.
