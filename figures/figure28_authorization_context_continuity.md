# Figure28 — Authorization Context Continuity Model

## Purpose

Figure28 defines the continuity model for an authorization context across multiple services.

The model establishes that an authorization context may be propagated across service boundaries while its provenance, scope, validity, transaction binding, and security constraints are preserved.

Propagation of context SHALL NOT by itself create, expand, or transfer authorization authority.

Each receiving service SHALL independently determine whether the propagated context is sufficient for local policy evaluation and authorization.

---

## 1. Core Principle

The authorization context follows the service path:

Authentication / Entitlement / Policy Context
→ Context Assembly
→ Service Boundary
→ Context Verification
→ Context Continuity
→ Local Policy Evaluation
→ Local Authorization Decision
→ Service Execution

The context may continue across multiple services.

The authorization authority does not automatically continue merely because the context continues.

---

## 2. Context Continuity

A propagated authorization context SHALL preserve, where applicable:

- Subject identity
- Authentication assurance
- Session binding
- Device binding
- Entitlement identity
- Entitlement state
- Entitlement scope
- Policy identity
- Policy version
- Transaction identity
- Resource identity
- Requested action
- Delegation state
- Risk / trust information
- Temporal constraints
- Consumption state
- Provenance
- Integrity information

The receiving service SHALL verify the applicable fields before relying upon them.

---

## 3. Service Boundary

A service boundary represents a security-relevant transition.

The originating service SHALL provide sufficient provenance and integrity information for the receiving service to determine whether the context can be trusted.

The receiving service SHALL NOT assume that a context is valid solely because it originated from another service.

Trust relationships SHALL be explicit.

---

## 4. Context Continuity vs Authority Continuity

Context continuity and authority continuity are separate concepts.

### Context Continuity

Context continuity means that security-relevant information remains semantically connected across service boundaries.

### Authority Continuity

Authority continuity means that the receiving service accepts the authority represented by the propagated context.

Authority continuity SHALL NOT be inferred solely from context continuity.

The receiving service SHALL apply its own policy and authorization rules.

---

## 5. Context Transformation

A receiving service MAY transform a canonical context into a service-specific representation.

Such transformation SHALL preserve semantic meaning.

Transformation SHALL NOT:

- Increase entitlement scope
- Increase authorization scope
- Extend expiration
- Remove mandatory security constraints
- Change the subject
- Change the transaction binding
- Convert untrusted information into trusted information
- Create authority not supported by the originating context

---

## 6. Local Policy Evaluation

Each receiving service SHALL evaluate the propagated context against its local policy.

Local evaluation MAY consider:

- Service identity
- Resource sensitivity
- Requested action
- Current transaction state
- Current entitlement state
- Current risk state
- Current session state
- Local security requirements
- Local regulatory requirements
- Context freshness

A previously granted decision SHALL NOT automatically constitute a local authorization decision unless the receiving service explicitly accepts such behavior under policy.

---

## 7. Reauthorization

The receiving service SHALL require reauthorization when:

- The propagated context is insufficient
- Required context is missing
- Context has expired
- Context has been revoked
- Context is outside the requested scope
- Transaction binding is invalid
- Resource sensitivity requires stronger assurance
- Local policy requires independent authorization
- Trust relationship is absent or invalid
- Context integrity cannot be verified

Reauthorization MAY require:

- Reauthentication
- Reentitlement
- Re-evaluation
- Additional policy conditions
- Additional authorization proof

---

## 8. Multi-Service Continuity Model

A multi-service path may be represented as:

Service A
→ Service Boundary
→ Service B
→ Service Boundary
→ Service C
→ Service Boundary
→ Service D

At each boundary:

1. Context is propagated.
2. Receiving service verifies provenance and integrity.
3. Context is correlated with the local request.
4. Context is transformed if necessary.
5. Local policy is evaluated.
6. Local authorization is determined.
7. Service execution occurs only when permitted.
8. Resulting security state may be propagated subject to policy.

---

## 9. Authority Non-Escalation

The following invariant SHALL hold:

> Context propagation SHALL NOT result in authority escalation.

The effective authorization scope at a downstream service SHALL be no greater than the authority supported by:

- The originating context
- The receiving service's local policy
- The current resource and action
- The current transaction state
- Applicable security constraints

A downstream service MAY reduce authority.

A downstream service SHALL NOT silently increase authority.

---

## 10. Context Freshness

Context continuity SHALL account for temporal validity.

A receiving service MAY require:

- Maximum context age
- Recent authentication
- Recent entitlement verification
- Recent risk evaluation
- Recent transaction verification

A context that was valid at Service A MAY become invalid at Service B because of elapsed time or changed state.

---

## 11. Revocation Propagation

Revocation information SHALL be propagated or independently retrieved when required.

If an entitlement, session, delegation, or authorization state is revoked, downstream services SHALL NOT continue relying upon the revoked state beyond the permitted revocation latency.

Where immediate revocation is required, the receiving service SHALL perform an authoritative status check.

---

## 12. Failure Handling

If context continuity cannot be established safely, the receiving service SHALL fail closed for protected operations unless an explicitly defined recovery policy permits another outcome.

Possible outcomes include:

- DENY
- REAUTHENTICATE
- REENTITLE
- REAUTHORIZE
- RECOVER
- RETRY
- ESCALATE

---

## 13. Audit and Traceability

Each service boundary SHOULD record:

- Request ID
- Subject ID
- Transaction ID
- Context ID
- Issuer
- Receiving service
- Context version
- Policy version
- Verification result
- Local authorization result
- Transformation result
- Revocation status
- Timestamp
- Correlation identifier

This permits reconstruction of the authorization path across services.

---

## 14. Security Invariants

The following invariants apply:

1. Context continuity SHALL preserve provenance.
2. Context continuity SHALL preserve integrity.
3. Context transformation SHALL preserve semantic meaning.
4. Context propagation SHALL NOT create authority.
5. Downstream authorization SHALL be locally evaluated unless explicitly delegated.
6. Context expiration SHALL remain effective across service boundaries.
7. Revocation SHALL remain effective across service boundaries.
8. Scope SHALL NOT expand through propagation.
9. Transaction binding SHALL remain enforceable.
10. Auditability SHALL extend across the service path.

---

## 15. Relationship to Figure27

Figure27 defines distributed authorization context reconstruction.

Figure28 extends that model by defining how the reconstructed context remains semantically continuous across multiple service boundaries while authorization authority remains independently controlled.

Figure27 therefore establishes:

Distributed Context
→ Verification
→ Normalization
→ Correlation
→ Reconstruction

Figure28 establishes:

Reconstructed Context
→ Service Boundary
→ Verification
→ Context Continuity
→ Local Policy Evaluation
→ Local Authorization

Together, Figure27 and Figure28 define the transition from distributed context reconstruction to multi-service authorization continuity.
