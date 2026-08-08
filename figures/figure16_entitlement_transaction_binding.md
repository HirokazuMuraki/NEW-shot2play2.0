# Figure16 Entitlement Transaction Binding

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure16 |
| Title | Entitlement Transaction Binding |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of the relationship between transactions, evidence, entitlement state, and authorization |

---

# 1. Purpose

Figure16 defines how an Entitlement is created, referenced,
validated, consumed, and updated across protocol transactions.

The figure establishes the relationship between:

- Transaction
- Evidence
- Entitlement
- Policy Evaluation
- Authorization
- Consumption
- State Update

The model ensures that an authorization decision is based on the
authoritative Entitlement state rather than on authentication alone.

---

# 2. Fundamental Binding Model

The fundamental relationship is:

Transaction

        ↓

Evidence

        ↓

Entitlement

        ↓

Policy Evaluation

        ↓

Authorization

        ↓

Service Action

        ↓

Entitlement State Update

The resulting state update SHALL remain associated with the
Entitlement Identifier.

---

# 3. Transaction Object

A Transaction represents an atomic protocol or business event.

Representative attributes include:

- Transaction Identifier
- Transaction Type
- Subject Identifier
- Service Identifier
- Timestamp
- Request Data
- Result
- Evidence Reference
- Entitlement Reference

Each transaction SHALL have a unique Transaction Identifier.

---

# 4. Evidence Object

Evidence represents information supporting the occurrence or result
of an event.

Representative evidence includes:

- Visit Evidence
- Attendance Evidence
- Purchase Evidence
- Registration Evidence
- Authentication Evidence
- Consumption Evidence
- External Event Evidence

Evidence SHOULD contain:

- Evidence Identifier
- Source
- Subject
- Timestamp
- Event Type
- Transaction Reference

Evidence SHALL be distinguishable from the Entitlement itself.

---

# 5. Entitlement Creation Binding

An Entitlement MAY be created from a successful transaction.

The logical relationship is:

Transaction

        ↓

Successful Result

        ↓

Evidence

        ↓

Entitlement Creation

        ↓

Entitlement Identifier

The Entitlement SHALL retain a traceable reference to the event or
transaction that established it.

---

# 6. Entitlement Reference

An authorization request SHALL identify the applicable Entitlement
or Entitlement Set.

Representative references include:

- Entitlement Identifier
- Subject Identifier
- Capability
- Resource
- Service
- Scope

The reference SHALL permit the authorization process to retrieve
the authoritative Entitlement state.

---

# 7. Authorization Binding

The authorization process binds the current request to the
applicable Entitlement.

The logical relationship is:

Authorization Request

        +

Authenticated Identity

        +

Entitlement Reference

        +

Policy Context

        ↓

Authorization Evaluation

        ↓

Authorization Decision

The decision SHALL be associated with the request and evaluated
Entitlement state.

---

# 8. Consumption Binding

When an Entitlement is consumable, a successful service action MAY
cause a Consumption Transaction.

The logical relationship is:

Authorization Decision = Allow

        ↓

Service Action

        ↓

Consumption Transaction

        ↓

Entitlement State Update

Consumption SHALL NOT occur before the authorization decision
permits the applicable action.

---

# 9. State Update

The authoritative Entitlement state SHALL be updated after a
successful state-changing operation.

Representative updates include:

- Remaining Quantity
- Usage Count
- Last Used Time
- Consumption State
- Completion State

The state update SHALL reference the transaction that caused the
change.

---

# 10. Transaction Chain

A representative transaction chain is:

Transaction T1

        ↓

Evidence E1

        ↓

Entitlement Q1 Created

        ↓

Transaction T2

        ↓

Authorization Request

        ↓

Authorization Decision

        ↓

Transaction T3

        ↓

Consumption

        ↓

Entitlement Q1 Updated

Each transaction SHALL remain independently identifiable.

---

# 11. Transaction Independence

Transactions SHALL remain logically independent even when they are
causally related.

For example:

- T1 = Entitlement Acquisition
- T2 = Authorization Request
- T3 = Entitlement Consumption

The relationship between T1, T2, and T3 SHALL be represented by
explicit references rather than by treating them as one transaction.

---

# 12. Evidence Chain

Evidence MAY form a traceable chain:

Event

        ↓

Evidence

        ↓

Transaction

        ↓

Entitlement

        ↓

Authorization

        ↓

Consumption

The chain SHOULD support auditability and dispute investigation.

---

# 13. Subject Binding

Transactions and Entitlements SHALL be associated with the relevant
Subject Identifier.

The logical relationship is:

Subject

        ↓

Transaction

        ↓

Evidence

        ↓

Entitlement

        ↓

Authorization

A transaction or entitlement SHALL NOT be attributed to a different
subject without an explicitly authorized transfer operation.

---

# 14. Service Binding

An Entitlement MAY be restricted to a particular Service or
Service Profile.

Representative restrictions include:

- Service Identifier
- Resource Identifier
- Capability
- Scope
- Policy

An Entitlement valid for one Service SHALL NOT automatically be
treated as valid for another Service.

---

# 15. Time Binding

Transaction and Entitlement state SHALL include applicable time
information.

Representative timestamps include:

- Transaction Time
- Evidence Time
- Acquisition Time
- Effective Time
- Expiration Time
- Authorization Time
- Consumption Time

Time relationships SHALL be evaluated according to the applicable
policy.

---

# 16. Version Binding

Where protocol or object versions are used, each relevant object
SHOULD identify its applicable version.

Representative information includes:

- Protocol Version
- Object Version
- Schema Version
- Policy Version

A version mismatch SHALL be handled according to the applicable
compatibility policy.

---

# 17. Idempotency

State-changing transactions SHOULD support idempotent processing.

If the same Consumption Transaction is submitted more than once,
the system SHALL NOT consume the same entitlement quantity more than
once.

The Transaction Identifier SHOULD be used as the idempotency key.

---

# 18. Replay Protection

A transaction or evidence object SHALL NOT be reused to create an
unauthorized additional Entitlement or consumption event.

Replay protection MAY use:

- Unique Transaction Identifier
- Nonce
- Timestamp
- Expiration
- State Check
- Transaction Status

A previously completed transaction SHALL NOT be treated as a new
transaction solely because it is retransmitted.

---

# 19. Atomicity

Where authorization and consumption are logically coupled, the
implementation SHALL prevent inconsistent state.

Representative protected sequence:

Authorization

        ↓

Allow

        ↓

Consumption

        ↓

State Update

The system SHALL prevent a successful consumption from being
recorded without the corresponding authorized operation.

---

# 20. Failure Handling

If the authorization decision is Deny:

Authorization

        ↓

Deny

        ↓

No Consumption

The Entitlement state SHALL remain unchanged unless another
independently authorized state transition is performed.

If the service operation fails after Allow, the system SHALL define
whether consumption is committed, rolled back, or compensated
according to the Service Profile.

---

# 21. Entitlement State Authority

The authoritative Entitlement state SHALL be maintained by the
designated authoritative system component.

Cached or replicated entitlement information MAY be used for
performance purposes only when permitted by the applicable
consistency policy.

A stale entitlement state SHALL NOT cause an unauthorized Allow
decision.

---

# 22. Authorization Lookup

The logical authorization lookup is:

Authenticated Identity

        ↓

Entitlement Reference

        ↓

Authoritative Entitlement Lookup

        ↓

State / Scope / Time / Revocation Check

        ↓

Policy Evaluation

        ↓

Authorization Decision

The lookup SHALL be performed using the applicable current state.

---

# 23. Traceability

The protocol SHOULD provide traceability between:

- Transaction Identifier
- Evidence Identifier
- Entitlement Identifier
- Authorization Request Identifier
- Authorization Decision
- Consumption Transaction

This relationship supports:

- Audit
- Security Investigation
- Dispute Resolution
- State Reconstruction
- Operational Monitoring

---

# 24. Version 2.0 Example

A representative example is:

T1: Visit Transaction

        ↓

E1: Visit Evidence

        ↓

Q1: Discount Entitlement Created

        ↓

T2: EC Authorization Request

        ↓

Policy Evaluation

        ↓

Authorization = Allow

        ↓

T3: Discount Consumption Transaction

        ↓

Q1: Remaining Quantity Updated

The three transactions remain independently identifiable while
remaining causally linked.

---

# 25. Security Properties

The transaction binding model provides:

- Explicit transaction identity
- Evidence traceability
- Entitlement traceability
- Subject binding
- Service binding
- Time binding
- Authorization binding
- Consumption binding
- Idempotency
- Replay protection
- State consistency
- Auditability

---

# 26. Non-Goals

Figure16 does not define:

- A specific database schema
- A specific distributed transaction protocol
- A specific message queue
- A specific API framework
- A specific storage engine
- A specific business policy

Implementation-specific mechanisms MAY be defined elsewhere.

---

# 27. SVG Design Requirements

The SVG representation SHALL include:

- Transaction
- Evidence
- Entitlement
- Authorization Request
- Policy Evaluation
- Authorization Decision
- Service Action
- Consumption Transaction
- Entitlement State Update
- Transaction references
- Traceability relationships
- Version 2.0 example

The causal transaction flow and object-reference relationships
SHALL be visually distinguishable.

All labels and connectors SHALL remain within their containing
visual regions.

---

# 28. Design Freeze Decision

Figure16 is designated as the Entitlement Transaction Binding
reference model for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

