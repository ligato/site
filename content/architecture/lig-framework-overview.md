---
title: "Building Framework"
date: 2019-04-16T05:23:30-07:00
layout: "arch"
draft: false
---




### Overview

Ligato is an open source Go framework for building and developing solutions for managing and controlling CNFs. Reusable software components, protobuf-modelled APIs, dependency injection, and logging/REST/CLI visibility are provided. Documentation, tutorials and examples are supplied to facilitate development. 

A set of VPP and Linux plugins, including a transaction-based configuration scheduler, are implemented in the VPP agent enabling agile and resilient deployments. Infrastructure plugins offer K8s lifecycle management, health care checking, notifications, data store/database integration, and logging for cloud-native operational affinities.  
  
Ligato is written in Golang as its programming language for the performance, concurrency, scale, readability, and broad community support necessary for developing and implementing cloud-native network solutions. 

<br />

{{< figure src="/images/ligato/ligato-framework-arch.svg" class="image-center figcaption"caption="Ligato Framework" >}}

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/intro/framework/" class="button btn-align-md secondary-btn raised">More on the Ligato Framework</a>
                        </div>
</div>


---

### Plugins

The VPP agent and cn-infra are the two constituent frameworks that, together, form the basis of the Ligato framework. With Ligato, each management / control plane application utilize one or modules called plugins. Each plugin supports a specific function or functions. Some plugins come with the cn-infra framework; Others come with the VPP agent; Yet others created by app developers perform custom tasks.

Plugins can be assembled in any combination to build solutions ranging from simple configuration tasks, to larger more complex operations such as managing configuration state across multiple nodes in a network. The plugin definition is standardized in the Ligato framework, and plugin behavior at startup can be modified through the use of configuration files. 




<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/plugins/plugin-overview/"class="button btn-align-md secondary-btn raised">Plugins Doc</a>
                        </div>
</div>


---

### VPP Agent

The VPP agent provides configuration and monitoring services for the VPP data plane.

* Supplies VPP and Linux plugins
* VPP agent and VPP packaged up in single container
* Dependency handling between related configuration items
* Transaction-based configuration processing and tracking
* Failover synchronization mechanisms, also known as resync
* Stateless configuration management based on a KV data store "watch" paradigm
* Direct access via REST or gRPC
* Component health checks

---

### VPP Agent Plugins

The VPP Agent includes VPP and Linux plugins for network configuration and visibility. CNFs and cloud-native network applications can be developed using VPP agent plugins to interact with other services/microservices in a cloud-native environment. 


{{< figure src="/images/ligato/components-ligato-framework-vpp-agent-SB-picture.svg" class="image-center figcaption"caption="" >}}

---

<div class="tile is-ancestor">
    <div class="tile is-12">
        <div class="tile">
            <div class="tile is-parent is-7">
                <article class="tile is-child box">
                <p>VPP network function plugins</p>
                    <div>
                        <pre>
                            <code>
vpp-agent/plugins/vpp

├── abfplugin // ACL-based forewarding
├── aclplugin // ACL
├── ifplugin // Interfaces
├── ipsecplugin // IPsec
├── l2plugin // L2 including bridge domain, x-connect and FIB
├── l3plugin // routes, VRFs
├── natplugin // NAT
├── puntplugin // Punt to Host
├── srplugin // segment routing
├── stnplugin // steal the nic
                           </code>
                        </pre>
                    </div>
                </article>
            </div>
            <div class="tile is-parent is-vertical is-5">
                <article class="tile is-child box">
                <p>Linux network function plugins</p>
                    <div>
                        <pre>
                            <code>
 vpp-agent/plugins/linux
 
├── ifplugin // host interface
├── iptablesplugin // Linux IP tables
├── l3plugin // routing, arp
└── nsplugin // namespace
                            </code>
                        </pre>
                    </div>
                </article>
            </div>
        </div>
    </div>
</div>

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/plugins/vpp-plugins/"class="button btn-align-md secondary-btn raised">VPP Plugins Doc</a>
                        </div>
</div>

---

### cn-infra Plugins

cn-infra provides the plugins needed for CNF and application operating in modern cloud-native CNFs and apps. Plugins offering logging, health checks, messaging, KV data store connectivity, REST/gRPC APIs are available. Developers can implement a mix of cn-infra plugins, together with other VPP agent and/or custom plugins, and define CNF functionality. 


{{< figure src="/images/ligato/ligato-framework-arch-infra.svg" class="image-center figcaption"caption="" >}}

---

cn-infra plugins fall into one of three categories: 

- Infra for logging, messaging, process management, status checking and service label
- Database for external data store connectivity and database integration
- RPC connections supporting gRPC/REST APIs

---

<div class="tile is-ancestor">
    <div class="tile is-12">
        <div class="tile">
            <div class="tile is-parent is-4">
                <article class="tile is-child box">
                    <p class="subtitle">Infra Plugins</p>
                    <ul>
                        <li>Status Check</li>
                        <li>Index Map</li>                    
                        <li>Log Manager</li>
                        <li>Messaging/Kafka</li>
                        <li>Process Manager</li>
                        <li>Supervisor</li>
                        <li>Service Label</li>
                    </ul>           
                </article>
            </div>
            <div class="tile is-parent is-vertical is-4">
                <article class="tile is-child box">
                    <p class="subtitle">Database Plugins</p>
                    <ul>
                        <li>Datasync</li>
                        <li>Data Broker</li>                    
                        <li>etcd</li>
                        <li>Redis</li>
                        <li>Consul</li>
                        <li>Bolt</li>
                    </ul>                    
                </article>
            </div>
            <div class="tile is-parent is-4">
                <article class="tile is-child box">
                    <p class="subtitle">RPC Connection Plugins</p>
                    <ul>
                        <li>REST</li>
                        <li>gRPC</li>                    
                     </ul> 
                </article>
            </div>
        </div>
    </div>
</div>
<div class="columns">
    <div class="column is-one-third">
        <div style="text-align: center">
            <a href="https://docs.ligato.io/en/latest/plugins/infra-plugins/"class="button btn-align-md secondary-btn raised">Infra Plugins Doc</a>
       </div> 
  </div>    
  <div class="column">
        <div style="text-align: center">
          <a href="https://docs.ligato.io/en/latest/plugins/db-plugins/"class="button btn-align-md secondary-btn raised">Database Plugins Doc</a>
        </div>  
  </div>
  <div class="column">
        <div style="text-align: center">
          <a href="https://docs.ligato.io/en/latest/plugins/connection-plugins/"class="button btn-align-md secondary-btn raised">RPC Connection Plugins Doc</a>
        </div>  
  </div>
</div>

---

### Models / Protobufs / Keys

Model abstractions, protobufs, key identifiers and KV data stores are fundamental concepts in Ligato. The application of models and protobuf definitions define northbound APIs for managing and controlling configuration objects in a CNF.

{{< figure src="/images/ligato/components-model-proto-KV-store2.svg" class="image-center figcaption"caption="" >}}

<br/>
In Ligato, each object is defined by a model. The model is composed of a model specification and protobuf .proto defintion. A key is generated for the model, and that key is used to read/write configuration and status information into a KV data store. The key is also used in REST API calls for retrieving configuration information.

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/user-guide/concepts/"class="button btn-align-md secondary-btn raised">Concepts Doc</a>
                        </div>
</div>

---

#### Models

The model represents an abstraction of an object that can be managed through northbound APIs exposed by the VPP agent. It consists of a model specification and a proto.Message.

Here is a snippet of model.Spec code for a VPP L3 route:

<div>
    <pre>
        <code>
// ModuleName is the module name used for models.
const ModuleName = "vpp"
...
	ModelRoute = models.Register(&Route{}, models.Spec{
		Module:  ModuleName,
		Type:    "route",
		Version: "v2",
	}, models.WithNameTemplate(
		`{{if .OutgoingInterface}}{{printf "if/%s/" .OutgoingInterface}}{{end}}`+
			`vrf/{{.VrfId}}/`+
			`{{with ipnet .DstNetwork}}{{printf "dst/%s/%d/" .IP .MaskSize}}`+
			`{{else}}{{printf "dst/%s/" .DstNetwork}}{{end}}`+
			`{{if .NextHopAddr}}gw/{{.NextHopAddr}}{{end}}`,
	))
...
        </code>
    </pre>
</div>


---

#### Proto

Protobufs is method that defines the structure and serialized format of the data associated with an object. Ligato implements a .proto definition for each configuration object.

Here is a snippet of the .proto file for a VPP L3 route:  

<div>
    <pre>
        <code>
syntax = "proto3";

package ligato.vpp.l3;

option go_package = "go.ligato.io/vpp-agent/v3/proto/ligato/vpp/l3;vpp_l3";

message Route {
    enum RouteType {
        // Forwarding is being done in the specified vrf_id only, or according to
        // the specified outgoing interface.
        INTRA_VRF = 0;
        // Forwarding is being done by lookup into a different VRF,
        ...
        INTER_VRF = 1;
        // Drops the network communication designated for specific IP address.
        DROP = 2;
    }
    RouteType type = 10;

    // VRF identifier, field required for remote client. This value should be
    // consistent with VRF ID in static route key. 
    ...
    uint32 vrf_id = 1;

    // Destination network defined by IP address and prefix.
    string dst_network = 3;

    // Next hop address.
    string next_hop_addr = 4;

    // Interface name of the outgoing interface.
    string outgoing_interface = 5;

    // Weight is used for unequal cost load balancing.
    uint32 weight = 6;
    // Preference defines path preference. Lower preference is preferred.
    
    uint32 preference = 7;

    // Specifies VRF ID for the next hop lookup / recursive lookup
    uint32 via_vrf_id = 8;
}
        </code>
    </pre>
</div>

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://github.com/ligato/vpp-agent/tree/master/proto/ligato"class="button btn-align-md secondary-btn raised">VPP Agent Proto Folder</a>
                        </div>
</div>

---

### Keys

Key identifiers are used to identify objects controlled and managed by Ligato-built CNFs.

The key for our VPP L3 route is:
```json
/vnf-agent/vpp1/config/vpp/v2/route/vrf/<vrf_id>/dst/<dst_network>/gw/<next_hop_addr>
```   
<br/>

This key is used to PUT L3 route values into a KV data store.
```json
agentctl kvdb put /vnf-agent/vpp1/config/vpp/v2/route/vrf/0/dst/10.1.1.3/32/gw/192.168.1.13 
'{  
    "type": "INTRA_VRF",
    "dst_network":"10.1.1.3/32",
    "next_hop_addr":"192.168.1.13",
    "outgoing_interface":"tap1",
    "weight":6
}'
```
<br/>

<div class="columns">
    <div class="column is-half">
        <div style="text-align: center">
            <a href="https://docs.ligato.io/en/latest/user-guide/concepts/#key-prefix"class="button btn-align-md secondary-btn raised">Keys Concepts Doc</a>
       </div> 
  </div>    
  <div class="column">
        <div style="text-align: center">
          <a href="https://docs.ligato.io/en/latest/user-guide/reference/"class="button btn-align-md secondary-btn raised">Keys Reference Doc</a>
        </div>  
  </div>
</div>

---

### KV Data Store

- Used by the VPP agent to store VPP/Linux configuration and stats
- Watches and reacts to events created by changes within the datastore
- Uses a watched key value and microservice label to distinguish its own config from that of another VPP
- Microservice label enables a single datastore to service multiple VPP agents

{{< figure src="/images/ligato/components-KVDB_microservice_label.png" class="image-center figcaption"caption="" >}}

<br/>


<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/user-guide/concepts/#key-value-data-store"class="button btn-align-md secondary-btn raised">KV Data Store Doc</a>
                        </div>
</div>

---

### KV Scheduler

The runtime configuration  of a CNF must accurately reflect the desired network function. One configuration item could be dependent on the successful configuration of another item. Example: a route cannot be configured without an interface, therefore we say the interface is a dependency configuration item of the route. And these dependencies must be tracked and coordinated to ensure the proper configure sequence.

<br/> 

{{< figure src="/images/ligato/components-ligato-framework-arch-KVS2.svg" class="image-center figcaption"caption="" >}}


The KV Scheduler is plugin that works with VPP and Linux agents on the SB side, and orchestrators/external data sources such as KV data stores, and RPC clients on the NB side. In a nutshell, it keeps track of the correct configuration order and associated dependencies.

<br/>

<div class="columns">
    <div class="column is-half">
        <div style="text-align: center">
            <a href="https://docs.ligato.io/en/latest/plugins/kvs-plugin/"class="button btn-align-md secondary-btn raised">KV Scheduler Plugin Doc</a>
       </div> 
  </div>    
  <div class="column">
        <div style="text-align: center">
          <a href="https://docs.ligato.io/en/latest/developer-guide/kvscheduler/"class="button btn-align-md secondary-btn raised">KV Scheduler Development Guide</a>
        </div>  
  </div>
</div>

---

### Agentctl

Agenctl is a CLI command line tool for managing and interacting with the software components of the Ligato framework. The CLI that enables the developer or operator to check status, control VPP, view models, configure logs, gather metrics, and includes commands for a select or complete system dump.

{{< figure src="/images/ligato/agentctl.svg" class="image-center figcaption"caption="Agentctl" >}}

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/user-guide/agentctl/"class="button btn-align-md secondary-btn raised">Agentctl Doc</a>
                        </div>
</div>

---




