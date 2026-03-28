
# 📡 Day 22: Telnet Configuration on Router

## 🧠 Concept

**Telnet** is a network protocol used to remotely access and manage devices over a network. It operates on **port 23** and allows administrators to log into a router or switch from a remote system.

In this lab, we configure Telnet access on a router and verify connectivity using multiple PCs.

---

## 🏗️ Network Setup

* **1 Router** – Cisco 2911
* **1 Switch** – Cisco 2960-24TT
* **5 PCs** connected to the switch

---

## ⚙️ Configuration Steps

### 1️⃣ Router Basic Configuration

```bash
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

do write
```

---

### 2️⃣ Assign IP Address to PCs

| PC  | IP Address  | Subnet Mask   | Default Gateway |
| --- | ----------- | ------------- | --------------- |
| PC1 | 192.168.1.5 | 255.255.255.0 | 192.168.1.1     |
| PC2 | 192.168.1.6 | 255.255.255.0 | 192.168.1.1     |
| PC3 | 192.168.1.7 | 255.255.255.0 | 192.168.1.1     |
| PC4 | 192.168.1.8 | 255.255.255.0 | 192.168.1.1     |
| PC5 | 192.168.1.9 | 255.255.255.0 | 192.168.1.1     |

---

### 3️⃣ Verify Connectivity

```bash
ping 192.168.1.1
```

✔️ All PCs should successfully ping the router.

---

### 4️⃣ Configure Telnet on Router

```bash
enable
configure terminal

hostname ROUTER-TELNET
enable password 123
ip domain-name cisco.com

username admin password 123

line vty 0 15
login local
transport input telnet
exit
```

---

### 5️⃣ Access Router via Telnet

From any PC:

```bash
telnet 192.168.1.1
```

**Login Credentials:**

```
Username: admin
Password: 123
```

---

