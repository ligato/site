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

* Already implemented in multiple solutions and frameworks (see below). By definition proves code reusability and operational deployment feasibility

* Composed of the Ligato VPP Agent plus the VPP dataplane. 

* Inherits all of the dataplane features and services from FD.io/VPP. This includes your standard L2/L3 forwarding, as well as more application-specific processing including IPsec tunnel termination, firewall policies and K8s services and policies. 

{{< figure src="/images/ligato/ligato-cnf-evolve.svg" >}}

Figure above illustrates the origins starting with separate open source components. At the "midpoint" the two open source pieces, the Ligato VPP Agent and the VPP dataplane are bolted together forming the programmable VPP vSwitch. The targets are solutions where this vSwitch forms THE core component. 

[Contiv VPP](https://contivpp.io) and the [Network Service Mesh](https://networkservicemesh.io), both explained below make use of the programmable VPP vSwitch. 

Other solutions, projects and ventures either working on, shipping or considering a container-based vSwitch include [hICN](https://fd.io/2019/02/introducing-hybrid-information-centric-networking-hicn-a-new-fd-io-project/), [ONAP](https://www.onap.org), [Service Function Chaining](https://github.com/ligato/sfc-controller), [StrongSwan-based IPsec VPN Gateway](https://fosdem.org/2019/schedule/event/vpp_ligato_as_ipsec_gateway/) as well as cable, mobility and service mesh edge proxies. 

[CNF Testbed](https://github.com/cncf/cnf-testbed) is using the VPP vSwitch in their evaluation. Early assesments of [VPP-based network functions](https://datatracker.ietf.org/doc/draft-mkonstan-nf-service-density/) look quite promising. 

A blog on this topic is available [here](/blog/cnf-ligato-fdio).
<br />
<br />
<br />
<br />
## 2. Contiv VPP

From their inception, Kubernetes Container Network Interfaces (CNI) define an API for managing connectivity between application pods in a K8s cluster. [CNI plugins](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/) come in many flavors with each focused on a particular aspect of cluster networking that at the end of the day accomplish the same general task: networking between pods on different hosts. The operative word is _networking_.

### Requirements

With the application pod networking problem solved, we need to turn our attention to K8s policies and services. The former defines which pods can talk to each other; the latter defines pods acting as service endpoints. Policies and services must be programmed into the network and meanwhile at the speed of the pod placement-create-destroy "work flow" orchestrated by K8s.

 Details on the overall problem statement (i.e. requirements) are discussed [here](https://contivpp.io/docs/concepts/what-is-contiv-vpp/). For convenience sake, several are reiterated here:
 
* Policies and Services are `first-class citizens` in cluster networking which translates to k8s policy and service awareness at the network layer. 

* Current and future feature/function support with performance.

* Classic (i.e. tap, veth) and high-performance, inter-VPP memif interface support 

* Fast Configuration Programmability - __Important__ Containers are added and removed frequently. They _must_ (re)start within a second and there could be multiple running instances on single host. They may even move to other hosts. Therefore it is crucial that there be minimal delay to programming in the correct configuration into the dataplane. 

### Solution - Contiv VPP

[Contiv-VPP](https://contivpp.io) is a Kubernetes CNI plugin (not to be confused with Ligato plugins) that exploits VPP as the forwarding plane for optimal performance and K8s policy and service handling. 


<br />
<br />  
{{< figure src="/images/ligato/ligato-contiv-vswitch2.svg" class="image-center figcaption" caption="Contiv-VPP and their contiv-vSwitches inside a K8s cluster" >}}
<br />
<br />
The figure above presents an abstracted view of Contiv-VPP deployed in a K8s cluster. The unique aspects of this solution are:

* __k8s Policy and Service Mapping to FD.io/VPP Configuration.__ This is an automated distribution pipeline where k8s policies and services are automatically reflected into FD.io/VPP configuration information which is then programmed into the network.    

* __contiv-vswitch__ This is the CNF composed of the FD.io/VPP dataplane and a Ligato-based VPP agent. It is across APIs enabled by the VPP agent/plugins that configurations rendered from policy and services are passed into the FD.io/VPP dataplane. The contiv-vswitch runs completely in userspace and uses [DPDK](https://dpdk.org/) for direct access to the network I/O layer. 

Reference: [Contiv-VPP](https://contivpp.io)

<br />
<br />
## 3. Network Service Mesh

The Network Service Mesh (NSM) is novel approach that manages L2/L3 "pipes" (another term is "wires) between client Pods that require a service and server pods that host the service. It is separate from CNI plugins but can co-exist in a K8s cluster network with any CNI plugin.

NSM addresses the problem of "too much network configuration complexity" for developers and operators who merely need to connect client pods (network service clients or NSCs) to service Pods (network service endpoints or NSEs).

An Ligato/VPP-based CNF is one implementation supporting the x-connect of local and remote interfaces (e.g. tap, memif, SRv6, vxlan tunnels, etc.) that when strung together form the aforementioned client to service pod wires.

Reference: [Network Service Mesh](https://networkservicemesh.io/)   



