 Small Office VLAN Network

For my first Cisco Packet Tracer project, I built a small office network for a company with three departments: Administration, Sales and IT

I used VLANs to separate the departments while keeping everything connected to the same switch. The main goal was to understand how VLANs work, how switch ports are assigned to VLANs, and how devices communicate within and between VLANs.

What I Wanted to Learn

* How to create VLANs on a Cisco switch
* How to assign devices to different VLANs
* How to configure IPv4 addresses
* How to test connectivity using ping
* How to use Cisco IOS commands
* Why devices in different VLANs cannot communicate without routing
* How to troubleshoot basic VLAN connectivity problems

 Network Topology

The network uses one Cisco 2960 switch and six PCs.

The PCs are divided into three departments:

Administration VLAN 10**
Sales  VLAN 20**
IT  VLAN 30**

Each department has two PCs.

 Devices Used

| Device                         | Quantity |
| ------------------------------ | -------: |
| Cisco 2960 Switch              |        1 |
| PCs                            |        6 |
| Copper Straight-Through Cables |        6 |

IP Addressing

| Device | Department     | VLAN | IP Address    | Subnet Mask   |
| ------ | -------------- | ---: | ------------- | ------------- |
| PC1    | Administration |   10 | 192.168.10.10 | 255.255.255.0 |
| PC2    | Administration |   10 | 192.168.10.20 | 255.255.255.0 |
| PC3    | Sales          |   20 | 192.168.20.10 | 255.255.255.0 |
| PC4    | Sales          |   20 | 192.168.20.20 | 255.255.255.0 |
| PC5    | IT             |   30 | 192.168.30.10 | 255.255.255.0 |
| PC6    | IT             |   30 | 192.168.30.20 | 255.255.255.0 |

## VLAN Configuration

I created three VLANs on the switch:

| VLAN | Name  | Ports       |
| ---: | ----- | ----------- |
|   10 | ADMIN | Fa0/1–Fa0/2 |
|   20 | SALES | Fa0/3–Fa0/4 |
|   30 | IT    | Fa0/5–Fa0/6 |

I created the VLANs using:


enable
configure terminal

vlan 10
name ADMIN
exit

vlan 20
name SALES
exit

vlan 30
name IT
exit


I then assigned the switch ports to their respective VLANs.

For example:


interface range fastEthernet 0/1-2
switchport mode access
switchport access vlan 10
exit


I repeated the same process for VLANs 20 and 30.

 Verification

I used:


show vlan brief


to check that the VLANs existed and that the correct ports were assigned to them.

## Testing the Network

I tested communication between devices in the same VLAN.

For example, PC1 was able to ping PC2:


PC1 → 192.168.10.20


The ping was successful.

I also tested communication between different VLANs.

PC1 was unable to ping PC3:


PC1 → 192.168.20.10

This was expected because VLAN 10 and VLAN 20 are separate networks and I had not configured a router or Layer 3 switch.

Troubleshooting

While building the project, I had an issue where **PC5 could not ping PC6**.

I checked the IP addresses and then used:

show vlan brief

to check the VLAN assignments.

I found that the ports needed to be correctly assigned to VLAN 30. After correcting the configuration, PC5 and PC6 were able to communicate.

This helped me understand that when two devices on the same VLAN cannot communicate, I should check things like:

* IP addresses
* Subnet masks
* Cable connections
* Switch port status
* VLAN assignment

 What I Learned

This project helped me understand VLANs much better because I actually had to build and troubleshoot the network instead of just reading about them.

The main things I learned were:

* A switch can be divided into multiple logical VLANs.
* An access port normally belongs to one VLAN.
* Devices in the same VLAN can communicate directly if their IP configuration is correct.
* Different VLANs are separate networks.
* A router or Layer 3 switch is needed for communication between VLANs.
* show vlan brief is useful for checking VLAN and port assignments.
* ping is useful for testing connectivity.

The complete Packet Tracer project is included in this repository:

project1.pkt
