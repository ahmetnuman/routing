---
layout: post
title: Hot Standby Routing Protocol
tags: Hot Standby Routing Protocol
categories: hsrp
---

>> Ahmet Numan Aytemiz, 23.09.2021

## HSRP Overview

- Cisco proprietary 
- Uses UDP port 1985 and multicast address 244.0.0.2
- Two Roles: Active and Standby
- HSRP router with the highest priority is considered "Active"
- Default priorty 100
- Virtual Mac address: 0000.0c07.acxx
  - xx refers to the group number in hexadecimal  
- Preemption default by default
- No load sharing feature

## Implementing Basic HSRP

- Enabling HSRP in the interface
  - `router(config-if)#standby <group-id> ip <virtual-ip>`
- Configuring priority
  - `router(config-if)#standby <group-id> priority <priority>`  
- Enabling preemption
  - `router(config-if)#standby <group-id> preempt`

## HSRP State Machine

- Disabled
- Initial (INIT)
- Learn 
- Listen
- Speak
- Standby / Active

## HSRP Hello Timers

- HSRP hello timer = 3-sec
- Dead Timer = 10-sec (or 3x hello)

## HSRP Authentication

- Authentication supported
  - Plain text
  - MD5

- Plain-text configuration
  - `router(config-if)#standby <group-id> authentication <password>`

- MD5 configuration
  - `router(config-if)#standby <group-id> authentication md5 key-string [0|7] string`

## HSRP Object tracking

- HSRP can track objects like interfaces, ip adress
- If tracked object fails, HSRP priority is reduced by configurable amount (default 10)

- `router-1(config)#track 1 interface fastethernet 0/21 line-protocol`
- `router-1(config-if)#standby group track <object#> [decrement value]`

## Verify HSRP 

- `show standby`
- `show standby brief`

