# 🌐 LAB 30 - Configuring Dedicated DHCP, DNS and Web Server

## 🎯 Objective

Configure dedicated DHCP, DNS, and Web Servers in a LAN environment. Configure DHCP for automatic IP assignment, DNS for name resolution, and Web Server hosting. Verify web access using a domain name.

---

## 🖧 Topology

### 🏢 Office LAN

* 1 × Cisco 2960-24TT Switch
* 5 × PCs
* Network: `192.168.10.0/24`

### 🏢 Data Center LAN

* 1 × Cisco 2960-24TT Switch
* 1 × DHCP Server
* 1 × DNS Server
* 1 × Web Server

### 🛜 Router

* Cisco 2911 Router
* IP Address: `192.168.10.1`

---

## 📋 Addressing Table

| Device         | IP Address   |
| -------------- | ------------ |
| 🛜 Router      | 192.168.10.1 |
| 📡 DHCP Server | 192.168.10.5 |
| 🌍 DNS Server  | 192.168.10.6 |
| 🌐 Web Server  | 192.168.10.7 |

### Common Server Settings

| Parameter       | Value         |
| --------------- | ------------- |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 192.168.10.1  |
| DNS Server      | 192.168.10.6  |

---

# 🔧 Step 1: Configure Router

```bash
enable
configure terminal

interface g0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit

do wr
```

---

# 🖥️ Step 2: Configure Server IP Addresses

## 📡 DHCP Server

```text
IP Address      : 192.168.10.5
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.10.1
DNS Server      : 192.168.10.6
```

## 🌍 DNS Server

```text
IP Address      : 192.168.10.6
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.10.1
DNS Server      : 192.168.10.6
```

## 🌐 Web Server

```text
IP Address      : 192.168.10.7
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.10.1
DNS Server      : 192.168.10.6
```

---

# 📡 Step 3: Configure DHCP Server

Navigate to:

```text
Services
→ DHCP
```

Create a DHCP Pool with the following settings:

| Parameter        | Value           |
| ---------------- | --------------- |
| Pool Name        | OFFICE-LAN-POOL |
| Default Gateway  | 192.168.10.1    |
| DNS Server       | 192.168.10.6    |
| Start IP Address | 192.168.10.11   |
| Subnet Mask      | 255.255.255.0   |
| Maximum Users    | 100             |

Click **Add**.

---

# 💻 Step 4: Configure Client PCs

For all PCs:

```text
Desktop
→ IP Configuration
→ DHCP
```

### Expected DHCP Range

```text
192.168.10.11 – 192.168.10.110
```

---

# 🌍 Step 5: Configure DNS Server

Navigate to:

```text
Services
→ DNS
```

Enable DNS Service.

Add the following DNS record:

| Name        | Address      |
| ----------- | ------------ |
| example.com | 192.168.10.7 |

Save the configuration.

---

# 🌐 Step 6: Configure Web Server

Navigate to:

```text
Services
→ HTTP
```

Ensure:

```text
HTTP = ON
```

Open:

```text
index.html
```

Example webpage:

```html
<html>
<head>
<title>LAB 30</title>
</head>

<body>
<h1>Welcome to My Web Server</h1>
<h2>Dedicated DHCP DNS and WEB Server Lab</h2>
<p>Website hosted successfully on Cisco Packet Tracer.</p>
</body>
</html>
```

Save the file.

---

# 🔍 Verification

## Verify DHCP

On any PC:

```text
Desktop
→ IP Configuration
```

Verify that the PC receives an IP address automatically.

---

## Verify DNS Resolution

Open Command Prompt:

```bash
ping example.com
```

Expected Result:

```text
Pinging example.com [192.168.10.7]
```

---

## Verify Web Access

Open:

```text
Desktop
→ Web Browser
```

Enter:

```text
http://example.com
```

The webpage hosted on the Web Server should open successfully.

---

# 🎉 Result

✅ DHCP Server Successfully Assigned IP Addresses

✅ DNS Server Successfully Resolved example.com

✅ Web Server Successfully Hosted Website

✅ Clients Successfully Accessed Website Using Domain Name

🏆 **LAB 30 Completed Successfully!**
