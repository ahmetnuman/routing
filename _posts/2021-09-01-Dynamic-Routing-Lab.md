---
layout: post
title: Dynamic Routing Labs Topology and Tasks
tags: Dynamic Routing Topology and Tasks
categories: dynamicRouting
---

# Routing Switching

> Ahmet Numan Aytemiz 01.09.2021

## Routing Topology and Tasks

![topology](/routing/img/topology.png)


#### According to topology perform following tasks

#### RIP Section

**Task 1**
- Assign 1.1.1.1/32 loopback interface on R1 
- Assign 4.4.4.4/32 loopback interface on R4

**Task 2**
- Enable RIPv2 between R1 and R4 

**Task 3**
- Ensure the loopback0 interface on R1 and R4 are advertised via RIPv2 using the original subnet masks

**Task 4**
- Configure a distrubite list to ensure that R4 does not use R1 reach the 10.0.18.0/28 network.
- Do not make any configuration changes os R4

---

#### Basic IPv6 Section 

**Task 5**
- Use EUI-64 to assign a link-local address to the fast ethernet 0/1 on R1 
- Configure the link-local address FE80::14:4 on the fast Ethernet 0/1 on R4 

**Task 6**

- Assign the unicast address 2001:db8:14::1/64 to R1's fastEthernet 0/1 interface
- Configure R4 to use SLAAC to receive an IPv6 Address from R1

**Task 7**
- Address all links accodirding to the IPv6 topology diagram
- Ensure all router use CEF to forward IPv6 packets

---

#### OSPF Section

**Task 8**
- Configure OSPF area 0 and area 23 according to the IPv4 topology diagram.
- Use the most specific wildcard mask possible on R1 
- Ensure R2 uses the router id 2.2.2.2 and R3 uses the router id 3.3.3.3

**Task 9**
- Configure OSPF Area 27 as a stub area according to the IPv4 topology diagram
- On R7 use OSPF process 7 and RID of 7.7.7.7

**Task 10**
- Configure area 18 as a totally stubby area according to the IPv4 topology diagram
- on R8 create and advertise the loopback0 Ip address 8.8.8.8/32 into the area
- ensure R8 uses a rid of 8.8.8.8 even if all loopbacks are deleted

**Task 11**
- Configure area 34 as not-so-stubby area

**Task 12**
- On R2 and R3 configure virtual link

**Task 13**
- Configure R3 as the designated router on the netwrok between R3 and R4 
- Ensure R4 never becomes the DR 

**Task 14**
- Ensure neither R1 nor R8 is designated router on the 10.0.18.0/28 network 

**Task 15**
- Configure OSPF authentication for area 0
- Ensure the authentication key is never sent in the clear

**Task 16**

- Configure loopback address 8.0.0.1/32 and 8.0.0.2/32 on R8
- Summarize the 8.0.0.0/8 major network as close to R8 as possible

**Task 17**

- Configure loopbacks on R1 as follows 
  - Loopback1 - 1.1.0.1/24
  - Loopback2 - 1.2.0.1/24
  - Loopback3 - 1.3.0.1/16
  - Loopback4 - 1.4.0.1/14 
- Redistrubute subnets of all connected interfaces into OSPF

**Task 18**

- On R1 , redistribute RIP learned subnets intp OSPF as E1 routes

**Task 19**

- Redistrubute all currnet and future prefixes matching 203.0.113.x/30 into OSPF as type E1 
- Your configuration must not affect any other prefixes

**Task 20**

- Summarize all of R1's loopback address as a 1.0.0.0/8 summary route

**Task 21**

- On R4, inject a default route into OSPF using ISP1's 198.51.100.2 address as the next hop

**Task 22**

- use a single command on R1 to inject a default route into OSPF

#### EIGRP Section 

**Task 23**

- Establish adjacencies for EIGRP AS 10 according to the IPv4 network topology diagram.
- Ensure R3 and R4 do not inadvertently establish an EIGRP adjacency with any other routers due to a misconfigured network statement
- Do not configure authentication on any routers
- Ensure R3 does not use more than % 20 of tha available bandwith on the interface to R6 for EIGRP traffic.


**Task 24**

- Configure R5 and R6 not to accept EIGRP updates from one another without proper
message authentication
- Use "cisco" as the sole authentication key 

**Task 25**

- On R5 , configure loopback0 with the address 5.5.5.5/24 and redistribute this prefix into EIGRP 
- Configure R5 to advertise only connected and summary routes into EIGRP 
- then configure R5 to receive bot not advertise any routes
- when finished, remove the stub configuration from R5


**Task 26**

- Create and advertise the following loopbacks on R6:
  - Loopback0 : 6.6.6.6/32
  - Loopback1 : 6.0.0.1/32
  - Loopback2 : 6.0.0.2/32
- Ensure R6 automatically summarizes networks to their classful boundaries

**Task 27**

- Configure and advertise the following loopbacks on R5 
  - Loopback0 : 5.5.5.5/32
  - Loopback1 : 5.0.0.1/32
  - Loopback2 : 5.0.0.2/32
  - Loopback3 : 5.0.0.3/32
- Configure R5 to advertise a classful summary for its loopbacks to R4 only

**Task 28**

- On R3 and R4, redistribute all routes from EIGRP AS 10 into OSPF as E1 routes
- Tag redistributed routes as follows:
  - R3 should tag all routes with 3333
  - R4 should tag all routes with 4444

**Task 29**

- Redistribute OSPF into EIGRP as 10 
- EIGRP metrics for redistributed routes should be derived from the interfaces leading to OSPF area 0
- Ensure R3 and R4 tag all redistributed routes with 333310 and 333340 respectively

**Task 30**

- R5 is taking a suboptimal route to R1's 1.1.1.1 loopback 
- Ensure R5 takes the shortest path to R1's 1.1.1.1 loopback 
- Do not create or modify any static or default routes 

**Task 31**

- On R3 and R4, create the loopback34 interface with the IP address of 34.34.34.34
- Advertise this prefix into EIGRP 
- Configure R6 to perform load sharing to this accross R3 and R5

**Task 32**

- Disable auto summarization on R6 
- On R4 and R6 , create the loopback46 interface with the IP address of 46.46.46.46/32
- Advertise this prefix into EIGRP 
- Verify R5 uses equal cost load sharing to reach this prefix via both R4 and R6 

**Task 33**

- Configure the following loopback:
   - R5: loopbacl50 - 50.50.50.50/32
- Advertise this loopback into EIGRP AS 10
- Ensure R5 dooe not advertise its new loopback directly to R6 

**Task 34**

- A static default route already exists on R4
- Configure R4 to advertise a default route into Eigrp
- Use an advertised bandwidth of 1000000 Kbps and delay of 10  mikro seconds
- Your configuration must not affect the metrics of any other current or future routes
- Do not use a route map

---

#### BGP Section 

**Task 35**

- On the ISP1 router confiure loopback interfaces as follows
  - loopback 11 : 11.11.11.11/32
  - loopback 12 : 12.12.12.12/32
  - loopback 13 : 13.13.13.13/24
  - loopback 14 : 14.14.14.14/32
  - loopback 15 : 15.15.15.15/32
  - loopback 55 : 55.55.55.55/32
  - loopback 55 : 66.66.666.66/24
  - loopback 77 : 77.77.77.77/32
  
- Configure ISP1 (AS 65535) to establish eBGP peerings with R1 and R4 (AS 65534)
- If a password required , it will be set to "cisco"
- Redistrube connected and static route in the BGP

- Configure R1 and R4 (AS 65534) to establish eBGP peerings with ISP1 (AS 65535)
- If a password required , it will be set to "cisco"

**Task 36**

- On R1 , redistribute connected routes into BGP as 65534
- Perform mutual redistribution as follows :
   - On R1, between BGS AS 65534 and OSPF
   - On R4, between BGP AS 65534 and EIGPR 10

**Task 37**

 - Ensure ISP1 selects the path through 198.51.100.1 as its best path to the 1.0.0.0/8 prefix
 - Do not make any changes to ISP1 

 **Task 38**

 - Ensure R4 removes its existing static default route if ISP1's interface IP 198.51.100.2 becomes unreachable.

 **Task 39**

- On R4 , configure a floating static route that uses R1's interface IP 10.0.141.1 as its default gateway
- Configure this route with an administative distance of 4

**Task 40**

- On R5, configure a static route for the 8.0.0.0/8 prefix using fa 0/1 as the next hop interface

**Task 41**

- On R1 and R3, configure loopback 31 with the anycast address 31.31.31.31/32
- Advertise this prefix into OSPF
- Ensure the path through R1 is preferred


