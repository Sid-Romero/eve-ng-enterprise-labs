# EVE-NG Enterprise Labs

[![Lab Status](https://img.shields.io/badge/Labs-In%20Progress-yellow)]()
[![Documentation](https://img.shields.io/badge/Docs-Available-green)]()
[![License](https://img.shields.io/badge/License-MIT-blue.svg)]()

## 📚 About This Repository

This repository contains a comprehensive series of enterprise-grade network engineering labs designed for hands-on learning and skill development. Each lab simulates real-world scenarios that network engineers face in production environments.

## 🎯 Lab Series Overview

### Lab 01: The Startup Acquisition ✅ In Progress
**Scenario**: Network integration following a corporate acquisition
**Focus**: Multi-site networking, OSPF, VPN, NAT, redundancy
**Difficulty**: Intermediate
**Estimated Time**: 8-12 hours

[View Lab Details](./lab-01-startup-acquisition/)

### Lab 02: The ISP Simulator (Coming Soon)
**Scenario**: Building a mini-ISP with BGP and MPLS
**Focus**: BGP, MPLS L3VPN, traffic engineering
**Difficulty**: Advanced

### Lab 03: The Data Center Fabric (Coming Soon)
**Scenario**: Modern datacenter with VXLAN overlay
**Focus**: VXLAN, EVPN, BGP, automation
**Difficulty**: Advanced

## 🛠️ Technical Stack

- **Virtualization**: EVE-NG
- **Network Devices**: Cisco IOS, CSR1000v, IOSvL2, pfSense
- **Automation**: Ansible, Python
- **Documentation**: Markdown, DrawIO
- **Version Control**: Git/GitHub

## 📖 Documentation Structure

Each lab contains:
-  Detailed scenario and business context
-  Network topology diagrams
-  IP addressing schemes
-  Step-by-step implementation guides
-  Validation procedures
-  Screenshots and verification outputs

##  Getting Started

### Prerequisites
- EVE-NG installed (Community or Professional)
- Basic understanding of networking concepts
- DrawIO for viewing diagrams
- Git for version control

### Using These Labs

1. Clone the repository:
```bash
git clone https://github.com/Sid-Romero/eve-ng-enterprise-labs
cd eve-ng-enterprise-labs
```

2. Navigate to desired lab:
```bash
cd lab-01-startup-acquisition
```

3. Read the documentation in order:
   - `docs/01-scenario.md` - Understand the business context
   - `docs/02-requirements.md` - Review technical requirements
   - `docs/03-topology.md` - Study the network design
   - `docs/04-addressing-plan.md` - Learn the IP scheme
   - `docs/05-implementation-guide.md` - Follow deployment steps

4. Import the EVE-NG topology:
   - Upload `eve-ng/*.unl` file to your EVE-NG server
   - Import required device images

## 📝 Blog Articles

Technical writeups and lessons learned from each lab are published on:
- [Medium](https://medium.com/@badjisidya])
- [LinkedIn](https://www.linkedin.com/in/el-hadji-sidya-badji-35a647264/)

## Contributing

These labs are personal learning projects, but suggestions and feedback are welcome! Feel free to:
- Open issues for questions or improvements
- Share your own implementations
- Suggest new lab scenarios

##  License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👤 Author

**Sid**
Cybersecurity & Network Engineering Student
[LinkedIn](https://www.linkedin.com/in/el-hadji-sidya-badji-35a647264/) | [Medium](https://medium.com/@badjisidya])

##  Acknowledgments

- EVE-NG community for the excellent virtualization platform
- Network engineering community for inspiration and knowledge sharing

---

⭐ Star this repository if you find it useful!
