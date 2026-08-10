# Figure29 — Authorization Decision State Continuity Model

## Purpose

Figure29 defines the lifecycle and continuity model of an Authorization Decision after policy evaluation.

The model establishes that an Authorization Decision is not merely a point-in-time ALLOW or DENY result.

An authorization decision MAY transition through multiple controlled states from request through evaluation, enforcement, consumption, completion, expiration, revocation, or failure.

Each state transition SHALL remain bound to the applicable subject, entitlement, policy, transaction, resource, action, authorization scope, and security context.

---

## 1. Core Principle

Authorization Decision lifecycle:

Requested
→ Evaluating
→ Authorized
→ Enforced
→ Consumed
→ Completed

Alternative terminal or exceptional paths include:

Authorized
→ Revoked

Authorized
→ Expired

Evaluating
→ Denied

Enforced
→ Failed

Consumed
→ Failed

A downstream service SHALL NOT assume that an earlier authorization state remains valid without verifying the current state and applicable constraints.

---

## 2. Decision Identity

Each Authorization Decision SHOULD have a unique Decision ID.

The Decision ID MAY be associated with:

- Request ID
- Subject ID
- Session ID
- Entitlement ID
- Policy ID
- Policy Version
- Transaction ID
- Resource ID
- Action
- Authorization Scope
- Issuer
- Decision Timestamp
- Expiration
- Revocation State
- Consumption State
- Provenance
- Integrity information

The Decision ID SHALL permit correlation of the decision across its lifecycle.

---

## 3. Requested State

The Requested state represents the initial request for protected service execution.

The request SHALL identify, where applicable:

- Subject
- Requested action
- Target resource
- Transaction
- Security context
- Entitlement context
- Policy context

No authorization authority SHALL be implied solely by the existence of a request.

---

## 4. Evaluating State

The Evaluating state represents active Policy Evaluation and Authorization processing.

The authorization engine MAY evaluate:

- Authentication assurance
- Entitlement state
- Policy conditions
- Resource sensitivity
- Requested action
- Transaction state
- Risk
- Device state
- Session state
- Context freshness
- Revocation state
- Service-specific constraints

The Evaluating state SHALL NOT be treated as authorization.

---

## 5. Authorized State

The Authorized state represents a successful authorization decision.

The decision SHALL identify the effective authorization scope.

The effective scope SHALL include applicable constraints such as:

- Permitted resource
- Permitted action
- Permitted transaction
- Time validity
- Usage limits
- Consumption conditions
- Service restrictions
- Risk restrictions

Authorization SHALL NOT exceed the authority supported by the evaluated context and policy.

---

## 6. Enforced State

The Enforced state represents the point at which the authorized decision is actively enforced by the protected service.

Enforcement MAY include:

- Access control
- Feature activation
- Transaction approval
- Resource release
- API execution
- Data access
- Physical or digital service activation

The service SHALL verify that the decision remains applicable before enforcement where required by policy.

---

## 7. Consumed State

The Consumed state represents actual use of the authorization.

Consumption MAY include:

- One-time use
- Partial use
- Quota consumption
- Benefit consumption
- Transaction completion
- Resource utilization

Consumption state SHALL be distinguishable from authorization state.

A decision MAY remain authorized while only a portion of its permitted scope has been consumed.

---

## 8. Completed State

The Completed state represents successful completion of the authorized operation.

The completion record SHOULD include:

- Decision ID
- Transaction ID
- Resource
- Action
- Consumption result
- Completion timestamp
- Service result
- Audit correlation

Completion SHALL NOT imply that the same authorization remains valid for a subsequent independent operation.

---

## 9. Revoked State

A previously authorized decision MAY be revoked.

Revocation MAY occur because of:

- Entitlement revocation
- Session termination
- Security incident
- Policy change
- Administrative action
- Risk escalation
- Transaction cancellation

After revocation, protected operations SHALL NOT continue relying on the revoked decision beyond the permitted revocation latency.

---

## 10. Expired State

Authorization SHALL remain subject to temporal validity.

A decision MAY expire because:

- Authorization lifetime elapsed
- Entitlement lifetime elapsed
- Session lifetime elapsed
- Transaction validity elapsed
- Context freshness requirement failed

Expired authorization SHALL NOT be treated as active authorization.

---

## 11. Denied State

The Denied state represents a failed authorization evaluation.

Denial MAY result from:

- Missing entitlement
- Policy failure
- Insufficient assurance
- Invalid scope
- Invalid transaction
- Revoked context
- Expired context
- Risk threshold failure
- Resource restriction

A Denied decision SHALL NOT be converted into Authorized without a new valid evaluation.

---

## 12. Failed State

The Failed state represents an operational or security failure during enforcement or consumption.

Examples include:

- Context verification failure
- Decision integrity failure
- Transaction mismatch
- Service execution failure
- Consumption failure
- Communication failure
- Revocation check failure

Protected operations SHALL fail closed when required security conditions cannot be verified.

---

## 13. State Transition Rules

Each transition SHALL be controlled.

Permitted primary transitions:

Requested
→ Evaluating

Evaluating
→ Authorized

Evaluating
→ Denied

Authorized
→ Enforced

Authorized
→ Revoked

Authorized
→ Expired

Enforced
→ Consumed

Enforced
→ Failed

Consumed
→ Completed

Consumed
→ Failed

Additional recovery transitions MAY be defined by policy.

A state transition SHALL NOT silently bypass required authorization controls.

---

## 14. Decision Continuity Across Services

When an authorization decision crosses a service boundary, the receiving service SHALL verify:

- Decision ID
- Decision provenance
- Decision integrity
- Current decision state
- Effective authorization scope
- Expiration
- Revocation
- Transaction binding
- Resource binding
- Action binding
- Applicable local policy

The receiving service MAY accept the decision, reduce its effective scope, require reauthorization, or deny the operation.

The receiving service SHALL NOT silently expand the authorization scope.

---

## 15. Decision State vs Context State

Decision state and security context state SHALL remain distinguishable.

Security context answers:

> Who is acting, under what security conditions, and with what entitlement and policy context?

Authorization Decision answers:

> What operation is currently permitted under those conditions?

The two states SHALL remain correlated but SHALL NOT be treated as identical.

A valid context does not automatically imply an active authorization decision.

An active authorization decision does not eliminate the need to verify required context conditions.

---

## 16. Consumption and One-Time Authorization

For one-time authorization, the lifecycle MAY be:

Requested
→ Evaluating
→ Authorized
→ Enforced
→ Consumed
→ Completed

After consumption, the authorization SHALL NOT be reusable unless the applicable policy explicitly permits reuse.

The Consumption State SHOULD include:

- Consumption ID
- Decision ID
- Consumption count
- Remaining allowance
- Consumption timestamp
- Consuming service
- Transaction correlation

---

## 17. State Monotonicity and Non-Regression

Security-sensitive state SHALL NOT regress silently.

Examples:

- Completed SHALL NOT return to Authorized.
- Consumed SHALL NOT become Unconsumed.
- Revoked SHALL NOT silently become Authorized.
- Expired SHALL NOT silently become Active.
- Denied SHALL NOT silently become Authorized.

Any transition that effectively restores authorization SHALL require a new valid authorization evaluation unless explicitly defined as a controlled recovery operation.

---

## 18. Audit Traceability

Every significant state transition SHOULD generate an audit event.

Audit information SHOULD include:

- Decision ID
- Previous state
- New state
- Transition reason
- Actor
- Service
- Policy Version
- Transaction ID
- Timestamp
- Correlation ID
- Security context reference

This permits reconstruction of the complete Authorization Decision lifecycle.

---

## 19. Security Invariants

The following invariants apply:

1. Every authorization decision SHALL be uniquely identifiable.
2. Authorization SHALL be bounded by effective scope.
3. Authorization state SHALL remain temporally valid.
4. Revocation SHALL invalidate protected use.
5. Expiration SHALL invalidate protected use.
6. Consumption SHALL be separately tracked.
7. State transitions SHALL be auditable.
8. State transitions SHALL NOT silently escalate authority.
9. Downstream services SHALL independently verify applicable decision state.
10. Context validity SHALL NOT automatically imply decision validity.
11. Decision validity SHALL NOT eliminate required context verification.
12. Security-sensitive state SHALL NOT silently regress.

---

## 20. Relationship to Previous Figures

Figure27 defines distributed authorization context reconstruction.

Figure28 defines authorization context continuity across service boundaries.

Figure29 extends the model from context continuity to Authorization Decision continuity.

The conceptual progression is:

Distributed Context
→ Context Reconstruction
→ Context Continuity
→ Policy Evaluation
→ Authorization Decision
→ Decision State Continuity
→ Enforcement
→ Consumption
→ Completion

Figure29 therefore establishes the lifecycle-level continuity of authorization after the context has been reconstructed and propagated.
