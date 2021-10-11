---
layout: post
title: Bgp Update Source and Multi-Hop Lab
tags: Bgp Update Source and Multi-Hop Lab
categories: bgp
---

> Ahmet Numan Aytemiz , 11.10.2021

---

![Image](/img/ebgp_multihop.PNG)

**According to topology form ebgp neighborships between Router1 and Router2 using both routers loopback ip address**

---

#### R1 Configuration

```
router1(config)#interface ethernet 0/1
router1(config-if)#no shutdown
router1(config-if)#no switchport
router1(config-if)#ip address 1.1.1.1 255.255.255.252
``` 

```
router1(config)#interface ethernet 0/0
router1(config-if)#no switchport
router1(config-if)#ip address 2.2.2.1 255.255.255.252
router1(config-if)#no shutdown

```

```
router1(config)#interface loopback 0
router1(config-if)#ip address 11.11.11.11 255.255.255.255
```

```
router1(config)#ip route 22.22.22.22 255.255.255.255 1.1.1.2
router1(config)#ip route 22.22.22.22 255.255.255.255 2.2.2.2 20
```

```
router1(config)#router bgp 100
router1(config-router)#neighbor 22.22.22.22 remote-as 200
router1(config-router)#neighbor 22.22.22.22 update-source loopback 0
router1(config-router)#neighbor 22.22.22.22 ebgp-multihop
```

#### R2 Configuration

```
router2(config)#interface ethernet 0/1
router2(config-if)#no switchport
router2(config-if)#no shutdown
router2(config-if)#ip address 1.1.1.2 255.255.255.252
```

```
router2(config)#interface ethernet 0/0
router2(config-if)#no switchport
router2(config-if)#no shutdown
router2(config-if)#ip address 2.2.2.2 255.255.255.252
```

```
router2(config)#interface loopback 0
router2(config-if)#ip address 22.22.22.22 255.255.255.255
```

```
router2(config)#ip route 11.11.11.11 255.255.255.255 1.1.1.1
router2(config)#ip route 11.11.11.11 255.255.255.255 2.2.2.1 20
```

```
router2(config)#router bgp 200
router2(config-router)#neighbor 11.11.11.11 remote-as 100
router2(config-router)#neighbor 11.11.11.11 update-source loopback 0
router2(config-router)#neighbor 11.11.11.11 ebgp-multihop
```




