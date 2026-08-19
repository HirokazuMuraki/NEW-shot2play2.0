# NEW-shot2play Technical Specification Version 2.0
# Design Freeze — Approved Baseline

## 1. Document Status

| Item | Value |
|---|---|
| Specification | NEW-shot2play Technical Specification |
| Version | 2.0 |
| Document | Design Freeze — Approved Baseline |
| Status | APPROVED |
| Purpose | Define the frozen technical architecture and design boundary of Version 2.0 |
| Scope | Transaction Framework, Authentication, Entitlement, Policy Evaluation, Authorization, Service Execution, Evidence, and Audit |
| Baseline | Version 2.0 Repository Baseline |

This document defines the frozen architectural and technical design
baseline of NEW-shot2play Technical Specification Version 2.0.

The contents of this document represent the design baseline from which
the protocol definitions, patent analysis, claim mapping, and subsequent
patent application documents SHALL be derived.

This document SHALL NOT be interpreted as a patent claim document.

It defines the technical design that forms the source material for
subsequent patent analysis and claim construction.

---

## 2. Design Freeze Principle

Version 2.0 is treated as a distinct technical specification and
invention baseline.

Version 2.0 is not defined merely as a revision of the Version 1.0
Authentication Protocol.

Version 1.0 provides the authentication foundation.

Version 2.0 extends the processing architecture above and around
authentication by introducing a unified model for:

1. Authentication
2. Entitlement
3. Policy Evaluation
4. Authorization
5. Service Execution
6. Evidence Collection
7. Audit Recording

The Version 2.0 architecture SHALL preserve a clear separation between
identity authentication and entitlement-based service authorization.

The design SHALL therefore distinguish:

- whether an entity has been authenticated;
- whether the entity possesses a required Entitlement;
- whether the applicable Policy conditions are satisfied; and
- whether the requested Service Execution is authorized.

---

## 3. Core Architectural Principle

The fundamental processing model of Version 2.0 is:

    Authentication
          ↓
    Entitlement
          ↓
    Policy Evaluation
          ↓
    Authorization
          ↓
    Service Execution

The above sequence represents the principal logical relationship between
the major processing functions.

The implementation MAY distribute these functions across multiple
components or services, provided that the logical relationships and
security properties defined by this specification are preserved.

Authentication SHALL NOT by itself be treated as sufficient authorization
for a protected service where an Entitlement or Policy condition is
required.

Authorization SHALL be based on the applicable authentication result,
Entitlement state, Policy evaluation, transaction context, and other
authorized decision inputs defined by the Service Profile.

---

## 4. Transaction-Centric Architecture

Version 2.0 SHALL use a common Transaction Framework as the architectural
foundation for business operations.

Each business operation SHALL be represented as a Transaction.

A Transaction SHALL be associated with a Transaction Object containing
the information required to process and complete the operation.

The Transaction Object provides the common information model shared by
the processing stages.

The processing stages MAY include:

- Transaction Creation
- Authentication
- Entitlement Evaluation
- Policy Evaluation
- Authorization
- Service Execution
- Evidence Collection
- Audit Recording
- Transaction Completion

Each stage SHALL have a defined logical responsibility.

A processing stage SHALL NOT improperly modify information owned by
another security or processing stage.

The Transaction Framework SHALL remain independent of application-specific
business logic.

Application-specific behavior SHALL be defined through a Service Profile
or an equivalent service-specific configuration.

---

## 5. Separation of Authentication and Entitlement

Authentication and Entitlement SHALL be treated as distinct concepts.

Authentication establishes an identity-related result.

Entitlement represents a right, qualification, condition, or other
information indicating that a subject may satisfy a requirement for a
service or operation.

Successful Authentication SHALL NOT automatically create or imply every
possible Entitlement.

An Entitlement MAY be obtained through a transaction, event, business
operation, or other authorized mechanism defined by the applicable
Service Profile.

An Entitlement SHALL be independently represented and evaluated.

This separation SHALL permit the same authenticated subject to possess
different Entitlements for different services, transactions, contexts,
or conditions.

---

## 6. Entitlement Model

An Entitlement SHALL be modeled as an independently managed object.

An Entitlement MAY include, but SHALL NOT be limited to:

- Entitlement Identifier
- Subject Identifier
- Entitlement Type
- Issuer
- Issue Time
- Activation Time
- Expiration Time
- Status
- Scope
- Conditions
- Usage Limit
- Consumption State
- Transaction Binding
- Evidence Reference

The Entitlement lifecycle SHALL support the logical states and
transitions required by the Version 2.0 specification.

The principal conceptual lifecycle is:

    Acquire
       ↓
     Hold
       ↓
      Use
       ↓
    Consume
       ↓
     Expire

The implementation MAY include additional intermediate states or
revocation states where required by security or business requirements.

An Entitlement MAY be:

- time limited;
- usage limited;
- condition dependent;
- bound to a subject;
- bound to a transaction;
- bound to a service;
- bound to a context; or
- subject to revocation or revalidation.

---

## 7. Entitlement as a Cross-Service Condition

Version 2.0 SHALL permit an Entitlement obtained through one transaction
or service context to be evaluated as a condition for a subsequent
transaction or service operation, where permitted by Policy and the
applicable Service Profile.

The service that creates or issues an Entitlement MAY be different from
the service that subsequently evaluates or consumes the Entitlement.

Accordingly, an Entitlement MAY represent a transferable logical
condition between otherwise separate service operations without
transferring the underlying authentication credential.

For example:

    Service A
       │
       └── Entitlement A issued
                    │
                    ↓
              Service B
                    │
                    └── Entitlement A evaluated
                              │
                              ↓
                       Authorization
                              │
                              ↓
                       Service B execution

The above example is architectural and SHALL NOT limit the invention to
any particular business domain.

---

## 8. Conditional Entitlement Evaluation

Version 2.0 SHALL support evaluation of one or more Entitlements as
authorization conditions.

A Service Profile MAY require:

- a single Entitlement;
- multiple Entitlements;
- an AND relationship;
- an OR relationship;
- nested logical conditions; or
- other policy-defined combinations.

Examples include:

    Entitlement A → Service X

    Entitlement A AND Entitlement B → Service X

    Entitlement A OR Entitlement B → Service X

The actual logical expression SHALL be determined by the applicable
Policy and Service Profile.

The evaluation result SHALL be available to the Authorization stage.

---

## 9. Policy Evaluation

Policy SHALL be treated as a distinct decision input and processing
function.

A Policy MAY evaluate:

- authentication results;
- Entitlement state;
- subject attributes;
- device attributes;
- transaction attributes;
- service attributes;
- environmental context;
- time;
- location or other permitted context;
- usage conditions;
- security conditions; and
- other authorized decision inputs.

Policy Evaluation SHALL produce a decision-relevant result.

The result MAY include:

- allow;
- deny;
- conditional;
- insufficient information;
- revalidation required; or
- another defined decision state.

Policy Evaluation SHALL remain logically distinguishable from the
authentication process.

---

## 10. Authorization

Authorization SHALL determine whether the requested Service Execution
may proceed.

Authorization SHALL use the decision inputs defined by the applicable
Service Profile and Policy.

These inputs MAY include:

- Authentication Result;
- Entitlement Evaluation Result;
- Policy Evaluation Result;
- Transaction Context;
- Authorization Scope;
- Authorization Decision State;
- Revocation State;
- Revalidation State; and
- other trusted decision inputs.

Authorization SHALL produce an Authorization Decision.

The principal decision states are:

- Granted
- Denied
- Invalid
- Pending Revalidation
- Revoked

Additional states MAY be defined where required by the implementation.

A granted Authorization Decision SHALL NOT be treated as permanent.

It MAY be limited by:

- time;
- scope;
- transaction;
- service;
- Entitlement state;
- Policy state;
- context; or
- other defined constraints.

---

## 11. Authorization Decision Binding

An Authorization Decision SHALL be associated with the transaction and
service operation for which the decision was generated.

The decision MAY be bound to:

- Transaction Identifier;
- Subject Identifier;
- Service Identifier;
- Entitlement Identifier;
- Policy Version;
- Context;
- Authorization Scope; and
- decision validity period.

A Service Execution component SHALL verify that the Authorization
Decision remains valid for the requested execution.

A decision associated with a different transaction, subject, service,
scope, or invalidated context SHALL NOT automatically authorize a new
service execution.

---

## 12. Authorization Scope Propagation

Where processing is distributed across multiple components or services,
the security and authorization context MAY be propagated between those
components.

The propagated context SHALL preserve sufficient information to permit
the receiving component to determine whether the Authorization Decision
continues to apply.

The receiving component SHALL NOT assume that an authorization decision
is valid merely because a message or request was received from another
component.

Authorization context SHALL be reconstructed or verified according to
the applicable trust and protocol requirements.

---

## 13. Authorization Decision Continuity

A distributed implementation SHALL preserve authorization decision
continuity across the processing path.

Decision continuity includes, where applicable:

- subject continuity;
- transaction continuity;
- service continuity;
- scope continuity;
- Entitlement continuity;
- Policy continuity;
- context continuity; and
- decision-state continuity.

If required decision dependencies cannot be verified, the receiving
component SHALL NOT grant authorization solely on the basis of an
unverified prior decision.

---

## 14. Revocation and Revalidation

Authorization SHALL support invalidation where an underlying decision
dependency becomes invalid.

Examples include:

- Entitlement revocation;
- Entitlement expiration;
- Policy change;
- context invalidation;
- transaction expiration;
- authorization scope change;
- security event; or
- other defined invalidation condition.

Where an authorization dependency is invalidated, dependent
Authorization Decisions SHALL be invalidated or revalidated according
to the applicable protocol rules.

A service component SHALL fail closed when a required authorization
dependency cannot be established as valid.

---

## 15. Service Execution

Service Execution SHALL occur only after the required Authorization
Decision has been successfully established.

Service-specific business behavior SHALL be defined independently from
the common Transaction Framework.

The same framework MAY therefore support different service profiles,
including but not limited to:

- attendance management;
- coupon validation;
- point management;
- access control;
- electronic ticketing;
- facility access;
- service-specific benefits; and
- other transaction-oriented services.

The examples above are illustrative and SHALL NOT limit the technical
scope of Version 2.0.

---

## 16. Evidence and Audit

Version 2.0 SHALL support the generation of processing evidence and
audit information.

Evidence MAY be generated for:

- authentication;
- Entitlement issuance;
- Entitlement evaluation;
- Policy Evaluation;
- Authorization;
- Service Execution;
- Entitlement consumption;
- revocation;
- revalidation; and
- transaction completion.

Audit information SHALL preserve sufficient information to support
traceability of the relevant processing sequence.

The audit model SHALL remain logically separate from business-specific
execution logic.

---

## 17. Security Boundary

The Version 2.0 architecture SHALL maintain explicit security boundaries
between:

- client devices;
- authentication components;
- Entitlement management components;
- Policy evaluation components;
- authorization components;
- service execution components; and
- external business systems.

Trust between components SHALL be explicitly defined.

Data crossing a security boundary SHALL be validated according to the
applicable protocol and trust model.

Untrusted input SHALL NOT directly establish authentication,
Entitlement, Policy, or Authorization state.

---

## 18. Failure and Fail-Closed Principle

Version 2.0 SHALL define explicit failure handling for security-relevant
processing.

Where a required security condition cannot be verified, the system SHALL
prefer denial or non-execution over implicit authorization.

Examples include:

- invalid Authentication Result;
- invalid Entitlement;
- expired Entitlement;
- revoked Entitlement;
- invalid Policy;
- inconsistent Authorization Decision;
- missing authorization context;
- expired transaction;
- failed revalidation; and
- unavailable required security evidence.

Failure handling SHALL NOT silently convert an unknown or invalid state
into an authorized state.

---

## 19. Application Independence

The common Transaction Framework SHALL remain independent from any
specific business application.

A new business service SHOULD be implementable by defining or selecting
a Service Profile and applicable Policies without changing the
fundamental security architecture.

This principle enables the same technical framework to support multiple
business domains.

The business examples used in the specification are therefore
implementation examples and SHALL NOT define the boundaries of the
technical architecture.

---

## 20. Version 1.0 Boundary

Version 2.0 builds upon the authentication foundation established by
Version 1.0.

The Version 1.0 authentication mechanisms SHALL be treated as an
authentication foundation and SHALL NOT, by themselves, define the
principal inventive scope of Version 2.0.

The Version 2.0 design focus is the processing architecture that extends
beyond authentication, including:

- independent Entitlement representation;
- Entitlement lifecycle management;
- Entitlement evaluation;
- Policy-driven evaluation;
- Authorization Decision generation;
- Authorization Decision binding;
- authorization context propagation;
- distributed decision continuity;
- invalidation and revalidation; and
- binding of authorization to subsequent Service Execution.

The precise legal boundary between Version 1.0 and Version 2.0 SHALL be
documented separately in:

    design/patent_boundary.md

That document SHALL be treated as the source of the patent-specific
boundary analysis.

---

## 21. Design Freeze Scope

The following architectural principles are frozen for Version 2.0:

1. Transaction-centric processing.
2. Shared Transaction Object model.
3. Separation of Authentication and Entitlement.
4. Independent Entitlement representation.
5. Entitlement lifecycle management.
6. Policy-driven Entitlement and authorization evaluation.
7. Explicit Authorization Decision.
8. Authorization Decision binding to the applicable transaction and
   service context.
9. Authorization context continuity across distributed processing.
10. Revocation and revalidation of authorization dependencies.
11. Fail-closed behavior for unverifiable security conditions.
12. Separation of common framework and business-specific Service Profiles.
13. Evidence generation and audit traceability.

Any future implementation detail SHALL be evaluated against these frozen
principles.

---

## 22. Design Change Control

After this document is approved, a change that modifies any frozen
architectural principle SHALL be treated as a Version 2.0 design change.

Such a change SHALL be documented before it is incorporated into:

- protocol definitions;
- figures;
- patent analysis;
- claim mapping; or
- patent application documents.

A change SHALL NOT be introduced solely for the purpose of strengthening
a patent claim if the corresponding technical design is not part of the
approved Version 2.0 architecture.

---

## 23. Source and Derivation Rule

The following derivation order SHALL be used:

    Version 2.0 Technical Specification
              ↓
          Design Freeze
              ↓
        Protocol Definition
              ↓
       Invention Definition
              ↓
       Patent Boundary
              ↓
        Claim Mapping
              ↓
       Patent Claims
              ↓
      Patent Specification

Patent documents SHALL be derived from the frozen technical design.

The patent documents SHALL NOT silently introduce technical features
that are absent from the approved Version 2.0 design.

---

## 24. Relationship to Figures

The Version 2.0 repository contains a figure set representing the
architecture, protocol objects, Entitlement lifecycle, Policy
Evaluation, Authorization, distributed authorization, decision
continuity, invalidation, revocation, and revalidation.

The figure definitions SHALL remain consistent with this Design Freeze.

In particular, the following conceptual groups SHALL remain aligned:

### Core Architecture

- Figure 01 — Overall Architecture
- Figure 02 — Transaction Framework
- Figure 03 — Transaction Object

### Registration and Authentication

- Figure 04 — Registration Transaction
- Figure 05 — Registration State
- Figure 06 — Authentication Transaction
- Figure 07 — Authentication State Transition

### Security

- Figure 08 — Security Architecture
- Figure 09 — Threat Mitigation

### Protocol Data

- Figure 10 — Protocol Data Objects
- Figure 11 — Message Objects
- Figure 12 — Authentication Message Objects

### Entitlement and Authorization

- Figure 13 — Entitlement, Policy, Authorization
- Figure 14 — Authorization Decision
- Figure 15 — Entitlement Lifecycle
- Figure 16 — Entitlement Transaction Binding
- Figure 17 — Authorization Evaluation Model
- Figure 18 — Decision Enforcement State Transition
- Figure 19 — Failure Handling and Recovery Model
- Figure 20 — Audit Traceability Model
- Figure 21 — Protocol Global Lifecycle
- Figure 22 — Protocol Security Boundary

### Distributed Authorization and Context

- Figure 23 — Security Context Propagation
- Figure 24 — Context Assembly and Policy Decision
- Figure 25 — Decision Binding and Service Execution
- Figure 26 — Authorization Scope Propagation
- Figure 27 — Distributed Authorization Context Reconstruction
- Figure 28 — Authorization Context Continuity
- Figure 29 — Authorization Decision State Continuity
- Figure 30 — Distributed Authorization Decision Consistency
- Figure 31 — Authorization Invalidation and Fail-Closed Propagation
- Figure 32 — Authorization Decision Dependency Precedence
- Figure 33 — Authorization Decision Binding and Service Execution
- Figure 34 — Authorization Decision State Lifecycle
- Figure 35 — Authorization Decision Revocation and Revalidation Propagation

The figures SHALL be treated as technical representations of the
Version 2.0 architecture and SHALL be interpreted together with the
corresponding protocol definitions.

---

## 25. Approval Statement

This document establishes the Version 2.0 Design Freeze baseline.

The technical architecture defined herein is the reference baseline for
subsequent Version 2.0 protocol documentation and patent preparation.

Any future change to the frozen architecture SHALL be explicitly
identified, reviewed, and versioned.

---

## 26. End of Design Freeze

**NEW-shot2play Technical Specification Version 2.0**

**Design Freeze — Approved Baseline**

End of Document.
