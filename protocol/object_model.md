# NEW-shot2play Technical Specification Version 2.0
# Protocol Object Model

## 1. Document Status

| Item | Value |
|---|---|
| Specification | NEW-shot2play Technical Specification |
| Version | 2.0 |
| Document | Protocol Object Model |
| Status | PROTOCOL DESIGN BASELINE |
| Authority | Design Freeze — Approved Baseline |
| Terminology | Canonical Vocabulary |
| Object Relationships | Canonical Object Graph |

This document defines the logical object model used by the Version 2.0
protocol suite.

The object model defines the semantic structure, identity, state,
relationship, dependency, and lifecycle characteristics of protocol
objects.

This document does not prescribe a particular implementation language,
database, deployment topology, or cloud platform.

---

## 2. Scope

The Version 2.0 Object Model covers the principal objects participating
in:

- Transaction processing
- Identity processing
- Credential processing
- Authentication
- Entitlement management
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Evidence
- Audit

The object model SHALL remain consistent with the Design Freeze,
Canonical Vocabulary, and Canonical Object Graph.

---

## 3. Object Model Principles

### 3.1 Logical Object Principle

A protocol object represents a logical protocol entity.

A logical object MAY be represented physically as:

- A database record
- A signed message
- A token
- An API object
- A server-side state object
- A transient object
- A persistent object
- A reference to another object
- Another implementation-defined representation

The physical representation SHALL NOT alter the semantic meaning of the
object.

---

### 3.2 Object Identity Principle

Each object MAY have an Object Identifier.

An Object Identifier SHALL identify or reference the corresponding
logical object according to the applicable protocol.

An Object Identifier SHALL remain distinct from the object itself.

An Object Identifier SHALL NOT by itself establish:

- Authentication
- Entitlement
- Trust
- Authorization
- Execution permission

---

### 3.3 Object State Principle

An object MAY have a lifecycle state.

Object existence and Object State SHALL remain distinguishable.

For example, an object MAY exist while being:

- Active
- Expired
- Revoked
- Invalid
- Suspended
- Superseded
- Revalidation Required

The applicable protocol SHALL determine which states are defined for
each object class.

---

### 3.4 Object Validity Principle

Object validity represents whether an object may currently be relied
upon for its intended purpose.

Validity MAY depend on:

- Integrity
- Authenticity
- Scope
- Freshness
- Issuer
- Transaction binding
- Service binding
- Policy
- Security Context
- Lifecycle state

An object SHALL NOT be considered valid solely because it exists.

---

### 3.5 Object Dependency Principle

An object MAY depend on one or more other objects.

For example:

Authentication Result
        |
        v
Authorization Evaluation
        |
        v
Authorization Decision

Where an object depends on another object, the dependent object SHALL
remain subject to the validity and applicability requirements of the
dependency.

---

### 3.6 Object Reference Principle

An object MAY reference another object using:

- Object Identifier
- Reference
- Cryptographic reference
- Transaction Identifier
- Embedded object
- Signed object
- Server-side lookup key
- Other defined representation

A reference SHALL NOT by itself establish the validity of the
referenced object.

---

### 3.7 Object Binding Principle

An object MAY be bound to one or more contextual objects.

Binding MAY include:

- Identity binding
- Credential binding
- Transaction binding
- Service binding
- Resource binding
- Operation binding
- Entitlement binding
- Policy binding
- Security Context binding
- Temporal binding

The applicable Policy SHALL determine which bindings are required.

---

### 3.8 Object Scope Principle

An object MAY have an applicable Scope.

Scope MAY restrict use by:

- Identity
- Account
- Device
- Transaction
- Service
- Resource
- Requested Operation
- Entitlement
- Policy
- Time
- Security Context

An object valid within one Scope SHALL NOT automatically be valid
outside that Scope.

---

### 3.9 Object Freshness Principle

An object MAY have freshness requirements.

Freshness MAY be established through:

- Timestamp
- Expiration
- Nonce
- Sequence number
- Version
- Revalidation
- Server-side state
- Other defined mechanism

Where freshness is required, stale information SHALL NOT be treated as
current.

---

### 3.10 Object Invalidation Principle

An object that was previously valid MAY become invalid.

Invalidation MAY result from:

- Expiration
- Revocation
- Integrity failure
- Scope violation
- Policy change
- Context change
- Security event
- Transaction cancellation
- Dependency invalidation
- Trust failure

Invalidation MAY propagate to dependent objects where required by the
applicable Policy.

---

### 3.11 Separation of Semantic Stages

The following stages SHALL remain conceptually distinguishable:

1. Authentication
2. Entitlement
3. Policy Evaluation
4. Authorization Evaluation
5. Authorization Decision
6. Enforcement
7. Service Execution

An implementation MAY combine multiple stages within one physical
component.

Combining components SHALL NOT eliminate the semantic distinctions
between these stages.

---

### 3.12 No Implicit Authorization

No individual object SHALL automatically constitute authorization merely
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

Authorization SHALL be established through the applicable Authorization
Evaluation and Authorization Decision process.

---

### 3.13 No Implicit Execution

An Authorization Decision SHALL remain distinct from Service Execution.

Therefore:

Authorization Decision
        |
        v
Enforcement
        |
        v
Service Execution

A Permit Authorization Decision SHALL NOT itself constitute successful
Service Execution.

---

### 3.14 Cross-Service Principle

A logical object MAY be referenced or processed across multiple
Services.

The receiving Service SHALL validate:

- Trust
- Integrity
- Scope
- Freshness
- Binding
- Applicability

according to the applicable Policy.

A logical object valid in one Service SHALL NOT automatically be valid in
another Service.

---

## End of Part 1
# 4. Identity Object

## 4.1 Definition

The Identity Object represents the logical identity associated with a
subject participating in the Version 2.0 protocol suite.

An Identity MAY represent:

- A human user
- An organization
- A service
- A device
- An application
- Another protocol-defined subject

The Identity Object SHALL remain distinct from the Credential Object.

---

## 4.2 Identity Attributes

An Identity MAY contain or reference:

- Identity Identifier
- Subject Identifier
- Account Identifier
- Tenant Identifier
- Identity Status
- Identity Attributes
- Issuer
- Creation information
- Lifecycle information

Sensitive identity attributes MAY be represented by references,
pseudonymous identifiers, hashes, or other protected representations.

---

## 4.3 Identity Identifier

An Identity Identifier uniquely identifies or references an Identity
within the applicable namespace.

An Identity Identifier SHALL NOT itself constitute:

- A Credential
- Authentication
- An Entitlement
- An Authorization Decision

---

## 4.4 Identity State

An Identity MAY have lifecycle states including:

- Active
- Suspended
- Disabled
- Deleted
- Other Policy-defined states

An Identity in a state that does not satisfy the applicable Policy SHALL
NOT be treated as an eligible Identity for the affected operation.

---

# 5. Credential Object

## 5.1 Definition

The Credential Object represents an authentication-related object used
to establish or verify control associated with an Identity.

A Credential MAY include or reference:

- Credential Identifier
- Public Key
- Credential Type
- Credential Status
- Issuer
- Associated Identity
- Registration information
- Lifecycle information

The private key corresponding to a cryptographic Credential MAY remain
under the control of the credential holder and SHALL NOT be required to
be transmitted to the Authentication Server.

---

## 5.2 Credential and Identity Relationship

A Credential MAY be associated with one or more Identity records
according to the applicable protocol.

Conceptually:

Identity
    |
    +----> Credential

The existence of a Credential SHALL NOT by itself establish successful
Authentication.

---

## 5.3 Credential State

A Credential MAY have states including:

- Registered
- Active
- Suspended
- Revoked
- Expired
- Invalid

The applicable Authentication Protocol SHALL determine the permitted
state transitions.

A Credential that is not valid under the applicable Policy SHALL NOT be
accepted for successful Authentication.

---

## 5.4 Credential Identifier

A Credential Identifier identifies or references a Credential.

The Credential Identifier MAY be used to:

- Locate credential metadata
- Correlate authentication transactions
- Associate a Credential with an Identity
- Reference credential lifecycle state

The Credential Identifier SHALL NOT itself constitute proof of control of
the Credential.

---

## 5.5 Public Key

Where a public-key Credential is used, the Credential MAY contain or
reference a Public Key.

The Public Key MAY be used by the Authentication Server to verify an
Authentication Response.

The corresponding Private Key SHALL remain under the control of the
credential holder unless another protocol explicitly defines otherwise.

---

# 6. Transaction Object

## 6.1 Definition

The Transaction Object represents the logical unit used to correlate
processing associated with a Version 2.0 protocol transaction.

A Transaction MAY correlate multiple protocol stages.

Conceptually:

Transaction
    |
    +--> Authentication
    |
    +--> Entitlement
    |
    +--> Policy Evaluation
    |
    +--> Authorization Evaluation
    |
    +--> Service Execution

---

## 6.2 Transaction Identifier

A Transaction Identifier uniquely identifies or references a Transaction
within the applicable protocol scope.

The Transaction Identifier MAY be used for:

- Correlation
- State management
- Message association
- Audit
- Evidence
- Recovery
- Idempotency

The Transaction Identifier SHALL NOT itself constitute authorization.

---

## 6.3 Transaction State

A Transaction MAY have lifecycle states.

Example states include:

- Created
- Initialized
- Authentication Pending
- Authenticated
- Entitlement Pending
- Policy Evaluation Pending
- Authorization Pending
- Authorized
- Execution Pending
- Executing
- Completed
- Failed
- Cancelled
- Expired
- Invalidated

The exact state model MAY be defined by the applicable Protocol.

---

## 6.4 Transaction Lifecycle

A Transaction MAY progress through multiple protocol stages.

A conceptual lifecycle is:

Created
   |
   v
Authentication
   |
   v
Entitlement Processing
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

A Transaction MAY terminate before reaching any subsequent stage.

---

## 6.5 Transaction Binding

Protocol objects MAY be bound to a Transaction.

Transaction binding MAY include:

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

A Transaction binding SHALL NOT override the independent validity
requirements of the bound object.

---

# 7. Transaction Object References

## 7.1 Reference Model

A Transaction MAY contain references to related protocol objects.

Conceptually:

Transaction
 |
 +-- Identity
 |
 +-- Credential
 |
 +-- Authentication Result
 |
 +-- Entitlement
 |
 +-- Policy
 |
 +-- Context
 |
 +-- Authorization Decision
 |
 +-- Service Execution

The referenced objects MAY be physically stored or processed by
different components.

---

## 7.2 Reference Integrity

A reference to another object SHALL be validated according to the
applicable protocol.

Validation MAY include:

- Identifier validation
- Cryptographic validation
- Signature validation
- State validation
- Scope validation
- Transaction binding
- Service binding
- Freshness validation

---

# 8. Transaction Context Object

## 8.1 Definition

The Transaction Context Object represents contextual information
associated with a Transaction.

Transaction Context MAY contain or reference:

- Transaction Identifier
- Identity information
- Credential information
- Authentication Result
- Entitlement information
- Policy information
- Security Context
- Requested Operation
- Service Profile
- Resource information
- Environmental information
- Trust information

---

## 8.2 Context Assembly

Transaction Context MAY be assembled from multiple sources.

Conceptually:

Identity
     \
Credential
       \
Authentication Result
         \
Entitlement
           \
Policy --------> Context Assembly
           /
Security Context
         /
Service Profile
       /
Requested Operation

The resulting Context MAY be supplied to subsequent evaluation stages.

---

## 8.3 Context Integrity

Where Transaction Context affects a security-sensitive decision, the
integrity and provenance of the relevant Context SHALL be validated
according to the applicable Policy.

Untrusted or unverifiable Context SHALL NOT be treated as authoritative
where the applicable Policy requires trusted Context.

---

## 8.4 Context Reconstruction

A Transaction Context MAY be reconstructed from referenced protocol
objects.

Context Reconstruction SHALL preserve the semantic relationships
between the source objects.

Reconstruction SHALL NOT create authorization merely by combining
references.

---

## 8.5 Context State

Transaction Context MAY change during the Transaction lifecycle.

A change in Context MAY cause:

- Re-evaluation
- Revalidation
- Authorization Decision invalidation
- Enforcement restriction
- Transaction termination

where required by the applicable Policy.

---

## End of Part 2
# 9. Service Profile Object

## 9.1 Definition

The Service Profile Object defines the requirements and processing
characteristics applicable to a Service.

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

A Service Profile SHALL remain distinct from an Authorization Decision.

---

## 9.2 Service Profile Identifier

A Service Profile Identifier identifies or references a Service Profile.

The identifier MAY be used to retrieve or correlate the applicable
requirements.

The identifier SHALL NOT itself constitute authorization.

---

## 9.3 Service Profile Binding

A Transaction MAY be associated with a Service Profile.

Conceptually:

Transaction
     |
     v
Service Profile
     |
     +--> Authentication Requirements
     |
     +--> Entitlement Requirements
     |
     +--> Policy Requirements
     |
     +--> Authorization Requirements
     |
     +--> Enforcement Requirements

The applicable Service Profile SHALL determine which requirements apply
to the Transaction.

---

## 9.4 Service Profile Version

A Service Profile MAY have a version.

A version MAY be used to identify changes to:

- Required Authentication
- Required Entitlements
- Policy
- Authorization scope
- Enforcement
- Evidence
- Audit

Where a version is security-sensitive, the applicable Policy SHALL
determine whether the current version MUST be revalidated.

---

# 10. Authentication Result Object

## 10.1 Definition

The Authentication Result Object represents the result of an
Authentication operation.

An Authentication Result MAY indicate:

- Success
- Failure
- Indeterminate
- Rejected
- Expired
- Revalidation Required
- Other defined state

---

## 10.2 Authentication Result Attributes

An Authentication Result MAY contain or reference:

- Authentication Result Identifier
- Transaction Identifier
- Identity Identifier
- Credential Identifier
- Authentication method
- Authentication timestamp
- Authentication state
- Authentication assurance
- Security Context
- Expiration
- Issuer
- Evidence reference

---

## 10.3 Successful Authentication

A successful Authentication Result indicates that the applicable
Authentication requirements have been satisfied.

Successful Authentication SHALL NOT automatically establish:

- Entitlement
- Policy approval
- Authorization
- Service Execution

The Authentication Result MAY instead serve as an input to subsequent
processing.

---

## 10.4 Authentication Result Binding

An Authentication Result MAY be bound to:

- Identity
- Credential
- Transaction
- Service
- Requested Operation
- Security Context
- Time

The applicable Authentication Protocol SHALL determine the required
bindings.

---

## 10.5 Authentication Result Freshness

An Authentication Result MAY have a validity interval.

Where freshness is required, the Authentication Result SHALL be
revalidated according to the applicable Policy.

An expired Authentication Result SHALL NOT be treated as current solely
because Authentication was previously successful.

---

# 11. Entitlement Object

## 11.1 Definition

The Entitlement Object represents a right, qualification, condition,
permission-related state, or other authorization-related state that MAY
be considered during Authorization Evaluation.

An Entitlement MAY be:

- Created
- Issued
- Acquired
- Activated
- Evaluated
- Bound
- Consumed
- Expired
- Revoked
- Revalidated

An Entitlement SHALL remain distinct from Authentication credentials.

---

## 11.2 Entitlement Identifier

An Entitlement Identifier identifies or references an Entitlement.

The identifier MAY be used for:

- Entitlement lookup
- Correlation
- Lifecycle management
- Audit
- Evidence
- Cross-Service reference

The identifier SHALL NOT itself prove that the Entitlement is valid.

---

## 11.3 Entitlement Issuer

The Entitlement Issuer represents the entity or component that issues or
establishes an Entitlement.

The Issuer MAY be:

- A Service
- A trusted application
- A transaction processor
- An organization
- Another authorized entity

The receiving component MAY validate the Issuer according to the
applicable Trust Model and Policy.

---

## 11.4 Entitlement Holder

The Entitlement Holder represents the subject or entity to which an
Entitlement is associated.

An Entitlement MAY be bound to:

- Identity
- Account
- Device
- Transaction
- Service
- Other defined subject

An Entitlement bound to one holder SHALL NOT automatically be valid for
another holder.

---

## 11.5 Entitlement Scope

An Entitlement MAY have an applicable Scope.

Scope MAY include:

- Service
- Resource
- Requested Operation
- Transaction
- Identity
- Account
- Device
- Time
- Security Context

An Entitlement SHALL be evaluated against the applicable Scope before it
is relied upon for authorization.

---

## 11.6 Entitlement Condition

An Entitlement MAY include one or more Conditions.

A Condition MAY specify:

- Time
- Location
- Service
- Resource
- Transaction
- Prior event
- Required Authentication
- Required Context
- Required Policy
- Other defined condition

An Entitlement Condition SHALL remain distinguishable from the final
Authorization Decision.

---

## 11.7 Entitlement State

An Entitlement MAY have states including:

- Issued
- Acquired
- Active
- Suspended
- Consumed
- Expired
- Revoked
- Invalid

The exact state transitions SHALL be determined by the applicable
Entitlement Protocol and Policy.

---

## 11.8 Entitlement Lifecycle

A conceptual Entitlement lifecycle is:

Created
   |
   v
Issued / Acquired
   |
   v
Activated
   |
   v
Evaluated
   |
   +----> Consumed
   |
   +----> Expired
   |
   +----> Revoked
   |
   +----> Revalidated

An Entitlement MAY skip or repeat lifecycle stages where permitted by
the applicable protocol.

---

## 11.9 Entitlement Binding

An Entitlement MAY be bound to one or more objects.

Binding MAY include:

- Identity
- Transaction
- Service
- Resource
- Requested Operation
- Policy
- Authorization Decision
- Security Context
- Time

Binding SHALL restrict the circumstances under which the Entitlement
may be relied upon.

---

## 11.10 Entitlement Evaluation

Entitlement Evaluation determines whether an Entitlement satisfies the
conditions applicable to a particular Transaction or Requested
Operation.

Entitlement Evaluation MAY consider:

- Entitlement state
- Entitlement scope
- Entitlement condition
- Holder
- Issuer
- Transaction binding
- Service binding
- Resource binding
- Temporal validity
- Security Context
- Revocation state

The result of Entitlement Evaluation SHALL remain distinct from the
Entitlement itself.

---

# 12. Entitlement Evaluation Result Object

## 12.1 Definition

The Entitlement Evaluation Result Object represents the result produced
when an Entitlement is evaluated against applicable conditions.

The result MAY indicate:

- Satisfied
- Not Satisfied
- Indeterminate
- Expired
- Revoked
- Invalid
- Revalidation Required

---

## 12.2 Evaluation Result Attributes

An Entitlement Evaluation Result MAY contain or reference:

- Evaluation Result Identifier
- Entitlement Identifier
- Transaction Identifier
- Subject Identifier
- Service Profile Identifier
- Evaluation timestamp
- Evaluated Scope
- Evaluated Conditions
- Result state
- Validity interval
- Policy reference
- Security Context reference
- Evidence reference

---

## 12.3 Evaluation Result Binding

An Entitlement Evaluation Result MAY be bound to:

- Entitlement
- Transaction
- Requested Operation
- Service
- Policy
- Security Context

The result SHALL NOT be treated as universally applicable if its
bindings restrict its applicability.

---

## 12.4 Evaluation Result Freshness

An Entitlement Evaluation Result MAY have a defined freshness period.

Where freshness is required, the result SHALL be re-evaluated or
revalidated when the applicable freshness requirement is no longer
satisfied.

---

## 12.5 Entitlement Evaluation and Authorization

Entitlement Evaluation MAY provide an input to Authorization Evaluation.

Conceptually:

Entitlement
      |
      v
Entitlement Evaluation
      |
      v
Entitlement Evaluation Result
      |
      v
Authorization Evaluation
      |
      v
Authorization Decision

A satisfied Entitlement Evaluation Result SHALL NOT itself constitute
an Authorization Decision.

---

## End of Part 3
# 13. Policy Object

## 13.1 Definition

The Policy Object represents a defined set of rules, conditions,
constraints, or decision requirements applicable to one or more
protocol operations.

A Policy MAY define requirements for:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization
- Enforcement
- Service Execution
- Revalidation
- Revocation
- Evidence
- Audit

A Policy SHALL remain distinct from its evaluation result and from an
Authorization Decision.

---

## 13.2 Policy Identifier

A Policy Identifier identifies or references a Policy.

The identifier MAY be used for:

- Policy lookup
- Policy selection
- Policy correlation
- Version management
- Audit
- Evidence

The Policy Identifier SHALL NOT itself constitute an Authorization
Decision.

---

## 13.3 Policy Version

A Policy MAY have a version.

A Policy Version MAY be used to identify the specific rules applicable to
an evaluation.

Where Policy Version affects a security-sensitive decision, the
applicable Policy SHALL determine whether the version MUST be retained
with the resulting evaluation or decision.

---

## 13.4 Policy Scope

A Policy MAY be restricted by Scope.

Scope MAY include:

- Service
- Resource
- Requested Operation
- Transaction
- Identity
- Account
- Device
- Entitlement
- Security Context
- Time
- Tenant

A Policy SHALL be applied only where its Scope is applicable.

---

## 13.5 Policy Conditions

A Policy MAY contain one or more Conditions.

Conditions MAY refer to:

- Authentication Result
- Entitlement
- Entitlement Evaluation Result
- Transaction Context
- Security Context
- Service Profile
- Requested Operation
- Resource
- Time
- Trust State
- External State
- Other Policy-defined inputs

---

## 13.6 Policy Dependencies

A Policy MAY depend on other objects or external state.

Dependencies MAY include:

- Identity state
- Credential state
- Entitlement state
- Authentication state
- Transaction state
- Security Context
- Trust state
- Service state
- External event state

A dependency that is required by the Policy SHALL be considered during
Policy Evaluation.

---

## 13.7 Policy Conflict

Multiple Policies MAY apply to the same Transaction or Requested
Operation.

Where multiple Policies apply, the applicable Policy framework SHALL
define:

- Precedence
- Combination
- Conflict resolution
- Dependency handling
- Failure handling

An implementation SHALL NOT silently ignore a conflicting Policy where
the conflict affects a security-sensitive decision.

---

# 14. Policy Evaluation Object

## 14.1 Definition

The Policy Evaluation Object represents the processing operation in
which a Policy is applied to applicable decision inputs.

Conceptually:

Policy
   |
   v
Policy Evaluation
   |
   v
Policy Evaluation Result

Policy Evaluation SHALL remain distinct from the Policy itself.

---

## 14.2 Evaluation Inputs

Policy Evaluation MAY use:

- Authentication Result
- Entitlement Evaluation Result
- Transaction Context
- Security Context
- Service Profile
- Requested Operation
- Resource
- Trust State
- Policy Version
- External State
- Other Policy-defined inputs

The actual inputs SHALL be determined by the applicable Policy.

---

## 14.3 Evaluation Context

The Evaluation Context represents the set of information made available
to Policy Evaluation.

The Evaluation Context MAY include:

- Subject
- Identity
- Credential status
- Authentication state
- Entitlement state
- Transaction state
- Service
- Resource
- Requested Operation
- Security Context
- Environmental information
- Temporal information
- Trust information

The Evaluation Context SHALL preserve the semantic relationships
required by the applicable Policy.

---

## 14.4 Policy Evaluation Processing

Policy Evaluation MAY perform:

1. Input validation
2. Dependency validation
3. Scope validation
4. State validation
5. Condition evaluation
6. Precedence processing
7. Conflict resolution
8. Result generation

The exact sequence MAY be implementation-specific where semantic
requirements remain satisfied.

---

## 14.5 Policy Evaluation State

A Policy Evaluation MAY have states including:

- Created
- Pending
- Evaluating
- Satisfied
- Not Satisfied
- Indeterminate
- Failed
- Expired
- Invalidated
- Revalidation Required

The applicable protocol MAY define additional states.

---

## 14.6 Policy Evaluation Identifier

A Policy Evaluation MAY have a unique Evaluation Identifier.

The identifier MAY be used to:

- Correlate evaluation processing
- Reference an evaluation result
- Support audit
- Support evidence
- Support recovery

The Evaluation Identifier SHALL NOT itself establish authorization.

---

# 15. Policy Evaluation Result Object

## 15.1 Definition

The Policy Evaluation Result Object represents the result produced by
Policy Evaluation.

The result SHALL remain distinguishable from:

- Policy
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Service Execution

---

## 15.2 Result States

A Policy Evaluation Result MAY indicate:

- Satisfied
- Not Satisfied
- Permit Candidate
- Deny Candidate
- Indeterminate
- Failed
- Expired
- Invalidated
- Revalidation Required

The exact result states SHALL be defined by the applicable Policy
framework.

---

## 15.3 Result Attributes

A Policy Evaluation Result MAY contain or reference:

- Evaluation Result Identifier
- Policy Identifier
- Policy Version
- Transaction Identifier
- Subject Identifier
- Service Profile Identifier
- Requested Operation
- Resource
- Evaluation timestamp
- Evaluation Context reference
- Evaluated Conditions
- Result state
- Validity interval
- Dependency references
- Evidence reference
- Audit reference

---

## 15.4 Result Scope

A Policy Evaluation Result MAY have a defined Scope.

The Scope MAY include:

- Transaction
- Service
- Resource
- Requested Operation
- Identity
- Entitlement
- Security Context
- Time

The result SHALL NOT automatically be treated as applicable outside its
defined Scope.

---

## 15.5 Result Freshness

A Policy Evaluation Result MAY have a freshness requirement.

Where freshness is required, the result SHALL be re-evaluated or
revalidated when its freshness condition is no longer satisfied.

A stale Policy Evaluation Result SHALL NOT be treated as current solely
because it was previously valid.

---

## 15.6 Result Dependencies

A Policy Evaluation Result MAY depend on:

- Authentication Result
- Entitlement Evaluation Result
- Transaction Context
- Security Context
- Service Profile
- Requested Operation
- Resource
- External State

Where a required dependency becomes invalid, the Policy Evaluation Result
MAY become invalid or require re-evaluation according to the applicable
Policy.

---

## 15.7 Policy Evaluation and Authorization Evaluation

Policy Evaluation MAY provide one or more inputs to Authorization
Evaluation.

Conceptually:

Policy
   |
   v
Policy Evaluation
   |
   v
Policy Evaluation Result
   |
   +--------------------+
                        |
                        v
                Authorization
                  Evaluation
                        |
                        v
              Authorization Decision

A Policy Evaluation Result SHALL NOT itself constitute an Authorization
Decision.

---

## 15.8 Indeterminate Evaluation

A Policy Evaluation MAY produce an Indeterminate result where the
applicable requirements cannot be conclusively evaluated.

Indeterminate conditions MAY include:

- Required dependency unavailable
- Required Context unavailable
- Required state unavailable
- Integrity failure
- Trust failure
- External state unavailable
- Evaluation error

Where fail-closed behavior applies, an Indeterminate result SHALL NOT
produce an effective Permit Decision.

---

## 15.9 Policy Change

A change to an applicable Policy MAY affect previously generated Policy
Evaluation Results.

Where the applicable Policy requires revalidation, previously generated
results SHALL NOT automatically remain effective after a relevant Policy
change.

---

## 15.10 Policy Evaluation Evidence

A Policy Evaluation MAY generate Evidence describing:

- Policy used
- Policy Version
- Evaluation time
- Relevant inputs
- Evaluated conditions
- Result
- Dependency state

Evidence SHALL remain distinguishable from the Policy Evaluation Result
itself.

---

## End of Part 4
# 16. Authorization Evaluation Object

## 16.1 Definition

The Authorization Evaluation Object represents the logical evaluation
process that determines whether the conditions required for a Requested
Operation are satisfied.

Authorization Evaluation MAY consume multiple decision inputs.

Conceptually:

Authentication Result
          \
Entitlement Evaluation Result
            \
Policy Evaluation Result
              \
Transaction Context ----> Authorization Evaluation
              /
Security Context
            /
Service Profile
          /
Requested Operation

Authorization Evaluation SHALL remain distinguishable from the
Authorization Decision produced by the evaluation.

---

## 16.2 Authorization Evaluation Inputs

Authorization Evaluation MAY use:

- Authentication Result
- Entitlement Evaluation Result
- Policy Evaluation Result
- Transaction Context
- Security Context
- Service Profile
- Requested Operation
- Resource
- Identity
- Credential State
- Entitlement State
- Policy State
- Trust State
- Temporal State
- External State
- Other Policy-defined inputs

The applicable Policy SHALL determine which inputs are required.

---

## 16.3 Required Input Validation

Before producing an Authorization Decision, the applicable Authorization
Evaluation SHALL validate all required inputs.

Validation MAY include:

- Object existence
- Object integrity
- Object authenticity
- Object state
- Object Scope
- Object freshness
- Object binding
- Transaction binding
- Service binding
- Resource binding
- Requested Operation binding
- Security Context integrity
- Dependency validity

A required input that fails validation SHALL be handled according to
the applicable Policy.

---

## 16.4 Authorization Evaluation Context

The Authorization Evaluation Context represents the complete set of
decision-relevant information available to the Authorization
Evaluation.

The Context MAY contain:

- Subject
- Identity
- Authentication Result
- Entitlements
- Entitlement Evaluation Results
- Policy Evaluation Results
- Transaction Context
- Security Context
- Service Profile
- Requested Operation
- Resource
- Trust State
- Temporal State
- External State

The Authorization Evaluation Context SHALL preserve the semantic
relationships between its constituent objects.

---

## 16.5 Context Assembly

Authorization Evaluation Context MAY be assembled from multiple sources.

Conceptually:

Identity
Credential State
Authentication Result
Entitlement
Policy Evaluation Result
Security Context
Service Profile
Requested Operation
Resource
      |
      v
Context Assembly
      |
      v
Authorization Evaluation Context
      |
      v
Authorization Evaluation

Context Assembly SHALL NOT itself constitute authorization.

The resulting Context SHALL remain subject to validation by the
Authorization Evaluation.

---

## 16.6 Context Reconstruction

In a distributed implementation, the Authorization Evaluation Context MAY
be reconstructed from references to protocol objects.

Reconstruction MAY involve:

- Object retrieval
- Reference resolution
- State retrieval
- Signature verification
- Integrity verification
- Scope verification
- Freshness verification
- Dependency verification

The reconstructed Context SHALL preserve the semantic relationships
required by the applicable Policy.

A reconstructed Context SHALL NOT be considered trustworthy solely
because all referenced objects can be retrieved.

---

## 16.7 Requested Operation

The Requested Operation identifies the operation for which
Authorization is being evaluated.

A Requested Operation MAY specify:

- Operation Identifier
- Operation Type
- Target Service
- Target Resource
- Transaction Identifier
- Required Scope
- Required Entitlement
- Required Policy
- Requested parameters
- Other Policy-defined attributes

Authorization Evaluation SHALL be performed against the applicable
Requested Operation.

A resulting Authorization Decision SHALL NOT automatically authorize a
different Requested Operation.

---

## 16.8 Resource Binding

The Requested Operation MAY target one or more Resources.

A Resource MAY be identified by:

- Resource Identifier
- Resource Type
- Service
- Tenant
- Scope
- Other Policy-defined attributes

Where Resource binding is required, the Authorization Decision SHALL
remain bound to the applicable Resource.

A Decision valid for one Resource SHALL NOT automatically authorize
another Resource.

---

## 16.9 Service Binding

Authorization Evaluation MAY be bound to a Service.

Service binding MAY include:

- Service Identifier
- Service Profile Identifier
- Service Version
- Tenant
- Service Scope

Where Service binding is required, a Decision produced for one Service
SHALL NOT automatically authorize execution by another Service.

---

## 16.10 Transaction Binding

Authorization Evaluation MAY be bound to a Transaction.

Transaction binding MAY establish that the evaluated conditions apply only
to the specified Transaction.

A Decision bound to one Transaction SHALL NOT automatically authorize a
different Transaction.

---

## 16.11 Temporal Evaluation

Authorization Evaluation MAY consider temporal conditions.

These MAY include:

- Current time
- Decision validity interval
- Entitlement validity
- Authentication freshness
- Policy validity
- Transaction lifetime
- Revalidation interval

An Authorization Decision SHALL NOT remain effective beyond a required
validity interval unless the applicable Policy explicitly permits
continued use.

---

## 16.12 Security Context Evaluation

Authorization Evaluation MAY consider Security Context.

Security Context MAY include:

- Device state
- Credential state
- Trust state
- Session state
- Network state
- Risk state
- Security events
- Other Policy-defined security information

A change in Security Context MAY require:

- Re-evaluation
- Revalidation
- Decision invalidation
- Enforcement restriction
- Transaction termination

where required by the applicable Policy.

---

## 16.13 Dependency Evaluation

Authorization Evaluation SHALL consider required dependencies.

Dependencies MAY include:

Authentication Result
        |
        +----> Dependency
Entitlement Evaluation Result
        |
        +----> Dependency
Policy Evaluation Result
        |
        +----> Dependency
Security Context
        |
        +----> Dependency
Transaction Context
        |
        +----> Dependency

If a required dependency is invalid, unavailable, expired, revoked, or
otherwise fails the applicable requirements, the Authorization
Evaluation SHALL process that condition according to the applicable
Policy.

---

## 16.14 Dependency Precedence

Where multiple dependencies affect the same Authorization Evaluation,
the applicable Policy MAY define dependency precedence.

Examples include:

- Explicit Deny before Permit
- Revocation before Permit
- Invalid Authentication before Entitlement processing
- Expired Entitlement before Service Execution
- Security failure before Enforcement

The precedence model SHALL be deterministic where required for
interoperability.

---

## 16.15 Authorization Evaluation State

An Authorization Evaluation MAY have states including:

- Created
- Pending
- Context Assembly
- Validating
- Evaluating
- Permit Candidate
- Deny Candidate
- Indeterminate
- Completed
- Failed
- Invalidated
- Revalidation Required

The applicable protocol MAY define additional states.

---

## 16.16 Authorization Evaluation Identifier

An Authorization Evaluation MAY have a unique Evaluation Identifier.

The identifier MAY be used for:

- Correlation
- State management
- Evidence
- Audit
- Recovery
- Decision reference

The identifier SHALL NOT itself constitute authorization.

---

## 16.17 Evaluation Completion

Authorization Evaluation SHALL produce an evaluation outcome before an
Authorization Decision is treated as effective.

An evaluation outcome MAY indicate:

- Permit Candidate
- Deny Candidate
- Indeterminate
- Revalidation Required
- Failure

The exact outcome semantics SHALL be determined by the applicable
Policy.

---

## 16.18 No Implicit Decision

Completion of Authorization Evaluation SHALL NOT itself be treated as
Service Execution.

Similarly, the evaluation process SHALL remain distinguishable from the
Authorization Decision object.

Conceptually:

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

---

## End of Part 5A
# 17. Authorization Decision Object

## 17.1 Definition

The Authorization Decision Object represents the decision produced from
Authorization Evaluation.

The Authorization Decision determines whether a Requested Operation MAY
proceed under the applicable Policy and validated decision inputs.

An Authorization Decision SHALL remain distinct from:

- Authentication Result
- Entitlement
- Entitlement Evaluation Result
- Policy
- Policy Evaluation Result
- Authorization Evaluation
- Enforcement
- Service Execution
- Execution Result

---

## 17.2 Decision States

An Authorization Decision MAY have the following logical states:

- Permit
- Deny
- Indeterminate
- Revalidation Required
- Invalidated
- Expired
- Revoked

The applicable Policy MAY define additional states.

A state SHALL have a deterministic semantic meaning within the applicable
protocol.

---

## 17.3 Permit Decision

A Permit Decision indicates that the applicable authorization
requirements have been satisfied for the defined Scope and Requested
Operation.

A Permit Decision MAY authorize Enforcement to proceed.

A Permit Decision SHALL NOT itself constitute Service Execution.

---

## 17.4 Deny Decision

A Deny Decision indicates that the applicable authorization
requirements have not been satisfied.

A Deny Decision SHALL prevent Service Execution where the applicable
Policy requires enforcement of the decision.

A Deny Decision MAY be produced because of:

- Failed Authentication
- Missing Entitlement
- Invalid Entitlement
- Failed Policy Evaluation
- Scope violation
- Expired state
- Revoked state
- Security Context failure
- Trust failure
- Requested Operation restriction
- Resource restriction
- Other Policy-defined condition

---

## 17.5 Indeterminate Decision

An Indeterminate Decision indicates that the applicable authorization
requirements could not be conclusively evaluated.

Indeterminate conditions MAY include:

- Required Context unavailable
- Required dependency unavailable
- Integrity failure
- Trust failure
- External state unavailable
- Evaluation failure
- Conflicting inputs
- Other Policy-defined condition

Where fail-closed behavior applies, an Indeterminate Decision SHALL NOT
permit Service Execution.

---

## 17.6 Revalidation Required

A Revalidation Required state indicates that the current decision
cannot continue to be relied upon without additional validation.

Revalidation MAY be required because of:

- Expired freshness interval
- Security Context change
- Policy change
- Entitlement change
- Credential state change
- Transaction state change
- External event
- Other Policy-defined dependency change

---

## 17.7 Decision Identifier

An Authorization Decision SHALL have or reference a Decision Identifier
where required by the applicable protocol.

The Decision Identifier MAY be used for:

- Enforcement
- Correlation
- Audit
- Evidence
- Revalidation
- Revocation
- Recovery

The Decision Identifier SHALL NOT itself establish the validity of the
Decision.

---

## 17.8 Decision Attributes

An Authorization Decision MAY contain or reference:

- Decision Identifier
- Transaction Identifier
- Authorization Evaluation Identifier
- Subject Identifier
- Identity Identifier
- Service Identifier
- Service Profile Identifier
- Requested Operation
- Resource
- Entitlement references
- Policy Identifier
- Policy Version
- Security Context reference
- Decision state
- Decision Scope
- Creation timestamp
- Effective timestamp
- Expiration timestamp
- Revalidation requirements
- Dependency references
- Issuer
- Integrity information
- Evidence reference
- Audit reference

The exact representation SHALL be defined by the applicable protocol.

---

## 17.9 Decision Scope

An Authorization Decision MAY define one or more Scope restrictions.

Scope MAY include:

- Identity
- Account
- Device
- Transaction
- Service
- Resource
- Requested Operation
- Entitlement
- Policy
- Time
- Security Context

A Decision SHALL be effective only within its applicable Scope.

A Decision valid for one Scope SHALL NOT automatically be valid outside
that Scope.

---

## 17.10 Decision Binding

An Authorization Decision MAY be bound to:

- Transaction
- Service
- Resource
- Requested Operation
- Identity
- Entitlement
- Policy
- Security Context
- Time

Required bindings SHALL be determined by the applicable Policy.

A Decision lacking a required binding SHALL NOT be treated as applicable.

---

## 17.11 Decision Freshness

An Authorization Decision MAY have a freshness requirement.

Freshness MAY be represented by:

- Effective timestamp
- Expiration timestamp
- Validity interval
- Sequence number
- Version
- Revalidation requirement
- Server-side state

A stale Decision SHALL NOT automatically remain effective.

---

## 17.12 Decision Validity

Decision validity MAY depend on:

- Integrity
- Issuer
- Scope
- Binding
- Freshness
- Policy
- Dependency state
- Security Context
- Transaction state
- Revocation state

A Decision SHALL be validated according to the applicable Policy before
Enforcement where validation is required.

---

## 17.13 Decision Dependencies

An Authorization Decision MAY depend on:

- Authentication Result
- Entitlement Evaluation Result
- Policy Evaluation Result
- Transaction Context
- Security Context
- Service Profile
- Requested Operation
- Resource
- Trust State
- External State

Conceptually:

Authentication Result
        \
Entitlement Evaluation Result
          \
Policy Evaluation Result
            \
Security Context
              \
Transaction Context ---> Authorization Decision
              /
Service Profile
            /
Requested Operation

The Decision SHALL NOT be considered independently valid where a
required dependency has become invalid and the applicable Policy
requires dependency propagation.

---

## 17.14 Dependency Invalidation

Where a required dependency becomes invalid, the associated
Authorization Decision MAY become:

- Invalidated
- Expired
- Revoked
- Revalidation Required
- Denied

The applicable Policy SHALL determine the resulting state.

---

## 17.15 Decision Revocation

An Authorization Decision MAY be explicitly revoked.

Revocation MAY occur because of:

- Security event
- Entitlement revocation
- Credential revocation
- Policy change
- Administrative action
- Transaction cancellation
- External state change

A revoked Decision SHALL NOT remain effective where the applicable Policy
requires immediate invalidation.

---

## 17.16 Decision Revalidation

An Authorization Decision MAY require revalidation.

Revalidation MAY verify:

- Authentication state
- Entitlement state
- Policy state
- Security Context
- Transaction state
- Resource state
- Service state
- External state

A Decision requiring revalidation SHALL NOT be treated as continuously
effective where the applicable Policy requires revalidation before
continued Enforcement.

---

## 17.17 Decision Version

An Authorization Decision MAY have a version or sequence.

A version MAY be used to:

- Detect stale Decisions
- Detect conflicting Decisions
- Support state synchronization
- Support distributed processing
- Support revocation
- Support recovery

The applicable protocol SHALL define version semantics where versions
are used.

---

## 17.18 Decision Precedence

Multiple Authorization Decisions MAY exist for a related Transaction or
Requested Operation.

Where multiple Decisions may apply, the applicable Policy SHALL define:

- Applicability
- Precedence
- Conflict resolution
- Combination
- Invalidation behavior

An implementation SHALL NOT arbitrarily select between conflicting
security-sensitive Decisions.

---

## 17.19 Decision Consistency

In a distributed environment, components processing an Authorization
Decision SHOULD preserve semantic consistency.

Consistency MAY require:

- Decision Identifier
- Decision Version
- Transaction Identifier
- Dependency references
- Policy Version
- State Version
- Revalidation state

The exact synchronization mechanism MAY be implementation-specific.

---

## 17.20 Decision and Enforcement

An Authorization Decision provides the basis for Enforcement.

Conceptually:

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

Enforcement SHALL validate the applicability of the Decision according
to the applicable Policy.

A Permit Decision SHALL NOT bypass required Enforcement controls.

---

## 17.21 Decision and Service Execution

Service Execution occurs only after the applicable Enforcement
requirements have been satisfied.

Therefore:

Authorization Decision
        !=
Service Execution

A Permit Decision SHALL NOT guarantee:

- Technical execution success
- Business success
- Completion
- Availability
- Transaction completion

The outcome of execution SHALL be represented separately by an
Execution Result.

---

## 17.22 Decision and Entitlement Consumption

An Authorization Decision SHALL NOT automatically imply that an
Entitlement has been consumed.

Conceptually:

Entitlement
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

Entitlement Consumption MAY occur as a separate lifecycle operation.

Where consumption is required, the applicable Entitlement Protocol and
Policy SHALL define when consumption occurs.

---

## 17.23 Decision Evidence

An Authorization Decision MAY reference Evidence describing the basis
of the Decision.

Evidence MAY include:

- Evaluation inputs
- Policy Version
- Entitlement evaluation
- Authentication Result
- Security Context
- Decision dependencies
- Evaluation timestamp
- Decision state

Evidence SHALL remain distinguishable from the Authorization Decision.

---

## 17.24 Decision Audit

An Authorization Decision MAY generate Audit information.

Audit MAY record:

- Decision creation
- Decision state transition
- Decision use
- Decision invalidation
- Decision revocation
- Decision revalidation
- Enforcement result

Audit information SHALL remain distinguishable from the Decision
itself.

---

## End of Part 5B
# 18. Enforcement Object

## 18.1 Definition

The Enforcement Object represents the processing layer that applies an
Authorization Decision to the requested Service Execution.

Enforcement SHALL remain distinct from:

- Authorization Evaluation
- Authorization Decision
- Service Execution
- Execution Result

Conceptually:

Authorization Decision
          |
          v
      Enforcement
          |
          v
  Service Execution

---

## 18.2 Enforcement Input

Enforcement MAY consume:

- Authorization Decision
- Transaction Context
- Security Context
- Service Profile
- Requested Operation
- Resource
- Service State
- Other Policy-defined inputs

The applicable Policy SHALL determine which inputs are required.

---

## 18.3 Enforcement Validation

Before allowing Service Execution, Enforcement MAY validate:

- Decision state
- Decision Scope
- Decision binding
- Decision freshness
- Decision integrity
- Decision issuer
- Policy Version
- Dependency state
- Security Context
- Transaction state
- Service state

A Decision that fails a required validation SHALL NOT be enforced as a
Permit Decision.

---

## 18.4 Enforcement States

An Enforcement operation MAY have states including:

- Created
- Pending
- Validating
- Permitted
- Restricted
- Denied
- Executing
- Completed
- Failed
- Terminated
- Revalidation Required

The applicable protocol MAY define additional states.

---

## 18.5 Enforcement Identifier

An Enforcement operation MAY have an Enforcement Identifier.

The identifier MAY be used for:

- Correlation
- State management
- Audit
- Evidence
- Recovery
- Execution tracking

The Enforcement Identifier SHALL NOT itself constitute authorization.

---

## 18.6 Permit Enforcement

Where a valid Permit Decision is received, Enforcement MAY allow the
Requested Operation to proceed.

Permit Enforcement SHALL remain subject to:

- Decision validity
- Decision Scope
- Required bindings
- Freshness
- Security Context
- Service state
- Other applicable Policy requirements

---

## 18.7 Deny Enforcement

Where a Deny Decision is received, Enforcement SHALL prevent the
Requested Operation from proceeding where the applicable Policy
requires denial.

The denial MAY result in:

- Service rejection
- Transaction termination
- Error response
- Revalidation request
- Audit event

---

## 18.8 Indeterminate Enforcement

Where the Authorization Decision is Indeterminate, Enforcement SHALL
follow the applicable Policy.

Where fail-closed behavior applies, Enforcement SHALL NOT permit Service
Execution based solely on an Indeterminate Decision.

---

## 18.9 Revalidation Enforcement

Where a Decision indicates Revalidation Required, Enforcement MAY:

- Pause execution
- Request revalidation
- Re-run Authorization Evaluation
- Obtain a new Authorization Decision
- Terminate the Transaction

The applicable Policy SHALL determine the required behavior.

---

## 18.10 Enforcement and Decision Binding

Enforcement SHALL respect required Decision bindings.

A Permit Decision bound to:

- Transaction A

SHALL NOT automatically authorize:

- Transaction B

Similarly, a Decision bound to:

- Service A

SHALL NOT automatically authorize:

- Service B

unless the applicable Policy explicitly permits such reuse.

---

## 18.11 Enforcement Invalidation

An active Enforcement operation MAY be invalidated when:

- Authorization Decision is revoked
- Authorization Decision expires
- Required dependency becomes invalid
- Security Context changes
- Policy changes
- Transaction is cancelled
- Service state changes

Where immediate invalidation is required, Enforcement SHALL prevent
continued Service Execution according to the applicable Policy.

---

# 19. Service Execution Object

## 19.1 Definition

The Service Execution Object represents the actual execution of the
Requested Operation by the Service.

Service Execution SHALL occur only when the applicable Enforcement
requirements have been satisfied.

Service Execution SHALL remain distinct from the Authorization Decision.

---

## 19.2 Execution Input

Service Execution MAY consume:

- Requested Operation
- Authorization Decision
- Enforcement Result
- Transaction Context
- Service Profile
- Resource
- Service State
- Execution Parameters
- Other Policy-defined inputs

---

## 19.3 Execution Identifier

A Service Execution MAY have a unique Execution Identifier.

The identifier MAY be used for:

- Execution tracking
- Correlation
- Audit
- Evidence
- Recovery
- Business processing

The Execution Identifier SHALL remain distinct from the Authorization
Decision Identifier.

---

## 19.4 Execution State

A Service Execution MAY have states including:

- Created
- Accepted
- Queued
- Started
- Running
- Suspended
- Completed
- Failed
- Cancelled
- Terminated

The applicable Service Protocol MAY define additional states.

---

## 19.5 Execution Start

Execution SHALL be considered started only when the Service has
accepted the Requested Operation for actual processing.

A Permit Decision SHALL NOT itself constitute execution start.

Likewise, successful Enforcement SHALL NOT necessarily constitute
business execution completion.

---

## 19.6 Execution Processing

Service Execution MAY perform:

- Resource access
- Business operation
- Data processing
- State modification
- External service invocation
- Transaction processing
- Other Service-defined operations

Execution behavior SHALL remain subject to applicable Service and Policy
requirements.

---

## 19.7 Execution Failure

Service Execution MAY fail even when:

- Authentication succeeded
- Entitlement evaluation succeeded
- Policy evaluation succeeded
- Authorization Decision is Permit
- Enforcement succeeded

Execution failure SHALL therefore remain distinguishable from
authorization failure.

---

# 20. Execution Result Object

## 20.1 Definition

The Execution Result Object represents the outcome of Service Execution.

An Execution Result SHALL remain distinct from:

- Authorization Decision
- Enforcement Result
- Service Execution
- Business outcome where separately defined

---

## 20.2 Execution Result States

An Execution Result MAY indicate:

- Success
- Partial Success
- Failure
- Cancelled
- Terminated
- Timeout
- Rejected During Execution
- Other Service-defined state

---

## 20.3 Execution Result Attributes

An Execution Result MAY contain or reference:

- Execution Result Identifier
- Execution Identifier
- Transaction Identifier
- Authorization Decision Identifier
- Service Identifier
- Requested Operation
- Resource
- Execution state
- Start timestamp
- Completion timestamp
- Error information
- Business result reference
- Evidence reference
- Audit reference

---

## 20.4 Execution Result and Authorization

An Execution Result SHALL NOT retroactively establish that the
Authorization Decision was valid.

Similarly, a successful Execution Result SHALL NOT prove that the
authorization process was correctly performed unless the applicable
Evidence and Audit records establish that relationship.

---

## 20.5 Execution Result and Business Success

Technical execution success and business success MAY be different.

For example:

Service Execution
      |
      v
Technical Execution Result
      |
      v
Business Result

The applicable Service Protocol SHALL determine whether a separate
Business Result is required.

Business success SHALL be determined from the applicable Execution
Result or Business Result according to the Service definition.

---

## 20.6 Execution Result Evidence

An Execution Result MAY reference Evidence describing:

- Execution start
- Execution completion
- Execution state
- Resource affected
- Service operation
- Error condition
- Result

Evidence SHALL remain distinguishable from the Execution Result itself.

---

## 20.7 Execution Result Audit

An Execution Result MAY generate Audit information.

Audit MAY record:

- Execution start
- Execution completion
- Execution failure
- Cancellation
- Termination
- Result state transition

Audit SHALL remain distinguishable from the Execution Result.

---

# 21. Enforcement Result Object

## 21.1 Definition

The Enforcement Result Object represents the outcome of applying an
Authorization Decision to a Requested Operation.

An Enforcement Result MAY indicate:

- Permit
- Deny
- Restricted
- Revalidation Required
- Execution Started
- Execution Not Started
- Terminated
- Failed

---

## 21.2 Enforcement Result Attributes

An Enforcement Result MAY contain or reference:

- Enforcement Result Identifier
- Enforcement Identifier
- Authorization Decision Identifier
- Transaction Identifier
- Requested Operation
- Service Identifier
- Enforcement state
- Enforcement timestamp
- Decision validation result
- Evidence reference
- Audit reference

---

## 21.3 Enforcement Result and Execution

An Enforcement Result indicating Permit SHALL NOT necessarily indicate
that Service Execution completed.

Conceptually:

Authorization Decision
          |
          v
      Enforcement
          |
          v
  Enforcement Result
          |
          v
  Service Execution
          |
          v
  Execution Result

The two result objects SHALL remain distinguishable.

---

## 21.4 Enforcement Failure

Enforcement MAY fail even where an Authorization Decision is Permit.

Examples include:

- Decision unavailable
- Decision validation failure
- Service unavailable
- Security Context failure
- Transaction cancellation
- Enforcement infrastructure failure

Such failure SHALL NOT automatically convert the original
Authorization Decision into a Deny Decision unless the applicable Policy
explicitly defines such behavior.

---

# 22. Protocol Object Relationship Summary

The principal Version 2.0 processing relationship is:

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
Entitlement Evaluation Result
   |
   v
Policy
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
   |
   v
Enforcement
   |
   v
Enforcement Result
   |
   v
Service Execution
   |
   v
Execution Result

This sequence represents logical processing relationships.

An implementation MAY combine processing components, provided that the
semantic distinctions defined by this Object Model are preserved.

---

## 22.1 Separation of Processing Layers

The following distinctions SHALL be preserved:

Authentication
        !=
Entitlement
        !=
Policy Evaluation
        !=
Authorization Evaluation
        !=
Authorization Decision
        !=
Enforcement
        !=
Service Execution

Combining implementation components SHALL NOT eliminate these semantic
distinctions.

---

## 22.2 Separation of Result Objects

The following result objects SHALL remain distinguishable:

Authentication Result
Entitlement Evaluation Result
Policy Evaluation Result
Authorization Decision
Enforcement Result
Execution Result

A system MAY represent multiple results within a common data structure
provided that their semantic identities remain distinguishable.

---

## 22.3 Transaction Correlation

All processing objects MAY be correlated using a Transaction Identifier.

Conceptually:

Transaction
   |
   +--> Authentication Result
   |
   +--> Entitlement Evaluation Result
   |
   +--> Policy Evaluation Result
   |
   +--> Authorization Decision
   |
   +--> Enforcement Result
   |
   +--> Execution Result

Transaction correlation SHALL NOT by itself establish authorization.

---

## 22.4 State Dependency

The validity of a downstream object MAY depend on the state of an
upstream object.

For example:

Authentication Result
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

A change to a required upstream state MAY require downstream
revalidation, invalidation, or termination according to the applicable
Policy.

---

## End of Part 6
# 23. Evidence Object

## 23.1 Definition

The Evidence Object represents information that supports the
verification, reconstruction, explanation, or auditability of a
protocol operation or result.

Evidence SHALL remain distinguishable from the object or result that it
supports.

Evidence MAY support:

- Authentication
- Entitlement processing
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Execution Result
- State Transition
- Security Event
- Audit

---

## 23.2 Evidence Identifier

An Evidence Object MAY have a unique Evidence Identifier.

The identifier MAY be used for:

- Evidence retrieval
- Correlation
- Audit
- Verification
- Traceability

The Evidence Identifier SHALL NOT itself establish the validity of the
associated object.

---

## 23.3 Evidence Attributes

Evidence MAY contain or reference:

- Evidence Identifier
- Transaction Identifier
- Source Object Identifier
- Source Object Type
- Event Identifier
- Timestamp
- Issuer
- Policy reference
- Policy Version
- Input references
- State information
- Decision information
- Integrity information
- Verification information

Sensitive information MAY be represented by references, pseudonymous
identifiers, hashes, or other protected representations.

---

## 23.4 Evidence Integrity

Evidence MAY be protected by:

- Digital signature
- Hash
- MAC
- Secure storage
- Trusted execution environment
- Other integrity mechanism

The applicable Trust Model SHALL determine the required integrity
mechanism.

---

## 23.5 Evidence Relationship

Evidence MAY reference the object for which it was generated.

Conceptually:

Source Object
     |
     v
Protocol Operation
     |
     v
Evidence

Evidence SHALL NOT replace the semantic meaning of the Source Object.

---

# 24. Audit Object

## 24.1 Definition

The Audit Object represents a record of a protocol event, state
transition, decision, enforcement operation, or service execution.

Audit SHALL remain distinguishable from Evidence.

---

## 24.2 Audit Identifier

An Audit Object MAY have a unique Audit Identifier.

The identifier MAY be used for:

- Audit retrieval
- Correlation
- Compliance
- Investigation
- Traceability

---

## 24.3 Audit Attributes

An Audit Object MAY contain or reference:

- Audit Identifier
- Transaction Identifier
- Event Identifier
- Source Object Identifier
- Source Object Type
- Actor
- Service
- Operation
- Timestamp
- Previous State
- New State
- Result
- Policy reference
- Evidence reference
- Security Context reference

---

## 24.4 Audit Immutability

Where audit integrity is required, Audit records SHOULD be protected
against unauthorized modification.

Protection MAY include:

- Append-only storage
- Cryptographic integrity
- Trusted storage
- Hash chaining
- Digital signatures
- Other integrity mechanisms

---

## 24.5 Audit and Authorization

Audit MAY record:

- Authentication
- Entitlement changes
- Policy Evaluation
- Authorization Evaluation
- Authorization Decision
- Decision invalidation
- Enforcement
- Service Execution

Audit SHALL NOT itself constitute authorization.

---

# 25. State Transition Object

## 25.1 Definition

The State Transition Object represents a logical transition from one
state of a protocol object to another state.

A state transition MAY apply to:

- Transaction
- Authentication Result
- Entitlement
- Policy Evaluation
- Authorization Decision
- Enforcement
- Service Execution
- Other protocol-defined object

---

## 25.2 State Transition Attributes

A State Transition MAY contain or reference:

- Transition Identifier
- Object Identifier
- Object Type
- Previous State
- Trigger
- New State
- Timestamp
- Actor
- Policy reference
- Security Context
- Evidence reference
- Audit reference

---

## 25.3 State Transition Validity

A state transition SHALL be valid only where the applicable protocol
permits the transition.

An implementation SHALL NOT create an arbitrary state transition that
violates the applicable protocol or Policy.

---

## 25.4 State Transition Trigger

A transition MAY be triggered by:

- Protocol message
- User action
- Service action
- Administrative action
- Timer
- Expiration
- Revocation
- Security event
- External event
- Dependency state change

---

## 25.5 State Transition Dependency

A state transition MAY depend on the state of another object.

For example:

Entitlement Revocation
        |
        v
Authorization Decision Invalidation
        |
        v
Enforcement Restriction
        |
        v
Service Execution Termination

Where such dependency is defined by Policy, the downstream transition
SHALL be processed consistently with the upstream state change.

---

# 26. Dependency Relationship Object

## 26.1 Definition

The Dependency Relationship Object represents a logical dependency
between two protocol objects.

If Object B depends on Object A, the validity or applicability of B MAY
depend on the state of A.

---

## 26.2 Dependency Attributes

A Dependency Relationship MAY contain or reference:

- Source Object
- Dependent Object
- Dependency Type
- Dependency Scope
- Dependency State
- Dependency Version
- Dependency Condition
- Invalidation Rule
- Revalidation Rule

---

## 26.3 Dependency Types

Dependency types MAY include:

- Required
- Conditional
- Temporal
- Security
- Scope
- Integrity
- Policy
- Transaction
- Service
- Resource

---

## 26.4 Dependency Invalidation

Where a required dependency becomes invalid, the dependent object MAY
become:

- Invalid
- Expired
- Revoked
- Revalidation Required
- Denied

The applicable Policy SHALL determine the required result.

---

## 26.5 Dependency Propagation

Dependency state MAY propagate through multiple protocol layers.

Conceptually:

Object A
   |
   v
Object B
   |
   v
Object C
   |
   v
Object D

A state change in A MAY require state re-evaluation of B, C, and D.

Propagation SHALL be performed according to the applicable Policy.

---

# 27. Distributed Object Consistency

## 27.1 Definition

Version 2.0 MAY be implemented across distributed components.

In such an implementation, protocol objects MAY be stored or processed
by different components.

Distributed processing SHALL preserve the semantic identity and
applicable state of the objects.

---

## 27.2 Object Identity Across Components

An object MAY be referenced across components by:

- Object Identifier
- Transaction Identifier
- Version
- Sequence
- Cryptographic reference
- Other defined identifier

An identifier used for distributed correlation SHALL NOT itself
constitute authorization.

---

## 27.3 Object Versioning

Protocol objects MAY have versions.

Versioning MAY be used to detect:

- Stale state
- Conflicting state
- Missing updates
- Out-of-order processing
- Replayed state

The applicable protocol SHALL define version semantics where required.

---

## 27.4 Distributed State Reconstruction

A component MAY reconstruct a protocol object or decision context from
distributed references.

Reconstruction MAY require:

- Reference resolution
- State retrieval
- Integrity verification
- Version verification
- Dependency verification
- Policy verification
- Security Context verification

A reconstructed object SHALL NOT be considered valid solely because
its references are resolvable.

---

## 27.5 Distributed Decision Consistency

Where an Authorization Decision is processed by multiple components,
the components SHOULD preserve:

- Decision Identifier
- Decision Version
- Decision Scope
- Decision Binding
- Policy Version
- Dependency State
- Validity Interval

A component SHALL NOT silently substitute a different Decision where
the applicable protocol requires Decision identity preservation.

---

## 27.6 Decision State Propagation

A Decision state change MAY need to propagate to dependent components.

Examples include:

- Permit to Invalidated
- Permit to Revoked
- Permit to Expired
- Permit to Revalidation Required

Where immediate propagation is required by Policy, dependent Enforcement
components SHALL process the new state before allowing continued
Service Execution.

---

# 28. Security Context Object

## 28.1 Definition

The Security Context Object represents security-relevant state associated
with a Transaction, Identity, Device, Service, or Authorization
operation.

Security Context MAY include:

- Authentication state
- Credential state
- Device state
- Trust state
- Session state
- Risk state
- Security event state
- Network state
- Other Policy-defined security information

---

## 28.2 Security Context Identifier

A Security Context MAY have a unique identifier.

The identifier MAY be used for:

- Correlation
- State retrieval
- Audit
- Evidence
- Decision binding

The identifier SHALL NOT itself establish authorization.

---

## 28.3 Security Context Change

A Security Context MAY change during a Transaction.

A change MAY be caused by:

- Credential revocation
- Device state change
- Risk state change
- Security event
- Session change
- Administrative action
- External event

A Security Context change MAY invalidate a previously generated
Authorization Decision where the applicable Policy establishes such a
dependency.

---

## 28.4 Security Context Propagation

Security Context MAY be propagated between:

- Authentication
- Entitlement Evaluation
- Policy Evaluation
- Authorization Evaluation
- Enforcement
- Service Execution

Propagation SHALL preserve the security semantics required by the
applicable Policy.

---

# 29. Object State and Existence

## 29.1 Distinction

The existence of an object SHALL NOT by itself imply that the object is
valid or applicable.

For example:

An Authorization Decision MAY exist while being:

- Expired
- Revoked
- Invalidated
- Revalidation Required

Similarly, an Entitlement MAY exist while being:

- Expired
- Revoked
- Suspended
- Invalid

---

## 29.2 State Validation

Where an object's state affects a security-sensitive operation, the
state SHALL be evaluated before the object is relied upon.

Object retrieval SHALL NOT be treated as state validation.

---

## 29.3 State Freshness

An object MAY require current state.

Where freshness is required, the applicable protocol SHALL provide a
mechanism for:

- Fresh state retrieval
- Revalidation
- Version comparison
- Revocation checking
- State synchronization

---

# 30. Object Lifecycle Summary

The principal object lifecycle relationships MAY be represented as:

Identity
   |
   v
Credential
   |
   v
Authentication Result
   |
   v
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
Enforcement Result
   |
   v
Execution Result

Each object MAY have an independent lifecycle.

An object lifecycle SHALL NOT be inferred solely from the lifecycle of
another object unless an explicit dependency is defined.

---

## 30.1 Independent Lifecycle Principle

The following objects SHALL remain independently stateful:

- Entitlement
- Policy
- Authorization Decision
- Enforcement
- Service Execution

For example:

A Service Execution MAY complete while an Authorization Decision later
becomes expired.

An Entitlement MAY be revoked after a Service Execution has completed.

A Policy MAY change after a historical Authorization Decision has been
recorded.

Historical state SHALL remain distinguishable from current validity.

---

## 30.2 Historical State

A protocol implementation MAY preserve historical object states for:

- Evidence
- Audit
- Compliance
- Investigation
- Recovery
- Dispute resolution

Historical state SHALL NOT automatically be treated as current state.

---

## 30.3 Lifecycle Correlation

Related lifecycle events MAY be correlated by:

- Transaction Identifier
- Object Identifier
- Event Identifier
- Decision Identifier
- Execution Identifier

Correlation SHALL NOT eliminate the semantic distinction between
objects.

---

## End of Part 7
