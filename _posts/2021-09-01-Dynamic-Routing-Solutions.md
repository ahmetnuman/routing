---
layout: post
title: Dynamic Routing Solutions
tags: Dynamic Routing Solutions
categories: dynamicRouting
---

> Ahmet Numan Aytemiz 09.01.2021

---

## RIP SECTION

**Task 1**
- Assign 1.1.1.1/32 loopback interface on R1 
- Assign 4.4.4.4/32 loopback interface on R4

**Solution 1**

```
R1#conf t
R1(config)#interface loopback 0
R1(config-if)#ip address 1.1.1.1 255.255.255.255
R1(config-if)#end
R1#show ip interface brief | include Loopback0
R1#wr me
```

```
R4#conf t
R4(config)#interface loopback 0
R4(config-if)#ip address 1.1.1.1 255.255.255.255
R4(config-if)#end
R4#show ip interface brief | include Loopback0
R4#wr me
```
---

**Task 2**
- Enable RIPv2 between R1 and R4

**Solution 2**

```
R1# conf t
R1(config)#router rip
R1(config-router)#version 2
R1(config-router)#network 10.0.14.0
R1(config-router)#end
R1#show run | section router rip 
```

```
R4#conf t 
R4(config)#router rip 
R4(config-router)#version 2
R4(config-router)#network 10.0.14.0 
R4(config-router)#end
R4#show run | section router rip 
R4#show ip route 
R4#show ip route connected | section 10.0.0.0
R4#show ip route 10.0.45.0
```

---

**Task 3**
- Ensure the loopback0 interface on R1 and R4 are advertised via RIPv2 using the original subnet masks

**Solution 3**

```
R1#show ip interface brief
R1#show interface loopback0
R1#conf t
R1(conf)#router rip
R1(config-router)#network 1.1.1.1
R1(config-router)#do show run | s router rip 
```

```
R4#show ip route
```

```
R1#show ip protocols
R1#conf t
R1(conf)#router rip
R1(config-router)#no auto-sumary
```

```
R4#show ip interface brief
R4#show interface loopback0
R4#conf t
R4(conf)#router rip
R4(config-router)#network 4.4.4.4
R4(conf-router)#no auto-sumary
R4(config-router)#do clear ip route *
R4(config-router)#do sh ip router rip
R4(config-router)#do show ip cef 
R4(config-router)#do show ip cef 1.1.1.1
```

---

**Task 4**
- Configure a distrubite list to ensure that R4 does not use R1 reach the 10.0.18.0/28 network.
- Do not make any configuration changes os R4

**Solution 4**

```
R1#conf t
R1(config)#router rip 
R1(config-router)#distribute-list ?
R1(config-router)#distribute-list 14 out fa0/1 
R1(config)#access-list 14 deny 10.0.18.0 0.0.0.15
R1(config)#access-list 14 permit 0.0.0.0 255.255.255.255
```

```
R4#clear ip route *
R4#show ip route rip
```

## BASIC IPv6 SECTION 

**Task 5**
- Use EUI-64 to assign a link-local address to the fast ethernet 0/1 on R1 
- Configure the link-local address FE80::14:4 on the fast Ethernet 0/1 on R4

**Solution 5**

```
R1#conf t 
R1(config)#int fa0/1
R1(config-if)#ipv6 enable
R1(config-if)#do sh ipv6 int br fa0/1
```

```
R4#conf t 
R4(config)#int fa0/1
R4(config-if)#ipv6 address fe80::14:4 link-local
R4(config-if)#do sh ipv6 int br fa0/1
```

```
R1#ping fe80::14:4
```

---

**Task 6**
- Assign the unicast address 2001:db8:14::1/64 to R1's fastEthernet 0/1 interface
- Configure R4 to use SLAAC to receive an IPv6 Address from R1

**Solution 6**

```
R1#conf t
R1(conf)#int fa0/1
R1(config-if)#ipv6 address 2001:db8:14::1/64
```

```
R4#conf t
R4(config)#int fa0/1
R4(config-if)#ipv6 address autoconfig
R4(config-if)#ipv6 unicast-routing
R4(config-if)#do sh ipv6 int bri fa0/1
```

---

**Task 7**
- Address all links accodirding to the IPv6 topology diagram
- Ensure all router use CEF to forward IPv6 packets

**Solution 7**

```
R1(config)#ipv6 unicast-routing
R4(config)#do show ipv6 cef 
R4(config)#int fa0/2 
R4(config-if)#ipv6 address 2001:db8:12::1/64
```

Note : All other interface configuration can be doing same way.

---

## OSPF SECTION

**Task 8**
- Configure OSPF area 0 and area 23 according to the IPv4 topology diagram.
- Use the most specific wildcard mask possible on R1 
- Ensure R2 uses the router id 2.2.2.2 and R3 uses the router id 3.3.3.3

**Solution 8**

```
R1#debug ip ospf hello
R1#conf t
R1(config)#router ospf 1
R1(config-router)#network 10.0.12.1 0.0.0.0 area 0
```

```
R2#debug ip ospf hello 
R2#conf t
R2(config)#interface loopback 0
R2(config-if)#ip address 2.2.2.2 255.255.255.255
R2(config-if)#router ospf 1
R2(config-router)#network 10.0.12.0 0.0.0.3 area 0
R2(config-router)#do un all 
R2#show ip ospf database router
R2#conf t
R2(config)#router ospf 1
R2(config-router)#network 10.0.23.2 0.0.0.0 area 23
```

```
R3#conf t
R3(config)#interface lo0
R3(config-if)#ip address 3.3.3.3 255.255.255.255
R3(config-if)#router ospf 1
R3(config-router)#network 10.0.23.3 0.0.0.0 area 23
R3(config-router)#do show ip ospf database router
R3(config-router)#do ping 10.0.12.1
R3(config-router)#do show ip ospf database
```

---

**Task 9**
- Configure OSPF Area 27 as a stub area according to the IPv4 topology diagram
- On R7 use OSPF process 7 and RID of 7.7.7.7

**Solution 9**

```
R2#conf t 
R2(config)#router ospf 1
R2(config-router)#network 10.0.27.2 0.0.0.0 area 27
R2(config-router)#area 27 stub
```

```
R7#conf t 
R7(config)#interface lo0
R7(config-if)#ip address 7.7.7.7 255.255.255.255
R7(config-if)#exit 
R7(config)#router ospf 7
R7(config-router)#network 10.0.27.7 0.0.0.0 area 27
R7(config-router)#area 27 stub 
R7(config-router)#do show ip ospf database 
R7(config-router)#do show ip route ospf
```

---

**Task 10**
- Configure area 18 as a totally stubby area according to the IPv4 topology diagram
- on R8 create and advertise the loopback0 Ip address 8.8.8.8/32 into the area
- ensure R8 uses a rid of 8.8.8.8 even if all loopbacks are deleted

**Solution 10**

```
R1#conf t
R1(config)#router ospf 1
R1(config-router)#network 10.0.18.1 0.0.0.0 area 18
R1(config-router)#area 18 stub no-summary
```

```
R8#conf t 
R8(config)#interface loopback0
R8(config-if)#ip address 8.8.8.8 255.255.255.255
R8(config-if)#router ospf 1
R8(config-router)#router-id 8.8.8.8 
R8(config-router)#network 8.0.0.0 0.255.255.255 area 18
R8(config-router)#network 10.0.18.8 0.0.0.0 area 18 
R8(config-router)#area 18 stub no-summary
R8(config-router)#do show ip ospf database
```

---

**Task 11**
- Configure area 34 as not-so-stubby area

**Solution 11**

```
R3#conf t
R3(config)#router ospf 1
R3(config-router)#network 10.0.34.3 0.0.0.0 area 34
R3(config-router)#area 34 nssa
```

```
R4#conf t
R4(config)#router ospf 1
R4(config-router)#network 10.0.34.4 0.0.0.0 area 34
R4(config-router)#area 34 nssa
R4(config-router)#do show ip ospf neighbour
R4(config-router)#do show ip database
```

---

**Task 12**
- On R2 and R3 configure virtual link

**Solution 12**

```
R3#conf t
R3(config)#router ospf 1
R3(config-router)#area 23 virtual-link 2.2.2.2
```

```
R2#conf
R2(config)#router ospf 1
R2(config-router)#area 23 virtual-link 3.3.3.3 
```

```
R4#show ip ospf database
R4#show ip route
```

---

**Task 13**
- Configure R3 as the designated router on the netwrok between R3 and R4 
- Ensure R4 never becomes the DR

**Solution 13**

```
R3#conf
R3(config)#interface fa 0/0
R3(config-if)#ip ospf priority 255
```

```
R4#conf
R4(config)#interface fa 1/0
R4(config-if)#ip ospf priority 0
R4#show ip ospf neighbour
R4#clear ip ospf proc 
R4#show ip ospf neighbour
R4#show ip ospf interface
```

---

**Task 14**
- Ensure neither R1 nor R8 is designated router on the 10.0.18.0/28 network

**Solution 14**

```
R1#show ip route ospf | i 18
R1#show ip ospf int fa 1/0   
R1#conf t
R1#(config)int fa 1/0
R1(config-if)#ip ospf network point-to-point
R1(config-if)#do show ip route ospf | i 18
R1(config-if)#do show ip ospf int fa 1/0
```

```
R8(config)#int fa0/0
R8(config-if)#ip ospf network point-to-point
R8(config-if)#do show ip ospf int fa 0/0
R8(config-if)#do show ip ospf int brief
```

---

**Task 15**
- Configure OSPF authentication for area 0
- Ensure the authentication key is never sent in the clea

**Solution 15**

```
R1#conf t 
R1(config)#router ospf 1
R1(config-router)#area 0 authentication message-digest 
R1(config-router)#int fa2/0 
R1(config-if)#ip ospf message-digest-key 1 md5 p1ssw0rd!
R1(config-if)#do show ip ospf | s Area
R1(config-if)#do show ip int fa2/0
```

```
R2#conf t 
R2(config)#router ospf 1
R2(config-router)#area 0 authentication message-digest 
R2(config-router)#int fa0/0
R2(config-if)#ip ospf message-digest-key 1 md5 p1ssw0rd!
R2(config-if)#router ospf 1
R2(config-router)#area 23 virtual-link 3.3.3.3 message-digest-key 1 md5 p1ssw0rd!
```

```
R3#conf
R3(config)#router ospf 1 
R3(config-router)#area 23 virtual-link 2.2.2.2 message-digest-key 1 md5 p1ssw0rd!
R3(config-router)#area 0 authentication message-digest
```

```
R4#show ip ospf database
```

---

**Task 16**

- Configure loopback address 8.0.0.1/32 and 8.0.0.2/32 on R8
- Summarize the 8.0.0.0/8 major network as close to R8 as possible

**Solution 16**

```
R8#conf t 
R8(config)#int lo1 
R8(config-if)#ip address 8.0.0.1 255.255.255.255
R8(config-if)#int lo2
R8(config-if)#ip address 8.0.0.2 255.255.255.255 
R8(config-if)#do show ip ospf int brief
R8(config-if)#do show ip proto | b ospf 
```

```
R2#show ip route ospf
```

```
R1#conf t 
R1(config)#router ospf 1 
R1(config-router)#area 18 range 8.0.0.0 255.0.0.0
```

```
R2#show ip route ospf
```

---

**Task 17**

- Configure loopbacks on R1 as follows 
  - Loopback1 - 1.1.0.1/24
  - Loopback2 - 1.2.0.1/24
  - Loopback3 - 1.3.0.1/16
  - Loopback4 - 1.4.0.1/14 
- Redistrubute subnets of all connected interfaces into OSPF

```
R1#conf t 
R1(config)#int loopback 1
R1(config-if)#ip address 1.1.0.1 255.255.255.0
R1(config-if)#int loopback 2
R1(config-if)#ip address 1.2.0.1 255.255.255.0 	
R1(config-if)#int loopback 3
R1(config-if)#ip address 1.3.0.1 255.255.0.0
R1(config-if)#int loopback 4
R1(config-if)#ip address 1.4.0.1 255.255.0.0
R1(config-if)#router ospf 1
R1(config-router)#redistribute connected subnets
```

```
R2#show ip route 
R2#show ip ospf database | begin Type-5 
R2#show ip ospf database router 1.1.1.1 
R2#show ip ospf database | begin ASB 
R2#show ip ospf database external 1.1.1.1
```

---

**Task 18**

- On R1 , redistribute RIP learned subnets intp OSPF as E1 routes

**Solution 18**

```
R1(config)#router ospf 1
R1(config-router)#redistribute rip subnets metric-type 1
R1(config-router)#do show ip ospf 1
```

```
R2#show ip route ospf | i O E
```

```
R1#show ip route rip
```

---

**Task 19**

- Redistrubute all currnet and future prefixes matching 203.0.113.x/30 into OSPF as type E1 
- Your configuration must not affect any other prefixes

**Solution 19**

```
R2#show ip route 203.0.113.0
```

```
R1#show ip route connected 
R1#conf 
R1(config)#router ospf 1
R1(config-router)#no redistribute connected
R1(config-router)#ip prefix-list ISP1 seq 10 permit 203.0.113.0/24 ge 30 le 30 
R1(config)#route-map CONN->OSPF permit
R1(config-route-map)#match ip address prefix-list ISP1
R1(config-route-map)#set metric-type type-1
R1(config)#router ospf 1
R1(config-router)#redistribute connected route-map CONN->OSFP subnets
```

```
R2#show ip route ospf 
```

```
R1(config)#router ospf 1
R1(config-router)#route-map CONN->OSPF 20 
R1(config-route-map)#do sh route-map
```

```
R2#show ip route ospf
```

---

**Task 20**

- Summarize all of R1's loopback address as a 1.0.0.0/8 summary route

**Solution 20**

```
R2#show ip route ospf
```

```
R1#conf t 
R1(config)#router ospf 1
R1(config-router)#summary-address 1.0.0.0 255.0.0.0 
R1(config-router)#do show ip route 1.0.0.0
```

```
R2#show ip route ospf
```

---

**Task 21**

- On R4, inject a default route into OSPF using ISP1's 198.51.100.2 address as the next hop

**Solution 21**

```
R4#conf t
R4(config)#ip route 0.0.0.0 0.0.0.0 198.51.100.2
R4(config)#do show ip route static
R4(config-router)#router ospf 1 
R4(config-router)#do debug ip ospf lsa-generation 
R4(config-router)#area 34 nssa default-information-originate
R4(config-router)#do show ip ospf dat | b Type-7 
```

```
R2#show ip route ospf 
R2#show ip route 0.0.0.0
R2#show ip ospf dat | b Type-5
```

```
R1#show ip route ospf 
R1#show ip ospf dat | b ASB
```

**Task 22**

- use a single command on R1 to inject a default route into OSPF

**Solution 22**

```
R1#show ip route
R1#conf t 
R1(config)#router ospf 1 
R1(config-router)#default-information originate always
R1(config-router)#do show ip ospf dat | b Type-5
```

```
R2#show ip ospf dat | b Type-5
R2#show ip route 
R2#show ip ospf rib 0.0.0.0
```

```
R3#show ip route ospf
```

## EIGRP SECTION

**Task 23**

- Establish adjacencies for EIGRP AS 10 according to the IPv4 network topology diagram.
- Ensure R3 and R4 do not inadvertently establish an EIGRP adjacency with any other routers due to a misconfigured network statement
- Do not configure authentication on any routers
- Ensure R3 does not use more than % 20 of tha available bandwith on the interface to R6 for EIGRP traffic.

**Solution 23**

```
R3#conf t 
R3(config)#router eigrp 10
R3(config-router)#passive-interface default
R3(config-router)#no passive-interface fa1/0
R3(config-router)#network 10.0.36.0 0.0.0.7 
R3(config-router)#int fa1/0 
R3(config-if)#ip bandwith-percent eigrp 10 20 
R3(config-if)#do show ip eigrp int det fa1/0
```

```
R6#conf t 
R6(config)#router eigrp 10 
R6(config-router)#network 10.0.0.0 
R6(config-router)#do show ip protocol | begin eigrp
```

```
R5#conf t
R5(config)#router eigrp 10 
R5(config-router)#network 10.0.0.0 
```

```
R4#conf t 
R4(config)#router eigrp 10 
R4(config-router)#passive-interface default
R4(config-router)#network 10.0.45.0 0.0.0.7 
R4(config-router)#no passive-interface fa 2/0
R4(config-router)#do show ip route eigrp 
R4(config-router)#do show ip eigrp topology 
R4(config-router)#do debug eigrp packet query detail reply detail
```

```
R5#conf t
R5(config)#int fa 0/0 
R5(config)#do show intert fa 0/0 
R5(config)#shutdown
```

```
R4#show ip eigrp topology
```

```
R5#conf t
R5(config)#int fa 0/0 
R5(config)#do show intert fa 0/0 
R5(config)#no shutdown
```

```
R4#show ip eigrp topology
R4#show ip route eigrp
```

```
R5(config)#do show ip eigrp neighbour
```

---

**Task 24**

- Configure R5 and R6 not to accept EIGRP updates from one another without proper message authentication
- Use "cisco" as the sole authentication key

**Solution 24**

```
R5#show ip eigrp neighbour
R5#config t 
R5(config)#key chain KC_EIGRP
R5(config-keychain)#key 1 
R5(config-keychain-key)#key-string cisco 
###R5(config-keychain-key)#cryptographic-algorithm md5 
R5(config-keychain-key)#int fa 0/0 
R5(config-if)#ip authentication key-chain eigrp 10 KC_EIGRP 
R5(config-if)#ip authentication mode eigrp 10 md5 
```

```
R6#conf t 
R6(config)# key chain KC_EIGRP
R6(config-keychain)#key 1 
R6(config-keychain-key)#key-string cisco 
###R6(config-keychain-key)#cryptographic-algorithm md5
R6(config-keychain-key)#int fa 0/1 
R6(config-if)#ip authentication key-chain eigrp 10 KC_EIGRP
R6(config-if)#ip authentication mode eigrp 10 md5 
```

```
R6(config-if)#do show ip eigrp 10 inter det fa 0/1
```

---

**Task 25**

- On R5 , configure loopback0 with the address 5.5.5.5/24 and redistribute this prefix into EIGRP 
- Configure R5 to advertise only connected and summary routes into EIGRP 
- then configure R5 to receive bot not advertise any routes
- when finished, remove the stub configuration from R5

**Solution 25**

```
R5#conf t
R5(config)#interface lo0
R5(config-if)#ip address 5.5.5.5 255.255.255.255
R5(config-if)#router eigrp 10
R5(config-router)#network 5.5.5.5 0.0.0.0
```

```
R4#show ip route eigrp
```

```
R5(config)#router eigrp 10
R5(config-router)#eigrp stub 
```

```
R4#show ip eigrp neighbour detail
R4#show ip route eigrp
R4#show ip eigrp topology 
```

```
R5(config)#router eigrp 10
R5(config-router)#eigrp stub receive-only 
```

```
R4#show ip eigrp topology
```

```
R5#show ip eigrp topology
R5(config)#router eigrp 10
R5(config-router)#no eigrp stub 
```

```
R4#show ip eigrp topology
```

---

**Task 26**

- Create and advertise the following loopbacks on R6:
  - Loopback0 : 6.6.6.6/32
  - Loopback1 : 6.0.0.1/32
  - Loopback2 : 6.0.0.2/32
- Ensure R6 automatically summarizes networks to their classful boundaries

```
R6#conf t 
R6(config)#interface loopback 0
R6(config-if)#ip address 6.6.6.6 255.255.255.255
R6(config-if)#interface loopback 1
R6(config-if)#ip address 6.0.0.1 255.255.255.255
R6(config-if)#interface loopback 2
R6(config-if)#ip address 6.0.0.2 255.255.255.255
R6(config-if)#router eigrp 10
R6(config-router)#network 6.0.0.0 
R6(config-router)#do show ip eigrp interface
```

```
R5#show ip eigrp topology 
R5#show ip route
```

```
R6(config-router)#auto-summary 
```

```
R5#show ip eigrp topology
R5#show ip route eigrp
```

```
R6(config-router)#do show ip proto | begin eigrp
```

---

**Task 27**

- Configure and advertise the following loopbacks on R5 
  - Loopback0 : 5.5.5.5/32
  - Loopback1 : 5.0.0.1/32
  - Loopback2 : 5.0.0.2/32
  - Loopback3 : 5.0.0.3/32
- Configure R5 to advertise a classful summary for its loopbacks to R4 only

**Solution 27**

```
R5#conf t 
R5(config)#interface loopback 0
R5(config-if)#ip address 5.5.5.5 255.255.255.255
R5(config-if)#interface loopback 1
R5(config-if)#ip address 5.0.0.1 255.255.255.255
R5(config-if)#interface loopback 2
R5(config-if)#ip address 5.0.0.2 255.255.255.255
R5(config-if)#interface loopback 3
R5(config-if)#ip address 5.0.0.3 255.255.255.255
R5(config-if)#router eigrp 10
R5(config-router)#network 5.0.0.0 
R5(config-router)#int fa 0/1 
R5(config-if)#ip summary-address eigrp 10 5.0.0.0 255.0.0.0 
R5(config-if)#do show ip proto | begin eigrp
```

```
R4#show ip eigrp topology 
R4#show ip eigrp traffic 
```
---

**Task 28**

- On R3 and R4, redistribute all routes from EIGRP AS 10 into OSPF as E1 routes
- Tag redistributed routes as follows:
  - R3 should tag all routes with 3333
  - R4 should tag all routes with 4444

**Solution 28**

```
R3#show ip route eigrp
R3#conf t 
R3(config)#router ospf 1 
R3(config-router)#redistribute eigrp 10 metric-type 1 tag 3333
R3(config-router)#do show ip proto | sec ospf
```

```
R2#show ip route ospf | i E1 
R2#show ip route 5.5.5.5
R2#show ip ospf dat adv-router 3.3.3.3 | b External 
```

```
R4#show ip route eigrp 
R4#conf t 
R4(config)#router ospf 1
R4(config-router)#redistribute eigrp as 10 metric-type 1 tag 4444
R4(config-router)#do show ip proto | sec ospf
R4(config-router)#do show route eigrp 
R4(config-router)#do show ip ospf dat adv-router 4.4.4.4
```

```
R1#show ip route ospf 
R6#show ip route 1.1.1.1
```

**Task 29**

- Redistribute OSPF into EIGRP as 10 
- EIGRP metrics for redistributed routes should be derived from the interfaces leading to OSPF area 0
- Ensure R3 and R4 tag all redistributed routes with 333310 and 333340 respectively

**Solution 29**

```
R3#show cdp neighbour
R3#show int fa 0/1
R3#conf t
R3(config)#router eigrp 10
R3(config-router)#redistribute ospf 1 metric 10000 1 255 1 1500 route-map RM_TAG 
R3(config-router)#route-map RM_TAG permit 10 
R3(config-router-map)#set tag 333310
R3(config-router-map)#end
R3#show ip route ospf 
```

```
R3#show ip route 1.1.1.1 
```

```
R4#show int fa 2/0 
R4#conf t 
R4(config)#router eigrp 10 
R4(config-router)#redistribute ospf 1 metric 10000 1 255 1 1500 route-map RM_TAG 
R4(config-router)#route-map RM_TAG permit 10
R4(config-router-map)#set tag 444410
R4(config-router-map)#end
R4#show ip eigrp topology 
```
---

**Task 30**

- R5 is taking a suboptimal route to R1's 1.1.1.1 loopback 
- Ensure R5 takes the shortest path to R1's 1.1.1.1 loopback 
- Do not create or modify any static or default routes 

**Solution 30**

```
R5#traceroute 1.1.1.1 
R5#show ip route 1.1.1.1
```

```
R4#show cdp neighbour
R4#conf t 
R4(config)#interface fa 0/1
R4(config)#router eigrp 10 
R4(config-router)#do show int fa 0/1
R4(config-router)#redistribute rip metric 10000 1 255 1 1500 
R4(config-router)#do show ip route 1.1.1.1 
R4(config-router)#router ospf 1
R4(config-router)#redistribute rip subnets
R4(config-router)#do clear ip route 
R4(config-router)#do show ip route 1.1.1.1
```

```
R5#traceroute 1.1.1.1.
R5#ping 1.1.1.1
```

**Task 31**

- On R3 and R4, create the loopback34 interface with the IP address of 34.34.34.34
- Advertise this prefix into EIGRP 
- Configure R6 to perform load sharing to this accross R3 and R5

**Solution 31**

```
R3#conf t 
R3(config)#interface lo34
R3(config-if)#ip address 34.34.34.34 255.255.255.255
R3(config-if)#router eigrp 10
R3(config-router)#network 34.34.34.34 0.0.0.0 
```

```
R4#conf t 
R4(config)#interface lo34
R4(config-if)#ip address 34.34.34.34 255.255.255.255
R4(config-if)#router eigrp 10
R4(config-router)#network 34.34.34.34 0.0.0.0
```

```
R6#show ip eigrp topology 34.34.34.34/32
```

```
R3(config)#router eigrp 10
R3(config-router)#variance 2 
R3(config-router)#do show ip protocol 
R3(config-router)#do show ip route 34.34.34.34
R3(config-router)#do show ip eigrp topology 
```
---

**Task 32**

- Disable auto summarization on R6 
- On R4 and R6 , create the loopback46 interface with the IP address of 46.46.46.46/32
- Advertise this prefix into EIGRP 
- Verify R5 uses equal cost load sharing to reach this prefix via both R4 and R6 

**Solution 32**

```
R6(config)#int lo46 
R6(config-if)#ip address 46.46.46.46 255.255.255.255
R6(config-if)#router eigrp 10
R6(config-router)#network 46.46.46.46 0.0.0.0
R6(config-router)#no auto-summary
```

```
R4(config)#int lo46 
R4(config-if)#ip address 46.46.46.46 255.255.255.255
R4(config-if)#router eigrp 10
R4(config-router)#network 46.46.46.46 0.0.0.0
R4(config-router)#no auto-summary
```

```
R5#show ip route 46.46.46.46
R5#show ip eigrp top 46.46.46.46/32
```

**Task 33**

- Configure the following loopback:
   - R5: loopbacl50 - 50.50.50.50/32
- Advertise this loopback into EIGRP AS 10
- Ensure R5 dooe not advertise its new loopback directly to R6 

**Solution 33**

```
R5(config)#interface lo 50
R5(config-if)#ip address 50.50.50.50 255.255.255.255
R5(config-if)#router eigrp 10
R5(config-router)#network 50.50.50.50 0.0.0.0
R5(config-router)#distribute-list 50 out fa 0/0
R5(config-router)#access-list 50 deny 50.50.50.50 0.0.0.0
R5(config)#access-list 50 permit any 
R5(config)#do show ip protocol | begin eigrp
```

```
R6#show ip route 50.50.50.50
R6#show ip eigrp topology 50.50.50.50/32
```

**Task 34**

- A static default route already exists on R4
- Configure R4 to advertise a default route into Eigrp
- Use an advertised bandwidth of 1000000 Kbps and delay of 10  mikro seconds
- Your configuration must not affect the metrics of any other current or future routes
- Do not use a route map

**Solution 34**

```
R4(config)#router eigrp 10 
R4(config-router)#redistribute static 
R4(config-router)#default-metric 1000000 1 255 1 1500 
R4(config-router)#do show ip route 0.0.0.0
```

```
R5#show ip route 0.0.0.0
```
---

## BGP SECTION

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

**Solution 35**

```
ISP1#conf t
ISP1(config)#int lo11
ISP1(config-if)#ip address 11.11.11.11 255.255.255.255
ISP1(config-if)#int lo12
ISP1(config-if)#ip address 12.12.12.12 255.255.255.255
ISP1(config-if)#int lo13
ISP1(config-if)#ip address 13.13.13.13 255.255.255.0
ISP1(config-if)#int lo14 
ISP1(config-if)#ip address 14.14.14.14 255.255.255.255
ISP1(config-if)#int lo15
ISP1(config-if)#ip address 15.15.15.15 255.255.255.255
ISP1(config-if)#int lo55
ISP1(config-if)#ip address 55.55.55.55 255.255.255.255
ISP1(config-if)#int lo66
ISP1(config-if)#ip address 66.66.66.66 255.255.255.0
ISP1(config-if)#int lo77
ISP1(config-if)#ip address 77.77.77.77 255.255.255.255
ISP1(config-if)#router bgp 65534
ISP1(config-router)#neighbour 203.0.113.1 remote-as 65534
ISP1(config-router)#neighbour 203.0.113.1 password cisco
ISP1(config-router)#neighbour 198.51.100.1 remote-as 65534
ISP1(config-router)#neighbour 198.51.100.1 password cisco
ISP1(config-router)#neighbour 203.0.113.1 remote-as 65534
ISP1(config-router)#redistribute connected

```

```
R1#conf t
R1(config)#router bgp 65534 
R1(config-router)#neighbour 203.0.113.2 remote-as 65535
R1(config-router)#do show ip bgp neighbour 
R1(config-router)#neighbour 203.0.113.2 password cisco 
R1(config-router)#do show ip bgp 
```

```
R1#conf t
R1(config)#router bgp 65534 
R1(config-router)#neighbour 198.51.100.2 remote-as 65535
R1(config-router)#do show ip bgp neighbour 
R1(config-router)#neighbour 198.51.100.2 password cisco 
R1(config-router)#do show ip bgp 
```

**Task 36**

- On R1 , redistribute connected routes into BGP as 65534
- Perform mutual redistribution as follows :
   - On R1, between BGS AS 65534 and OSPF
   - On R4, between BGP AS 65534 and EIGPR 10

**Solution 36**

```
R1#conf t 
R1(config)#router bgp 65534
R1(config-router)#redistribute connected
R1(config-router)#redistribute ospf 1 
R1(config-router)#router ospf 1
R1(config-router)#redistribute bgp 65534
```

```
R4#conf t 
R4(config)#router 65534
R4(config-router)#redistribute eigrp 10
R4(config-router)#router eigrp 10
R4(config-router)#redistribute bgp 65534 metric 10000 10 255 1 1500
R4(config-router)#do show ip bgp
R1(config-router)#do show ip bgp 203.0.113.1
```

**Task 37**

 - Ensure ISP1 selects the path through 198.51.100.1 as its best path to the 1.0.0.0/8 prefix
 - Do not make any changes to ISP1 


**Solution 37**

```
ISP1#show ip bgp 
ISP1#show ip bgp 1.0.0.0
```

```
R4#show ip route 1.0.0.0 255.0.0.0
R4#conf t
R4(config)#router bgp 65534
R4(config-router)#network 1.0.0.0
```

```
ISP1#show ip bgp 
ISP1#show ip bgp 1.0.0.0
```

**Task 38**

 - Ensure R4 removes its existing static default route if ISP1's interface IP 198.51.100.2 becomes unreachable.

**Solution 38** 

```
4#show ip route 0.0.0.0
R4#conf t
R4(conf)#int fa 0/0
R4(config-if)#shutdown
R4(config-if)#do show ip route 0.0.0.0
R4(config-if)#do show ip route 198.51.100.2
R4(config-if)#do trace 198.51.100.2
R4(config-if)#ip sla 1
R4(config-ip-sla)#icmp-echo 198.51.100.2
R4(config-ip-sla-echo)#timeout 5000
R4(config-ip-sla-echo)#frequency 5 
R4(config-ip-sla-echo)#ip sla schedule 1 life forever start-time now
R4(config)#track 1 ip sla 1 reachability
R4(config-track)#do show track 
R4(config-track)#int fa 0/0
R4(config-if)#no shutdown
R4(config-if)#do show track 
R4(config-if)#do show run | i ip route
R4(config-if)#no ip route 0.0.0.0 0.0.0.0 198.51.100.2
R4(config)#ip route 0.0.0.0 0.0.0.0 198.51.100.2 track 1 
R4(config)#do show track 
R4(config)#int fa 0/0
R4(config-if)#shutdown 
R4(config-if)#do show ip route 0.0.0.0
R4(config-if)#no shutdown

```

**Task 39**

- On R4 , configure a floating static route that uses R1's interface IP 10.0.141.1 as its default gateway
- Configure this route with an administative distance of 4

**Solution 39**

```
R4(config)#ip route 0.0.0.0 0.0.0.0 10.0.14.1 4
R4(config)#do show ip route 0.0.0.0
R4(config)#int fa 0/0 
R4(config-if)#shutdown
R4(config-if)#do show ip route 0.0.0.0
```

**Task 40**

- On R5, configure a static route for the 8.0.0.0/8 prefix using fa 0/1 as the next hop interface 

**Solution 40**

```
R5#conf t
R5(config)#ip route 8.0.0.0 255.0.0.0 fa 0/1
```

**Task 41**

- On R1 and R3, configure loopback 31 with the anycast address 31.31.31.31/32
- Advertise this prefix into OSPF
- Ensure the path through R1 is preferred

**Solution 41**

```
R1(config)#int lo 31
R1(config-if)#ip address 31.31.31.31 255.255.255.255
R1(config-router)#network 31.31.31.31 0.0.0.0 area 0

```

```
R3(config)#int lo 31
R3(config-if)#ip address 31.31.31.31 255.255.255.255
R3(config-router)#network 31.31.31.31 0.0.0.0 area 23
```

```
R2#show ip route 31.31.31.31
R2#show ip ospf int bri
R2#conf t
R2(config)#int fa 0/0
R2(config-if)#ip ospf cost 9
```

---