---
layout: post
title: Bgp Message Types
tags: Bgp Message Types
categories: bgp
---

> Ahmet Numan Aytemiz , 21.10.2021

#### BGP Message Header and Types

- All BGP messages carried within IP/TCP Headers

![topology](/img/bgp_header.PNG)

- BGP uses four types of messages for its operation:

- **Open** Message
- **Update** Message
- **Keepalive** Message
- **Notification** Message

#### BGP Message Types - **Open**

- BGP Open Message:
  - **Used in neighbor establishment**
  - BGP values and capabilities are exchanged

![topology](/img/bgp_open.PNG)

![topology](/img/open_sniff.PNG)

#### BGP Message Types - Update

- BGP Update Message:
  - Inform neighbors about withdrawn routes, changes routes and new routes.
  - Used to exchange PAs and associated prefix/length (NLRI) that use those attributes.

![topology](/img/bgp_update.PNG)

![topology](/img/update_sniff.PNG)

#### BGP Message Types - Notification

- BGP Notification Message :
  - Used to signal a BGP error, typically results in a reset to the neighbor relationship

![topology](/img/bgp_notification.PNG)

![topology](/img/noti_sniff.PNG)

#### BGP Message Types - Keepalive

- BGP Keepalive message :
  - Sent on a periodic basis to maintain the neighbor relationship. The lack of receipt of a Keepalive message within the negotiated Hold timer causes BGP to bring down the neighbor connection.

![topology](/img/bgp_keepalive.PNG)

![topology](/img/keep_sniff.PNG)

## Examining BGP table

- To verify BGP table , use command : `show ip bgp`
- The output will list all the BGP learned routes, locally injected plus learned from neighbors.
- With each prefix it will have multiple attributes that can be examined and used for best path selection.
- Each prefix can have multiple paths with different next-hops.

![topology](/img/showipbgp.PNG)

- Prefix with **\*** are vaild to be considered for best-path algorithm.
- Best path is presented by **\>** 
- The Path heading shows the AS_Path Attribute.
- The BGP **show** commands list the AS_Path with the first-added ASN on the right and the las added ASN on the left.

## Verification Commands for eBGP Learned Routes

```
show ip bgp <prefix> [subnet-mask]
show ip bgp neighbors <ip-address> received-routes
show ip bgp neighbors <ip-address> routes
show ip bgp neighbors <ip-address> advertised-routes
show ip bgp summary 
```
