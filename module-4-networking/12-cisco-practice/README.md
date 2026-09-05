# Cisco Packet Tracer — Networking Practicals

This repository contains my practical work on **basic network configuration, VLANs, network segmentation, and inter-VLAN routing** using **Cisco Packet Tracer**.

The practicals were designed as simulated networking labs to develop hands-on experience with Cisco switches, routers, IPv4 addressing, VLANs, trunking, and connectivity troubleshooting.

---

## 📚 Tasks

### Task 1 — Basic Network Configuration

Configured a basic LAN using:

* 3 Cisco 2960 switches
* 9 PCs
* IPv4 network: `192.168.1.0/24`
* Switch-to-switch connections
* PC IP address configuration
* Connectivity testing using `ping`
* Simple PDU testing in Packet Tracer

### Network

```text
192.168.1.0/24
```

All PCs were configured within the same network to demonstrate basic Layer 2 connectivity.

---

### Task 2 — VLAN Configuration

Created three VLANs to logically separate different departments.

| VLAN | Name  | PCs           |
| ---: | ----- | ------------- |
|   10 | SALES | PC1, PC4, PC7 |
|   20 | HR    | PC2, PC5, PC8 |
|   30 | IT    | PC3, PC6, PC9 |

### VLAN Networks

| VLAN | Network           |
| ---: | ----------------- |
|   10 | `192.168.10.0/24` |
|   20 | `192.168.20.0/24` |
|   30 | `192.168.30.0/24` |

The following concepts were configured and tested:

* VLAN creation
* Access ports
* VLAN assignment
* Trunk links
* VLAN segmentation
* Connectivity testing

### VLAN Concept

```text
                 Switch Network
                      |
        +-------------+-------------+
        |             |             |
     VLAN 10       VLAN 20       VLAN 30
      SALES           HR            IT
        |             |             |
      PCs           PCs           PCs
```

Devices in different VLANs are logically separated at Layer 2.

---

### Task 3 — Inter-VLAN Routing

Configured **Router-on-a-Stick** to allow communication between different VLANs.

The router was configured with multiple subinterfaces, with each subinterface acting as the default gateway for its VLAN.

### Router Subinterfaces

| Interface | VLAN | Gateway          |
| --------- | ---: | ---------------- |
| `G0/0.10` |   10 | `192.168.10.254` |
| `G0/0.20` |   20 | `192.168.20.254` |
| `G0/0.30` |   30 | `192.168.30.254` |

### Configuration Concepts

* Router subinterfaces
* `802.1Q` VLAN encapsulation
* Default gateways
* Trunk connection between router and switch
* Inter-VLAN routing
* Connectivity testing

### Router-on-a-Stick Concept

```text
                 Router
              G0/0 Interface
                    |
                 Trunk Link
                    |
                 Switch
          _________|_________
         |         |         |
      VLAN 10   VLAN 20   VLAN 30
       SALES       HR        IT
```

The router receives tagged traffic from the trunk and routes traffic between the different VLAN networks.

---

## 🧪 Testing

Connectivity between devices was tested using `ping`.

Example:

```bash
ping <destination-ip>
```

Examples:

```bash
ping 192.168.20.1
ping 192.168.30.1
```

Packet Tracer's **Simple PDU** tool was also used to visually test communication between network devices.

---

## 🔍 Cisco Verification Commands

### Display VLAN Configuration

```cisco
show vlan brief
```

### Display Trunk Information

```cisco
show interfaces trunk
```

### Display Interface Status and IP Addresses

```cisco
show ip interface brief
```

### Verify Router Subinterfaces

```cisco
show running-config
```

These commands help verify VLAN assignments, trunk configuration, interface status, and router configuration.

---

## 🛠️ Technologies Used

* Cisco Packet Tracer
* Cisco 2960 Switch
* Cisco 2911 Router
* IPv4
* VLAN
* Access Ports
* Trunking
* IEEE 802.1Q
* Router-on-a-Stick
* ICMP / `ping`

---

## 📖 Key Learning

Through these practicals, I learned:

* Basic IPv4 addressing
* Basic switch configuration
* VLAN creation and assignment
* Access port configuration
* Trunk port configuration
* Network segmentation using VLANs
* Inter-VLAN routing
* Router subinterfaces
* IEEE 802.1Q encapsulation
* Default gateway configuration
* Cisco network verification commands
* Basic network connectivity troubleshooting

---

## 🔐 Security & Documentation Note

The IP addresses used in these practicals are **private RFC1918 address ranges** intended for lab and internal networking.

No real public IP addresses, credentials, passwords, or production network information are included.

When publishing Packet Tracer projects publicly, sensitive information such as the following should be removed:

* Real network credentials
* Enable secrets
* SNMP community strings
* VPN information
* Real internal infrastructure details
* Production IP addresses
* API keys or authentication information

---

## 🏁 Conclusion

These Cisco Packet Tracer practicals provided hands-on experience with **Cisco networking fundamentals**, including IPv4 addressing, switch configuration, VLAN-based network segmentation, trunking, and inter-VLAN routing.

The practicals progressed from a basic LAN to VLAN-based segmentation and finally to **Router-on-a-Stick**, where communication between separate VLAN networks was enabled through router subinterfaces.

Overall, these labs strengthened my understanding of **Layer 2 switching, VLANs, trunking, Layer 3 routing, and network troubleshooting** using Cisco Packet Tracer.
