# NEW-shot2play Technical Specification Version 2.0
# Canonical Vocabulary

## 1. Document Status

| Item | Value |
|---|---|
| Specification | NEW-shot2play Technical Specification |
| Version | 2.0 |
| Document | Canonical Vocabulary |
| Status | DESIGN BASELINE |
| Purpose | Define the canonical terminology used by Version 2.0 |
| Authority | Design Freeze — Approved Baseline |

This document defines the canonical terminology used throughout
NEW-shot2play Technical Specification Version 2.0.

The definitions in this document SHALL be used consistently across:

- Design documents
- Protocol documents
- Figures
- Technical specification chapters
- Patent analysis
- Claim mapping
- Patent application documents

Where a term has a different meaning in Version 1.0 or in a legacy
document, the Version 2.0 canonical meaning defined herein SHALL take
precedence for Version 2.0 design and patent analysis.

This document defines terminology only.

It does not itself define patent claims.

---

# 2. Terminology Principles

## 2.1 Canonical Meaning

Each canonical term SHALL have one primary technical meaning.

Synonyms SHOULD NOT be introduced where they could create ambiguity
between distinct processing stages or security objects.

In particular, the following concepts SHALL remain distinct:

- Authentication
- Entitlement
- Policy
- Policy Evaluation
- Authorization
- Authorization Decision
- Service Execution

These concepts represent different responsibilities in the Version 2.0
architecture.

---

## 2.2 Authentication Is Not Authorization

Authentication determines whether an identity or credential has been
successfully verified.

Authorization determines whether a requested operation may proceed.

Successful Authentication SHALL NOT automatically imply successful
Authorization.

---

## 2.3 Entitlement Is Not Authentication

An Entitlement represents a right, qualification, condition, or other
service-related authorization input.

An Entitlement SHALL NOT be treated as an authentication credential.

A subject MAY be authenticated without possessing a required
Entitlement.

A subject MAY possess multiple Entitlements after successful
authentication.

---

## 2.4 Policy Is Not a Decision

A Policy defines rules or conditions.

Policy Evaluation applies those rules to available decision inputs.

The result of Policy Evaluation is an input to the Authorization stage.

A Policy SHALL NOT itself be treated as an Authorization Decision.

---

## 2.5 Authorization Decision Is Not Service Execution

An Authorization Decision determines whether execution is permitted.

Service Execution performs the requested business operation.

A granted Authorization Decision SHALL NOT itself constitute completion
of the business operation.

---

# 3. Core Architectural Terms

## 3.1 NEW-shot2play

**Definition**

The technical platform and protocol architecture defined by the
NEW-shot2play Technical Specification.

In Version 2.0, NEW-shot2play provides a transaction-centric framework
for authentication, entitlement processing, policy evaluation,
authorization, service execution, evidence generation, and audit.

---

## 3.2 Version 2.0

**Definition**

The distinct technical specification baseline extending the authentication
foundation of Version 1.0 with entitlement, policy, authorization, and
service-execution processing.

Version 2.0 SHALL be treated as a separate design and invention baseline.

---

## 3.3 Transaction Framework

**Definition**

The common processing architecture used to execute transactions across
different services.

The Transaction Framework defines the common processing model while
allowing application-specific behavior to be supplied through Service
Profiles.

The Transaction Framework is independent of any particular business
application.

---

## 3.4 Transaction

**Definition**

A bounded processing instance representing a requested operation or
business interaction within the Transaction Framework.

A Transaction has an identifiable lifecycle and SHALL be associated with
a Transaction Object.

A Transaction MAY contain or reference authentication, entitlement,
policy, authorization, execution, evidence, and audit information.

---

## 3.5 Transaction Object

**Definition**

The common information object representing a Transaction throughout its
processing lifecycle.

The Transaction Object provides the shared information model through
which the processing stages cooperate.

The Transaction Object MAY include or reference:

- Transaction Identifier
- Transaction Token
- Status
- Service Profile
- Policy
- Entitlement information
- Context
- Authentication Result
- Authorization Decision
- Execution Result
- Evidence
- Audit Information
- Expiration information

Each processing stage SHALL modify only the information under its
defined responsibility.

---

## 3.6 Transaction Identifier

**Definition**

A unique identifier assigned to a Transaction.

The Transaction Identifier is used to associate requests, responses,
objects, decisions, and evidence with the corresponding Transaction.

A Transaction Identifier SHALL NOT by itself constitute an authentication
credential.

---

## 3.7 Transaction Token

**Definition**

A temporary token associated with a Transaction and used to establish
or correlate a transaction-specific processing context.

A Transaction Token MAY be one-time or otherwise constrained according
to the applicable protocol.

A Transaction Token SHALL NOT automatically be treated as a reusable
authentication credential.

---

## 3.8 Transaction Context

**Definition**

The information associated with the current Transaction that is required
for processing, evaluation, authorization, execution, security, or
audit.

Transaction Context MAY include identity, service, transaction,
Entitlement, Policy, authorization, environmental, and security
information.

---

## 3.9 Transaction State

**Definition**

The current processing state of a Transaction.

A Transaction State SHALL change only when the conditions defined by the
applicable protocol are satisfied.

---

## 3.10 Service Profile

**Definition**

A service-specific configuration defining the business behavior and
processing requirements applied to a Transaction.

A Service Profile MAY define:

- Required Authentication
- Required Entitlements
- Applicable Policies
- Authorization conditions
- Execution procedures
- Required Evidence
- Audit requirements
- Service-specific constraints

A Service Profile SHALL remain logically separate from the common
Transaction Framework.

---

# 4. Identity and Credential Terms

## 4.1 Identity

**Definition**

The subject identity associated with a Transaction or security operation.

Identity MAY refer to a user, device, account, organizational entity, or
other subject recognized by the applicable service.

Identity SHALL NOT be equated with a Credential.

---

## 4.2 Credential

**Definition**

A security object or cryptographic credential used to establish or verify
an identity.

In the Version 1.0 authentication foundation, a Credential may include
a WebAuthn credential associated with a registered device.

A Credential SHALL remain distinct from an Entitlement.

---

## 4.3 Credential Identifier

**Definition**

An identifier used to reference a registered Credential.

A Credential Identifier SHALL NOT itself prove possession of the
corresponding private key.

---

## 4.4 Private Key

**Definition**

The private cryptographic key associated with a public-key Credential.

The private key SHALL remain under the control of the registered
credential device or equivalent protected execution environment.

The Authentication Server SHALL NOT receive or store the private key.

---

## 4.5 Public Key

**Definition**

The public cryptographic key corresponding to a private key used for
cryptographic verification.

The Authentication Server MAY store the public key associated with a
registered Credential.

---

## 4.6 Challenge

**Definition**

A transaction-specific cryptographic value generated for a
challenge-response operation.

A Challenge SHALL have limited validity and SHALL be accepted only under
the conditions defined by the Authentication Protocol.

---

## 4.7 Authentication Response

**Definition**

The response generated by an authenticating party in response to an
Authentication Challenge.

The Authentication Response MAY contain a cryptographic signature or
other protocol-defined verification material.

---

# 5. Authentication Terms

## 5.1 Authentication

**Definition**

The process of verifying an identity or credential according to the
applicable Authentication Protocol.

Authentication establishes an Authentication Result.

Authentication does not by itself establish an Authorization Decision.

---

## 5.2 Authentication Transaction

**Definition**

A Transaction specifically used to perform Authentication.

An Authentication Transaction SHALL have its own transaction state,
validity period, Challenge, and verification requirements.

---

## 5.3 Authentication Result

**Definition**

The result produced by successful or unsuccessful Authentication
processing.

An Authentication Result MAY indicate:

- Authenticated
- Failed
- Expired
- Invalid
- Other protocol-defined states

An Authentication Result SHALL be treated as an input to subsequent
processing where Authentication is required.

---

## 5.4 Authenticated State

**Definition**

A Transaction or authentication context state indicating that the
required Authentication procedure has successfully completed.

Authenticated State SHALL NOT automatically mean that the requested
service is authorized.

---

## 5.5 Authentication Server

**Definition**

The trusted server-side component responsible for processing and
verifying Authentication according to the Authentication Protocol.

The Authentication Server MAY validate:

- Transaction state
- Transaction Identifier
- Challenge
- Credential
- Signature
- Expiration
- Replay conditions
- Authentication Policy

---

# 6. Entitlement Terms

## 6.1 Entitlement

**Definition**

A managed representation of a right, qualification, condition, or
service-related authorization input associated with a subject or other
defined entity.

An Entitlement MAY be created, issued, stored, evaluated, consumed,
expired, revoked, or revalidated according to the applicable protocol
and Policy.

An Entitlement is independent from the authentication credential.

---

## 6.2 Entitlement Identifier

**Definition**

A unique identifier assigned to an Entitlement.

The Entitlement Identifier SHALL permit an Entitlement to be referenced
during evaluation, consumption, audit, revocation, or revalidation.

---

## 6.3 Entitlement Issuer

**Definition**

The trusted component or service authorized to create or issue an
Entitlement.

The issuer MAY be different from the service that later evaluates or
consumes the Entitlement.

---

## 6.4 Entitlement Holder

**Definition**

The subject or other authorized entity to which an Entitlement is
associated.

An Entitlement MAY be bound to a user, account, device, transaction,
service, or other subject as defined by the applicable Policy.

---

## 6.5 Entitlement Scope

**Definition**

The set of services, operations, resources, or conditions for which an
Entitlement may be used.

An Entitlement SHALL NOT automatically apply to every service.

Its applicability SHALL be determined by its scope and the applicable
Policy.

---

## 6.6 Entitlement Condition

**Definition**

A condition that must be satisfied for an Entitlement to be valid,
usable, or applicable.

Conditions MAY include:

- Time
- Service
- Transaction
- Subject
- Context
- Usage count
- Policy
- Other defined conditions

---

## 6.7 Entitlement Lifecycle

**Definition**

The lifecycle through which an Entitlement is created, becomes usable,
is evaluated, consumed, expires, is revoked, or otherwise becomes
invalid.

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

Additional states MAY be defined.

---

## 6.8 Entitlement Acquisition

**Definition**

The process through which an Entitlement is obtained or issued to a
subject or authorized entity.

---

## 6.9 Entitlement Activation

**Definition**

The transition by which an acquired Entitlement becomes usable under
the applicable conditions.

Activation MAY be immediate or condition-dependent.

---

## 6.10 Entitlement Evaluation

**Definition**

The process of determining whether an Entitlement satisfies the
requirements applicable to a Transaction or Service Profile.

Entitlement Evaluation MAY consider:

- Entitlement status
- Scope
- Conditions
- Expiration
- Revocation
- Subject
- Transaction
- Service
- Context
- Usage state

---

## 6.11 Entitlement Consumption

**Definition**

The process of using an Entitlement in a manner that reduces or
terminates its remaining availability according to its usage conditions.

Consumption MAY be:

- single-use;
- count-based;
- quantity-based;
- time-based; or
- otherwise Policy-controlled.

---

## 6.12 Entitlement Expiration

**Definition**

The transition by which an Entitlement becomes invalid because its
validity period has ended.

An expired Entitlement SHALL NOT satisfy an authorization condition.

---

## 6.13 Entitlement Revocation

**Definition**

The explicit invalidation of an otherwise potentially valid Entitlement
before or at the end of its normal validity period.

A revoked Entitlement SHALL NOT satisfy an authorization condition.

---

## 6.14 Entitlement Revalidation

**Definition**

The process of determining whether an Entitlement that is currently
being relied upon remains valid under the latest applicable conditions.

Revalidation MAY be triggered by:

- Time
- Policy change
- Revocation
- Security event
- Context change
- Service requirement

---

## 6.15 Entitlement Binding

**Definition**

The association of an Entitlement with one or more defined objects or
contexts, such as:

- Subject
- Transaction
- Service
- Policy
- Context
- Authorization Decision

Binding restricts the circumstances in which the Entitlement may be
used.

---

## 6.16 Cross-Service Entitlement

**Definition**

An Entitlement issued or obtained in one service or transaction context
and subsequently evaluated or consumed by another service or transaction
context.

Cross-Service Entitlement does not require transfer of the underlying
authentication Credential.

Its use SHALL be controlled by Scope and Policy.

---

# 7. Legacy Rights Terminology

## 7.1 Rights

**Definition**

A legacy or descriptive term appearing in earlier Version 2.0 design
materials to represent a service-related right or required condition.

For canonical Version 2.0 architecture and patent analysis, the more
precise managed-object concept SHALL be expressed as **Entitlement**
where the meaning is an independently managed right or qualification.

The term Rights MAY remain in existing technical documents where it is
part of an already established field or description.

Such usage SHALL NOT automatically be interpreted as a separate
architectural object from Entitlement.

---

## 7.2 Rights Evaluation

**Definition**

A legacy or descriptive term for evaluation of service-related rights.

Where the evaluated item is an independently managed Version 2.0
Entitlement, the canonical term SHALL be:

**Entitlement Evaluation**

Existing documents containing the term Rights Evaluation SHALL be
interpreted consistently with the surrounding technical definition.

A later document SHALL NOT introduce ambiguity by treating Rights
Evaluation and Entitlement Evaluation as unrelated mechanisms unless a
new design explicitly defines such a distinction.

---

# 8. Policy Terms

## 8.1 Policy

**Definition**

A configurable set of rules, requirements, conditions, or decision logic
used during Transaction processing.

A Policy MAY determine:

- Authentication requirements
- Entitlement requirements
- Evaluation conditions
- Authorization requirements
- Execution conditions
- Evidence requirements
- Audit requirements

Policy is configuration or decision logic, not the decision result itself.

---

## 8.2 Authentication Policy

**Definition**

A Policy defining requirements for successful Authentication.

---

## 8.3 Entitlement Policy

**Definition**

A Policy defining requirements for Entitlement acquisition, evaluation,
usage, consumption, expiration, revocation, or revalidation.

---

## 8.4 Authorization Policy

**Definition**

A Policy defining the conditions under which a requested operation may
be authorized.

---

## 8.5 Execution Policy

**Definition**

A Policy defining conditions governing Service Execution.

---

## 8.6 Audit Policy

**Definition**

A Policy defining requirements for evidence generation, retention, or
audit recording.

---

## 8.7 Policy Version

**Definition**

An identifier representing a particular version of a Policy.

An Authorization Decision MAY be bound to a Policy Version so that a
subsequent Policy change can be detected.

---

# 9. Policy Evaluation Terms

## 9.1 Policy Evaluation

**Definition**

The process of applying a Policy to the applicable decision inputs and
producing a decision-relevant evaluation result.

Policy Evaluation MAY use:

- Authentication Result
- Entitlement state
- Subject attributes
- Transaction attributes
- Service attributes
- Context
- Time
- Device information
- Security state
- Other authorized inputs

---

## 9.2 Policy Evaluation Result

**Definition**

The result produced by Policy Evaluation.

The result MAY indicate:

- Allow
- Deny
- Conditional
- Insufficient information
- Revalidation required
- Other defined states

The Policy Evaluation Result SHALL be distinct from the final
Authorization Decision unless the applicable architecture explicitly
defines them as equivalent for a particular operation.

---

## 9.3 Decision Input

**Definition**

Information that is authorized for use in determining a Policy Evaluation
Result or Authorization Decision.

Decision Inputs MAY include authentication, entitlement, policy,
transaction, service, context, security, and time information.

---

## 9.4 Decision Context

**Definition**

The set of trusted contextual information used by Policy Evaluation or
Authorization.

Decision Context MAY include:

- Subject
- Transaction
- Service
- Entitlement
- Policy
- Device
- Time
- Location
- Security state
- Other authorized environmental information

---

# 10. Authorization Terms

## 10.1 Authorization

**Definition**

The processing function that determines whether a requested Service
Execution is permitted.

Authorization uses the applicable Authentication Result, Entitlement
Evaluation, Policy Evaluation, context, and other authorized decision
inputs.

---

## 10.2 Authorization Request

**Definition**

A request for an Authorization Decision concerning a specified service,
operation, resource, or Transaction.

---

## 10.3 Authorization Decision

**Definition**

The explicit decision produced by the Authorization stage indicating
whether a requested operation may proceed.

An Authorization Decision MAY include:

- Decision state
- Subject
- Transaction
- Service
- Scope
- Policy Version
- Entitlement dependencies
- Context dependencies
- Validity period
- Decision evidence

---

## 10.4 Authorization Decision State

**Definition**

The current validity and processing state of an Authorization Decision.

Principal states include:

- Granted
- Denied
- Invalid
- Pending Revalidation
- Revoked

Additional states MAY be defined.

---

## 10.5 Granted

**Definition**

An Authorization Decision state indicating that the requested Service
Execution is permitted subject to the decision's scope and validity
conditions.

Granted SHALL NOT mean permanently authorized.

---

## 10.6 Denied

**Definition**

An Authorization Decision state indicating that the requested Service
Execution is not permitted.

---

## 10.7 Invalid

**Definition**

An Authorization Decision state indicating that a previously generated
decision can no longer be relied upon.

---

## 10.8 Pending Revalidation

**Definition**

An Authorization Decision state indicating that required dependencies
must be revalidated before execution may proceed.

---

## 10.9 Revoked

**Definition**

An Authorization Decision state indicating that the decision has been
explicitly invalidated.

---

## 10.10 Authorization Scope

**Definition**

The set of services, operations, resources, or actions to which an
Authorization Decision applies.

An Authorization Decision SHALL NOT automatically authorize operations
outside its defined scope.

---

## 10.11 Authorization Decision Binding

**Definition**

The association of an Authorization Decision with the Transaction,
Subject, Service, Scope, Policy, Entitlement, Context, or other
dependencies upon which the decision was based.

Binding prevents a decision generated for one context from being
implicitly reused in an unrelated context.

---

## 10.12 Authorization Dependency

**Definition**

A condition or object whose validity is necessary for an Authorization
Decision to remain valid.

Authorization Dependencies MAY include:

- Authentication Result
- Entitlement
- Policy
- Policy Version
- Transaction
- Context
- Service
- Scope
- Security state

---

## 10.13 Authorization Dependency Precedence

**Definition**

The defined relationship among multiple Authorization Dependencies used
to determine whether a Decision remains valid.

Where a higher-priority dependency becomes invalid, dependent
Authorization Decisions SHALL be invalidated or revalidated according
to the applicable protocol.

---

# 11. Distributed Authorization Terms

## 11.1 Authorization Context

**Definition**

The trusted information required by a processing component to evaluate
whether an Authorization Decision applies to the current operation.

---

## 11.2 Authorization Context Propagation

**Definition**

The controlled transmission of Authorization Context between components
participating in distributed processing.

---

## 11.3 Authorization Context Reconstruction

**Definition**

The process of reconstructing or verifying sufficient authorization
information at a receiving component before relying on an Authorization
Decision.

Receipt of a message SHALL NOT by itself establish a valid
Authorization Context.

---

## 11.4 Authorization Context Continuity

**Definition**

The preservation of the relevant authorization dependencies as a
Transaction or operation moves between distributed processing
components.

Continuity MAY include:

- Subject continuity
- Transaction continuity
- Service continuity
- Entitlement continuity
- Policy continuity
- Scope continuity
- Context continuity
- Decision-state continuity

---

## 11.5 Authorization Decision Continuity

**Definition**

The property that an Authorization Decision remains logically
consistent and applicable across distributed processing stages while all
required dependencies remain valid.

---

## 11.6 Distributed Authorization

**Definition**

Authorization processing in which decision generation, propagation,
verification, or enforcement is performed by multiple cooperating
components.

Distributed Authorization SHALL preserve the trust and binding
requirements of the Authorization Decision.

---

## 11.7 Decision Consistency

**Definition**

The property that distributed components derive or enforce compatible
Authorization Decisions from the same valid decision dependencies.

---

## 11.8 Fail Closed

**Definition**

A security behavior in which failure to establish a required
authorization condition results in denial or non-execution rather than
implicit authorization.

---

# 12. Service Execution Terms

## 12.1 Service

**Definition**

A business or technical operation exposed through the Transaction
Framework.

---

## 12.2 Service Execution

**Definition**

The actual execution of a business operation after required processing
and Authorization conditions have been satisfied.

---

## 12.3 Execution Result

**Definition**

The result generated by Service Execution.

Execution Result SHALL remain distinct from Authorization Decision.

A service MAY be authorized but fail during execution.

---

## 12.4 Protected Operation

**Definition**

A Service Execution that requires an Authorization Decision before
execution.

---

# 13. Context Terms

## 13.1 Context

**Definition**

Runtime or environmental information associated with a Transaction,
Policy Evaluation, Authorization, or Service Execution.

Context MAY include:

- Subject
- Device
- Service
- Transaction
- Time
- Location
- Network
- Security state
- Entitlement state
- Other authorized environmental information

---

## 13.2 Security Context

**Definition**

The subset of Context relevant to security decisions and enforcement.

Security Context SHALL be protected against unauthorized modification.

---

## 13.3 Context Assembly

**Definition**

The process of collecting and combining authorized Context information
required for Policy Evaluation or Authorization.

---

## 13.4 Trusted Context

**Definition**

Context information whose origin, integrity, and applicability have been
verified according to the applicable Trust Model.

---

# 14. Evidence and Audit Terms

## 14.1 Evidence

**Definition**

Information generated during processing that supports verification,
traceability, or later examination of an operation or decision.

Evidence MAY be generated for:

- Authentication
- Entitlement issuance
- Entitlement evaluation
- Policy Evaluation
- Authorization
- Service Execution
- Entitlement consumption
- Revocation
- Revalidation

---

## 14.2 Evidence Reference

**Definition**

A reference linking a Transaction, Entitlement, Decision, or other object
to associated Evidence.

---

## 14.3 Audit Information

**Definition**

Information recorded to preserve a trace of relevant processing events.

Audit Information SHALL support traceability of security-relevant
operations.

---

## 14.4 Audit Record

**Definition**

A discrete record representing a processing or security event retained
for audit purposes.

---

## 14.5 Audit Traceability

**Definition**

The ability to associate processing events, decisions, Entitlements,
Transactions, and Service Executions through retained audit information.

---

# 15. Security and Trust Terms

## 15.1 Trust Boundary

**Definition**

A logical boundary across which data or control passes between components
with different trust assumptions.

---

## 15.2 Trusted Component

**Definition**

A component that is authorized to perform a specified security or
processing function and whose relevant behavior is trusted under the
Trust Model.

---

## 15.3 Untrusted Input

**Definition**

Input whose origin, integrity, or validity has not yet been sufficiently
verified.

Untrusted Input SHALL NOT directly establish Authentication,
Entitlement, Policy, or Authorization state.

---

## 15.4 Security Event

**Definition**

An event that may affect the validity, trust, or security state of a
Transaction, Entitlement, Policy, or Authorization Decision.

---

## 15.5 Revocation

**Definition**

The explicit invalidation of a previously valid security or
authorization object.

Revocation MAY apply to:

- Credential
- Entitlement
- Policy
- Authorization Decision
- Context
- Other security-dependent object

---

## 15.6 Revalidation

**Definition**

The process of verifying that a previously established security or
authorization condition remains valid.

---

# 16. Lifecycle Terms

## 16.1 Acquire

**Definition**

The lifecycle action by which an object or right is obtained or issued.

---

## 16.2 Hold

**Definition**

A lifecycle state in which an object remains possessed or reserved but
has not yet been consumed.

---

## 16.3 Use

**Definition**

The lifecycle action in which an object or Entitlement is relied upon
for an operation.

---

## 16.4 Consume

**Definition**

The lifecycle action in which an Entitlement or usage-limited object is
used in a manner that reduces its remaining availability.

---

## 16.5 Expire

**Definition**

The lifecycle transition by which an object becomes invalid because a
defined validity period has ended.

---

## 16.6 Invalidate

**Definition**

The transition by which an object, decision, or context is no longer
permitted to be relied upon.

Invalidation MAY result from:

- Expiration
- Revocation
- Policy change
- Context change
- Security event
- Dependency failure
- Transaction completion

---

# 17. Decision and State Terms

## 17.1 Decision

**Definition**

A result produced by an evaluation process.

A Decision SHALL be distinguished from the Policy or rules that produced
it.

---

## 17.2 Decision State

**Definition**

The current state of a Decision, including its validity and applicability
to the associated operation.

---

## 17.3 Decision Validity

**Definition**

The condition under which a Decision may be relied upon.

Decision Validity MAY depend on:

- Time
- Transaction
- Subject
- Service
- Entitlement
- Policy
- Context
- Scope
- Security state

---

## 17.4 Decision Invalidation

**Definition**

The process by which a previously generated Decision ceases to be
reliable.

---

# 18. Processing Stage Terms

## 18.1 Create

**Definition**

The processing stage that creates and initializes a Transaction.

---

## 18.2 Authenticate

**Definition**

The processing stage that performs Authentication.

---

## 18.3 Evaluate Entitlement

**Definition**

The processing stage that determines whether required Entitlements
satisfy the applicable conditions.

---

## 18.4 Evaluate Policy

**Definition**

The processing stage that applies Policy to authorized decision inputs.

---

## 18.5 Authorize

**Definition**

The processing stage that generates or confirms an Authorization
Decision.

---

## 18.6 Execute

**Definition**

The processing stage that performs the requested Service Execution.

---

## 18.7 Collect Evidence

**Definition**

The processing stage that generates or records Evidence associated with
the Transaction or processing stages.

---

## 18.8 Audit

**Definition**

The processing stage that records required Audit Information.

---

## 18.9 Complete

**Definition**

The processing stage that finalizes the Transaction and establishes its
terminal processing state.

---

# 19. Canonical Concept Boundaries

The following boundaries SHALL be preserved.

| Concept A | Concept B | Required distinction |
|---|---|---|
| Identity | Credential | Identity is the subject; Credential is the security mechanism |
| Credential | Entitlement | Credential establishes authentication; Entitlement represents a service-related right or condition |
| Authentication | Authorization | Authentication verifies identity; Authorization permits operation |
| Authentication Result | Authorization Decision | Authentication result proves authentication status; Authorization Decision determines service permission |
| Entitlement | Policy | Entitlement is an evaluated object; Policy defines evaluation rules |
| Policy | Policy Evaluation | Policy defines rules; Policy Evaluation applies them |
| Policy Evaluation Result | Authorization Decision | Evaluation result is decision input; Authorization Decision controls execution |
| Authorization Decision | Service Execution | Decision permits operation; Execution performs operation |
| Entitlement | Authorization Decision | Entitlement may be a dependency of a Decision |
| Context | Security Context | Context is broader runtime information; Security Context is security-relevant Context |
| Evidence | Audit Record | Evidence supports verification; Audit Record preserves traceability |
| Revocation | Expiration | Revocation is explicit invalidation; Expiration results from validity ending |
| Invalidation | Revalidation | Invalidation removes reliance; Revalidation determines whether reliance may resume |

---

# 20. Canonical Processing Chain

The principal Version 2.0 processing relationship SHALL be expressed as:

    Transaction
         ↓
    Authentication
         ↓
    Authentication Result
         ↓
    Entitlement Evaluation
         ↓
    Policy Evaluation
         ↓
    Authorization
         ↓
    Authorization Decision
         ↓
    Service Execution
         ↓
    Evidence / Audit

The actual processing order MAY vary where the applicable Service Profile
requires a different sequence, provided that the security dependencies
defined by this specification are preserved.

---

# 21. Canonical Security Relationship

The canonical security relationship SHALL be understood as:

    Credential
        ↓
    Authentication
        ↓
    Authentication Result
        │
        ├──────────────┐
        ↓              ↓
    Entitlement    Context
        │              │
        └──────┬───────┘
               ↓
          Policy Evaluation
               ↓
       Authorization Decision
               ↓
         Service Execution

This representation is conceptual.

It does not require a particular physical deployment topology.

---

# 22. Patent Terminology Guidance

For patent analysis, the following terms SHOULD be preferred where the
corresponding technical concept is intended:

| Preferred term | Avoid using as an uncontrolled synonym |
|---|---|
| Entitlement | Rights, privilege, benefit, qualification |
| Entitlement Evaluation | Rights Evaluation |
| Authorization Decision | Permission Result, Access Result |
| Policy Evaluation | Rule Check, Policy Check |
| Service Execution | Business Processing |
| Transaction Object | Session Object, Request Object |
| Authorization Context | Access Context |
| Entitlement Consumption | Rights Deduction |
| Entitlement Revocation | Rights Cancellation |
| Revalidation | Recheck |

The alternatives in the right-hand column MAY be used descriptively
where appropriate, but SHALL NOT be introduced as independent technical
objects unless explicitly defined.

---

# 23. Legacy Document Interpretation

Existing Version 2.0 technical materials may contain terminology that
was introduced during earlier design stages.

Such terminology SHALL be interpreted according to the following rules:

1. A term that clearly describes an independently managed service right
   SHOULD be interpreted as Entitlement.
2. "Rights Evaluation" SHOULD be interpreted as Entitlement Evaluation
   where the evaluated object is an Entitlement.
3. "Authorization Result" SHOULD be interpreted as Authorization
   Decision where the result represents the explicit authorization
   decision.
4. Existing terminology SHALL NOT be retroactively changed merely for
   terminology consistency unless a separate revision is approved.
5. Patent documents SHALL use the canonical terminology defined in this
   document unless a legacy term is necessary to explain an existing
   document or implementation.

---

# 24. Naming Convention

Canonical terms SHALL use the following capitalization when used as
named technical concepts:

- Transaction
- Transaction Object
- Transaction Framework
- Service Profile
- Identity
- Credential
- Authentication
- Authentication Result
- Entitlement
- Entitlement Evaluation
- Policy
- Policy Evaluation
- Policy Evaluation Result
- Authorization
- Authorization Decision
- Authorization Scope
- Authorization Context
- Service Execution
- Evidence
- Audit Information
- Revocation
- Revalidation

Lowercase usage MAY be used when a term is used as ordinary prose
rather than as a defined technical concept.

---

# 25. Non-Canonical Terms

The following terms SHALL NOT be treated as independent Version 2.0
architectural objects unless explicitly introduced by a later approved
design:

- Login
- Permission
- Privilege
- Benefit
- Coupon
- Point
- Ticket
- Visit Record
- Attendance Record
- Business Rule
- Access Token
- Session Token

These terms MAY appear as application-specific representations or
examples.

They SHALL NOT replace the canonical architecture terms.

---

# 26. Application-Specific Terms

An application MAY define domain-specific objects.

Examples include:

- Coupon
- Point
- Ticket
- Membership
- Attendance
- Visit
- Discount
- Reservation
- Benefit

Such objects MAY generate, represent, or be associated with an
Entitlement.

However, the application-specific object SHALL remain distinct from the
canonical Entitlement object unless the Service Profile explicitly
defines the relationship.

---

# 27. Canonical Object Classification

The principal Version 2.0 objects SHALL be classified as follows.

| Category | Canonical Objects |
|---|---|
| Identity | Identity |
| Security Credential | Credential, Credential Identifier, Public Key, Private Key |
| Transaction | Transaction, Transaction Object, Transaction Identifier, Transaction Token |
| Authentication | Authentication Transaction, Authentication Result, Challenge, Authentication Response |
| Entitlement | Entitlement, Entitlement Identifier, Entitlement Scope, Entitlement Condition |
| Policy | Policy, Policy Version |
| Evaluation | Entitlement Evaluation Result, Policy Evaluation Result |
| Authorization | Authorization Request, Authorization Decision, Authorization Scope |
| Context | Context, Security Context, Authorization Context |
| Execution | Service, Service Profile, Service Execution, Execution Result |
| Evidence | Evidence, Evidence Reference |
| Audit | Audit Information, Audit Record |
| Security State | Revocation, Revalidation, Invalidation |

---

# 28. Canonical Responsibility Separation

The following responsibility separation SHALL be preserved.

| Component / Stage | Primary responsibility |
|---|---|
| Authentication | Verify identity or credential |
| Entitlement Management | Create, maintain, and manage Entitlements |
| Entitlement Evaluation | Determine whether Entitlements satisfy conditions |
| Policy Evaluation | Apply configurable rules to decision inputs |
| Authorization | Generate or enforce Authorization Decisions |
| Service Execution | Perform the business operation |
| Evidence | Preserve verifiable processing information |
| Audit | Preserve processing traceability |

No stage SHALL silently assume the primary responsibility of another
stage.

---

# 29. Final Canonical Statement

The central conceptual distinction of Version 2.0 is:

    Authentication
        = "Who or what has been verified?"

    Entitlement
        = "What right, qualification, or condition is associated
           with the subject or transaction?"

    Policy
        = "What rules determine whether the condition is satisfied?"

    Policy Evaluation
        = "What is the result when those rules are applied?"

    Authorization
        = "May the requested operation proceed?"

    Authorization Decision
        = "What explicit decision governs execution?"

    Service Execution
        = "What business operation is actually performed?"

These concepts SHALL remain distinct throughout the Version 2.0
technical specification and patent preparation.

---

# 30. Document Control

This document is subordinate to:

    design/design_freeze_v2_approved.md

and SHALL serve as the canonical terminology reference for:

    protocol/
    patent/

Any conflict between a later document and this vocabulary SHALL be
reviewed before the later document is treated as authoritative.

A terminology change that changes the technical meaning of a defined
object or processing stage SHALL be treated as a Design Change under the
Version 2.0 Design Freeze.

---

## End of Document

NEW-shot2play Technical Specification Version 2.0
Canonical Vocabulary
