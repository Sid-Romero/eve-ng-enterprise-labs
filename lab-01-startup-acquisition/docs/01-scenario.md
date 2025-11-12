# Lab 01: The Startup Acquisition - Business Scenario

## Executive Summary

**Date**: November 2024  
**Project**: Post-Merger Network Integration  
**Timeline**: 4 weeks  
**Budget**: $50,000 (equipment reuse prioritized)  
**Project Lead**: You (Senior Network Engineer)

TechCorp, a well-established aerospace engineering firm, has acquired StartupXYZ, an innovative drone software company. Your mission is to integrate both networks while maintaining zero downtime and meeting strict compliance requirements.

---

## Company Profiles

### TechCorp Industries (Acquirer)

**Industry**: Aerospace Manufacturing and Engineering  
**Founded**: 1987  
**Employees**: 150  
**Annual Revenue**: $45M  
**Headquarters**: San Francisco, CA

#### Business Operations
TechCorp specializes in precision manufacturing of aerospace components for major defense contractors (Boeing, Lockheed Martin, Northrop Grumman). Their core business includes:
- CNC machining and fabrication
- CAD/CAM design services
- Quality assurance and testing
- Supply chain management

#### Sites

**San Francisco HQ (120 employees)**
- Executive team (15)
- Engineering department (40)
- Sales and operations (30)
- IT and administration (20)
- Finance and HR (15)

**San Jose Manufacturing Plant (30 employees)**
- Plant manager and supervisors (5)
- Machinists and technicians (20)
- Quality control (5)

#### Technology Environment
- **ERP System**: SAP Business One (critical - 24/7 uptime required)
- **CAD Software**: SolidWorks, AutoCAD (large file transfers)
- **Email**: Microsoft Exchange Server (on-premises)
- **VoIP System**: Cisco Unified Communications Manager (50 extensions)
- **File Storage**: Windows File Server (2TB data)
- **Backup**: Daily incremental, weekly full

#### IT Infrastructure (Before Acquisition)
- Aging network equipment (8+ years old)
- Basic VLAN segmentation
- No redundancy at HQ
- Flat network at Plant
- Manual firewall rule management
- Static routing between sites

#### Pain Points
- No disaster recovery plan
- Single points of failure at HQ
- Slow WAN link to Plant (congestion during CAD file transfers)
- Limited remote access capability
- No network monitoring tools

---

### StartupXYZ Technologies (Acquired Company)

**Industry**: Software Development (Autonomous Drone Systems)  
**Founded**: 2019  
**Employees**: 30  
**Annual Revenue**: $8M (growing 200% YoY)  
**Headquarters**: Palo Alto, CA

#### Business Operations
StartupXYZ develops AI-powered flight control software for commercial drones. Their technology enables:
- Autonomous navigation in GPS-denied environments
- Real-time obstacle avoidance
- Swarm coordination algorithms
- Edge computing for drone fleets

Their software is licensed to drone manufacturers and is integrated into agricultural, delivery, and inspection drones.

#### Sites

**Palo Alto Office (25 employees on-site + 5 remote)**
- CTO and engineering leads (5)
- Software developers (15)
- DevOps engineers (3)
- Sales and marketing (2)
- Remote developers (5 - distributed across US)

#### Technology Environment
- **Development**: GitLab (self-hosted), Jenkins CI/CD
- **Cloud Infrastructure**: Heavy AWS usage (EC2, S3, RDS)
- **Collaboration**: Slack, Zoom, Google Workspace
- **Testing**: Hardware-in-the-loop test rigs (drone simulators)
- **Version Control**: Git repositories (500GB+ code and test data)

#### IT Infrastructure (Before Acquisition)
- Modern network (installed 2022)
- All Gigabit switches
- FortiGate firewall
- Guest WiFi for visitors
- Cloud-first mentality
- VPN for remote developers (OpenVPN)

#### Strengths
- Modern infrastructure
- Cloud-native applications
- Strong security practices
- Automated deployments

#### Challenges
- Small IT team (1 person part-time)
- No formal change management
- Documentation gaps
- Rapid growth straining resources

---

## The Acquisition

### Strategic Rationale

TechCorp's CEO identified autonomous drone technology as critical to their future. Defense contractors are increasingly requesting integrated drone capabilities for:
- Warehouse inventory management
- Facility security patrols
- Inspection of large aerospace structures
- Supply chain logistics

By acquiring StartupXYZ, TechCorp gains:
1. **Technology IP**: Cutting-edge drone AI algorithms
2. **Talent**: 20 software engineers (TechCorp had zero)
3. **Market Position**: Entry into the commercial drone market
4. **Revenue Diversification**: Software licensing vs hardware manufacturing

### Acquisition Terms

- **Deal Value**: $35M (cash and stock)
- **Integration Timeline**: 6 months full integration
- **IT Integration Deadline**: 4 weeks (network must be operational)
- **Employee Retention**: All StartupXYZ employees retained
- **Brand**: StartupXYZ becomes "TechCorp Autonomous Systems Division"

---

## Business Requirements

### Critical Constraints

1. **Zero Downtime Mandate**
   - TechCorp's ERP system operates 24/7 (manufacturing runs three shifts)
   - Any downtime costs $15,000/hour in lost production
   - SAP system absolutely cannot go offline during business hours
   - VoIP system must remain operational (911 compliance)

2. **Timeline Pressure**
   - 4-week deadline is non-negotiable
   - Week 5: StartupXYZ lease ends (must vacate office)
   - Week 6: Major customer demo (needs integrated systems)
   - Board presentation in Week 8 (show integration success)

3. **Budget Constraints**
   - $50,000 allocated for network integration
   - Reuse existing equipment where possible
   - No budget for forklift upgrade of all infrastructure
   - Must justify any new equipment purchases

4. **Compliance Requirements**
   - **SOX Compliance**: TechCorp is publicly traded, must maintain financial system segregation
   - **ITAR**: Some aerospace work is export-controlled, requires network segmentation
   - **CMMC Level 2**: Defense contractor requirement (in progress, network must support)
   - **GDPR**: StartupXYZ has European customers, data protection required

5. **Security Requirements**
   - All WAN communications must be encrypted
   - Management network must be isolated (out-of-band preferred)
   - No direct Internet access for production VLANs
   - Firewall inspection between all sites
   - Multi-factor authentication for remote access

6. **Performance SLAs**

| Service | Availability | Max Latency | Packet Loss | Business Impact if Down |
|---------|-------------|-------------|-------------|------------------------|
| ERP/SAP | 99.9% | <100ms | <0.5% | Production halt |
| VoIP | 99.9% | <150ms | <1% | Cannot communicate |
| CAD/CAM | 99.5% | <200ms | <1% | Design delays |
| Email | 99.5% | <300ms | <2% | Business disruption |
| Internet | 99% | <500ms | <5% | Limited impact |

---

## The Technical Challenge

### Discovered Problem: IP Address Overlap

During the initial site survey, the network team discovered a critical issue:

**Both companies independently chose the 172.16.0.0/16 private address space.**

#### TechCorp's Addressing:
- HQ Management: 172.16.10.0/24
- HQ Voice: 172.16.20.0/24
- HQ Data: 172.16.30.0/24
- HQ Servers: 172.16.40.0/24
- Plant Management: 172.16.11.0/24
- Plant Voice: 172.16.21.0/24
- Plant Data: 172.16.31.0/24
- Plant Industrial: 172.16.51.0/24

#### StartupXYZ's Addressing:
- Management: 172.16.12.0/24
- Data: 172.16.32.0/24
- Dev Servers: 172.16.42.0/24
- Guest WiFi: 172.16.62.0/24

**This overlap makes direct routing impossible.** You cannot have 172.16.x.x networks communicating across the WAN without conflicts.

### Stakeholder Reactions

**TechCorp CTO (Your Boss)**:  
"We absolutely cannot renumber our networks. We have 200+ devices with static IPs in our manufacturing plant. Changing those would require recertifying our quality management system with the FAA. That's a 6-month process. StartupXYZ has to change."

**StartupXYZ CTO (Now Reports to You)**:  
"Our entire infrastructure is in 172.16.0.0/16. Every hardcoded IP in our codebase, every configuration file, every API endpoint. We have 500+ repositories. Renumbering would take months of regression testing. Can't TechCorp change?"

**Your CEO**:  
"I don't care whose fault it is. Fix it. I need both companies talking to each other by next month. Figure it out."

---

## Your Solution Approach

After analyzing the situation, you propose a **four-phase migration strategy**:

### Phase 1: Foundation (Week 1)
**Objective**: Establish basic WAN connectivity

- Deploy firewalls at each site
- Configure WAN links through ISP
- Set up management VLANs
- Document existing infrastructure
- Test basic reachability between routers

**Success Criteria**: Can ping between routers

---

### Phase 2: Temporary NAT (Week 1-2)
**Objective**: Enable immediate cross-company communication

**Strategy**: Implement NAT for StartupXYZ networks as a temporary measure
- Translate StartupXYZ's 172.16.0.0/16 to 192.168.100.0/22
- Configure static routes on TechCorp side to reach NATted networks
- This buys time for proper renumbering while allowing business to proceed

**Why This Works**:
- TechCorp can reach StartupXYZ via 192.168.100.x addresses
- StartupXYZ sees TechCorp's real 172.16.x.x addresses (no NAT needed their direction)
- Zero downtime implementation
- Temporary solution while planning full renumbering

**Success Criteria**: TechCorp HQ can access StartupXYZ resources via NAT

---

### Phase 3: Dynamic Routing and VPN (Week 2-3)
**Objective**: Implement scalable routing and secure connectivity

- Deploy OSPF multi-area design
- Configure IPsec VPN tunnels between sites
- Encrypt all inter-site traffic
- Remove static routes, transition to dynamic routing
- Implement OSPF authentication

**Success Criteria**: 
- OSPF adjacencies stable
- VPN tunnels encrypted and operational
- Automatic route propagation working

---

### Phase 4: Advanced Services and Renumbering (Week 3-4)
**Objective**: Production-grade infrastructure with redundancy

- **Renumber StartupXYZ** to new addressing scheme:
  - StartupXYZ Management: 172.16.12.0/24 → 10.30.10.0/24
  - StartupXYZ Data: 172.16.32.0/24 → 10.30.30.0/24
  - StartupXYZ Dev: 172.16.42.0/24 → 10.30.40.0/24
  - StartupXYZ Guest: 172.16.62.0/24 → 10.30.60.0/24

- **Simultaneously renumber TechCorp** to clean scheme:
  - TechCorp HQ: 172.16.x.x → 10.10.x.x
  - TechCorp Plant: 172.16.x.x → 10.20.x.x

- **Remove all NAT** (no longer needed with unique addressing)

- **Deploy redundancy at TechCorp HQ**:
  - Configure HSRP for gateway redundancy
  - Implement LACP for link aggregation
  - Ensure no single point of failure

- **Implement QoS**:
  - Prioritize VoIP traffic (DSCP EF)
  - Guarantee bandwidth for SAP/ERP
  - Classify and mark traffic at edge

**Success Criteria**:
- All sites using unique IP space
- NAT removed
- HSRP failover tested
- LACP bundles operational
- QoS policies enforced
- Zero packet loss during failover tests

---

## Stakeholder Communication Plan

### Weekly Status Reports To:

**CEO**: High-level progress, risks, timeline
**TechCorp CTO**: Technical details, TechCorp impact
**StartupXYZ CTO**: Migration schedule, StartupXYZ impact
**Plant Manager**: Manufacturing downtime windows
**HR**: Employee communication (email/phone disruptions)

### Scheduled Maintenance Windows

- **Phase 1**: No downtime (new equipment only)
- **Phase 2**: 2-hour window (Saturday 6 AM - 8 AM) for NAT implementation
- **Phase 3**: 4-hour window (Sunday 2 AM - 6 AM) for OSPF/VPN deployment
- **Phase 4**: 
  - TechCorp renumbering: Friday 6 PM - Monday 6 AM (full weekend)
  - StartupXYZ renumbering: Saturday 8 AM - 6 PM (10 hours)
  - Rollback plan prepared for both

---

## Success Metrics

### Technical KPIs

- [ ] 100% site-to-site reachability
- [ ] <150ms latency for VoIP (99th percentile)
- [ ] <100ms latency for ERP (99th percentile)
- [ ] 0 unplanned outages during migration
- [ ] OSPF convergence <5 seconds
- [ ] VPN tunnels 99.9% uptime
- [ ] HSRP failover <3 seconds

### Business KPIs

- [ ] Zero production downtime during business hours
- [ ] On-time completion (Week 4)
- [ ] Under budget ($50,000)
- [ ] All compliance requirements met
- [ ] Positive feedback from both CTOs
- [ ] CEO approval for Phase 2 projects

---

## Risks and Mitigation

### Risk 1: Renumbering Breaks Applications
**Probability**: High  
**Impact**: Critical  
**Mitigation**: 
- Comprehensive application inventory
- Test renumbering in lab environment first
- Coordinate with application owners
- Maintain NAT during transition period
- Rollback plan ready

### Risk 2: VPN Performance Issues
**Probability**: Medium  
**Impact**: High  
**Mitigation**:
- Size VPN concentrators appropriately
- Monitor CPU/bandwidth during testing
- QoS to prioritize VPN traffic
- Backup WAN circuits available

### Risk 3: OSPF Instability
**Probability**: Low  
**Impact**: High  
**Mitigation**:
- Implement OSPF authentication
- Use BFD for fast failure detection
- Configure route filtering at ABRs
- Monitor LSA database size

### Risk 4: Timeline Slippage
**Probability**: Medium  
**Impact**: High  
**Mitigation**:
- Weekly checkpoints
- Parallel work streams where possible
- Pre-stage equipment
- After-hours work approved if needed

### Risk 5: Firewall Misconfiguration
**Probability**: Medium  
**Impact**: Critical  
**Mitigation**:
- Peer review all firewall rules
- Test in lab first
- Default-deny policy
- Change control process
- Emergency rollback procedure

---

## Post-Integration Vision (6 Months)

Once the initial 4-week integration is complete, the long-term roadmap includes:

1. **Unified Security Operations Center (SOC)**
   - SIEM deployment (Splunk or ELK)
   - Centralized log management
   - 24/7 monitoring

2. **SD-WAN Migration**
   - Replace traditional MPLS with SD-WAN
   - Improve WAN performance and reduce costs
   - Leverage multiple ISPs for redundancy

3. **Network Automation**
   - Ansible playbooks for configuration management
   - Git-based infrastructure as code
   - Automated compliance checking

4. **Zero Trust Architecture**
   - Micro-segmentation with firewalls
   - Identity-based access control
   - Least-privilege networking

---

## Your Assignment

As the lead network engineer, you are responsible for:

1. Designing the network architecture
2. Creating detailed IP addressing plan
3. Configuring all network devices
4. Testing and validating connectivity
5. Documenting the implementation
6. Training the operations team
7. Creating runbooks for troubleshooting

**You have 4 weeks. The clock is ticking.**

**Welcome to Lab 01: The Startup Acquisition.**

---

*This is a realistic scenario based on actual enterprise network integrations. The technical challenges you'll face mirror those encountered in real-world M&A network projects.*
