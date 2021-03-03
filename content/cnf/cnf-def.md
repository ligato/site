---
title: "What is a CNF?"
date: 2019-04-23T14:25:06-07:00
layout: "cnf"
draft: false
---

This section defines a CNF, provides background context, discusses different scenarios, and lays out a composite architecture.  

---

## Physical Network Function (PNF)

Physical network functions (PNF) are physical devices that process packets supporting a network and/or application service.  A PNF comprises tightly-coupled, purpose-built hardware and software functions aligned with their station in the network. A sampling of PNFs deployed in your network might include CPE routers, VPN gateways and firewall appliances.

A set interconnected PNFs forms a network. You assemble interconnected networks into larger networks characterized by a broad range of applications and services, diverse customer profiles, and heterogeneous connectivity. 

---

## Virtual Network Function (VNF)

A virtual network function (VNF) is a network function that runs in a virtual environment. By convention, you run VNFs in virtual machines (VMs) managed by operators using industry best practices and VM orchestration tools. VM-based VNFs operate on COTS servers and exist in an "enclosed space" surrounded by a guest OS/kernel, hypervisor, host OS/kernel, and network I/O. Excluding performance and scale, VNFs mirror many of the functions and features you find in PNFs, modulo the requisite adaptations for operating in virtual environments. 


{{< figure src="/images/ligato/pnf-vnf.svg" class="image-center figcaption" caption="PNF appliances transition to software VNFs running on COTS hardware" >}}

---

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
VNF advantages:
</p>
</div>

- Decouples software NFs from purpose-built hardware resulting in potential capex and opex savings.
- Automated VNF "pre-staging" handles projected increased traffic workloads driven by customer growth and/or new services.
- Service Chaining allows operators to build and deploy customer-facing services from a set of interconnected VNFs.  
- Well understood performance and scale numbers. Proper capacity planning should not yield any surprises. 
- Network architecture and operations affinity with physical networks. If you have worked with physical networks, you will not encounter a "sea change" of differences with VNFs.    

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
VNF challenges:
</p>
</div>

- Performance impact due to VM software overhead. 
- Availability impact due to long boot times. 
- Multi-VNF scale out requires investment in servers and PNF gear. 
- Idle VM resources. VNFs deploy in coarse VM increments. Optimizing VNF resource allocations to meet traffic workload demands is not precise. You must over-provision and then resources sit idle. Lack of optimal VNF traffic engineering capabilities limits assignment of VM resources to cloud-ready applications and server expansion.   
- Implemented as code monoliths. All development, testing, maintenance, deployment, and troubleshooting must consider the VNF as a single atomic unit.
- Kernel network feature dependencies. Your VNF implementation might rely on customized kernel modifications, or "hacks", to perform the desired function. This adds complexity.  

VNFs are the current defacto standard for standing up and running network functions in cloud environments.   

---

## Cloud Native

[Cloud Native](https://www.cncf.io/) defines a new approach for developing and running applications in virtual cloud environments. Cloud native principles consist of the following:

- __Applications are "carved up" into smaller units called microservices__. A constellation of smaller, inter-connected microservices replaces application monoliths.
- __Containers house the microservices and provide a run-time environment__. Containers dispense with VM overhead, and instead, package up the application code, binaries and dependencies. Containers share a guest and/or host OS/kernel. 
- __Kubernetes orchestration provides complete container lifecycle management__ including scheduling, placement, start/stop/re-start, and visibility.  

Kubernetes pods run on physical or virtual hosts, and contain one or more containers. A cluster defines a collection of hosts and pods managed by Kubernetes.

Cloud native embraces the notions of open source, [12-factor app](https://12factor.net/), and devops CI/CD pipelines.      

A Cloud Native Compute Foundation (CNCF) [definition](https://github.com/cncf/toc/blob/main/DEFINITION.md) summarizes these principles:

``` 
“Cloud native technologies empower organizations to build and run scalable applications in 
modern, dynamic environments such as public, private, and hybrid clouds. 
Containers, service meshes, microservices, immutable infrastructure, 
and declarative APIs exemplify this approach.
  
These techniques enable loosely coupled systems that are resilient, manageable, 
and observable. Combined with robust automation, they allow engineers to make 
high-impact changes frequently and predictably with minimal toil.”
```

---

### Container Networking Requirements

One aspect of [K8s networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/) involves inter-pod connectivity defined by the Kubernetes [Container Network Interface (CNI) specification](https://github.com/containernetworking/cni/blob/master/SPEC.md). You have multiple CNI network plugins to choose from for establishing different forms of virtual L2/L3 overlay networks between pods. CNI plugins are quite suitable for your web traffic workloads. However, as cloud native applications and services evolve, you might find that CNI plugins do not meet your needs.  


<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
A new set of requirements emerge for container networking that leverage VNF capabilities:
</p>
</div>

- __(NFs) adapted for cloud native deployment and operations__. This assumes a container (pod) form-factor under K8s control, or a control/management plane co-existing or compatible with K8s.  

    In addition, you should align CNF architecture, development, and operations with 12-factor app precepts, and where appropriate, [X-factor CNF](https://x.cnf.dev/).     
- __Lightweight data plane configuration__. To date, SDN-like centralized management systems communicating through a control channel to a configuration agent, handle configuration.   [Netconf](https://tools.ietf.org/html/rfc6241), [RESTCONF](https://tools.ietf.org/html/rfc8040) and [VPP Honeycomb](https://wiki.fd.io/view/Honeycomb) come to mind. However, the dynamics and velocity of container start/stop activity preclude centralized control. Cloud native must employ stateless and lightweight data plane configuration.     
- __Feature-rich data plane__. Traffic workload growth coupled with per-service processing options such as ACL or encrypt/decrypt necessitate a robust, performant, and multi-feature software data plane. 
- __User space networking__. Performance gains with user space packet processing vs kernel networking are well [documented](https://ieeexplore.ieee.org/document/8398225). User space permits network function processes to talk directly to physical NIC components, while bypassing the kernel altogether. You can innovate free of kernel dependencies. You can start/stop NF processes without fear of disrupting kernel functions.
- __Topology "Meshiness"__. A service mesh describes a mesh of L7 inter-container RPCs that constitute a cloud native application or service. You can expand your service portfolio with a full or partial mesh of inter-CNF L2/L3 connections. This allows you to set up service chains, or provision point-to-point "wires" between pods.   
- __Observability__. Logging, tracing, UI/UX visualization, telemetry, metrics, diagnostic/troubleshooting APIs, and CLI are _mandatory_ for your development, testing, troubleshooting, and operations tasks.
- __Common APIs__. Well documented declarative RPCs (REST, gRPC) expedite development, and integration with external systems. Low-level APIs add speed and accuracy to data plane programming.     

---
## Cloud Native Network Functions (CNF)

A [cloud native network function (CNF)](https://ligato.io/blog/cnf-ligato-fdio/) is a network function designed and implemented to run inside containers. CNFs inherit all cloud native architectural and operational principles including K8s lifecycle management, agility, resilience, and observability. Your CNF development and deployments should strive to meet the requirements outlined above.

{{< figure src="/images/ligato/pnf-vnf-cnf-arch.svg" class="image-center figcaption" caption="VM-based VNFs transition to Container-based Network Functions known as CNFs" >}}



CNFs place your physical (PNF) and virtual network functions (VNF) inside containers. You receive many of the VNF advantages discussed earlier. In addition, you are no longer burdened with VM software overhead. Containers do not require a guest OS or hypervisor, and you can quickly spin up/spin down container CNFs as needed. 

---


{{< figure src="/images/ligato/cnf-net5.svg" class="image-center figcaption" caption="CNFs run on pods in a cluster managed by Kubernetes" >}}

<br>
</br>

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
Kubernetes and CNFs:
</p>
</div>

- __K8s master and worker deployment.__ You can think of a CNF as just another pod, albeit with specialized network functionality.
- __Versatility__. You can implement a CNF with a single purpose in mind, with an example being a load balancer. Or you can design and build a [CNF for programming and executing specific data plane functions](https://cdnf.io/). Consider a CNF platform programmed through APIs/agent to perform L3 routing in one instantiation, and firewall/NAT functions in another, or combine the two in a service chain.  
- __Application pod network functions__. You do not have to confine a CNF to its own pod. For example, you can implement a [memif](https://docs.fd.io/vpp/17.10/libmemif_doc.html) interface in both CNF and application pods. You now have a point-to-point, user space communications channel offering kernel-free packet transmit and receive functions.

---
          
<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
CNF Advantages:
</p>
</div>


* __Lifecycle parity with application containers.__ CNFs receive, at no cost, all existing best practices bestowed to application pods. These include: development environments, toolchains, CI/CD, K8s orchestration, 12-factor app, distributed management, logging, and telemetry streaming. 
  

* __Stateless Configuration.__ CNFs operate in pods. Pods are durable, but if something breaks, or new functions are needed, then you can tear down pods and start a new instance. The dynamic nature of cloud native environments precludes centralized or stateful CNF configuration management.  Instead, a stateless approach is employed where a key-value data store pushes configuration updates to one or more CNFs. Another lightweight approach uses a service request to trigger an RPC call that pushes configuration state into the CNF data plane. 

* __Smaller footprint vis-a-vis physical or virtual devices.__ This reduces resource consumption, with potential savings re-allocated to applications and/or infrastructure expansion. 

* __Rapid development, innovation and [immutability](https://docs.google.com/document/d/1fwV3vZB5IgHb6uUgQpHts9mFR0HheYbIrlg9hGfWYfA/edit).__ You should develop and run CNF functions in user space. You benefit from better performance, faster development and testing cycles, container and kernel immutability, and reduced risk when deploying new features and innovations. You do not need to interact with the stable and mature Linux kernel used by other systems in your environment.

* __Performance.__ CNF processing in user space, NIC hardware control, multi-core tuning and kernel bypass all contribute to maximum throughput and resource efficiency. Characterizing CNF performance is underway in the [CNF Testbed](https://github.com/cncf/cnf-testbed) project. 

* __Agnostic to host environments__. [Bare-metal](https://en.wikipedia.org/wiki/Bare-metal_server) and VMs are the most common. Since VMs do not add value to CNFs, you might go with bare-metal as the preferred host of choice. Bare-metal servers offer 100% resource allocation to your CNF (and application pods), predictable performance, security, and isolation. 

---

## CNF Solution Scenarios

CNFs are rolled out with functions suited for their role in a cloud native network. For illustrative purposes, the figure below depicts several scenarios. 
<br>
</br>

<div class="tile is-ancestor">
    <div class="tile is-parent">
        <article class="tile is-child box">
            <p class="title">Load Distribution</p>
            <p class="subtitle">Ingress Controller/Load Balancer</p>
            <figure class="image is-4by3">
                <img src="/images/ligato/cnf-LB-example.svg">
            </figure>
        </article>
    </div>
    <div class="tile is-parent">
        <article class="tile is-child box">
            <p class="title">Cluster Networking</p>
            <p class="subtitle">Improved CNI / NSM vWires</p>
            <figure class="image is-4by3">
                <img src="/images/ligato/cnf-pod-network-example.png">
            </figure>
        </article>
    </div>
    <div class="tile is-parent">
        <article class="tile is-child box">
            <p class="title">Service Bundles</p>
            <p class="subtitle">by wiring up CNFs</p>
            <figure class="image is-4by3">
                <img src="/images/ligato/cnf-sfc-example.svg">
            </figure>
        </article>
    </div>
</div>
<br>
</br>
<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
Load Distribution
</p>
</div>

An inherent property of cloud native networking is service scalability and resiliency. In this scenario, a CNF acts as a load balancer directing traffic to the back-end pods of a K8s service, or as a policy-based forwarder steering traffic to a specific application pod. While you can perform these functions with kube-proxy, a CNF might be a better fit playing the role of ingress or loadbalancer for multiple reasons:

- Maximum throughput and scale with user space packet processing.
- Flexible load distribution strategies based on round-robin, multipath, server load and policy as examples.
- Observability and visibility into this discrete network function vital to K8s services access, scale and availability.    

---

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
Enhanced Cluster Networking
</p>
</div>

New CNF implementations improve upon existing solutions, and enable new network services. The top portion of the figure depicts a CNI-bootstrapped L2 bridge domain for inter-pod connectivity. A CNF vSwitch performs two key functions: 

- Multi-interface support for attaching multiple application pods to the bridge domain. 
- Mapping K8s service/policy traffic to a fast data plane. For example, you can program QoS, NAT, and ACLs into the CNF vSwitch data plane based on K8s services or policies. This enhances throughput and provides flow visibility for service/policy traffic. The contiv-vswitch in [Contiv-VPP](https://contivpp.io/) performs this function.  

The bottom portion of the figure depicts a CNF serving as a [Network Service Mesh (NSM)](https://networkservicemesh.io/) x-connect. This provides a logical point-to-point "vWire" connecting an NSM client pod to an NSM server pod. NSM binds interface mechanisms and payload types to construct the set of contiguous x-connected segments that form a vWire.  

Two additional items:

- Enhanced CNI relies on existing K8s CNI orchestration methods augmented with [multi-interface pod customizations](https://github.com/contiv/vpp/blob/master/docs/operation/CUSTOM_POD_INTERFACES.md), and programming of K8s services/policies into a low-level data plane configuration functions.
- NSM uses its own Network Service Mesh Control Plane (NSMC) to program x-connects for vWire setup.
- Independent of control plane, both solutions use the same programmable data plane exposed by common low-level APIs. 
- Solutions are not mutually exclusive. Both can co-exist in a cluster network.

---

<div>
<p style="text-decoration: underline;font-weight: 600">Service Bundles</p>
</div>

You can arrange CNFs into a service function chain (SFC) of contiguous network functions supporting an application or network service offering. NSMC, a customized SFC control plane, or CLI can handle SFC provisioning.   

The top figure shows cloud storage service SFC; The bottom figure shows an ML training app SFC.

You can employ CNF functions, such as memif interfaces, on the terminating cloud storage and ML training app pods.  

---

## CNF Architecture

You make design choices tailored for specific implementations. Simplifying those options to meet your expectations requires an architectural approach focused on the following:
 
- Targets on performance, flexibility, programmability, observability, and scale.   
- Adheres to cloud native principles.
- Embraces "evolvability" as cloud native matures and new services emerge.

With these in mind, it becomes possible to paint a picture of a composite CNF architecture. This architecture model is inspired by, and reflects an architecture, defined by the [Ligato framework]( https://docs.ligato.io/en/latest/).

`Note: To simplify the picture, the pod, host, kernels, NIC and physical underlay components have been omitted.` 

{{< figure src="/images/ligato/cnf-generic-arch2.svg" class="image-center figcaption" >}}

<br>
</br>
The architecture incorporates a layered approach: Applications, orchestration, management plane on top; software data plane on the bottom. The term, `plugin`, is a placeholder for integrated and/or configured software components supporting a function or functions performed by the CNF. 
 
A discussion follows beginning with the data plane.


<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">Data Plane
</p>
</div>

* Supports a user space data plane offering maximum throughput and kernel bypass. [FD.io](https://fd.io/) in one such data plane to consider. This open source project enjoys a large and active open source community, 10+ years of installed product implementations, and comes with a rich feature set.  
* Linux kernel. Of course, you must contend with the throughput ceiling and feature upgrade lag time. However, it is, by far, the most common data plane in virtual cloud environments. You can build CNFs that use the kernel network stack. Work on kernel extensions such as [eBPF](https://opensource.com/article/17/9/intro-ebpf) improve performance.

---

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">Control Plane
Agent</p>
</div>

* Exposes northbound APIs to: Internal applications, processes, plugins; External applications, KV data stores, orchestrators and management plane functions.
* Implements a low-level southbound API for programming data plane functions.
* Includes agent plugins to enable one or more CNF functions. You can implement existing plugins. Or build your own to support new data plane features, or interactions with external systems. 

---

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">Custom Applications / Plugins
</p>
</div>

* Enables customized applications and/or plugins to add solution-specific functions to your CNF. As an example, an IPsec VPN CNF could integrate a control plane for IKEv2 SA setup.
* CNFs dedicated to network control plane functions are possible. Consider a specialized BGP-only pod configured with eBGP Multihop to exchange prefixes with an external routing domain. 
* Architecture and implementation of a distributed CNF solution may call for the control plane and data plane functions to run in separate pods. 

---

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">External Applications/Orchestration/CNF Management Plane
</p>
</div>

* Cloud native is a dynamic and evolving environment. New features, open source projects, new applications, architectures, and tools are invented all of the time. You need only examine the [CNCF landscape](https://landscape.cncf.io/) to appreciate the breadth of this work.
* External components and projects supporting CNF development, implementation, and deployments, are candidates for inclusion in current and evolving CNF endeavors.  
 
--- 
 
In any domain, the network plays a crucial role, The momentum behind cloud native, the architecture, applications, and the greater ecosystem has, perhaps inadvertently, pushed networking into the background.  

The advent of CNFs serves as a vehicle to establish the network as playing a prominent and vital role in the success of cloud native. 




