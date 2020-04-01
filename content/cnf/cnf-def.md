---
title: "What is a CNF?"
date: 2019-04-23T14:25:06-07:00
layout: "cnf"
draft: false
---

This section defines a CNF, different scenarios where it applies and a composite architecture. Prerequisite context is provided upfront with a brief discussion of VNFs, cloud native, container networking and CNF requirements.   

## Virtualized Network Function (VNF)

Network functions (NF) are physical devices that process packets supporting a network and/or application service. Performance, features, scale and operational control are paramount. Routers, campus switches and firewalls are NF examples. A topology composed of similar or dissimilar interconnected NFs, form a network. Interconnected networks are assembled into larger networks characterized by a broad range of applications and services, diverse customer profiles, and heterogeneous connectivity. This network model applies in all cases, from the Internet all the way down to residential wifi networks, where physical network infrastructure is used.

A virtual machine (VM) is a software-only version of a physical server machine. It runs a complete instance of the application code, a guest OS/kernel, and hypervisor that coordinates VM resource management. A host OS/kernel is present, and collectively, all run on commodity-off-the-shelf (COTS) hardware. The advantages of VMs include: cost savings due to NF hardware decoupling; automated configuration and operations using mature VM management systems such as VMware and Openstack; multi-tenancy; security and isolation. The disadvantage is the overhead required to emulate, in software, all software and hardware functions, that normally run on physical servers. VMs operate in public or private clouds largely comprised of data center server farms.

Applications and NFs that run on dedicated physical servers are referred to as bare-metal. The advantages of bare-metal are single tenancy, security, isolation, and predictable performance. The disadvantages are slower deployment operations, kernel network bottlenecks, and additional hardware costs.

A virtualized network function (VNF) is an NF designed to run in a virtualized environment. By convention, VNFs run in VMs and accrue all aforementioned VM advantages and disadvantages. VNFs are typically networked together into a virtual overlay network running on top of a physical underlay network. Excluding performance and scale, VNFs mirror just about all of the functions and features found in dedicated hardware NFs, modulo the requisite adaptations for operating in cloud environments.

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
VNF advantages:
</p>
</div>

- VNFs are easily pre-staged to handle increased traffic workloads driven by customer growth and/or new services.
- Service Chaining. A uniform customer application or service is assembled from a set of interconnected VNFs. 
- Performance and scale numbers are well understood. There should be no surprises with proper capacity planning, a statement applicable to any network environment.
- Network architecture and operations affinity with physical networks. Those who have worked with physical networks will not encounter a "sea change" of differences with VNFs now included.    

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
VNF challenges:
</p>
</div>

- Performance impact due to VM software overhead including the guest OS/kernel, host OS/kernel and hypervisor. 
- Long boot times under normal maintenance, failure restart, and burst scenarios can impact availability. 
- VNF scale out requires investment in more servers and network gear such as routers, switches and load balancers.
- Potential for VM resources to sit idle. VNFs are deployed in coarse VM increments. Optimizing VNF resource allocations sufficient to meet traffic workload demands will not be precise. Over-provisioning is required and by definition, resources will sit idle. This is a classic traffic engineering problem with the exception here being those idle VM resources could be assigned to cloud-ready applications or server expansion.   
- Implemented as indivisible code monoliths.  This means all development, testing, maintenance, deployment and troubleshooting must consider the VNF as a single atomic unit.
- Kernel network feature dependencies. The VNF implementation could rely on customized kernel modifications or "hacks" to perform the desired function. This adds complexity because these hacks are now part of the code monolith.  

VNFs are the current defacto standard for standing up network functions in cloud environments.   

## Cloud Native

[Cloud Native](https://github.com/cncf/toc/blob/master/DEFINITION.md) is a new approach for developing and running applications in a virtualized cloud environment. The cornerstone principles of cloud native are:

- Applications are "carved up" into smaller units called microservices. Application monoliths are replaced by a constellation of smaller, inter-connected microservices.
- Containers house the microservices and provide a run-time environment. Containers dispense with VM overhead, and instead just package up the application code, binaries and dependencies. Containers share a guest and/or host OS/kernel. 
- Orchestration provides complete container lifecycle management including scheduling, placement, start/stop/re-start and visibility. [Kubernetes](https://kubernetes.io/) is the cloud native orchestration platform. 

    In the Kubernetes environment, one or more containers are grouped into pods that run on physical or virtual hosts. A pod is the smallest unit of work managed by Kubernetes. A collection of hosts and pods managed by Kubernetes is referred to as a cluster.

In addition, cloud native embraces the notions of open source, [12-factor app](https://12factor.net/), and devops CI/CD pipelines.      

These principles are encapsulated in the Cloud Native Compute Foundation (CNCF) definition:
 
`“using open source software stack to be containerized, where each part of the app is packaged in its own container, dynamically orchestrated so each part is actively scheduled and managed to optimize resource utilization, and microservices-oriented to increase the overall agility and maintainability of applications.”`

### Container Networking Requirements

One aspect of [K8s networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/) involves inter-pod connectivity provided by the Kubernetes [Container Network Interface (CNI) specification](https://github.com/containernetworking/cni/blob/master/SPEC.md). There are multiple CNI network plugins for establishing different forms of virtual L2/L3 overlay networks between pods. CNI solutions are quite suitable for web traffic workloads. However, they are not ideal for enabling services that leverage VNF-like functionality, process compute-intensive traffic workloads, or exploit the agility and dynamics of cloud native environments. 

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
A new set of requirements emerge for augmenting basic container networking with VNF-like capabilities:
</p>
</div>

- __(NFs) adapted for cloud native deployment and operations__. This assumes a container (pod) form-factor under K8s orchestration control, or a control/management plane co-existing or compatible with K8s. The latter is useful if pods need to be wired up into a mesh or chained topology. 

    In addition, CNF architects, developers and operators should align with 12-factor app precepts and where appropriate, [X-factor CNF](https://x.cnf.dev/).     
- __Lightweight data plane configuration__. NF and VNF configurations have traditionally been performed by human operators, or centralized management systems communicating through a control channel to a configuration agent. [Netconf](https://tools.ietf.org/html/rfc6241), [RESTCONF](https://tools.ietf.org/html/rfc8040) and [VPP Honeycomb](https://wiki.fd.io/view/Honeycomb) come to mind. The dynamics and velocity of container start/stop activity preclude central control. Stateless and lightweight data plane configuration performed at cloud native speed is a must.     
- __Feature-rich data plane__. Increases in traffic workload volumes coupled with per-service processing options such as ACL, NAT, tunnel encap/decap, load-balancing and encrypt/decrypt necessitate a robust, and multi-feature software data plane. 
- __User space networking__. Performance gains with user space packet processing vs kernel networking are well documented. User space permits network function processes to talk directly to physical NIC components, while bypassing the kernel altogether. Developers are free to innovate without kernel dependencies. Operators can start/stop NF processes without fear of disrupting kernel functions. The kernel is and should remain immutable in a CNF network.
- __Observability__ through logging, tracing, UI/UX visualization, telemetry, metrics, diagnostic/troubleshooting APIs and CLI is _mandatory_ for testing and operations.
- __Common APIs__ available to developers, applications and operators. There are myriad advantages ranging from faster development to simplified integration with external systems.      

---
## Cloud Native Network Functions (CNF)

A [cloud native network function (CNF)](https://ligato.io/blog/cnf-ligato-fdio/) is a network function designed and implemented to run inside containers. CNFs inherit all cloud native architectural and operational principles including K8s lifecycle management, agility, resilience, and observability. A CNF implementation should strive to meet the requirements outlined above.

{{< figure src="/images/ligato/cnf-net5.svg" class="image-center" >}}

<br>
</br>

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
Items to highlight from the figure above:
</p>
</div>

- __Application and CNF pods are deployed on workers__. As mentioned, a CNF is just another pod, albeit with specialized network functionality.
- __CNFs are versatile__. They can implemented with a single purpose in mind, an example being a load balancer. Or designed and built for programming and executing specific data plane functions. Consider a CNF platform that is programmed through APIs/agent to perform L3 routing in one instantiation, and firewall/NAT functions in another. Operators can and will choose from a broad portfolio of CNF options. 
- __CNF capabilities can be embedded in application pods__. For example, a [memif](https://docs.fd.io/vpp/17.10/libmemif_doc.html) interface can run on both CNF and application pods. The result is a point-to-point, user space communications channel that offers kernel-free packet transmit and receive functions.
          
<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
CNF Advantages:
</p>
</div>


* __Lifecycle management parity with application containers.__ CNFs receive at no cost, all existing best practices bestowed to application pods. These include: development environments, toolchains, CI/CD, K8s orchestration and scheduling, 12-factor app, distributed management, logging and telemetry streaming. 
  

* __Stateless Configuration.__ CNFs operate in pods. Pods are durable, but if something breaks or new functions are needed, then pods can be torn down and a new instance started up. The dynamic nature of cloud native environments precludes centralized or stateful CNF configuration management.  Instead, a stateless approach is employed where CNFs “watch” for and then process configuration updates stored in an external data store. Another lightweight option uses a service request to trigger a local gRPC call that pushes configuration state into the CNF data plane. 

* __Smaller footprint vis-a-vis physical or virtual devices.__ This reduces resource consumption, with the potential savings allocated to applications and/or infrastructure expansion. 

* __Rapid development, innovation and immutability.__ CNF functions should run in user space where it is easier, faster and less risky to develop and deploy new features and innovations. There is no need to interact with the stable and mature Linux kernel used by other components in the environment.

* __Performance.__ CNF processing in user space, NIC hardware control, multi-core tuning and kernel bypass all contribute to maximum throughput and resource efficiency. Characterizing CNF performance is underway with the [CNF Testbed](https://github.com/cncf/cnf-testbed) project. 

* __Agnostic to host environments__. Bare-metal and VMs are the most common. Since VMs do not add value to CNFs, bare-metal could become the preferred host of choice.

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

An inherent property of cloud native networking is service scalability and resiliency. In this scenario, a CNF can act as a load balancer directing traffic to the back-end pods of a K8s service, or as a policy-based forwarder for steering traffic to a specific application pod. While it is possible to perform these functions with kube-proxy, a CNF might be a better fit playing the role of ingress or loadbalancer for multiple reasons:

- Maximum throughput and scale with user space packet processing.
- Flexible load distribution strategies based on round-robin, multipath, server load and policy as examples.
- Observability. 

In addition, it provides visibility into this discrete network function vital to K8s services access, scale and availability.    

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">
Enhanced Cluster Networking
</p>
</div>

New CNF implementations can improve upon existing solutions, or enable a brand new network service. The top portion of the figure depicts a CNI-bootstrapped L2 bridge domain for inter-pod connectivity. Included is a CNF vSwitch performing two key functions: 

- Multi-interface support for attaching multiple application pods to the bridge domain.
- Mapping K8s service/policy traffic to a fast data plane. For example, QoS, NAT or ACLs can be programmed into the CNF vSwitch data plane based on K8s services or policies. This enhances throughput and provides flow visibility for service/policy traffic. The contiv-vswitch in [Contiv-VPP](https://contivpp.io/) performs this very function.  

The bottom portion of the figure depicts a CNF serving as a [Network Service Mesh (NSM)](https://networkservicemesh.io/) x-connect for a vWire connecting an NSM client to an NSM server. NSM binds interface mechanisms and payload types to construct the set of contiguous x-connected segments that form a vWire.  

Two additional items:

- CNI relies on existing K8s CNI orchestration methods augmented with [multi-interface pod customizations](https://github.com/contiv/vpp/blob/master/docs/operation/CUSTOM_POD_INTERFACES.md), and programming of K8s services/policies into a low-level data plane configuration functions.
- NSM uses its own Network Service Mesh Control Plane (NSMC) to program x-connects for vWire setup.
- Independent of control plane, both solutions use the same programmable data plane exposed by common low-level APIs. 
- Both solutions are mutually exclusive; Both can co-exist in a cluster network.

<div>
<p style="text-decoration: underline;font-weight: 600">Service Bundles</p>
</div>

CNFs are arranged into a chain of contiguous network functions supporting an application or network service offering. NSMC, a customized SFC control plane or CLI can handle SFC provisioning.   

In the top figure, an SFC is created to for a cloud storage service; the bottom file shows an ML training app.

Note that the cloud storage and ML training app pods could employ CNF functions such as the memif example described above.  

---

## CNF Architecture

Developers make design choices tailored for specific implementations. Simplifying the options to meet expectations requires an architectural approach:
 
- Targeted on performance, flexibility, programmability, observability, and scale.   
- Adhering to cloud native principles.
- Embracing evolvability as cloud native matures and new services emerge.

With these in mind, it becomes possible to paint a picture of a composite CNF architecture. Note this is inspired by and reflects an architecture supported by the Ligato framework.

`Note: To simplify the picture, the pod, host, kernels, NIC and physical underlay components have been omitted.` 

{{< figure src="/images/ligato/cnf-generic-arch2.svg" class="image-center figcaption" >}}

<br>
</br>
The architecture is assembled from and presented as a system using a layered approach: Applications, orchestration, management plane on top; Software data plane on the bottom. The term, `plugin`, is a placeholder for integrated and/or configured software components supporting a function or functions performed by the CNF 
 
A discussion follows beginning with the data plane.


<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">Data Plane
</p>
</div>

* User space data plane functionality was discussed earlier. It offers maximum throughput and kernel bypass. [FD.io](https://fd.io/) in one such data plane. It is an open source project with large community support, 10+ years of installed product implementations, and comes with a rich feature set.  
* Linux kernel. Of course there is the throughput ceiling and feature upgrade time lag to contend with. However, it is the most common data plane in virtual cloud environments. Some CNF implementations can and will make use of kernel networking. Work on kernel extensions such as [eBPF](https://opensource.com/article/17/9/intro-ebpf) can improve performance.

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">Control Plane
Agent</p>
</div>

* Exposes northbound APIs to: Internal applications, processes, plugins; External applications, KV data stores, orchestrators and management plane functions.
* Implements a low-level southbound API for programming data plane functions.
* Agent plugins are included to enable one or more CNF functions. Plugins for data plane programmability, internal communications with custom applications, and interactions with external systems are feasible and desired. 

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">Custom Applications / Plugins
</p>
</div>

* Customized applications and plugins can be wired into the CNF for additional solution-specific functions. As an example, an IPsec VPN CNF could integrate a control plane for IKEv2 SA setup.
* CNFs dedicated to network control plane functions are possible. Consider a specialized BGP-only pod configured with eBGP Multihop to exchange prefixes with an external routing domain, even one operating physical routers only. 
* Note that the architecture and implementation of a distributed CNF solution may call for  the control plane and data plane functions to run in separate pods. 

<div>
<p class="is-size-5" style="text-decoration: underline;font-weight: 600">External Applications/Orchestration/CNF Management Plane
</p>
</div>

* Cloud native is a dynamic and evolving environment. New features, open source projects, new applications, architectures, toolchains and so on are being invented and added all of the time. One need only examine the [CNCF landscape](https://landscape.cncf.io/) to appreciate the breadth of this work.
* External components that directly or indirectly support CNF development, implementation, programmability, observability and operations are candidates for inclusion in current and evolving CNF endeavors.  
 
In any domain, the network plays a crucial role. This is especially true in cloud native environments . The momentum behind cloud native, the architecture, applications, and the greater ecosystem has perhaps inadvertently pushed networking into the background.  

The advent of CNFs can serve as a vehicle to establish the network as playing a prominent and critical role in the success of cloud native. 




