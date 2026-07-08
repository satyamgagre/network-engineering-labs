# 🌐 LAB 62 - WLAN Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure multiple Wireless Access Points (APs), connect wireless devices securely using WPA2-PSK, and assign IP addresses through a DHCP server.

---

## 🖧 Devices Used

- 1 × Cisco 2960-24TT Switch
- 1 × Server
- 3 × AccessPoint-PT
- 1 × Laptop
- 1 × Smartphone-PT
- 1 × TabletPC-PT

---

# Step 1 - Configure Access Points

## Access Point - IT

**Config → Port1**

| Setting | Value |
|---------|-------|
| SSID | IT |
| Authentication | WPA2-PSK |
| Passphrase | `IT987654321` |

---

## Access Point - HR

**Config → Port1**

| Setting | Value |
|---------|-------|
| SSID | HR |
| Authentication | WPA2-PSK |
| Passphrase | `HR987654321` |

---

## Access Point - FIN

**Config → Port1**

| Setting | Value |
|---------|-------|
| SSID | FIN |
| Authentication | WPA2-PSK |
| Passphrase | `FIN987654321` |

---

# Step 2 - Install Wireless Module on Laptop

1. Open **Laptop**.
2. Go to **Physical**.
3. Turn **Power OFF**.
4. Remove the Ethernet module.
5. Insert **WPC300N Wireless Module**.
6. Turn **Power ON**.

---

# Step 3 - Connect Wireless Devices

## Laptop

**Desktop → PC Wireless**

- Click **Connect**
- Click **Refresh**
- Select **SSID: IT**
- Enter Password:

```
IT987654321
```

---

## Smartphone

**Config → Wireless0**

| Setting | Value |
|---------|-------|
| SSID | HR |
| Authentication | WPA2-PSK |
| Passphrase | `HR987654321` |

---

## Tablet

**Config → Wireless0**

| Setting | Value |
|---------|-------|
| SSID | FIN |
| Authentication | WPA2-PSK |
| Passphrase | `FIN987654321` |

---

# Step 4 - Configure DHCP Server

## Desktop → IP Configuration

| Setting | Value |
|---------|-------|
| IP Address | 10.10.10.10 |
| Subnet Mask | 255.0.0.0 |
| Default Gateway | 10.10.10.1 |

---

## Services → DHCP

| Setting | Value |
|---------|-------|
| Pool Name | dhcp-pool |
| Default Gateway | 10.10.10.1 |
| DNS Server | 10.10.10.1 |
| Start IP Address | 10.10.10.151 |
| Subnet Mask | 255.0.0.0 |
| Maximum Users | 256 |

---

# Step 5 - Obtain IP Address

On each wireless device:

**Desktop/Config → IP Configuration**

Select:

```
DHCP
```

The device will automatically receive an IP address from the DHCP server.

---

# ✅ Result

- Configured three wireless access points with WPA2-PSK security.
- Connected Laptop, Smartphone, and Tablet to their respective wireless networks.
- Configured the DHCP server to assign IP addresses automatically.
- Verified successful IP assignment to all wireless devices.
