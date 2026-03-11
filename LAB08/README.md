
# DAY 8 – SSH Remote Access Configuration

**Devices Used**

* 1 × Cisco Router **2911**
* 1 × Cisco Switch **2960-24TT**
* 3 PCs

  * 2 Client PCs
  * 1 Admin PC
* Copper Straight-Through Cables

---

# 📌 Lab Objective

* Assign IP addresses to router and hosts
* Configure **Switch Management IP**
* Configure **SSH on Cisco Switch**
* Generate **RSA keys**
* Secure remote access using **SSH Version 2**
* Access the switch remotely from **Admin PC**

---

# 🧾 IP Addressing Table

| Device   | Interface | IP Address    | Subnet Mask   | Default Gateway |
| -------- | --------- | ------------- | ------------- | --------------- |
| PC1      | NIC       | 192.168.2.10  | 255.255.255.0 | 192.168.2.1     |
| PC2      | NIC       | 192.168.2.20  | 255.255.255.0 | 192.168.2.1     |
| Admin PC | NIC       | 192.168.1.10  | 255.255.255.0 | 192.168.1.1     |
| Router   | G0/0      | 192.168.2.1   | 255.255.255.0 | —               |
| Router   | G0/1      | 192.168.1.1   | 255.255.255.0 | —               |
| Switch   | VLAN1     | 192.168.2.254 | 255.255.255.0 | 192.168.2.1     |

---

# ⚙️ Router Configuration

```
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.2.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
```

---

# ⚙️ Switch Management Configuration

```
enable
configure terminal

interface vlan 1
ip address 192.168.2.254 255.255.255.0
no shutdown
exit

ip default-gateway 192.168.2.1

do write
```

---

# 🔐 SSH Configuration on Switch

### 1️⃣ Configure Hostname and Domain

```
hostname S1-SSH
ip domain-name cisco.com
```

---

### 2️⃣ Configure User Authentication

```
enable password 123
username admin password 123
```

---

### 3️⃣ Generate RSA Keys

```
crypto key generate rsa
How many bits in the modulus [512]: 1024
```

---

### 4️⃣ Enable SSH Version 2

```
ip ssh version 2
```

---

### 5️⃣ Configure VTY Lines

```
line vty 0 15
login local
transport input ssh
exit

do write
```

---

# 🧪 Connectivity Test

From **Admin PC**

```
ping 192.168.2.254
```

Verify that the switch responds.

---

# 💻 Remote SSH Login

From **Admin PC Command Prompt**

```
ssh -l admin 192.168.2.254
```

Password

```
123
```

---

✅ **Result**

Secure remote management of the Cisco switch is successfully established using **SSH protocol**.

