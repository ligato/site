---
title: "Building Framework"
date: 2019-04-16T05:23:30-07:00
layout: "arch"
draft: false
---




### Overview

Ligato is an open source Go framework for building and developing solutions for managing and controlling CNFs. Reusable software components, model/protobuf-modelled APIs, dependency injection, and logging/REST/CLI visibility are provided. Documentation, tutorials and examples are supplied to facilitate development. 

A set of VPP and Linux plugins, including a transaction-based configuration scheduler, are implemented in the VPP agent enabling agile and resilient deployments. Infrastructure plugins offer K8s lifecycle management, health care checking, notifications, data store/database integration, and logging for cloud-native operational affinities.  
  
 Ligato is written in Golang as its programming language for the performance, concurrency, scale, readability, and broad community support necessary for developing and implementing cloud-native network solutions. 

<br />

{{< figure src="/images/ligato/ligato-framework-arch.svg" class="image-center figcaption"caption="Ligato Framework" >}}

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/intro/framework/"class="button is-success">More on the Ligato Framework</a>
                        </div>
</div>


---

### Plugins

The VPP agent and cn-infra are the two constituent frameworks that, together, form the basis of the Ligato framework. With Ligato, each management or control plane applicaton utilize one or modules called plugins. Each plugin supports a specific function or functions. Some plugins come with the cn-infra framework; Others come with the VPP agent; Yet others created by app developers perform custom tasks.

Plugins can be assembled in any combination to build solutions ranging from simple configuration tasks to larger more complex operations such as managing configuration state across multiple nodes in a network. The plugin definition is standardized in the Ligato framework, and can be easily extended with customized plugins to create new solutions and applications.




<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/plugins/plugin-overview/"class="button is-success">More on Ligato Plugins</a>
                        </div>
</div>


---

### VPP Agent

The VPP agent provides configuration and monitoring services for the VPP data plane.

* Supplies VPP-specific plugins
* VPP agent and VPP packaged up in single container
* Dependency handling between related configuration items
* Transaction-based configuration processing and tracking
* Failover synchronization mechanisms, also known as resync
* Stateless configuration management based on a KV data store "watch" paradigm
* Direct access via REST or gRPC
* Component health checks

---

### VPP Agent Plugins

The VPP Agent is composed of a set of VPP-specific plugins. Used in combination with cn-infra, CNFs and apps can be developed to interact with other services/microservices in a cloud-native environment. 


{{< figure src="/images/ligato/ligato-framework-vpp-agent2-picture.svg" class="image-center figcaption"caption="VPP Agent Southbound Plugins" >}}

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
├── srplugin
├── stnplugin
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
├── ifplugin
├── iptablesplugin
├── l3plugin
└── nsplugin
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
                            <a href="https://docs.ligato.io/en/latest/plugins/vpp-plugins/"class="button is-success">More on VPP Plugins</a>
                        </div>
</div>

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/plugins/linux-plugins/"class="button is-success">More on Linux Plugins</a>
                        </div>
</div>

---

### Infra / Connection / Database Plugins

cn-infra can be decomposed into a suite of plugins supporting capabilities present in modern cloud-native CNFs and apps. Plugins offering logging, health checks, messaging, KV data store connectivity, REST and gRPC APIs are available. Developers can implement a mix of cn-infra plugins, that together with other VPP agent and/or custom plugins, define CNF functionality. cn-infra provides plugin lifecycle management such as initialization and graceful shutdown.



{{< figure src="/images/ligato/ligato-framework-arch-infra.svg" class="image-center figcaption"caption="Infra/Connection/DB plugins" >}}




<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/plugins/plugin-overview/"class="button is-success">More on Ligato Plugins</a>
                        </div>
</div>




---

### Models / Protobufs / Keys

Model abstractions, protobufs, key identifiers and KV data stores are fundamental concepts for managing configuration objects in a network. It is the application of the models and protobuf definitions that define northbound 

{{< figure src="/images/ligato/components-model-proto-KV-store2.svg" class="image-center figcaption"caption="" >}}

<br/>
In Ligato, each object is defined by a model. The model is composed of a model specification and .proto protobuf defintion. A key is generated for the model, and that key is used to read/write configuration and status information into a KV data store.

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

The is the .proto file for the VPP L3 route:  

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
        // specified as via_vrf_id field. In case of these routes, the outgoing
        // interface should not be specified. The next hop IP address
        // does not have to be specified either, in that case VPP does full
        // recursive lookup in the via_vrf_id VRF.
        INTER_VRF = 1;
        // Drops the network communication designated for specific IP address.
        DROP = 2;
    }
    RouteType type = 10;

    // VRF identifier, field required for remote client. This value should be
    // consistent with VRF ID in static route key. If it is not, value from
    // key will be preffered and this field will be overriden.
    // Non-zero VRF has to be explicitly created (see api/models/vpp/l3/vrf.proto)
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
    // Only paths with the best preference contribute to forwarding (a poor man's primary and backup).
    uint32 preference = 7;

    // Specifies VRF ID for the next hop lookup / recursive lookup
    uint32 via_vrf_id = 8;
}
        </code>
    </pre>
</div>

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/intro/framework/"class="button is-success">More on Protobufs</a>
                        </div>
</div>

---

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/intro/framework/"class="button is-success">More about the Ligato Framework</a>
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

---



---

### KV Scheduler

The runtime configuration  of a CNF must accurately reflect the desired network function. One configuration item could be dependent on the successful configuration of another item. Example: a route cannot be configured without an interface, therefore we say the interface is a dependency configuration item of the route. And these dependencies must be tracked and coordinated to ensure the proper configure sequence. 

{{< figure src="/images/ligato/ligato-framework-arch-KVS2.svg" class="image-center figcaption"caption="KV Scheduler" >}}

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/intro/framework/#kv-scheduler"class="button is-success">More about the KV Scheduler</a>
                        </div>
</div>

The KV Scheduler is plugin that works with VPP and Linux agents on the SB side, and orchestrators/external data sources such as KV data stores and RPC clients on the NB side. In a nutshell, it keeps track of the correct configuration order and associated dependencies.

---

### Agentctl

some agentctl text

{{< figure src="/images/ligato/agentctl.png" class="image-center figcaption"caption="Agentctl" >}}

---

#### Stash

- Plugins

- Models / Protobufs

- KV Data Store

- KV Scheduler

- Agentctl

- Logs




---

---



##### Data Store Connector plugins:

<div>
    <pre>
        <code>
$ $ tree cn-infra/db/keyval -U -L 1

cn-infra/db/keyval
├── bolt
├── redis
├── consul
├── etcd
└── filedb
...
        </code>
    </pre>
</div>


#### Northbound Plugins

{{< figure src="/images/ligato/components-cn-infra-northbound-plugins.svg" class="image-center figcaption"caption="Northbound Plugins" >}}


