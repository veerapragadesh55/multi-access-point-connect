# Network Configuration Guide

## Project: Multi-Access Point Connectivity

This document details all device configurations, VLAN mappings, IP addressing, and wireless settings for the multi-AP network deployment.

---

## 1. Network Topology Summary

```
┌─────────────────────────────────────────────────────────┐
│         Internet / WAN Connection                        │
│                    (Router)                              │
└────────────────────┬────────────────────────────────────┘
                     │
            ┌────────┴─────────┐
            │                  │
       ┌────▼────┐       ┌──────▼───┐
       │  Core   │       │  Core    │
       │ Switch  │───────│ Switch   │
       │    1    │       │    2     │
       └────┬────┘       └─────┬────┘
            │                  │
      ┌─────┴──────┐      ┌────┴─────┐
      │            │      │          │
   ┌──▼──┐    ┌──▼──┐  ┌─▼──┐   ┌──▼──┐
   │ AP1 │    │ AP2 │  │AP3 │   │ AP4 │
   │2.4G │    │2.4G │  │5.0G│   │5.0G │
   └─────┘    └─────┘  └────┘   └─────┘
     ▲          ▲        ▲        ▲
     └──────────┴────────┴────────┘
          Wireless Clients
```

---

## 2. VLAN Configuration

### VLAN Overview

| VLAN ID | VLAN Name | Purpose | Subnet | Gateway |
|---------|-----------|---------|--------|---------|
| 1 | Native/Management | Default, switch management | 192.168.1.0/24 | 192.168.1.1 |
| 10 | Guest_WiFi | Guest wireless access | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Corporate_WiFi | Employee wireless access | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Servers | Wired server network | 192.168.30.0/24 | 192.168.30.1 |
| 40 | IoT_Devices | IoT/Smart devices | 192.168.40.0/24 | 192.168.40.1 |

### VLAN Port Assignment

#### Access Points

**AP1 (Access Point 1)**
- Model: Cisco Aironet 2800
- Wireless SSID: CorporateNet-2.4G
- Frequency Band: 2.4 GHz
- VLAN Assignment: 20 (Corporate)
- IP: 192.168.1.50
- Management: Enabled

**AP2 (Access Point 2)**
- Model: Cisco Aironet 2800
- Wireless SSID: GuestNet-2.4G
- Frequency Band: 2.4 GHz
- VLAN Assignment: 10 (Guest)
- IP: 192.168.1.51
- Management: Enabled

**AP3 (Access Point 3)**
- Model: Cisco Aironet 3800
- Wireless SSID: CorporateNet-5G
- Frequency Band: 5.0 GHz
- VLAN Assignment: 20 (Corporate)
- IP: 192.168.1.52
- Management: Enabled

**AP4 (Access Point 4)**
- Model: Cisco Aironet 3800
- Wireless SSID: IoT_Network-5G
- Frequency Band: 5.0 GHz
- VLAN Assignment: 40 (IoT)
- IP: 192.168.1.53
- Management: Enabled

---

## 3. Wireless Configuration

### SSID Broadcasting

| SSID Name | Band | Channel | Power | Security |
|-----------|------|---------|-------|----------|
| CorporateNet-2.4G | 2.4 GHz | 6 | Full | WPA2 Enterprise |
| GuestNet-2.4G | 2.4 GHz | 11 | Full | WPA2 Personal |
| CorporateNet-5G | 5.0 GHz | 36 | Full | WPA2 Enterprise |
| IoT_Network-5G | 5.0 GHz | 100 | Medium | WPA2 Personal |

### Channel Planning

**2.4 GHz Band (Non-overlapping channels: 1, 6, 11)**
- AP1: Channel 1
- AP2: Channel 6
- AP3: Not used (2.4G)
- AP4: Not used (2.4G)

**5.0 GHz Band (Wider channel availability)**
- AP1: Not used (5.0G)
- AP2: Not used (5.0G)
- AP3: Channel 36-40
- AP4: Channel 100-104

### Transmit Power Settings

| AP | Band | Power Level | Coverage Area |
|----|------|-------------|----------------|
| AP1 | 2.4G | Full (30 dBm) | Zone A |
| AP2 | 2.4G | Full (30 dBm) | Zone B |
| AP3 | 5.0G | Full (27 dBm) | Zone A |
| AP4 | 5.0G | Full (27 dBm) | Zone B |

---

## 4. IP Addressing Scheme

### Management Network (VLAN 1)

```
Network: 192.168.1.0/24
Gateway: 192.168.1.1
Range: 192.168.1.0 - 192.168.1.255

Assignments:
├── Router WAN: DHCP (ISP)
├── Router LAN: 192.168.1.1
├── Core Switch 1: 192.168.1.10
├── Core Switch 2: 192.168.1.11
├── AP1: 192.168.1.50
├── AP2: 192.168.1.51
├── AP3: 192.168.1.52
└── AP4: 192.168.1.53
```

### Guest WiFi (VLAN 10)

```
Network: 192.168.10.0/24
Gateway: 192.168.10.1
DHCP Pool: 192.168.10.100 - 192.168.10.254

Security:
├── Access: Allowed to Guest Zone only
├── Internet: YES
├── Intranet: NO
└── Bandwidth Limit: 10 Mbps per client
```

### Corporate WiFi (VLAN 20)

```
Network: 192.168.20.0/24
Gateway: 192.168.20.1
DHCP Pool: 192.168.20.100 - 192.168.20.254

Security:
├── Access: Full network access
├── Internet: YES
├── Intranet: YES
└── QoS: Priority traffic enabled
```

### Server Network (VLAN 30)

```
Network: 192.168.30.0/24
Gateway: 192.168.30.1
Addresses: Fixed (No DHCP)

Assignments:
├── DHCP Server: 192.168.30.50
├── DNS Server: 192.168.30.51
├── AAA/RADIUS: 192.168.30.52
└── File Server: 192.168.30.53
```

### IoT Devices (VLAN 40)

```
Network: 192.168.40.0/24
Gateway: 192.168.40.1
DHCP Pool: 192.168.40.100 - 192.168.40.254

Security:
├── Access: Limited to designated services
├── Internet: Restricted
└── Port Filtering: Enabled
```

---

## 5. DHCP Configuration

### DHCP Server Settings

**Primary DHCP Server** (Server1 in VLAN 30)

```
VLAN 10 (Guest):
  Pool: Guest_WiFi_DHCP
  Network: 192.168.10.0 255.255.255.0
  Range: 192.168.10.100 - 192.168.10.254
  Gateway: 192.168.10.1
  DNS: 192.168.30.51
  Lease Time: 24 hours
  Domain: guest.local

VLAN 20 (Corporate):
  Pool: Corporate_WiFi_DHCP
  Network: 192.168.20.0 255.255.255.0
  Range: 192.168.20.100 - 192.168.20.254
  Gateway: 192.168.20.1
  DNS: 192.168.30.51
  Lease Time: 48 hours
  Domain: corp.local

VLAN 40 (IoT):
  Pool: IoT_DHCP
  Network: 192.168.40.0 255.255.255.0
  Range: 192.168.40.100 - 192.168.40.254
  Gateway: 192.168.40.1
  DNS: 192.168.30.51
  Lease Time: 48 hours
  Domain: iot.local
```

---

## 6. Wireless Security Configuration

### WPA2 Enterprise (Corporate SSID)

```
Security Protocol: WPA2 (802.11i)
Encryption: AES-CCMP
Authentication: 802.1X with RADIUS
RADIUS Server: 192.168.30.52
RADIUS Port: 1812 (Authentication)
RADIUS Secret: [Configure in Packet Tracer]
```

### WPA2 Personal (Guest & IoT SSID)

```
Security Protocol: WPA2
Encryption: AES-CCMP
Pre-Shared Key (PSK):
├── GuestNet: "GuestWiFi2024!"
├── IoT_Network: "IoT@Network2024!"
└── Key Length: 16-63 characters
```

### Additional Security

```
Features Enabled:
├── SSID Broadcast: Enabled
├── 802.11 Rate Limiting: Standard
├── MAC Filtering: Disabled (for flexibility)
├── WPS: Disabled (security best practice)
└── Rogue AP Detection: Enabled (if available)
```

---

## 7. Switch Configuration

### Core Switch 1 & 2 (Catalyst 3850)

```
Hostname: CoreSwitch-1 / CoreSwitch-2
IP Address: 192.168.1.10 / 192.168.1.11
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1

VLAN Trunking:
├── Mode: 802.1Q
├── Native VLAN: 1
├── Allowed VLANs: 1-40
└── Trunk Link Speed: 1 Gbps
```

### Port Configuration

**AP Connection Ports (Access Mode)**

```
Port 1-4: Access ports
├── Mode: Access
├── VLAN: Respective AP VLAN (20, 10, 20, 40)
├── Speed: Auto (1000 Mbps full-duplex)
├── PortFast: Enabled
└── BPDU Guard: Enabled
```

**Uplink Ports (Trunk Mode)**

```
Ports G0/1 - G0/2 (Trunk to other switch):
├── Mode: Trunk
├── Encapsulation: 802.1Q
├── Native VLAN: 1
└── Allowed VLANs: All
```

---

## 8. Router Configuration

### Gateway Router Settings

```
Device: Cisco ISR4321 / Similar

Interfaces:
├── WAN (GigabitEthernet0/0/0):
│   └── DHCP: Enabled (ISP)
└── LAN (GigabitEthernet0/0/1):
    ├── IP: 192.168.1.1
    └── Netmask: 255.255.255.0

Routing:
├── Dynamic Routing: OSPF/RIP (optional)
├── Default Route: To ISP gateway
└── Static Routes: To VLAN gateways

NAT (if connecting to external network):
├── Inside: 192.168.0.0/16
└── Outside: ISP-assigned public IP
```

### Router Access Control Lists (ACLs)

```
Guest VLAN Restrictions:
├── Permit: Guest VLAN to Internet
├── Permit: Guest VLAN to DHCP/DNS
├── Deny: Guest VLAN to Corporate (VLAN 20)
├── Deny: Guest VLAN to Servers (VLAN 30)
└── Deny: Guest VLAN to IoT (VLAN 40)

IoT Device Restrictions:
├── Permit: IoT to specific services
├── Permit: IoT to Internet (if required)
└── Deny: IoT to other internal VLANs
```

---

## 9. Testing & Verification Commands

### Connection Tests

```
Ping Tests:
- ping 192.168.20.1 (Corporate VLAN gateway)
- ping 192.168.10.1 (Guest VLAN gateway)
- ping 192.168.30.50 (DHCP server)

Wireless Connection:
- Check AP signal strength
- Verify SSID broadcast
- Confirm encryption type

DHCP Verification:
- ipconfig (Windows)
- ifconfig (Linux)
- Check IP is in correct range
```

### Configuration Verification

```
Show Commands:
- show vlan brief (VLAN status)
- show interfaces (Interface info)
- show ip route (Routing table)
- show wireless clients (Connected clients)
- show access-lists (ACL details)
```

---

## 10. Troubleshooting Guide

### Wireless Connectivity Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| Can't see SSID | SSID broadcast disabled | Enable SSID broadcast in AP settings |
| Authentication fails | Wrong security key | Verify WPA2 password is correct |
| Low signal | Distance/interference | Move closer to AP or reduce interference |
| Slow speeds | Channel overlap | Use recommended channel plan |
| No internet | DHCP failure | Check DHCP server and gateway config |

### VLAN Issues

| Problem | Cause | Solution |
|---------|-------|----------|
| Can't reach DHCP | VLAN not routed | Verify VLAN gateway routing |
| Inter-VLAN no access | ACL blocking | Check router ACLs allow traffic |
| Wrong IP assigned | DHCP pool issue | Verify pool configuration |

---

## 11. Best Practices Implemented

✓ Separate 2.4GHz and 5GHz deployments  
✓ Non-overlapping channel assignments  
✓ VLAN-based traffic segmentation  
✓ WPA2 encryption for all SSID  
✓ DHCP pool management  
✓ Guest network isolation  
✓ IoT device restriction  
✓ Redundant core switches  
✓ Access point redundancy  
✓ Centralized DHCP server  

---

## 12. Maintenance Checklist

- [ ] Monthly: Review wireless client connections
- [ ] Quarterly: Update AP firmware
- [ ] Semi-Annual: Audit VLAN configuration
- [ ] Annual: Review security policies
- [ ] As-Needed: Monitor IP address pool usage
- [ ] As-Needed: Update ACL rules

---

**Document Version:** 1.0  
**Last Updated:** August 2026  
**Network Administrator:** [Your Name]  
**Contact:** [Your Email]
