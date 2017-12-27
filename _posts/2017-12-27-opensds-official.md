---
layout: post
title: Make Storage Simple Again with OpenSDS
---

*[OpenSDS](https://opensds.io) is a new collaborative open source community under Linux Foundation established by the end of 2016. Throughout year 2017 the core dev team has been working arduously on prototyping various exciting new ideas and also delivering the first beta release. Now we are proud to anounce the first release coming out of the OpenSDS community, and this blog post aims to provide a detail run down of the OpenSDS design and functionality*

### Background

#### Motivation

We've adopted the slogan "Make Storage Simple Again" for OpenSDS promotion throughout this year, and this phrase actually speaks the core motivation behind this new open source project.

Storage, similar to network, has been an industry driven by great vendor products and open source solutions designed by vetarans and professionals. However with the advent of cloud computing and later cloud native computing, the adoptation of storage solutions in cloud era has become more and more difficult. Any user will face the problem of choosing one cloud computing platform (OpenStack, Kubernetes, Mesos, Docker, ...) and then choose the storage that platform could support (hundreds of mal-managed drivers). Any storage solution provider will also face the problem of developing endless plugins/drivers/what-have-you for more and more state-of-the-art cloud computing platforms.

But we just want a place to store stuff, could we make it a bit simpler instead of navigating a maze of compatibility and usability ?

#### Community

The OpenSDS community was born out of the motivation described above with the sole goal of consolidating storage industry to provide a simple solution for the user. OpenSDS community governance currently consists of Technical Steering Committee and End User Advisory Commitee, of which consists of representatives from member companies like Intel, IBM, Hitachi, Huawei, Vodafone, Yahoo Japan and NTT respectively. We are looking forward to having more members joining in 2018 and also establishing the official board to handle larger scale community operation. 

It should be noted  it is somewhat extraordinary for a new communtiy to have EUAC in such early stage. This is actually one distinguishing point about OpenSDS: we care about user requirements. We don't want to be another open source project that deliver something only developers understand. We want the user participate and feedback every step of the way of the development process, and this enablement of user voice differentiates OpenSDS from most of its peer projects IMHO.

### OpenSDS Zealand Release

#### Architecture Overview

#### Hotpot Project - OpenSDS Controller

#### Sushi Project - OpenSDS Northbound Plugin

### Service Oriented Storage Orchestration

#### Kubernetes Integration with OpenSDS

#### OpenStack integration with OpenSDS

#### More on Service Broker

### The Future
