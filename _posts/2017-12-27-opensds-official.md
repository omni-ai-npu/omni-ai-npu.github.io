---
layout: post
title: Make Storage Simple Again with OpenSDS
---

*[OpenSDS](https://opensds.io) is a new collaborative open source community under Linux Foundation established by the end of 2016. Throughout year 2017 the core dev team has been working arduously on prototyping various exciting new ideas and also delivering the first beta release. Now we are proud to anounce the beta release coming out of the OpenSDS community, and this blog post aims to provide a detail run down of the OpenSDS design and functionality*

### Background

#### Motivation

We've adopted the slogan **"Make Storage Simple Again"** for OpenSDS promotion throughout this year, and this phrase actually speaks the core motivation behind this new open source project.

Storage, similar to network, has been an industry driven by great vendor products and open source solutions designed by vetarans and professionals. However with the advent of cloud computing and later cloud native computing, the adoptation of storage solutions in cloud era has become more and more difficult. Any user will face the problem of choosing one cloud computing platform (OpenStack, Kubernetes, Mesos, Docker, ...) and then choose the storage that platform could support (hundreds of mal-managed drivers). Any storage solution provider will also face the problem of developing endless plugins/drivers/what-have-you for more and more state-of-the-art cloud computing platforms.

But we just want a place to store stuff, could we make it a bit simpler instead of navigating a maze of compatibility and usability ?

#### Community

The OpenSDS community was born out of the motivation described above with the sole goal of consolidating storage industry efforts to provide a simple solution for the user. OpenSDS community governance currently consists of **Technical Steering Committee (TSC)** and **End User Advisory Commitee (EUAC)**, which consists of representatives from member companies like Intel, IBM, Hitachi, Huawei, Vodafone, Yahoo Japan and NTT. We are looking forward to having more members joining in 2018 and also establishing the official board to handle larger scale community operation. 

It should be noted that it is somewhat extraordinary for a new communtiy to have structure like EUAC in such early stage. This is actually one distinguishing point about OpenSDS: we care about user requirements. We don't want to be yet another open source project that delivers something only developers understand. We want the user participate and feedback every step of the way of the development process, and this enablement of user voice differentiates OpenSDS from most of its peer projects in my opinion.

#### Uniqueness

OpenSDS is **not just yet another controller project** that will be jacked into the current cloud computing stack, or dumped to replace many existing great open source solution which already has a good ecosystem just because *we* think it is somehow more awesome than others.

As will be shown in the following sections, OpenSDS provides a unique value for storage management in cloud computing and explores new exciting possibilities that no one has tried before (at least in the open source field)

<img src="https://github.com/hannibalhuang/hannibalhuang.github.io/raw/master/image/boldly%20go.jpg" width="600" height="400">

### OpenSDS Zealand Release

The Zealand Release is the beta release of OpenSDS software (yes this is a beta release, not official first release). OpenSDS provides a policy driven orchestration platform which provides a **unified management entry point for user**, an **aggregation view of hetrogeneous storage resources**, as well as an **automated provision engine**.

#### Architecture Overview

![OpenSDS Zealand Release Architecture](https://github.com/opensds/opensds/raw/master/architecture.png)

As shown in the above figure, OpenSDS software consists of two main components: **Controller** and **NBP (North Bound Plugin)**. We will go into the details of the two components in the following subsections:

#### Hotpot - OpenSDS Controller Project

Sitting at the core of OpenSDS architecture is the OpenSDS controller component developed by the Hotpot project team. OpenSDS controller project adopted a microservice architecture and got three submodules: controller, hub and db.

#### *Controller Module*

The controller module has a layered architecture: API layer, Orchestration layer, Controll layer

* API layer: [OpenSDS API](https://github.com/opensds/opensds/blob/master/openapi-spec/swagger.yaml) provides the entry point for users. In Zealand release, other than the basic block storage managament operations, OpenSDS offers a set of additional apis for users to consume:

<img src="https://github.com/hannibalhuang/hannibalhuang.github.io/raw/master/image/opensds-api-swagger.PNG" width="600" height="500">

As shown in the above figure, OpenSDS supports operation on storage pool level, OpenSDS Dock and OpenSDS profile. Storage pool is an abstraction that admin could define. Profile is another OpenSDS feature that we want the controller to be profile/policy driven so that user could have a declarative way of requiring storage resource. We will cover profile and dock in later sections in detail.

* Orchestration layer: there are two components in this layer: *Selector* and *Policy Controller*. Selector provides an interface for policy enforcement (the enforcement is carried out by the *filter*). Policy Controller provides an interface for two functionalities: *Register* (carried out by *storagetag* function for admin to tag storage capabilities) and *Executor* (for taskflow operation). Therefore the reader could see that the orchestration layer basically provides the policy driven automation semantics for OpenSDS. 

Moreover, OpenSDS also could connect to external **policy engines** like **[OPA]**(https://github.com/open-policy-agent/opa). If you are interested please check out our [experiment](https://github.com/opensds/opensds/tree/opa-scheduler) which has not been included in Zealand release.

* Control layer: the functionality of this layer is carried out by the *volumecontroller* function which interacts with the hub to enforce the decisions made by the controller module.

#### *Hub Module*

The hub module also has a layered architecture: dock layer and driver layer.

* Dock layer: The Dock provides a unified southbound abstraction layer for all the underlying storage resources. It interacts with controller module via gRPC which provides a nice decoupling feature. In essence the user could deploy OpenSDS dock on any number of storage nodes where OpenSDS controller module deployed in the master node. This microservice architecture enables OpenSDS to avoid the general pitfall of a centralized controller that it could not be distributed or scaled out very good.

The usage of gRPC also enables OpenSDS Dock to add many southbound resource type as it see fit. For example we have experiments with [CSI](https://github.com/opensds/opensds/tree/csi-driver) and [Swordfish](https://github.com/opensds/opensds/tree/swordfish_implementation) southbound supports by adding their protobuf based data models to the Dock. The CSI southbound support experiment is especially interesting because it positions OpenSDS as a **SO (Storage Orchestrator)** in addtion to the CO (container orchestrator) concept defined in the current CSI spec.

<img src="https://github.com/hannibalhuang/hannibalhuang.github.io/raw/master/image/opensds-csi-sb-impact.PNG" width="500" height="300">

* Driver layer: All of the storage resource drivers could be found at this layer. For Zealand release OpenSDS natively provides the drivers for [LVM](https://github.com/opensds/opensds/wiki/Local-Cluster-Installation-with-LVM), [Ceph](https://github.com/opensds/opensds/wiki/Local-Cluster-Installation-with-Ceph) and [Cinder](https://github.com/opensds/opensds/wiki/Local-Cluster-Installation-with-Cinder-Standalone). Drivers from storage vendor product are more than welcomed.

#### Sushi - OpenSDS Northbound Plugin Project



### Service Oriented Storage Orchestration

#### Kubernetes Integration with OpenSDS

#### OpenStack integration with OpenSDS

#### More on Service Broker

### The Future
