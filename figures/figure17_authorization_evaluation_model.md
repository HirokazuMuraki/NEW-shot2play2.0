# Figure17 Authorization Evaluation Model

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure17 |
| Title | Authorization Evaluation Model |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of the authorization evaluation model combining identity, entitlement, policy, context, and service action |

---

# 1. Purpose

Figure17 defines the authorization evaluation model of the
NEW-shot2play Protocol Suite Version 2.0.

The model separates:

- Authentication
- Identity
- Entitlement
- Policy
- Authorization
- Service Action

Authentication establishes the authenticated identity.

Authorization determines whether that identity is permitted to
perform the requested action under the applicable entitlement,
policy, resource, service, and contextual conditions.

Authentication success SHALL NOT by itself constitute authorization.

---

# 2. Fundamental Authorization Model

The fundamental relationship is:

Authenticated Identity

        +

Authorization Request

        +

Entitlement

        +

Policy

        +

Authorization Context

        ↓

Authorization Evaluation

        ↓

Authorization Decision

        ↓

Allow / Deny

The authorization decision SHALL be derived from the applicable
current state and policy.

---

# 3. Authentication Boundary

Authentication establishes an authenticated identity.

Representative authentication outputs include:

- Subject Identifier
- Authentication Method
- Authentication Result
- Authentication Time
- Authentication Transaction Identifier
- Authentication Assurance Information

The authentication protocol SHALL establish identity evidence.

The authorization protocol SHALL determine whether the authenticated
identity is permitted to perform the requested action.

---

# 4. Authorization Request

An Authorization Request represents a request to perform an action
against a resource or service.

Representative attributes include:

- Authorization Request Identifier
- Subject Identifier
- Action
- Resource
- Service
- Entitlement Reference
- Timestamp
- Context
- Transaction Reference

The request SHALL identify the action that is being requested.

---

# 5. Entitlement Input

An Entitlement represents a grant or right applicable to a subject.

Representative attributes include:

- Entitlement Identifier
- Subject Identifier
- Capability
- Resource
- Service
- Scope
- Quantity
- Effective Time
- Expiration Time
- Status
- Revocation State

The entitlement SHALL be evaluated using its authoritative current
state.

---

# 6. Policy Input

A Policy defines the conditions under which an action is permitted.

Representative policy conditions include:

- Subject
- Resource
- Action
- Service
- Scope
- Time
- Location
- Device
- Risk
- Entitlement State
- Transaction State

The policy MAY require multiple conditions to be satisfied.

---

# 7. Authorization Context

Authorization Context contains contextual information used by the
policy evaluation.

Representative context includes:

- Current Time
- Service Context
- Device Context
- Session Context
- Transaction Context
- Risk Context
- External Event Context

Context SHALL be evaluated according to the applicable policy.

---

# 8. Entitlement Validation

Before an entitlement can contribute to an Allow decision, the
following conditions SHALL be evaluated as applicable:

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

An invalid entitlement SHALL NOT produce an Allow decision.

---

# 9. Policy Evaluation

Policy Evaluation combines the authorization inputs.

The logical model is:

Identity

        +

Request

        +

Entitlement

        +

Policy

        +

Context

        ↓

Policy Evaluation

        ↓

Conditions Satisfied?

The implementation MAY use any suitable policy evaluation mechanism.

The protocol specification defines the semantic result rather than
requiring a specific policy engine.

---

# 10. Authorization Decision

The Authorization Decision SHALL provide at least:

- Decision
- Authorization Request Identifier
- Evaluated Subject
- Applicable Entitlement
- Policy Reference
- Decision Time

The decision MAY additionally contain:

- Reason
- Conditions
- Obligation
- Advice
- Decision Expiration
- Policy Version
- Entitlement Version

The primary decision values are:

- Allow
- Deny

---

# 11. Allow Processing

If the decision is Allow:

Authorization Decision

        ↓

Allow

        ↓

Service Action

The service MAY perform the requested operation.

If the operation consumes an entitlement, a Consumption Transaction
SHALL be generated or otherwise recorded according to the applicable
Service Profile.

---

# 12. Deny Processing

If the decision is Deny:

Authorization Decision

        ↓

Deny

        ↓

Service Action Blocked

No entitlement consumption SHALL occur as a consequence of the
denied authorization request.

The Deny decision MAY include a reason or policy reference.

---

# 13. Decision Independence

Authentication and authorization decisions SHALL remain logically
independent.

The following condition SHALL NOT be assumed:

Authentication = Success
therefore
Authorization = Allow

Instead:

Authentication = Success

        ↓

Authenticated Identity

        ↓

Authorization Evaluation

        ↓

Allow / Deny

---

# 14. Decision Determinism

For a given authoritative state, request, policy, and context,
authorization evaluation SHOULD produce a deterministic result.

Any non-deterministic input SHALL be explicitly represented as part
of the authorization context or policy.

---

# 15. State Consistency

The authorization evaluator SHALL use an entitlement state that is
current according to the applicable consistency requirement.

The following SHALL be prevented:

- Expired entitlement producing Allow
- Revoked entitlement producing Allow
- Consumed entitlement producing Allow
- Insufficient quantity producing Allow
- Wrong-subject entitlement producing Allow
- Wrong-service entitlement producing Allow

---

# 16. Authorization and Consumption

Authorization and consumption SHALL be logically distinguishable.

Authorization answers:

"Is this action permitted?"

Consumption answers:

"Has the entitlement been used?"

The sequence is:

Authorization Evaluation

        ↓

Allow

        ↓

Service Action

        ↓

Consumption

        ↓

Entitlement State Update

The implementation SHALL prevent unauthorized consumption.

---

# 17. Multiple Entitlements

A request MAY reference multiple Entitlements.

Examples include:

- Entitlement A AND Entitlement B
- Entitlement A OR Entitlement B
- Entitlement A with required Capability B
- Multiple quantity-based entitlements

The applicable Policy SHALL define the relationship.

The evaluation result SHALL remain a single authorization decision
for the requested action.

---

# 18. Policy Versioning

A Policy MAY have a version identifier.

The Authorization Decision SHOULD record the Policy Version used
for evaluation.

This supports:

- Audit
- Reproducibility
- Incident Investigation
- Policy Migration

---

# 19. Entitlement Versioning

An Entitlement MAY have a version or state revision identifier.

The Authorization Decision SHOULD identify the entitlement state
used for evaluation where required for auditability.

A subsequent entitlement state update SHALL NOT retroactively alter
the historical decision record.

---

# 20. Transaction Binding

The authorization request and decision SHOULD be associated with
a Transaction Identifier.

Where consumption occurs, the Consumption Transaction SHALL
reference:

- Authorization Request Identifier
- Authorization Decision
- Entitlement Identifier

This provides traceability across the authorization lifecycle.

---

# 21. Replay Protection

An Authorization Request SHALL NOT be replayed to obtain an
additional unauthorized service action.

Replay protection MAY use:

- Unique Authorization Request Identifier
- Nonce
- Timestamp
- Expiration
- Request State
- Transaction State

A previously completed request SHALL NOT automatically create a new
authorization event.

---

# 22. Fail-Safe Behavior

If a required authorization input cannot be reliably evaluated,
the implementation SHOULD fail closed.

Examples include:

- Entitlement lookup failure
- Invalid entitlement state
- Missing required policy
- Unknown subject
- Invalid context
- Policy evaluation failure
- Integrity failure

Unless explicitly permitted by policy, uncertainty SHALL NOT be
interpreted as Allow.

---

# 23. Authorization Result and Service

The service SHALL consume the authorization result according to the
applicable Service Profile.

The logical relationship is:

Authorization Decision

        ↓

Service Enforcement Point

        ↓

Service Action

The Service Enforcement Point SHALL prevent the protected action
when the decision is Deny.

---

# 24. Audit and Traceability

The authorization system SHOULD provide traceability across:

- Subject Identifier
- Authentication Transaction
- Authorization Request
- Entitlement Identifier
- Policy Identifier
- Policy Version
- Authorization Decision
- Consumption Transaction
- Service Result

This permits reconstruction of the authorization event.

---

# 25. Version 2.0 Example

A representative Version 2.0 scenario is:

Authentication

        ↓

Authenticated User

        ↓

Visit Evidence

        ↓

Discount Entitlement

        ↓

EC Authorization Request

        ↓

Entitlement + Policy + Context Evaluation

        ↓

Authorization = Allow

        ↓

Discount Applied

        ↓

Consumption Transaction

        ↓

Entitlement State Updated

Authentication establishes the identity.

Visit Evidence establishes the qualifying event.

Discount Entitlement establishes the applicable right.

Policy determines whether the right is valid for the requested
service action.

Authorization determines whether the action is permitted.

Consumption updates the entitlement state after successful use.

---

# 26. Security Properties

The authorization evaluation model provides:

- Authentication / Authorization separation
- Explicit entitlement validation
- Policy-based authorization
- Context-aware evaluation
- State consistency
- Revocation enforcement
- Expiration enforcement
- Quantity enforcement
- Subject binding
- Service binding
- Replay protection
- Fail-safe behavior
- Auditability

---

# 27. Non-Goals

Figure17 does not define:

- A specific policy language
- A specific policy engine
- A specific authorization server implementation
- A specific database
- A specific API framework
- A specific service application

Implementation-specific mechanisms MAY be defined elsewhere.

---

# 28. Design Freeze Decision

Figure17 is designated as the Authorization Evaluation Model
reference model for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

