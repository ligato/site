---
title: "What is a CNF?"
date: 2019-04-23T14:25:06-07:00
layout: "cnf"
draft: false
---

## Problem to Solve

* Network functions (NF) are physical or virtual devices that process packets supporting a network and/or application service. Performance, features, scale and operational control are paramount. Routers, campus switches and firewalls are example NFs.
* VM-based virtual network functions (VNF) are available, mature, deployable, manageable and service-enabled.
* Cloud Native is _new_; NFs implemented for and running in Cloud Native networks is _new_.
* Container networking needs high-performance network functions (NFs) such as NAT, Firewalls, VPN gateways, load balancers, IPS, DPI, L2/L3 forwarding and more.
* Reasons why they are needed: Fast, flexible, elastic and resilient container orchestration; For spinning up advanced communication services, composed of NF service chains; Rapid development and introduction of new network and services innovations to cloud native environments.
* What about Kubernetes container network interface (CNI)? Not ideal for this purpose; Only provides simple application pod networking
* Kernel network stack currently used for VM and container networks can hinder throughput for some applications

A solution for building and operating NFs for cloud native networks must exhibit the following:

* Foster developer productivity with modern toolsets, standard/customized APIs, documentation, libraries, 12-factor, modularity, open source and cloud native project integration, large and engaged community.
* Adhere to K8s lifecycle management.
* Observability through logging, tracing, telemetry, diagnostic/troubleshooting APIs and CLI.
* Feature rich, programmable and extensible for control plane, data plane and customization
* High-performance user space data plane

---

## Cloud Native Network Functions (CNF) is the Solution

A cloud native network function (CNF) is a VNF designed and implemented to run inside containers. CNFs inherit all cloud native architectural and operational principles including, 12-factor app, immutability and K8s lifecycle management.

CNF advantages include:

* __Lifecycle management parity with application containers.__ Existing best practicies applied to application microservice containers related to development, CI/CD, K8s orchestration and scheduling, distributed management, telemetry collection are available to CNFs.

* __Stateless Configuration.__ CNFs operate in container pods. Pods are durable, but if something breaks or new functions are needed, then Pods can be torn down and a new instance started up. The dynamic nature of cloud native environments precludes centralized or stateful CNF config management.  Instead, a stateless approach is incorporated where CNFs “watch” for and then process config updates stored in an external data store.

* __Smaller footprint vis-a-vis physical or virtual devices.__ This reduces NF resource consumption, with the savings allocated to applications.

* __Rapid development, innovation and immutability.__ CNF functions should run in user space where new features and innovations can be applied. There is no need to modify the linux kernel; It becomes immutable.

* __Performance.__ Bypassing the Linux kernel has shown to increase performance. New CNFs are being developed to run in user space.

---

## CNFs are Multi-Purpose

CNF are built and implemented with functions suited for their role in a cloud native network. For illustrative purposes, the figure below depicts several different scenarios.

<div class="tile is-ancestor">
    <div class="tile is-parent">
        <article class="tile is-child box">
            <p class="title">Load Balancer</p>
            <p class="subtitle">Ingress Controller/K8s Services</p>
            <figure class="image is-4by3">
                <img src="/images/ligato/cnf-LB-example.svg">
            </figure>
        </article>
    </div>
    <div class="tile is-parent">
        <article class="tile is-child box">
            <p class="title">Cluster Networking</p>
            <p class="subtitle">Improved CNI / NSM Pipes</p>
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

* Scaling load balancing functions for steering traffic towards application pods
* Enhanced cluster networking can be achieved using a CNF vswitch for policy/service-aware switching (CNI-formed L2 bridge domain as an example), or as an x-connect for network service mesh (NSM) L2/L3 pipes setup between pods
* Service Function Chains orchestrated to deliver different application services to customers. Examples show a cloud storage service on top, and a machine learning training application on the bottom.


---

## CNF Pods in a Kubernetes Cluster


<div>
<figure class="image is-1by1">
  <img src="/images/ligato/cnf-in-k8s-cluster.svg">
</figure>
</div>

<div class="tile is-ancestor">
  <div class="tile">
    <!-- Add content or other tiles -->
  </div>
  <div class="tile">
    <!-- Add content or other tiles -->
  </div>
</div>

---

## Composite CNF Architecture

<div class="tile is-ancestor">
    <div class="tile is-12">
        <div class="tile">
            <div class="tile is-parent is-8">
                <article class="tile is-child box">
                    <p class="title"></p>
                    <figure class="image is-4by3">
                        <img src="/images/ligato/cnf-generic-arch.svg">
                    </figure>
                 </article>
            </div>
            <div class="tile is-parent is-vertical is-4">
                <article class="tile is-child box markdown-body">
                <div class="is-size-6 text">
                    <p>Developers make design choices tailored for specific implementations. Simplifying the options to meet expectations requires flexible and adaptable focused on performance, flexibility, programmability and scale allows one to paint a picture of a composite CNF architecture.</p>
                    <ul>
                        <li>User space data plane for best performance</li>
                        <li>Control plane agent contains southbound, low-level API for data plane programming, plugins to add new features and and generate northbound APIs for application and orchestrator control</li>
                        <li>Custom plugin and application integration component integrations</li>
                        <li>Infra plugins for logging, tracing, internal APIs, health checks</li>
                        <li>Plugins/APIs for external application access</li>
                    </ul>
                    </div>
                </article>
             </div>
        </div>
    </div>
</div>




