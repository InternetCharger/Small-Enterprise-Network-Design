# Designing a VLAN-Based Branch Network for XYZ Company Using Cisco Devices

## Project Overview
XYZ Company, a fast-growing food retail business based in Houston TX, plans to expand by opening a branch near the Downtown Area. As a network engineer, I was tasked with designing a small, independent branch network that meets the company’s operational requirements while ensuring departmental segmentation, wireless access, and inter-department communication.  

Demonstrating the end-to-end design and implementation of the branch network using Cisco devices, VLANs, DHCP, and wireless connectivity.

---

## Project Requirements
1. Use **one Cisco router** and **one Cisco switch**.  
2. Create **3 separate departments**:
   - Admin/IT  
   - Finance/HR  
   - Customer Service/Reception  
3. Each department should be in a **different VLAN**.  
4. Provide **wireless connectivity** for each department.  
5. Host devices must obtain **IP addresses automatically** using DHCP.  
6. Devices in all departments must be able to **communicate with each other**.  
7. Network base IP provided: **192.168.1.0/24**.  

---

## Network Design Overview
- **Router**: Connects the branch network to the ISP and provides inter-VLAN routing.  
- **Switch**: Implements VLANs for departmental segmentation.  
- **Wireless Access Points (WAPs)**: Provide department-specific wireless networks.  
- **DHCP**: Dynamically assigns IPv4 addresses to all devices.  

**VLAN Assignment:**

| Department                  | VLAN ID | Subnet           |
|-----------------------------|---------|----------------|
| Admin/IT                    | 10      | 192.168.1.0/26 |
| Finance/HR                  | 20      | 192.168.1.64/26 |
| Customer Service/Reception  | 30      | 192.168.1.128/26 |

---

## Implementation Steps & Commands

<details>
<summary>1. Configure VLANs on the Switch</summary>

```bash
enable
configure terminal

 - Create VLANs

vlan 10
name Admin_IT
vlan 20
name Finance_HR
vlan 30
name Customer_Service

- Assign ports to VLANs
- VLAN 10: ports 1-4 (PC, Laptop, AP, Printer)

interface range fa0/1-4
switchport mode access
switchport access vlan 10

- VLAN 20: ports 5-8
interface range fa0/5-8
switchport mode access
switchport access vlan 20

- VLAN 30: ports 9-12
interface range fa0/9-12
switchport mode access
switchport access vlan 30

- Set up trunk port to router 
interface fa0/24
switchport mode trunk
switchport trunk encapsulation dot1q
exit

- Save configuration
write memory
````

</details>

<details>
<summary>2. Configure Router for Inter-VLAN Routing</summary>

```bash
enable
configure terminal

- Configure subinterfaces for each VLAN
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.1.1 255.255.255.192
no shutdown

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.1.65 255.255.255.192
no shutdown

interface g0/0.30
encapsulation dot1Q 30
ip address 192.168.1.129 255.255.255.192
no shutdown
```

</details>

<details>
<summary>3. Configure DHCP Pools on Router</summary>

```bash
ip dhcp pool Admin_IT
network 192.168.1.0 255.255.255.192
default-router 192.168.1.1

ip dhcp pool Finance_HR
network 192.168.1.64 255.255.255.192
default-router 192.168.1.65

ip dhcp pool Customer_Service
network 192.168.1.128 255.255.255.192
default-router 192.168.1.129

- Exclude router IPs from DHCP
ip dhcp excluded-address 192.168.1.1 192.168.1.3
ip dhcp excluded-address 192.168.1.65 192.168.1.67
ip dhcp excluded-address 192.168.1.129 192.168.1.131
```

</details>

---

## Network Diagram

![Small Enterprise Topology](https://github.com/user-attachments/assets/80c82203-6ae9-4cf7-8c76-b52b69668e5d)


---

## Outcome

* Each department has a **separate VLAN**.
* All hosts **receive IP addresses automatically** via DHCP.
* Departments can **communicate with each other** through router inter-VLAN routing.
* Wireless connectivity is provided for all departments.
* The branch network is **independent** but ready for future integration with the HQ network.

---

## Tools & Devices Used

* Cisco Router (e.g., Cisco ISR 4000 series)
* Cisco Switch (e.g., Cisco Catalyst 2960 series)
* Cisco Wireless Access Points
* Cisco Packet Tracer (for simulation)

---
