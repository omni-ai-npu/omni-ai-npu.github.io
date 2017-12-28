---
layout: post
title: Make Storage Simple Again With OpenSDS
---

*[OpenSDS](https://opensds.io) is a new collaborative open source community under Linux Foundation established by the end of 2016. Throughout year 2017 the core dev team has been working arduously on prototyping various exciting new ideas. Now we are proud to announce the [beta release](https://github.com/opensds/opensds/tree/stable/zealand) coming out of the OpenSDS community, and this blog post aims to provide a detail run down of the OpenSDS design and functionality*

*Huge thanks to our dev team Leon Wang, Xing Yang, Edison Xiang, Erik Xu and other members to make this release happen*

# Background

## Motivation

We've adopted the slogan **"Make Storage Simple Again"** for OpenSDS promotion throughout this year, and this phrase actually speaks the core motivation behind this new open source project.

Storage, similar to network, has been an industry driven by great vendor products and open source solutions designed by veterans and professionals. It used to be plug-n-play : plug in the storage solution you have and then you can start to use it. 

However with the advent of cloud computing and later cloud native computing, the adaptation of storage solutions in cloud era has become more and more difficult. 

For traditional storage products, any user will face the problem of choosing one cloud computing platform (OpenStack, Kubernetes, Mesos, Docker, ...) and then choose the storage that platform could support (hundreds of mal-managed drivers). Any storage solution provider will also face the problem of developing endless plugins/drivers/what-have-you for more and more state-of-the-art cloud computing platforms.

For open source projects like Ceph, the provisioning and automation at a reasonable scale is still a daunting task even for users who are technical savvy enough.

But we just want a place to store stuff, could we make it a bit simpler instead of navigating a maze of compatibility and usability ?

## Community

The OpenSDS community was born out of the motivation described above with the sole goal of consolidating storage industry efforts to provide a simple solution for the user. OpenSDS community governance currently consists of **Technical Steering Committee (TSC)** and **End User Advisory Committee (EUAC)**, which consists of representatives from member companies like Intel, IBM, Hitachi, Huawei, Vodafone, Yahoo Japan and NTT. We are looking forward to having more members joining in 2018 and also establishing the official board to handle larger scale community operation. 

It should be noted that it is somewhat extraordinary for a new communitiy to have structure like EUAC in such early stage. This is actually one distinguishing point about OpenSDS: we care about user requirements. We don't want to be an open source project that delivers something only developers could understand. We want the user participate and feedback every step of the way of the development process, and this enablement of user voice differentiates OpenSDS from most of its peer projects in my opinion.

## Uniqueness

OpenSDS is **not just yet another controller project** that will be jacked into the current cloud computing stack, or dumped to replace any existing great open source solution which already has a good ecosystem just because *there is a group of people* think it is somehow more awesome than others. 

OpenSDS community always positions itself as a complimentary effort to the existing ecosystem, and values collaboration over blind meaningless competition (typical *my stuff is better thus should replace your stuff* nonsense)

As will be shown in the following sections, OpenSDS provides a unique value for storage management in cloud computing and explores new exciting possibilities that no one has tried before (at least in the open source field)

<img src="https://github.com/hannibalhuang/hannibalhuang.github.io/raw/master/image/boldly%20go.jpg" width="500" height="300">

# OpenSDS Zealand Release

The Zealand Release is the beta release of OpenSDS software (yes this is a beta release, not official first release). OpenSDS provides a policy driven orchestration platform which provides 
* a **unified management entry point for user**
* an **aggregation view of heterogeneous storage resources**
* an **automated provisioning engine**
* a **standardized layered architecture that could ease the storage adaptation in cloud native era**

## Architecture Overview

![OpenSDS Zealand Release Architecture](https://github.com/opensds/opensds/raw/master/architecture.png)

As shown in the above figure, OpenSDS software consists of two main components: **Controller** and **NBP (North Bound Plugin)**, both of which adopt a **layered architecture** that standardize the management workflow. We will go into the details of the two components in the following subsections:

### Hotpot - OpenSDS Controller Project

Sitting at the core of OpenSDS architecture is the OpenSDS controller component developed by the [Hotpot](https://github.com/opensds/opensds) project team. OpenSDS controller project adopted a microservice and layered architecture. It got three submodules: controller, hub and db.

### *Controller Module*

The controller module has a layered architecture: API layer, Orchestration layer, Controll layer

#### API layer

[OpenSDS API](https://github.com/opensds/opensds/blob/master/openapi-spec/swagger.yaml) provides the entry point for users. In Zealand release, other than the basic block storage management operations, OpenSDS offers a set of additional apis for users to consume:

<img src="https://github.com/hannibalhuang/hannibalhuang.github.io/raw/master/image/opensds-api-swagger.PNG" width="500" height="300">

As shown in the above figure, OpenSDS supports operation on storage pool level, OpenSDS Dock and OpenSDS profile. Storage pool is an abstraction that admin could define. Profile is another OpenSDS feature that we want the controller to be profile/policy driven so that user could have a declarative way of requiring storage resource. We will cover profile and dock in later sections in detail.

#### Orchestration layer

There are two components in this layer: *Selector* and *Policy Controller*. Selector provides an interface for policy enforcement (the enforcement is carried out by the *filter*). Policy Controller provides an interface for two functionalities: *Register* (carried out by *storagetag* function for admin to tag storage capabilities) and *Executor* (for taskflow operation). Therefore the reader could see that the orchestration layer basically provides the policy driven automation semantics for OpenSDS. 

Moreover, OpenSDS also could connect to external **policy engines** like [OPA](https://github.com/open-policy-agent/opa). If you are interested please check out our [experiment](https://github.com/opensds/opensds/tree/opa-scheduler) which has not been included in Zealand release.

#### Control layer 

The functionality of this layer is carried out by the *volumecontroller* function which interacts with the hub to enforce the decisions made by the controller module.

### *Hub Module*

The hub module also has a layered architecture: dock layer and driver layer.

#### Dock layer

The Dock provides a unified southbound abstraction layer for all the underlying storage resources. It interacts with controller module via gRPC which provides a nice decoupling feature. In essence the user could deploy OpenSDS dock on any number of storage nodes where OpenSDS controller module deployed in the master node. This microservice architecture enables OpenSDS to avoid the general pitfall of a centralized controller that it could not be distributed or scaled out very good.

The usage of gRPC also enables OpenSDS Dock to add many southbound resource type as it see fit. For example we have experiments with [CSI](https://github.com/opensds/opensds/tree/csi-driver) and [Swordfish](https://github.com/opensds/opensds/tree/swordfish_implementation) southbound supports by adding their protobuf based data models to the Dock. The CSI southbound support experiment is especially interesting because it positions OpenSDS as a **SO (Storage Orchestrator)** in addition to the CO (container orchestrator) concept defined in the current CSI spec. *（Be noted that both CSI and Swordfish southbound implementation are not included in Zealand release for its experiment nature.）*

<img src="https://github.com/hannibalhuang/hannibalhuang.github.io/raw/master/image/opensds-csi-sb-impact.PNG" width="300" height="200">

#### Driver layer

All of the storage resource drivers could be found at this layer. For Zealand release OpenSDS natively provides the drivers for [LVM](https://github.com/opensds/opensds/wiki/Local-Cluster-Installation-with-LVM), [Ceph](https://github.com/opensds/opensds/wiki/Local-Cluster-Installation-with-Ceph) and [Cinder](https://github.com/opensds/opensds/wiki/Local-Cluster-Installation-with-Cinder-Standalone). Drivers from storage vendor product are more than welcomed.

### *DB Module*

The DB module is designed to be pluggable and our default option is to use [ETCD](https://github.com/coreos/etcd). ETCD will store all the OpenSDS cluster config and state information.

### Sushi - OpenSDS Northbound Plugin Project

OpenSDS [Sushi](https://github.com/opensds/nbp) project also has a layered architecture: plugin layer and client layer.

#### Plugin layer 

Provides the Flex plugin and external storage provisioner for OpenSDS to be used as a southbound to Kubernetes. Sushi also provides the plugin for CSI alpha version in Kubernetes 1.9 and service broker which is used for integration with Kubernetes Service Catalog. It also provides the functionality of attach/detach/ for iscsi and rbd in Zealand release.

#### Client layer 

Provides opensds client and serves as a SDK for interacting with the OpenSDS Controller.

### Play Around With OpenSDS Zealand

Please checkout detail procedures for playing around OpenSDS from the [wiki](https://github.com/opensds/opensds/wiki). In a word OpenSDS could be deployed via Ansible and also available in docker format.

## Service Oriented Storage Orchestration

One of the new features we experimented with OpenSDS is to adopt a new service oriented storage architecture which could be illustrated as follows:

<img src="https://github.com/hannibalhuang/hannibalhuang.github.io/raw/master/image/opensds-serive-arch.PNG" width="500" height="300">

It means that in the upcoming cloud native computing era, in order to make storage management and offering more simple to consume, storage will be ultimately offered as a service to the end user. The user should no longer concern about any detailed storage knowledge other than the general storage need for its application. Through the interaction layer (which could be an AI bot), the user just request a storage service for its app to consume, and the service layer will interact with the orchestration layer to handle all the details, thus shield user away from the storage domain specific knowledge.

[Open Service Broker API](https://github.com/openservicebrokerapi/servicebroker), [Kubernetes Service Catalog](https://github.com/kubernetes-incubator/service-catalog) and OpenSDS will together play imortant roles in this transformation.

### Kubernetes Service Catalog Integration with OpenSDS

One thing we are excited about OpenSDS is that it provides the possibility of a new way of integration with the Kubernetes platform: via Service Catalog. We could always just arbitrarily write a new CRD controller for OpenSDS, however it is time consuming and potentially wheel-reinventing. The reason why we choose Service Catalog is that as a incubated project, it has already been in a great shape and a great community. Moreover the service broker semantic of service class and service plan fits well into the profile driven architecture that OpenSDS has adopted.

Once user chooses a storage service plan via Kubernetes Service Catalog, the plan will launch a service instance which will materialize a service class that could be translated to a storage profile by OpenSDS Service Boker. Then OpenSDS through nbp consume the storage profile outlined by the serice class and automatically setup, prepare and provision the storage resources without any involvement of the user. Then the user bind the storage information provided by OpenSDS to the pod via **pod-reset** and then it just spawn up the application cluster as usual.

In this way we provide a nice out-of-band storage service for Kubernetes users, and if you combined with the CSI southbound support, you could have a really nice architecture where you don't have a long storage control path and all the "XXX-set" you have to learn.

<img src="https://github.com/hannibalhuang/hannibalhuang.github.io/raw/master/image/opensds-csi-sb-arch.PNG" width="500" height="300">

Detailed procedure on how you could play OpenSDS with Service Catalog is [here](https://github.com/opensds/nbp/blob/master/service-broker/INSTALL.md)

### More on Service Broker

OpenSDS team has a strong intention to work with the OSB API community in the future to forging the way of storage service. As a matter of fact OpenSDS service broker implementation has been documented as one of the [official use cases](https://github.com/openservicebrokerapi/servicebroker/blob/master/gettingStarted.md#go)

## Cross Community Efforts

* [OPNFV Stor4NFV project](https://wiki.opnfv.org/display/STOR/Stor4NFV+Architecture)

# The Future

OpenSDS team is looking forward to have the first official release Aruba next year. We welcome developers to join us in this exciting venture and also users to provide feedback in our development cycle.

We will look for helps on monitoring and tracing components for OpenSDS. We will also be looking at adding file and object interface for OpenSDS controller. Machine learning is another aspect we will actively investigate in 2018. More detailed roadmap for controller project could be found [here](https://github.com/opensds/opensds/wiki/OpenSDS-Controller-Project-Roadmap)

Please feel free to contact our team via the following methods:
* [Slack](https://opensds.slack.com)
* [Google Group](https://groups.google.com/forum/#!forum/opensds-dev) for offline discussion as well as weekly team meeting information
* Fork OpenSDS repo and [submit](https://github.com/opensds/opensds/wiki/OpenSDS-Controller-Project-Contribution-Tutorial) a PR
* DM me on [twitter](https://twitter.com/nopainkiller)

