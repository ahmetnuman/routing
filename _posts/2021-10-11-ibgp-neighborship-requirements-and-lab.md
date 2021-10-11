---
layout: post
title: iBGP Neighbourship Requirements, Configuration and lab
tags: iBGP Neighbourship Requirements, Configuration and Lab
categories: bgp
---

> Ahmet Numan Aytemiz , 11.10.2021

---

#### iBGP Neighborship Requirements

- BGP neighborship between routers that are in the same AS are iBGP neighbors
- When is iBGP neighborship required ?
- When two Internet-connected routers need to communicate BGP routes to each other because these routers may want to forward IP packets to the other Inernet-connected router
- The neighborship requirements for iBGP are the same as eBGP except for the "asn" value in the **neigbor remote asn** command
- The **neighbor remote-as** command will list the same AS number as the local AS number, which is configured with the **router bgp asn** command
- The other requirements for iBGP neighborship are just like eBGP
  - The BGP router IDs of the two routers must not be the same
  - If configured , MD5 authentication must pass
  - Each router must be part of a TCP connection with the other router, with the remote router's IP address used in that TCP connection matching what the local router configures in a bgp **neighbor remote-as** command
- The concept of the update source is the same with iBGP neighbor as it was with eBGP neighbors
- **The exception here is the TTL value for iBGP neighbors, 255 by default**
- There is no need to change the TTL value for iBGP neighbors through any multihop command.

#### iBGP Configuration Commands

- The following commands are used to configure iBGP neighborships.

```
router bgp asn
neighbor remote-as asn
neighbor update-source
neighbor password
```

- To verify the iBGP neighborship, use the command `show ip bgp summary`

---

## Lab Time 

**Configure ibgp neighborship between Router1 and Router2 with authentciaton (password:cisco)**

![Image](/img/ibgp.PNG)

#### Router1

```
interface Loopback0
 ip address 11.11.11.11 255.255.255.255
!
interface Ethernet0/0
 no switchport
 ip address 12.12.12.1 255.255.255.252
```

```
ip route 22.22.22.22 255.255.255.255 12.12.12.2
```

```
router bgp 100
 bgp log-neighbor-changes
 neighbor 22.22.22.22 remote-as 100
 neighbor 22.22.22.22 password cisco
 neighbor 22.22.22.22 update-source Loopback0
```

#### Router2

```
interface Loopback0
 ip address 22.22.22.22 255.255.255.255
!
interface Ethernet0/0
 no switchport
 ip address 12.12.12.2 255.255.255.252
!

```

```
ip route 11.11.11.11 255.255.255.255 12.12.12.1
```

```
router bgp 100
 bgp log-neighbor-changes
 neighbor 11.11.11.11 remote-as 100
 neighbor 11.11.11.11 password cisco
 neighbor 11.11.11.11 update-source Loopback0
```

