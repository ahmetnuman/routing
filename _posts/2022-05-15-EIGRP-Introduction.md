---
layout: post
title: eigrp introduction
tags: eigrp introduction
categories: bgp
---

> Ahmet Numan Aytemiz , 15.05.2022

---

## EIGRP (Enhanced Interior Gateway Routing Protocol)

- Eigrp stands for Enhanced Interior Gateway Routing Protocol
- EIGRP is a Cisco Proprietary routing Protocol however open in 2013.
- It is hybrid routing protocol sometime called Advanced Distance Vector.
- It has characteristics of both distance vector and link state protocols.
- It uses DUAL (Diffusing Update Algorithm) to select best path.
- It uses RTP (Reliable Transport Protocol) to communicate with neighbors.
- Enhanced Interior Gateway Routing Protocol uses multicast for updates.
- EIGRP supports both Internet Protocol V4 and IPv6 roueted protocols.
- EIGRP includes subnet mask information in the routing update messages.
- EIGRP supports route summarization and discountinuous networks.
- EIGRP protocol supports VLSM, CIDR also support trigger updates.
- It sends partial or full update only when something is changed in the network.
- The default Internal Administrative Distance of EIGRP protocol is 90.
- The default External Administrative Distance of EIGRP protocol is 170 (when we apply redistribution).
- The EIGRP default hop count support is 100 but it can be tune 255.
- EIGRP protocol support Equal Cost Load and Unequal Cost Load Balancing.
- EIGRP take load balancing by default up to 4 paths can configure up to 32
- Hello time of EIGRP protocol is 5 seconds and dead time is 15 seconds.
- EIGRP updates are sent to 224.0.0.10 on multicast internet protocol address.
- EIGRP support md5 authenticaiton and by default auto summarization is enabled.

## EIGRP Tables 

- Neighbor Table
- Topology Table
- Routing Table

![Image](/img/eigrp-intro.png)

- [Eigrp Pcap File](https://github.com/ahmetnuman/routing/raw/main/eigrp-intro.pcapng)

