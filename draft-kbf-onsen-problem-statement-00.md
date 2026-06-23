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
  github: USER/REPO
  latest: https://example.com/LATEST

author:
 -
    fullname: Your Name Here
    organization: Your Organization Here
    email: your.email@example.com

normative:
  RFC2119:
  RFC8174:

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

TODO Introduction

The IETF has produced several YANG data models that are instrumental
for automating the provisioning and delivery of connectivity
services, as described in {{RFC8969}}.  These include models such as
L3SM {{RFC8299}}, L3NM {{RFC9182}}, L2SM {{RFC8466}}, L2NM
{{RFC9291}}, Network Slice Service {{RFC9543}}, and Service
Attachment Points (SAPs) {{RFC9408}}.

While these abstractions are widely deployed, operators report
persistent challenges in operationalizing them.  As highlighted by
the IAB NEMOPS Workshop {{NEMOPS}}, these challenges are systemic
and operational in nature.  They are not confined to a specific
technology or service type, but recur across abstraction domains and
deployment environments.

The Operationalizing Network and SErvice abstractioNs (ONSEN)
Working Group is chartered to address this problem space by focusing
on the operational aspects of network and service abstractions.  It
aims to make it easier to implement and use the IETF's service and
network abstractions, with the goal of improving network automation,
operational efficiency, and interoperability.

This document defines the problem space for ONSEN.  It does not
propose solutions, protocols, or new data models.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

The following terms are used in this document:

AC:
: Attachment Circuit.

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

TODO Background

This section provides a brief overview of the existing IETF YANG
model landscape relevant to the ONSEN problem space, and the
RFC8969 framework.


## IETF Service and Network Abstraction Models

TODO: Brief summary of existing models (L2SM, L3SM, L2NM, L3NM,
AC, SAP, NSS, ACTN, SAIN, VN) and their intended roles.


## The RFC8969 Framework

TODO: Summary of the Automating Service and Network Management
Framework and how it relates to the abstraction layers ONSEN
addresses.


# Operational Problems with Service and Network Abstractions

TODO Introduction to problem areas section.

This section identifies the core operational problems that motivate
the ONSEN Working Group.  Each problem is described in terms of its
operational impact and why it cannot be resolved by implementing
automation of the existing LxNS/LxSM models in their current forms.


## Fragmented Operational Lifecycles

TODO

Operational workflows associated with service abstractions — service
instantiation, monitoring, modification, troubleshooting, and
decommissioning — are often fragmented and inconsistently handled.

Key issues to elaborate:

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

TODO

Service abstractions are realized through a combination of
service-level models, network-level models, control-plane behavior,
and management interfaces.  These layers are often developed
independently, with limited coordination across working groups or
operational domains.

Key issues to elaborate:

- Service abstractions that do not cleanly map to underlying network
  capabilities.

- Network models that expose parameters without clear service-level
  semantics.

- Control-plane behaviors that are difficult to correlate with
  service-level intent.

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

- [Additional operator-reported challenges to be added here from
  contributors.]


# Operator Experiences

TODO

This section documents operational problems reported directly by
network operators.  [To be populated by operator contributors.]


# Security Considerations

TODO Security

The problems identified in this document are operational in nature.
To the extent that addressing them involves defining new or updated
YANG data models, API interfaces, or operational guidance, the
security implications of those outputs will be addressed in the
respective solution documents.


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.

The editors thank all contributors to the ONSEN Working Group
discussions and the authors of draft-xie-onsen-problem-statement
for their prior work documenting the problem space.
