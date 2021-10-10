---
layout: post
title: Overview of iBGP and eBGP
tags: Overview of iBGP and eBGP
categories: bgp
---

> Ahmet Numan Aytemiz , 10.10.2021

---

- There are two types of neighbours in BGP:
  - **internal BGP (İbgp)**
  - **external BGP (eBGP)**
- When a BGP neighbourship is formed between routers that are in the same AS, they are iBGP neighbours
- When a BGP neighbourship is formed between routers that are in the diffrent AS, ther are eBGP neighbours.
- A BGP routers behaves differently in several ways depending on whether the peer (neighbour) is an iBGP or eBGP
- The neighbourship requirements are different for routers to become iBGP neighbours than become eBGP neighbours.
- There are diffrent rules about which routes the BGP best-path algorithm choose as best.
- The other major differences is how the routers update the BGP AS_PATH PA when advertising BGP routes to iBGP neighbour compared with the eBGP neighbor.
- When routers are advertised to an eBGP peer, a BGP router updates the AS_PATH PA , but it does not to do so when advertising to an iBGP peer.

![Image](/img/aspath.PNG)

- There are two kinds of AS numbers : **public and private**
  - **Public AS numbers can be advertised over the internet**
  - **Private AS numbers should not be advertised over the internet**
- The range for public and private AS numbers is : 
  - **Public AS numbers 1 - 64495**
  - **Private AS numbers 65512 - 65534**
- The following numbers and ranges are reserved :
  - 0, 64496 - 65511, 65535  

![Image](/img/nobgp.PNG)

![Image](/img/privateas.PNG)

![Image](/img/pubicas.PNG)





  







