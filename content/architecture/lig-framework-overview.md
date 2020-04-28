---
title: "CNF Building Components"
date: 2019-04-16T05:23:30-07:00
layout: "arch"
draft: false
---




### Overview

Ligato is an open source framework for building and developing solutions for managing and controlling CNFs. Reusable software components, model/protobuf-modelled APIs, dependency injection, and logging/REST/CLI visibility are provided. Standard coding patterns along with accompanying documentation, tutorials and examples are supplied to facilitate development. 

A set of VPP and Linux plugins including a transaction-based configuration scheduler are implemented into a VPP agent enabling agile and resilient deployments. Infrastructure plugins offer K8s lifecycle management, health care checking, notifications, data store/database integration, and logging for cloud-native operational affinities.  
  
 Ligato is written in Golang as its programming language for the performance, concurrency, scale, readability, and broad community support necessary for developing and implementing cloud-native network solutions. 

<br />

{{< figure src="/images/ligato/ligato-framework-arch.svg" class="image-center figcaption"caption="Ligato Framework" >}}

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/intro/framework/"class="button is-success">More about the Ligato Framework</a>
                        </div>
</div>

---

### Plugins


### Models


#### Proto

<div>
    <pre>
        <code>
        // Interface defines a VPP interface.
        message Interface {
            // Name is mandatory field representing logical name for the interface.
            // It must be unique across all configured VPP interfaces.
            string name = 1;
        
            // Type defines VPP interface types.
            enum Type {
                UNDEFINED_TYPE = 0;
                SUB_INTERFACE = 1;
                SOFTWARE_LOOPBACK = 2;
                DPDK = 3;
                MEMIF = 4;
                TAP = 5;
                AF_PACKET = 6;
                VXLAN_TUNNEL = 7;
                IPSEC_TUNNEL = 8 [deprecated=true]; // Deprecated in VPP 20.01+. Use IPIP_TUNNEL + ipsec.TunnelProtection instead.
                VMXNET3_INTERFACE = 9;
                BOND_INTERFACE = 10;
                GRE_TUNNEL = 11;
                GTPU_TUNNEL = 12;
                IPIP_TUNNEL = 13;
            };       
        </code>
    </pre> 
</div>
<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/intro/framework/"class="button is-success">More about the Ligato Framework</a>
                        </div>
</div>

---

### KV Scheduler

The runtime configuration  of a CNF must accurately reflect the desired network function. One configuration item could be dependent on the successful configuration of another item. Example: a route cannot be configured without an interface, therefore we say the interface is a dependency configuration item of the route. And these dependencies must be tracked and coordinated to ensure the proper configure sequence. 

{{< figure src="/images/ligato/components-kvs-architecture-lite.svg" class="image-center figcaption"caption="KV Scheduler" >}}

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/intro/framework/#kv-scheduler"class="button is-success">More about the KV Scheduler</a>
                        </div>
</div>

The KV scheduler is plugin that works with VPP and Linux agents on the SB side, and orchestrators/external data sources such as KV data stores and RPC clients on the NB side. In a nutshell, it keeps track of the correct configuration order and associated dependencies.

#### Stash

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



