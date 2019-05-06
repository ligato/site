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
managed through CLI, SNMP or Netconf. Alternatively, an NFV may be a part
of an SDN architecture, where the control plane resides in an SDN 
controller and the data plane is implemented in the VNF.



A *cloud-native VNF* is a VNF designed for the emerging cloud environment -
it runs in a container rather than a VM, its lifecycle is orchestrated 
by a container orchestration system, such as Kubernetes, and it's using
cloud-native orchestration paradigms. In other words, its control/management
plane looks just like any other container based [12-factor app](https://12factor.net). to 
orchestrator or external clients it exposes REST or gRPC APIs, data stored
in centralized KV data stores, communicate over message bus, cloud-friendly
logging and config, cloud friendly build & deployment process, etc.,
Depending on the desired functionality, scale and performance, a cloud-
native VNF may provide a high-performance data plane, such as the [VPP](https://fd.io).
<br />
<br />
<br />

{{< figure src="/images/ligato/cnf-net2.svg" class="image-center figcaption" caption="Examples of CNFs in a K8s cluster">}}

<br />
The figure above can help explain why CNFs are so attractive to developers, cloud operators (public, private or hybrid) and customers or rather those that will run apps inside K8s clusters. It depicts a K8s cluster composed of a master and set of worker nodes. The latter contains pods housing containerized CNFs including security gateway, switching/routing and a L7 service proxy supporting a service mesh. 

There are a raft of advantages so let's run through them in no particular order:

* __Lifecycle management parity with application containers.__ All of the love shown to application microservice containers related to development, CI/CD, K8s orchestration and scheduling, distributed management, telemetry collection and so on can now be shared with CNFs.

* __Immutability.__ CNFs are not fiddled with while in service. If something is broke or new functions needed, then tear down the old one and deploy a new one.

* __Smaller footprint vis-a-vis physical or virtual devices.__ This reduces VNF density and frees up more resources for the things people care about: the apps.

* __Rapid development and innovation delivery.__ This is especially true if the CNF functions run in user space because there is no need to modify the linux kernel.

* __Performance.__ Software technologies evolve and a scale/performance ramp up is expected. Of course there are multiple factors that impact performance. So rather than indulge in outlandish, evidence-dodging assertions, let's keep our eye on the informal [CNF testbed](https://github.com/cncf/cnf-testbed) undertaken by the CNCF.

And finally, the __versatility.__ Note in the figure above the existence of three different types of CNFs. All running on same hardware, orchestrated from the same master node components (i.e. K8s) and wired up to realize a communication service. This will be a recurring theme throughout the discussion of and development with Ligato.

