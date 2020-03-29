---
title: "What is a CNF?"
date: 2019-04-23T14:25:06-07:00
layout: "cnf"
draft: false
---

## Virtualized Network Function (VNF)

Network functions (NF) are physical devices that process packets supporting a network and/or application service. Performance, features, scale and operational control are paramount. Routers, campus switches and firewalls are NF examples. A topology composed of similar or dissimilar interconnected NFs, form a network. Interconnected networks are assembled into larger networks supporting a broad range of applications, services, customers, and connectivity. This network model applies in all cases, from the Internet all the way down to residential wifi networks, where physical network infrastructure is used.

A virtual machine (VM) is a software-only version of a physical server machine. It runs a complete instance of the application code, a guest OS/kernel, and hypervisor that coordinates VM resource management. A host OS/kernel is present, and collectively, all run on commodity-off-the-shelf (COTS) hardware. The advantages of VMs include: cost savings due to NF hardware decoupling; automated configuration and operations using mature VM management tools; multi-tenancy; security and isolation. The disadvantage is the overhead required to emulate in software, all software and hardware functions, that normally run on physical servers. VMs operate in public or private clouds largely comprised of data center server farms.

Applications and NFs that run on dedicated physical servers are referred to as bare-metal. The advantages of bare-metal are single tenancy, security, isolation, and predictable performance. The disadvantages are slower deployment operations, kernel network bottlenecks, and additional hardware costs.

A virtualized network function (VNF) is an NF designed to run in a virtualized environment. By convention, VNFs run in VMs and accrue all aforementioned VM advantages and disadvantages. VNFs can be networked together into a virtual overlay network running on top of a physical underlay network. Excluding performance and scale, VNFs mirror just about all of the functions and features found in dedicated hardware NFs, modulo the requisite adaptations for operating in cloud environments.

Additional VNF advantages include:

- VNFs can be easily pre-staged to handle increased traffic workloads driven by customer growth and/or new services.
- Service Chaining. A choreographed formation of VNFs can be assembled into a uniform customer application or service.
- Performance and scale numbers are well understood. There should be no surprises with proper capacity planning, a statement applicable to any network environment.
- Network architecture and operations affinity with physical networks.   

VNF challenges include :

- Performance impact due to VM software overhead including the guest OS/kernel, host OS/kernel and hypervisor. 
- Long boot times under normal maintenance, failure restart, and burst scenarios can impact availability. 
- VNF scale out requires investment in more servers and network gear such as routers, switches and load balancers.
- Potential for VM resources to sit idle. VNFs can only be deployed in coarse VM increments. Optimizing VNF resource allocations sufficient to meet traffic workload demands will not be precise. Over-provisioning is required and by definition, resources will sit idle. This is a classic traffic engineering problem with the exception here being those idle VM resources could be assigned to cloud-ready applications or server expansion.   
- Implemented as indivisible code monoliths.  This means all development, testing, maintenance, deployment and troubleshooting must consider the VNF as a single atomic unit.

VNFs are the current defacto standard for standing up network functions in cloud environments.   

## Cloud Native

Cloud Native is a new approach for developing and running applications in a virtualized cloud environment. The cornerstone principles of cloud native are:

- Applications are "carved up" into smaller units called microservices. Application monoliths are replaced by a constellation of smaller inter-connected microservices.
- Containers house the microservices and provide a run-time environment. Containers dispense with VM overhead, and instead just package the application code, binaries and dependencies. Containers share a guest and/or OS kernel. 
- Orchestration provides complete container lifecycle support including scheduling, placement, start/stop/re-start and visibility. Kubernetes is the cloud native orchestration platform. 

    In the Kubernetes environment, one or more containers are grouped into pods that run on physical or virtual hosts. A pod is the smallest unit of work supported by Kubernetes. A collection of hosts and pods managed by Kubernetes is referred to as a cluster.

In addition, cloud native embraces the notions of open source, 12-factor app, and devops CI/CD pipelines.      

These principles are encapsulated in the Cloud Native Compute Foundation (CNCF) definition:
 
`“using open source software stack to be containerized, where each part of the app is packaged in its own container, dynamically orchestrated so each part is actively scheduled and managed to optimize resource utilization, and microservices-oriented to increase the overall agility and maintainability of applications.”`

### Container Networking Requirements

One aspect of K8s networking involves inter-pod connectivity provided by the Kubernetes Container Network Interface (CNI) specification. There are multiple CNI solutions for establishing different forms of virtual L2/L3 overlay networks between pods. CNI solutions are quite suitable for web traffic workloads. However, they are not ideal for enabling services that leverage VNF-like functionality, process compute-intensive traffic workloads, or exploit the agility and dynamics of cloud native environments. 

A new set of requirements emerge for augmenting basic container networking with VNF-like capabilities:

- __(NFs) adapted for cloud native deployment and operations__. This assumes a container (pod) form-factor under K8s orchestration control, or a control/management plane co-existing or compatible with K8s. The latter is useful if pods need to be wired up into a mesh topology. Note that K8s does not currently support "partial mesh" or "service function chain" provisioning. 
- __Lightweight NF data plane configuration__. NF and VNF configurations have traditionally been performed by human operators, or centralized management systems communicating through a control channel to a configuration agent. Netconf, restconf and VPP honeycomb come to mind. The dynamics and velocity of container start/stop activity preclude central control. Stateless and lightweight data plane configuration performed at cloud native speed is a must.     
- __Feature-rich data plane__. Increases in traffic workload volumes coupled with per-service processing options such as ACL, NAT, tunnel encap/decap, load-balancing and encrypt/decrypt necessitate a robust, and multi-feature software data plane. 
- __User space networking__. Performance gains with user space packet processing vs kernel networking are well documented. User space permits network function processes to talk directly to physical NIC components, and bypass the kernel altogether. Developers are free to innovate without kernel dependencies. Operators can start/stop NF processes without fear of disrupting kernel functions. The kernel is and should remain immutable in a CNF network.

Although not per se a new requirement for cloud environments, observability through logging, tracing, UI/UX visualization, telemetry, diagnostic/troubleshooting APIs and CLI is mandatory for operations. 

---
## Cloud Native Network Functions (CNF) is the Solution

A cloud native network function (CNF) is a network function designed and implemented to run inside containers. CNFs inherit all cloud native architectural and operational principles including K8s lifecycle management, agility, resilience, and observability.

{{< figure src="/images/ligato/cnf-net5.svg" class="image-center" >}}

<br>
</br>

Items to highlight from the figure above:

- __Application and CNF pods are deployed on workers__. As mentioned, a CNF is just another pod, albeit with specialized network functionality.
- __CNFs are versatile__. They can implementated with a single purpose in mind, an example being a load balancer. Or can be designed and built for programming and executing specific data plane functions. Consider a CNF platform that can be programmed through APIs/agent to perform L3 routing in one instantiation, and firewall/NAT functions in another. Operators can and will choose from a broad portfolio of CNF options. 
- __CNF capabilities can be embedded in application pods__. For example, a memif interface can run on both CNF and application pods. The result is a point-to-point communications channel, with kernel bypass, enabling an efficient and performant data transport channel. Indeed, a topology of CNF and application pods interconnected with memif links is optimal for performance and operational flexibility.          
 
CNF advantages include:

* __Lifecycle management parity with application containers.__ CNFs receive at no cost, all existing best practices bestowed to application pods. These include: development environments, toolchains, CI/CD, K8s orchestration and scheduling, 12-factor app, distributed management, logging and telemetry streaming. 
  

* __Stateless Configuration.__ CNFs operate in pods. Pods are durable, but if something breaks or new functions are needed, then pods can be torn down and a new instance started up. The dynamic nature of cloud native environments precludes centralized or stateful CNF configuration management.  Instead, a stateless approach is employed where CNFs “watch” for and then process configuration updates stored in an external data store. Another lightweight option uses a service request to trigger a local gRPC call that pushes configuration state into the CNF data plane. 

* __Smaller footprint vis-a-vis physical or virtual devices.__ This reduces resource consumption, with the potential savings allocated to applications and/or infrastructure expansion. 

* __Rapid development, innovation and immutability.__ CNF functions should run in user space where new features and innovations can be developed and applied. There is no need to interact with the linux kernel.

* __Performance.__ CNF processing in user space, NIC hardware control, multi-core tuning and kernel bypass all contribute to maximum throughput and resource efficiency. 

* __Agnostic to host environments__. Bare-metal and VMs are the most common. Since VMs do not add value to CNFs, bare-metal could become the preferred host of choice.

---

## CNF Solution Scenarios

CNFs are rolled out with functions suited for their role in a cloud native network. For illustrative purposes, the figure below depicts several scenarios. 

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
            <p class="title">SFC Service Bundles</p>
            <p class="subtitle">by wiring up CNFs</p>
            <figure class="image is-4by3">
                <img src="/images/ligato/cnf-sfc-example.svg">
            </figure>
        </article>
    </div>
</div>

__Load Distribution__

An inherent property of cloud native networking is service scalability and resiliency. In this scenario, a CNF can act as a load balancer directing traffic to the back-end pods of a K8s service, or as a policy-based forwarder for steering traffic to a specific application pod. While it is true that both functions can be achieved with kube-proxy, a CNF might be a better fit playing the role of ingress or loadbalancer for multiple reasons: better throughput, flexible load distribution and observability. In addition, it provides visibility into this discrete network function vital to K8s services access, scale and availability.    

__Enhanced Cluster Networking__

New CNF implementations can improve upon existing solutions, or enable a brand new network service. The top portion of the figure depicts a CNI that bootstrapped  a L2 bridge domain for inter-pod connectivity. Included is a CNF vSwitch for the policy/service-aware switching of traffic workloads between pods. The CNF vSwitch pods can support multiple interfaces, and data plane "awareness", and data plane traffic visibility for K8s service or policy traffic. The latter is significant because, for example, QoS, NAT or ACLs can be programmed into the CNF vSwitch data plane based on K8s services or policies. 

The bottom portion of the figure depicts a CNF serving as a network service mesh (NSM) x-connect for a vWire connecting an NSM client to an NSM server. 

Two additional items:

- CNI relies on existing K8s CNI orchestration methods augmented with multi-interface pod support, and programming of K8s services/policies into a low-level data plane configuration functions.
- NSM applies its own and very simple control plane to program x-connects needed for vWire setup. 
- Both solutions are mutually exclusive; Both can co-exist in a cluster network.

__SFC Service Bundles__ 

CNFs can be wired up into a chain of contiguous network functions supporting an application or network service offering. This is NOT supported in current K8s CNI solutions. A customized SFC control plane or CLI could handle SFC provisioning. It could even be the NSM control plane because in fact, an SFC topology is a partial mesh. 

In the top figure, an SFC is created to support a cloud storage service. In the bottom, an ML training app.

Note that the cloud storage and ML training app pods could employ CNF functions such as the memif example described above.  

---

## CNF Architecture

Developers make design choices tailored for specific implementations. Simplifying the options to meet expectations requires an architectural approach:
 
- targeted on performance, flexibility, programmability, observability, and scale 
- adhering to cloud native principles.
- embracing evolvability as cloud native matures and support for new services emerge.

With these in mind, we can paint a picture of a composite CNF architecture. Note this is inspired by and reflects an architecture supported by the Ligato framework.

`Note: To simplify the picture, the pod, host, kernels, NIC and physical underlay components have been omitted.` 

{{< figure src="/images/ligato/cnf-generic-arch2.svg" class="image-center figcaption" caption="Composite CNF Architecture" >}}

The architecture is assembled from and presented as a system using a layered approach: applications, orchestration, management plane on top; software data plane on the bottom. The term, `plugin`, is used in several layers as a placeholder for integrated and/or configured software components supporting a function or functions. 
 
 A discussion follows beginning with the data plane.


__Data Plane__

* A user space data plane was discussed earlier and clearly is the best option. FD.io is open source project with large community support. It has been implemented in products for 10+ years and comes with a rich feature set.  
* Linux kernel support. Of course there is the performance ceiling and feature upgrade delivery lag to contend with. However, it is the most common data plane in virtual cloud environments. Some CNF implementations will make use of kernel networking. Work on kernel extensions such as eBPF can improve performance.

__Control Plane Agent__

* Exposes northbound APIs to: internal applications, processes, plugins; external applications, KV data stores, orchestrators and management plane functions.
* Implements a low-level southbound API for programming data plane functions.
* Agent plugins are turned on to enable one or more CNF functions. This includes plugin for data plane programmability, internal communications with custom applications, communications with external applications. 

__Custom Applications / Plugins__

* Customized applications and plugins can be wired into the CNF for additional functions needed for a particular service. As an example, an IPsec VPN CNF could integrate an IKE control plane supporting IKEv2 negotioans for SA setup.
* CNFs dedicated to network control plane functions are possible. For example, a specialized BGP-only pod can be configured with eBGP Multihop to exchange prefixes with an external routing domain, even one operating physical routers only. 
* Note that the architecture and implementation of a cloud native network service could call for the control plane functions to run in separate pods. If so, it can be argued that the control plane function is providing external CNF management plane functions as described below. 

__External Applications/Orchestration/CNF Management Plane__

* Cloud native is a dynamic and evolving environment. New features, open source projects, customizations, tools are being invented and added all of the time. One need only examine the CNCF landscape to appreciate the breadth of this work.
* External components that directly or indirectly support CNF development, implementation, programmability, observability and operations are certain candidates for current and evolving CNF environments.  
 
In any domain, networking plays a crucial role. This is especially true in cloud native environments . The momentum behind cloud native, the architecture, applications, and the greater ecosystem has inadvertently pushed networking into the background.  

The advent of CNFs can serve as a vehicle to establish the network as playing a prominent and critical role in the success of cloud native. 




