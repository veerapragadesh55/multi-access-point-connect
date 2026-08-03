# RIP Multi-Site Network Configuration Guide

## Project: Multi-Site Network with RIP Routing & Static IP

Complete configuration documentation for a two-site enterprise network using RIPv2 dynamic routing protocol and static IP addressing.

---

## 1. Network Topology

```
                    WAN BACKBONE (RIP)
          10.1.1.0/24 ←→ 10.0.1.0/24
                ↓              ↓
         ┌──────────┐    ┌──────────┐
         │ Router1  │    │ Router2  │
         │ PT       │    │ PT       │
         └────┬─────┘    └─────┬────┘
              │ 192.168.1.1    │ 192.168.2.1
              │                │
        ┌─────▼────────┐  ┌────▼─────────┐
        │  Switch1     │  │  Switch2     │
        │  2950-24     │  │  2950-24     │
        └┬┬┬┬┬┬┬┬┬┬────┘  └┬┬┬┬┬┬┬┬┬┬────┘
         ││││││││││         ││││││││││
        AP3,PC,L1,L2      AP4,L3,PC,L4
        SITE 1            SITE 2
      192.168.1.0/24    192.168.2.0/24
```

---

## 2. Router Configuration

### Router1 (Site 1 - Left Campus)

#### Basic Information
```
Hostname: Router-PT (Router1)
Device Type: Cisco Router (ISR series equivalent)
Credentials:
  Password: password
```

#### Interface Configuration

**LAN Interface (GigabitEthernet0/0/0)**
```
Interface: GigabitEthernet0/0/0
IP Address: 192.168.1.1
Subnet Mask: 255.255.255.0
Status: Up
Speed: Auto (1000 Mbps)
Description: Connection to Switch1
```

**WAN Interface (Serial0/0/0)**
```
Interface: Serial0/0/0
IP Address: 10.1.1.1
Subnet Mask: 255.255.255.0
Status: Up
Clock Rate: 64000 (DCE side)
Description: WAN Link to Router2
```

#### Static Routes
```
No static routes configured
Routes are learned dynamically via RIP
```

#### RIP Configuration

```
Router Configuration Mode:
  router rip
  version 2
  no auto-summary
  network 192.168.1.0
  network 10.1.1.0
  passive-interface GigabitEthernet0/0/0
```

**RIP Parameters:**
- Protocol Version: RIPv2
- Admin Distance: 120
- Metric: Hop Count
- Update Timer: 30 seconds
- Invalid Timer: 180 seconds
- Holddown Timer: 180 seconds
- Flush Timer: 240 seconds

**Advertised Routes:**
- 192.168.1.0/24 (Site1 Local)
- 10.1.1.0/24 (WAN Link)

---

### Router2 (Site 2 - Right Campus)

#### Basic Information
```
Hostname: Router-PT (Router2)
Device Type: Cisco Router (ISR series equivalent)
Credentials:
  Password: word
```

#### Interface Configuration

**LAN Interface (GigabitEthernet0/0/0)**
```
Interface: GigabitEthernet0/0/0
IP Address: 192.168.2.1
Subnet Mask: 255.255.255.0
Status: Up
Speed: Auto (1000 Mbps)
Description: Connection to Switch2
```

**WAN Interface (Serial0/0/0)**
```
Interface: Serial0/0/0
IP Address: 10.0.1.0
Subnet Mask: 255.255.255.0
Status: Up
Clock Rate: 64000 (DTE side)
Description: WAN Link to Router1
```

#### RIP Configuration

```
Router Configuration Mode:
  router rip
  version 2
  no auto-summary
  network 192.168.2.0
  network 10.0.1.0
  passive-interface GigabitEthernet0/0/0
```

**Advertised Routes:**
- 192.168.2.0/24 (Site2 Local)
- 10.0.1.0/24 (WAN Link)

---

## 3. Switch Configuration

### Switch1 (Site 1)

#### Basic Information
```
Hostname: Switch1
Model: Cisco Catalyst 2950-24
Management IP: 192.168.1.3
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1
```

#### Port Configuration

**Router Connection (Port FastEthernet0/1)**
```
Port: FastEthernet0/1
Mode: Access
VLAN: 1 (Native)
Speed: Auto (100 Mbps)
Status: Connected to Router1
PortFast: Enabled
BPDU Guard: Enabled
```

**Device Connections (Ports F0/2-F0/24)**
```
F0/2: PC1 (192.168.1.50)
F0/3: PC2 (192.168.1.51)
F0/4: Laptop1 (192.168.1.100)
F0/5: Laptop2 (192.168.1.101)
F0/6-F0/24: Available
```

**AP Connection**
```
Port: (Wireless - Not wired to switch)
Access Point3 (Wireless)
Management IP: 192.168.1.2
SSID: CompanyNet-Site1
```

#### VLAN Configuration
```
VLAN 1 (Default):
  Name: default
  Ports: All
  IP: 192.168.1.3/24
  Gateway: 192.168.1.1
```

#### Port Speed Settings
```
All Access Ports: Auto-negotiate
  - Speed: 100 Mbps
  - Duplex: Full
  - Flow Control: Enabled
```

---

### Switch2 (Site 2)

#### Basic Information
```
Hostname: Switch2
Model: Cisco Catalyst 2950-24
Management IP: 192.168.2.3
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1
```

#### Port Configuration

**Router Connection (Port FastEthernet0/1)**
```
Port: FastEthernet0/1
Mode: Access
VLAN: 1 (Native)
Speed: Auto (100 Mbps)
Status: Connected to Router2
PortFast: Enabled
BPDU Guard: Enabled
```

**Device Connections (Ports F0/2-F0/24)**
```
F0/2: PC3 (192.168.2.50)
F0/3: PC4 (192.168.2.51)
F0/4: Laptop3 (192.168.2.100)
F0/5: Laptop4 (192.168.2.101)
F0/6-F0/24: Available
```

**AP Connection**
```
Port: (Wireless - Not wired to switch)
Access Point4 (Wireless)
Management IP: 192.168.2.2
SSID: CompanyNet-Site2
```

---

## 4. Static IP Addressing Scheme

### Site 1 Network (192.168.1.0/24)

```
Network Address: 192.168.1.0
Broadcast Address: 192.168.1.255
Subnet Mask: 255.255.255.0
Usable Range: 192.168.1.1 - 192.168.1.254
Gateway: 192.168.1.1
```

#### Device IP Assignments - Site 1

| # | Device Name | Device Type | IP Address | Subnet Mask | Default Gateway |
|---|-------------|------------|-----------|------------|-----------------|
| 1 | Router1 | Router (LAN) | 192.168.1.1 | 255.255.255.0 | N/A |
| 2 | Access Point3 | AP (Mgmt) | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| 3 | Switch1 | Switch (Mgmt) | 192.168.1.3 | 255.255.255.0 | 192.168.1.1 |
| 4 | PC-PT PC1 | Wired PC | 192.168.1.50 | 255.255.255.0 | 192.168.1.1 |
| 5 | PC-PT PC2 | Wired PC | 192.168.1.51 | 255.255.255.0 | 192.168.1.1 |
| 6 | Laptop-PT Laptop1 | Wired Laptop | 192.168.1.100 | 255.255.255.0 | 192.168.1.1 |
| 7 | Laptop-PT Laptop2 | Wired Laptop | 192.168.1.101 | 255.255.255.0 | 192.168.1.1 |
| 8 | SMARTPHONE-PT Smartphone1 | Wireless | 192.168.1.110 | 255.255.255.0 | 192.168.1.1 |

---

### Site 2 Network (192.168.2.0/24)

```
Network Address: 192.168.2.0
Broadcast Address: 192.168.2.255
Subnet Mask: 255.255.255.0
Usable Range: 192.168.2.1 - 192.168.2.254
Gateway: 192.168.2.1
```

#### Device IP Assignments - Site 2

| # | Device Name | Device Type | IP Address | Subnet Mask | Default Gateway |
|---|-------------|------------|-----------|------------|-----------------|
| 1 | Router2 | Router (LAN) | 192.168.2.1 | 255.255.255.0 | N/A |
| 2 | Access Point4 | AP (Mgmt) | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 |
| 3 | Switch2 | Switch (Mgmt) | 192.168.2.3 | 255.255.255.0 | 192.168.2.1 |
| 4 | PC-PT PC3 | Wired PC | 192.168.2.50 | 255.255.255.0 | 192.168.2.1 |
| 5 | PC-PT PC4 | Wired PC | 192.168.2.51 | 255.255.255.0 | 192.168.2.1 |
| 6 | Laptop-PT Laptop3 | Wired Laptop | 192.168.2.100 | 255.255.255.0 | 192.168.2.1 |
| 7 | Laptop-PT Laptop4 | Wired Laptop | 192.168.2.101 | 255.255.255.0 | 192.168.2.1 |
| 8 | SMARTPHONE-PT Smartphone2 | Wireless | 192.168.2.110 | 255.255.255.0 | 192.168.2.1 |

---

### WAN Link Network (10.0.0.0/24)

```
Network Address: 10.0.0.0
Broadcast Address: 10.0.0.255
Subnet Mask: 255.255.255.0
Usable Range: 10.0.0.1 - 10.0.0.254
```

#### WAN Interface IP Assignments

| Device | Interface | IP Address | Subnet Mask | Link Partner |
|--------|-----------|-----------|------------|--------------|
| Router1 | Serial0/0/0 | 10.1.1.1 | 255.255.255.0 | Router2 |
| Router2 | Serial0/0/0 | 10.0.1.0 | 255.255.255.0 | Router1 |

**Note:** IPs are on same network (10.0.0.0/24) for point-to-point WAN link.

---

## 5. Wireless Configuration

### Access Point 3 (Site 1)

#### Basic Settings
```
Hostname: Access Point3
Model: Cisco Aironet (generic in Packet Tracer)
Management IP: 192.168.1.2
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1
```

#### Wireless SSID - CompanyNet-Site1

```
SSID: CompanyNet-Site1
Broadcast: Enabled
Band: 2.4 GHz / 5.0 GHz (mixed)
Channel: 6 (2.4G) or 36 (5G)
Transmit Power: Full
Security: Open (WPA2 recommended for production)
```

#### Wireless Clients Support
```
Maximum Clients: 10+
Client Assignments in Packet Tracer:
  └── SMARTPHONE-PT Smartphone1 (192.168.1.110)
```

---

### Access Point 4 (Site 2)

#### Basic Settings
```
Hostname: Access Point4
Model: Cisco Aironet (generic in Packet Tracer)
Management IP: 192.168.2.2
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1
```

#### Wireless SSID - CompanyNet-Site2

```
SSID: CompanyNet-Site2
Broadcast: Enabled
Band: 2.4 GHz / 5.0 GHz (mixed)
Channel: 11 (2.4G) or 149 (5G)
Transmit Power: Full
Security: Open (WPA2 recommended for production)
```

#### Wireless Clients Support
```
Maximum Clients: 10+
Client Assignments in Packet Tracer:
  └── SMARTPHONE-PT Smartphone2 (192.168.2.110)
```

---

## 6. End Device Configuration

### Site 1 Wired Devices

**PC1 (192.168.1.50)**
```
Device Type: Desktop PC
IP Configuration: Static
IP Address: 192.168.1.50
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1
DNS: (Optional - not used)
```

**PC2 (192.168.1.51)**
```
Device Type: Desktop PC
IP Configuration: Static
IP Address: 192.168.1.51
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1
```

**Laptop1 (192.168.1.100)**
```
Device Type: Laptop
IP Configuration: Static
IP Address: 192.168.1.100
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1
```

**Laptop2 (192.168.1.101)**
```
Device Type: Laptop
IP Configuration: Static
IP Address: 192.168.1.101
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1
```

### Site 1 Wireless Device

**Smartphone1 (192.168.1.110)**
```
Device Type: Smartphone
IP Configuration: Static (Manual)
IP Address: 192.168.1.110
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.1.1
Wireless SSID: CompanyNet-Site1
```

---

### Site 2 Wired Devices

**PC3 (192.168.2.50)**
```
Device Type: Desktop PC
IP Configuration: Static
IP Address: 192.168.2.50
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1
```

**PC4 (192.168.2.51)**
```
Device Type: Desktop PC
IP Configuration: Static
IP Address: 192.168.2.51
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1
```

**Laptop3 (192.168.2.100)**
```
Device Type: Laptop
IP Configuration: Static
IP Address: 192.168.2.100
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1
```

**Laptop4 (192.168.2.101)**
```
Device Type: Laptop
IP Configuration: Static
IP Address: 192.168.2.101
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1
```

### Site 2 Wireless Device

**Smartphone2 (192.168.2.110)**
```
Device Type: Smartphone
IP Configuration: Static (Manual)
IP Address: 192.168.2.110
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.2.1
Wireless SSID: CompanyNet-Site2
```

---

## 7. RIP Routing Protocol Details

### RIP Overview
```
Protocol: Routing Information Protocol (RIP)
Version: RIPv2 (not RIPv1)
Admin Distance: 120 (default)
Metric: Hop Count (15 maximum)
Convergence: Slow (up to 3+ minutes)
Update Type: Periodic broadcasts
```

### RIP Timers

```
Update Timer: 30 seconds
  └─ RIP sends routing updates every 30 seconds

Invalid Timer: 180 seconds (6 update cycles)
  └─ Route marked as invalid after no updates

Holddown Timer: 180 seconds
  └─ Route held before deletion

Flush Timer: 240 seconds
  └─ Route completely removed from routing table
```

### Expected RIP Routes

**On Router1:**
```
R - RIP Origin
C - Connected

Codes: C - Connected, S - Static, R - RIP

C   192.168.1.0/24 is directly connected
C   10.1.1.0/24 is directly connected
R   192.168.2.0/24 [120/2] via 10.1.1.0, 00:00:15

(Route to 192.168.2.0 learned from Router2 via WAN)
```

**On Router2:**
```
C   192.168.2.0/24 is directly connected
C   10.0.1.0/24 is directly connected
R   192.168.1.0/24 [120/2] via 10.0.1.0, 00:00:15

(Route to 192.168.1.0 learned from Router1 via WAN)
```

### Hop Count Calculation
```
From Router1 to 192.168.2.0:
  Router1 → Router2: Hop Count = 1
  Through 10.0.0.0/24 network: Hop Count = 2 (final metric)

From Site1 PC to Site2 PC:
  Source → Router1 (local) → Router2 → Destination
  Hop count varies based on path
```

---

## 8. Connectivity Matrix

| From | To | Route | Hops | Next Hop |
|------|----|----|------|----------|
| PC1 (192.168.1.50) | PC3 (192.168.2.50) | Direct via RIP | 2 | Router1 → Router2 |
| PC1 (192.168.1.50) | Laptop4 (192.168.2.101) | Direct via RIP | 2 | Router1 → Router2 |
| Smartphone1 (192.168.1.110) | Smartphone2 (192.168.2.110) | RIP + WiFi | 2 | Router1 → Router2 |
| Laptop1 (192.168.1.100) | Any Site2 device | RIP routing | 2 | Router1 → Router2 |
| Any Site1 device | Any Site2 device | Full connectivity | 2 | Via WAN link |

---

## 9. Configuration Commands Reference

### Router RIP Configuration

```
Router# configure terminal
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# network 192.168.1.0
Router(config-router)# network 10.1.1.0
Router(config-router)# no auto-summary
Router(config-router)# passive-interface GigabitEthernet0/0/0
Router(config-router)# end
```

### Verify RIP Status

```
Router# show ip protocols
Router# show ip route
Router# show ip rip database
Router# show interfaces serial0/0/0
Router# debug ip rip
```

### Clear RIP Routes (if needed)

```
Router# clear ip route *
Router# clear ip rip process
Router# restart
```

---

## 10. Testing & Verification Procedures

### Test 1: Basic Connectivity (Ping)

**Test Site1 → Site1 (Local):**
```
From: PC1 (192.168.1.50)
To: PC2 (192.168.1.51)
Command: ping 192.168.1.51
Expected: Success (same site)
```

**Test Site1 → Site2 (Remote):**
```
From: PC1 (192.168.1.50)
To: PC3 (192.168.2.50)
Command: ping 192.168.2.50
Expected: Success (after RIP converges ~60 seconds)
```

**Test Site2 → Site1 (Remote):**
```
From: Laptop3 (192.168.2.100)
To: Laptop1 (192.168.1.100)
Command: ping 192.168.1.100
Expected: Success
```

### Test 2: Wireless Connectivity

**Test Smartphone1 → Site1 Wired:**
```
From: Smartphone1 (192.168.1.110) via WiFi
To: PC1 (192.168.1.50) wired
Command: ping 192.168.1.50
Expected: Success (same site WiFi to wired)
```

**Test Smartphone1 → Site2 (Remote):**
```
From: Smartphone1 (192.168.1.110) via WiFi
To: PC3 (192.168.2.50)
Command: ping 192.168.2.50
Expected: Success (WiFi to remote site via RIP)
```

### Test 3: Router Communication

**Test Router1 WAN Interface:**
```
From: Router1 console
Command: ping 10.0.1.0
Expected: Success (direct to Router2 WAN interface)
```

**Test Router2 Remote Network:**
```
From: Router2 console
Command: ping 192.168.1.1
Expected: Success (learned via RIP)
```

### Test 4: RIP Convergence

**Monitor Route Learning:**
```
On Router1:
1. show ip route (initial - only connected routes)
2. Wait 30-60 seconds
3. show ip route (should see 192.168.2.0 via RIP)
4. show ip rip database (verify RIP database)
```

---

## 11. Troubleshooting Guide

### Issue: RIP Routes Not Appearing

**Symptoms:**
- `show ip route` only shows connected networks
- Can't ping remote sites
- RIP updates not visible

**Causes & Solutions:**
```
1. RIP not enabled on router
   └─ Verify: router rip under config
   
2. Network statements missing
   └─ Check: network commands include both local & WAN
   
3. WAN link down
   └─ Verify: show interfaces serial0/0/0
   
4. Version mismatch
   └─ Ensure: version 2 enabled on both routers
```

### Issue: Intermittent Connectivity

**Symptoms:**
- Ping sometimes works, sometimes fails
- Intermittent packet loss
- Routes flapping

**Causes & Solutions:**
```
1. WAN link unstable
   └─ Check: serial interface status and errors
   
2. Device IP misconfigured
   └─ Verify: all devices have correct IP/gateway
   
3. RIP convergence in progress
   └─ Wait: 3-5 minutes for full convergence
```

### Issue: Can't Reach Wireless Devices

**Symptoms:**
- Wireless clients can't ping
- Only some devices unreachable
- Wireless to wireless works, but wireless to wired fails

**Causes & Solutions:**
```
1. Wireless device IP incorrect
   └─ Check: ipconfig on smartphone
   
2. Gateway not set
   └─ Set: default gateway to router IP
   
3. AP not bridging to wired network
   └─ Verify: AP can ping router gateway
```

### Issue: Router Can't Reach Remote Network

**Symptoms:**
- `ping 192.168.2.50` from Router1 fails
- Routes show up but don't work
- Specific devices unreachable

**Causes & Solutions:**
```
1. ACLs blocking traffic
   └─ Check: show access-lists (remove if testing)
   
2. Interface down
   └─ Verify: show interfaces (all should be "up up")
   
3. Incorrect gateway on source device
   └─ Check: local default gateway points to correct router
```

---

## 12. Maintenance Tasks

### Weekly Checks
- [ ] Verify all devices can ping across sites
- [ ] Check router uptime: `show version`
- [ ] Ensure RIP timers are running

### Monthly Checks
- [ ] Review interface errors: `show interfaces`
- [ ] Verify IP addressing scheme hasn't changed
- [ ] Test WAN link failover (if applicable)

### Quarterly Checks
- [ ] Document any configuration changes
- [ ] Review RIP routing efficiency
- [ ] Plan for network expansion

---

## 13. Best Practices Implemented

✓ RIPv2 instead of RIPv1 (better security)  
✓ Static IPs (no complexity from DHCP)  
✓ Clear naming convention  
✓ Passive interface on LAN (no RIP on local subnet)  
✓ No auto-summary (better control)  
✓ Separate subnets per site (scalability)  
✓ Management IPs for APs and switches  
✓ Wireless + Wired devices for testing  

---

## 14. Assumptions & Limitations

⚠️ **15-Hop Limit:**
- RIP can't route through more than 15 hops
- Not suitable for very large networks

⚠️ **Slow Convergence:**
- Takes 60-180 seconds to detect topology changes
- OSPF is faster for time-critical networks

⚠️ **No Redundancy:**
- Single WAN link (no backup)
- Single router per site

⚠️ **Static IPs Required:**
- Manual IP management needed
- Not scalable to thousands of devices

---

## 15. Quick Configuration Checklist

- [ ] Router1 interfaces configured (LAN & WAN)
- [ ] Router2 interfaces configured (LAN & WAN)
- [ ] RIP v2 enabled on both routers
- [ ] Network statements added for all subnets
- [ ] All end devices have static IPs assigned
- [ ] All devices have correct default gateway
- [ ] Wireless APs configured with SSID
- [ ] Wireless clients can join AP
- [ ] Ping works within same site
- [ ] Ping works across sites (after convergence)
- [ ] Router routing tables showing RIP routes

---

**Document Version:** 1.0  
**Last Updated:** August 2026  
**Configuration Type:** Production-Ready Lab  
**Network Administrator:** [Your Name]  
**Support Contact:** [Your Email]
