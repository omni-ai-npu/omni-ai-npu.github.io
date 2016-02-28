---
layout: post
title: Swim in Brahmaputra With OPNFV Parser
---

*OPNFV Parser project was approved by OPNFV TSC on Feb 2015, since then the team have been discussing and developing
interesting features, and after a year of hard N' fun work, the first release of Parser will be out on March 2016 as
part of OPNFV Brahmaputra release. Here documented a brief intro about the project as well as personal experiences as
a freshmen PTL of an open source project.*

### The Project

OPNFV community was established on the last day of September last year. It is a new type of open source project that provides
an integration platform with existing open source projects fulfilling different functionalities, of which contains NFV related 
feature enhancements.

From a background of ETSI NFV standard rapporteur, I was hyped about the new opportunities brought by the new OPNFV adventure. 
When searching for ideas, one thing pop out of my eye is that although we have defined a rich content of NFV descriptors in ETSI,
if I want to run these type of descriptors intuitively on OPNFV platform to have my NFV service working, I simply cannot do that.
In the early MANO GS, and later on specs, OASIS TOSCA and YANG are the most commanly used format. There are also cases that people 
might use Apache Brooklyn as an NFV Orchestrator and therefore OASIS CAMP will be used to format the descriptor, and this, also 
cannot be understand by OPNFV platform, which integrates OpenStack that uses Heat for IaaS Orchestration.

This is where Parser proposal formed from, that a deployment template translation tool is needed for NFV operators to deploy 
their services on OPNFV in a simple and agile way. The translation output endpoint would be targeting Heat Orchestration Template,
while TOSCA serves as the intermediate template format since I found that Heat-Translator project in OpenStack already taking care
of tosca2heat translation.

I sitll remember it took two months of discussion to have Parser approved as an incubated project by the TSC, and during that review
perioud I got great comments from the community, many of which came from operators that had real work experiences. 

Parser project luckily enough has many contributors from different companies: Huawei, ZTE, HP, TCS, Chima Mobile. 
