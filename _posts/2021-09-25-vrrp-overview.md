---
layout: post
title: Virtual Routing Redundancy 
tags: Virtual Routing Redundancy
categories: vrrp
---

>> Ahmet Numan Aytemiz

#### VRRP 

- Open Standart (rfc 5798)
- Built in transport protoco:112 (Use its own protocol)
- Multicast address 224.0.0.18
- Master router replies to ARP request for virtual ip address
- Preemption enabled by default
  - Default priority = 100
- Higher ip address will become master  
- Timers
  - Advertisement interval = 1 seconds
  - Master down interval = 3.6 secondas
- MAC address : 0000.5e00.01xx
  - xx refers to the VRRP group number in hexadecimal
- No load-sharing feature
- Different instance of VRRP can provide load-sharing  

#### Impelenting VRRP

- Enabling VRRP in the interface
  - `sw1(config-if)#vrrp <group-id> ip virtual ip`

- Configuring priority
  - `sw1(config-if)#vrrp <group-id> priority <priority>`

#### VRRP Authentication

- Authentcaiton supported
  - plaint-text
  - md5

- Plain-text configuration
  - `sw1(config-if)#vrrp <group-id> authentication <password>`

- MD5 configuration
  - Key String method
    - `sw1(config-ig)#vrrp <group-id> authentication md5 key-string <password>`
  - Key-chain method
    - `sw1(config-if)#vrrp <group-id> authentication md5 key-chanin <key-chanin name>`

#### Optimizing VRRP Timers

- All routers in VRRP group must share the same Hello timers
- Configuring advertise timer
  - `sw1(config-if)# vrrp <group-id> timers advertise msec <value>`

#### Optimizing VRRP Timers

- When increasing VRRP hello timer, all other routers must "learn" the new timer (not default behavior)
  - `sw1(config-if)#vrrp <group-id> timers learn`

#### VRRP Verfication

- `show vrrp`
- `show vrrp brief`
