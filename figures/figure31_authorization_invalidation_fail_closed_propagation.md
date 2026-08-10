# Figure31 Authorization Invalidation and Fail-Closed Propagation

## 1. Figure Purpose

Figure31 illustrates the invalidation and fail-closed propagation model of the
NEW-shot2play Protocol Suite Version 2.0.

The figure defines how an authorization state that was previously valid becomes
invalid or restricted and how the resulting security state is propagated to
dependent authorization decisions and protected service execution.

The figure focuses on post-decision security enforcement rather than the initial
authorization evaluation itself.

---

## 2. Core Principle

An authorization decision that becomes invalid SHALL NOT remain effective merely
because a previously issued decision or derived decision exists.

When an authoritative security condition invalidates or restricts an
authorization state, dependent authorization states SHALL be re-evaluated,
restricted, invalidated, or otherwise prevented from producing unauthorized
service execution.

The default enforcement behavior for an unresolved security conflict is
FAIL-CLOSED.

---

## 3. Invalidation Sources

The model identifies the following representative invalidation or restriction
sources:

- Entitlement revocation
- Authorization expiration
- Authentication or assurance invalidation
- Security context invalidation
- Transaction cancellation
- Transaction mismatch
- Policy change
- Resource or action restriction
- Risk or assurance deterioration
- Parent decision invalidation

These conditions MAY arise after an authorization decision has already been
issued.

---

## 4. Authorization State Before Invalidation

A previously evaluated authorization state MAY contain:

- Decision ID
- Parent Decision ID
- Transaction ID
- Subject identity
- Entitlement reference
- Effective scope
- Resource
- Action
- Security context
- Validity period
- Policy reference
- Provenance

The existence of these values does not permanently authorize execution.

Their validity remains subject to authoritative security state.

---

## 5. Invalidation Event

An invalidation event is associated with the affected authorization context.

Representative event information includes:

- Invalidation Event ID
- Decision ID
- Parent Decision ID
- Transaction ID
- Event Type
- Event Time
- Source
- Reason
- Effective Time
- Propagation Status

The invalidation event SHALL be traceable to the affected decision lineage.

---

## 6. Propagation

After an authoritative invalidation or restriction is detected, the system
propagates the resulting security state through the dependent decision graph.

Propagation MAY result in:

1. Immediate invalidation
2. Scope reduction
3. Re-evaluation
4. Reauthorization
5. Temporary suspension
6. Execution blocking

A downstream service SHALL NOT independently restore a broader authorization
state than permitted by the authoritative upstream security state.

---

## 7. Fail-Closed Enforcement

When the receiving service cannot establish that the required authorization
state remains valid, protected execution SHALL be denied.

Representative fail-closed conditions include:

- Missing decision lineage
- Invalid parent decision
- Expired authorization
- Revoked entitlement
- Invalid transaction binding
- Invalid security context
- Unresolved policy conflict
- Unavailable required security state
- Failed integrity verification
- Unknown authorization status

The system SHALL NOT interpret an unknown or unresolved state as authorization.

---

## 8. Downstream Enforcement

The protected service evaluates the effective authorization state immediately
before protected execution.

Possible enforcement results include:

- ALLOW
- REDUCE SCOPE
- RECHECK
- REAUTHORIZE
- DENY
- INVALIDATE
- FAIL CLOSED

Only a verified effective authorization state may permit protected execution.

---

## 9. Recovery

Recovery from an invalidated authorization state SHALL require a new valid
security basis.

Possible recovery paths include:

- Re-authentication
- Re-establishment of security context
- Re-evaluation of entitlement
- Policy re-evaluation
- Reauthorization
- New transaction binding

Recovery SHALL NOT be achieved by simply reusing the invalidated decision.

---

## 10. Security Invariant

The following invariant applies:

> Once an authoritative security condition invalidates or restricts an
> authorization state, no dependent execution path may obtain a broader
> effective authorization state from that invalidated state.

This establishes a fail-closed security boundary between authorization state
and protected service execution.

---

## 11. Figure Semantics

The figure is organized into the following conceptual layers:

1. Previously Valid Authorization
2. Invalidation Source
3. Invalidation Event
4. Decision-Lineage Propagation
5. Downstream State Enforcement
6. Fail-Closed Boundary
7. Protected Service Execution
8. Recovery / Reauthorization

The figure SHALL visually distinguish the normal authorization path from the
invalidation and containment path.

---

## 12. Relationship to Previous Figures

Figure29 describes continuity of an authorization decision state.

Figure30 describes consistency of authorization decisions across distributed
services.

Figure31 extends those models by defining what happens when an authoritative
security condition invalidates or restricts a previously valid authorization
state.

The figure therefore establishes the transition:

Authorization Continuity
→ Distributed Consistency
→ Invalidation Propagation
→ Fail-Closed Enforcement
→ Controlled Recovery

---

## 13. Patent-Relevant Technical Significance

The model demonstrates that authorization is not treated as a static permission
issued once and then trusted indefinitely.

Instead, authorization is represented as a continuously constrained security
state whose validity is dependent upon entitlement, transaction binding,
security context, policy state, and decision lineage.

When a governing condition changes, the system propagates the resulting
restriction or invalidation through dependent authorization states and prevents
protected execution unless a new valid authorization basis is established.

This provides a technical mechanism for maintaining authorization integrity
across distributed services and across the lifetime of an authorization
transaction.
