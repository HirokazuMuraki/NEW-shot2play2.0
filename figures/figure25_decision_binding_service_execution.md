# Figure25 Decision Binding and Service Execution Model

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure25 |
| Title | Decision Binding and Service Execution Model |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Define how an authorization decision is bound to a protected operation and consumed by service execution |

---

# 1. Purpose

Figure25 defines the processing model by which an authorization
decision is bound to a specific protected operation and subsequently
consumed by the Enforcement and Service Execution Domains.

The model establishes the following sequence:

Authentication

↓

Entitlement

↓

Policy Evaluation

↓

Authorization Decision

↓

Decision Binding

↓

Enforcement

↓

Service Execution

The authorization decision SHALL NOT constitute unrestricted authority.
It SHALL be valid only within the security context and operation for
which it was generated.

---

# 2. Core Principle

A protected operation SHALL be executable only when a valid
authorization decision is bound to the corresponding:

- Subject
- Transaction
- Resource
- Action
- Policy
- Policy Version
- Applicable Entitlement
- Decision Validity Period

The binding relationship prevents a valid decision from being reused for
an unrelated operation.

---

# 3. Protected Operation

A protected operation is represented by a request containing the
information necessary to identify the requested service operation.

Representative elements include:

- Subject Identifier
- Transaction Identifier
- Resource Identifier
- Action Identifier
- Request Identifier
- Authentication Context
- Entitlement Context
- Request Parameters where applicable

The protected operation SHALL be uniquely associated with its current
transaction.

---

# 4. Authorization Decision

The Authorization Domain produces a decision associated with the
evaluation context.

Representative decision elements include:

- Decision Identifier
- Decision Result
- Subject Identifier
- Transaction Identifier
- Resource Identifier
- Action Identifier
- Policy Identifier
- Policy Version
- Entitlement Identifier
- Creation Time
- Expiration Time
- Decision Version
- Integrity Protection

The decision SHALL be uniquely identifiable.

---

# 5. Decision Binding

Decision Binding establishes the relationship between the authorization
decision and the protected operation.

Representative bindings include:

Decision

↔ Subject

Decision

↔ Transaction

Decision

↔ Resource

Decision

↔ Action

Decision

↔ Policy Version

Decision

↔ Entitlement

A decision SHALL NOT be considered valid if any required binding fails.

---

# 6. Subject Binding

The decision SHALL be bound to the subject for whom authorization was
evaluated.

A decision issued for one subject SHALL NOT be reused by another subject.

Subject identity SHALL be derived from an authoritative authentication
context or another explicitly defined trusted source.

---

# 7. Transaction Binding

The decision SHALL be associated with the transaction for which it was
created.

A decision created for Transaction A SHALL NOT be reused for Transaction
B unless an explicit protocol rule defines such reuse.

Transaction binding prevents replay of an otherwise valid authorization
decision across unrelated operations.

---

# 8. Resource Binding

The decision SHALL identify the resource to which authorization applies.

A decision for Resource A SHALL NOT authorize access to Resource B
unless the policy explicitly defines a resource scope covering both.

Resource identifiers SHALL be canonicalized before comparison where
required.

---

# 9. Action Binding

The decision SHALL identify the permitted action.

Representative actions include:

- READ
- WRITE
- CREATE
- UPDATE
- DELETE
- EXECUTE
- TRANSFER
- REDEEM

A decision authorizing one action SHALL NOT implicitly authorize a
different action.

---

# 10. Policy Binding

The decision SHALL identify the policy used to produce the decision.

The binding SHALL include:

- Policy Identifier
- Policy Version

A decision generated under one policy version SHALL NOT silently be
interpreted under another policy version.

---

# 11. Entitlement Binding

Where entitlement is part of the authorization condition, the decision
SHALL identify the entitlement context used during evaluation.

Representative elements include:

- Entitlement Identifier
- Entitlement State
- Entitlement Version
- Entitlement Validity
- Entitlement Transaction Binding

If the entitlement state materially changes, the decision MAY become
invalid and require reevaluation.

---

# 12. Temporal Binding

The decision SHALL have a defined validity interval where required.

Representative temporal elements include:

- Decision Creation Time
- Decision Not-Before Time
- Decision Expiration Time

The Enforcement Domain SHALL reject a decision that is outside its
validity interval.

---

# 13. Integrity Binding

The authorization decision SHALL provide an integrity mechanism
appropriate to the protocol.

Representative mechanisms MAY include:

- Digital Signature
- MAC
- Authenticated Token
- Secure Channel Protection
- Trusted Internal Reference

The selected mechanism SHALL prevent unauthorized modification of the
decision.

---

# 14. Binding Verification

Before enforcement, the Enforcement Domain SHALL verify the required
bindings.

Representative verification steps are:

1. Verify Decision Integrity
2. Verify Decision Freshness
3. Verify Subject Binding
4. Verify Transaction Binding
5. Verify Resource Binding
6. Verify Action Binding
7. Verify Policy Version
8. Verify Entitlement State where applicable
9. Verify Request Consistency
10. Permit or Reject Execution

Failure of a required verification SHALL prevent execution.

---

# 15. Request Consistency

The Enforcement Domain SHALL compare the authorization decision with the
current request.

At minimum, the following SHALL be consistent:

- Subject
- Transaction
- Resource
- Action

Where applicable, consistency SHALL also include:

- Entitlement
- Policy Version
- Request Scope
- Security Context

A mismatch SHALL result in rejection or controlled recovery.

---

# 16. Decision Consumption

A valid authorization decision SHALL be consumed by the Enforcement
Domain.

Consumption MAY be:

- Single-use
- Limited-use
- Time-limited
- Transaction-scoped
- Service-scoped

The selected consumption model SHALL be explicitly defined.

Where the protocol requires one-time authorization, successful
consumption SHALL prevent subsequent reuse.

---

# 17. Replay Prevention

The system SHALL prevent unauthorized replay of authorization decisions.

Representative mechanisms include:

- Transaction Binding
- Decision Identifier Tracking
- Nonce
- Request Identifier
- Expiration
- Consumption State
- Monotonic Sequence
- Replay Detection Record

Replay detection SHALL be performed before protected execution where
required.

---

# 18. Consumption State

Where authorization decisions are stateful, the decision MAY transition
through states such as:

- ISSUED
- VALID
- CONSUMED
- EXPIRED
- REVOKED
- INVALID

A consumed, expired, revoked, or invalid decision SHALL NOT be used for
a new protected operation.

---

# 19. Enforcement Decision

The Enforcement Domain produces the final execution determination.

Representative enforcement outcomes include:

| Outcome | Meaning |
|---|---|
| EXECUTE | Protected operation may proceed |
| REJECT | Protected operation shall not proceed |
| REAUTH | Authentication must be renewed |
| REENTITLE | Entitlement must be established or updated |
| REAUTHORIZE | A new authorization decision is required |
| RECOVER | Controlled recovery processing is required |

The enforcement result SHALL be associated with the transaction.

---

# 20. Service Execution

Service Execution SHALL occur only after successful enforcement.

The Service Execution Domain SHALL receive the authorized operation
within the scope established by the decision.

The service SHALL NOT independently expand the authorization scope.

---

# 21. Scope Preservation

The scope granted by the authorization decision SHALL be preserved
through service execution.

For example:

Authorization:

Resource = R1

Action = READ

Service Execution SHALL NOT transform this into:

Resource = R2

Action = WRITE

without a new authorization decision.

---

# 22. Parameter Binding

Where request parameters affect authorization semantics, the decision
SHALL be bound to the relevant parameters or to a canonical representation
of their security-relevant properties.

Representative security-relevant parameters include:

- Amount
- Quantity
- Destination
- Account
- Resource Subset
- Geographic Scope
- Time Window

A material parameter change SHALL trigger reevaluation where required.

---

# 23. Transaction State

The Enforcement and Service Execution Domains SHALL maintain awareness
of the transaction state where transaction binding is required.

Representative transaction states include:

- CREATED
- AUTHORIZED
- EXECUTING
- COMPLETED
- FAILED
- CANCELLED
- EXPIRED

A transaction state inconsistent with the authorization decision SHALL
prevent execution.

---

# 24. Authorization-to-Execution Continuity

The authorization decision SHALL maintain semantic continuity from
policy evaluation through service execution.

The processing chain is:

Policy Evaluation

↓

Authorization Decision

↓

Decision Binding

↓

Binding Verification

↓

Enforcement

↓

Service Execution

No implicit authorization step SHALL be introduced between these stages.

---

# 25. Conditional Execution

A conditional authorization decision SHALL identify the condition that
must be satisfied before execution.

Representative conditions include:

- Additional Authentication
- Additional Entitlement
- Transaction State
- Time Condition
- Location Condition
- Resource Condition
- Action Condition

The condition SHALL be verified before service execution.

---

# 26. Reauthentication Path

Where additional authentication is required:

Authorization Decision

↓

REAUTH

↓

Authentication Update

↓

Security Context Update

↓

Policy Reevaluation

↓

New Authorization Decision

↓

Binding Verification

↓

Execution

The previous decision SHALL NOT be treated as an unconditional allow.

---

# 27. Reentitlement Path

Where additional entitlement is required:

Authorization Decision

↓

REENTITLE

↓

Entitlement Establishment / Update

↓

Entitlement Validation

↓

Security Context Update

↓

Policy Reevaluation

↓

New Authorization Decision

↓

Binding Verification

↓

Execution

The entitlement update SHALL originate from an authoritative source.

---

# 28. Reauthorization Path

Where the authorization decision becomes invalid because of a material
context change:

Existing Decision

↓

Invalidation

↓

Context Reevaluation

↓

Policy Evaluation

↓

New Authorization Decision

↓

Decision Binding

↓

Enforcement

↓

Service Execution

The previous decision SHALL NOT silently remain effective.

---

# 29. Failure Handling

The system SHALL define controlled handling for:

- Binding Failure
- Integrity Failure
- Expiration
- Replay Detection
- Transaction Mismatch
- Resource Mismatch
- Action Mismatch
- Entitlement Mismatch
- Policy Version Mismatch
- Parameter Mismatch
- Service Execution Failure

Failure SHALL NOT silently degrade into execution.

---

# 30. Service Execution Failure

If service execution fails after authorization:

- The failure SHALL be associated with the transaction.
- The authorization decision SHALL remain distinguishable from the
  execution result.
- Consumption state SHALL be updated according to protocol rules.
- Recovery SHALL NOT automatically grant new authorization.
- A retry MAY require a new authorization decision where the operation
  semantics have materially changed.

---

# 31. Audit Trace

The complete authorization-to-execution lifecycle SHOULD be traceable
using correlated identifiers.

Representative identifiers include:

- Request Identifier
- Transaction Identifier
- Authentication Identifier
- Entitlement Identifier
- Policy Identifier
- Policy Version
- Evaluation Identifier
- Decision Identifier
- Enforcement Identifier
- Service Execution Identifier
- Event Identifier

The audit record SHOULD permit reconstruction of:

Request

↓

Evaluation

↓

Decision

↓

Binding

↓

Enforcement

↓

Execution

---

# 32. Authority Separation

The model preserves the following separation:

Authentication

= Establish identity and authentication state

Entitlement

= Establish eligibility or rights state

Policy Evaluation

= Determine applicable policy result

Authorization

= Produce an enforceable decision

Enforcement

= Verify and apply the decision

Service Execution

= Perform the authorized operation

No layer SHALL silently assume the authority of another layer.

---

# 33. Security Properties

The Decision Binding and Service Execution Model provides:

- Subject binding
- Transaction binding
- Resource binding
- Action binding
- Policy version binding
- Entitlement binding
- Temporal validity
- Integrity protection
- Request consistency
- Replay prevention
- Decision consumption
- Scope preservation
- Parameter binding
- Transaction-state validation
- Conditional execution
- Reauthentication support
- Reentitlement support
- Reauthorization support
- Controlled failure handling
- Audit traceability
- Authority separation

---

# 34. Non-Goals

Figure25 does not define:

- A specific service implementation
- A specific API protocol
- A specific database
- A specific cryptographic algorithm
- A specific policy language
- A specific cloud platform
- A specific programming language
- A specific user interface

Implementation details MAY be defined elsewhere.

---

# 35. Design Freeze Decision

Figure25 is designated as the reference model for authorization decision
binding, enforcement verification, and protected service execution within
the NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

