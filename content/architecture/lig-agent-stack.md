---
title: "Ligato Framework"
date: 2019-04-16T05:23:30-07:00
layout: "arch"
sidebar: "true"
sidebarlogo: "ligato-diamond"
draft: false
---

### Requirements:

* Control and management of container-based VNFs (CNFs)

* Customizable

* Architecturally layered

* VPP and Linux Kernel dataplane support

* Incorporates software plugins for RPC, KV Datastores, Messaging and Telemetry

* Developer - friendly with documentation, tutorials, examples and libraries

* Programming language developed for speed, efficiency, broad community support with an extensive suite of libraries

* Agnostic to control/management platforms

* Implemented in running code
<br />
<br />
<br />

{{< figure src="/images/ligato/ligato-framework-arch.svg" >}}

<br />
<br />
<br />

### Solution - Ligato Framework

* framework for controlling and managing CNFs

* Layered approach with southbound plugins for handling VPP and Linux Kernel dataplane configuration, KV scheduler for coordinating/tracking configuration changes, northbound plugins for interacting with other external components, and a common infra for lifecycle management and logging

* Written Golang
<br />
<br />
<br />
### Reference: [Ligato Overview](http://docs.ligato.io/en/latest/intro/overview/)


