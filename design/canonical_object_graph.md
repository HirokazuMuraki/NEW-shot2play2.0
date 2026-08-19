# NEW-shot2play Technical Specification Version 2.0
# Canonical Object Graph

## 1. Document Status

| Item | Value |
|---|---|
| Specification | NEW-shot2play Technical Specification |
| Version | 2.0 |
| Document | Canonical Object Graph |
| Status | DESIGN BASELINE |
| Purpose | Define the canonical object relationships and dependencies of Version 2.0 |
| Authority | Design Freeze — Approved Baseline |
| Terminology Authority | design/canonical_vocabulary.md |

This document defines the canonical object relationships of
NEW-shot2play Technical Specification Version 2.0.

The purpose of this document is to establish how the principal
technical objects are related, bound, referenced, evaluated, invalidated,
and propagated during processing.

This document is concerned with object relationships.

Terminology SHALL be interpreted according to:

    design/canonical_vocabulary.md

Architectural boundaries SHALL be interpreted according to:

    design/design_freeze_v2_approved.md

This document does not itself define patent claims.

---

# 2. Object Graph Principles

## 2.1 Object-Centric Architecture

Version 2.0 SHALL be understood as an object-centric transaction
architecture.

A Transaction provides the common processing context.

Objects associated with a Transaction SHALL retain their defined
identity, lifecycle, and dependency relationships throughout the
processing stages.

---

## 2.2 Object Identity

Each independently managed object SHALL have an identifier or an
equivalent means of unambiguous reference.

Principal identifiable objects include:

- Transaction
- Transaction Object
- Credential
- Entitlement
- Policy
- Authorization Decision
- Evidence
- Audit Record

An object identifier SHALL NOT automatically constitute proof of
ownership, authenticity, or authorization.

---

## 2.3 Object Reference

An object MAY reference another object without containing the complete
representation of that object.

Reference relationships SHALL preserve sufficient information to
determine:

- Which object is referenced
- Whether the reference is valid
- Whether the referenced object remains applicable
- Which security dependencies apply

---

## 2.4 Object Ownership

Ownership SHALL be distinguished from reference.

A component that references an object SHALL NOT automatically become the
owner of that object.

Ownership determines which component has responsibility for:

- Creation
- Maintenance
- State transition
- Revocation
- Expiration
- Revalidation

---

## 2.5 Object Dependency

An object dependency exists where the validity or applicability of one
object depends on another object.

For example:

    Authorization Decision
            ↓
       depends on
            ↓
       Entitlement

A dependency MAY be direct or indirect.

---

## 2.6 Object Graph Authority

The Canonical Object Graph SHALL describe logical relationships between
objects.

It SHALL NOT prescribe:

- A particular database
- A particular programming language
- A particular cloud provider
- A particular network topology
- A particular physical deployment

Logical relationships SHALL remain valid across different
implementations.

---

## 2.7 Separation of Logical Objects

The following objects SHALL remain conceptually distinct:

- Identity
- Credential
- Transaction
- Entitlement
- Policy
- Policy Evaluation Result
- Authorization Decision
- Service Execution
- Evidence
- Audit Record

Multiple logical objects MAY be represented within one physical data
structure.

Such representation SHALL NOT eliminate the logical distinctions
defined by this document.

---

## End of Part 1
# 3. Principal Object Classes

The principal Version 2.0 object classes are:

- Identity
- Credential
- Transaction
- Transaction Object
- Service Profile
- Entitlement
- Policy
- Policy Evaluation Result
- Authorization Decision
- Context
- Service
- Execution Result
- Evidence
- Audit Record

These objects SHALL remain conceptually distinct even where multiple
objects are represented within one physical data structure.

A physical implementation MAY combine multiple logical objects into a
single database record, message, or storage structure.

Such implementation choices SHALL NOT alter the logical distinctions
defined by this document.

---

# 4. High-Level Canonical Object Graph

The principal logical relationship among the Version 2.0 objects is:

Identity
  |
  v
Credential
  |
  v
Authentication
  |
  v
Authentication Result
  |
  v
Transaction
  |
  +--------------------+--------------------+
  |                    |                    |
  v                    v                    v
Entitlement          Policy              Context
  |                    |                    |
  v                    v                    |
Entitlement        Policy Evaluation         |
Evaluation              |                    |
  |                     +---------+----------+
  |                               |
  +-------------------------------+
                  |
                  v
       Authorization Decision
                  |
                  v
        Service Execution
                  |
                  v
          Execution Result
                  |
          +-------+-------+
          |               |
          v               v
       Evidence       Audit Record

This graph represents logical relationships.

It does not prescribe a particular physical deployment topology.

---

## 4.1 Graph Interpretation

The graph represents the logical dependency and processing relationships
among the principal Version 2.0 objects.

An arrow represents a logical relationship.

An arrow SHALL NOT automatically imply:

- Physical data transfer
- Database foreign-key implementation
- Network communication
- Object ownership
- Cryptographic trust
- Physical co-location

The precise meaning of each relationship SHALL be determined by the
object definitions and protocol requirements applicable to that
relationship.

---

## 4.2 Processing Direction

The principal processing direction is:

Authentication
  |
  v
Entitlement
  |
  v
Policy Evaluation
  |
  v
Authorization Decision
  |
  v
Service Execution

This direction represents logical processing dependency.

An implementation MAY perform individual evaluations asynchronously or
in parallel where the defined dependencies are preserved.

The processing model SHALL NOT permit Service Execution to bypass a
required Authorization Decision.

---

## 4.3 Transaction Correlation

The Transaction provides a common correlation context for the processing
objects.

The Transaction MAY be associated with:

- Identity
- Credential
- Authentication Result
- Entitlement
- Policy
- Context
- Authorization Decision
- Service
- Execution Result
- Evidence
- Audit Record

The Transaction Identifier MAY be used to correlate these objects.

Correlation SHALL NOT by itself establish authorization.

---

## End of Part 2A
# 5. Transaction-Centered Object Graph

## 5.1 Transaction as the Common Anchor

The Transaction SHALL be the common anchor for processing.

A Transaction represents a bounded processing instance within the
Version 2.0 protocol suite.

A Transaction MAY include or reference:

- Identity
- Credential
- Authentication Result
- Entitlement
- Policy
- Context
- Authorization Decision
- Service
- Execution Result
- Evidence
- Audit Record

The Transaction SHALL provide sufficient correlation information to
associate objects belonging to the same processing instance.

---

## 5.2 Transaction Object

The Transaction Object represents the managed state of a Transaction.

Conceptually, the Transaction Object includes or references:

- Transaction Identifier
- Transaction Token
- Transaction State
- Service Profile
- Authentication Result
- Entitlement References
- Policy References
- Context References
- Authorization Decision Reference
- Execution Result
- Evidence References
- Audit References

The Transaction Object MAY contain values directly.

The Transaction Object MAY instead contain references to independently
managed objects.

The choice between direct representation and reference representation
SHALL NOT alter the logical object relationships.

---

## 5.3 Transaction Identifier

The Transaction Identifier uniquely identifies a Transaction within the
scope in which the Transaction is managed.

The Transaction Identifier SHALL be usable for correlation of protocol
messages and associated objects.

The Transaction Identifier MAY be referenced by:

- Authentication processing
- Entitlement processing
- Policy Evaluation
- Authorization processing
- Service Execution
- Evidence generation
- Audit processing

A Transaction Identifier SHALL NOT itself constitute:

- An Authentication Result
- An Entitlement
- A Policy
- An Authorization Decision
- Proof of identity
- Proof of authorization

---

## 5.4 Transaction Identifier Relationship

The logical relationship may be represented as:

Transaction Identifier
  |
  +-- Authentication
  |
  +-- Authentication Result
  |
  +-- Entitlement Evaluation
  |
  +-- Policy Evaluation
  |
  +-- Authorization Decision
  |
  +-- Service Execution
  |
  +-- Evidence
  |
  +-- Audit Record

The Transaction Identifier provides correlation.

It SHALL NOT replace the semantic validation of the referenced objects.

---

## 5.5 Transaction Token

A Transaction Token MAY be associated with a Transaction.

The Transaction Token MAY provide a temporary reference to the
Transaction or a transaction-specific protocol state.

A Transaction Token MAY be used to:

- Correlate protocol messages
- Associate a request with a Transaction
- Establish transaction freshness
- Detect unauthorized reuse
- Support replay protection

The Transaction Token SHALL be distinguishable from the Transaction
Identifier where the two have different lifecycle or security
properties.

A Transaction Token SHALL NOT by itself constitute an Authorization
Decision.

---

## 5.6 Transaction Freshness

A Transaction MAY have a defined validity period.

Transaction freshness MAY be established using:

- Time-based expiration
- Nonce values
- One-time-use tokens
- Transaction state
- Cryptographic binding
- Server-side state validation

A stale Transaction SHALL NOT automatically be considered valid merely
because its Transaction Identifier remains known.

---

## 5.7 Transaction State

A Transaction SHALL have a defined state.

A conceptual lifecycle is:

Created
  |
  v
Initialized
  |
  v
Authenticating
  |
  v
Authenticated
  |
  v
Evaluating
  |
  v
Authorizing
  |
  v
Authorized
  |
  v
Executing
  |
  v
Completed

The actual implementation MAY use additional states.

Failure, expiration, cancellation, revocation, or invalidation MAY cause
a Transaction to transition to a terminal or recovery state.

---

## 5.8 Transaction State and Processing Stage

Transaction State SHALL remain distinguishable from the state of any
individual protocol object.

For example, a Transaction MAY be in an Authorizing state while an
Authorization Decision is still being evaluated.

Likewise, a Transaction MAY be in an Executing state after an
Authorization Decision has already been granted.

The Transaction State therefore represents the state of the overall
processing instance rather than the state of one individual object.

---

## 5.9 Transaction Completion

Completion of a Transaction SHALL NOT automatically imply successful
business execution.

For example:

Transaction State = Completed

MAY coexist with:

Execution Result = Failed

where Completed indicates that the defined processing sequence has
terminated.

Business success SHALL be determined from the applicable Execution
Result.

---

## End of Part 2B
# 6. Identity and Credential Graph

## 6.1 Identity-Credential Relationship

The principal relationship is:

Identity
  |
  +-- associated with --> Credential

An Identity MAY have multiple Credentials.

A Credential MAY be associated with one or more defined Identity
representations where the applicable implementation permits such
association.

The association between an Identity and a Credential SHALL be
independently distinguishable from an Authentication Result.

---

## 6.2 Identity

An Identity represents the subject or entity to which authentication,
entitlement, policy, or authorization processing may apply.

An Identity MAY be represented by:

- An account identifier
- A subject identifier
- A service identifier
- A device-associated identity
- Another implementation-defined identifier

An Identity identifier SHALL NOT itself constitute proof that the
corresponding subject has been authenticated.

---

## 6.3 Credential

A Credential represents authentication material or an authentication
mechanism associated with an Identity.

A Credential MAY contain or reference:

- Credential Identifier
- Public Key
- Credential metadata
- Registration information
- Credential status
- Authentication-related parameters

A Credential SHALL remain conceptually distinct from:

- Identity
- Authentication Result
- Entitlement
- Authorization Decision

Possession or presentation of a Credential SHALL NOT automatically
constitute authorization.

---

## 6.4 Credential Identifier

A Credential Identifier identifies a Credential within its applicable
management scope.

The Credential Identifier MAY be referenced by:

- Registration
- Authentication
- Credential status management
- Audit records
- Evidence records

A Credential Identifier SHALL NOT itself be treated as secret
authentication material.

---

## 6.5 Public-Key Credential Relationship

A public-key Credential MAY be represented conceptually as:

Credential
  |
  +-- Credential Identifier
  |
  +-- Public Key
  |
  +-- Credential Metadata

The corresponding private cryptographic key is maintained by the
credential device or protected execution environment.

The server-side authentication component SHALL use the public
verification material or equivalent verification data.

---

## 6.6 Private Key Protection

The Private Key SHALL remain under control of the credential device or
protected execution environment.

The Authentication Server SHALL NOT require transmission of the Private
Key for ordinary Authentication.

A Private Key SHALL NOT be represented as an Entitlement.

A Private Key SHALL NOT itself constitute an Authorization Decision.

---

## 6.7 Credential Lifecycle

A Credential MAY progress through states such as:

Registered
  |
  v
Active
  |
  +----------+
  |          |
  v          v
Suspended  Revoked
  |
  v
Reactivated
  |
  v
Active

Expiration MAY independently invalidate a Credential.

A revoked Credential SHALL NOT be used to establish a new valid
Authentication Result.

---

## 6.8 Credential Replacement

A Credential MAY be replaced without creating a new Identity.

Conceptually:

Identity
  |
  +-- Credential A
  |
  +-- Credential B
  |
  +-- Credential C

The lifecycle of each Credential SHALL remain independently manageable.

Replacement of a Credential SHALL NOT automatically invalidate all
Entitlements associated with the Identity unless an applicable policy
requires such invalidation.

---

## 6.9 Authentication Relationship

Authentication establishes a processing relationship among:

- Identity
- Credential
- Authentication Challenge
- Authentication Response
- Transaction

Conceptually:

Identity
  |
  v
Credential
  |
  v
Authentication Challenge
  |
  v
Authentication Response
  |
  v
Authentication Result
  |
  v
Transaction

The Authentication Result represents the result of Authentication.

The Authentication Result SHALL remain distinct from an Authorization
Decision.

---

## 6.10 Authentication Result Dependency

An Authentication Result MAY be used as an input to subsequent
Entitlement, Policy Evaluation, or Authorization processing.

Conceptually:

Authentication Result
        |
        +------> Entitlement Evaluation
        |
        +------> Policy Evaluation
        |
        +------> Authorization Decision

Successful Authentication SHALL NOT automatically produce an
Authorization Decision.

---

## End of Part 3A
# 7. Authentication Result and Entitlement Graph

## 7.1 Authentication Result

An Authentication Result represents the result produced by an
Authentication process.

The Authentication Result MAY indicate:

- Authentication success
- Authentication failure
- Authentication status
- Authenticated Identity
- Credential reference
- Transaction reference
- Authentication method
- Authentication time
- Authentication assurance information

An Authentication Result SHALL remain associated with the Transaction
for which the Authentication was performed.

---

## 7.2 Authentication Result as an Input

An Authentication Result MAY be used as an input to subsequent
processing.

The relationship is:

Authentication Result
  |
  +---> Entitlement Evaluation
  |
  +---> Policy Evaluation
  |
  +---> Authorization Decision

The use of an Authentication Result as an input SHALL NOT convert the
Authentication Result into an Entitlement or Authorization Decision.

---

## 7.3 Authentication Result Validity

An Authentication Result MAY have a validity period.

Validity MAY depend on:

- Authentication time
- Transaction state
- Credential state
- Session state
- Security policy
- Reauthentication requirements

An Authentication Result that is no longer valid SHALL NOT be treated as
a current Authentication Result for processing that requires current
authentication.

---

## 7.4 Authentication Result and Credential State

The continued validity of an Authentication Result MAY depend on the
state of the associated Credential.

For example, a Credential that becomes revoked MAY cause a subsequent
validation of the Authentication Result to fail.

Such dependency SHALL be explicitly represented by the applicable
security or policy rules.

---

# 8. Entitlement Object Graph

## 8.1 Entitlement

An Entitlement represents a right, qualification, condition, or other
managed basis upon which access to a service or operation MAY be
permitted.

An Entitlement SHALL remain conceptually distinct from:

- Identity
- Credential
- Authentication Result
- Policy
- Authorization Decision
- Service Execution

An Entitlement MAY be created, issued, acquired, activated, evaluated,
consumed, expired, revoked, or revalidated.

---

## 8.2 Entitlement Identifier

An Entitlement Identifier identifies an Entitlement within its
applicable management scope.

The Entitlement Identifier MAY be referenced by:

- Transaction Objects
- Entitlement Evaluation
- Policy Evaluation
- Authorization Decisions
- Evidence
- Audit Records

The identifier SHALL NOT itself constitute proof that the Entitlement is
currently valid.

---

## 8.3 Entitlement Issuer

An Entitlement Issuer is the entity or component responsible for issuing
or creating an Entitlement.

Conceptually:

Entitlement Issuer
  |
  v
Entitlement
  |
  v
Entitlement Holder

The Issuer relationship SHALL remain distinct from the Holder
relationship.

---

## 8.4 Entitlement Holder

The Entitlement Holder is the subject or authorized entity to which an
Entitlement applies.

An Entitlement MAY be bound to:

- Identity
- Account
- Device
- Credential
- Transaction
- Service
- Other defined object

The binding rules SHALL determine the circumstances under which the
Entitlement may be used.

---

## 8.5 Entitlement Scope

An Entitlement MAY define a scope describing where or for what purpose
the Entitlement may be used.

An Entitlement Scope MAY identify:

- Service
- Service category
- Operation
- Resource
- Transaction type
- Geographic or environmental condition
- Time period
- Other applicable constraint

An Entitlement SHALL NOT automatically apply to every Service merely
because it is valid.

---

## 8.6 Entitlement Condition

An Entitlement Condition specifies a condition that must be satisfied
for an Entitlement to be considered applicable.

A condition MAY depend on:

- Identity
- Authentication Result
- Transaction
- Time
- Location
- Service
- Previous transaction
- External state
- Another Entitlement
- Policy

The evaluation of an Entitlement Condition SHALL be distinguishable
from the final Authorization Decision.

---

## End of Part 3B
# 9. Entitlement Lifecycle and Evaluation

## 9.1 Entitlement Lifecycle

An Entitlement MAY progress through a lifecycle such as:

Created
  |
  v
Issued
  |
  v
Acquired
  |
  v
Activated
  |
  v
Usable
  |
  +-------------------+
  |                   |
  v                   v
Consumed           Expired
  |                   |
  v                   v
Completed           Invalid
  |
  v
Closed

An Entitlement MAY also be revoked from an otherwise valid lifecycle
state.

Revocation SHALL cause the Entitlement to be treated as invalid for
subsequent processing where the applicable rules require revocation to
be effective.

---

## 9.2 Entitlement Acquisition

Entitlement Acquisition is the process through which an Entitlement is
obtained or issued to a Holder.

Acquisition MAY occur as a result of:

- Authentication
- Transaction completion
- Service usage
- Purchase
- Registration
- External verification
- Administrative issuance
- Another Entitlement
- Satisfaction of an Entitlement Condition

Acquisition SHALL NOT by itself imply that the Entitlement is active
or usable.

---

## 9.3 Entitlement Activation

Entitlement Activation is the transition by which an acquired
Entitlement becomes usable under its defined conditions.

Activation MAY require:

- Successful Authentication
- Confirmation of a Transaction
- Satisfaction of a Policy
- Verification of external state
- Explicit activation
- Time-based activation
- Other defined conditions

An acquired but inactive Entitlement SHALL NOT automatically satisfy a
condition requiring an active Entitlement.

---

## 9.4 Entitlement Evaluation

Entitlement Evaluation determines whether an Entitlement satisfies the
conditions applicable to a particular processing context.

The conceptual relationship is:

Entitlement
  |
  v
Entitlement Evaluation
  |
  +-- Transaction
  +-- Identity
  +-- Authentication Result
  +-- Entitlement Scope
  +-- Entitlement Condition
  +-- Credential State
  +-- Context
  |
  v
Entitlement Evaluation Result

The Entitlement Evaluation Result MAY indicate:

- Valid
- Invalid
- Applicable
- Not Applicable
- Active
- Inactive
- Expired
- Revoked
- Condition Satisfied
- Condition Not Satisfied

---

## 9.5 Entitlement Evaluation Result

An Entitlement Evaluation Result represents the result of evaluating
one or more Entitlements against a defined context.

The result MAY contain or reference:

- Entitlement Identifier
- Transaction Identifier
- Evaluation time
- Evaluation context
- Satisfied conditions
- Unsatisfied conditions
- Entitlement status
- Evaluation outcome
- Evidence references

The result SHALL remain distinguishable from the Authorization
Decision.

---

## 9.6 Multiple Entitlements

A Transaction MAY reference multiple Entitlements.

Conceptually:

Transaction
  |
  +-- Entitlement A
  |
  +-- Entitlement B
  |
  +-- Entitlement C
  |
  v
Entitlement Evaluation
  |
  v
Entitlement Evaluation Result

Multiple Entitlements MAY be evaluated independently.

The applicable Policy MAY require:

- Any one Entitlement
- All Entitlements
- A particular combination
- A minimum number
- A specific Entitlement type
- An ordered sequence
- A conditional relationship

The combination rule SHALL be determined by the applicable Policy.

---

## 9.7 Entitlement Dependency

An Entitlement MAY depend on another Entitlement.

Conceptually:

Entitlement A
  |
  | depends on
  v
Entitlement B

A dependent Entitlement SHALL NOT be considered independently valid
where the applicable rules require the dependency to remain valid.

Dependency relationships MAY be represented directly or by reference.

---

## 9.8 Entitlement Consumption

Entitlement Consumption represents use of an Entitlement in a manner
that changes its lifecycle or remaining availability.

Consumption MAY:

- Decrease a quantity
- Mark the Entitlement as used
- Transition the Entitlement to a consumed state
- Create evidence
- Update audit information
- Trigger a subsequent Policy Evaluation

Consumption SHALL be distinguishable from Authorization.

An Authorization Decision MAY permit Consumption, but the Decision does
not itself constitute Consumption.

---

## 9.9 Entitlement Expiration

An Entitlement MAY have an expiration condition.

Expiration MAY be determined by:

- Absolute time
- Relative time
- Transaction completion
- Number of uses
- External state
- Policy
- Other defined condition

An expired Entitlement SHALL NOT satisfy an authorization condition
requiring a currently valid Entitlement.

---

## 9.10 Entitlement Revocation

Entitlement Revocation explicitly invalidates an Entitlement.

A revoked Entitlement SHALL NOT satisfy an authorization condition
where revocation is effective.

Revocation MAY occur because of:

- Administrative action
- Security event
- Policy change
- Transaction cancellation
- Credential status
- External state change
- Fraud detection
- Other defined event

---

## End of Part 3C
# 10. Entitlement Revalidation and Binding

## 10.1 Entitlement Revalidation

Entitlement Revalidation is the process of determining whether an
Entitlement that was previously evaluated remains valid for subsequent
processing.

Revalidation MAY be required when:

- The Transaction continues for an extended period
- The Entitlement has a limited validity period
- The external state has changed
- The Policy has changed
- The Credential state has changed
- The Identity state has changed
- The Service context has changed
- A security event has occurred

The result of Revalidation SHALL be distinguishable from the original
Entitlement Evaluation Result.

---

## 10.2 Revalidation Relationship

The conceptual relationship is:

Existing Entitlement
        |
        v
Revalidation Context
        |
        v
Entitlement Revalidation
        |
        v
Revalidation Result

The Revalidation Result MAY indicate:

- Still Valid
- No Longer Valid
- Expired
- Revoked
- Condition Changed
- Context Changed
- Revalidation Failed

A previously valid Entitlement SHALL NOT be assumed to remain valid
when applicable rules require Revalidation.

---

## 10.3 Entitlement Binding

Entitlement Binding associates an Entitlement with one or more defined
objects or conditions.

An Entitlement MAY be bound to:

- Identity
- Account
- Credential
- Device
- Transaction
- Service
- Service Profile
- Resource
- Context
- Policy
- Another Entitlement

Binding restricts the circumstances in which the Entitlement may be
used.

---

## 10.4 Transaction Binding

An Entitlement MAY be bound to a specific Transaction.

Conceptually:

Entitlement
    |
    +-- bound to --> Transaction

A Transaction-bound Entitlement SHALL NOT automatically be usable in a
different Transaction.

A new Transaction MAY require a new Entitlement Evaluation.

---

## 10.5 Identity Binding

An Entitlement MAY be bound to an Identity.

Conceptually:

Identity
    |
    +-- bound to --> Entitlement

An Identity-bound Entitlement SHALL be evaluated against the Identity
participating in the current Transaction.

Presentation of the Entitlement Identifier without the required Identity
relationship SHALL NOT automatically establish entitlement validity.

---

## 10.6 Credential Binding

An Entitlement MAY be bound to a Credential.

Conceptually:

Credential
    |
    +-- bound to --> Entitlement

Credential binding MAY be used where an Entitlement is intended to be
usable only through a particular Credential or credential class.

Credential binding SHALL NOT cause the Credential itself to become an
Entitlement.

---

## 10.7 Service Binding

An Entitlement MAY be bound to a Service or Service Profile.

Conceptually:

Entitlement
    |
    +-- Service Scope --> Service Profile
                              |
                              v
                           Service

A Service-bound Entitlement SHALL be evaluated against the Service
identified by the current Transaction.

An Entitlement valid for one Service SHALL NOT automatically be assumed
to be valid for another Service.

---

## 10.8 Context Binding

An Entitlement MAY be bound to a Context.

A Context MAY contain information such as:

- Time
- Location
- Device state
- Transaction state
- Authentication state
- Service state
- Environmental information
- External verification results

Context binding permits the applicability of an Entitlement to depend
on conditions existing at the time of evaluation.

---

## 10.9 Cross-Service Entitlement

An Entitlement MAY be issued or acquired in one Service or Transaction
context and subsequently evaluated for use by another Service.

Such an Entitlement is a Cross-Service Entitlement.

Conceptually:

Service A
   |
   v
Entitlement
   |
   v
Cross-Service Evaluation
   |
   v
Service B

Cross-Service use SHALL be governed by the Entitlement Scope and
applicable Policy.

Cross-Service Entitlement SHALL NOT require transfer of the underlying
authentication credential.

---

## 10.10 Cross-Service Validation

A Cross-Service Entitlement MAY require validation of:

- Issuer
- Holder
- Entitlement status
- Original Transaction
- Entitlement Scope
- Current Service
- Current Transaction
- Current Context
- Applicable Policy

The receiving Service SHALL NOT be required to trust an Entitlement
solely because another Service previously accepted it.

---

## 10.11 Entitlement and Authorization

An Entitlement MAY be one input to an Authorization Decision.

The relationship is:

Entitlement
    |
    v
Entitlement Evaluation
    |
    v
Policy Evaluation
    |
    v
Authorization Decision

The existence of an Entitlement SHALL NOT automatically produce an
Authorization Decision.

The Authorization Decision SHALL be determined according to the
applicable Policy and all required decision inputs.

---

## End of Part 3D
# 11. Policy Object Graph

## 11.1 Policy

A Policy defines rules, conditions, constraints, or decision criteria
used to determine whether a requested operation may proceed.

A Policy MAY define:

- Required Authentication
- Required Entitlements
- Required Context
- Service restrictions
- Resource restrictions
- Operation restrictions
- Time restrictions
- Geographic restrictions
- Risk conditions
- Dependency conditions
- Precedence rules
- Failure handling rules
- Revalidation requirements

A Policy SHALL remain conceptually distinct from a Policy Evaluation
Result and an Authorization Decision.

---

## 11.2 Policy Identifier

A Policy Identifier identifies a Policy within its applicable management
scope.

The Policy Identifier MAY be referenced by:

- Service Profile
- Transaction Object
- Policy Evaluation
- Authorization Decision
- Evidence
- Audit Record

The Policy Identifier SHALL NOT itself constitute an Authorization
Decision.

---

## 11.3 Policy Version

A Policy MAY have a version.

A Policy Version identifies the particular rule set used for a Policy
Evaluation.

Conceptually:

Policy
  |
  +-- Policy Version A
  |
  +-- Policy Version B
  |
  +-- Policy Version C

The Policy Version used for an Authorization Decision SHOULD be
identifiable for evidence and audit purposes.

---

## 11.4 Policy Scope

A Policy MAY define a scope describing the circumstances in which the
Policy applies.

The scope MAY include:

- Service
- Resource
- Operation
- Transaction type
- Identity class
- Entitlement class
- Context
- Time period
- Geographic area
- Security level

A Policy SHALL be evaluated within its applicable scope.

---

## 11.5 Policy Evaluation

Policy Evaluation is the process of applying a Policy to defined
decision inputs.

The conceptual relationship is:

Policy
  |
  +-----------------------------+
  |                             |
  v                             v
Decision Inputs             Evaluation Rules
  |                             |
  +-------------+---------------+
                |
                v
         Policy Evaluation
                |
                v
     Policy Evaluation Result

Policy Evaluation MAY be performed by a dedicated Policy Evaluation
component or by another component implementing the required semantics.

---

## 11.6 Policy Evaluation Inputs

Policy Evaluation MAY use one or more of the following inputs:

- Identity
- Credential state
- Authentication Result
- Entitlement
- Entitlement Evaluation Result
- Transaction
- Transaction State
- Service Profile
- Service
- Requested Operation
- Resource
- Context
- Environmental information
- External verification result
- Previous decision state
- Security state

The applicable Policy SHALL determine which inputs are required.

---

## 11.7 Decision Context

The collection of inputs used for Policy Evaluation constitutes a
Decision Context.

Conceptually:

Decision Context
  |
  +-- Identity
  +-- Authentication Result
  +-- Entitlement Evaluation Result
  +-- Transaction
  +-- Service Profile
  +-- Requested Operation
  +-- Resource
  +-- Environmental Context
  +-- Security Context
  |
  v
Policy Evaluation

The Decision Context MAY be assembled from multiple sources.

The resulting Policy Evaluation SHALL remain bound to the Transaction
for which the evaluation was performed.

---

## 11.8 Policy Evaluation Result

The Policy Evaluation Result represents the result produced by applying
a Policy to the applicable Decision Context.

The result MAY indicate:

- Permit
- Deny
- Condition Satisfied
- Condition Not Satisfied
- Indeterminate
- Insufficient Information
- Revalidation Required

The Policy Evaluation Result SHALL remain distinct from the final
Authorization Decision.

---

## 11.9 Policy Evaluation and Authorization

Policy Evaluation MAY provide one or more inputs to Authorization.

The logical relationship is:

Decision Context
      |
      v
Policy Evaluation
      |
      v
Policy Evaluation Result
      |
      v
Authorization Evaluation
      |
      v
Authorization Decision

A Policy Evaluation Result SHALL NOT automatically constitute an
Authorization Decision.

The Authorization stage MAY consider additional inputs and rules.

---

## 11.10 Policy Failure

A Policy Evaluation MAY fail to produce a definitive result.

Possible causes include:

- Missing decision input
- Unavailable external information
- Invalid context
- Policy version mismatch
- Evaluation error
- Security validation failure
- Expired decision context

Where the applicable Policy requires fail-closed behavior, an
indeterminate or failed Policy Evaluation SHALL NOT result in
unauthorized Service Execution.

---

## End of Part 4A
# 12. Authorization Decision Object Graph

## 12.1 Authorization

Authorization is the process of determining whether a requested Service
operation is permitted under the applicable decision rules and inputs.

Authorization SHALL remain distinct from:

- Authentication
- Entitlement
- Policy
- Policy Evaluation
- Service Execution

Authorization produces or establishes an Authorization Decision.

---

## 12.2 Authorization Decision

An Authorization Decision represents the result of Authorization
processing for a defined Transaction and requested operation.

An Authorization Decision MAY indicate:

- Permit
- Deny
- Conditional Permit
- Indeterminate
- Revalidation Required
- Additional Authentication Required
- Additional Entitlement Required

The Authorization Decision SHALL be associated with the Transaction for
which it was produced.

---

## 12.3 Authorization Decision Inputs

An Authorization Decision MAY depend on:

- Identity
- Authentication Result
- Credential state
- Entitlement Evaluation Result
- Policy Evaluation Result
- Transaction state
- Service Profile
- Requested Operation
- Resource
- Context
- Security state
- External verification result
- Previous Authorization Decision
- Revalidation result

The applicable Policy SHALL determine the required inputs.

---

## 12.4 Authorization Evaluation

Authorization Evaluation determines whether the collected decision
inputs satisfy the requirements applicable to the requested operation.

The conceptual relationship is:

Identity
       |
Authentication Result
       |
Entitlement Evaluation Result
       |
Policy Evaluation Result
       |
Transaction Context
       |
Service Profile
       |
Requested Operation
       |
       v
Authorization Evaluation
       |
       v
Authorization Decision

Authorization Evaluation MAY use additional implementation-defined
inputs where permitted by the applicable Policy.

---

## 12.5 Authorization Decision and Service Execution

The Authorization Decision controls whether Service Execution may
proceed.

The relationship is:

Authorization Decision
       |
       | Permit
       v
Service Execution

A Deny Authorization Decision SHALL prevent the corresponding Service
Execution where the decision is authoritative for that operation.

A Permit Authorization Decision SHALL NOT itself constitute Service
Execution.

---

## 12.6 Conditional Authorization

An Authorization Decision MAY contain conditions.

A Conditional Permit MAY require:

- A defined Entitlement
- A defined Context
- A defined Transaction State
- Additional Authentication
- Additional Policy Evaluation
- Revalidation
- A time limitation
- A resource limitation
- A usage limitation

The Service SHALL enforce applicable conditions before or during
Service Execution.

---

## 12.7 Authorization Decision Scope

An Authorization Decision SHALL have an applicable scope.

The scope MAY identify:

- Transaction
- Service
- Service Profile
- Resource
- Operation
- Entitlement
- Context
- Time period
- Other defined authorization boundary

An Authorization Decision SHALL NOT automatically apply outside its
defined scope.

---

## 12.8 Authorization Decision Binding

An Authorization Decision MAY be bound to:

- Transaction Identifier
- Transaction State
- Service
- Service Profile
- Requested Operation
- Resource
- Entitlement Evaluation Result
- Policy Version
- Decision Context

Binding permits the implementation to determine whether a previously
generated Authorization Decision remains applicable.

---

## 12.9 Decision Freshness

An Authorization Decision MAY have a validity period.

Freshness MAY be established using:

- Timestamp
- Expiration time
- Transaction state
- Context version
- Policy version
- Revalidation
- One-time-use state
- Cryptographic binding
- Server-side state

A stale Authorization Decision SHALL NOT be treated as current where
the applicable Policy requires a current decision.

---

## 12.10 Decision Revalidation

An Authorization Decision MAY require revalidation before Service
Execution.

The relationship is:

Existing Authorization Decision
          |
          v
Decision Revalidation
          |
          v
Revalidation Result
          |
          v
Current Authorization State

A previously granted Authorization Decision SHALL NOT necessarily remain
valid when its dependent inputs or conditions have changed.

---

## 12.11 Decision Revocation

An Authorization Decision MAY be revoked or invalidated before Service
Execution or during an ongoing processing sequence.

Revocation MAY result from:

- Entitlement revocation
- Credential revocation
- Policy change
- Security event
- Transaction cancellation
- Context change
- Service state change
- Explicit administrative action

Where the applicable Policy requires immediate invalidation, the
Service SHALL NOT continue execution based solely on the invalidated
Decision.

---

## End of Part 4B
# 13. Authorization Decision Dependency and Precedence

## 13.1 Decision Dependency

An Authorization Decision MAY depend on multiple decision inputs.

A dependency exists where the validity or outcome of one input affects
the validity or outcome of the Authorization Decision.

Conceptually:

Authentication Result
        |
        v
Entitlement Evaluation Result
        |
        v
Policy Evaluation Result
        |
        v
Authorization Evaluation
        |
        v
Authorization Decision

The dependency relationship SHALL remain distinguishable from the
physical implementation of the participating components.

---

## 13.2 Dependency Chain

A dependency chain MAY contain multiple levels.

For example:

Identity
  |
  v
Authentication
  |
  v
Authentication Result
  |
  v
Entitlement Evaluation
  |
  v
Entitlement Evaluation Result
  |
  v
Policy Evaluation
  |
  v
Policy Evaluation Result
  |
  v
Authorization Evaluation
  |
  v
Authorization Decision

A failure or invalidation at an earlier dependency level MAY cause a
later decision to become invalid or unavailable.

The applicable Policy SHALL determine the effect of such dependency
failure.

---

## 13.3 Decision Dependency Graph

An Authorization Decision MAY depend on multiple independent inputs.

Conceptually:

                    Identity
                       |
                       v
              Authentication Result
                       |
                       |
Entitlement ---------->|
Evaluation Result      |
                       |
Policy Evaluation ---->+----> Authorization Evaluation
Result                 |              |
                       |              v
Context -------------->|    Authorization Decision
                       |
Transaction State ---->|
                       |
Service Profile ------>|

The graph represents logical dependencies.

It does not require a single physical component to perform all
evaluations.

---

## 13.4 Decision Precedence

Multiple decision inputs MAY produce different outcomes.

For example:

- One Policy may permit an operation.
- Another Policy may deny the operation.
- An Entitlement may be valid.
- A required Context condition may be unsatisfied.
- A Security Policy may require denial.

The applicable Policy or Policy set SHALL define precedence rules for
resolving such outcomes.

---

## 13.5 Deny Precedence

A Policy MAY define Deny as having precedence over Permit.

Conceptually:

Permit
  |
  +------------------+
                     |
Deny ---------------+----> Final Decision = Deny

Where Deny Precedence applies, the presence of a required Deny
condition SHALL prevent a conflicting Permit from producing the final
Authorization Decision.

---

## 13.6 Permit Precedence

A Policy MAY define Permit as having precedence over another lower
priority Permit condition.

Permit precedence SHALL NOT override a mandatory security or denial
condition unless the applicable Policy explicitly permits such behavior.

---

## 13.7 Mandatory Conditions

A Policy MAY define one or more Mandatory Conditions.

A Mandatory Condition is a condition that SHALL be satisfied before an
Authorization Decision can produce a Permit outcome.

Conceptually:

Mandatory Condition A
        |
Mandatory Condition B
        |
Mandatory Condition C
        |
        v
Authorization Evaluation
        |
        v
Permit

Failure of a Mandatory Condition SHALL prevent Permit where the
applicable Policy requires all Mandatory Conditions to be satisfied.

---

## 13.8 Optional Conditions

A Policy MAY define Optional Conditions.

An Optional Condition MAY affect the decision without necessarily
preventing authorization.

The effect of an Optional Condition SHALL be defined by the applicable
Policy.

An Optional Condition SHALL NOT be interpreted as Mandatory merely
because it is present in the Decision Context.

---

## 13.9 Condition Combination

A Policy MAY define logical combinations of conditions.

Supported logical relationships MAY include:

- AND
- OR
- NOT
- Threshold
- Ordered sequence
- Conditional dependency
- Mutual exclusion

The logical combination SHALL be evaluated according to the Policy
applicable to the Transaction.

---

## 13.10 Conflict Resolution

A conflict exists where two or more applicable decision rules produce
incompatible outcomes.

A Policy MAY resolve a conflict using:

- Explicit precedence
- Deny-overrides
- Permit-overrides
- Rule priority
- Specificity
- Security level
- Latest valid rule
- Administrative precedence
- Fail-closed behavior

A conflict SHALL NOT be resolved implicitly where the applicable Policy
requires an explicit resolution rule.

---

## 13.11 Indeterminate Decision

An Authorization Evaluation MAY produce an Indeterminate result.

Indeterminate MAY occur when:

- Required information is unavailable
- A dependency cannot be validated
- A Policy cannot be evaluated
- Context integrity cannot be established
- A required Entitlement cannot be validated
- Security state is unknown
- A required external service is unavailable

Where fail-closed behavior applies, Indeterminate SHALL NOT produce
Service Execution.

---

## 13.12 Decision Consistency

The same Authorization Decision inputs and applicable Policy Version
SHOULD produce a consistent decision outcome unless an explicitly
time-varying or state-dependent condition has changed.

Decision consistency MAY be supported by recording:

- Policy Identifier
- Policy Version
- Decision Context
- Evaluation time
- Input states
- Decision outcome
- Decision dependencies

Such information MAY be used to reconstruct the basis of an
Authorization Decision.

---

## End of Part 4C
# 14. Service Execution and Enforcement Object Graph

## 14.1 Service

A Service represents a business or technical operation that may be
requested within a Transaction.

A Service MAY provide:

- A business operation
- A protected resource
- An API operation
- A transaction operation
- A physical or digital service
- Another implementation-defined operation

A Service SHALL define or reference the conditions under which an
Authorization Decision is required.

---

## 14.2 Service Profile

A Service Profile describes the authorization requirements applicable
to a Service or operation.

A Service Profile MAY specify:

- Required Authentication
- Required Entitlements
- Applicable Policies
- Required Context
- Authorization requirements
- Revalidation requirements
- Execution conditions
- Evidence requirements
- Audit requirements
- Failure handling requirements

The Service Profile MAY be referenced by a Transaction.

---

## 14.3 Requested Operation

A Requested Operation identifies the operation that the subject is
attempting to perform.

The Requested Operation MAY identify:

- Service
- Resource
- Operation type
- Parameters
- Transaction
- Requested scope

The Requested Operation SHALL be available to Authorization Evaluation
where the applicable Policy requires operation-specific authorization.

---

## 14.4 Enforcement

Enforcement is the process by which the Service applies an
Authorization Decision to an attempted Service Execution.

Conceptually:

Authorization Decision
        |
        v
    Enforcement
        |
        v
Service Execution

Enforcement SHALL verify that the Authorization Decision is applicable
to the requested operation according to the applicable rules.

---

## 14.5 Enforcement Conditions

Enforcement MAY verify:

- Decision status
- Decision scope
- Transaction Identifier
- Transaction State
- Service
- Requested Operation
- Resource
- Decision freshness
- Entitlement state
- Policy Version
- Required Context
- Revalidation state

If a required enforcement condition is not satisfied, Service
Execution SHALL NOT proceed where the applicable Policy requires
fail-closed behavior.

---

## 14.6 Authorization Decision Binding to Execution

An Authorization Decision MAY be explicitly bound to Service Execution.

Conceptually:

Authorization Decision
        |
        +-- Transaction
        +-- Service
        +-- Requested Operation
        +-- Resource
        +-- Decision Scope
        |
        v
   Enforcement
        |
        v
Service Execution

The binding SHALL permit the implementation to determine whether the
Decision authorizes the particular execution attempt.

A valid Authorization Decision for one operation SHALL NOT
automatically authorize a different operation.

---

## 14.7 Service Execution

Service Execution is the actual performance of the requested Service
operation after applicable authorization and enforcement requirements
have been satisfied.

Service Execution MAY:

- Modify a resource
- Transfer information
- Issue a result
- Consume an Entitlement
- Change Transaction state
- Generate Evidence
- Generate an Audit Record
- Trigger another Transaction

Service Execution SHALL remain distinguishable from Authorization.

---

## 14.8 Execution Result

An Execution Result represents the result of Service Execution.

The result MAY indicate:

- Success
- Failure
- Partial Success
- Cancelled
- Rejected by Enforcement
- Timeout
- Interrupted
- Pending
- Other defined execution state

An Execution Result SHALL be associated with the Transaction and the
corresponding Service Execution.

---

## 14.9 Authorization and Execution Independence

An Authorization Decision and an Execution Result represent different
logical events.

For example:

Authorization Decision = Permit
Execution Result = Failure

This combination SHALL be permitted.

Likewise:

Authorization Decision = Permit
Execution Result = Success

indicates that the Service Execution was permitted and successfully
completed.

A Permit Decision SHALL NOT guarantee business success.

---

## 14.10 Enforcement Failure

Enforcement MAY reject an execution attempt even when an earlier
Authorization Decision indicated Permit.

This MAY occur where:

- The Decision has expired
- A required Entitlement has been revoked
- The Transaction has been invalidated
- The Decision scope does not match the request
- Required Context has changed
- Revalidation has failed
- A security condition has changed

The resulting rejection SHALL be distinguishable from an Authorization
Decision that originally produced Deny.

---

## 14.11 Execution State

A Service Execution MAY progress through states such as:

Requested
  |
  v
Authorized
  |
  v
Enforced
  |
  v
Executing
  |
  v
Completed

Failure or interruption MAY cause transition to:

Failed
Cancelled
Interrupted
Rejected

The exact state model MAY be implementation-defined provided that the
required semantic distinctions are preserved.

---

## 14.12 Execution and Transaction State

Service Execution MAY update the Transaction State.

For example:

Authorized
  |
  v
Executing
  |
  v
Completed

The Transaction State SHALL remain distinguishable from the
Execution Result.

A Transaction MAY terminate with an Execution Result indicating
failure.

---

## 14.13 Execution Result and Entitlement Consumption

A Service Execution MAY consume an Entitlement.

The relationship MAY be:

Authorization Decision
        |
        v
Service Execution
        |
        v
Entitlement Consumption
        |
        v
Updated Entitlement State

Consumption SHALL occur only according to the applicable Service and
Policy rules.

An Authorization Decision SHALL NOT automatically imply that an
Entitlement has been consumed.

---

## End of Part 4D
# 15. Evidence and Audit Object Graph

## 15.1 Evidence

Evidence represents information that may be used to demonstrate or
reconstruct an event, state, decision, or transaction.

Evidence MAY be generated by:

- Registration
- Authentication
- Entitlement issuance
- Entitlement evaluation
- Policy Evaluation
- Authorization Evaluation
- Enforcement
- Service Execution
- Entitlement Consumption
- Revalidation
- Revocation
- Failure handling

Evidence SHALL remain distinguishable from the event or decision that
generated it.

---

## 15.2 Evidence Reference

A processing object MAY reference one or more Evidence objects.

For example:

Authentication Result
        |
        +-- Evidence Reference

Entitlement Evaluation Result
        |
        +-- Evidence Reference

Authorization Decision
        |
        +-- Evidence Reference

Execution Result
        |
        +-- Evidence Reference

An Evidence Reference SHALL permit the applicable Evidence to be
associated with the originating processing object.

---

## 15.3 Transaction Evidence

A Transaction MAY accumulate Evidence throughout its lifecycle.

Conceptually:

Transaction
   |
   +-- Registration Evidence
   |
   +-- Authentication Evidence
   |
   +-- Entitlement Evidence
   |
   +-- Policy Evaluation Evidence
   |
   +-- Authorization Evidence
   |
   +-- Enforcement Evidence
   |
   +-- Execution Evidence

The collection of Evidence MAY be used to reconstruct the processing
history of the Transaction.

---

## 15.4 Evidence and Decision Reconstruction

Evidence MAY contain or reference information sufficient to reconstruct
the basis of a decision.

Such information MAY include:

- Transaction Identifier
- Identity reference
- Credential reference
- Authentication Result
- Entitlement Identifier
- Entitlement Evaluation Result
- Policy Identifier
- Policy Version
- Decision Context
- Authorization Decision
- Decision time
- Enforcement result
- Execution Result

Evidence SHALL NOT be interpreted as changing the original outcome of a
decision.

---

## 15.5 Audit Record

An Audit Record represents a record of an event, operation, state
transition, decision, or other auditable activity.

An Audit Record MAY reference:

- Transaction
- Identity
- Credential
- Entitlement
- Policy
- Authorization Decision
- Service Execution
- Evidence

Audit Records MAY be generated independently of Evidence or MAY
reference Evidence.

---

## 15.6 Audit Trace

An Audit Trace represents a sequence of related Audit Records.

Conceptually:

Transaction
     |
     v
Audit Trace
     |
     +-- Registration
     +-- Authentication
     +-- Entitlement
     +-- Policy Evaluation
     +-- Authorization
     +-- Enforcement
     +-- Service Execution
     +-- Completion

The Audit Trace MAY preserve the chronological or logical ordering of
events.

---

## 15.7 Decision Traceability

An Authorization Decision SHOULD be traceable to the inputs used to
produce it.

The trace MAY include:

- Authentication Result
- Entitlement Evaluation Result
- Policy Evaluation Result
- Decision Context
- Policy Version
- Transaction State
- Service Profile
- Requested Operation
- Security Context

Decision traceability MAY be implemented through direct references,
identifiers, hashes, signed records, or other integrity-preserving
mechanisms.

---

## 15.8 Execution Traceability

A Service Execution MAY be traceable to the Authorization Decision that
permitted the execution.

Conceptually:

Authorization Decision
        |
        v
Enforcement
        |
        v
Service Execution
        |
        v
Execution Result

The relationship SHALL permit determination of whether the execution
was associated with an applicable Authorization Decision.

---

## 15.9 Evidence Integrity

Evidence MAY be protected against unauthorized modification.

Integrity protection MAY use:

- Cryptographic hash
- Digital signature
- Secure storage
- Append-only storage
- Versioning
- Trusted timestamp
- Other integrity-preserving mechanisms

The selected mechanism SHALL be appropriate to the applicable security
requirements.

---

## 15.10 Audit and Security

Audit information MAY itself be subject to access control and security
requirements.

Audit Records SHALL NOT be assumed to be publicly accessible merely
because they describe system events.

Access to Audit Records MAY require:

- Authentication
- Authorization
- Administrative privilege
- Service-specific permission
- Legal or operational controls

---

## 15.11 Audit and Privacy

Evidence and Audit Records MAY contain information associated with
Identity, Credential, Transaction, or Service usage.

Implementations SHOULD minimize unnecessary disclosure of such
information.

Where applicable, sensitive values MAY be represented by references,
pseudonymous identifiers, hashes, or other protected representations.

---

## End of Part 4E
# 16. Transaction-Centric Object Graph

## 16.1 Transaction as the Primary Correlation Object

A Transaction represents the logical unit within which one or more
protocol operations are performed.

A Transaction MAY contain or reference:

- Transaction Identifier
- Transaction Token
- Transaction Context
- Transaction State
- Identity
- Credential
- Authentication Result
- Entitlement
- Entitlement Evaluation Result
- Policy
- Policy Evaluation Result
- Authorization Decision
- Service Profile
- Requested Operation
- Enforcement Result
- Service Execution
- Execution Result
- Evidence
- Audit Records

The Transaction Identifier MAY be used to correlate these objects.

Correlation SHALL NOT by itself establish authorization.

---

## 16.2 Transaction Identifier

A Transaction Identifier uniquely identifies a Transaction within its
applicable scope.

The Transaction Identifier MAY be referenced by:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization
- Service Execution
- Evidence
- Audit

The identifier SHALL NOT itself constitute:

- Authentication
- Entitlement
- Authorization
- Permission
- Service Execution

---

## 16.3 Transaction Token

A Transaction MAY use a Transaction Token to identify or authenticate
participation in a particular protocol transaction.

A Transaction Token MAY be:

- Randomly generated
- Cryptographically protected
- Time-limited
- Single-use
- Bound to a Transaction
- Bound to a Service
- Bound to an Authentication operation

The specific Token semantics SHALL be defined by the applicable
protocol.

---

## 16.4 Transaction Context

Transaction Context represents information applicable to the current
Transaction.

The Context MAY contain or reference:

- Identity
- Authentication Result
- Credential state
- Entitlement state
- Policy state
- Authorization state
- Service Profile
- Requested Operation
- Resource
- Time
- Location
- Device state
- Security state
- External verification
- Previous Transaction state

Transaction Context MAY be assembled dynamically during the lifecycle
of the Transaction.

---

## 16.5 Context Assembly

Context Assembly is the process of collecting and combining information
required for subsequent evaluation.

Conceptually:

Identity
    |
Authentication Result
    |
Entitlement State
    |
Transaction State
    |
Service Profile
    |
Environmental Context
    |
Security Context
    |
    v
Context Assembly
    |
    v
Transaction Context

Context Assembly SHALL preserve the semantic identity of the source
objects.

Combining source information into a Context SHALL NOT cause the source
objects to become indistinguishable.

---

## 16.6 Context Provenance

A Transaction Context MAY maintain provenance information identifying
the source of individual context elements.

Provenance MAY identify:

- Source object
- Source service
- Source Transaction
- Issuer
- Timestamp
- Version
- Verification state
- Integrity state

Context provenance MAY be used to determine whether an input is
trustworthy for Policy Evaluation or Authorization.

---

## 16.7 Transaction State

Transaction State represents the current lifecycle state of a
Transaction.

A Transaction MAY progress through states such as:

Created
  |
  v
Initialized
  |
  v
Authenticated
  |
  v
Entitled
  |
  v
Evaluated
  |
  v
Authorized
  |
  v
Executing
  |
  v
Completed

Alternative states MAY include:

Pending
Failed
Cancelled
Expired
Rejected
Revoked
Invalidated

The exact state model MAY depend on the Transaction type.

---

## 16.8 State Transition Conditions

A Transaction State transition MAY depend on:

- Authentication Result
- Entitlement Evaluation Result
- Policy Evaluation Result
- Authorization Decision
- Enforcement Result
- Service Execution
- Execution Result
- Security events
- Time
- External state

A state transition SHALL occur only when the applicable conditions are
satisfied.

---

## 16.9 Transaction State and Authorization

Transaction State MAY be an input to Authorization Evaluation.

For example:

Transaction State
      |
      v
Authorization Evaluation
      |
      v
Authorization Decision

A Policy MAY prohibit authorization when the Transaction is in a state
such as:

- Failed
- Cancelled
- Expired
- Revoked
- Invalidated

The applicable Policy SHALL determine the effect of Transaction State
on Authorization.

---

## 16.10 Transaction State and Execution

Service Execution MAY cause a Transaction State transition.

For example:

Authorized
    |
    v
Executing
    |
    v
Completed

A failed Service Execution MAY instead result in:

Executing
    |
    v
Failed

Transaction State SHALL remain distinguishable from the Execution
Result.

---

## 16.11 Transaction Correlation

Objects participating in a Transaction MAY be correlated using:

- Transaction Identifier
- Object Identifier
- Parent Identifier
- Reference Identifier
- Cryptographic binding
- Other defined correlation mechanism

Correlation mechanisms MAY be combined.

Correlation SHALL NOT itself imply that all referenced objects are
valid or authoritative.

---

## 16.12 Transaction Object Graph

The principal Transaction-centric relationship is:

Transaction
   |
   +-- Identity
   |
   +-- Credential
   |
   +-- Authentication Result
   |
   +-- Entitlement
   |       |
   |       +-- Entitlement Evaluation Result
   |
   +-- Policy
   |       |
   |       +-- Policy Evaluation Result
   |
   +-- Service Profile
   |
   +-- Requested Operation
   |
   +-- Authorization Decision
   |
   +-- Enforcement
   |
   +-- Service Execution
   |       |
   |       +-- Execution Result
   |
   +-- Evidence
   |
   +-- Audit

This graph represents logical relationships and does not prescribe a
particular deployment topology.

---

## End of Part 5A
# 17. Global Transaction Lifecycle

## 17.1 Global Lifecycle

The Version 2.0 protocol suite MAY be understood as a global lifecycle
consisting of multiple logically distinct processing stages.

The principal lifecycle is:

Transaction Creation
        |
        v
Identity / Credential
        |
        v
Authentication
        |
        v
Authentication Result
        |
        v
Entitlement Acquisition / Evaluation
        |
        v
Policy Evaluation
        |
        v
Authorization Evaluation
        |
        v
Authorization Decision
        |
        v
Enforcement
        |
        v
Service Execution
        |
        v
Execution Result
        |
        v
Evidence / Audit

Each stage SHALL remain semantically distinguishable.

---

## 17.2 Lifecycle Independence

The stages of the global lifecycle MAY be implemented by separate
components.

For example:

Authentication Service
        |
        v
Entitlement Service
        |
        v
Policy Evaluation Service
        |
        v
Authorization Service
        |
        v
Protected Service

The logical protocol relationships SHALL remain valid regardless of
whether the stages are implemented within one component or multiple
distributed components.

---

## 17.3 Cross-Component Processing

A Transaction MAY move between processing components during its
lifecycle.

A component receiving a Transaction MAY receive:

- Transaction Identifier
- Transaction Context
- Authentication Result
- Entitlement reference
- Policy reference
- Authorization Decision
- Security Context
- Other defined protocol objects

The receiving component SHALL determine validity according to the
applicable protocol and Policy.

Receipt of an object SHALL NOT automatically establish its
trustworthiness.

---

## 17.4 Cross-Service Transaction

A Transaction MAY involve multiple Services.

Conceptually:

Service A
   |
   v
Transaction
   |
   +------> Service B
   |
   +------> Service C
   |
   +------> Service D

The Transaction Identifier MAY provide logical correlation across the
participating Services.

Each Service MAY perform its own:

- Authentication
- Entitlement Evaluation
- Policy Evaluation
- Authorization
- Enforcement

where required by the applicable Service Profile.

---

## 17.5 Cross-Service Authentication State

An Authentication Result MAY be referenced by a subsequent Service.

Conceptually:

Service A
   |
   v
Authentication
   |
   v
Authentication Result
   |
   +--------------------+
                        |
                        v
                    Service B

Reuse of an Authentication Result SHALL be subject to the applicable
trust, freshness, scope, and Policy requirements.

Successful Authentication in one Service SHALL NOT automatically imply
authorization in another Service.

---

## 17.6 Cross-Service Entitlement State

An Entitlement MAY be created or acquired through one Service and
evaluated by another Service.

Conceptually:

Service A
   |
   v
Entitlement
   |
   v
Service B
   |
   v
Entitlement Evaluation
   |
   v
Authorization

The receiving Service SHALL evaluate whether the Entitlement is
applicable to the receiving Transaction and Service.

---

## 17.7 Cross-Service Authorization

An Authorization Decision MAY be generated by a component separate
from the Service performing the requested operation.

Conceptually:

Transaction
     |
     v
Authorization Service
     |
     v
Authorization Decision
     |
     v
Protected Service
     |
     v
Enforcement
     |
     v
Service Execution

The Protected Service SHALL enforce the Decision according to the
applicable binding and scope rules.

---

## 17.8 Distributed Decision Processing

Authorization Evaluation MAY be distributed across multiple components.

For example:

Component A
    |
    +-- Authentication Result
    |
    v
Component B
    |
    +-- Entitlement Evaluation
    |
    v
Component C
    |
    +-- Policy Evaluation
    |
    v
Component D
    |
    +-- Authorization Decision
    |
    v
Component E
    |
    +-- Enforcement
    |
    v
Service

The distributed implementation SHALL preserve the logical dependency
relationships defined by the protocol.

---

## 17.9 Context Propagation

A Transaction Context MAY be propagated between processing components.

Context propagation MAY include:

- Transaction Identifier
- Authentication state
- Entitlement state
- Policy state
- Authorization state
- Security state
- Service information
- Requested operation
- Context provenance

Propagated Context SHALL remain subject to integrity, scope, freshness,
and trust requirements.

---

## 17.10 Context Reconstruction

A receiving component MAY reconstruct a Transaction Context from
multiple protocol objects.

For example:

Authentication Result
        +
Entitlement State
        +
Policy State
        +
Transaction State
        +
Service Profile
        +
Security Context
        |
        v
Context Reconstruction
        |
        v
Decision Context

Context Reconstruction SHALL preserve the semantic relationships
between the source objects.

---

## End of Part 5B
# 18. Distributed Trust and Security Context

## 18.1 Trust Relationship

A processing component SHALL determine whether information received from
another component is sufficiently trustworthy for the intended use.

Trust MAY be based on:

- Cryptographic verification
- Trusted communication channel
- Trusted issuer
- Known Service
- Known Policy
- Transaction binding
- Identity binding
- Credential binding
- Integrity verification
- Freshness verification
- Administrative trust configuration

Receipt of information SHALL NOT by itself establish trust.

---

## 18.2 Trust Boundary

A Trust Boundary identifies a boundary across which information must be
validated before being relied upon.

Conceptually:

Trusted Component A
        |
        | Protected Transfer
        v
+-------------------------+
|      Trust Boundary     |
+-------------------------+
        |
        v
Trusted Component B

The receiving component SHALL apply the applicable trust requirements
before using received information in a security-sensitive decision.

---

## 18.3 Security Context

Security Context represents security-relevant information applicable to
a Transaction or processing operation.

A Security Context MAY contain or reference:

- Authentication state
- Credential state
- Entitlement state
- Policy state
- Authorization state
- Trust state
- Integrity state
- Device state
- Risk state
- Security events
- Revalidation state

Security Context MAY be propagated between processing components.

---

## 18.4 Security Context Propagation

Security Context MAY be propagated as:

Security Context
       |
       v
Processing Component A
       |
       v
Protected Transfer
       |
       v
Processing Component B
       |
       v
Security Context Reconstruction

The receiving component SHALL validate the propagated Security Context
according to applicable integrity, trust, and freshness requirements.

---

## 18.5 Context Integrity

Context Integrity represents the degree to which a Context can be
relied upon as an accurate representation of the relevant source state.

Context Integrity MAY be established using:

- Digital signature
- Message authentication code
- Cryptographic hash
- Trusted transport
- Server-side state
- Signed object references
- Secure storage
- Other integrity mechanisms

An implementation MAY combine multiple mechanisms.

---

## 18.6 Context Freshness

A Context MAY have a defined freshness requirement.

Freshness MAY be established using:

- Timestamp
- Expiration
- Nonce
- Sequence number
- Version
- Transaction state
- Revalidation
- Server-side state

A stale Context SHALL NOT be used for a security-sensitive decision
where the applicable Policy requires current information.

---

## 18.7 Context Provenance

Context Provenance identifies the origin and processing history of
Context information.

Provenance MAY identify:

- Originating Service
- Originating Transaction
- Issuer
- Object Identifier
- Creation time
- Modification time
- Verification state
- Policy Version
- Processing component

Provenance MAY be used to evaluate trustworthiness.

---

## 18.8 Cross-Service Trust

A Service receiving information originating from another Service MAY
accept that information only when the applicable trust relationship is
satisfied.

For example:

Service A
    |
    +-- Authentication Result
    |
    v
Trust Validation
    |
    v
Service B

Service B SHALL NOT be required to accept Service A information without
performing the validation required by its applicable Policy.

---

## 18.9 Issuer Trust

An Entitlement, Credential-related object, Policy reference, or other
security-sensitive object MAY identify an Issuer.

The receiving component MAY validate:

- Issuer identity
- Issuer authorization
- Issuer trust status
- Object integrity
- Object validity
- Object scope

Issuer identification alone SHALL NOT establish object validity.

---

## 18.10 Decision Trust

An Authorization Decision received from another component MAY be
accepted only when the receiving Service can establish that the
Decision is:

- Authentic
- Integrity-protected
- Within scope
- Current
- Applicable to the Transaction
- Applicable to the Service
- Applicable to the Requested Operation

The receiving Service SHALL apply its local enforcement requirements.

---

## 18.11 Trust and Authorization

Trust validation MAY be an input to Authorization Evaluation.

Conceptually:

Received Decision Context
        |
        v
Trust Validation
        |
        v
Validated Decision Context
        |
        v
Authorization Evaluation
        |
        v
Authorization Decision

Failure of a required Trust Validation SHALL prevent authorization
where the applicable Policy requires fail-closed behavior.

---

## 18.12 Trust State

A Transaction MAY maintain a Trust State.

Trust State MAY include:

- Trusted
- Partially Trusted
- Untrusted
- Unknown
- Invalidated

Trust State MAY change during the Transaction lifecycle.

A change in Trust State MAY trigger:

- Revalidation
- Policy Evaluation
- Authorization Evaluation
- Decision invalidation
- Service Execution termination

---

## 18.13 Security Context Dependency

An Authorization Decision MAY depend on Security Context.

Conceptually:

Security Context
       |
       v
Policy Evaluation
       |
       v
Authorization Evaluation
       |
       v
Authorization Decision

A change in Security Context MAY invalidate a previously generated
Authorization Decision where the applicable Policy establishes such a
dependency.

---

## End of Part 5C
# 19. Object Graph Integrity and Invalidity Propagation

## 19.1 Object Validity

Each protocol object MAY have a validity state.

Validity MAY depend on:

- Integrity
- Authenticity
- Scope
- Freshness
- Issuer
- Transaction binding
- Identity binding
- Service binding
- Policy requirements
- Security state
- Lifecycle state

An object SHALL NOT be treated as valid solely because the object can
be identified or referenced.

---

## 19.2 Object Invalidity

An object MAY become invalid after it was initially considered valid.

Invalidation MAY result from:

- Expiration
- Revocation
- Integrity failure
- Scope violation
- Transaction cancellation
- Identity change
- Credential revocation
- Entitlement revocation
- Policy change
- Context change
- Security event
- Trust failure

Object invalidity MAY affect dependent objects.

---

## 19.3 Dependency-Based Invalidation

Where Object B depends on Object A, invalidation of Object A MAY cause
Object B to become invalid.

Conceptually:

Object A
   |
   | dependency
   v
Object B

If Object A becomes invalid, the applicable Policy SHALL determine
whether Object B:

- Remains valid
- Requires Revalidation
- Becomes Indeterminate
- Becomes Invalid
- Must be revoked

---

## 19.4 Authorization Dependency Invalidation

An Authorization Decision MAY depend on:

- Authentication Result
- Entitlement Evaluation Result
- Policy Evaluation Result
- Transaction State
- Security Context
- Service Profile
- Decision Context

Conceptually:

Authentication Result --------+
                              |
Entitlement Evaluation ------+
                              |
Policy Evaluation -----------+
                              |
Transaction State ------------+--> Authorization Decision
                              |
Security Context -------------+
                              |
Service Profile -------------+

Invalidation of a required dependency MAY invalidate the Authorization
Decision.

---

## 19.5 Invalidation Propagation

Invalidation MAY propagate through the Object Graph.

For example:

Entitlement
    |
    v
Entitlement Evaluation Result
    |
    v
Policy Evaluation Result
    |
    v
Authorization Decision
    |
    v
Enforcement
    |
    v
Service Execution

If the Entitlement becomes invalid, the applicable Policy MAY require
the invalidity to propagate through the dependent processing stages.

---

## 19.6 Fail-Closed Propagation

Where fail-closed behavior applies, an invalid or untrusted dependency
SHALL NOT produce an effective Permit Authorization Decision.

Conceptually:

Invalid Dependency
        |
        v
Dependent Evaluation
        |
        v
Fail-Closed
        |
        v
No Effective Permit
        |
        v
No Authorized Execution

Fail-Closed behavior MAY be applied at:

- Entitlement Evaluation
- Policy Evaluation
- Authorization Evaluation
- Enforcement
- Service Execution

---

## 19.7 Decision Invalidation

An Authorization Decision MAY be explicitly marked invalid.

Possible invalid states include:

- Revoked
- Expired
- Superseded
- Invalidated
- Revalidation Required
- Dependency Failed
- Context Changed
- Policy Changed
- Security Invalidated

An invalid Authorization Decision SHALL NOT be used to authorize
Service Execution where the applicable Policy prohibits such use.

---

## 19.8 Decision Revocation Propagation

Revocation of a dependent object MAY propagate to an Authorization
Decision.

For example:

Entitlement Revocation
        |
        v
Entitlement State
        |
        v
Authorization Dependency
        |
        v
Authorization Decision
        |
        v
Decision Revocation
        |
        v
Enforcement
        |
        v
Service Execution State

The propagation semantics SHALL be determined by the applicable Policy.

---

## 19.9 Execution Invalidation

A Service Execution MAY be invalidated or terminated when a required
Authorization Decision becomes invalid.

This MAY occur:

- Before execution
- During execution
- Between execution stages
- Before completion
- During a long-running Transaction

The applicable Service Profile and Policy SHALL determine the required
behavior.

---

## 19.10 Long-Running Transaction

A Transaction MAY remain active while Service Execution is in progress.

During a long-running Transaction:

- Authentication state MAY change
- Entitlement state MAY change
- Policy state MAY change
- Context MAY change
- Security state MAY change
- Authorization state MAY change

The system MAY therefore require periodic or event-triggered
Revalidation.

---

## 19.11 Event-Triggered Revalidation

A change in a dependency MAY trigger Revalidation.

For example:

Entitlement Revoked
       |
       v
Dependency Change Event
       |
       v
Authorization Revalidation
       |
       v
Current Authorization State

Other triggering events MAY include:

- Credential revocation
- Identity state change
- Policy change
- Security event
- Context change
- Service state change
- Transaction cancellation

---

## 19.12 Invalidity State Propagation

Invalidity propagation MAY update multiple related objects.

For example:

+----------------------+
| Entitlement          |
| State = Revoked      |
+----------+-----------+
           |
           v
+----------------------+
| Entitlement Eval.    |
| State = Invalid      |
+----------+-----------+
           |
           v
+----------------------+
| Authorization        |
| State = Invalid      |
+----------+-----------+
           |
           v
+----------------------+
| Enforcement          |
| State = Rejected     |
+----------+-----------+
           |
           v
+----------------------+
| Service Execution    |
| State = Terminated   |
+----------------------+

The exact state names MAY be implementation-defined.

---

## 19.13 No Implicit Validity Restoration

An object that has become invalid SHALL NOT automatically become valid
merely because another dependent object becomes valid.

Validity restoration SHALL require the applicable lifecycle transition,
Revalidation, or other defined mechanism.

---

## 19.14 Graph Consistency

The Object Graph SHOULD remain internally consistent.

For example:

- A revoked Entitlement SHOULD NOT remain represented as an active
  dependency of an effective Permit Decision.
- An expired Policy Version SHOULD NOT silently replace the Policy
  Version used for a recorded Decision.
- An invalid Transaction SHOULD NOT remain represented as an active
  execution context where the applicable Policy prohibits execution.

Graph consistency MAY be maintained through explicit state transitions,
references, event processing, or other defined mechanisms.

---

## End of Part 5D
# 20. Global Canonical Object Graph

## 20.1 Purpose

This section consolidates the principal object relationships defined
throughout this document into a single Version 2.0 Canonical Object
Graph.

The Global Canonical Object Graph represents logical dependencies and
relationships among the principal protocol objects.

It does not prescribe:

- A specific server architecture
- A specific deployment topology
- A specific programming language
- A specific database
- A specific communication protocol
- A specific cloud provider

The graph defines semantic relationships rather than implementation
details.

---

## 20.2 Principal Processing Chain

The principal Version 2.0 processing chain is:

Identity
    |
    v
Credential
    |
    v
Authentication
    |
    v
Authentication Result
    |
    v
Entitlement
    |
    v
Entitlement Evaluation
    |
    v
Policy
    |
    v
Policy Evaluation
    |
    v
Authorization Evaluation
    |
    v
Authorization Decision
    |
    v
Enforcement
    |
    v
Service Execution
    |
    v
Execution Result

Evidence and Audit MAY be generated throughout the chain.

---

## 20.3 Transaction-Centered Graph

The principal objects are correlated by the Transaction:

                         Transaction
                              |
          +-------------------+-------------------+
          |                   |                   |
          v                   v                   v
      Identity            Context            Service Profile
          |                   |                   |
          v                   |                   v
      Credential              |          Requested Operation
          |                   |                   |
          v                   |                   |
   Authentication             |                   |
          |                   |                   |
          v                   +---------+---------+
 Authentication Result                  |
          |                             |
          v                             v
    Entitlement ----------------> Policy Evaluation
          |                             |
          v                             v
 Entitlement Evaluation        Policy Evaluation Result
          |                             |
          +-------------+---------------+
                        |
                        v
              Authorization Evaluation
                        |
                        v
             Authorization Decision
                        |
                        v
                   Enforcement
                        |
                        v
                Service Execution
                        |
                        v
                 Execution Result

The Transaction provides logical correlation but does not itself grant
permission.

---

## 20.4 Security and Trust Overlay

The processing chain MAY be evaluated together with Security Context
and Trust information.

Conceptually:

                    Security Context
                          |
                          v
Identity ---> Authentication ---> Authentication Result
                          |               |
                          |               v
                          |         Entitlement
                          |               |
                          |               v
                          |       Entitlement Evaluation
                          |               |
                          +---------------+
                                  |
                                  v
                           Policy Evaluation
                                  |
                                  v
                         Authorization Evaluation
                                  |
                                  v
                        Authorization Decision
                                  |
                                  v
                             Enforcement
                                  |
                                  v
                          Service Execution

Security Context MAY affect any stage where the applicable Policy
requires security-sensitive evaluation.

---

## 20.5 Context Assembly Overlay

Transaction Context MAY be assembled from multiple sources.

Authentication Result
        |
Entitlement State
        |
Transaction State
        |
Service Profile
        |
Requested Operation
        |
Security Context
        |
Environmental Context
        |
Policy State
        |
        v
+----------------------+
|   Context Assembly   |
+----------+-----------+
           |
           v
+----------------------+
| Transaction Context  |
+----------+-----------+
           |
           v
+----------------------+
| Policy Evaluation    |
+----------+-----------+
           |
           v
+----------------------+
| Authorization        |
| Evaluation           |
+----------+-----------+
           |
           v
+----------------------+
| Authorization        |
| Decision             |
+----------------------+

The assembled Context SHALL retain sufficient provenance to identify
the origin of security-sensitive inputs where required by the
applicable Policy.

---

## 20.6 Entitlement Overlay

Entitlement is an independent object class that may participate in
authorization without becoming an authentication credential.

The relationship is:

Identity
   |
   v
Authentication
   |
   v
Authentication Result
   |
   +----------------------+
                          |
                          v
                    Entitlement
                          |
                          v
               Entitlement Evaluation
                          |
                          v
                    Policy Evaluation
                          |
                          v
              Authorization Evaluation
                          |
                          v
               Authorization Decision

Successful Authentication SHALL NOT automatically create an
Entitlement.

Possession of an Entitlement SHALL NOT automatically establish
Authentication.

---

## 20.7 Policy Overlay

Policy defines the rules applicable to evaluation.

Conceptually:

Policy
   |
   +------------------------+
   |                        |
   v                        v
Policy Evaluation      Authorization Evaluation
   |                        |
   v                        |
Policy Evaluation Result    |
   |                        |
   +-----------+------------+
               |
               v
      Authorization Decision

Policy Evaluation and Authorization Evaluation MAY be implemented as
separate processing stages.

A Policy SHALL NOT itself be treated as an Authorization Decision.

---

## 20.8 Authorization Overlay

Authorization Evaluation combines applicable inputs to determine
whether a requested operation may proceed.

Inputs MAY include:

- Authentication Result
- Entitlement Evaluation Result
- Policy Evaluation Result
- Transaction State
- Transaction Context
- Security Context
- Service Profile
- Requested Operation
- Resource
- Trust State
- Other Policy-defined inputs

The conceptual relationship is:

                 +----------------------+
                 | Authentication       |
                 | Result               |
                 +----------+-----------+
                            |
                 +----------v-----------+
                 | Entitlement          |
                 | Evaluation Result    |
                 +----------+-----------+
                            |
                 +----------v-----------+
                 | Policy Evaluation    |
                 | Result               |
                 +----------+-----------+
                            |
                 +----------v-----------+
                 | Transaction Context  |
                 +----------+-----------+
                            |
                 +----------v-----------+
                 | Security Context     |
                 +----------+-----------+
                            |
                 +----------v-----------+
                 | Service Profile      |
                 +----------+-----------+
                            |
                 +----------v-----------+
                 | Requested Operation  |
                 +----------+-----------+
                            |
                            v
                 +----------------------+
                 | Authorization        |
                 | Evaluation            |
                 +----------+-----------+
                            |
                            v
                 +----------------------+
                 | Authorization        |
                 | Decision              |
                 +----------------------+

The Authorization Decision SHALL be based on the applicable Policy and
required decision inputs.

---

## 20.9 Enforcement Overlay

Authorization Decision is applied to the requested operation through
Enforcement.

Conceptually:

Authorization Decision
        |
        v
+------------------+
|    Enforcement   |
+--------+---------+
         |
         v
+------------------+
| Service Execution|
+--------+---------+
         |
         v
+------------------+
| Execution Result |
+------------------+

Enforcement SHALL determine whether the Decision is applicable to the
particular execution attempt.

A Permit Decision SHALL NOT be interpreted as proof that Service
Execution successfully completed.

---

## 20.10 Evidence and Audit Overlay

Evidence and Audit MAY observe or record the lifecycle without becoming
part of the authorization logic itself.

Conceptually:

Authentication --------+
                       |
Entitlement ------------+
                       |
Policy Evaluation ------+
                       |
Authorization ---------+
                       |
Enforcement ------------+
                       |
Service Execution ------+
                       |
Execution Result -------+
                       |
                       v
                +-------------+
                |   Evidence  |
                +------+------+
                       |
                       v
                +-------------+
                |    Audit    |
                +-------------+

Evidence MAY support reconstruction of processing history.

Audit MAY preserve an auditable representation of events and state
transitions.

Evidence and Audit SHALL remain distinguishable from the actual
authorization and execution decisions.

---

## End of Part 5E-1
## 20.11 Object Dependency Graph

The principal dependency relationships among Version 2.0 objects may be
represented as follows:

Identity
   |
   +--> Credential
   |
   +--> Authentication Result
             |
             +--> Transaction Context
             |
             +--> Authorization Evaluation

Entitlement
   |
   +--> Entitlement Evaluation Result
             |
             +--> Transaction Context
             |
             +--> Authorization Evaluation

Policy
   |
   +--> Policy Evaluation Result
   |
   +--> Authorization Evaluation
             |
             v
      Authorization Decision

Transaction
   |
   +--> Transaction Context
   |
   +--> Transaction State
   |
   +--> Requested Operation
   |
   +--> Service Profile

Authorization Decision
   |
   +--> Enforcement
             |
             v
      Service Execution
             |
             v
      Execution Result

The dependency graph represents logical dependency and does not imply
that the dependent object is physically stored together with its source
object.

---

## 20.12 Dependency Direction

Dependencies SHOULD be understood as directional relationships.

For example:

Authentication Result
        |
        v
Authorization Evaluation

The reverse relationship does not automatically apply.

An Authorization Decision SHALL NOT be treated as proof that the
Authentication Result remains valid unless the applicable Policy
explicitly defines such a relationship.

Similarly:

Entitlement
        |
        v
Entitlement Evaluation
        |
        v
Authorization Evaluation

An Authorization Decision SHALL NOT modify the underlying Entitlement
unless an explicit lifecycle operation is performed.

---

## 20.13 Lifecycle Dependency Graph

The lifecycle dependency may be represented as:

+----------------------+
| Transaction Created  |
+----------+-----------+
           |
           v
+----------------------+
| Authentication       |
+----------+-----------+
           |
           v
+----------------------+
| Authentication       |
| Result               |
+----------+-----------+
           |
           v
+----------------------+
| Entitlement          |
| Evaluation            |
+----------+-----------+
           |
           v
+----------------------+
| Policy Evaluation    |
+----------+-----------+
           |
           v
+----------------------+
| Authorization         |
| Evaluation            |
+----------+-----------+
           |
           v
+----------------------+
| Authorization         |
| Decision              |
+----------+-----------+
           |
           v
+----------------------+
| Enforcement           |
+----------+-----------+
           |
           v
+----------------------+
| Service Execution     |
+----------+-----------+
           |
           v
+----------------------+
| Execution Result      |
+----------------------+

Each stage MAY be skipped, repeated, or re-evaluated where permitted by
the applicable protocol and Policy.

---

## 20.14 Revalidation Dependency

A previously generated result MAY require revalidation.

For example:

Authentication Result
        |
        v
Entitlement Evaluation
        |
        v
Authorization Decision
        |
        v
Context Change
        |
        v
Revalidation
        |
        v
Current Authorization State

Revalidation MAY verify:

- Authentication state
- Credential state
- Entitlement state
- Policy state
- Transaction state
- Security Context
- Service applicability
- Decision freshness

---

## 20.15 Revocation Dependency

Revocation MAY propagate through dependent objects.

For example:

Credential Revocation
        |
        v
Authentication State
        |
        v
Entitlement Applicability
        |
        v
Authorization Dependency
        |
        v
Authorization Decision
        |
        v
Enforcement
        |
        v
Service Execution

The exact propagation behavior SHALL be determined by the applicable
Policy.

Where fail-closed behavior applies, a required revoked dependency
SHALL NOT permit continued authorized execution.

---

## 20.16 Policy Change Dependency

A Policy MAY change while a Transaction remains active.

Conceptually:

Policy Version N
       |
       v
Policy Evaluation
       |
       v
Authorization Decision
       |
       +------ Transaction remains active
                         |
                         v
                   Policy Version N+1
                         |
                         v
                     Revalidation

A Policy change MAY require re-evaluation of an existing Authorization
Decision.

The applicable Policy SHALL determine whether previously generated
Decisions remain effective.

---

## 20.17 Context Change Dependency

A Transaction Context MAY change after an Authorization Decision has
been generated.

For example:

Initial Context
      |
      v
Authorization Decision
      |
      v
Context Change
      |
      v
Revalidation
      |
      v
Current Decision

Context changes MAY include:

- Location change
- Device state change
- Security state change
- Entitlement state change
- Transaction state change
- External state change
- Risk state change

Where a Policy defines the changed Context as a decision dependency,
the Authorization Decision MAY become invalid.

---

## 20.18 Cross-Service Object Relationship

An object generated by one Service MAY be referenced by another
Service.

Conceptually:

+------------------+
| Service A        |
|                  |
| Authentication   |
| Entitlement      |
+--------+---------+
         |
         | Protected Object
         v
+------------------+
| Service B        |
|                  |
| Policy Evaluation|
| Authorization    |
+--------+---------+
         |
         v
+------------------+
| Service C        |
|                  |
| Enforcement      |
| Execution        |
+------------------+

The receiving Service SHALL evaluate the object according to its local
trust and applicability requirements.

---

## 20.19 Cross-Service Transaction Identity

A Transaction MAY retain a logical identity while crossing Service
boundaries.

For example:

Transaction T1
     |
     +----> Service A
     |
     +----> Service B
     |
     +----> Service C

Each Service MAY maintain a local processing identifier while retaining
a reference to the logical Transaction Identifier.

Local identifiers SHALL NOT be assumed to be globally identical.

---

## 20.20 Cross-Service Decision Binding

An Authorization Decision MAY be bound to:

- Transaction
- Service
- Requested Operation
- Resource
- Entitlement
- Policy Version
- Security Context
- Other applicable scope

The receiving Service SHALL verify the required bindings before
enforcement.

A Decision valid for one Service or operation SHALL NOT automatically
become valid for another Service or operation.

---

## 20.21 Failure Propagation

A failure in one stage MAY affect subsequent stages.

For example:

Authentication Failure
        |
        v
No Valid Authentication Result
        |
        v
Entitlement Evaluation
        |
        v
Authorization
        |
        v
Fail-Closed
        |
        v
No Authorized Execution

Similarly:

Policy Evaluation Failure
        |
        v
Indeterminate Policy Result
        |
        v
Authorization Evaluation
        |
        v
Fail-Closed where required
        |
        v
No Authorized Execution

Failure propagation SHALL follow the applicable Policy and protocol
requirements.

---

## 20.22 Partial Failure

A distributed Transaction MAY encounter partial failure.

For example:

Service A       Service B       Service C
   |               |               |
   |   success     |   success     |
   +-------------->+-------------->+
                                   |
                                   X failure

A partial failure SHALL NOT automatically be interpreted as successful
completion of the overall Transaction.

The Transaction State and applicable Policy SHALL determine the
resulting state.

---

## 20.23 Indeterminate State

An evaluation MAY produce an indeterminate result when required
information cannot be reliably established.

Indeterminate conditions MAY include:

- Missing Context
- Unavailable Entitlement state
- Unavailable Policy
- Failed integrity verification
- Unknown Trust State
- Expired information
- Conflicting state
- Dependency failure

Where fail-closed behavior applies, an Indeterminate result SHALL NOT
produce an effective Permit Decision.

---

## End of Part 5E-2
## 20.24 Global Integrated Object Graph

The Version 2.0 protocol suite may be represented by the following
integrated logical object graph.

                              +----------------+
                              |    Identity    |
                              +-------+--------+
                                      |
                                      v
                              +----------------+
                              |   Credential  |
                              +-------+--------+
                                      |
                                      v
                              +----------------+
                              | Authentication |
                              +-------+--------+
                                      |
                                      v
                              +----------------+
                              | Authentication |
                              |     Result     |
                              +-------+--------+
                                      |
                     +----------------+----------------+
                     |                                 |
                     v                                 v
              +-------------+                  +---------------+
              | Entitlement |                  | Transaction   |
              +------+------+                  |    Context    |
                     |                         +-------+-------+
                     v                                 |
              +-------------+                           |
              | Entitlement|                           |
              | Evaluation  |                           |
              +------+------+                           |
                     |                                 |
                     +----------------+----------------+
                                      |
                                      v
                              +----------------+
                              |     Policy     |
                              +-------+--------+
                                      |
                                      v
                              +----------------+
                              | Policy         |
                              | Evaluation     |
                              +-------+--------+
                                      |
                                      v
                              +----------------+
                              | Authorization  |
                              |  Evaluation    |
                              +-------+--------+
                                      |
                                      v
                              +----------------+
                              | Authorization  |
                              |    Decision    |
                              +-------+--------+
                                      |
                                      v
                              +----------------+
                              |  Enforcement   |
                              +-------+--------+
                                      |
                                      v
                              +----------------+
                              | Service        |
                              | Execution      |
                              +-------+--------+
                                      |
                                      v
                              +----------------+
                              | Execution      |
                              | Result        |
                              +----------------+

Security Context, Trust State, Service Profile, Requested Operation,
Environmental Context, and other Policy-defined inputs MAY provide
additional inputs to the applicable evaluation stages.

Evidence and Audit MAY observe and record the lifecycle.

---

## 20.25 Security Context Integration

The integrated graph MAY be viewed with Security Context as a
cross-cutting dependency.

                         Security Context
                                |
             +------------------+------------------+
             |                  |                  |
             v                  v                  v
       Authentication     Entitlement        Policy Evaluation
             |                  |                  |
             +------------------+------------------+
                                |
                                v
                       Authorization Evaluation
                                |
                                v
                       Authorization Decision
                                |
                                v
                           Enforcement

Security Context SHALL NOT be interpreted as a replacement for
Authentication, Entitlement, Policy Evaluation, or Authorization.

Instead, Security Context provides security-relevant information that
MAY affect the applicable evaluation.

---

## 20.26 Service Profile Integration

A Service Profile defines the requirements applicable to a Service.

A Service Profile MAY specify:

- Required Authentication
- Required Entitlements
- Required Policy
- Required Authorization
- Required Security Context
- Required Decision Scope
- Required Decision Freshness
- Required Revalidation
- Required Enforcement
- Required Evidence
- Required Audit

Conceptually:

                    Service Profile
                          |
        +-----------------+-----------------+
        |                 |                 |
        v                 v                 v
 Authentication      Entitlement       Policy
 Requirement         Requirement       Requirement
        |                 |                 |
        +-----------------+-----------------+
                          |
                          v
                Authorization Requirement
                          |
                          v
                    Enforcement
                          |
                          v
                     Service

A Service Profile SHALL NOT itself constitute an Authorization
Decision.

---

## 20.27 Requested Operation Binding

Authorization Evaluation MAY be bound to a Requested Operation.

The Requested Operation MAY identify:

- Operation type
- Target Service
- Target Resource
- Transaction
- Requested action
- Required Entitlement
- Required Policy
- Required Authorization scope

Conceptually:

Transaction
      |
      v
Requested Operation
      |
      +----------------------+
      |                      |
      v                      v
Service Profile          Policy
      |                      |
      +----------+-----------+
                 |
                 v
      Authorization Evaluation
                 |
                 v
      Authorization Decision

A Decision SHALL NOT automatically authorize a different Requested
Operation.

---

## 20.28 Resource Binding

An Authorization Decision MAY be bound to one or more Resources.

Conceptually:

Authorization Decision
          |
          +--------> Service
          |
          +--------> Requested Operation
          |
          +--------> Resource
          |
          +--------> Transaction
          |
          +--------> Entitlement
          |
          +--------> Policy

The applicable Policy SHALL determine which bindings are mandatory.

A Decision lacking a required binding SHALL NOT be treated as
applicable where the applicable Policy requires that binding.

---

## 20.29 Temporal Binding

A protocol object MAY be bound to temporal conditions.

Temporal conditions MAY include:

- Not-before time
- Expiration time
- Validity interval
- Transaction lifetime
- Decision lifetime
- Revalidation interval
- Event-triggered validity
- Service-defined execution interval

Conceptually:

Object Creation
      |
      v
Object Validity
      |
      +----> Revalidation
      |
      +----> Expiration
      |
      +----> Revocation
      |
      v
Object Invalidity

Temporal validity SHALL be evaluated according to the applicable
lifecycle and Policy.

---

## 20.30 Scope Binding

An object MAY be restricted by Scope.

Scope MAY include:

- Identity
- Account
- Device
- Transaction
- Service
- Resource
- Operation
- Entitlement
- Policy
- Time
- Security Context

Scope restrictions SHALL be evaluated before the object is relied upon
for a security-sensitive operation.

An object valid within one Scope SHALL NOT automatically be valid
outside that Scope.

---

## 20.31 Canonical Object Relationships

The principal Version 2.0 relationships are:

Identity
    |
    +--> Credential
    |
    +--> Authentication

Transaction
    |
    +--> Transaction Object
    +--> Transaction Context
    +--> Transaction State
    +--> Requested Operation

Authentication
    |
    +--> Authentication Result

Entitlement
    |
    +--> Entitlement Evaluation
    +--> Entitlement Lifecycle
    +--> Entitlement Binding

Policy
    |
    +--> Policy Evaluation
    +--> Policy Evaluation Result

Authentication Result
    |
    +--> Authorization Evaluation

Entitlement Evaluation Result
    |
    +--> Authorization Evaluation

Policy Evaluation Result
    |
    +--> Authorization Evaluation

Transaction Context
    |
    +--> Authorization Evaluation

Security Context
    |
    +--> Authorization Evaluation

Authorization Evaluation
    |
    +--> Authorization Decision

Authorization Decision
    |
    +--> Enforcement

Enforcement
    |
    +--> Service Execution

Service Execution
    |
    +--> Execution Result

Any of the above objects MAY additionally generate Evidence or Audit
information where required.

---

## 20.32 Logical Versus Physical Relationships

The relationships defined in the Canonical Object Graph are logical.

A logical object MAY be implemented as:

- A database record
- A signed message
- A token
- A server-side object
- A cryptographic object
- An API response
- A transient in-memory structure
- A persistent state object
- A reference to another object

The physical representation SHALL NOT alter the semantic relationship
defined by this document.

---

## 20.33 Object Reference

A protocol object MAY refer to another object by:

- Identifier
- Reference
- Cryptographic reference
- Transaction Identifier
- Embedded object
- Signed object
- Server-side lookup key
- Other defined representation

A reference SHALL NOT by itself establish the validity of the referenced
object.

The receiving component SHALL apply the applicable validation
requirements.

---

## 20.34 Object Identity and Identifier Separation

An Object Identifier SHALL remain distinguishable from the semantic
object itself.

For example:

Transaction Identifier
        |
        v
Transaction

The identifier MAY be used to locate, correlate, or reference a
Transaction.

The identifier SHALL NOT by itself establish:

- Authentication
- Entitlement
- Authorization
- Trust
- Service Execution permission

The same principle applies to other protocol object identifiers.

---

## 20.35 Object State and Object Existence

Existence of an object SHALL NOT automatically imply that the object is
currently valid.

For example:

Entitlement Exists
        |
        +----> Active
        |
        +----> Expired
        |
        +----> Revoked
        |
        +----> Suspended
        |
        +----> Invalid

Similarly:

Authorization Decision Exists
        |
        +----> Effective
        |
        +----> Expired
        |
        +----> Revoked
        |
        +----> Invalidated
        |
        +----> Revalidation Required

Object state SHALL therefore remain distinct from object existence.

---

## End of Part 5E-3A
## 20.36 Canonical Invariants

The following invariants define fundamental semantic constraints of the
Version 2.0 object model.

Subsequent protocol definitions, implementation requirements, figures,
and patent analysis SHOULD remain consistent with these invariants.

---

## 20.37 Authentication Invariant

Authentication establishes whether the applicable Identity or
Credential satisfies the Authentication requirements.

Authentication SHALL remain semantically distinct from Authorization.

Therefore:

Authentication
        |
        v
Authentication Result

Authentication Result
        |
        v
Authorization Evaluation

Authentication SHALL NOT itself constitute an Authorization Decision.

Successful Authentication SHALL NOT automatically authorize a
Requested Operation.

---

## 20.38 Entitlement Invariant

An Entitlement represents a right, qualification, condition, or other
authorization-related state that MAY be considered during
Authorization Evaluation.

An Entitlement SHALL remain distinct from:

- Identity
- Credential
- Authentication
- Policy
- Authorization Decision
- Service Execution

Therefore:

Authentication Result
        |
        v
Entitlement
        |
        v
Entitlement Evaluation
        |
        v
Authorization Evaluation

Authentication SHALL NOT automatically imply possession of every
Entitlement.

Possession of an Entitlement SHALL NOT automatically establish
Authentication.

---

## 20.39 Policy Invariant

A Policy defines rules or conditions used during evaluation.

A Policy SHALL remain distinct from its evaluation result.

Therefore:

Policy
   |
   v
Policy Evaluation
   |
   v
Policy Evaluation Result

A Policy SHALL NOT itself be treated as an Authorization Decision.

A Policy Evaluation Result SHALL remain distinguishable from the Policy
that produced it.

---

## 20.40 Authorization Evaluation Invariant

Authorization Evaluation determines whether the required conditions
for a Requested Operation are satisfied.

Authorization Evaluation MAY use multiple inputs.

These MAY include:

- Authentication Result
- Entitlement Evaluation Result
- Policy Evaluation Result
- Transaction Context
- Security Context
- Service Profile
- Requested Operation
- Resource
- Trust State
- Other Policy-defined inputs

Therefore:

Multiple Decision Inputs
          |
          v
Authorization Evaluation
          |
          v
Authorization Decision

Authorization Evaluation SHALL remain distinguishable from the
Authorization Decision produced by that evaluation.

---

## 20.41 Authorization Decision Invariant

An Authorization Decision represents the result of Authorization
Evaluation.

The Decision MAY indicate:

- Permit
- Deny
- Indeterminate
- Revalidation Required
- Other Policy-defined states

An Authorization Decision SHALL remain distinct from Service
Execution.

Therefore:

Authorization Decision
        |
        v
Enforcement
        |
        v
Service Execution

A Permit Decision SHALL NOT itself constitute successful execution.

---

## 20.42 Enforcement Invariant

Enforcement applies an Authorization Decision to an actual execution
attempt.

Enforcement SHALL determine whether the Decision is applicable to the
specific execution context.

The Enforcement stage MAY verify:

- Decision validity
- Decision scope
- Transaction binding
- Service binding
- Resource binding
- Operation binding
- Temporal validity
- Security Context
- Current Policy requirements

A Decision that is not applicable SHALL NOT authorize the execution.

---

## 20.43 Service Execution Invariant

Service Execution performs the requested business or technical
operation after the applicable authorization requirements have been
satisfied.

Service Execution SHALL remain distinct from:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Decision
- Execution Result

Successful authorization SHALL NOT guarantee successful Service
Execution.

Conversely, successful Service Execution SHALL NOT retroactively prove
that the preceding authorization process was valid.

---

## 20.44 Execution Result Invariant

An Execution Result represents the outcome of Service Execution.

The result MAY indicate:

- Success
- Failure
- Partial Success
- Cancellation
- Timeout
- Other defined execution state

Execution Result SHALL remain distinct from Authorization Decision.

Therefore:

Authorization Decision
        |
        v
Service Execution
        |
        v
Execution Result

A Permit Decision SHALL NOT be interpreted as an Execution Result.

---

## 20.45 Transaction Correlation Invariant

The Transaction provides logical correlation among related protocol
objects.

The Transaction MAY correlate:

- Identity
- Credential
- Authentication Result
- Entitlement
- Policy
- Context
- Authorization Decision
- Service Execution
- Evidence
- Audit

The Transaction Identifier MAY be used to correlate these objects.

Transaction correlation SHALL NOT itself establish:

- Authentication
- Entitlement
- Trust
- Authorization
- Execution permission

---

## 20.46 Context Invariant

Transaction Context and Security Context provide decision-relevant
information.

Context SHALL remain distinguishable from the decision produced using
that Context.

Therefore:

Context
   |
   v
Policy Evaluation
   |
   v
Authorization Evaluation
   |
   v
Authorization Decision

Context SHALL NOT itself constitute an Authorization Decision.

---

## 20.47 Scope Invariant

Security-sensitive objects MAY be constrained by Scope.

Scope MAY include:

- Identity
- Account
- Device
- Transaction
- Service
- Resource
- Operation
- Entitlement
- Policy
- Time
- Security Context

An object valid within one Scope SHALL NOT automatically be valid
outside that Scope.

---

## 20.48 Freshness Invariant

Security-sensitive information MAY be subject to freshness
requirements.

Where freshness is required:

Expired or stale information SHALL NOT be treated as current solely
because the information was previously valid.

Freshness MAY be established through:

- Timestamp
- Expiration
- Nonce
- Sequence
- Version
- Revalidation
- Server-side state
- Other defined mechanism

---

## 20.49 Revocation Invariant

A revoked object SHALL NOT remain effective where the applicable Policy
requires revocation to terminate its use.

Revocation MAY propagate to dependent objects.

For example:

Revoked Entitlement
        |
        v
Authorization Dependency
        |
        v
Authorization Decision
        |
        v
Enforcement
        |
        v
Service Execution

The propagation semantics SHALL be defined by the applicable Policy.

---

## 20.50 Fail-Closed Invariant

Where fail-closed behavior is required, uncertainty or failure of a
required security dependency SHALL NOT produce an effective Permit
Decision.

Examples include:

- Authentication uncertainty
- Entitlement uncertainty
- Policy evaluation failure
- Trust failure
- Context integrity failure
- Decision integrity failure
- Required state unavailable

The exact fail-closed behavior MAY vary according to the Service
Profile and Policy.

---

## 20.51 No Implicit Authorization Invariant

No individual object SHALL automatically establish authorization merely
because it exists or is successfully processed.

In particular:

Authentication
        != Authorization

Entitlement
        != Authorization

Policy
        != Authorization

Policy Evaluation Result
        != Authorization

Transaction
        != Authorization

Context
        != Authorization

Credential
        != Authorization

Authorization SHALL be established only through the applicable
Authorization Evaluation and Decision process.

---

## 20.52 No Implicit Execution Invariant

Authorization SHALL NOT automatically imply execution.

Therefore:

Authorization Decision
        != Service Execution

The Service SHALL perform the applicable Enforcement before executing
the requested operation where required by the applicable Policy.

---

## 20.53 No Implicit Consumption Invariant

Authorization SHALL NOT automatically imply Entitlement Consumption.

Therefore:

Entitlement
        |
        v
Authorization Decision

does not by itself establish:

Entitlement Consumption

Consumption SHALL occur only when the applicable Entitlement lifecycle
and Policy explicitly require it.

---

## 20.54 Separation of Concerns Invariant

The following stages SHALL remain conceptually distinguishable:

1. Authentication
2. Entitlement
3. Policy Evaluation
4. Authorization Evaluation
5. Authorization Decision
6. Enforcement
7. Service Execution

An implementation MAY combine multiple stages within one component,
provided that their semantic distinctions are preserved.

---

## 20.55 Distributed Processing Invariant

The Version 2.0 object relationships SHALL remain valid whether the
protocol is implemented:

- Centrally
- Federated
- Distributed
- Cross-Service
- Multi-tenant
- Multi-component
- Hybrid

Physical distribution SHALL NOT eliminate the logical dependencies
defined by the Canonical Object Graph.

---

## 20.56 Evidence Separation Invariant

Evidence and Audit information MAY record protocol processing.

Evidence and Audit SHALL remain distinguishable from:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution

Recording an event SHALL NOT itself change the authorization state
unless an applicable protocol operation explicitly causes such a state
transition.

---

## 20.57 Canonical Dependency Invariant

Where an object depends on another object, the dependent object SHALL
remain subject to the validity and applicability requirements of its
dependencies.

Conceptually:

Dependency A
     |
     v
Dependent Object B

The validity of B SHALL be evaluated consistently with the state of A
where A is a required dependency.

---

## End of Part 5E-3B
