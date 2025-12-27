# Designing a VLAN-Based Branch Network for XYZ Company Using Cisco Devices

## Project Overview
XYZ Company, a fast-growing food retail business based in Eastern Australia, plans to expand by opening a branch near the local village of Bonalbo. As a network engineer, I was tasked with designing a small, independent branch network that meets the company’s operational requirements while ensuring departmental segmentation, wireless access, and inter-department communication.  

This project demonstrates the end-to-end design and implementation of the branch network using Cisco devices, VLANs, DHCP, and wireless connectivity.

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

### 1. Configure VLANs on the Switch
```bash
enable
configure terminal
vlan 10
name Admin_IT
vlan 20
name Finance_HR
vlan 30
name Customer_Service
exit

interface range GigabitEthernet0/1-10
switchport mode access
switchport access vlan 10

interface range GigabitEthernet0/11-20
switchport mode access
switchport access vlan 20

interface range GigabitEthernet0/21-24
switchport mode access
switchport access vlan 30
exit
