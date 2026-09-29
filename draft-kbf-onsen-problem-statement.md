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
    email: samier.barguil_giraldo@nokia.com
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
  RFC9315:
  RFC9375:
  RFC9417:
  RFC9418:
  RFC9922:
  RFC8466:
  RFC9182:
  RFC9291:
  RFC9543:
  draft-ietf-teas-ietf-network-slice-nbi-yang-26:
    title: "A YANG Data Model for the RFC 9543 Network Slice Service"
    target: https://datatracker.ietf.org/doc/html/draft-ietf-teas-ietf-network-slice-nbi-yang-26
  RFC9408:
  RFC8969:
  NEMOPS:
    title: "IAB Workshop Report: Next Era of Network Management Operations (NEMOPS)"
    target: https://datatracker.ietf.org/doc/draft-ietf-nemops-workshop-report/

...

--- abstract

The IETF has produced numerous YANG data models for automating the
provisioning and delivery of network and connectivity services,
including L2SM, L3SM, L2NM, L3NM, Attachment Circuits, and Network
Slicing models.  Despite their wide availability, operators report
persistent challenges in operationalizing these abstractions in a
consistent, scalable, and automatable manner.  This document
describes the problem space for the ONSEN Working Group, identifying
the operational gaps and deficiencies in existing IETF service and
network abstraction models that prevent effective end-to-end
automation.  The problems documented here are drawn from operator
experience and from the findings of the IAB NEMOPS Workshop.  This
document does not propose solutions, protocols, or new data models.


--- middle

# Introduction

The IETF has produced several YANG data models that are instrumental for
automating the provisioning and delivery of connectivity services, as
described in {{RFC8969}}.  These include models such as L3SM
{{RFC8299}}, L3NM {{RFC9182}}, L2SM {{RFC8466}}, L2NM
{{RFC9291}} and Service Attachment Points (SAPs) {{RFC9408}}. Current
IETF work adds on a YANG model for Network Slice Service {{RFC9543}}, in{{draft-ietf-teas-ietf-network-slice-nbi-yang-26}}.

While some of these abstractions have been deployed, operators report
persistent challenges in operationalizing them.  As highlighted by the
IAB NEMOPS Workshop {{NEMOPS}}, these challenges are systemic and
operational in nature.  They are not confined to a specific technology
or service type, but recur across abstraction domains and deployment
environments.

In addition, despite the availability of numerous YANG data models -
covering configuration, assurance, and fault management - and the
ongoing effort to make these models coexist within a common framework
under the IETF umbrella, operators continue to face significant
challenges in operationalizing YANG-based service APIs in a consistent,
scalable, and interoperable manner. While models such as the L3SM,
L2SM, L3NM, L2NM, AC/SAP abstractions and Network Slice Service each
address specific aspects
of service delivery, it is not always clear which models should be used
together, in which scenarios, or to what extent a given implementation
actually supports the full model. The usage of these APIs remains
fragmented - often partially implemented - and difficult to automate
end-to-end. In practice, APIs generated from similar YANG models often
differ in service semantics, and the lack of clear guidance on model
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
: Attachment Circuit.

Abstraction:
: The process of defining simplified, high-level constructs that
represent network and service-level capabilities, while hiding the
details of their underlying realization.  Abstraction enables
interaction between management and automation systems without
requiring direct exposure of device-specific configurations or
protocol behaviors.

LxNM:
: Layer x Network Model (L2NM or L3NM).

LxSM:
: Layer x Service Model (L2SM or L3SM).

NEMOPS:
: Next Era of Network Management Operations.

ONSEN:
: Operationalizing Network and SErvice abstractioNs.

OSS:
: Operation Support Systems.

Service Model Intent:
: The desired service behavior expressed by an operator through a
  service model (LxSM), as described in {{RFC8309}}.  This refers to
  the set of customer requirements — connectivity, performance,
  availability — submitted to the orchestration layer and expected to
  be maintained by the network over time.  When this document uses the
  term "service intent", it refers to Service Model Intent in this
  sense.


# Background

This section provides a brief overview of the existing IETF YANG model
landscape relevant to the ONSEN problem space and the RFC 8969
framework. It describes the key data models that form the foundation of
this work, including the L3VPN and L2VPN Service Models (L3SM, L2SM),
the L3VPN and L2VPN Network Models (L3NM, L2NM), and the Attachment
Circuit (AC) and Service Attachment Point (SAP) abstractions. Together,
these models define how services are specified, provisioned, and
delivered across a provider's network.

## The RFC8969 Framework

The YANG Automation Framework provides a programmatic approach to
representing services and networks through data models. It is designed
to automate the management life cycle-including instantiation,
provisioning, optimization, and monitoring-while enabling closed-loop
control for adaptive service maintenance.

The framework uses a layered approach to promote data
reusability and prevent feature duplication across different management
levels:

- Service Models: These are customer-facing modules that define
  high-level network services (e.g., L3VPN) independently of specific
  technologies. They capture customer requirements such as
  communication scope (pipe, hose, or funnel) and performance
  guarantees.
- Network Models: These describe network-level abstractions across
  multiple devices, including topologies, resources, and protocols at
  the link and network layers.
- Device Models: Also known as Network Element models, these are
  technology-specific modules (e.g., BGP, ACL, or interface management)
  used to realize services on individual functions or hardware.

The framework organizes automation into two primary procedural blocks:

- Service Life-Cycle Management: This manages the end-to-end service
from a technology-independent perspective.

  - Service Exposure: Captures services offered to customers via model
    catalogs.
  - Service Creation/Modification: Validates resources and maps service
    requests to specific network or device models.
  - Service Assurance & Optimization: Uses telemetry to monitor
    performance against Service Level Agreements (SLAs) and dynamically
    adjusts configuration if objectives are not met.
  - Service Diagnosis & Decommission: Provides OAM
    (Operations, Administration, and Maintenance) for troubleshooting and
    handles the release of resources when a service is terminated.

- Service Fulfillment Management: Focused on the technical execution and
  operational state at the device level.

  - Intended Configuration Provision: Maps high-level service views into
    detailed device settings such as VRF definitions, IP layers, and
    QoS features.
  - Configuration Validation: Ensures the intended configuration
    successfully takes effect in the operational datastore.
  - Monitoring & Fault Diagnostics: Aggregates operational states to
    build network visibility and uses RPC (Remote Procedure Call)
    commands for fault isolation.

The framework translates end-to-end abstract views into domain-specific
views (mapping) and then into specific device-level modules
(decomposition). In practice, YANG Module Integration mechanisms such
as Schema Mount allow multiple YANG modules to be combined into a
tailored model for specific use cases. It also includes Closed-Loop
Control: by correlating telemetry data with configuration data, the
framework allows orchestrators to continuously adjust network resources
to meet intended service parameters.

The primary benefits of the framework are:

- Vendor-Agnosticism: Enables unified management of multi-vendor
  environments through standardized interfaces.
- Operational Agility: Moves away from manual, device-specific
  configuration toward network-wide provisioning.
- Unified Orchestration: Allows orchestrators and controllers to manage
  resources across different network domains and layers.

## The Service Models (LxSM)

The L3VPN Service Model (L3SM) and the L2VPN Service Model (L2SM) are
customer-facing YANG data models used to define the characteristics of
network services between a customer and a service provider. Both models
act as abstracted interfaces for management systems (such as
orchestrators) to automate the provisioning and management of VPN
services.

Defined in {{RFC8299}}, the L3SM is used to deliver Layer 3
provider-provisioned VPN services, specifically limited to BGP PE-based
VPNs.

Defined in {{RFC8466}}, the L2SM is used to configure and manage Layer 2
provider-provisioned VPN services. It supports point-to-point Virtual
Private Wire Services (VPWS), multipoint Virtual Private LAN Services
(VPLS), and Ethernet VPNs (EVPNs). Both models include parameters for
bandwidth, MTU, QoS, BUM traffic, and availability.

Neither model is intended for the direct configuration of network
elements; instead, an orchestration layer takes these models as input
and translates them into technology-specific device models (such as BGP
or interface configurations).

## The Network Models (LxNM)

The L3VPN Network Model (L3NM) and the L2VPN Network Model (L2NM) are
network-centric YANG data models designed to manage VPN services within
a service provider's network. While the Service Models focus on the
customer's requirements, these Network Models provide an internal,
resource-facing view used by controllers to automate technical
configurations across multiple devices. Both models preserve specific
parameters for traffic management, covering bandwidth, MTU, QoS, and
BUM traffic.

Defined in {{RFC9182}}, the L3NM is used for the internal provisioning
of Layer 3 VPN services, specifically focusing on BGP PE-based VPNs and
Multicast VPNs.

Defined in {{RFC9291}}, the L2NM is the network-centric counterpart to
the L2SM, providing the internal view required to instantiate Layer 2
services. It covers a wide range of L2VPNs, including VPLS, VPWS, and
various EVPN flavors (EVPN over MPLS, VXLAN, and PBB-EVPN).

Unlike customer-facing service models, these models can expose internal
operational states and performance metrics to help controllers
continuously adjust the network to meet SLAs.

## Attachment Circuits (AC) and Service Attachment Points (SAP)

In the context of the YANG Automation Framework, Attachment Circuits
(ACs) and Service Attachment Points (SAPs) are fundamental abstractions
used to define how customer networks connect to a provider's network
and where services are delivered.

An Attachment Circuit, as defined in {{RFC9408}}, is a physical or
logical channel that connects a Customer Edge (CE) device to a Provider
Edge (PE) device.

A Service Attachment Point is an abstract network reference point -
typically the PE side of an AC - where network services are actually
delivered or "grafted" to the customer. The SAP Network Model {
{RFC9408}} provides an abstract view of the provider's topology,
exposing only the nodes and interfaces where services can be attached.

# Operational Problems with Service and Network Abstractions

This section identifies the six core operational problems that
motivate the ONSEN Working Group.  Each problem is described in terms
of its operational impact and why it cannot be resolved by
implementing automation of the existing LxNM/LxSM models in their
current forms.  The problems are drawn from operator experience and
from the findings of the IAB NEMOPS Workshop {{NEMOPS}}.

## Insufficient Guidance on YANG Model Usage and Composition

Despite the availability of numerous YANG data models for service and
network automation, operators report persistent difficulty in applying
them consistently in real-world deployments.  The root cause is the
absence of guidance on how existing models are intended to be used
together, how responsibilities are divided across abstraction layers
and working groups, and how common deployment patterns should be
approached.  This is not a gap in the models themselves but in the
documentation and operational guidance that would allow implementers
to use them correctly and consistently.

### Inconsistent Service Operation Semantics Across Models

Service operations such as instantiation, modification, and
decommissioning are initiated through YANG-based service APIs but
require coordination across orchestration systems, controllers, and
device configurations.  These components are rarely aligned in terms
of what each operation means or implies — for example, what
constitutes a successfully instantiated service, or what state a
service should be in after a modification is applied.  This
inconsistency makes it difficult to build reliable, automated
operational workflows.

### Absence of Reusable Service Configuration Constructs

The LxSM models do not provide templates or reusable constructs to
aid operators in reducing the input parameters required for common
site deployment patterns.  Operators must manually configure each
service instance, which increases the risk of misconfiguration and
reduces operational efficiency.

For the purposes of this document, "template" refers to reusable
constructs defined within a YANG model itself — not external
template-based configuration mechanisms.  The absence of such
constructs in the current LxSM models is the problem being
identified.

### Management Domain Integration Challenges

Despite the availability of numerous YANG data models, operators
depend on a heterogeneous mix of models, vendor-specific APIs, and
legacy mechanisms (CLI, SNMP), even within a single deployment.
Configuration management and the collection of statistics and
telemetry data continue to exist as separate silos in both the
organizational structure and technology stacks.  There is no guidance
on how these domains should be integrated or how responsibilities
should be divided.

### Absence of Architectural Documentation

There is no document that explains how LxSM, LxNM, AC, SAP, NSS,
and related IETF models are intended to work together as a system.
Operators express difficulty understanding which abstractions to use,
how they should be combined, and how responsibilities are divided
across layers and working groups.  The absence of cohesive guidance
leads to divergent interpretations and inconsistent deployments.

The architectural guidance produced by ONSEN should not be a
standalone descriptive document.  It should be directly tied to the
concrete work items the WG undertakes, informing model design
decisions and helping implementers apply the abstractions correctly.
The collection and documentation of real-world operational experience
— including implementation lessons, interoperability findings, and
deployment patterns — is considered part of the scope of this work
item.

## Misalignment Between and Within Abstraction Layers

The service and network abstraction layers defined by the IETF were
developed independently and with limited cross-layer coordination.
This results in structural and semantic misalignment both within the
same abstraction layer and across layers.  Some degree of difference
across layers is intentional and necessary — each layer serves a
distinct purpose.  The problem is the absence of guidance on where
alignment is appropriate, where separation should be preserved, and
how the layers are intended to interwork.  Defining authoritative
mappings between layers is out of scope for ONSEN; providing guidance
on how such mappings can be approached and what properties they
should preserve is in scope.

### Inconsistent Parameter Availability and Naming

Very similar, if not identical, features and functionality across
different models at the same abstraction layer often use different
parameter names, different YANG data types, or are not configurable
to the same level of detail.  This inconsistency increases
implementation effort and complicates integration across models.

### Cannot Combine Service Instances Across LxSM Models

An operator offering a diverse set of services (L3VPN, L2VPN,
internet access, etc.) cannot use the LxSM models to offer a
combination of these services through a consistent representation on
the same orchestrator.

### LxSM and Network Slice Service Model Relationship

The published LxSM models and
{{draft-ietf-teas-ietf-network-slice-nbi-yang-26}} act as Service
Models with a similar level of abstraction.  Operators need guidance
on the use cases for both model sets, and when one should be used
versus the other, or whether both can and should be combined for a
given deployment scenario.

### No Defined Interworking Between Service and Network Layers

Some service abstractions do not have a defined relationship to
underlying network models, making it difficult to implement and
automate end-to-end service provisioning.  Similarly, the Network
Models (LxNM) expose parameters that have no equivalent in the
Service Models (LxSM), making consistent bidirectional correlation
difficult.  The absence of guidance on how these layers should
interwork — and how controllers should interpret and adapt
Service Model Intent {{RFC8309}} at each stage — is a recurring
source of proprietary implementations and inconsistent behavior.

### Inconsistent Semantics Across Layers

Abstraction models frequently rely on metrics, attributes, or
parameters whose semantics vary across vendors, models,
implementations, or consumption contexts.  Concepts such as cost,
availability, or performance may be represented using different
definitions, units, scopes, or update frequencies.  Control-plane
behaviors that represent vendor differentiators are not captured in
service-level intent, further complicating the correlation between
what was requested and what is delivered.

## On-Demand and Scheduled Service Operations

Existing YANG service and network models are primarily designed for
static, long-lived services.  They do not provide the constructs
needed to express service behaviors that vary over time, are triggered
on demand, or follow a defined schedule.  As operators introduce
services such as data-intensive workload transmission or SD-WAN-like
dynamic VPN capabilities, this gap becomes a concrete operational
blocker.

{{RFC9922}} defines a generic schedule model in NETMOD that could be
bound to LxSM service instances.  ONSEN's role is to define how that
binding works and what additional model constructs are needed.  Any
enhancements required at the NETCONF protocol level would fall under
the NETCONF Working Group.

Use cases driving this requirement include:

- Data-intensive workload transmission services requiring on-demand
  ultra-high bandwidth and deterministic scheduling.

- Customer expectations for SD-WAN-like dynamic capabilities within
  traditional managed VPN services.

- Time-based network security policies such as IP-based access
  control rules with scheduled expiry.

### Absence of Temporal and State Attributes in Service Models

Existing service and network abstractions lack native constructs to
express temporal attributes such as activation time, duration,
expiration, or rollback behavior.  Service Model Intent {{RFC8309}}
that is transient in nature must therefore be tracked and enforced
outside the abstraction framework, increasing operational complexity
and reducing automation reliability.

### Limited Support for Dynamic Service Instantiation and Modification

Existing service and network abstractions provide limited support for
on-demand service instantiation, dynamic bandwidth adjustment, or
temporary service suspension.  Services are implemented in a top-down
manner, so both the model constructs to express time-varying Service
Model Intent and the operational workflows to trigger and manage
changes are required.  Operators must implement custom logic outside
the abstraction framework to support these operations.

## Limited Observability and Feedback

Existing service and network models focus on configuration and
provide no standardised mechanism for reporting whether a service is
being delivered as ordered.  This gap applies both to discrete
operational state — whether the service is up and fault-free at a
given moment — and to historical SLO compliance — whether the service
has met its contracted performance targets over a given period.

SAIN ({{RFC9417}}, {{RFC9418}}) addresses observability internally
within the provider domain through an assurance graph that enables
operators to troubleshoot outages.  It is not designed to be exposed
to customers or to the service model layer.  SAIN can contribute
through a feedback loop from the SAIN Controller to the Service
Orchestrator, which would then populate relevant state in LxSM/LxNM;
however, that interworking is not currently defined.

{{RFC9375}} provides raw performance monitoring data but does not
include a model for expressing the outcome of evaluating that data
against the SLOs set in the service model.

The gap is a customer-facing service delivery reporting layer that
does not currently exist in LxSM or LxNM.

### Lack of Operational State in LxSM and LxNM Models

Some of the LxSM and LxNM models provide operational state
information, but this is not consistent across models, and the
information provided is often insufficient for operators to determine
whether the service is functioning as intended.

For example, the L3SM model does not provide any operational state
information, while the L2SM model provides some operational state
information, but it is limited to the status of the service and does
not include details on SLO violations or other operational metrics
that would be useful for troubleshooting and monitoring.

## OSS/BSS Interface and API Interoperability — TMF Mapping

Many operators use TMF640/641 as the northbound API for service
ordering from their BSS.  There is no specification of how these
interfaces align with YANG service and network models.  Operators
must either pay commercial OSS/BSS vendors to build bespoke
interfaces or build and maintain their own adaptation layer.  The
absence of a defined alignment creates integration complexity, vendor
lock-in risk, and inconsistent implementations across deployments.

This problem area is in scope for ONSEN.  The WG expects to address
it after the foundational problems in Sections 4.1 through 4.4 are
sufficiently progressed.

## YANG Model to Northbound API Domain Mapping

YANG data models are increasingly used as the basis for northbound
APIs exposed to orchestration systems and customers.  However,
different implementations of similar YANG models produce APIs that
differ in service semantics — parameter naming, data types, scoping
— even when the underlying models are closely related.  There is no
guidance on how YANG-based service abstractions should be translated
into northbound APIs in a consistent and interoperable way.  This
divergence complicates integration across systems and vendors and
undermines the portability gains that standardised YANG models are
intended to provide.

# Evidence from the IAB NEMOPS Workshop

This section summarizes the relevant findings of the IAB NEMOPS
Workshop {{NEMOPS}} that corroborate the problems identified in
Section 4.

- Despite significant progress in protocol development and data
  modeling, operational workflows remain fragmented and difficult to
  automate end-to-end.

- Model-driven network management is generally successful, yet
  insufficient on its own to address higher-level operational needs.

- Gaps between device-level and service-level abstractions: existing
  models often lack the semantic alignment and contextual information
  required by orchestration and OSS/BSS systems.

- Operators must perform extensive model mapping, data
  transformation, and system-specific integration outside the scope
  of standardized abstractions.

- Limited ability to validate whether service intent is being met
  over time or to correlate operational state across abstraction
  layers.

- Additional operator-reported challenges to be added here from contributors.


# Operator Experiences

TODO

This section documents operational problems reported directly by
network operators.  To be populated by operator contributors.

# Items for Future Study

The following problem areas were raised during IETF 126 discussions
and subsequent mailing list exchanges.  They are not yet sufficiently
developed or prioritised for inclusion in the core problem statement.
They are documented here for completeness and to solicit community
input on whether and how they should be addressed by ONSEN or other
working groups.

## Quantum-Security Requirements in Service Models

An increasing number of customers require long-term data
confidentiality and protection against future quantum-computing
threats.  Current service models do not provide a way to express
quantum-security requirements or capabilities.  It is not yet clear
whether this is best addressed through extensions to existing models
or through new model constructs.  This item is considered lower
priority relative to the core problems in Section 4 and may be
revisited in a later revision.

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

# IANA Considerations

This memo includes no request to IANA.

# Security Considerations

TODO
