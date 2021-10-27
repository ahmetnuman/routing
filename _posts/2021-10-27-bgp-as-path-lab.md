---
layout: post
title: Bgp As Path Attribute Lab
tags: Bgp As Path Attribute Lab
categories: bgp
---

> Ahmet Numan Aytemiz , 27.10.2021

![Image](/img/as_path_topology.PNG)

**Tasks**

1. Configure hostnames adn IP addresses on all routers as illustrated in the network topology.
2. Configure VPC's IP address.
3. Configure eBGP neigborship between, bu using phsical interfaces ;
  - R1 <---> R2
  - R1 <---> R6
  - R6 <---> R5
  - R5 <---> R7
  - R4 <---> R7

4. Configure static route on R2  
  - 34.34.34.2/32 forward to gateway 23.23.23.2

5. Configure static route on R4
  - 23.23.23.1/32 forward to gateway 34.34.34.1

6. Confiure static route on R3 ;
  - 1.1.1.0/24 forward to gateway 23.23.23.1
  - 7.7.7.0/24 forwart to gateway 34.34.34.2

7. Configure iBGP neigborship between R2 and R4 routers, using phical interface's ip address. 
8. On the R1 inject 1.1.1.0/24 network
9. On the R7 inject 7.7.7.0/24 network
10. Confiure route reflector R2 for R4 iBGP neighbor. 
11. On the R2, configure route-map, set next-hop attribute 23.23.23.1 and inject it R4
12. On the R4, configure route-map, set next-hop attribure 34.34.34.2 and inject it R2 
13. Examine routers bgp table 
14. trace from 1.1.1.10 to 7.7.7.10 and verify the packets path (R1 --> R2 --> R3 --> R4 --> R7) 



