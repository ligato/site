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
                            <a href="https://docs.ligato.io/en/latest/intro/framework/"class="button is-success">More about the Ligato Framework</a>
                        </div>
</div>



- test

- blah

---

### Plugins

Component-based architecture (CBA) have become fundamental in software development, engineering and operations. System platforms are decomposed into smaller components that can be "wired up" into a complete solution. React and Go are two popular examples illustrating this trend.

Ligato embraces CBA by providing a set of plugins for CNF configuration and telemetry, and cloud-native infrastructure lifecyclement management, APIs, health checking and data store/database integration.

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/plugins/plugin-overview/"class="button is-success">More on Ligato Plugins</a>
                        </div>
</div>


---

### VPP Agent plugins

some text on VPP agent

{{< figure src="/images/ligato/ligato-framework-vpp-agent2-picture.svg" class="image-center figcaption"caption="VPP Agent Southbound Plugins" >}}



<div class="tile is-ancestor">
    <div class="tile is-12">
        <div class="tile">
            <div class="tile is-parent is-7">
                <article class="tile is-child box">
                    <p class="title">VPP Agent Plugins</p>
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
├── natplugin
├── puntplugin
├── srplugin
├── stnplugin
                           </code>
                        </pre>
                    </div>
                </article>
            </div>
            <div class="tile is-parent is-vertical is-5">
                <article class="tile is-child box">
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

some text on ... other plugins

{{< figure src="/images/ligato/ligato-framework-arch-infra.svg" class="image-center figcaption"caption="Infra/Connection/DB plugins" >}}




<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/plugins/plugin-overview/"class="button is-success">More on Ligato Plugins</a>
                        </div>
</div>




---

### Models / Protobufs / Keys

A fundamental concept of Ligato are the notions of a model combined with a protobuf definition to identify, manage, and generate NB APIs. 

{{< figure src="/images/ligato/components-model-proto-KV-store.svg" class="image-center figcaption"caption="Model-protoBuf-KV-data-store" >}}

---

#### Models

The model represents an abstraction of an object that can be managed through northbound APIs exposed by the VPP agent. It consists of a model specification and a proto.Message.

Here is a snippet of model.Spec code for a VPP L3 route. 

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

some text on keys ...

---

### KV Data Store

some text on data stores

---

### KV Scheduler

The runtime configuration  of a CNF must accurately reflect the desired network function. One configuration item could be dependent on the successful configuration of another item. Example: a route cannot be configured without an interface, therefore we say the interface is a dependency configuration item of the route. And these dependencies must be tracked and coordinated to ensure the proper configure sequence. 

{{< figure src="/images/ligato/ligato-framework-arch-KVS2.svg" class="image-center figcaption"caption="KV Scheduler" >}}

<div style="padding-top: 50px">
                        <div style="text-align: center">
                            <a href="https://docs.ligato.io/en/latest/intro/framework/#kv-scheduler"class="button is-success">More about the KV Scheduler</a>
                        </div>
</div>

The KV scheduler is plugin that works with VPP and Linux agents on the SB side, and orchestrators/external data sources such as KV data stores and RPC clients on the NB side. In a nutshell, it keeps track of the correct configuration order and associated dependencies.

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


