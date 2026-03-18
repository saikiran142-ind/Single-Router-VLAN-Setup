# Single-Router-VLAN-Setup

# 1-Router, 1-Switch, 4-PC Network with VLANs

This project demonstrates **Inter-VLAN Routing** (Router-on-a-Stick) using a single router and switch.

## 🛠️ Network Topology
- **VLAN 10 (IT):** PC 1 & PC 2
- **VLAN 20 (SALES):** PC 3 & PC 4
- **Router-on-a-Stick:** Enabled via sub-interfaces on Router Gig0/0/0.

<img width="1920" height="1080" alt="Screenshot 2026-03-18 193229" src="https://github.com/user-attachments/assets/070b017c-23ff-4ad1-a806-10f27b81472d" />



## Switch Configuration 

1. **Switch>** enable
2. **Switch#** configure terminal

! Create VLANs
---
1. **Switch(config)#** vlan 10
2. **Switch(config-vlan)#** name IT
3. **Switch(config)#** vlan 20
4. **Switch(config-vlan)#** name SALES

! Assign Ports to VLAN 10 (PC 1 & 2)
----
#### PC1

1. **Switch(config)#** interface fa0/1
2. **Switch(config-if)#** switchport mode access
3. **Switch(config-if)#** switchport access vlan 10

#### PC2

1. **Switch(config)#** interface range fa0/2
2. **Switch(config-if)#** switchport mode access
3. **Switch(config-if)#** switchport access vlan 10

! Assign Ports to VLAN 20 (PC 3 & 4)
----
#### PC3

1. **Switch(config)#** interface range fa0/3
2. **Switch(config-if)#** switchport mode access
3. **Switch(config-if)#** switchport access vlan 20
   
#### PC4

1. **Switch(config)#** interface range fa0/4
2. **Switch(config-if)#** switchport mode access
3. **Switch(config-if)#** switchport access vlan 20



## 📊 IP Addressing Table


| Device | VLAN | IP Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **PC 1** | 10 | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 |
| **PC 2** | 10 | 192.168.1.20 | 255.255.255.0 | 192.168.1.1 |
| **PC 3** | 20 | 192.168.2.10 | 255.255.255.0 | 192.168.2.1 |
| **PC 4** | 20 | 192.168.2.20 | 255.255.255.0 | 192.168.2.1 |

## ⚙️ Configuration Snippet (Router)

1. **Router>** enable
2. **Router#** configure terminal
3. **Router(config)#** interface gig0/0
4. **Router(config-if)#** no shutdown

! Sub-interface for VLAN 10
---

1. **Router(config)#** interface gig0/0.10
2. **Router(config-subif)#** encapsulation dot1Q 10
3. **Router(config-subif)#** ip address 192.168.1.1 255.255.255.0

! Sub-interface for VLAN 20
---

1. **Router(config)#** interface gig0/0.20
2. **Router(config-subif)#** encapsulation dot1Q 20
3. **Router(config-subif)#** ip address 192.168.2.1 255.255.255.0

### The last setp

! Trunk Port to Router (Gig 0/1)
---
1. **Switch(config)#** interface gig0/1
2. **Switch(config-if)#** switchport mode trunk
----

## 🚀 Key Features Implemented
1. **VLAN Creation:** Created VLAN 10 and VLAN 20 on the switch.
2. **Access Ports:** Assigned specific ports to respective VLANs for the PCs.
3. **Trunking:** Configured a Trunk link between the Switch and the Router to carry traffic for multiple VLANs.
4. **Router-on-a-Stick:** Configured sub-interfaces with IEEE 802.1Q encapsulation on the router to act as default gateways.

## ✅ Verification
- **Ping Test:** PC 0 (VLAN 10) can successfully ping PC 4 (VLAN 20).

  <img width="1003" height="647" alt="Screenshot 2026-03-18 195624" src="https://github.com/user-attachments/assets/6ba93f3b-da68-4c99-81db-6e681b40b770" />


- **CLI Check:** Verified routing table using `show ip route` and trunk status using `show interfaces trunk`.
