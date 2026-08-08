# Figure14 Authorization Decision

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure14 |
| Title | Authorization Decision |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of the authorization decision process and Allow / Deny outcomes |

---

# 1. Purpose

Figure14 defines the authorization decision process of the
NEW-shot2play Protocol Suite Version 2.0.

The figure shows how the authorization decision is derived from:

- Authenticated Identity
- Requested Action
- Target Resource
- Entitlement Context
- Policy Conditions
- Security State

The model produces an explicit:

- Allow
- Deny

decision.

---

# 2. Fundamental Decision Model

The fundamental decision model is:

Authenticated Identity

        +

Requested Action

        +

Target Resource

        +

Entitlement Context

        +

Policy Conditions

        +

Security State

        ↓

Authorization Decision

        ↓

Allow / Deny

The Authorization Decision SHALL NOT be derived solely from
authentication success.

---

# 3. Authentication Precondition

Authorization processing requires a valid authenticated identity.

The authentication precondition includes:

- Authentication completed
- Credential verified
- Authentication transaction valid
- Authentication result accepted

If authentication has failed, authorization SHALL NOT produce
an Allow decision.

---

# 4. Authenticated Identity

The Authenticated Identity identifies the subject for which the
authorization decision is being evaluated.

Representative information includes:

- Subject Identifier
- Registration Identifier
- Authentication Transaction Identifier
- Authentication Result
- Authentication Assurance Context

The identity context SHALL be associated with the current
authorization request.

---

# 5. Requested Action

The Requested Action identifies the operation that the subject
attempts to perform.

Representative actions include:

- View
- Access
- Redeem
- Transfer
- Modify
- Execute
- Consume

The requested action SHALL be explicitly identified before the
authorization decision is made.

---

# 6. Target Resource

The Target Resource identifies the resource or service affected
by the requested action.

Representative information includes:

- Resource Identifier
- Service Identifier
- Resource Type
- Resource Scope

Authorization SHALL evaluate the requested action against the
specified target resource.

---

# 7. Entitlement Context

The Entitlement Context represents the applicable eligibility
or capability information.

Representative information includes:

- Subject
- Capability
- Resource Scope
- Service
- Effective Time
- Expiration Time
- Entitlement State
- Revocation State

The entitlement state SHALL be checked before an Allow decision.

---

# 8. Policy Conditions

Policy Conditions define the rules that must be satisfied for
the requested action.

Representative conditions include:

- Identity
- Entitlement
- Action
- Resource
- Time
- Service Context
- Transaction Context
- Security State

All mandatory policy conditions SHALL be evaluated.

---

# 9. Security State

Security State represents security-related conditions that may
affect the authorization decision.

Representative states include:

- Active
- Suspended
- Revoked
- Expired
- Security Incident
- Restricted

A security state that prevents service access SHALL result in
Deny.

---

# 10. Authorization Evaluation

The authorization engine evaluates the applicable inputs:

Authenticated Identity

        +

Requested Action

        +

Target Resource

        +

Entitlement Context

        +

Policy Conditions

        +

Security State

        ↓

Authorization Evaluation

The evaluation SHALL produce a deterministic decision for the
applicable policy state.

---

# 11. Decision Preconditions

An Allow decision requires all mandatory conditions to be satisfied.

Representative Allow preconditions are:

1. Authenticated Identity is valid.
2. Authentication transaction is valid.
3. Requested Action is valid.
4. Target Resource is valid.
5. Applicable entitlement exists.
6. Entitlement is active.
7. Entitlement is not revoked.
8. Entitlement is not expired.
9. Policy conditions are satisfied.
10. Security State permits the action.

If any mandatory condition fails, the decision SHALL be Deny.

---

# 12. Allow Decision

An Allow decision indicates that the requested action is permitted.

The logical result is:

Authorization Evaluation

        ↓

Allow

        ↓

Protected Service Action

The service MAY execute the requested operation after receiving
an Allow decision.

---

# 13. Deny Decision

A Deny decision indicates that the requested action is not
permitted.

Representative causes include:

- Authentication failure
- Missing entitlement
- Expired entitlement
- Revoked entitlement
- Invalid action
- Invalid resource
- Policy violation
- Security restriction
- Invalid transaction context

The protected service action SHALL NOT be executed after Deny.

---

# 14. Decision Reason

The Authorization Decision MAY include a decision reason or
decision code.

Representative decision codes include:

- ALLOW
- AUTHENTICATION_REQUIRED
- ENTITLEMENT_REQUIRED
- ENTITLEMENT_EXPIRED
- ENTITLEMENT_REVOKED
- POLICY_DENIED
- RESOURCE_DENIED
- SECURITY_DENIED

The decision reason SHALL NOT expose unnecessary sensitive
security information to an untrusted requester.

---

# 15. Revocation Check

Revocation SHALL be evaluated before an Allow decision.

The logical sequence is:

Entitlement

        ↓

Revocation Check

        ↓

Policy Evaluation

        ↓

Authorization Decision

A revoked entitlement SHALL NOT produce an Allow decision.

---

# 16. Expiration Check

Applicable expiration conditions SHALL be evaluated before an
Allow decision.

Expiration MAY apply to:

- Authentication Transaction
- Entitlement
- Policy
- Authorization Request
- Service Access

Expired authorization context SHALL result in Deny unless an
explicitly defined valid replacement context exists.

---

# 17. Time Evaluation

Where policy conditions include time, the authorization engine
SHALL evaluate the applicable authoritative time source.

Representative time conditions include:

- Effective From
- Effective Until
- Access Window
- Transaction Expiration
- Entitlement Expiration

Time conditions SHALL be evaluated consistently for the
authorization transaction.

---

# 18. Decision Consistency

For identical:

- Authenticated Identity
- Requested Action
- Target Resource
- Entitlement Context
- Policy Conditions
- Security State

the authorization engine SHOULD produce the same decision under
the same authoritative state.

Changes in authoritative state MAY produce a different decision.

---

# 19. Decision Binding

The Authorization Decision SHALL be associated with the
authorization request from which it was derived.

The logical binding is:

Authorization Request

        +

Authorization Context

        ↓

Authorization Decision

The decision SHALL NOT be reused for an unrelated request unless
explicitly permitted by the applicable authorization model.

---

# 20. Service Enforcement

The service layer SHALL enforce the Authorization Decision.

The logical relationship is:

Authorization Decision

        ↓

Allow

        ↓

Service Execution

or:

Authorization Decision

        ↓

Deny

        ↓

Service Execution Blocked

The service SHALL NOT treat authentication success as sufficient
authorization.

---

# 21. Version 2.0 Processing Sequence

The complete sequence is:

Authentication

        ↓

Authenticated Identity

        ↓

Entitlement Context

        ↓

Policy Evaluation

        ↓

Authorization Request

        ↓

Authorization Decision

        ↓

Allow / Deny

        ↓

Service

This sequence explicitly separates identity assurance from
permission evaluation.

---

# 22. Example Scenario

A representative scenario is:

Authenticated User

        ↓

Visit Evidence

        ↓

Discount Entitlement

        ↓

Policy Evaluation

        ↓

EC Authorization

        ↓

Allow

        ↓

Discount Service

If the entitlement is expired or revoked, the same request results
in Deny.

---

# 23. Security Properties

The authorization decision model provides:

- Explicit authorization evaluation
- Separation from authentication
- Entitlement validation
- Policy enforcement
- Revocation enforcement
- Expiration enforcement
- Security-state enforcement
- Explicit Allow / Deny outcomes
- Service-side decision enforcement

---

# 24. Non-Goals

Figure14 does not define:

- A specific policy language
- A specific authorization engine
- A specific database schema
- A specific API framework
- A specific user interface
- Business-specific authorization rules

Those details may be defined by implementation-specific
specifications.

---

# 25. SVG Design Requirements

The SVG representation SHALL include:

- Authentication precondition
- Authenticated Identity
- Requested Action
- Target Resource
- Entitlement Context
- Policy Conditions
- Security State
- Authorization Evaluation
- Allow decision
- Deny decision
- Service enforcement
- Version 2.0 processing sequence
- Example scenario

The Allow and Deny paths SHALL be visually distinguishable.

No text or connector SHALL extend outside its containing visual
region.

---

# 26. Design Freeze Decision

Figure14 is designated as the authorization decision reference
model for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

