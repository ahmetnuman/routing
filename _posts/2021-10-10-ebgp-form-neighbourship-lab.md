---
layout: post
title: eBGP form Neighbourship Lab
tags: eBGP form Neighbourship Lab
categories: bgp
---

> Ahmet Numan Aytemiz , 10.10.2021

---

![Image](/img/ebgp_lab1.PNG)

#### Tasks

- On the Router1
  - Configure interface e0/0 ip address 12.12.12.1/24 and enable this interface
  - Configure bgp as number : 1
  - Configure neighbourship with the 12.12.12.2 ip address , remote-as 2
  - Configure bgp authentication : password cisco

- On the Router2
  - Configure interface e0/0 ip address 12.12.12.2/24 and enable this interface
  - Configure bgp as number : 2
  - Configure neighbourship with the 12.12.12.1 ip address , remote-as 1
  - Configure bgp authentication : password cisco

- Verify the status of the bgp neihbourship

---

## Solutions On the R1

```
router1(config)#interface ethernet 0/0
router1(config-if)#no shutdown
router1(config-if)#no switchport
router1(config-if)#ip address 12.12.12.1 255.255.255.0

```

```
router1(config)#router bgp 1
router1(config-router)#neighbor 12.12.12.2 remote-as 2
router1(config-router)#neighbor 12.12.12.2 password cisco
```

## Solutions On the R2

```
router2(config)#interface ethernet 0/0
router2(config-if)#no shutdown
router2(config-if)#no switchport
router2(config-if)#ip address 12.12.12.2 255.255.255.0

```

```
router2(config)#router bgp 2
router2(config-router)#neighbor 12.12.12.1 remote-as 1
router2(config-router)#neighbor 12.12.12.1 password cisco

```

## Verify BGP Neighbourship 

`show bgp neig`

![Image](/img/show1.PNG)

![Image](/img/show2.PNG)











