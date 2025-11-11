# Lab 01: The Startup Acquisition

## 🎯 Scenario Overview

**TechCorp**, a 150-employee aerospace engineering firm, has just acquired **StartupXYZ**, an innovative 30-person software company specializing in drone control systems. As the lead network engineer, you're tasked with integrating both networks while maintaining zero downtime for critical business operations.

**The Challenge**: Both companies independently chose the 172.16.0.0/16 address space. Your mission is to connect these networks, resolve IP conflicts, and create a unified, secure, and redundant infrastructure.

## 📊 Lab Statistics

- **Difficulty**: Intermediate
- **Estimated Time**: 8-12 hours
- **Devices**: 11 network devices
- **Technologies**: OSPF, IPsec VPN, NAT, HSRP, LACP, QoS
- **Lines of Config**: ~800 lines

## 🏢 Company Profiles

### TechCorp (Acquirer)
- **Business**: Aerospace subcontracting and mechanical engineering
- **Employees**: 150
- **Sites**:
  - HQ in San Francisco (120 employees)
  - Manufacturing plant in San Jose (30 employees)
- **Critical Services**:
  - CAD/CAM servers
  - ERP system
  - VoIP phone system (50 extensions)
  - Email and collaboration tools

### StartupXYZ (Acquired)
- **Business**: Drone control software and AI navigation
- **Employees**: 30
- **Sites**:
  - Main office in Palo Alto (25 employees)
  - 5 remote developers
- **Critical Services**:
  - Development servers and CI/CD
  - Cloud-based code repositories
  - Real-time drone telemetry systems
  - VPN for remote workers

## 🎓 Learning Objectives

By completing this lab, you will:

1. **Master IP conflict resolution** using NAT and systematic renumbering
2. **Implement dynamic routing** with OSPF multi-area design
3. **Deploy site-to-site VPN** with IPsec for secure WAN connectivity
4. **Configure network redundancy** using HSRP and LACP
5. **Implement QoS policies** for voice traffic prioritization
6. **Practice zero-downtime migration** strategies
7. **Document enterprise-grade networks** professionally

## 🗂️ Documentation Structure

| Document | Description |
|----------|-------------|
| [01-scenario.md](docs/01-scenario.md) | Detailed business context and constraints |
| [02-requirements.md](docs/02-requirements.md) | Technical and business requirements |
| [03-topology.md](docs/03-topology.md) | Network architecture and design decisions |
| [04-addressing-plan.md](docs/04-addressing-plan.md) | Complete IP addressing scheme |
| [05-implementation-guide.md](docs/05-implementation-guide.md) | Step-by-step deployment instructions |
| [06-validation.md](docs/06-validation.md) | Testing procedures and verification |
| [07-troubleshooting.md](docs/07-troubleshooting.md) | Common issues and solutions |

## 🏗️ Network Architecture

### Physical Topology
- 4 Routers (WAN connectivity and routing)
- 4 Layer 3 Switches (distribution and access)
- 2 Firewalls (security and NAT)
- 12 VLANs across three sites
- 3 WAN links (2 primary, 1 backup)

### Logical Design
- OSPF multi-area (Area 0, Area 10, Area 20)
- IPsec VPN tunnels over WAN
- NAT for IP conflict resolution
- HSRP for gateway redundancy
- LACP for link aggregation

## 📈 Implementation Phases

### Phase 1: Foundation (2-3 hours)
- Basic device configuration
- Physical connectivity
- Initial IP addressing

### Phase 2: WAN Connectivity (2-3 hours)
- WAN router configuration
- NAT implementation
- Basic reachability testing

### Phase 3: Routing & Security (2-3 hours)
- OSPF deployment
- IPsec VPN configuration
- Firewall rules

### Phase 4: Advanced Services (2-3 hours)
- HSRP configuration
- LACP implementation
- QoS policies
- Final validation

## 🔧 Technical Requirements

### EVE-NG Images Needed
- Cisco IOSv or CSR1000v (routers)
- Cisco IOSvL2 (Layer 3 switches)
- pfSense or Cisco ASAv (firewalls)
- VPC or Linux (endpoints for testing)

### Recommended Resources
- CPU: 4 cores minimum
- RAM: 8GB minimum
- Disk: 50GB free space

## ✅ Success Criteria

The lab is complete when:
- [ ] All sites can communicate without IP conflicts
- [ ] OSPF adjacencies established and stable
- [ ] IPsec VPN tunnels operational
- [ ] VoIP traffic prioritized with QoS
- [ ] HSRP providing gateway redundancy
- [ ] LACP bundles operational
- [ ] Zero packet loss during failover tests
- [ ] Complete documentation written
- [ ] Blog article published

## 📸 Screenshots

Progress screenshots are stored in the `screenshots/` directory, organized by phase.

## 📝 Blog Article

Draft articles for publication:
- `articles/draft-en.md` - English version
- Technical lessons learned and best practices

## 🔗 Useful Resources

- [Cisco OSPF Configuration Guide](https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/7039-1.html)
- [IPsec VPN Fundamentals](https://www.cisco.com/c/en/us/support/docs/security-vpn/ipsec-negotiation-ike-protocols/14106-how-vpn-works.html)
- [HSRP Configuration](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipapp_fhrp/configuration/xe-16/fhp-xe-16-book/fhp-hsrp.html)

## 🚀 Getting Started

1. Review all documentation in the `docs/` folder
2. Study the topology diagrams in `diagrams/`
3. Import the EVE-NG topology from `eve-ng/`
4. Follow the implementation guide step-by-step
5. Document your progress with screenshots
6. Validate using the test procedures
7. Write your blog article!

---

**Ready to integrate these networks? Let's go! 🚀**
