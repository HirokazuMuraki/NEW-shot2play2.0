# Figure13 Entitlement Policy Authorization

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure13 |
| Title | Entitlement Policy Authorization |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Definition of the authorization decision model following authentication |

---

# 1. Purpose

Figure13 defines the authorization processing model introduced
and formalized in the NEW-shot2play Protocol Suite Version 2.0.

The model separates:

- Authentication
- Entitlement
- Policy Evaluation
- Authorization
- Service Execution

Authentication establishes identity assurance.

Authorization determines whether the authenticated subject is
permitted to perform the requested action on the requested resource.

---

# 2. Fundamental Processing Model

The fundamental processing model is:

Authentication

        ↓

Entitlement

        ↓

Policy Evaluation

        ↓

Authorization

        ↓

Service

Authentication and authorization SHALL remain logically separate.

A successful authentication SHALL NOT by itself imply authorization.

---

# 3. Authenticated Identity

The Authenticated Identity is the result of successful
authentication.

It provides the identity assurance required by subsequent
authorization processing.

## Representative Information

- Subject Identifier
- Registration Identifier
- Authentication Transaction Identifier
- Authentication Result
- Authentication Assurance Context

The Authenticated Identity SHALL be established before entitlement
or authorization processing begins.

---

# 4. Entitlement

Entitlement represents the subject's eligibility to possess or use
a particular capability, service, resource, or scope.

## Representative Elements

- Subject
- Capability
- Service
- Resource Scope
- Validity Period
- Entitlement State
- Issuer or Source
- Revocation State

## Entitlement States

Representative states include:

- Entitled
- Not Entitled
- Expired
- Revoked
- Suspended

An entitlement SHALL be evaluated using authoritative state.

---

# 5. Entitlement Context

The Entitlement Context is the protocol representation used by
authorization processing to describe applicable entitlements.

The Entitlement Context may contain:

- Subject Identifier
- Capability Identifier
- Resource Scope
- Service Identifier
- Effective Time
- Expiration Time
- Entitlement State
- Revocation Information

The Entitlement Context SHALL be associated with the authenticated
identity.

---

# 6. Policy

A Policy defines the conditions under which an authenticated subject
may perform an action.

A Policy may evaluate:

- Subject
- Resource
- Requested Action
- Entitlement
- Time
- Service Context
- Transaction Context
- Security State

Policy evaluation SHALL occur after the applicable identity and
entitlement information has been established.

---

# 7. Policy Evaluation

Policy Evaluation determines whether the applicable conditions
satisfy the authorization requirements.

The logical input is:

Authenticated Identity

        +

Requested Action

        +

Target Resource

        +

Entitlement Context

        +

Policy

        ↓

Policy Evaluation

The result SHALL be supplied to the Authorization decision process.

---

# 8. Authorization Request

The Authorization Request represents the request to determine
whether a particular action is permitted.

## Representative Elements

- Subject Identifier
- Requested Action
- Target Resource
- Service Identifier
- Entitlement Reference
- Policy Reference
- Transaction Context

The request SHALL be associated with an authenticated identity.

---

# 9. Authorization Decision

The Authorization Decision represents the final permission result.

Representative outcomes include:

- Allow
- Deny

The decision MAY also include a reason or decision code.

## Decision Conditions

An Allow decision requires that all applicable authorization
conditions are satisfied.

A Deny decision SHALL result when any mandatory authorization
condition fails.

---

# 10. Authorization Decision Model

The logical model is:

Authenticated Identity

        ↓

Entitlement Context

        ↓

Policy Evaluation

        ↓

Authorization Decision

The Authorization Decision SHALL NOT be derived solely from the
Authentication Result.

---

# 11. Service Execution

Service execution occurs only after an Allow decision.

The logical relationship is:

Authorization Decision

        ↓

Allow

        ↓

Service

A Deny decision SHALL prevent the protected service action.

The service layer SHALL NOT independently assume that authentication
implies permission.

---

# 12. Security Boundary

The model defines three distinct security boundaries.

## Identity Boundary

Authentication establishes:

"Who is the subject?"

## Entitlement Boundary

Entitlement establishes:

"What capability or service is the subject eligible to use?"

## Authorization Boundary

Authorization establishes:

"Is the requested action permitted under the applicable policy?"

These boundaries SHALL remain logically distinguishable.

---

# 13. Authentication vs Authorization

Authentication answers:

"Who are you?"

Authorization answers:

"What are you permitted to do?"

The Version 2.0 architecture SHALL preserve this distinction.

A subject MAY be successfully authenticated but denied authorization.

A subject MAY possess an entitlement that is nevertheless unusable
when the applicable policy conditions are not satisfied.

---

# 14. Policy Conditions

Policy evaluation MAY consider multiple conditions.

Representative conditions include:

- Identity
- Entitlement
- Resource
- Action
- Time
- Service
- Transaction State
- Security State

The policy engine SHALL evaluate the applicable conditions before
producing an authorization decision.

---

# 15. Entitlement and Policy Relationship

Entitlement and Policy have different responsibilities.

## Entitlement

Entitlement expresses eligibility or capability.

## Policy

Policy expresses conditions for permitted use.

Therefore:

Entitlement

        ≠

Authorization

An entitlement alone SHALL NOT automatically produce an Allow
decision when policy conditions are not satisfied.

---

# 16. Revocation

Authorization processing SHALL respect entitlement revocation.

The logical model is:

Entitlement

        ↓

Revocation Check

        ↓

Policy Evaluation

        ↓

Authorization Decision

A revoked entitlement SHALL NOT be used to authorize a protected
service action.

---

# 17. Expiration

Authorization processing SHALL respect applicable expiration
conditions.

Expiration MAY apply to:

- Authentication Transaction
- Entitlement
- Policy
- Authorization Request
- Service Access

Expired state SHALL be evaluated using authoritative time and
state information.

---

# 18. Transaction Context

The authorization process MAY inherit transaction context from the
authentication process.

The transaction context may provide:

- Authentication Transaction Identifier
- Authenticated Subject
- Authentication Result
- Request Context
- Service Context

The authorization layer SHALL NOT modify the historical
authentication proof.

---

# 19. Version 2.0 Processing Sequence

The complete Version 2.0 sequence is:

Authentication Request

        ↓

Authentication Response

        ↓

Verification Result

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

Service

This sequence explicitly separates identity assurance from
permission evaluation.

---

# 20. Example Scenario

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

Discount Service

The authenticated user is not automatically entitled to the
discount.

The entitlement and applicable policy conditions must be satisfied
before the EC Authorization decision becomes Allow.

---

# 21. Decision Denial

An authorization request SHALL be denied when applicable conditions
fail.

Representative denial conditions include:

- No authenticated identity
- Missing entitlement
- Expired entitlement
- Revoked entitlement
- Policy violation
- Invalid resource
- Invalid action
- Security condition failure

The denial result SHALL prevent the protected service action.

---

# 22. Separation of Responsibilities

## Authentication Layer

Responsible for:

- Identity verification
- Credential verification
- Transaction freshness
- Cryptographic proof validation

## Entitlement Layer

Responsible for:

- Capability eligibility
- Scope
- Validity
- Revocation

## Policy Layer

Responsible for:

- Condition evaluation
- Context evaluation
- Rule application

## Authorization Layer

Responsible for:

- Final permission decision

## Service Layer

Responsible for:

- Execution of an authorized operation

---

# 23. Security Properties

The Version 2.0 authorization model provides:

- Separation of authentication and authorization
- Explicit entitlement evaluation
- Policy-based authorization
- Revocation enforcement
- Expiration enforcement
- Context-aware decisions
- Explicit Allow / Deny outcomes
- Prevention of unauthorized service execution

---

# 24. Non-Goals

Figure13 does not define:

- A specific policy language
- A specific authorization engine
- A specific database schema
- A specific API framework
- A specific user interface
- Specific business rules for individual services

Those details may be defined by implementation-specific
specifications.

---

# 25. SVG Design Requirements

The SVG representation SHALL include:

- Authenticated Identity
- Entitlement
- Entitlement Context
- Policy
- Policy Evaluation
- Authorization Request
- Authorization Decision
- Service
- Authentication / Authorization boundary
- Allow / Deny outcomes
- Revocation / Expiration controls
- Version 2.0 processing sequence

The distinction between Entitlement, Policy, and Authorization SHALL
be visually explicit.

No connector or text SHALL extend outside its containing visual
region.

---

# 26. Design Freeze Decision

Figure13 is designated as the authorization processing reference
model for NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

