# Multi-Switch VLAN Network

For my second Cisco Packet Tracer project, I built a network using two Cisco switches. I wanted to take what I learned from my first VLAN project and understand how VLANs work when multiple switches are connected together.

The network has three departments: Administration, Sales and IT. Each department has its own VLAN, and the two switches are connected using a trunk link.

## What I Wanted to Learn

* How VLANs work across multiple switches
* How to configure a trunk port
* How multiple VLANs can travel through one physical link
* How to assign switch ports to VLANs
* How to test connectivity between devices on different switches
* How to troubleshoot VLAN and trunk connectivity

## Network Topology

The network uses two Cisco 2960 switches and twelve PCs.

Each switch has PCs from all three departments.

* **Administration — VLAN 10**
* **Sales — VLAN 20**
* **IT — VLAN 30**

The two switches are connected through FastEthernet 0/24 on each switch.

The link between the switches is configured as a trunk.

## IP Addressing

| Device | Department     | VLAN | IP Address    | Subnet Mask   |
| ------ | -------------- | ---: | ------------- | ------------- |
| PC1    | Administration |   10 | 192.168.10.10 | 255.255.255.0 |
| PC2    | Administration |   10 | 192.168.10.20 | 255.255.255.0 |
| PC7    | Administration |   10 | 192.168.10.30 | 255.255.255.0 |
| PC8    | Administration |   10 | 192.168.10.40 | 255.255.255.0 |
| PC3    | Sales          |   20 | 192.168.20.10 | 255.255.255.0 |
| PC4    | Sales          |   20 | 192.168.20.20 | 255.255.255.0 |
| PC9    | Sales          |   20 | 192.168.20.30 | 255.255.255.0 |
| PC10   | Sales          |   20 | 192.168.20.40 | 255.255.255.0 |
| PC5    | IT             |   30 | 192.168.30.10 | 255.255.255.0 |
| PC6    | IT             |   30 | 192.168.30.20 | 255.255.255.0 |
| PC11   | IT             |   30 | 192.168.30.30 | 255.255.255.0 |
| PC12   | IT             |   30 | 192.168.30.40 | 255.255.255.0 |

## VLAN Configuration

I created the same three VLANs on both switches:

```text
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
```

I then assigned the PC ports to their respective VLANs.

For example:

```text
interface range fastEthernet 0/1-2
switchport mode access
switchport access vlan 10
exit
```

I repeated the same process for VLAN 20 and VLAN 30.

## Trunk Configuration

The connection between SW1 and SW2 uses FastEthernet 0/24 on both switches.

On both switches, I configured the link as a trunk:

```text
interface fastEthernet 0/24
switchport mode trunk
exit
```

This allows traffic from VLAN 10, VLAN 20 and VLAN 30 to travel between the two switches over the same physical connection.

I verified the trunk using:

```text
show interfaces trunk
```

## Testing the Network

I tested communication between devices belonging to the same VLAN but connected to different switches.

For example:

```text
PC1 → 192.168.10.30
```

PC1 is connected to SW1 while PC7 is connected to SW2.

The ping was successful, showing that VLAN 10 could communicate across the trunk link.

I also tested communication between different VLANs.

For example:

```text
PC1 → 192.168.20.30
```

This ping failed, which was expected because VLAN 10 and VLAN 20 are separate networks and I had not configured any Layer 3 routing.

## Troubleshooting

During this project, I checked the VLAN assignments and trunk configuration when testing connectivity between the switches.

The main commands I used for troubleshooting were:

```text
show vlan brief
show interfaces trunk
ping
```

These helped me verify that the correct ports were assigned to the correct VLANs and that the switch-to-switch link was operating as a trunk.

## What I Learned

This project helped me understand trunking much better.

The main things I learned were:

* VLANs can exist across multiple switches.
* A trunk can carry traffic for multiple VLANs.
* Access ports are normally used for end devices.
* Trunk ports are used to connect network devices and carry multiple VLANs.
* Devices in the same VLAN can communicate across switches when the trunk is configured correctly.
* Different VLANs still require Layer 3 routing to communicate.
* `show interfaces trunk` is useful for checking trunk links.

The complete Packet Tracer project is included in this repository:

`Multi-Switch-VLAN-Network.pkt`
