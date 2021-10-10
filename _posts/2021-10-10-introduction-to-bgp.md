---
layout: post
title: Introduction to Border Gateway Protocol
tags: Introduction to Bortder Gateway Protocol
categories: bgp
---

> Ahmet Numan Aytemiz , 10.10.2021

---

- Designed as an Exterior Gateway Protocol (EGP)
- Advertises, learns, and chooses the best paths inside the global internet
- ISPs typically use BGP to exchange routing information between themselves
- Can also be used for very large Enterprise Networks
- Enterprises sometimes use BGP to exchange routing information with one or more ISPs
- BGP has a robust best-path algorithm
- BGP uses this best path algorithm to choose the best BGP path
- The best path selected based on multiple criteria
- Because of this complex and robust best-path algorithm, different attributes of BGP can be changed, accourding to design needs, to influence selection of the best path

## Comparison Between IGPs and BGP

- Similarities and differences between BGP and IGPs (OSPF and EIGRP)
  - BGP needs to form neighbourship like IGPs
  - BGP needs to advertise prefixes, just like IGPs.
  - BGP also advertises Next Hop for those prefixes.
  - Neighbour IP address may not be on a common subnet for BGP
  - IGPs do not use TCP, whereas BGP uses TCP port 179 between neighbours.
  - BGP-advertised prefix/length is also known as network layer reachability information (NLRI)
  - BGP needs to advertise different path attributes so other BGP speaking routers can make best-path desicion, just like IGPs (which advertise metric/cost)
  - IGPs emphasize fast convergence to the most efficient route, whereas BGP emphasizes scalability
  - BGP uses path vector logic (similiar to distance vector)