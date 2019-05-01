---
title: "Ligato Use Cases"
date: 2019-04-23T14:54:27-07:00
layout: "use-cases"
draft: false
---


## 1. MultiSolution Diversity

### Requirements

* Container-based high-Performance, scalable, FeatureHeavy, lightweight software component

* based on open source

* easily programmable from multiple control/management entities (e.g. K8s) 

* maximum code reusability

* function plugin options to building and applying solution specificity

* Stats and Telemetry steaming and export

### Solution: Programmable VPP vSwitch

* Programmable VPP vSwitch 

* Already implemented in multiple solutions and frameworks (see below). By definition proves code reusability

* Composed of the Ligato Agent plus the VPP dataplane 

* Inherits all of the dataplane features and services from FD.io/VPP. 

A decent blog on this topic is available [here](/blog/cnf-ligato-fdio).

{{< figure src="/images/ligato/ligato-cnf-evolve.svg" >}}

Figure above illustrates the origins starting with separate open source components. At the midpoint the two open source pieces, the Ligato Agent and the VPP dataplane are bolted together forming the programmable VPP vSwitch. The targets are solutions where this vSwitch forms THE core component. 

[Contive VPP](https://contivpp.io) and the [Network Service Mesh](https://networkservicemesh.io), both explained below make use of the programmable VPP vSwitch. 

Other solutions, projects and ventures either working on, shipping or considering a container-based vSwitch include [hICN](https://fd.io/2019/02/introducing-hybrid-information-centric-networking-hicn-a-new-fd-io-project/), [ONAP](https://www.onap.org), [Service Function Chaining](https://github.com/ligato/sfc-controller), [StrongSwan-based IPsec VPN Gateway](https://fosdem.org/2019/schedule/event/vpp_ligato_as_ipsec_gateway/) as well as cable, mobility and service mesh edge proxies. 

[CNF Testbed](https://github.com/cncf/cnf-testbed) is using the VPP vSwitch in their evaluation. Early assesments of [VPP-based network functions](https://datatracker.ietf.org/doc/draft-mkonstan-nf-service-density/) look quite promising. 
<br />
<br />
<br />
<br />
## 2. Contiv VPP

### Requirements

to be added

### Solution - Contiv VPP

to be added
  
{{< figure src="/images/ligato/ligato-contiv-vswitch.svg" caption="Ligato Agent + VPP = contiv-vswitch" >}}

some more comments ...
<br />
<br />
<br />
<br />
## 3. Network Service Mesh

### Requirements

to be added ..

### Solution - Network Service Mesh

to be added

`add picture`

some more comments
<br />
<br />
<br />
<br />
## 4. IPsec VPN Gateway

### Requirements

to be added ..

### Solution - StrongSwan IPsec Gateway

to be added

`add picture`

some more comments
<br />
<br />
<br />
<br />
## 5. vCMTS

### Requirements

to be added ..

### Solution - StrongSwan IPsec Gateway

to be added

`add picture`

some more comments
<br />
<br />
<br />
<br />
## 6. Mobile Cloud

### Requirements

to be added ..

### Solution - StrongSwan IPsec Gateway

to be added

`add picture`

some more comments


