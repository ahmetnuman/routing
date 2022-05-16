---
layout: post
title: Eigrp Basic Configuration
tags: Eigrp Basic Configuration
categories: eigrp
---

> Ahmet Numan Aytemiz , 15.05.2022

---

## EIGRP Basic Configuration


![Image](/img/basic-eigrp.png)


- Wildcard mask is inverted subnet masks. (1's --> 0's, 0's --> 1's )
- Wildcard mask is 32 bits long.
- To form a neighborship , EIGRP has these requirements :
  - Interface's primary IP addresses must be on the same subnet.
  - Connected interface must not be passive.
  - Routers must use the same AS number
  - Must pass authentication
  - k-values must match.

| Commands      | Description |
| ----------- | ----------- |
| R1(config)#router eigrp 1      | Enter eigrp mode      |
| R1(config-router)#network 192.168.12.0   | Accept eigrp packets matching interface and Advertise 192.168.12.0 network (if it is in your routing table)        |
| R1(config-router)#network 1.1.1.0   | Accept eigrp packets matching interface and Advertise 1.1.1.0 network (if it is in your routing table)        |
| R1(config-router)#network 0.0.0.0   | Accept eigrp packets matching interface and Advertise all network in your routing table network         |

---

| Eigrp Commands      | Description |
| ----------- | ----------- |
| R1(config-router)#auto-summary      | enable auto summarization feature (enable by default)      |
| R1(config-router)#no auto-summary      | disable auto summarization feature       |
| R1(config-router)#metric wights tos k1 k2 k3 k4 k5     | Adjusting EIGRP Metric Weigths      |
| R1(config-router)#metric maximum-hops <1-255>    | Advertise greater than hops      |
| R1(config-router)#maximum-paths <1-32>    | Set the maximum equal paths      |
| R1(config-router)#variance <1-128>    | Control unequal load balancing      |
| R1(config-if)#ip hello-interval eigrp -asn-  -interval-   | Changing eigrp hello interval      |
| R1#show ip eigrp neighbors   | Display the neighbor table in brief      |
| R1#show ip eigrp neighbors detail   | Display the neighbor table in detail. To verify the neighbor is configured as stub router      |
| R1#show ip eigrp interfaces   | Display info about all EIGRP interfaces      |
| R1#show ip eigrp interfaces f 0/0   | Display info EIGRP interface      |
| R1#show ip eigrp interfaces 20   | Display info EIGRP interfaces AS 20      |
| R1#show ip eigrp topology   | Display the topology table      |
| R1#show ip eigrp traffic   | Display EIGRP packets     |
| R1#show ip route eigrp    | Display EIGRP route from routing table     |
| R1#debug eigrp fsm    | Display the events releated to FSM     |
| R1#no debug eigrp fsm    | turn off FSM debug     |
| R1#debug eigrp packet    | Display EIGRP event packets   |
| R1#no debug eigrp packet   | turn off eigrp packets debug     |







