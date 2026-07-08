# 🌐 LAB 63 - Wireless LAN Controller (WLC) Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure a Wireless LAN Controller (WLC), DHCP server, Lightweight Access Points (LAPs), WLANs, AP Groups, and connect wireless clients.

---

## 🖧 Devices Used

- 1 × Cisco WLC-2504
- 4 × LAP-PT Access Points
- 1 × Cisco 2960-24TT Switch
- 1 × DHCP Server
- 1 × PC
- 8 × Smartphone-PT

---

# Step 1 - Power On the Access Points

For each **LAP-PT**:

- Open **Physical** tab.
- Drag **Access Point Power Adapter** into the power slot.

---

# Step 2 - Configure the WLC

**Config → Management**

| Setting | Value |
|---------|-------|
| IP Address | 192.168.20.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.20.1 |
| DNS Server | 192.168.20.1 |

---

# Step 3 - Configure the PC

| Setting | Value |
|---------|-------|
| IP Address | 192.168.20.7 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.20.1 |
| DNS Server | 192.168.20.1 |

---

# Step 4 - Configure the DHCP Server

## Desktop → IP Configuration

| Setting | Value |
|---------|-------|
| IP Address | 192.168.20.5 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.20.1 |
| DNS Server | 192.168.20.1 |

---

## Services → DHCP

| Setting | Value |
|---------|-------|
| Pool Name | DHCP-POOL |
| Default Gateway | 192.168.20.1 |
| DNS Server | 192.168.20.1 |
| Start IP Address | 192.168.20.101 |
| Subnet Mask | 255.255.255.0 |
| Maximum Users | 100 |
| WLC Address | 192.168.20.10 |

---

# Step 5 - Configure Access Points

For each **LAP-PT**:

**Config → Settings**

Select:

```
DHCP
```

The APs will obtain an IP address automatically.

---

# Step 6 - Access the WLC

Verify connectivity:

```text
ping 192.168.20.10
```

Open the PC Web Browser:

```
http://192.168.20.10
```

Create the administrator account:

| Setting | Value |
|---------|-------|
| Username | admin |
| Password | Admin123 |

---

## Basic WLC Configuration

| Setting | Value |
|---------|-------|
| System Name | SATYAWLC |
| Country | India |
| Management IP | 192.168.20.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.20.1 |

---

## Create the First WLAN

| Setting | Value |
|---------|-------|
| Network Name (SSID) | IT |
| Security | WPA2-Personal |
| Passphrase | IT12345678 |

Click **Apply**.

---

# Step 7 - Log In to the WLC

Open:

```
https://192.168.20.10
```

Login:

| Username | Password |
|-----------|----------|
| admin | Admin123 |

---

# Step 8 - Create WLANs

## Wireless → WLANs → Create New

### WLAN 1

| Setting | Value |
|---------|-------|
| Profile Name | HR |
| SSID | HR |
| Status | Enabled |
| Security | WPA2-PSK |
| Password | HR12345678 |

Click **Apply**.

---

### WLAN 2

| Setting | Value |
|---------|-------|
| Profile Name | FIN |
| SSID | FIN |
| Status | Enabled |
| Security | WPA2-PSK |
| Password | FIN12345678 |

Click **Apply**.

---

### WLAN 3

| Setting | Value |
|---------|-------|
| Profile Name | GUEST |
| SSID | GUEST |
| Status | Enabled |
| Security | WPA2-PSK |
| Password | GUEST12345678 |

Click **Apply**.

---

# Step 9 - Create AP Groups

Go to:

**Wireless → AP Groups**

Create the following groups:

| AP Group | Description |
|-----------|-------------|
| IT | IT WiFi Users |
| HR | HR WiFi Users |
| FIN | FIN WiFi Users |
| GUEST | Guest WiFi Users |

---

# Step 10 - Assign WLANs to AP Groups

For each AP Group:

**AP Group → WLANs → Add New**

| AP Group | WLAN |
|-----------|------|
| IT | IT |
| HR | HR |
| FIN | FIN |
| GUEST | GUEST |

Click **Apply**.

---

# Step 11 - Assign Access Points

In each AP Group:

**APs → Add AP**

| AP Group | Access Point |
|-----------|--------------|
| IT | IT-AP |
| HR | HR-AP |
| FIN | FIN-AP |
| GUEST | GUEST-AP |

Click **Apply**.

---

# Step 12 - Connect Smartphones

## IT Smartphones (SM1, SM2)

**Config → Wireless0**

| Setting | Value |
|---------|-------|
| SSID | IT |
| Security | WPA2-PSK |
| Password | IT12345678 |

---

## HR Smartphones (SM3, SM4)

| Setting | Value |
|---------|-------|
| SSID | HR |
| Security | WPA2-PSK |
| Password | HR12345678 |

---

## FIN Smartphones (SM5, SM6)

| Setting | Value |
|---------|-------|
| SSID | FIN |
| Security | WPA2-PSK |
| Password | FIN12345678 |

---

## GUEST Smartphones (SM7, SM8)

| Setting | Value |
|---------|-------|
| SSID | GUEST |
| Security | WPA2-PSK |
| Password | GUEST12345678 |

---

# ✅ Result

- Configured the Wireless LAN Controller (WLC).
- Configured the DHCP server.
- Registered all Lightweight Access Points (LAPs).
- Created four WLANs (IT, HR, FIN, GUEST).
- Created AP Groups and assigned WLANs and APs.
- Successfully connected all smartphones to their respective wireless networks.
