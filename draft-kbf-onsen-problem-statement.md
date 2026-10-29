---
title: "ONSEN Problem Statement"
abbrev: "ONSEN Problem Statement"
category: info

docname: draft-kbf-onsen-problem-statement-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: Operations and Management
workgroup: ONSEN Working Group
keyword:
 - YANG
 - service abstractions
 - network automation
 - operationalization

venue:
  group: ONSEN
  type: Working Group
  mail: onsen@ietf.org
  arch: https://mailarchive.ietf.org/arch/browse/onsen/
  github: sbarguil/ONSEN_Problem_Statement
  latest: "https://sbarguil.github.io/ONSEN_Problem_Statement/draft-kbf-onsen-problem-statement.html"

author:
 -
    fullname: Samier Barguil
    organization: Nokia
    email: samier.barguil@nokia.com
 -
    fullname: Kris Lambrechts
    organization: Intwine
    email: kris@intwine.net
 -
    fullname: Chongfeng Xie
    organization: China Telecom
    email: xiechf@chinatelecom.cn

informative:
  RFC8309:
  RFC8299:
  RFC9375:
  RFC9417:
  RFC9418:
  RFC9922:
  RFC8466:
  RFC9182:
  RFC9291:
  RFC9543:
  I-D.ietf-teas-ietf-network-slice-nbi-yang:
  I-D.tt-netmod-yang-config-templates:
  RFC9408:
  RFC8969:
  RFC7950:
  RFC8199:
  RFC8342:
  RFC9834:
  NEMOPS:
    title: "IAB Workshop Report: Next Era of Network Management Operations (NEMOPS)"
    target: https://datatracker.ietf.org/doc/draft-ietf-nemops-workshop-report/
  TMF640:
    author:
      organization: TM Forum
    title: "TMF640 Service Activation and Configuration API User Guide v4.0.1"
    target: https://www.tmforum.org/resources/specifications/tmf640-service-activation-and-configuration-api-user-guide-v4-0/
  TMF641:
    author:
      organization: TM Forum
    title: "TMF641 Service Ordering Management API User Guide v4.1.1"
    target: https://www.tmforum.org/resources/specifications/tmf641-service-ordering-management-api-user-guide-v4-1-1/
  LxNM-Updates:
    author:
      organization: LxNM-Updates Contributors
    title: "LxNM-Updates"
    target: https://github.com/sbarguil/LxNM-Updates
...

--- abstract

   The IETF has published a number of YANG data models for automating
   the provisioning and delivery of network and connectivity services,
   including the L2SM, L3SM, L2NM, L3NM, Attachment Circuits, and
   Network Slicing models.  These models are intended to provide
   abstracted representations of common operator services, exposing only
   the necessary parameters for configuring and managing services.
   These abstractions help to contain the complexity of service
   provisioning within the necessary domains, making the implementation
   of complex, multi-domain services easier to implement and manage.
   However, operators have reported persistent challenges in
   operationalizing these abstractions in a consistent, scalable, and
   automatable manner.  This document describes the problem space for
   the ONSEN Working Group, identifying the operational gaps and
   deficiencies in existing IETF service and network abstraction models
   that complicate, or prevent effective end-to-end automation.  The
   problems documented here are initially drawn from operator
   experience, and from the findings of the IAB NEMOPS Workshop.  It is
   a living draft, not intended for publication as an RFC, and will be
   updated with new problem descriptions as the ONSEN work proceeds.
   This document does not propose solutions, protocols, or new data
   models.


--- middle

# Introduction

The IETF has published several YANG data models that are intended for
automating the provisioning and management of service provider
connectivity services.  These include models such as L3SM [RFC8299],
L3NM [RFC9182], L2SM [RFC8466], L2NM [RFC9291] and Service Attachment
Points (SAPs) [RFC9408].  A new YANG model for the Network Slice
Service [RFC9543] is currently being authored:
[I-D.ietf-teas-ietf-network-slice-nbi-yang].

While some of these abstractions have been deployed, operators report
persistent challenges in operationalizing them.  As highlighted by
the IAB NEMOPS Workshop [NEMOPS], these challenges are systemic and
operational in nature.  They are not confined to a specific
technology or service type, but reoccur across abstraction domains
and deployment environments.

In addition, despite the availability of numerous YANG data models -
covering configuration, assurance, and fault management - and the
ongoing effort to make these models coexist within a common framework
under the IETF umbrella, operators continue to face significant
challenges in operationalizing YANG-based service APIs in a
consistent, scalable, and interoperable manner.  While models such as
the L3SM, L2SM, L3NM, L2NM, AC/SAP abstractions and Network Slice
Service each address specific aspects of service delivery, it is not
always clear which models should be used together, in which
scenarios, or to what extent a given implementation actually supports
the full model.  The usage of these APIs remains fragmented - often
partially implemented - and difficult to automate end-to-end.  In
practice, APIs generated from similar YANG models often differ in
service semantics, and the lack of clear guidance on model
composition and interoperability complicates integration across
systems, vendors, and deployment environments.

The Operationalizing Network and SErvice abstractioNs (ONSEN) Working
Group is chartered to address this problem space by focusing on the
operational aspects of network and service abstractions.  It aims to
make it easier to implement and use the IETF's service and network
abstractions, with the goal of improving network automation,
operational efficiency, and interoperability.

This document defines the problem space for ONSEN.  It does not propose
solutions, protocols, or new data models.


# Conventions and Definitions

The following terms are used in this document:

AC:
: Attachment Circuit

Abstraction:
: The process of defining simplified, high-level constructs that
represent network and service-level capabilities, while hiding the
details of their underlying realization.  Abstraction enables
interaction between management and automation systems without
requiring direct exposure of device-specific configurations or
protocol behaviors.

BSS:
: Business Support System

LxNM:
: Layer x Network Model (L2NM or L3NM)

LxSM:
: Layer x Service Model (L2SM or L3SM)

NEMOPS:
: Next Era of Network Management Operations

ONSEN:
: Operationalizing Network and SErvice abstractioNs

OSS:
: Operation Support System

SLO:
: Service Level Objective

Service Model Intent:
: The desired service behavior expressed by an operator through a
  service model (LxSM), as described in {{RFC8309}}.  This refers to
  the set of customer requirements — connectivity, performance,
  availability — submitted to the orchestration layer and expected to
  be maintained by the network over time.  When this document uses the
  term "service intent", it refers to Service Model Intent in this
  sense.

# Problem Area Qualification Criteria

The ONSEN problem space has been divided into five main areas. These
areas were chosen to enable work in each area to advance independently
as much as possible.

- Missing LxSM and LxNM functionality: Minor omissions or extensions to
the existing LxNM and LxSM models that can be resolved with
backward-compatible updates.

- Structural LxNM/LxSM Problems: More significant structural problems with
the LxNM and/or LxSM models that may require deeper changes.

- Limited observability and service assurance: Absence of a standardized
approach and the relevant model constructs for reporting operational
state and service health.

- OSS/BSS interface and API interoperability: Problems related to the
interworking between YANG-based service models and the northbound APIs
exposed by orchestrators toward OSS/BSS systems.

- Insufficient guidance for YANG-based orchestration: Problems arising
from the lack of concrete guidance or examples on how to deploy and
combine the LxNM and LxSM models in practice.

Finally, the Items for Future Study section captures problem areas that
are recognized by the WG but are not sufficiently developed or
prioritized for inclusion in the current work.

#  Missing LxSM and LxNM Functionality

Operators who use the LxSM and LxNM models in real-world deployments
report that the models do not provide the constructs needed to
express the full range of service behaviors and operational
requirements. This section lists a number of YANG backward compatible
enhancements to these models.

## LxNM Updates

The operator-driven LxNM updates effort [LxNM-Updates] captures
concrete update requests and implementation feedback.

## On-Demand and Scheduled Service Operations

Existing YANG service and network models are primarily designed for
static, long-lived services.  They do not provide the constructs
needed to express service behaviors that vary over time, are
triggered on demand, or follow a defined schedule.  As operators
introduce services such as data-intensive workload transmission or
SD-WAN-like dynamic VPN capabilities, this gap becomes a concrete
operational blocker.

[RFC9922] defines a generic model for scheduling that could be bound
to LxSM service instances.  ONSEN can leverage this model to define
how dynamic scheduling can be applied to the LxSM service models.

# Structural LxNM/LxSM Problems

Both during the NEMOPS workshop and as part of the ONSEN working group
formation BoF, operators have reported various difficulties which
they experienced in implementing and using these models.

The RFC 8969 framework [RFC8969] proposes a hierarchical three-layer
approach: Service, Network, and Device, intended to promote reusability
and prevent feature duplication across management layers. Operators who
followed this hierarchical approach and elected to deploy both LxSM and
LxNM together did so in alignment with that architecture. They report
challenges in integrating the two models effectively, due to differences
in parameter naming, data types, and the lack of guidance on integration
logic.

In the absence of standardized integration guidance, operators have
resolved these challenges by implementing bespoke adaptation logic outside
the abstraction framework. The consequence is that each such deployment
produces a custom, closed integration layer that is tightly coupled to a
specific vendor stack or orchestrator implementation. These solutions are
difficult to share, extend, or migrate, and reproduce the same integration
effort across independent deployments.

Some operators report difficulties in selecting the relevant
service model(s) for a given deployment scenario, and in applying
them in real-world deployments.  The L2SM [RFC8466], L3SM [RFC8299],
ACaaS [RFC9834], and NSS [I-D.ietf-teas-ietf-network-slice-nbi-yang]
all define service abstractions at a similar level in adjacent
domains.  Insufficient guidance exists on how to effectively use and
compose these service models in practice.

Overall, the working group would benefit from collecting operator's
concrete implementation experiences and best practices around the
LxNM and LxSM models, both successful and unsuccessful. Other operators
can learn from these experiences, and the working group can assess whether
structural (e.g., non-backward compatible) changes to the models are warranted.

# Limited Observability and Service Assurance

Existing service and network models focus on configuration and
provide no standardised mechanism for reporting whether a service is
being delivered as ordered.  This gap applies both to discrete
operational state — whether the service is up and fault-free at a
given moment — and to historical SLO compliance — whether the service
has met its contracted performance targets over a given period.

SAIN ([RFC9417], [RFC9418]) addresses observability internally within
the provider domain through an assurance graph that enables operators
to troubleshoot outages.  It is not designed to be exposed to
customers or to the service model layer.  SAIN can contribute through
a feedback loop from the SAIN Controller to the Service Orchestrator,
which would then populate relevant state in LxSM/LxNM; however, that
interworking is not currently defined.

[RFC9375] provides raw performance monitoring data but does not
include a model for expressing the outcome of evaluating that data
against the SLOs set in the service model.

The gap is a customer-facing service delivery reporting layer that
does not currently exist in LxSM or LxNM.

#  OSS/BSS Interface and API Interoperability

YANG data models are commonly used as the basis for APIs that expose
service abstractions to external systems.  However, existing work
provides limited guidance on how these abstractions should be
exposed, versioned, or consumed in a predictable and interoperable
manner.

## Northbound API Semantics

Orchestration and controller systems have widely adopted YANG as a
schema language, but a lack of equivalent standards for API semantics
on orchestrator northbound APIs has resulted in divergent
implementations and interoperability challenges.

Standardized semantics for service creation, modification, and
decommissioning would simplify the implementation of orchestrators
and controllers, and enable better interoperability across vendors
and deployments.

## TMF API-Based Semantics

Some operators use [TMF640]/[TMF641] as the northbound API for
service ordering from their BSS.  There is no specification of how
these interfaces can be aligned with YANG service and network models.
Operators must either pay commercial OSS/BSS vendors to build bespoke
interfaces or build and maintain their own adaptation layer.  The
absence of a defined API creates integration complexity, lack of
reuseability, vendor lock-in risk, and inconsistent implementations
across deployments.

#  Insufficient Guidance for Implementing YANG-based Orchestration

With the YANG Data Store [RFC7950] and the NMDA architecture
[RFC8342] the IETF has standardized a clear operational model for
exposing configuration and operational state from an individual
NETCONF/RESTCONF server.

YANG has since gained widespread adoption as a schema language for
describing the data model at various abstraction layers, above the
network devices themselves.  In other words, YANG is increasingly
used to define the schemas for OSS systems, orchestrators, and
controllers.  A prime example are the IETF LxSM and LxNM models.

A variety of Informational RFCs, including The YANG Module
Classification [RFC8199], Service Models Explained [RFC8309], and A
Framework for Automating Service and Network Management with YANG
[RFC8969] have advanced the classification of, and relationships
between YANG modules as well as the software components that
implement them.

Operators consistently report they require more precise guidelines,
and a much more concrete service stack.  This includes a requirement
for clear guidelines on how and when to combine IETF standardized
models with vendor models and user models at each layer.  Further
guidance is needed on the interactions between the Service and
Network Models.  Previous IETF work has attempted to define an exact
mapping between these layers, which was determined to be impractical,
yet guidance on how to manage these interactions is still needed.

## Other Southbound Management Interfaces

Operators depend on a heterogeneous mix of network devices, and
network controllers, some of which are YANG model-driven, and others
that are not.  Vendor-specific APIs, and legacy management mechanisms
such as CLI and SNMP remain prevalent.  A mix of these interfaces is
common even within a single deployment.

Operators require a standardized approach to integrating non-YANG
modeled devices into YANG-model driven orchestration systems.  This
includes the ability to apply a YANG-modeled intent to non-NETCONF/
RESTCONF devices, and to collect telemetry data from non-YANG modeled
devices in a way that can be correlated with YANG modeled service
state and performance data.

# Items for Future Study

The following problem areas were raised during the IETF126 ONSEN work
group meeting and subsequent mailing list exchanges.  They are
currently insufficiently developed or prioritised for inclusion in the
core problem statement.  They are documented here for completeness
and to solicit community input on whether and how they should be
addressed by ONSEN or other working groups.

## Quantum-Security Requirements in Service Models

An increasing number of customers require long-term data
confidentiality and protection against future quantum-computing
threats.  Current service models do not provide a way to express
quantum-security requirements or capabilities.  It is not yet clear
whether this is best addressed through extensions to existing models
or through new model constructs.  This item is considered lower
priority relative to the core problems above and may be revisited in
a later revision.

## Secure Handling of Secrets in Orchestration Workflows

Device-level configuration often requires credentials, authentication
keys, or other sensitive data.  There is currently no standardised
mechanism for referencing sensitive information securely across the
orchestration stack and resolving it only at the point of device
configuration.  Whether this belongs within the scope of ONSEN,
NETMOD, or another WG is an open question that requires further
discussion.

## Generic Abstraction-to-Underlay Mapping

Operators repeatedly encounter the challenge of mapping service-layer
abstractions to underlying infrastructure resources in a generic,
technology-independent way.  Existing work addresses this problem in
specific contexts (e.g., TE-based approaches in TEAS) but no broadly
applicable mechanism exists.  The ONSEN charter defines the device
layer as the lowest layer in scope; mapping below the device layer is
therefore out of scope.  Further information from the community on
the exact problem being identified is needed before this item can be
evaluated for inclusion.

## Absence of Reusable Service Configuration Constructs

The LxSM models do not provide templates or reusable constructs to
aid operators in reducing the input parameters required for common
site deployment patterns.  Operators must manually configure each
service instance, which increases the risk of misconfiguration and
reduces operational efficiency.

YANG Configuration Templates [I-D.tt-netmod-yang-config-templates],
being worked on in NETMOD already provide a generic solution that
addresses repetitive YANG configuration instances.

## Abstractions for Telco-Cloud Scenarios
Cloud-native networking and distributed cloud infrastructure are
reshaping how telecommunication services are deployed and operated.
Telco-Cloud environments span central data centers, edge sites, and
far-edge locations, and combine networking, compute, storage, and
application resources.

Emerging services require placement decisions that jointly consider
network and compute characteristics, but current workflows often depend
on multiple cloud managers, network controllers, monitoring systems,
and inventory repositories. This complexity can slow admission control
and service instantiation. Better abstractions can provide a consistent
view of infrastructure capabilities so orchestration systems can make
faster decisions without exposing implementation detail.

# IANA Considerations

This memo includes no request to IANA.

# Security Considerations

TODO
