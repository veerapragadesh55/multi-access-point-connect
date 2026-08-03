# Multi-Site Network with RIP Routing

A Cisco Packet Tracer network simulation demonstrating inter-site connectivity using RIP dynamic routing protocol with static IP addressing across two campus locations.

## Project Overview

This project models a **two-site enterprise network** using:
- **RIP (Routing Information Protocol)** for dynamic route discovery
- **Static IP addressing** (no DHCP servers)
- **Two geographically separated sites** with independent subnets
- **Access points** at each location for wireless connectivity
- **Managed switches** for LAN infrastructure
- **WAN link** between routers for site-to-site communication

## Network Architecture

```
┌─────────────────────────────────────────────────────────┐
│                  WAN Connection (RIP)                    │
│            10.1.1.1 ←→ 10.0.1.0 (Router-to-Router)     │
└───────────────────┬─────────────────────────────────────┘
                    │
        ┌───────────┴──────────┐
        │                      │
   ┌────▼─────┐          ┌─────▼────┐
   │ Router1  │          │ Router2   │
   │192.168.1.1         │192.168.2.1
   └────┬─────┘          └──────┬────┘
        │                       │
   ┌────▼─────────┐      ┌──────▼──────┐
   │ Switch1      │      │ Switch2     │
   │2950-24       │      │2950-24      │
   └┬──┬───┬──┬───┘      └┬──┬──┬──┬───┘
    │  │   │  │          │  │  │  │
   AP3 PC  L1 L2        AP4 L3 PC L4
   (WiFi)(Wired)(WiFi)  (WiFi)(Wired)(WiFi)
        
   SUBNET: 192.168.1.0/24  SUBNET: 192.168.2.0/24
```

## Key Features

✅ **RIP Dynamic Routing** - Automatic route learning between sites  
✅ **Static IP Configuration** - Manual IP assignment (no DHCP)  
✅ **Wireless Access Points** - WiFi at each location  
✅ **Multi-site Connectivity** - Full inter-site communication  
✅ **WAN Simulation** - Router-to-router link over network backbone  
✅ **Managed Switches** - Catalyst 2950 series switches  
✅ **Mixed Devices** - Laptops, PCs, Smartphones  

## Technologies Used

- **Cisco Packet Tracer** 8.x
- **RIP v2** (Routing Information Protocol Version 2)
- **Catalyst Switches** (2950-24 series)
- **Cisco Routers** (Generic ISR series)
- **Static IP Addressing** (Manual configuration)
- **Wireless LAN** (802.11 Access Points)

## Network Specifications

### Site 1 (Left Campus - Router1)
- **Router:** Router-PT (Router1)
- **IP Address:** 192.168.1.1/24
- **WAN Link:** 10.1.1.1/24
- **Switch:** Catalyst 2950-24 (Switch1)
- **Access Point:** Access Point3 (Wireless)
- **End Devices:** 5 devices (mix of wired & wireless)
- **Credentials:** Router1-password

### Site 2 (Right Campus - Router2)
- **Router:** Router-PT (Router2)
- **IP Address:** 192.168.2.1/24
- **WAN Link:** 10.0.1.0/24
- **Switch:** Catalyst 2950-24 (Switch2)
- **Access Point:** Access Point4 (Wireless)
- **End Devices:** 5 devices (mix of wired & wireless)
- **Credentials:** Router2-word

### WAN Configuration
- **Link Type:** Serial/Leased Line
- **Router1 Side:** 10.1.1.1/24
- **Router2 Side:** 10.0.1.0/24
- **Routing Protocol:** RIP v2
- **Metrics:** Hop count based

## Use Cases

- 🏢 **Multi-campus enterprise networks**
- 🎓 **University site interconnection**
- 🏢 **Branch office connectivity**
- 🏗️ **Distributed office network**
- 📚 **Networking training labs**

## How to Use

### 1. Open Project in Packet Tracer
```
File → Open → multi_access_point_connect.pkt
```

### 2. Verify Static IP Configuration
- Click each device
- Desktop → IP Configuration
- Verify manual IP addresses match CONFIGURATION.md

### 3. Check RIP Routing
- Click each router
- CLI → Check: `show ip route`
- Verify RIP routes are learned

### 4. Test Connectivity

**Test Site-to-Site Communication:**
```
From Site1 PC (192.168.1.100):
  ping 192.168.2.100 (Site2 PC)
  
From Site2 Laptop (192.168.2.50):
  ping 192.168.1.50 (Site1 Laptop)
```

**Test Wireless Access:**
```
Smartphone1 (WiFi to AP3):
  Can reach all devices in Site1
  Can ping Site2 devices via Router1
```

### 5. Simulate Network Events
- Disable a link and watch RIP recalculate routes
- Monitor convergence time
- Observe how traffic reroutes

## Static IP Addressing Scheme

### Site 1 (192.168.1.0/24)
| Device | IP Address | Role |
|--------|-----------|------|
| Router1 | 192.168.1.1 | Gateway |
| Access Point3 | 192.168.1.2 | Management IP |
| PC-PT PC1 | 192.168.1.11 | Wired Client |
| PC-PT PC2 | 192.168.1.13 | Wired Client |
| Laptop-PT Laptop1 | 192.168.1.12 | Wired Client |
| Laptop-PT Laptop2 | 192.168.1.14 | Wired Client |
| SMARTPHONE-PT Smartphone1 | 192.168.1.15 | Wireless Client |

### Site 2 (192.168.2.0/24)
| Device | IP Address | Role |
|--------|-----------|------|
| Router2 | 192.168.2.1 | Gateway |
| Access Point4 | 192.168.2.2 | Management IP |
| PC-PT PC3 | 192.168.2.14 | Wired Client |
| PC-PT PC4 | 192.168.2.11 | Wired Client |
| Laptop-PT Laptop3 | 192.168.2.15 | Wired Client |
| Laptop-PT Laptop4 | 192.168.2.13 | Wired Client |
| SMARTPHONE-PT Smartphone2 | 192.168.2.12 | Wireless Client |

### WAN Link (10.0.0.0/24)
| Device | Interface | IP Address |
|--------|-----------|-----------|
| Router1 | Serial0/0/0 | 10.1.1.1/24 |
| Router2 | Serial0/0/0 | 10.0.1.0/24 |

## RIP Configuration

### RIP Version
- **Protocol:** RIPv2 (not RIPv1)
- **Auto-summarization:** Disabled
- **Metric:** Hop count (max 15 hops)
- **Update Interval:** 30 seconds
- **Route Timeout:** 180 seconds

### Advertised Networks

**Router1:**
```
network 192.168.1.0
network 10.1.1.0
```

**Router2:**
```
network 192.168.2.0
network 10.0.1.0
```

## Commands Reference

### View RIP Configuration
```
show ip protocols
show ip route
show ip rip database
```

### Enable RIP Routing
```
Router# config terminal
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# network 192.168.1.0
Router(config-router)# network 10.1.1.0
Router(config-router)# no auto-summary
```

### Debug RIP Activity
```
debug ip rip
```

## Testing Checklist

- [ ] Ping from Site1 to Site2 devices
- [ ] Ping from Site2 to Site1 devices
- [ ] Wireless client can reach Site2
- [ ] View routing tables on both routers
- [ ] Observe RIP route advertisement
- [ ] Test with link down/up scenarios
- [ ] Verify convergence time
- [ ] Check hop count metrics

## Learning Outcomes

By studying this project, you'll learn:
- ✓ RIP v2 dynamic routing protocol
- ✓ Static IP configuration
- ✓ Multi-site network design
- ✓ Router configuration basics
- ✓ Inter-router connectivity
- ✓ Route metrics and convergence
- ✓ WAN link simulation
- ✓ Wireless AP integration

## Troubleshooting

### RIP Routes Not Showing Up
- Verify RIP is enabled on both routers
- Check `show ip protocols` output
- Ensure network statements are correct
- Verify WAN link connectivity

### Can't Ping Between Sites
- Check static IPs are correct
- Verify default gateways point to routers
- Ensure RIP has converged (wait 60 seconds)
- Check `show ip route` for learned routes

### Wireless Clients Can't Connect
- Verify AP is powered on
- Check SSID broadcast
- Confirm client IP is in correct range
- Verify default gateway matches site router

## Project Structure
```
multi-site-rip-network/
├── README.md
├── CONFIGURATION.md
├── multi_access_point_connect.pkt
├── network-topology.png
└── docs/
    ├── RIP-config-guide.txt
    ├── IP-addressing-scheme.txt
    └── troubleshooting-guide.txt
```

## Advantages of This Design

✅ No DHCP servers needed (simpler setup)  
✅ Dynamic routing handles topology changes  
✅ Simple to understand and configure  
✅ Good for learning networking fundamentals  
✅ Scalable to more sites  
✅ Demonstrates WAN concepts  

## Limitations to Know

⚠️ RIP has 15-hop limit (not suitable for large networks)  
⚠️ Static IPs require manual management  
⚠️ No automatic failover for WAN link  
⚠️ RIP slower to converge than OSPF  

## Future Enhancements

- Migrate to OSPF for faster convergence
- Implement DHCP servers at each site
- Add redundant WAN links
- Implement QoS policies
- Add firewall/ACL filtering
- Introduce VLANs at each site

## Requirements

- Cisco Packet Tracer 7.2+
- Basic networking knowledge
- Understanding of IP addressing
- Familiarity with routing concepts

## References

- Cisco RIP Configuration Guide
- Routing Information Protocol (RIP) RFC 2453
- Static IP vs DHCP comparison
- WAN Technology Fundamentals
- Multi-site Network Design

## Support

For issues or questions:
1. Check CONFIGURATION.md for detailed settings
2. Verify all IP addresses match the scheme
3. Ensure RIP is enabled on both routers
4. Test basic connectivity before advanced configs

---

**Project Version:** 1.0  
**Last Updated:** August 2026  
**Difficulty Level:** Intermediate  
**Time to Setup:** 30-45 minutes  
**Learning Time:** 2 hours
