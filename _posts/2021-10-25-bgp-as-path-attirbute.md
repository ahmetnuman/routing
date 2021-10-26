---
layout: post
title: Bgp As Path Attribute
tags: Bgp As Path Attribute
categories: bgp
---

> Ahmet Numan Aytemiz , 21.10.2021

#### **BGP AS_SEQ Attribute**

- BGP uses multiple path attribute to determine the best path for a certain prefix.
- By default, if no BGP PAs have been expilicitly set, BGP use the BGP AS_PATH (autonomous system path) PA when choosing the bes route among competing routes.
- AS_SEQ is the component of the AS_PATH 
- Each company is usually represented by a unique Autonomous System (AS) number.
- When a router uses BGP to advertise a route, the prefix/length is associated with a set of PAs, including the AS_Path.
- The AS_Path PA associated with a prefix/length lists the ASNs that would be part of an end-to-end path for that prefix was learned using BGP.
- The list of AS numbers associated with a particular prefix/length means that to get to that destination, a packet would go through those autonomus systems.
- BGP uses AS_Path to perform two functions:
  - Choose the best route for a prefix based on the shortest AS_Path (fewest number of ASNs listed)
  - Prevent routing loops.
- When a route is advertised by BGP, each time that advertisement goes through a different AS, that number is prepended in the AS_PATH attribute.
- When a BGP router receives an update and a route advertisement lists an AS_Path with its own ASN, the router ignores that route.