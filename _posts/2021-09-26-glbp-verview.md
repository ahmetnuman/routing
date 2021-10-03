---
layout: post
title: Gateway Load Balancing Protocol
tags: Gateway Load Balancing Protocol Protocol
categories: glbp
---

> Ahmet Numan Aytemiz , 26.09.2021

---

## GLBP Overview

- Cisco Proprietary
- Provides gateway redundancy and per-host load-balancing
- AVG (Active Virtual Gateway) in charge of determining host-to-gateway allocataions
- AVG (Active Virtual Gateway) replies to the arp request sent to the virtual IP
- Preemption for role of AVG on by default
- A single AVG per group;
  - Highest priority 
  - Highest IP address
- Gateways capable of forwarding packets in GLBP are called AVF 
  - Active Virtual Forwarder
  - Maximum of 4-AVF per group
- AVG is also an AVF
- Each AVF assigned virtual MAC
  - 0007.b4xx:xxyy where:
     - xx:xx = GLBP group
     - yy = AVF
- AVFs request their AVF# and virtual MAC from AVG
- AVG and AVFs all send Hello packets (3-secs)

## Implementing GLBP

- Enabling GLBP in the interface
  - `sw1(config-if)#glbp <group-id> ip <virtual-ip>`

- Configuring Priority
  - `sw1(config-if)#glbp <group-id> priority <priority>`

- Disabling Preemption
  - `sw1(config-if)#no glbp <group-id> preempt`

## GLBP Load Balancing

- Load Balancing Algorithms:
  - Round-robin (default)
  - Host dependent
  - weighted

- Configure with the following command `glbp <group-id> load-balancing <wieghted | round | host>`  

## GLBP Weighted Load Balancing

- Configured on the AVG:
  - `glbp <group-id> load-balancing weighted`

- Configured on AVFs
  - glbp <group-id> weighted <value> lower <value> upper <value>

## AVF Object Tracking

- Every router has default AFV weight = 100 (maximum value)
- Benath "lower" weight, router can no longer pariticipate as AVF
- Object tracking can be used to dynamically decrement weight value if tracking object fails.

- `sw1(config)# track 1 < application | interface | ip | list | ....>`
- `sw1(config)# glbp 1 weighting track 1 <decrement value>`
