---
layout: post
title: Injecting Routes Into BGP
tags: Injecting Routes Into BGP
categories: bgp
---

> Ahmet Numan Aytemiz , 29.10.2021

---

- There are a few ways to inject routes into BGP:
  - By using the BGP **network** command
  - Bu using redistribution
- Routes injected by either of these methods have some advantages and requirements.
- Redistribution can be an easy way to inject many IGP routes into BGP table.
- This can be accomplished by using the **redistribute** command in BGP configuration mode
- To verify BGP routes, use the following command:
  - `show ip bgp` 

#### **Downsides of Redistribution**

- Will also advertise private IGP networks
- If redistributing static routes, actual reachability is not tracked.
- Origin code not preferred over routes advertised with "network" command.

#### **Route Injectinon**

- The **network** command for BGP is different than IGP.
- BGP does not use the **network** command to enable BGP on interfaces.
- The BGP **network** command looks **exact** prefix/length matches in the IP routing table , and originates that prefix/lenght int the BGP table.
- To inject a route into BGP, use the following command:
  - `network <subnet> mask <subnet-mask>`
- If the **mask** parameter is omitted, IOS assumes a classfull network mask.
- By default auto-summary is disabled.
- What happens when auto-summary is enabled? How is the **network** command affected for route injection?

#### **Effect of Auto-Summary on Network Command**

- If auto-summary is configured but mask parameter is configured with the **network** command, the logic of the network statement is not affected.
- When auto-summary is configured , the logic of the network statement changes only if the **mask** parameter is omitted.
- With no mask parameter configured, and with auto-summary configured, the router adds a route for that classful network to the BGP table. 
- The classful route is added if:
  - The exact classful route is in the IP routing table.
  - Any subset routes of that classful network are in the routing table.
- The firsr occurs regardless of the auto-summary setting, and the seconf occurs only if auto-summary is configured.  

#### **Injecting Routes Using Aggregation**

- Another way to add routes into the BGP table is using BGP summarization.
  - Subsets of summarized routes must first be matched by either "redistribution" or "network" statements.
- To configure BGP summarization/aggregation, use the following command:
  - `aggregate-address <prefix> <prefix-length> [summary-only]`   
- When any subnet route (subordinate routes) of prefix/prefix-length exists in the BGP table, summary route is injected into the BGP table.
- When subset routes (sub-ordinate routes) are not advertised with summary route, they are suppressed and can be recognized in BGP by the code **s** beside them.
- To verify subordinate routes, use the following command.
  - `show ip bgp <prefix> <prefix-length> longer-prefixes`

#### **Lab Time**

![topology](/img/bgp_network.PNG)

- Configure all interfaces ip address according to topology
- Configure eBGP neigborships according to topology
- On the R1 ;
  - Advertise 10.0.0.0 classfull network and verify 10.0.0.0 network is not in the bgp table.
  - Change lo0 ip address with 10.10.10.1/8 and verify bgp table again and verify the 10.0.0.0 network is in the bgp table.
  - Chahge lo0 ip address with 10.10.10.1/24 , advertise 10.10.10.0/24 network in the bgp and do auoto summary. 


---------------------

#### **Answers**

- Configure all interfaces ip address according to topology

#### R1

```
interface Loopback0
 ip address 10.0.0.1 255.255.255.0
!
interface Ethernet0/0
 ip address 12.12.12.1 255.255.255.252
!
```

#### R2

```
interface Loopback0
 ip address 2.2.2.2 255.255.255.0
!
interface Ethernet0/0
 ip address 12.12.12.2 255.255.255.252
!         
interface Ethernet0/1
 ip address 23.23.23.1 255.255.255.252
!
```

#### R3 

```
interface Loopback0
 ip address 3.3.3.3 255.255.255.0
!
interface Ethernet0/0
 ip address 23.23.23.2 255.255.255.252
!
```

---

- Configure eBGP neigborships according to topology

#### R1

```
!
router bgp 100
 neighbor 12.12.12.2 remote-as 200
```

#### R2

```
!
router bgp 200
 neighbor 12.12.12.1 remote-as 100
 neighbor 23.23.23.2 remote-as 300
```

#### R3

```
!
router bgp 300
 neighbor 23.23.23.1 remote-as 200
```

----

- On the R1 ;
  - Advertise 10.0.0.0 classfull network and verify 10.0.0.0 network is not in the bgp table.

```
router bgp 100
 network 10.0.0.0
```

`show ip bgp`

![topology](/img/doshowipbgp.PNG)




  - Change lo0 ip address with 10.10.10.1/8 and verify bgp table again and verify the 10.0.0.0 network is in the bgp table.

```
!
interface Loopback0
 ip address 10.10.10.1 255.0.0.0
```

`show ip bgp`

![topology](/img/doshowipbgp2.PNG)

  - Chahge lo0 ip address with 10.10.10.1/24 , advertise 10.10.10.10/24 network in the bgp and do auoto summary. 

```
!
interface Loopback0
 ip address 10.10.10.1 255.255.255.0
```

```
router bgp 100
 network 10.10.10.0 mask 255.255.255.0
 neighbor 12.12.12.2 remote-as 200
 auto-summary
```



















