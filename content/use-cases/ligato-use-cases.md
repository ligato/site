---
title: "Use Cases"
date: 2019-04-23T14:54:27-07:00
layout: "use-cases"
draft: false
---



<div class="tile is-ancestor">
    <div class="tile is-12 is-parent">
        <div class="tile is-child box notification is-success">
            <p class="title">Core CNF</p>
        </div>
    </div>>
</div>


<div class="tile is-ancestor">
    <div class="tile is-12">
        <div class="tile">
            <div class="tile is-parent is-8">
                <article class="tile is-child box">
                    <p class="title">Ligato + VPP</p>
                    <figure class="image is-4by3">
                        <img src="/images/ligato/ligato-cnf-evolve.svg">
                    </figure>
                    <div class="is-size-6" style="margin-top: 30px">
                    <p>
                     Ligato VPP Agent and VPP dataplane are "bolted" together in one container forming the programmable VPP vSwitch</p>
                    </div>                   
                </article>
            </div>
            <div class="tile is-parent is-vertical is-4">
                <article class="tile is-child box markdown-body">
                    <p class="title">Problem To Solve</p>
                    <p class="subtitle">Need core network function with:</p>
                    <div class="is-size-6">              
                    <ul>
                        <li>Container lifecycle best practices</li>
                        <li>Control plane agent</li>
                        <li>Code reusability</li>
                        <li>Function plugin options</li>
                        <li>Stats/Telemetry</li>
                        <li>12-factor app principles</li>
                        <li>Operate in user space</li>
                        <li>Extensibility</li>
                        <li>Immutability</li>
                        <li>Open Source</li>
                    </ul>
                    </div>
                </article>
                <article class="tile is-child box">
                    <p class="title">Solution</p>
                    <p class="subtitle">Programmable VPP vSwitch</p>
                    <div class="is-size-6">
                    <ul>
                       <li>FD.io/VPP dataplane feature set</li>
                       <li>Lightweight container packaging
                       <li>Innovation ready</li>
                       <li>Cloud Native distributed configuration 
                       <li>Implemented in multiple solutions
                       <li>Demonstrated code reusability, platform versatility and operational deployment feasibility</li>
                    </ul>                                                                                                                   
                </div>
                </article>
            </div>
        </div>
    </div>
</div>

An important aspect of any container-based network soltion is rapid and accurate configuration programmability. Containers are added and removed frequently. They _must_ (re)start within a second and there could be multiple running instances on a single host. They could move to other hosts. The complete configuration required for run-time operations may require each separate comfiguration action to be performed in proper sequence. Therefore it is crucial there be minimal delay to programming the correct configuration in the correct into the dataplane. 

Other solutions, projects and ventures either being worked on, shipping or considering the use of a container-based VPP vSwitch include [hICN](https://fd.io/2019/02/introducing-hybrid-information-centric-networking-hicn-a-new-fd-io-project/), [ONAP](https://www.onap.org), [Service Function Chaining](https://github.com/ligato/sfc-controller) as well as cable, mobility and service mesh edge proxies.

<br/>
---
<div class="tile is-ancestor">
    <div class="tile is-12 is-parent">
        <div class="tile is-child box notification is-link">
            <p class="title">Contiv - VPP</p>
        </div>
    </div>>
</div>

<div class="tile is-ancestor">
    <div class="tile is-12">
        <div class="tile">
            <div class="tile is-parent is-8">
                <article class="tile is-child box">
                    <p class="title">Kubernetes CNI Plugin with a VPP Dataplane</p>
                    <figure class="image is-4by3">
                        <img src="/images/ligato/ligato-contiv-vswitch2.svg">
                    </figure>                   
                    <div style="padding-top: 100px">
                        <div style="text-align: center">
                            <a href="https://contivpp.io"class="button is-link">Contivpp.io</a>
                        </div>
                    </div>
                </article>
            </div>
            <div class="tile is-parent is-vertical is-4">
                <article class="tile is-child box">
                    <p class="title">Problem To Solve</p>
                    <p class="subtitle">for K8s cluster networks</p>             
                        <ul class="is-size-6">
                            <li>K8s policy/services awareness at the network layer</li>
                            <li>Flexible networking options</li>
                            <li>Interface flexibility (i.e. TAPv2, veth, memif)
                            <li>Fast config programmability
                            <li>Container lifecycle best practices</li>
                            <li>Stats and Telemetry streaming and export</li>
                            <li>12-factor app principles</li>
                            <li>Operate in user space</li>
                        </ul>                    
                </article>
                <article class="tile is-child box outside">
                    <p class="title">Solution</p>
                    <p class="subtitle">Contiv - VPP</p>
                    <ul class="is-size-6">
                       <li>K8s Policies/Services mapped to VPP networks</li>
                       <li>contiv-vSwitch CNF tailored for K8s / Contiv - VPP networking</li>
                       <li>User space operations</li>
                       <li>Web UI</li>
                    </ul>                                                                                                                       
                </article>
            </div>
        </div>
    </div>
</div>

The figure above presents an abstracted view of Contiv-VPP deployed in a K8s cluster. The unique aspects of this solution are:

* __K8s Policy and Service Mapping to FD.io/VPP Configuration.__ This is an automated distribution pipeline where k8s policies and services are automatically reflected into FD.io/VPP configuration information which is then programmed into the network.    

* __contiv-vswitch.__ This is the CNF composed of the FD.io/VPP dataplane and a Ligato-based VPP agent. It is across APIs enabled by the contiv and VPP agent/plugins that configurations rendered from policy and services are pushed down into the VPP dataplane. The contiv-vswitch runs in user space and uses [DPDK](https://dpdk.org/) for direct access to the network I/O layer.


Extensions to Contiv - VPP in development are support for IPv6, SRv6 and SFC:

<div class="columns">
    <div class="column is-one-third">
        <div style="text-align: center">
            <a href="https://github.com/contiv/vpp/blob/master/docs/setup/IPV6.md"class="button is-link">IPv6</a>
       </div> 
  </div>    
  <div class="column">
        <div style="text-align: center">
          <a href="https://github.com/contiv/vpp/blob/master/docs/setup/SRV6.md"class="button is-link">SRv6</a>
        </div>  
  </div>
  <div class="column">
        <div style="text-align: center">
          <a href="https://github.com/contiv/vpp/tree/master/k8s/examples/sfc"class="button is-link">SFC</a>
        </div>  
  </div>
</div>

 

<br/>
---


<div class="tile is-ancestor">
    <div class="tile is-12 is-parent">
        <div class="tile is-child box notification is-danger">
            <p class="title">IPsec VPN Gateway</p>
        </div>
    </div>
</div>

<div class="tile is-ancestor">
    <div class="tile is-12">
        <div class="tile">
                <div class="tile is-parent is-8">
                        <article class="tile is-child box">
                            <p class="title">Cloud Native Solution</p>
                            <figure class="image is-5by5">
                                <img src="/images/ligato/kiknos-ipsec-vpn-gateway3.png">
                            </figure>
                            <div style="padding-top: 180px">
                                <div style="text-align: center">
                                    <a href="https://www.strongswan.org/" class="button is-danger has-text-white">StrongSwan VPN</a>
                                    <a href="https://wiki.fd.io/view/VPP/IPSec_and_IKEv2" class="button is-danger has-text-white">VPP IPsec Features</a>
                                </div>
                            </div>
                        </article>
        </div>
        <div class="tile is-parent is-vertical is-4">
                        <article class="tile is-child box">
                            <p class="title">Problem To Solve</p>
                            <p class="subtitle">capacity & performance needs</p>                                      
                            <ul class="is-size-6">
                                <li>Tunnel b/w from Mbps to multi-Gbps</li>
                                <li>Tunnel termination scale up to tens of thousands</li>
                                <li>Per-client tunnel stats and telemetry</li>
                                <li>K8s cloud native operations and lifecycle</li>
                                <li>IPsec and IKEv2 support</li>                               
                            </ul> 
                        </article>
                        <article class="tile is-child box outside">
                            <p class="title">Solution</p>
                            <p class="subtitle">StrongSwan VPN control plane with VPP dataplane</p>
                            <ul class="is-size-6">
                                <li>Leverages complete IPsec VPP feature set</li>
                                <li>Scales up to Multi-Gbps throughput</li>
                                <li>Extensive client tunnel stats/telemetry</li>
                                <li>Pod/container horizontal scaling</li>
                                <li>IPsec and IKEv2 standard feature support</li> 
                                <li>StrongSwan, Ligato and FD.io/VPP are open source projects</li>  
                            </ul>                                                                                                                     
                        </article>
        </div>
    </div>
</div>
</div>


