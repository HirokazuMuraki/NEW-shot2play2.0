# Figure34 Authorization Decision State Lifecycle

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure34 |
| Title | Authorization Decision State Lifecycle |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of the lifecycle and state transitions of an Authorization Decision from evaluation through enforcement and completion |

---

# 1. Purpose

Figure34 defines the lifecycle of an Authorization Decision in the
NEW-shot2play Protocol Suite Version 2.0.

The model describes how an Authorization Decision progresses from
creation through validation, enforcement, service execution, and
completion.

The Authorization Decision lifecycle SHALL remain distinguishable
from the Authentication lifecycle and from the Entitlement
lifecycle.

---

# 2. Fundamental Lifecycle

The fundamental Authorization Decision lifecycle is:

Decision Request

        ↓

Evaluation

        ↓

Decision Issued

        ↓

Decision Valid

        ↓

Enforcement

        ↓

Service Execution

        ↓

Completed

A decision that cannot satisfy the required conditions SHALL NOT
progress to protected service execution.

---

# 3. Decision Request State

The Decision Request state represents the initial request for an
authorization decision.

Representative information includes:

- Authorization Request Identifier
- Subject Identifier
- Action
- Resource
- Service Identifier
- Entitlement Reference
- Transaction Reference
- Authorization Context
- Request Timestamp

The request SHALL establish the operation for which authorization is
required.

---

# 4. Evaluation State

The Evaluation state represents the processing of the authorization
inputs.

The evaluation MAY include:

- Authenticated Identity
- Authorization Request
- Entitlement State
- Policy
- Authorization Context
- Transaction State
- Security State
- Service Context

The evaluation SHALL determine whether the required authorization
conditions are satisfied.

---

# 5. Decision Issued State

The Decision Issued state represents the creation of an
Authorization Decision.

Representative attributes include:

- Decision Identifier
- Decision Result
- Subject Identifier
- Action
- Resource
- Service Identifier
- Policy Reference
- Entitlement Reference
- Authorization Context Reference
- Decision Timestamp
- Validity Information
- Transaction Reference

The issued decision SHALL remain traceable to the evaluation that
produced it.

---

# 6. Decision Valid State

A Decision Issued with an Allow result SHALL enter the Decision Valid
state only when the applicable validity conditions are satisfied.

Representative conditions include:

- Decision exists
- Decision result is Allow
- Subject binding is valid
- Action binding is valid
- Resource binding is valid
- Service binding is valid
- Required Entitlement is valid
- Required Policy is valid
- Required context is valid
- Decision validity period is active
- Required transaction relationship is valid

The Decision Valid state represents an authorization decision that is
currently eligible for enforcement.

---

# 7. Enforcement State

The Enforcement state represents verification of the Authorization
Decision at the protected service boundary.

The enforcement process SHALL verify the applicable decision binding
conditions.

Representative checks include:

- Decision identity
- Request relationship
- Subject relationship
- Action relationship
- Resource relationship
- Service relationship
- Entitlement validity
- Policy validity
- Decision freshness
- Required transaction relationship

Successful enforcement verification permits progression toward
protected service execution.

---

# 8. Service Execution State

The Service Execution state represents execution of the protected
operation following successful authorization enforcement.

Examples include:

- Accessing protected data
- Executing a transaction
- Redeeming an entitlement
- Performing a purchase
- Accessing a restricted function
- Performing a privileged operation

The service SHALL execute only when the applicable Authorization
Decision has passed enforcement verification.

---

# 9. Completed State

The Completed state represents successful completion of the protected
service operation.

The completion event MAY produce:

- Service Result
- Transaction Result
- Consumption Event
- Entitlement State Update
- Audit Record
- Security Event
- Downstream Event

Where an Entitlement is consumable, the resulting state update SHALL
be associated with the applicable Entitlement Identifier.

---

# 10. Deny State

An authorization evaluation MAY produce a Deny result.

A Deny result SHALL terminate the protected execution path.

The Deny state SHALL NOT transition to Protected Service Execution
without a new authorization process establishing a valid Allow
decision.

---

# 11. Invalid State

An Authorization Decision MAY become invalid after issuance.

Invalidity MAY result from:

- Decision expiration
- Entitlement revocation
- Entitlement expiration
- Policy invalidation
- Subject mismatch
- Action mismatch
- Resource mismatch
- Service mismatch
- Transaction mismatch
- Security state change
- Authorization context change

An invalid decision SHALL NOT authorize protected execution.

---

# 12. Revoked State

Where the authorization basis or decision is explicitly revocable,
the decision MAY enter a Revoked state.

A revoked decision SHALL be treated as invalid for protected
execution.

Revocation SHALL take precedence over a previously issued Allow
result.

---

# 13. Revalidation

A decision MAY require revalidation before enforcement or execution.

Revalidation MAY be triggered by:

- Decision age
- Context change
- Security state change
- Entitlement state change
- Policy change
- Service requirement
- Risk change
- Distributed state synchronization

A decision that fails revalidation SHALL NOT proceed to protected
execution.

---

# 14. Recovery

Recovery from an invalid, expired, or revoked decision SHALL require
a new valid authorization basis.

Recovery MAY include:

- New Authorization Request
- New Policy Evaluation
- Updated Entitlement
- Updated Authorization Context
- New Authorization Decision
- New Enforcement Verification

Recovery SHALL NOT be achieved merely by reusing an invalid
Authorization Decision.

---

# 15. Decision State Independence

The Authorization Decision state SHALL remain logically distinct
from:

- Authentication State
- Entitlement State
- Policy State
- Service Execution State
- Transaction State

Changes in any of these states MAY cause the Authorization Decision
to become invalid or require revalidation.

The decision state therefore represents the authorization result at a
specific point in the applicable security context.

---

# 16. State Transition Rules

The principal transitions are:

Decision Request → Evaluation

Evaluation → Decision Issued

Decision Issued → Decision Valid

Decision Issued → Deny

Decision Valid → Enforcement

Decision Valid → Invalid

Decision Valid → Revoked

Enforcement → Service Execution

Enforcement → Deny

Service Execution → Completed

Invalid → New Authorization Request

Revoked → New Authorization Request

Deny → New Authorization Request

A transition to protected execution SHALL require a valid Allow
decision and successful enforcement verification.

---

# 17. No Implicit Allow Transition

The state model SHALL NOT permit an implicit transition from:

- Decision Request → Service Execution
- Evaluation → Service Execution
- Decision Issued → Service Execution
- Invalid → Service Execution
- Revoked → Service Execution
- Deny → Service Execution

Protected execution SHALL require the explicit authorization and
enforcement stages.

---

# 18. Decision Freshness

A Decision Valid state MAY be limited by a freshness requirement.

Freshness MAY be expressed through:

- Decision Timestamp
- Expiration Time
- Maximum Decision Age
- Policy-defined validity period
- Service-defined validity period

When freshness is required, an expired or stale decision SHALL NOT
progress to enforcement.

---

# 19. Distributed Decision State

In a distributed deployment, the Authorization Decision state MAY be
observed by multiple services or authorization components.

The authoritative decision state SHALL remain distinguishable from
local cached or propagated representations.

A distributed component SHALL NOT treat an outdated local
representation as authoritative when current state verification is
required.

State inconsistency SHALL result in revalidation, Deny, or
Fail-Closed behavior according to the applicable policy.

---

# 20. Relationship to Earlier Figures

Figure34 is complementary to:

- Figure29 — Authorization Decision State Continuity
- Figure30 — Distributed Authorization Decision Consistency
- Figure31 — Authorization Invalidation, Fail-Closed Enforcement and Controlled Recovery
- Figure32 — Authorization Decision Dependency and Precedence Model
- Figure33 — Authorization Decision Binding to Service Execution

Figure34 focuses specifically on the lifecycle and state transitions
of an Authorization Decision.

---

# 21. Security Invariant

The fundamental security invariant represented by Figure34 is:

"Only a currently valid Authorization Decision that has passed the
required enforcement verification SHALL permit protected service
execution."

An invalid, expired, revoked, stale, inconsistent, or unverifiable
decision SHALL NOT authorize protected execution.

---

# 22. Design Principle

Figure34 establishes the following design principle:

**Authorization Decision is stateful.**

**Decision validity is conditional.**

**Enforcement is a required transition.**

**Protected execution occurs only from a valid and enforceable
decision state.**

A previous Allow result SHALL NOT create a permanent authorization
state.

---

# 23. Summary

Figure34 defines the lifecycle of an Authorization Decision from
request through completion.

The model establishes that:

1. Authorization begins with an Authorization Request.
2. Authorization Evaluation produces an Authorization Decision.
3. An Allow decision becomes enforceable only when applicable
   validity conditions are satisfied.
4. Enforcement Verification is a distinct state transition.
5. Protected Service Execution occurs only after successful
   enforcement.
6. Successful execution may produce completion and state updates.
7. Deny terminates the protected execution path.
8. Expired, revoked, stale, inconsistent, or otherwise invalid
   decisions SHALL NOT authorize execution.
9. Revalidation MAY be required when security or contextual state
   changes.
10. Recovery from an invalid decision requires a new valid
    authorization basis.
11. Distributed implementations SHALL distinguish authoritative state
    from stale local representations.
12. The lifecycle SHALL preserve the security invariant that only a
    valid and enforceable decision can authorize protected execution.

