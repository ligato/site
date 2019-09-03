---
title: "Definition of a Cloud Native Network Function"
date: 2019-04-23T14:25:06-07:00
layout: "cnf"
draft: false
---

So what is a *cloud native* virtual network function?

 

A virtual network function (or VNF), as commonly known today, is a software
implementation of a network function that runs on one or more *virtual 
machines* (VMs) on top of the hardware networking infrastructure — routers,
switches, etc. Individual virtual network functions can be connected or
combined together as building blocks to offer a full-scale networking 
communication service. A VNF may be implemented as standalone entity using
existing networking and orchestration paradigms - for example being 
managed through CLI, SNMP or Netconf. Alternatively, an VNF may be a part
of an SDN/NFV architecture, where the control plane resides in an SDN 
controller and the data plane is implemented in the VNF.



A *cloud-native VNF* is a VNF referred to as a *cloud-network network function (CNF)* and designed for the emerging cloud environment.
It resides in a container rather than a VM, its lifecycle is managed  
by a container orchestration system, such as Kubernetes, and it subscribes to
cloud-native orchestration paradigms. In other words, its control/management
plane looks just like any other container based [12-factor app](https://12factor.net). To 
an orchestrator or external clients it exposes REST or gRPC APIs, watches for changes stored
in a centralized KV data store, communicates over a message bus, employs cloud-friendly
logging and config methods and conforms to cloud friendly devops build & deployment processes.
Depending on the desired functionality, scale and performance, a CNF may come packaged with a high-performance data plane, such as [VPP](https://fd.io).
<br />
<br />
<br />

{{< figure src="/images/ligato/cnf-net2.svg" class="image-center figcaption" caption="Examples of CNFs in a K8s cluster">}}

<br />
The figure above can help explain why CNFs are attractive to developers, cloud operators (public, private or hybrid) and customers - really anybody who will run apps inside K8s clusters. It abstracts a K8s cluster composed of a master and set of worker nodes. The latter contains pods housing containerized CNFs including security gateways, switching/routing functions and a L7 service proxy operating as a sidecar component in a service mesh. 

There are a raft of advantages so let's run through them in no particular order:

* __Lifecycle management parity with application containers.__ All of the love shown to application microservice containers related to development, CI/CD, K8s orchestration and scheduling, distributed management, telemetry collection and so on can now be shared with CNFs.

* __Immutability.__ CNFs are not fiddled with while in service. If something is broke or new functions needed, then tear down the old one and deploy a new one. Note that this behavior conveys immutability upon the linux kernel.

* __Smaller footprint vis-a-vis physical or virtual devices.__ This reduces he VNF resource consumption region and frees up more resources for the things people care about: apps.

* __Rapid development and innovation delivery.__ This is especially true if the CNF functions run in user space because there is no need to modify the linux kernel.

* __Performance.__ Software technologies evolve and a scale/performance ramp up is expected. Of course there are multiple factors that impact performance. So rather than indulge in outlandish, evidence-dodging assertions, let's keep our eye on the informal [CNF testbed](https://github.com/cncf/cnf-testbed) undertaken by the CNCF as well as new published findings soon to emerge.

And finally, the __versatility.__ Note in the figure above the existence of three different types of CNFs. All running on the same hardware, orchestrated from the same master node components (i.e. K8s) and wired up to realize a communication service. This will be a recurring theme throughout the discussion of and development with Ligato.

