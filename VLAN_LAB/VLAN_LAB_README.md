# Cisco Packet Tracer: Inter-VLAN Routing Lab

## Project Overview
This lab demonstrates a multi-VLAN network topology with two logically segmented VLANs on a single Cisco switch, configured to communicate across VLAN boundaries using inter-VLAN routing.

**Key Concepts:** VLAN segmentation, 802.1Q encapsulation, trunk/access ports, inter-VLAN routing, network troubleshooting

## Network Topology

```
┌──────────────────────────────────────────────────────┐
│         Cisco Switch (Single Device)                  │
├──────────────────────────────────────────────────────┤
│                                                        │
│  VLAN 10 (Finance Dept)      VLAN 20 (Tech Dept)     │
│  ├─ Finance-PC1              ├─ Tech-PC1             │
│  ├─ Finance-PC2              ├─ Tech-PC2             │
│  └─ Finance-PC3              └─ Tech-PC3             │
│                                                        │
│  [Trunk Port for Inter-VLAN Routing]                 │
│                                                        │
└──────────────────────────────────────────────────────┘
```

## Configuration Details

### VLAN Setup
- **VLAN 10:** Finance Department
  - IP Subnet: 192.168.10.0/24
  - Gateway: 192.168.10.1
  - Purpose: Finance workstations and departmental resources
  
- **VLAN 20:** Technical Department
  - IP Subnet: 192.168.20.0/24
  - Gateway: 192.168.20.1
  - Purpose: Technical team workstations and infrastructure tools

### Switch Configuration

#### VLAN Creation
```
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name Finance
Switch(config-vlan)# exit
Switch(config)# vlan 20
Switch(config-vlan)# name Technical
Switch(config-vlan)# exit
```

#### Access Port Configuration (FastEthernet 0/1)
```
Switch(config)# interface fastethernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit
```

#### Trunk Port Configuration (FastEthernet 0/24)
```
Switch(config)# interface fastethernet 0/24
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk encapsulation dot1q
Switch(config-if)# switchport trunk allowed vlan 10,20
Switch(config-if)# exit
```

### Inter-VLAN Routing
Routing between VLANs is enabled via a routing device (router or Layer 3 switch) connected to the trunk port with subinterfaces:

```
Router> enable
Router# configure terminal
Router(config)# interface fastethernet 0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface fastethernet 0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit
```

## Testing & Verification

### Connectivity Tests
1. **Same VLAN Communication (No Routing Required)**
   - Ping from Finance-PC1 (VLAN 10) → Finance-PC2 (VLAN 10): ✓ Successful

2. **Cross-VLAN Communication (Requires Routing)**
   - Ping from Finance-PC1 (VLAN 10) → Tech-PC1 (VLAN 20): ✓ Successful
   - Ping from Finance-PC3 (VLAN 10) → Tech-PC2 (VLAN 20): ✓ Successful

### Verification Commands
```
Switch# show vlan brief
Switch# show interfaces switchport
Switch# show interfaces trunk

Router# show ip route
Router# ping 192.168.20.1 (from VLAN 10 device)
```

## Key Learning Outcomes
- ✓ Understanding VLAN segmentation and logical network division
- ✓ Configuring access and trunk ports on Cisco switches
- ✓ Implementing 802.1Q encapsulation for VLAN tagging
- ✓ Setting up inter-VLAN routing using subinterfaces
- ✓ Testing end-to-end connectivity across VLAN boundaries
- ✓ Troubleshooting common VLAN communication issues

## Technologies Used
- **Cisco Packet Tracer** — Network simulation and configuration tool
- **Cisco IOS** — Operating system for switches and routers
- **802.1Q (dot1q)** — VLAN encapsulation standard
- **TCP/IP** — Network communication protocol stack

## Files Included
- `vlan-lab.pkt` — Cisco Packet Tracer topology file (ready to load and modify)
- `README.md` — This documentation file

## How to Use This Lab

1. **Load the Packet Tracer File:**
   - Open Cisco Packet Tracer
   - File → Open → Select `vlan-lab.pkt`

2. **Explore the Topology:**
   - Examine the switch and router configuration
   - Review VLAN assignment for each port
   - Check subinterface configuration on the router

3. **Modify & Experiment:**
   - Change VLAN IP subnets
   - Add additional VLANs (VLAN 30, VLAN 40)
   - Implement access control lists (ACLs) for inter-VLAN security
   - Test with additional end devices

4. **Troubleshooting Scenarios:**
   - Disable a trunk port and observe connectivity loss
   - Misconfigure an access port VLAN and verify isolation
   - Change the native VLAN on trunk ports and observe behavior

## Extensions & Advanced Topics

### Potential Enhancements
- Implement VLAN access control lists (VACLs) to restrict traffic between VLANs
- Add port security to prevent unauthorized device connections
- Configure VLAN trunking protocol (VTP) for dynamic VLAN management
- Implement 802.1X for dynamic VLAN assignment based on user authentication
- Add a Layer 3 switch to replace the router for more efficient inter-VLAN routing

## References
- Cisco VLAN Configuration Guide: https://www.cisco.com/c/en/us/support/docs/
- CCNA Study Guide: VLAN Fundamentals (Chapter 8-10)
- Cisco Packet Tracer Official Documentation
- Jeremy's IT Lab (YouTube)

---

**Lab Created By:** Stephen Oluwatofunmi Ogunde  
**Date:** 2026  
**Skill Level:** Entry-Level / Associate  
**CCNA Alignment:** VLAN Segmentation, Inter-VLAN Routing (CCNA Core Topics)
