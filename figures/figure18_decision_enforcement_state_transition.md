# Figure18 Decision Enforcement and State Transition

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure18 |
| Title | Decision Enforcement and State Transition |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of the enforcement and entitlement state transition following an authorization decision |

---

# 1. Purpose

Figure18 defines the relationship between an Authorization Decision,
the Service Enforcement Point, the protected Service Action, and the
subsequent Entitlement State Transition.

The model establishes a strict separation between:

- Authorization Evaluation
- Authorization Decision
- Service Enforcement
- Service Execution
- Consumption
- Entitlement State Update

An Allow decision permits the requested action.

A Deny decision prevents the protected action.

---

# 2. Fundamental Decision Enforcement Model

The fundamental relationship is:

Authorization Request

        ↓

Authorization Evaluation

        ↓

Authorization Decision

        ↓

Service Enforcement Point

        ↓

Allow → Service Action → Consumption → State Update

Deny → Service Action Blocked

The enforcement point SHALL enforce the authorization result.

---

# 3. Authorization Decision

The Authorization Decision contains the result of policy evaluation.

Primary decision values are:

- Allow
- Deny

Representative decision information includes:

- Decision
- Authorization Request Identifier
- Subject Identifier
- Entitlement Identifier
- Policy Identifier
- Policy Version
- Decision Time
- Evaluated Entitlement State

The decision SHALL remain traceable to the authorization request.

---

# 4. Service Enforcement Point

The Service Enforcement Point (SEP) is the logical boundary that
enforces the authorization result before the protected service action
is performed.

The SEP SHALL:

- Receive the authorization result
- Validate that the result applies to the requested action
- Prevent the protected action when Deny
- Permit the protected action when Allow
- Preserve required transaction references

The SEP MAY be implemented within an application, gateway,
authorization service, API layer, or another trusted enforcement
component.

---

# 5. Allow Path

The Allow path is:

Authorization Decision = Allow

        ↓

Service Enforcement Point

        ↓

Service Action

        ↓

Consumption Transaction

        ↓

Entitlement State Update

The Service Action SHALL NOT be considered authorized solely because
authentication succeeded.

---

# 6. Deny Path

The Deny path is:

Authorization Decision = Deny

        ↓

Service Enforcement Point

        ↓

Service Action Blocked

        ↓

No Consumption

        ↓

No Entitlement Consumption State Change

A Deny decision SHALL prevent the protected operation.

---

# 7. Authorization Result Binding

The enforcement point SHALL verify that the authorization result
corresponds to the requested operation.

Representative binding attributes include:

- Subject Identifier
- Action
- Resource
- Service
- Entitlement Identifier
- Authorization Request Identifier
- Decision Time
- Decision Expiration
- Policy Version

A result that does not match the requested operation SHALL NOT be
accepted.

---

# 8. Decision Expiration

An Authorization Decision MAY have a validity period.

If the decision has expired:

Authorization Decision

        ↓

Expired

        ↓

Not Applicable

The service SHALL NOT execute the protected operation based solely
on an expired decision.

A new authorization evaluation MAY be required.

---

# 9. Decision Replay Protection

An authorization result SHALL NOT be replayed to obtain an
additional unauthorized service action.

Replay protection MAY use:

- Authorization Request Identifier
- Decision Identifier
- Nonce
- Timestamp
- Expiration
- Transaction State
- Consumption State

A completed one-time authorization result SHALL NOT automatically
authorize another consumption.

---

# 10. Service Action

The Service Action represents the protected operation.

Examples include:

- Applying a Discount
- Redeeming a Benefit
- Accessing a Protected Resource
- Executing a Transaction
- Consuming a Service
- Granting a Capability

The Service Action SHALL be associated with the authorization result
that permitted it.

---

# 11. Consumption Transaction

If the Service Action consumes an entitlement, the system SHALL
create or record a Consumption Transaction according to the
applicable Service Profile.

Representative attributes include:

- Consumption Transaction Identifier
- Authorization Request Identifier
- Authorization Decision
- Entitlement Identifier
- Subject Identifier
- Quantity Consumed
- Consumption Time
- Service Result

The Consumption Transaction SHALL remain independently identifiable.

---

# 12. Entitlement State Update

Following successful consumption, the authoritative Entitlement
state SHALL be updated.

Representative state changes include:

- Remaining Quantity Decreased
- Usage Count Increased
- Last Used Time Updated
- Consumption State Updated
- Completion State Updated

The update SHALL reference the transaction that caused the state
change.

---

# 13. State Transition Atomicity

Where the Service Action and Entitlement Consumption are logically
coupled, the implementation SHALL prevent inconsistent state.

The protected sequence is:

Allow

        ↓

Service Action

        ↓

Consumption

        ↓

State Update

The implementation SHALL define failure handling for each boundary.

---

# 14. Successful State Transition

A successful consumable operation follows:

Pre-Consumption State

        ↓

Authorization = Allow

        ↓

Service Action Successful

        ↓

Consumption Recorded

        ↓

Post-Consumption State

The post-consumption state SHALL reflect the actual successful
operation.

---

# 15. Failed Service Action

If Authorization = Allow but the Service Action fails, the
implementation SHALL define whether the Consumption Transaction is:

- Not created
- Rolled back
- Compensated
- Recorded as failed

The selected behavior SHALL be defined by the applicable Service
Profile.

---

# 16. Partial Failure

The implementation SHALL account for failures between:

- Authorization and Service Action
- Service Action and Consumption
- Consumption and State Update

The system SHALL avoid silently treating a partially completed
operation as fully completed.

---

# 17. Idempotency

Consumption operations SHOULD be idempotent.

The Consumption Transaction Identifier SHOULD serve as the
idempotency key.

Repeated processing of the same successful consumption request SHALL
NOT consume the same entitlement more than once.

---

# 18. Concurrency Control

Concurrent consumption attempts against the same Entitlement SHALL
be controlled according to the applicable consistency requirement.

The implementation SHALL prevent:

- Double consumption
- Negative remaining quantity
- Lost state updates
- Conflicting state transitions

Concurrency mechanisms MAY include:

- Atomic state transition
- Conditional update
- Optimistic concurrency
- Pessimistic locking
- Version checks

---

# 19. State Version

The Entitlement MAY contain a state revision.

Representative information includes:

- Entitlement State Version
- Previous State Version
- New State Version
- Update Transaction Identifier

A state transition SHOULD verify the expected previous state.

---

# 20. Audit Chain

The enforcement flow SHOULD provide an auditable chain:

Authorization Request

        ↓

Authorization Decision

        ↓

Service Enforcement

        ↓

Service Action

        ↓

Consumption Transaction

        ↓

Entitlement State Update

This chain supports:

- Audit
- Security Investigation
- Dispute Resolution
- Transaction Reconstruction

---

# 21. Security Boundary

The Service Enforcement Point SHALL be treated as a trusted
security boundary.

The protected Service Action SHALL NOT be directly executable in a
manner that bypasses the enforcement point when authorization is
required.

---

# 22. Fail-Closed Behavior

If the enforcement point cannot reliably determine whether a
required authorization result is valid, the protected action SHOULD
be denied.

Examples include:

- Missing decision
- Invalid decision
- Expired decision
- Mismatched subject
- Mismatched resource
- Mismatched action
- Invalid entitlement reference
- Integrity failure

Uncertainty SHALL NOT be interpreted as Allow unless explicitly
permitted by policy.

---

# 23. Version 2.0 Example

A representative Version 2.0 transaction is:

Visit Evidence

        ↓

Discount Entitlement

        ↓

EC Authorization Request

        ↓

Authorization Evaluation

        ↓

Authorization = Allow

        ↓

Service Enforcement Point

        ↓

Discount Applied

        ↓

Consumption Transaction

        ↓

Discount Entitlement State Updated

If the authorization result is Deny:

EC Authorization Request

        ↓

Authorization = Deny

        ↓

Service Enforcement Point

        ↓

Discount Not Applied

        ↓

No Consumption

---

# 24. Decision / Enforcement Separation

The Authorization Decision and Service Enforcement Point are
logically distinct.

Authorization Decision answers:

"Is the requested action permitted?"

Service Enforcement answers:

"Will the protected service action be allowed to execute?"

The separation provides a clear security boundary and enables
independent verification of authorization results.

---

# 25. Security Properties

The model provides:

- Explicit authorization decision
- Enforcement-point control
- Allow / Deny separation
- Decision binding
- Decision expiration
- Replay protection
- Consumption idempotency
- Concurrency protection
- State versioning
- Fail-closed behavior
- Auditability
- Transaction traceability

---

# 26. Non-Goals

Figure18 does not define:

- A specific transaction manager
- A specific database locking mechanism
- A specific API gateway
- A specific policy engine
- A specific application architecture
- A specific distributed consensus mechanism

Implementation-specific mechanisms MAY be defined elsewhere.

---

# 27. Design Freeze Decision

Figure18 is designated as the Decision Enforcement and State
Transition reference model for NEW-shot2play Protocol Suite
Version 2.0.

Future modifications require Design Freeze review.

