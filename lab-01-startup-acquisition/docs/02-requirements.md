# Lab 01: Technical Requirements & Specifications

## 1. Business Requirements

### 1.1 Critical Constraints
- **Zero Downtime**: No disruption to production services during migration
- **Timeline**: Complete integration within 4 weeks
- **Budget**: Reuse existing equipment where possible
- **Compliance**: Maintain network segmentation for SOX compliance
- **Scalability**: Support 200+ employees within 12 months

### 1.2 Service Level Requirements
| Service | Availability | Max Latency | Packet Loss |
|---------|-------------|-------------|-------------|
| VoIP | 99.9% | <150ms | <1% |
| ERP/CAD | 99.5% | <100ms | <0.5% |
| Email | 99.5% | <200ms | <1% |
| Internet | 99% | <300ms | <2% |

### 1.3 Security Requirements
- Firewall between each site
- Encrypted WAN communications
- Management network isolation
- No direct Internet access for production VLANs

## 2. Network Architecture

### 2.1 Site Topology
```
                    [INTERNET/WAN CLOUD]
                            |
        +-------------------+-------------------+
        |                   |                   |
   [TechCorp HQ]      [TechCorp Plant]    [StartupXYZ]
   San Francisco         San Jose          Palo Alto
        |                   |                   |
   FW01-HQ            FW01-PLANT          FW01-STARTUP
        |                   |                   |
   RTR01-HQ           RTR01-PLANT         RTR01-STARTUP
        |                   |                   |
   SW01-CORE          SW01-PLANT          SW01-STARTUP
   SW02-CORE               |                   |
        |                   |                   |
   [Distribution]     [Access Layer]      [Access Layer]
```

### 2.2 Equipment List

#### TechCorp HQ (San Francisco)
| Device | Hostname | Role | Model |
|--------|----------|------|-------|
| Firewall | FW01-HQ | Edge security, NAT | pfSense / ASAv |
| Router | RTR01-HQ | WAN routing, VPN termination | CSR1000v / IOSv |
| L3 Switch | SW01-CORE-HQ | Core distribution, HSRP primary | IOSvL2 |
| L3 Switch | SW02-CORE-HQ | Core distribution, HSRP secondary | IOSvL2 |

#### TechCorp Plant (San Jose)
| Device | Hostname | Role | Model |
|--------|----------|------|-------|
| Firewall | FW01-PLANT | Edge security, NAT | pfSense / ASAv |
| Router | RTR01-PLANT | WAN routing, VPN termination | CSR1000v / IOSv |
| L3 Switch | SW01-PLANT | Distribution and access | IOSvL2 |

#### StartupXYZ (Palo Alto)
| Device | Hostname | Role | Model |
|--------|----------|------|-------|
| Firewall | FW01-STARTUP | Edge security, NAT | pfSense / ASAv |
| Router | RTR01-STARTUP | WAN routing, VPN termination | CSR1000v / IOSv |
| L3 Switch | SW01-STARTUP | Distribution and access | IOSvL2 |

#### WAN Simulation
| Device | Hostname | Role | Model |
|--------|----------|------|-------|
| WAN Router | RTR-ISP | Internet/WAN simulator | IOSv |

**Total**: 11 network devices + endpoints

### 2.3 VLAN Design

#### TechCorp HQ VLANs
| VLAN ID | Name | Purpose | IP Range (Initial) | IP Range (Final) |
|---------|------|---------|-------------------|------------------|
| 10 | MGMT-HQ | Management | 172.16.10.0/24 | 10.10.10.0/24 |
| 20 | VOICE-HQ | VoIP phones | 172.16.20.0/24 | 10.10.20.0/24 |
| 30 | DATA-HQ | Workstations | 172.16.30.0/24 | 10.10.30.0/24 |
| 40 | SERVERS-HQ | Local servers | 172.16.40.0/24 | 10.10.40.0/24 |

#### TechCorp Plant VLANs
| VLAN ID | Name | Purpose | IP Range (Initial) | IP Range (Final) |
|---------|------|---------|-------------------|------------------|
| 11 | MGMT-PLANT | Management | 172.16.11.0/24 | 10.20.10.0/24 |
| 21 | VOICE-PLANT | VoIP phones | 172.16.21.0/24 | 10.20.20.0/24 |
| 31 | DATA-PLANT | Workstations | 172.16.31.0/24 | 10.20.30.0/24 |
| 51 | INDUSTRIAL-PLANT | Manufacturing equipment | 172.16.51.0/24 | 10.20.50.0/24 |

#### StartupXYZ VLANs
| VLAN ID | Name | Purpose | IP Range (Initial) | IP Range (Final) |
|---------|------|---------|-------------------|------------------|
| 12 | MGMT-STARTUP | Management | 172.16.12.0/24 | 10.30.10.0/24 |
| 32 | DATA-STARTUP | Workstations | 172.16.32.0/24 | 10.30.30.0/24 |
| 42 | DEV-STARTUP | Development servers | 172.16.42.0/24 | 10.30.40.0/24 |
| 62 | GUEST-STARTUP | Guest WiFi | 172.16.62.0/24 | 10.30.60.0/24 |

### 2.4 WAN Connectivity

#### Link Specifications
| Link | From | To | Bandwidth | Latency | Technology |
|------|------|-----|-----------|---------|------------|
| WAN1 | TechCorp HQ | TechCorp Plant | 100 Mbps | 5ms | Metro Ethernet |
| WAN2 | TechCorp HQ | StartupXYZ | 50 Mbps | 8ms | Internet VPN |
| WAN3 | TechCorp Plant | StartupXYZ | 50 Mbps | 10ms | Internet VPN (backup) |

#### WAN IP Addressing
| Device | Interface | IP Address | Purpose |
|--------|-----------|------------|---------|
| RTR-ISP | Gi0/0 | 203.0.113.1/30 | To FW01-HQ |
| RTR-ISP | Gi0/1 | 203.0.113.5/30 | To FW01-PLANT |
| RTR-ISP | Gi0/2 | 203.0.113.9/30 | To FW01-STARTUP |
| FW01-HQ | Outside | 203.0.113.2/30 | WAN interface |
| FW01-PLANT | Outside | 203.0.113.6/30 | WAN interface |
| FW01-STARTUP | Outside | 203.0.113.10/30 | WAN interface |

## 3. Routing Design

### 3.1 OSPF Architecture
- **Process ID**: 1 (all sites)
- **Router ID**: Loopback0 IP
- **Authentication**: MD5 on all interfaces
- **Areas**:
  - **Area 0 (Backbone)**: WAN routers and inter-site links
  - **Area 10**: TechCorp HQ internal networks
  - **Area 20**: TechCorp Plant internal networks
  - **Area 30**: StartupXYZ internal networks

### 3.2 Loopback Addressing
| Device | Loopback0 | Purpose |
|--------|-----------|---------|
| RTR01-HQ | 1.1.1.1/32 | OSPF Router ID, VPN endpoint |
| RTR01-PLANT | 2.2.2.2/32 | OSPF Router ID, VPN endpoint |
| RTR01-STARTUP | 3.3.3.3/32 | OSPF Router ID, VPN endpoint |

## 4. Security Design

### 4.1 IPsec VPN Tunnels
| Tunnel | From | To | IKEv2 PSK | Encryption | Interesting Traffic |
|--------|------|----|-----------|------------|---------------------|
| VPN1 | RTR01-HQ | RTR01-STARTUP | Lab01SecureKey! | AES-256 | 10.10.0.0/16 ↔ 10.30.0.0/16 |
| VPN2 | RTR01-PLANT | RTR01-STARTUP | Lab01SecureKey! | AES-256 | 10.20.0.0/16 ↔ 10.30.0.0/16 |

### 4.2 Firewall Zones
| Zone | Networks | Access Rules |
|------|----------|--------------|
| Outside | WAN links | Deny all inbound by default |
| Inside | All internal VLANs | Allow outbound, deny inbound |
| DMZ | N/A (future use) | N/A |

### 4.3 NAT Configuration
**Phase 2 (Temporary)**: NAT StartupXYZ networks
- 172.16.0.0/16 (StartupXYZ) → 192.168.100.0/22 (translated)

**Phase 3 (Final)**: Remove NAT after renumbering

## 5. High Availability

### 5.1 HSRP Configuration
#### TechCorp HQ
| VLAN | VIP | Priority SW01 | Priority SW02 | Preempt |
|------|-----|---------------|---------------|---------|
| 10 | 10.10.10.1 | 110 | 100 | Yes |
| 20 | 10.10.20.1 | 110 | 100 | Yes |
| 30 | 10.10.30.1 | 110 | 100 | Yes |
| 40 | 10.10.40.1 | 110 | 100 | Yes |

### 5.2 LACP Link Aggregation
- **SW01-CORE-HQ ↔ SW02-CORE-HQ**: Po1 (2x 1Gbps)
- **SW01-CORE-HQ ↔ RTR01-HQ**: Po2 (2x 1Gbps)
- **SW02-CORE-HQ ↔ RTR01-HQ**: Po3 (2x 1Gbps)

## 6. QoS Requirements

### 6.1 Traffic Classification
| Traffic Type | DSCP | Queue | Bandwidth % | Priority |
|--------------|------|-------|-------------|----------|
| VoIP (RTP) | EF (46) | LLQ | 25% | Strict Priority |
| VoIP Signaling | CS3 (24) | Queue 2 | 5% | High |
| CAD/ERP | AF31 (26) | Queue 3 | 30% | Medium |
| Default | 0 | Queue 4 | 40% | Best Effort |

### 6.2 QoS Policy Locations
- Applied outbound on all WAN router interfaces
- Trust DSCP from internal switches
- Mark VoIP traffic at access layer (based on VLAN 20, 21)

## 7. Migration Strategy

### Phase 1: Foundation (Week 1)
**Objective**: Establish basic connectivity
- Configure all device hostnames, management IPs
- Establish WAN connectivity via firewalls
- Configure basic ACLs on firewalls
- Test reachability between routers

### Phase 2: NAT Implementation (Week 1-2)
**Objective**: Connect networks with IP overlap
- Implement NAT on StartupXYZ side
- Configure static routes for NATted networks
- Test connectivity TechCorp → StartupXYZ (with NAT)

### Phase 3: Dynamic Routing (Week 2-3)
**Objective**: Implement OSPF and VPN
- Deploy OSPF multi-area
- Configure IPsec VPN tunnels
- Remove static routes, transition to dynamic routing
- Verify OSPF adjacencies and routing tables

### Phase 4: Advanced Services (Week 3-4)
**Objective**: Redundancy and QoS
- Renumber StartupXYZ networks (remove NAT)
- Configure HSRP on core switches
- Implement LACP bundles
- Deploy QoS policies
- Final validation and documentation

## 8. Validation Criteria

### 8.1 Connectivity Tests
- [ ] Ping from any workstation VLAN to any other VLAN
- [ ] Traceroute shows optimal path via OSPF
- [ ] VPN tunnels up and passing traffic
- [ ] OSPF neighbors in FULL state

### 8.2 Redundancy Tests
- [ ] Shutdown SW01-CORE-HQ: SW02 takes over (HSRP)
- [ ] Break one link in LACP bundle: traffic continues
- [ ] Shutdown primary WAN link: traffic fails over

### 8.3 QoS Verification
- [ ] VoIP packets marked with DSCP EF
- [ ] VoIP traffic prioritized during congestion
- [ ] Latency <150ms for voice during load

### 8.4 Security Validation
- [ ] Cannot ping from Outside zone to Inside
- [ ] IPsec tunnels encrypted (verify with packet capture)
- [ ] OSPF authentication working

## 9. Documentation Deliverables

- [ ] Complete network diagrams (physical, logical, IP scheme)
- [ ] All device configurations exported and saved
- [ ] Test results and verification outputs
- [ ] Troubleshooting log (issues encountered and resolved)
- [ ] Blog article draft (technical lessons learned)
