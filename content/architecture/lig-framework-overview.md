---
title: "Ligato Framework"
date: 2019-04-16T05:23:30-07:00
layout: "arch"
draft: false
---

### Requirements:

* Control and management of container-based VNFs (CNFs)

* Re(usability) to the greatest extent possible for code, functions, plugins (components), APIs, development patterns, development patterns

* Architecturally layered

* VPP and Linux Kernel dataplane support

* Modularized so developers can use(re-use) software plugins (like Go packages) 

* Developer - friendly with documentation, tutorials, examples and libraries

* Programming language developed for speed, efficiency, ease of use and broad community support with an extensive suite of libraries

* Agnostic to control/management platforms including K8s

* Implemented in running code. No better way to prove feasibility than in real networks.

<br />

{{< figure src="/images/ligato/ligato-framework-arch.svg" class="image-center figcaption"caption="Ligato Framework" >}}

<br />

### Solution: Ligato

Ligato is an open source framework for developing solutions for managing and controlling CNFs or really any kind of microservice-based application. Reusable software components (dubbed plugins) with the accompanying documentation, tutorials and examples are supplied to facilitate development. A set of VPP-specific plugins including a configuration scheduler are packaged into a VPP agent enabling separate microservices, other plugins and/or processes and applications to manage and configure VPP-based CNFs. Ligato employs Golang as its programming language for the  performance, concurrency, scale and broad community support necessary for developing and implementing network functions inside containers. 

<br />

### Reference: [Ligato Overview](http://docs.ligato.io/en/latest/intro/overview/)


