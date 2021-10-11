---
layout: post
title: Bgp Update Source and Multi-Hop Requiremets
tags: Bgp Update Source and Multi-Hop Requiremets
categories: bgp
---

> Ahmet Numan Aytemiz , 11.10.2021

---

#### BGP Update Source

- Each router must be part of a TCP connection with the other router, with the remote router's IP address used in that TCP connection matching what the local router configures in a BGP `neighbour remote-as` command for eBGP neighborship requirements.

- This TCP connection should be form before BGP flow over this TCP connection.

- The local router tries to form a TCP connection witht the IP address defined in the `neighbor remote-as` command

- The local router then finds the outgoing interface to be used to reach that IP address

- The IP address of the outgoing interface is used as source IP address for TCP connection , by default.

- This is true for other direction as well

- What happens when there are two or redundant layer 3 paths between the same pair of routers?

![Image](/img/ebgp_multihop.PNG)

- The failure in one link can cause BGP neighborship to fail

- There are two solutions to resolve this issue:
  - Configure two neighbor commands on each router
  - Use loopback interfaces as the TCP connection endpoints

- The use of two neighborships between the same pair of routers can consume bandwidth and more memory in the BGP table

- To configure eBGP neighborship using loopback interfaces, follow these steps:
  - Configure an IP address on a loopback interface on each router
  - Configure the BGP neighbor command on each router to refer to the other router's loopback IP address at the neighbor IP address in the `neighbor neighbor-ip remote-as` command
  - Tell BGP on each router to use the loopback IP address as the source IP address using the `neighbor update-source ip-address` command
  - Make sure each router has IP routes so that they can forward packets to the loopback interface IP address of the other router.
  - Configure eBGP multihop using the `neighbor ebgp-multihop` hops command. 

## eBGP Multihop Concept

- By default, when building packets to send to an eBGP peer, IOS sets the IP Time-To-Live (TTL) field in the IP header to a value of 1.

- When using a loopback IP address to form eBGP neighborship with TTL value of 1, neighbor will not come up.

- This can also be true if using phsical IP addresses to form neighborship for routers that are not directly connected.

- eBGP neighbors do not come up as the packet with TTL value of 1 gets dropped.

- TTL value is decremented by 1 before giving the packet to the loopback interface.

- To increase TTL value, use the `neighbor ebgp-multihop` command.

- To verify eBGP neighbors, use the following commands:
  - `show ip bgp summary`
  - `show ip bgp neighbors`




 

- 


