---
layout: post
title: Bgp Neighbor States
tags: Bgp Neighbor States
categories: bgp
---

> Ahmet Numan Aytemiz , 18.10.2021

---

- **Idle State :** 
  - The BGP process is either or administratively down or awaiting the next retry attempt.

- **Connect State :**
  - The BGP process is waiting for the TCP connecition to be completed.
- **Active State :**
  - The TCP conneciton failed, Connect-retry timer running, listening for incoming TCP connection

- **Opensent State :**
  - The TCP connection exists, and a BGP Open message has been sent to the peer, but matchibg Open messages has not yet been received from the other router.

- **Openconfirm State :**
  - An open message has been both sent to and received from the other.

- **Established State :** All neighbor parameters match, the neighbor relationship works and the peer can now exchange Update messages.

- **The Overall process works as follows:**
  - A router tries to establish a TCP connection with the IP address listed on a **neighbor** command, using well-known destination port 179.
  - When the three-way TCP connection copmletes, the router sends its first BGP message, the BGP Open message, which generally performs the same function as the EIGRP and OSPF Hello messages. The Open message contains several BGP parameters, including those that must be verified before allowing the routers to become neighbors.