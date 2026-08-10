# Figure35 Authorization Decision Revocation and Revalidation Propagation

## Document Information

| Item | Value |
| --- | --- |
| Figure ID | Figure35 |
| Title | Authorization Decision Revocation and Revalidation Propagation |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of the propagation of authorization invalidation following revocation or security-state change and the subsequent revalidation process |

---

# 1. Purpose

Figure35 defines the propagation model that applies when an authorization-relevant state changes after an Authorization Decision has already been established.

The model addresses the relationship between:

- Revocation or Security State Change
- Authorization Decision Invalidation
- Invalidation Propagation
- Downstream State Enforcement
- Revalidation
- New Authorization Evaluation
- New Authorization Decision

The purpose of the model is to ensure that a previously valid Authorization Decision is not treated as continuously valid after the security state on which that decision depended has changed.

Figure35 complements Figure31, which defines fail-closed propagation, and Figure34, which defines the lifecycle of an Authorization Decision.

Figure35 specifically defines the transition from an existing decision to its invalidation, propagation, revalidation, and replacement by a newly evaluated decision.

---

# 2. Fundamental Model

The fundamental propagation relationship is:

Revocation or Security State Change

↓

Authorization Decision Invalidation

↓

Invalidation Propagation

↓

Downstream State Enforcement

↓

Revalidation

↓

New Authorization Evaluation

↓

New Authorization Decision

The propagation SHALL preserve the security meaning of the originating state change.

A downstream component SHALL NOT continue to rely on an Authorization Decision that has been determined to be invalid.

---

# 3. Authorization Decision Basis

An Authorization Decision is derived from a set of authorization inputs and contextual conditions.

Representative decision inputs include:

- Authenticated Identity
- Authorization Request
- Entitlement State
- Policy
- Authorization Context
- Transaction State
- Security State

The resulting Authorization Decision represents the evaluation of those inputs at a particular authorization state.

The decision MAY therefore become invalid when a decision-relevant input changes.

---

# 4. Revocation Event

A Revocation Event represents an event that removes or invalidates a previously established authorization basis.

Representative revocation causes include:

- Entitlement Revocation
- Credential Revocation
- Administrative Revocation
- Security Incident
- Policy Change
- Account State Change
- Transaction Cancellation
- Fraud Detection
- Service Security Event

A Revocation Event SHALL be associated with sufficient information to identify the affected authorization state.

---

# 5. Security State Change

A Security State Change represents a change that affects the validity of an existing Authorization Decision without necessarily being an explicit revocation.

Representative examples include:

- Security Policy Change
- Risk State Change
- Device Trust Change
- Session State Change
- Transaction State Change
- Contextual Security Change
- External Security Event

A Security State Change SHALL be treated as authorization-relevant when the applicable Policy requires reevaluation.

---

# 6. Decision Invalidation

When a Revocation Event or authorization-relevant Security State Change affects an existing Authorization Decision, the affected decision SHALL be marked invalid.

The logical transition is:

Existing Valid Decision

↓

Decision Invalidation

↓

Invalid Decision State

The invalidation SHALL prevent the affected decision from being treated as an active authorization basis.

A decision that has entered an invalid state SHALL NOT independently authorize a protected service action.

---

# 7. Invalidation Identity

Invalidation SHALL identify the authorization state to which the invalidation applies.

Representative identifiers include:

- Authorization Decision Identifier
- Authorization Request Identifier
- Subject Identifier
- Entitlement Identifier
- Policy Identifier
- Service Identifier
- Transaction Identifier
- Security State Identifier

The identifiers MAY be combined when necessary to uniquely identify the affected authorization state.

---

# 8. Invalidation Propagation

The invalidation state SHALL be propagated to downstream components that may rely on the affected Authorization Decision.

Representative downstream targets include:

- Authorization Service
- Policy Enforcement Point
- Service Gateway
- Application Service
- Distributed Authorization Node
- Cached Decision Store
- Session State Store

Propagation MAY occur through:

- Event Notification
- State Synchronization
- Decision Revocation Message
- Cache Invalidation
- State Update
- Secure Context Propagation

The propagation mechanism SHALL preserve the identity of the affected authorization state.

---

# 9. Propagation Semantics

A downstream component receiving an invalidation SHALL determine whether it currently relies on the affected Authorization Decision.

If the downstream component does not rely on the affected decision, the invalidation MAY be recorded without further enforcement action.

If the downstream component relies on the affected decision, the component SHALL invalidate the local authorization state associated with that decision.

A downstream component SHALL NOT silently convert an invalidation event into continued authorization.

---

# 10. Downstream State Enforcement

Downstream State Enforcement represents the application of the invalidation state to local execution controls.

The enforcement MAY include:

- Removing an active authorization state
- Disabling continued execution
- Invalidating a cached decision
- Blocking a protected operation
- Requiring revalidation
- Terminating an authorization-dependent operation

The enforcement result SHALL be consistent with the applicable Policy.

When the validity of the authorization basis cannot be established, enforcement SHALL follow the applicable fail-closed requirement.

---

# 11. Protected Service Boundary

The Protected Service Boundary represents the point beyond which an Authorization Decision is required for service execution.

A previously valid decision SHALL NOT remain sufficient after the decision has been invalidated.

The service boundary SHALL therefore evaluate the current authorization state before allowing continued protected execution.

If a valid authorization basis is unavailable, the protected service action SHALL NOT proceed.

---

# 12. Revalidation Trigger

Revalidation is triggered when a previously established authorization state has been invalidated or when the applicable Policy requires renewed evaluation.

Representative triggers include:

- Decision Invalidation
- Entitlement State Change
- Policy Change
- Security State Change
- Context Change
- Transaction State Change
- Explicit Reauthorization Request

The revalidation process SHALL use the current authoritative state rather than relying exclusively on the previous decision.

---

# 13. Revalidation Inputs

Revalidation SHALL evaluate the current authorization inputs as required by the applicable Policy.

Representative inputs include:

- Current Authenticated Identity
- Current Authorization Request
- Current Entitlement State
- Current Policy
- Current Authorization Context
- Current Transaction State
- Current Security State

The revalidation process MAY require new evidence when the existing evidence is no longer sufficient.

---

# 14. New Authorization Evaluation

After revalidation inputs have been assembled, a new Authorization Evaluation SHALL be performed.

The new evaluation SHALL be logically independent from the invalidated Authorization Decision.

The previous decision MAY be retained as historical information, but it SHALL NOT be treated as the authoritative basis for the new decision.

The new evaluation SHALL determine whether the current authorization conditions are satisfied.

---

# 15. New Authorization Decision

The result of the new evaluation SHALL produce a new Authorization Decision.

The new decision MAY be:

- Allow
- Deny
- Fail-Closed

A new Allow decision SHALL require that all required authorization conditions are currently satisfied.

A new Deny or Fail-Closed decision SHALL prevent protected service execution as required by the applicable Policy.

The new decision SHALL have its own decision identity or state version sufficient to distinguish it from the invalidated decision.

---

# 16. Decision Version Continuity

Where authorization state is versioned, the invalidation and subsequent new decision SHOULD preserve a monotonic state relationship.

A representative relationship is:

Previous Decision Version

→

Invalidated State

→

New Decision Version

A stale downstream representation SHALL NOT override a newer authorization state.

Where conflicting state versions are detected, the implementation SHALL apply the applicable consistency and fail-closed requirements.

---

# 17. Distributed Propagation

In a distributed authorization environment, the invalidation MAY propagate to multiple authorization or enforcement nodes.

Each affected node SHALL maintain sufficient information to determine whether its local authorization state remains valid.

The propagation model is:

Authoritative Security State Change

↓

Invalidation Event

↓

Distributed Authorization Nodes

↓

Local State Invalidation

↓

Revalidation

The local state SHALL NOT become authoritative merely because it was previously valid.

---

# 18. Failure and Unavailability

If an invalidation event is received but the resulting authorization state cannot be established, the affected protected operation SHALL follow the applicable fail-closed behavior.

Examples include:

- Missing Invalidation Information
- Unavailable Authorization State
- Conflicting State Versions
- Stale Local State
- Unavailable Policy
- Unavailable Entitlement State
- Incomplete Security Context

An implementation SHALL NOT assume that a previously valid decision remains valid solely because the new authorization state is temporarily unavailable.

---

# 19. Security Invariant

The following invariant SHALL hold:

An Authorization Decision that has been invalidated SHALL NOT authorize protected service execution.

A new Allow decision SHALL require successful evaluation of the current authorization state.

A stale, revoked, expired, inconsistent, or otherwise invalid authorization state SHALL NOT be used as an Allow basis.

---

# 20. Relationship to Other Figures

Figure35 complements the following figures:

- Figure31 defines authorization invalidation and fail-closed propagation.
- Figure34 defines the lifecycle of an Authorization Decision.
- Figure35 defines the specific propagation path from revocation or security-state change through invalidation, downstream enforcement, revalidation, and creation of a new Authorization Decision.

The figures are therefore complementary rather than redundant.

---

# 21. Summary

Figure35 defines the lifecycle transition that occurs when an existing Authorization Decision is affected by revocation or an authorization-relevant security-state change.

The model establishes that:

1. A Revocation Event or Security State Change MAY invalidate an existing Authorization Decision.
2. The invalidation SHALL identify the affected authorization state.
3. The invalidation SHALL propagate to downstream components that may rely on the affected decision.
4. A downstream component relying on the affected decision SHALL invalidate its corresponding local authorization state.
5. Protected service execution SHALL NOT continue solely on the basis of an invalidated decision.
6. Revalidation SHALL use current authoritative authorization inputs.
7. The new evaluation SHALL be independent of the invalidated decision.
8. A new Authorization Decision SHALL be generated from the current authorization state.
9. A new Allow decision SHALL require satisfaction of all required authorization conditions.
10. Stale, conflicting, unavailable, or inconsistent authorization state SHALL NOT be used to establish an Allow decision.
11. The model applies to both centralized and distributed authorization environments.

