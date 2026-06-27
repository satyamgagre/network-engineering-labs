# 🌐 LAB 51 - DHCPv6 Configuration on Cisco Packet Tracer

## 🎯 Objective

Configure a DHCPv6 Server to automatically assign IPv6 addresses to client PCs and verify IPv6 connectivity.

---

## 🖧 Devices Used

- 1 × Server (DHCPv6 Server)
- 1 × Cisco 2960-24TT Switch
- 2 × Host PCs

---

# 📋 Network Information

## IPv6 Network

| Network | Prefix |
|---------|--------|
| LAN A | 2001:0BB9:AABB:1234::/64 |

---

## DHCPv6 Server Configuration

| Setting | Value |
|---------|-------|
| IPv6 Address | 2001:0BB9:AABB:1234::10 |
| Prefix Length | /64 |
| Default Gateway | 2001:0BB9:AABB:1234::1 |
| DNS Server | 2001:0BB9:AABB:1234::10 |

---

## DHCPv6 Pool

| Setting | Value |
|---------|-------|
| Pool Name | LANA-POOL |
| IPv6 Prefix | 2001:0BB9:AABB:1234::/64 |
| Start IPv6 Address | 2001:0BB9:AABB:1234::11 |
| DNS Server | 2001:0BB9:AABB:1234::10 |
| Domain Name | example.com |
| Preferred Lifetime | 10000 |
| Valid Lifetime | 10000 |

---

# Step 1 - Configure the DHCPv6 Server

Open the server and navigate to:

```
Desktop → IP Configuration
```

Configure:

| Setting | Value |
|---------|-------|
| IPv6 Address | 2001:0BB9:AABB:1234::10 |
| Prefix Length | 64 |
| Default Gateway | 2001:0BB9:AABB:1234::1 |
| DNS Server | 2001:0BB9:AABB:1234::10 |

---

# Step 2 - Configure the DHCPv6 Pool

Navigate to:

```
Services → DHCPv6
```

Create a new pool with the following settings:

| Field | Value |
|-------|-------|
| Pool Name | LANA-POOL |
| IPv6 Prefix | 2001:0BB9:AABB:1234::/64 |
| Start IPv6 Address | 2001:0BB9:AABB:1234::11 |
| DNS Server | 2001:0BB9:AABB:1234::10 |
| Domain Name | example.com |
| Preferred Lifetime | 10000 |
| Valid Lifetime | 10000 |

Click **Add** to save the pool.

---

# Step 3 - Configure the PCs

For **P1** and **P2**:

```
Desktop → IP Configuration
```

Set:

```
IPv6 Configuration → Automatic
```

The PCs will automatically receive IPv6 addresses from the DHCPv6 server.

---

# Step 4 - Verify Connectivity

From either PC:

```text
ping 2001:0BB9:AABB:1234::10
```

Expected Result:

- Successful replies from the DHCPv6 Server.

---

# ✅ Result

Successfully configured a DHCPv6 Server in Cisco Packet Tracer. Both client PCs automatically obtained IPv6 addresses from the DHCPv6 pool and successfully communicated with the DHCPv6 Server.
