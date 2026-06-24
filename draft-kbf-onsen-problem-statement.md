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
    organization: NetEdge
    email: kris@netedge.plus

informative:
  RFC8299:
  RFC8466:
  RFC9182:
  RFC9291:
  RFC9543:
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
described in {{RFC8969}}.  These include models such as L3SM {
{RFC8299}}, L3NM {{RFC9182}}, L2SM {{RFC8466}}, L2NM{
{RFC9291}}, Network Slice Service {{RFC9543}}, and Service Attachment
Points (SAPs) {{RFC9408}}.

While these abstractions are widely deployed, operators report
persistent challenges in operationalizing them.  As highlighted by the
IAB NEMOPS Workshop {{NEMOPS}}, these challenges are systemic and
operational in nature.  They are not confined to a specific technology
or service type, but recur across abstraction domains and deployment
environments.

In addition, despite the availability of numerous YANG data models —
covering configuration, assurance, and fault management — and the
ongoing effort to make these models coexist within a common framework
under the IETF umbrella, operators continue to face significant
challenges in operationalizing YANG-based service APIs in a consistent,
scalable, and interoperable manner. While models such as the L3SM,
L2SM, L3NM, L2NM, and AC/SAP abstractions each address specific aspects
of service delivery, it is not always clear which models should be used
together, in which scenarios, or to what extent a given implementation
actually supports the full model. The usage of these APIs remains
fragmented — often partially implemented — and difficult to automate
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

{::boilerplate bcp14-tagged}

The following terms are used in this document:

AC:
: Attachment Circuit as defined in {{RFC9835}}.

API:
: Application Programming Interface.

Abstraction:
: The process of defining simplified, high-level constructs that
represent network and service-level capabilities, while hiding the
details of their underlying realization.  Abstraction enables
interaction between management and automation systems without
requiring direct exposure of device-specific configurations or
protocol behaviors.

Abstraction Layer:
: The layer between managed components and the automation management
system that exposes simplified service and network constructs.

BSS:
: Business Support Systems.

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
to automate the management life cycle—including instantiation,
provisioning, optimization, and monitoring—while enabling closed-loop
control for adaptive service maintenance. Layered Data Model
Architecture The framework uses a layered approach to promote data
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

1. Service Life-Cycle Management — This manages the end-to-end service
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

2. Service Fulfillment Management — This focuses on the technical
execution and operational state at the device level.

Intended Configuration Provision: Maps high-level service views into
detailed device settings such as VRF definitions, IP layers, and QoS
features. Configuration Validation: Ensures the intended configuration
successfully takes effect in the operational datastore. Performance
Monitoring & Fault Diagnostics: Aggregates operational states to build
network visibility and uses RPC (Remote Procedure Call) commands for
fault isolation.

The framework translates end-to-end abstract views into domain-specific
views (mapping) and then into specific device-level modules
(decomposition). In practice, YANG Module Integration mechanisms such
as Schema Mount allow multiple YANG modules to be combined into a
tailored model for specific use cases. It also includes Closed-Loop
Control: by correlating telemetry data with configuration data, the
framework allows orchestrators to continuously adjust network resources
to meet intended service parameters.

The Primary Benefits of the framework are:

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

Defined in {{RFC8049}}, the L3SM is used to deliver Layer 3
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

A Service Attachment Point is an abstract network reference point —
typically the PE side of an AC — where network services are actually
delivered or "grafted" to the customer. The SAP Network Model {
{RFC9408}} provides an abstract view of the provider's topology,
exposing only the nodes and interfaces where services can be attached.

# Operational Problems with Service and Network Abstractions

This section identifies the core operational problems that motivate
the ONSEN Working Group.  Each problem is described in terms of its
operational impact and why it cannot be resolved by implementing
automation of the existing LxNM/LxSM models in their current forms.

## Fragmented Operational Lifecycles

Operational workflows associated with service abstractions — service
instantiation, monitoring, modification, troubleshooting, and
decommissioning — are often fragmented and inconsistently handled.

- Operators depend on a heterogeneous mix of management protocols,
  vendor-specific APIs, and legacy mechanisms (CLI, SNMP) even
  within a single deployment.

- Lifecycle actions initiated through YANG-based service APIs often
  require coordination across orchestration systems, controllers, and
  device configurations, but these components are rarely aligned in
  terms of lifecycle semantics or data models.

- Existing service and network abstractions lack native constructs to
  express lifecycle attributes such as activation time, duration,
  expiration, or rollback behavior.  Transient service intents must
  therefore be tracked and enforced outside the abstraction
  framework.

- Configuration management and the collection of statistics /
  telemetry data continue to exist as separate silos in both the
  organizational chart and technology stacks/APIs.


## Misalignment Between Abstraction Layers

Service abstractions are realized through a combination of
service-level models, network-level models, control-plane behavior,
and management interfaces.  These layers are often developed
independently, with limited coordination across working groups or
operational domains.

- Service abstractions that do not cleanly map to underlying network
  capabilities.

- Network models that expose parameters without clear service-level
  semantics.

- Control-plane behaviors (vendor differentiators) that are difficult to
  correlate with service-level intent.

- Difficulty in combining different services into a higher-level
  composite service.

- An operator offering a diverse set of services (L3VPN, L2VPN,
  internet access, etc.) cannot use the LxSM models to offer a
  combination of these services through a single service
  orchestrator.


## Inconsistent Service Semantics

TODO

Abstraction models frequently rely on metrics, attributes, or
parameters whose semantics vary across models, implementations, or
consumption contexts.  Concepts such as cost, availability, or
performance may be represented using different definitions, units,
scopes, or update frequencies.

Key issues to elaborate:

- APIs derived from similar YANG models differ in service semantics
  across vendors and deployments, complicating integration for
  operators and OSS/BSS systems.

- The lack of consistent guidance on how abstractions should be
  modeled, exposed, and consumed results in APIs that vary
  significantly across vendors and deployments.

- Inconsistent semantics complicate integration between systems and
  undermine the reliability of automation, typically addressed
  through custom logic or manual processes that reduce portability
  and interoperability.

- The LxSM models lack administrative state control and operational
  state information.


## Limited Observability and Feedback

TODO

Existing abstractions primarily focus on configuration and offer
limited standardized mechanisms for reporting whether requested
behaviors have been successfully applied or remain valid over time.

Key issues to elaborate:

- Operators have limited ability to validate whether service intent
  is being met over time or to correlate operational state across
  abstraction layers.

- The lack of consistent feedback undermines closed-loop automation
  and complicates troubleshooting, particularly in multi-vendor and
  multi-domain environments.

- This lack of feedback assurance increases reliance on manual
  monitoring and intervention.


## OSS/BSS Interface and API Interoperability

TODO

YANG data models are commonly used as the basis for APIs that expose
service abstractions to external systems.  However, existing work
provides limited guidance on how these abstractions should be
exposed, versioned, or consumed in a predictable and interoperable
manner.

Key issues to elaborate:

- Some operators adopt TMF640/641 as APIs for service ordering from
  their BSS, but how these interfaces can be aligned with
  service/network YANG models is not specified.  Operators face the
  challenge of either paying commercial OSS/BSS providers to create
  bespoke interfaces or building an adaptation layer themselves.

- The declarative model-driven nature of NETCONF/YANG is powerful,
  but the lack of commercial off-the-shelf systems that implement
  this methodology creates risk of vendor lock-in and limits uptake.

- APIs generated from similar YANG models often differ in service
  semantics, complicating integration across systems, vendors, and
  deployment environments.


## Lack of Architectural Guidance and Documentation

TODO

A recurring theme from the NEMOPS discussions is the absence of
architectural documentation and operational guidance explaining how
existing abstractions, models, protocols, and tools are intended to
work together as a system.

Key issues to elaborate:

- Operators express difficulty understanding which abstractions to
  use, how they should be combined, and how responsibilities are
  divided across layers and working groups.

- The absence of cohesive guidance leads to divergent
  interpretations and inconsistent deployments.


# Evidence from the IAB NEMOPS Workshop

TODO

This section summarizes the relevant findings of the IAB NEMOPS
Workshop {{NEMOPS}} that corroborate the problems identified in
Section 4.

Key points to elaborate:

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


# Security Considerations

TODO S
