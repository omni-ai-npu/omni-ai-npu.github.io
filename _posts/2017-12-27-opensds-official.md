---
layout: post
title: Make Storage Simple Again with OpenSDS
---

*OpenSDS is a new collaborative open source community under Linux Foundation established by the end of 2016. Throughout year 2017 the core dev team has been working arduously on prototyping various exciting new ideas and also delivering the first beta release. Now we are proud to anounce the first release coming out of the OpenSDS community, and this blog post aims to provide a detail run down of the OpenSDS design and functionality*

### Background

#### Motivation

OPNFV community was established on the last day of September last year. It is a new type of open source project that provides
an integration platform with existing open source projects fulfilling different functionalities, of which contains NFV related 
feature enhancements.

From a background of ETSI NFV standard rapporteur, I was hyped about the new opportunities brought by the new OPNFV adventure. 
When searching for ideas, one thing came into my mind is that although we have defined a rich content of NFV descriptors in ETSI,
if I want to run these type of descriptors intuitively on OPNFV platform to have my NFV service working, I simply cannot do that.
In the early MANO GS, and later on specs, [OASIS TOSCA](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=tosca) and [YANG](https://tools.ietf.org/html/rfc6020) are the most commonly used format. There are also cases that people might use [Apache Brooklyn](https://brooklyn.apache.org/) as an NFV Orchestrator and therefore [OASIS CAMP](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=camp) will be used to format the descriptor, and this, also cannot be understand by OPNFV platform, which integrates OpenStack that uses Heat for IaaS Orchestration.

#### Community

### OpenSDS Zealand Release

#### Architecture Overview

#### Hotpot Project - OpenSDS Controller

#### Sushi Project - OpenSDS Northbound Plugin

### Service Oriented Storage Orchestration

#### Kubernetes Integration with OpenSDS

#### OpenStack integration with OpenSDS

#### More on Service Broker

### The Future
