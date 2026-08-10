# Figure30 — Distributed Authorization Decision Consistency Model

## Purpose

Figure30 defines the consistency model for Authorization Decisions distributed across multiple services.

The model establishes that each service MAY independently evaluate authorization according to its local policy while maintaining a common Decision Lineage and transaction correlation.

Independent local authorization SHALL NOT require identical decisions across services.

However, security-critical state such as revocation, expiration, transaction invalidation, scope reduction, and decision invalidity SHALL propagate consistently so that downstream services do not rely upon a superseded or invalid authorization state.

---

## 1. Core Principle

The distributed authorization model is:

Decision Lineage
→ Cross-Service Correlation
→ Local Policy Evaluation
→ Decision Consistency Verification
→ Conflict Detection
→ Effective Authorization State
→ Service Execution Constraint

Each service MAY produce its own local authorization decision.

The effective result SHALL remain bounded by:

- Common security state
- Transaction state
- Entitlement state
- Decision validity
- Applicable authorization scope
- Local policy
- Resource requirements
- Action requirements

---

## 2. Decision Lineage

A distributed authorization operation SHALL maintain a Decision Lineage.

The lineage MAY include:

- Root Decision ID
- Derived Decision ID
- Request ID
- Transaction ID
- Parent Decision ID
- Service ID
- Subject ID
- Entitlement ID
- Policy ID
- Policy Version
- Resource ID
- Action
- Scope
- Timestamp
- Expiration
- Revocation State
- Decision State

The lineage SHALL permit reconstruction of the complete authorization path.

---

## 3. Root Decision

The Root Decision represents the initial authorization context for a transaction or protected operation.

The Root Decision MAY establish:

- Subject
- Entitlement
- Transaction
- Initial authorization scope
- Security constraints
- Policy reference
- Validity period

A Root Decision SHALL NOT automatically authorize every downstream service.

Downstream services SHALL apply their own authorization rules.

---

## 4. Derived Decision

A downstream service MAY create a Derived Decision based on:

- Verified upstream context
- Parent Decision
- Current transaction
- Local resource
- Requested action
- Local policy
- Local risk
- Local security requirements

A Derived Decision SHALL maintain a reference to its Parent Decision.

A Derived Decision SHALL NOT exceed the authority supported by its parent context.

---

## 5. Local Decision Independence

Each service MAY independently produce:

- ALLOW
- DENY
- CONDITIONAL
- REAUTHORIZE

A local decision MAY differ from another service's decision without constituting a consistency violation.

For example:

Service A → ALLOW
Service B → ALLOW
Service C → DENY

This MAY be valid when Service C applies a stricter local policy or protects a different resource.

Consistency therefore does NOT mean identical authorization results.

---

## 6. Security State Consistency

The following states SHALL remain consistent across dependent services:

- Revoked
- Expired
- Invalid
- Transaction Cancelled
- Session Terminated
- Entitlement Revoked
- Security Context Invalid
- Decision Integrity Failure

A downstream service SHALL NOT treat an authorization as active when an authoritative state indicates that it is no longer valid.

---

## 7. Scope Consistency

Authorization scope SHALL remain bounded across the decision lineage.

A downstream service MAY:

- Preserve scope
- Reduce scope
- Restrict resource
- Restrict action
- Restrict time
- Restrict usage
- Add local conditions

A downstream service SHALL NOT silently expand the scope inherited from upstream authorization.

---

## 8. Transaction Consistency

All decisions participating in a transaction SHALL remain correlated to the same Transaction ID where transaction binding is required.

A decision associated with another transaction SHALL NOT be reused.

If transaction state becomes invalid, dependent decisions SHALL be re-evaluated or invalidated according to policy.

---

## 9. Policy Version Consistency

A decision SHALL identify the Policy Version used for evaluation.

A downstream service MAY use a newer or stricter local policy.

A downstream service SHALL NOT represent a decision as having been evaluated under a policy version that was not actually used.

Policy changes MAY trigger:

- Re-evaluation
- Scope reduction
- Decision invalidation
- Reauthorization

---

## 10. Conflict Detection

The system SHOULD detect security-relevant conflicts such as:

- Active decision vs revoked decision
- Valid decision vs expired decision
- Matching transaction vs mismatched transaction
- Valid scope vs expanded scope
- Trusted context vs invalid context
- Parent ALLOW vs invalidated parent
- Completed transaction vs new use of consumed decision

Conflict detection SHALL prioritize authoritative security state.

---

## 11. Conflict Resolution

When a security-critical conflict is detected, the receiving service SHALL NOT silently select the more permissive state.

Possible outcomes include:

- DENY
- REAUTHORIZE
- RECHECK
- INVALIDATE
- REDUCE SCOPE
- FAIL CLOSED

The resolution SHALL be determined by applicable policy.

---

## 12. Effective Authorization State

The Effective Authorization State represents the state that may actually be relied upon by the protected service.

It SHALL be derived from:

- Verified decision lineage
- Current security context
- Current entitlement state
- Transaction state
- Revocation state
- Expiration state
- Effective scope
- Local policy
- Resource requirements
- Action requirements

The effective state SHALL NOT be more permissive than the applicable constraints.

---

## 13. Service Execution Constraint

Service execution SHALL be constrained by the Effective Authorization State.

Execution MAY occur only when:

- Required context is valid
- Required entitlement is valid
- Decision is valid
- Transaction is valid
- Scope permits the requested operation
- Local policy permits the operation
- Required security checks succeed

Execution SHALL be denied or interrupted when a required condition becomes invalid.

---

## 14. Decision Lineage Across Services

A distributed transaction MAY follow:

Root Decision
→ Service A Decision
→ Service B Decision
→ Service C Decision
→ Service D Decision

Each downstream decision MAY:

- Preserve authority
- Reduce authority
- Require reauthorization
- Deny the operation

Each decision SHALL remain traceable to its parent.

---

## 15. Authorization Convergence

Authorization convergence does NOT mean that every service eventually produces ALLOW.

Instead, convergence means that all services participating in a common authorization lineage agree on the authoritative security constraints.

For example:

Shared State:
- Transaction = T123
- Entitlement = E456
- Revocation = false
- Expiration = valid

Local Decisions:
- Service A = ALLOW
- Service B = ALLOW
- Service C = DENY

This is consistent if Service C's local policy denies the requested operation.

---

## 16. Revocation Dominance

Revocation SHALL have dominance over previously granted authorization where required by policy.

If:

Authorized
→ Revoked

then downstream services SHALL NOT continue treating the decision as Authorized.

Revocation state SHALL be checked or propagated within the permitted revocation latency.

---

## 17. Expiration Dominance

Expiration SHALL have dominance over previously valid authorization.

If:

Authorized
→ Expired

then downstream services SHALL NOT continue relying upon the decision.

A new authorization MAY be required.

---

## 18. Transaction Cancellation

If a transaction is cancelled:

Transaction Active
→ Transaction Cancelled

dependent authorization decisions MAY become invalid.

The receiving service SHALL apply the transaction cancellation policy before executing protected operations.

---

## 19. Parent Decision Invalidity

If a Parent Decision becomes invalid, dependent Derived Decisions SHALL be:

- Revalidated
- Reduced
- Invalidated
- Reauthorized

according to policy.

A Derived Decision SHALL NOT remain valid solely because it was previously issued.

---

## 20. Distributed Failure Handling

Distributed failures MAY prevent immediate synchronization of decision state.

Examples include:

- Network partition
- Service outage
- Revocation service unavailable
- Decision store unavailable
- Context verification unavailable

When current security state cannot be verified, the system SHALL apply the configured fail-open or fail-closed policy.

For high-risk protected operations, fail-closed SHOULD be used.

---

## 21. Audit and Lineage Traceability

Each decision event SHOULD record:

- Decision ID
- Parent Decision ID
- Root Decision ID
- Transaction ID
- Service ID
- Subject ID
- Policy Version
- Previous State
- New State
- Conflict Result
- Effective Scope
- Authorization Result
- Execution Result
- Timestamp
- Correlation ID

This permits reconstruction of the distributed decision lineage.

---

## 22. Security Invariants

The following invariants apply:

1. Every derived decision SHALL reference its parent decision.
2. Decision lineage SHALL remain traceable.
3. Local decisions MAY differ without violating consistency.
4. Revocation SHALL dominate prior authorization where required.
5. Expiration SHALL dominate prior authorization.
6. Transaction invalidation SHALL constrain dependent decisions.
7. Authorization scope SHALL NOT expand downstream.
8. Security-critical state SHALL remain consistent across dependent services.
9. Conflict resolution SHALL NOT silently choose the more permissive state.
10. Effective authorization SHALL be bounded by all applicable constraints.
11. Service execution SHALL require valid effective authorization.
12. Audit records SHALL permit reconstruction of the distributed decision path.

---

## 23. Relationship to Previous Figures

Figure27 defines distributed authorization context reconstruction.

Figure28 defines context continuity across service boundaries.

Figure29 defines the lifecycle continuity of an individual Authorization Decision.

Figure30 extends the model from a single decision lifecycle to distributed decision consistency.

The progression is:

Distributed Context
→ Context Reconstruction
→ Context Continuity
→ Authorization Decision
→ Decision State Continuity
→ Decision Lineage
→ Cross-Service Consistency
→ Effective Authorization
→ Service Execution

Figure30 therefore establishes the distributed consistency layer required when authorization decisions participate in a multi-service transaction.
