
# 📡 LAB 21: Connecting Multiple Networks Using a Router

## 🎯 Objective

To connect two different networks using a router and enable communication between them.

---

## 🧾 IP Addressing Scheme

### 🔹 IT Department

| Device | IP Address  | Subnet Mask   | Default Gateway |
| ------ | ----------- | ------------- | --------------- |
| PC1    | 192.168.1.5 | 255.255.255.0 | 192.168.1.1     |
| PC2    | 192.168.1.6 | 255.255.255.0 | 192.168.1.1     |

---

### 🔹 Sales Department

| Device | IP Address  | Subnet Mask   | Default Gateway |
| ------ | ----------- | ------------- | --------------- |
| PC3    | 192.168.2.5 | 255.255.255.0 | 192.168.2.1     |
| PC4    | 192.168.2.6 | 255.255.255.0 | 192.168.2.1     |

---

## ⚙️ Router Configuration

```bash
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

end
write memory
```

---

## 🔍 Verification

```bash
show ip interface brief
```

---

## 🧪 Testing Connectivity

### 🔹 Same Network

```bash
ping 192.168.1.6
```

### 🔹 Different Network

```bash
ping 192.168.2.5
```
