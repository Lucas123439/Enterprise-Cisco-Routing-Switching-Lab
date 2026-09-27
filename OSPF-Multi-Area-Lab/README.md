# Enterprise Cisco Routing & Switching Lab

## Overview

This project is a multi-protocol enterprise network designed and implemented
using Cisco Modeling Labs (CML).

The purpose of this project is to demonstrate practical knowledge of enterprise
Layer 2 and Layer 3 networking, dynamic routing, first-hop redundancy,
network segmentation, route redistribution, and network troubleshooting.

The lab was designed as a hands-on CCNP Enterprise / ENCOR portfolio project.

---

## Network Topology

See:

topology/topology.png

The network contains:

- 3 Cisco IOSv routers
- Cisco IOSvL2 switches
- Multiple VLANs
- EIGRP AS 100
- OSPF
- HSRP
- EtherChannel
- 802.1Q trunking
- Route redistribution
- OSPF multi-area design
- Network troubleshooting scenarios

---

# Technologies Demonstrated

## Layer 2

- VLANs
- 802.1Q trunking
- Inter-switch trunking
- EtherChannel
- LACP
- Spanning Tree Protocol
- VLAN segmentation

## EIGRP

- EIGRP AS 100
- EIGRP neighbor formation
- Passive interfaces
- EIGRP stub routing
- EIGRP summarization
- EIGRP authentication
- EIGRP metrics
- Feasible Distance
- Reported Distance
- Successor
- Feasible Successor
- Feasibility Condition
- EIGRP topology table
- EIGRP route verification

## OSPF

- OSPF Area 0
- OSPF Area 10
- OSPF ABR
- OSPF neighbor formation
- Passive interfaces
- OSPF authentication
- OSPF LSDB
- OSPF LSA analysis
- Inter-area routing
- OSPF area summarization
- Stub areas
- Default route injection

## First-Hop Redundancy

- HSRP
- HSRP version 2
- Active/standby operation
- HSRP priority
- HSRP preemption
- Redundant default gateways

## Route Redistribution

- EIGRP to OSPF redistribution
- OSPF to EIGRP redistribution
- Redistribution metrics
- OSPF external routes
- EIGRP external routes

## Troubleshooting

- VLAN connectivity
- Trunk problems
- EtherChannel failures
- STP problems
- EIGRP adjacency failures
- OSPF adjacency failures
- Routing table analysis
- HSRP failover
- Redistribution problems
- Routing protocol authentication problems

---

# Network Architecture

The network is divided into multiple routing domains.

R1 operates primarily as an EIGRP router.

R2 acts as the boundary between the EIGRP and OSPF routing domains.

R3 operates as an OSPF router and is used to demonstrate OSPF
multi-area functionality.

R2 and R3 provide redundant default gateways using HSRP.

Conceptual design:

                    EIGRP AS 100
                         |
                         |
                        R1
                         |
                         |
                        R2
                   EIGRP / OSPF
                         |
                       Area 0
                         |
                        R3
                         |
                       Area 10


---

# Router Roles

## R1

Primary role:

- EIGRP router
- EIGRP AS 100
- Remote/edge routing domain
- EIGRP stub router
- EIGRP summarization and authentication practice

## R2

Primary role:

- EIGRP router
- OSPF router
- EIGRP/OSPF redistribution point
- HSRP Active router
- Router-on-a-Stick gateway
- Routing protocol boundary

## R3

Primary role:

- OSPF router
- OSPF ABR
- HSRP Standby router
- Router-on-a-Stick gateway
- OSPF multi-area testing

---

# Layer 2 Design

The switches provide VLAN segmentation and Layer 2 connectivity.

VLANs:

- VLAN 10
- VLAN 20
- VLAN 30
- VLAN 40
- VLAN 50
- VLAN 60

The switch-to-switch connection uses an LACP EtherChannel.

Example:

SW1
 ||
 || LACP EtherChannel
 ||
SW2

The EtherChannel allows multiple physical links to operate as one
logical Port-Channel.

---

# Routing Design

## EIGRP

R1 and R2 operate EIGRP AS 100.

R1 and R2 form an EIGRP adjacency across:

10.12.12.0/30

R1:

10.12.12.1/30

R2:

10.12.12.2/30


## OSPF

R2 and R3 form an OSPF adjacency across:

10.23.23.0/30

R2:

10.23.23.1/30

R3:

10.23.23.2/30

This link belongs to OSPF Area 0.

R3 is also used as an OSPF ABR for Area 10.

---

# First-Hop Redundancy

R2 and R3 provide redundant default gateways using HSRP.

R2 is configured with a higher HSRP priority and operates as the
preferred Active router.

R3 operates as the Standby router.

The HSRP virtual IP addresses are used by hosts as their default gateway.

Current VLAN addressing:

VLAN 10
Network: 192.168.10.0/24
HSRP VIP: 192.168.10.1
R2: 192.168.10.2
R3: 192.168.10.3

VLAN 20
Network: 192.168.20.0/24
HSRP VIP: 192.168.20.1
R2: 192.168.20.2
R3: 192.168.20.3

VLAN 30
Network: 192.168.30.0/24
HSRP VIP: 192.168.30.1
R2: 192.168.30.2
R3: 192.168.30.3

VLAN 40
Network: 192.168.40.0/24
HSRP VIP: 192.168.40.1
R2: 192.168.40.2
R3: 192.168.40.3

VLAN 50
Network: 192.168.50.0/24
HSRP VIP: 192.168.50.1
R2: 192.168.50.2
R3: 192.168.50.3

VLAN 60
Network: 192.168.60.0/24
HSRP VIP: 192.168.60.1
R2: 192.168.60.2
R3: 192.168.60.3

NOTE:

The VLAN addressing will be finalized before the OSPF Area 10
implementation is published. The same subnet should not be placed
into multiple OSPF areas.

---

# Key Design Decisions

## Why EIGRP?

EIGRP is used to demonstrate advanced enterprise routing concepts
including DUAL, successor/feasible successor selection, metrics,
summarization, authentication, stub routing, and redistribution.

## Why OSPF?

OSPF is used to demonstrate link-state routing, LSAs, multi-area
design, ABRs, summarization, area types, authentication, and
inter-area routing.

## Why HSRP?

HSRP provides redundant default gateway functionality.

Hosts use the HSRP virtual IP as their default gateway rather than
depending on the physical IP address of one router.

## Why EtherChannel?

EtherChannel allows multiple physical links to operate as one logical
connection while providing additional bandwidth and redundancy.

## Why Route Redistribution?

R2 provides a controlled boundary between the EIGRP and OSPF routing
domains.

This allows the lab to demonstrate communication between different
routing protocols.

---

# Loopback Addresses

R1:

Loopback0:
1.1.1.1/32

Loopback1:
11.11.11.1/32


R2:

Loopback0:
2.2.2.2/32

Loopback1:
22.22.22.2/32


R3:

Loopback0:
3.3.3.3/32

Loopback1:
33.33.33.3/32

Loopback0 is used as the routing protocol router ID.

Loopback1 is used as an advertised/test network.

---

# Router-to-Router Addressing

R1 to R2:

Network:
10.12.12.0/30

R1:
10.12.12.1/30

R2:
10.12.12.2/30


R2 to R3:

Network:
10.23.23.0/30

R2:
10.23.23.1/30

R3:
10.23.23.2/30

---

# Router-on-a-Stick

R2 and R3 use Router-on-a-Stick for VLAN gateway functionality.

The physical trunk interface does not have an IP address.

Example:

interface GigabitEthernet1/0
 no ip address
 no shutdown

VLAN-specific Layer 3 interfaces are configured as subinterfaces.

Example:

interface GigabitEthernet1/0.10
 encapsulation dot1q 10
 ip address 192.168.10.2 255.255.255.0

The switch sends VLAN-tagged traffic across the 802.1Q trunk.

The router's physical interface receives the frame and forwards it
to the appropriate subinterface based on the VLAN tag.

---

# Verification

## Layer 2

show vlan brief

show interfaces trunk

show etherchannel summary

show spanning-tree


## EIGRP

show ip eigrp neighbors

show ip eigrp topology

show ip eigrp topology all-links

show ip route eigrp

show ip protocols


## OSPF

show ip ospf neighbor

show ip ospf database

show ip ospf interface brief

show ip route ospf

show ip protocols


## HSRP

show standby brief

show standby


## General Routing

show ip route

show ip protocols

show ip interface brief

---

# Troubleshooting Exercises

The lab includes intentional failure scenarios.

1. Disable an EtherChannel member.
2. Create an LACP mismatch.
3. Break EIGRP authentication.
4. Break OSPF authentication.
5. Shut down the active HSRP router.
6. Remove an OSPF network statement.
7. Remove EIGRP redistribution.
8. Modify an EIGRP metric.
9. Change an OSPF area assignment.
10. Verify routing convergence.
11. Identify missing routes.
12. Identify incorrect next hops.
13. Verify HSRP failover.
14. Verify EIGRP neighbor recovery.
15. Verify OSPF neighbor recovery.

---

# Lab Environment

Platform:

Cisco Modeling Labs (CML)

Virtual devices:

- Cisco IOSv
- Cisco IOSvL2

Router limitation:

3 routers running simultaneously.

---

# Project Goals

This project was created to demonstrate practical knowledge of:

- Enterprise networking
- Cisco IOS
- Layer 2 switching
- VLAN segmentation
- Trunking
- EtherChannel
- STP
- EIGRP
- OSPF
- OSPF multi-area architecture
- HSRP
- Route redistribution
- Route filtering
- Network troubleshooting
- Network design
- Routing protocol verification

The project emphasizes not only configuration but also verification,
troubleshooting, and understanding the reasoning behind each
configuration decision.
