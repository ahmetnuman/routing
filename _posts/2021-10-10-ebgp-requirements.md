---
layout: post
title: eBGP Neighbourship Requirements
tags: eBGP Neighbourship Requirements
categories: bgp
---

> Ahmet Numan Aytemiz , 10.10.2021

---

#### eBGP Neigbourship Overview

- Like any other routing protocol, BGP must also complete three steps to get best routes ;
  - form neighbourships
  - exchange topology information
  - run a best path algorithm

- The first step is always to form neighbourship
- BGP forms neighbourship using TCP port 179
- BGP neighbours do not need to be on the same IP subnet
- To configure eBGP neighbours, use the following commands :

```
router bgp asn (global command)
neighbour ip-address remote-as remote-asn (bgp subcommand)
```

- The asn number in the router bgp command is the local AS number of the router
- The remote-as is the autonomous system of the eBGP peer
- Like any other routing protocols, to form neighborship certain requirements or cheks must pass before two reouters can become neighbours, the same is true for BGP

#### eBGP Neighbourship Requirements

- The following the requirements must be met for router's to become neighbours
  - A local router's ASN (on the router bgp asn command) must match the neighbouring router's reference th that ASN with its ``neighbour remote-as asn` command
  - The BGP router IDs of the two routers must not be the same
  - If configured, MD5 authentication must pass
  - The peer must reachable via an IGP
  - Each router must be part of a TCP conneciton with the other router, with the remote router's IP address used in that
  - TCP connection matching what the local router configures in a BGP neighbour remote-as command
  - Just any IGP , BGP elects a Router-ID
  - The BGP router id is elected as the following
    - Use the setting of the `bgp router-id rid` router subcommand
    - Choose the highest numeric IP address of any up/up loopback interface, at the time the BGP process initializes.
    - Choose the highest numeric IP address of any up/up non-loopback interface, at the time the BGP process initializes.  
  - The third requirement for BGP neighbourship is the MD5 authenticaiton check
  - To configure authenticaition for BGP, use the following command
    - `neighbour neighbour-ip password key` (BGP subcommand)
  - This command must be configured on both routers
  - If keys do not match or this command is only configured on one router, neighbourship will not be formed.    
