# Figure32 Authorization Decision Dependency and Precedence Model

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure32 |
| Title | Authorization Decision Dependency and Precedence Model |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of the dependency relationships and fail-closed precedence governing an Authorization Decision |

---

# 1. Purpose

Figure32 defines the dependency and precedence relationships governing
the generation of an Authorization Decision in the NEW-shot2play
Protocol Suite Version 2.0.

The model establishes that an Authorization Decision is not derived
from Authentication alone.

An Allow decision SHALL depend on the validity of the applicable:

- Authenticated Identity
- Authorization Request
- Entitlement State
- Policy State
- Authorization Context

The model further establishes a fail-closed precedence rule.

If an applicable authorization input is invalid, unavailable,
expired, revoked, inconsistent, or otherwise fails the required
validation condition, the corresponding authorization evaluation
SHALL NOT produce an Allow decision.

---

# 2. Fundamental Dependency Model

The fundamental dependency relationship is:

Authenticated Identity
        +
Authorization Request
        +
Entitlement State
        +
Policy State
        +
Authorization Context
        ↓
Authorization Decision Evaluation
        ↓
Authorization Decision

The Authorization Decision SHALL be derived from the applicable
current state of the authorization inputs.

---

# 3. Authentication Dependency

Authentication establishes the identity context used by the
authorization process.

Representative authentication outputs include:

- Subject Identifier
- Authentication Result
- Authentication Time
- Authentication Method
- Authentication Assurance Information
- Authentication Transaction Reference

A successful authentication SHALL establish an authenticated
identity but SHALL NOT by itself establish authorization.

If the authenticated identity required by the authorization request
cannot be established or validated, the authorization evaluation
SHALL NOT produce an Allow decision.

---

# 4. Authorization Request Dependency

The Authorization Request identifies the requested operation.

Representative attributes include:

- Authorization Request Identifier
- Subject Identifier
- Action
- Resource
- Service
- Timestamp
- Entitlement Reference
- Transaction Reference

The requested action SHALL be evaluated against the applicable
authorization conditions.

An incomplete, malformed, expired, or otherwise invalid request
SHALL NOT produce an Allow decision.

---

# 5. Entitlement State Dependency

The authorization process SHALL evaluate the authoritative current
state of the applicable Entitlement.

Representative validation conditions include:

- Entitlement exists
- Subject matches
- Capability matches
- Resource matches
- Service matches
- Scope matches
- Effective time is valid
- Expiration time has not passed
- Entitlement is not revoked
- Remaining quantity is sufficient
- Entitlement state permits the requested action

An invalid or unavailable Entitlement SHALL NOT satisfy an
authorization condition requiring an active Entitlement.

Accordingly, an invalid Entitlement SHALL prevent an Allow decision
where that Entitlement is a required authorization basis.

---

# 6. Policy State Dependency

Policy defines the conditions under which an operation may be
authorized.

Representative policy conditions include:

- Subject
- Action
- Resource
- Service
- Scope
- Time
- Location
- Device
- Risk
- Entitlement State
- Transaction State

The applicable Policy SHALL be identified and evaluated using its
authoritative current version or state.

An unavailable, invalid, or otherwise non-applicable Policy SHALL NOT
be treated as satisfying an authorization condition.

Where Policy validity is required for an Allow decision, Policy
failure SHALL result in Deny or another explicitly fail-closed
authorization outcome.

---

# 7. Authorization Context Dependency

Authorization Context contains contextual information required by
the applicable Policy.

Representative context includes:

- Current Time
- Service Context
- Device Context
- Session Context
- Transaction Context
- Risk Context
- External Event Context

Context SHALL be evaluated according to the applicable Policy.

If a required context value cannot be established or validated, the
authorization process SHALL NOT assume that the missing condition
has been satisfied.

Where the required context is unavailable or invalid, the decision
SHALL fail closed.

---

# 8. Decision Evaluation

Authorization Decision Evaluation combines the required
authorization inputs.

The conceptual model is:

Authenticated Identity
        +
Authorization Request
        +
Entitlement State
        +
Policy State
        +
Authorization Context
        ↓
Authorization Decision Evaluation
        ↓
Authorization Decision

The evaluation SHALL operate on the applicable authoritative
security state.

The evaluation SHALL NOT substitute an outdated, revoked,
inconsistent, or otherwise invalid state for the required
authorization input.

---

# 9. Decision Precedence

The Authorization Decision SHALL follow a fail-closed precedence
model.

The conceptual precedence is:

Required Input Valid
        ↓
Evaluation Permitted
        ↓
Allow may be considered

Conversely:

Required Input Invalid
        ↓
Evaluation Cannot Establish Required Basis
        ↓
Allow is Not Permitted
        ↓
Deny / Fail-Closed

The fail-closed rule applies independently to each required
authorization input.

---

# 10. Fail-Closed Conditions

The following conditions MAY prevent an Allow decision when the
corresponding input is required:

- Authentication invalid
- Authorization Request invalid
- Entitlement invalid
- Entitlement revoked
- Entitlement expired
- Policy invalid
- Policy unavailable
- Required Context unavailable
- Required Context invalid
- Authoritative State inconsistent
- Required security state cannot be established

The authorization implementation SHALL NOT convert such a failure
into an Allow result by default.

---

# 11. Allow Decision

An Allow decision MAY be produced only when all required
authorization conditions have been successfully established.

The conceptual relationship is:

All Required Conditions Valid
        ↓
Authorization Evaluation
        ↓
ALLOW
        ↓
Enforcement

An Allow decision SHALL therefore represent a decision based on the
applicable authenticated identity, request, entitlement, policy, and
contextual state.

---

# 12. Deny Decision

A Deny decision SHALL be produced when the applicable authorization
conditions are not satisfied.

Representative causes include:

- Identity mismatch
- Request mismatch
- Entitlement mismatch
- Entitlement expiration
- Entitlement revocation
- Policy violation
- Context violation
- Security state inconsistency
- Required state unavailable

The Deny decision SHALL prevent the corresponding protected service
operation from being authorized.

---

# 13. Relationship to Enforcement

The Authorization Decision SHALL precede protected service
execution.

The conceptual relationship is:

Authorization Decision
        ↓
Enforcement
        ↓
Protected Service Execution

An Allow decision SHALL be enforceable at the protected service
boundary.

A Deny or Fail-Closed result SHALL prevent the protected operation
from proceeding.

---

# 14. Relationship to Distributed Consistency

In a distributed deployment, the authorization inputs MAY be
assembled or reconstructed across multiple services.

The Authorization Decision SHALL nevertheless depend on the
authoritative security state applicable to the decision.

A distributed implementation SHALL NOT treat a stale or
inconsistent local state as equivalent to the authoritative state
when such equivalence would permit an unauthorized Allow decision.

This relationship is complementary to the distributed consistency
model defined by Figure30.

---

# 15. Relationship to Invalidation and Recovery

When an authorization basis becomes invalid after an earlier
decision, the resulting invalidation SHALL be capable of propagating
to downstream enforcement according to the authorization lifecycle.

This relationship is complementary to the invalidation,
fail-closed, and controlled recovery model defined by Figure31.

---

# 16. Security Invariant

The fundamental security invariant represented by Figure32 is:

"An Allow decision SHALL NOT be produced when a required
authorization basis cannot be established as valid."

The invariant applies to the combined authorization inputs and
remains applicable across distributed authorization processing.

---

# 17. Design Principle

Figure32 establishes the following design principle:

Authentication establishes identity.

Entitlement establishes the applicable right.

Policy establishes the applicable conditions.

Context establishes the applicable state.

Authorization Evaluation determines whether those conditions are
satisfied.

Enforcement determines whether the resulting decision is permitted
to affect protected service execution.

The separation of these functions prevents authentication success
from being interpreted as authorization success.

---

# 18. Figure Relationship

Figure32 is complementary to:

- Figure17 — Authorization Evaluation Model
- Figure30 — Distributed Authorization Decision Consistency
- Figure31 — Authorization Invalidation, Fail-Closed Enforcement and Controlled Recovery

Figure32 focuses specifically on the dependency and precedence
relationship among authorization inputs and the fail-closed rule
governing the resulting decision.

---

# 19. Summary

Figure32 defines an Authorization Decision as the result of an
evaluation over multiple required authorization inputs.

The model establishes that:

1. Authentication establishes identity but does not establish
   authorization.
2. The Authorization Request identifies the requested operation.
3. The Entitlement State establishes the applicable right.
4. Policy establishes the conditions for authorization.
5. Authorization Context supplies the required contextual state.
6. Authorization Evaluation combines these inputs.
7. An Allow decision requires all required conditions to be valid.
8. Invalid, unavailable, revoked, expired, or inconsistent required
   inputs SHALL prevent an Allow decision.
9. Deny and Fail-Closed outcomes protect the service boundary when
   authorization cannot be established.
10. The model remains applicable to distributed authorization
    processing.

