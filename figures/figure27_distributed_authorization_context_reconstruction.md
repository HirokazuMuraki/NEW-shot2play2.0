# Figure27 Distributed Authorization Context Reconstruction Model

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure27 |
| Title | Distributed Authorization Context Reconstruction Model |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Define reconstruction of distributed security contexts across service boundaries before authorization and execution |

---

# 1. Purpose

Figure27 defines how security-relevant context distributed across multiple
services is collected, verified, normalized, and reconstructed into an
effective authorization context.

The model extends the Context Assembly concept of Version 2.0 to
distributed service environments.

The central principle is:

Distributed Context Sources

↓

Context Verification

↓

Context Normalization

↓

Context Correlation

↓

Authorization Context Reconstruction

↓

Policy Evaluation

↓

Authorization Decision

---

# 2. Distributed Context Principle

Security-relevant authorization information MAY originate from multiple
independent services.

Representative sources include:

- Authentication Service
- Entitlement Service
- Policy Service
- Transaction Service
- Resource Service
- Risk / Trust Service
- Device / Session Service
- Delegation Service
- External Trust Provider

No single source SHALL automatically override the semantics established by
another authoritative source.

---

# 3. Context Classes

The distributed authorization context MAY contain:

- Authentication Context
- Subject Context
- Session Context
- Device Context
- Entitlement Context
- Policy Context
- Transaction Context
- Resource Context
- Delegation Context
- Risk Context
- Temporal Context
- Consumption Context

Each context SHALL retain sufficient metadata to establish its origin and
validity.

---

# 4. Authentication Context

Authentication Context represents evidence that the subject or acting
principal has been authenticated.

Representative information includes:

- Authentication Identifier
- Subject Identifier
- Authentication Method
- Authentication Assurance
- Authentication Time
- Authentication Validity
- Session Identifier
- Device Binding
- Credential / Key Reference

Authentication Context SHALL NOT by itself establish authorization.

---

# 5. Subject Context

Subject Context represents the identity and attributes used during policy
evaluation.

Representative information includes:

- Subject Identifier
- Subject Type
- Organizational Context
- Account State
- Attribute Set
- Acting Principal
- Original Principal

Subject Context SHALL be correlated with the Authentication Context.

---

# 6. Session Context

Session Context represents the active interaction in which the protected
operation is being performed.

Representative information includes:

- Session Identifier
- Session Creation Time
- Session State
- Authentication Reference
- Device Reference
- Session Expiration
- Session Security State

A stale or invalid session SHALL NOT silently produce a valid authorization
context.

---

# 7. Device Context

Where device state contributes to policy evaluation, Device Context MAY
include:

- Device Identifier
- Device Binding
- Device Trust State
- Device Assurance
- Device Registration State
- Device Security State
- Device Last-Verified Time

A change in material device state MAY require policy reevaluation.

---

# 8. Entitlement Context

Entitlement Context represents the rights, privileges, or conditions
established for the subject or transaction.

Representative information includes:

- Entitlement Identifier
- Entitlement Version
- Entitlement Type
- Entitlement State
- Entitlement Validity
- Entitlement Transaction Binding
- Entitlement Resource Scope
- Entitlement Conditions

Entitlement Context SHALL be independently verifiable.

---

# 9. Policy Context

Policy Context identifies the policy semantics used to evaluate the
authorization request.

Representative information includes:

- Policy Identifier
- Policy Version
- Policy Revision
- Policy Source
- Policy Validity
- Policy Priority
- Policy Evaluation Mode

The policy version SHALL be retained through context reconstruction.

---

# 10. Transaction Context

Transaction Context represents the operation for which authorization is
requested.

Representative information includes:

- Transaction Identifier
- Transaction Type
- Transaction State
- Request Identifier
- Resource
- Action
- Parameters
- Creation Time
- Expiration
- Previous Decision Reference

Transaction Context SHALL remain consistent throughout the authorization
process.

---

# 11. Resource Context

Resource Context identifies the protected object or service target.

Representative information includes:

- Resource Identifier
- Resource Type
- Resource Owner
- Resource State
- Resource Classification
- Resource Scope
- Resource Version

The resource presented during execution SHALL match the resource used during
authorization unless an explicitly permitted transformation exists.

---

# 12. Delegation Context

Delegation Context represents authority transferred between trusted
components.

Representative information includes:

- Delegation Identifier
- Original Subject
- Delegating Service
- Delegated Service
- Delegation Scope
- Delegation Depth
- Delegation Validity
- Delegation Policy
- Revocation State

Delegation Context SHALL NOT expand originating authority.

---

# 13. Risk Context

Risk Context MAY contain information used to modify policy evaluation.

Representative information includes:

- Risk Identifier
- Risk Score
- Risk Level
- Risk Source
- Risk Evaluation Time
- Risk Validity
- Risk Decision

Risk Context SHALL be treated as time-sensitive information.

---

# 14. Temporal Context

Temporal Context defines when a distributed context is valid.

Representative elements include:

- Not-Before
- Expiration
- Maximum Age
- Last Verification Time
- Evaluation Time
- Service-Specific Lifetime

The reconstructed context SHALL NOT outlive the shortest applicable
validity constraint unless a new trusted source explicitly establishes a
new validity period.

---

# 15. Consumption Context

Consumption Context tracks whether a security decision or entitlement
has been consumed.

Representative states include:

- ISSUED
- ACTIVE
- CONSUMED
- EXPIRED
- REVOKED
- INVALID

Consumption state SHALL be preserved across service boundaries.

A downstream service SHALL NOT reset consumption state without explicit
authorization.

---

# 16. Context Provenance

Every authoritative context element SHOULD be associated with provenance.

Representative provenance information includes:

- Source Identifier
- Issuer
- Creation Time
- Verification Time
- Version
- Integrity Reference
- Correlation Identifier
- Trust Level

Provenance allows the authorization engine to determine whether the source
is trusted and current.

---

# 17. Context Verification

Before context reconstruction, each received context SHOULD be verified
according to its type.

Representative checks include:

- Integrity
- Authenticity
- Issuer
- Signature / MAC
- Version
- Expiration
- Revocation
- Subject Consistency
- Transaction Consistency
- Scope Consistency

A required verification failure SHALL prevent the affected context from
being treated as authoritative.

---

# 18. Context Normalization

Different services MAY represent equivalent information differently.

Before policy evaluation, the system MAY normalize:

- Identifier Formats
- Timestamp Formats
- Enumerations
- Attribute Names
- Resource Names
- Action Names
- Version Representations
- State Representations

Normalization SHALL preserve semantic meaning.

Normalization SHALL NOT create new authority.

---

# 19. Context Correlation

The system SHALL correlate distributed context using security-relevant
identifiers.

Representative correlation keys include:

- Subject Identifier
- Session Identifier
- Transaction Identifier
- Entitlement Identifier
- Policy Identifier
- Decision Identifier
- Delegation Identifier
- Request Identifier

Context that cannot be reliably correlated SHALL NOT automatically be
combined into the authorization context.

---

# 20. Context Conflict Detection

Distributed context sources MAY provide conflicting information.

Representative conflicts include:

- Subject Mismatch
- Transaction Mismatch
- Entitlement State Mismatch
- Policy Version Mismatch
- Resource Mismatch
- Expiration Conflict
- Delegation Conflict
- Device Trust Conflict

Conflicting authoritative information SHALL trigger a defined conflict
resolution or rejection path.

The system SHALL NOT silently choose an arbitrary value.

---

# 21. Authority Ranking

Where multiple sources provide overlapping information, the system MAY
define an authority ranking.

Representative ranking dimensions include:

- Source Trust Level
- Issuer Authority
- Policy Authority
- Data Freshness
- Cryptographic Assurance
- Transaction Binding
- Administrative Priority

Authority ranking SHALL be deterministic or otherwise policy-defined.

---

# 22. Context Aggregation

Verified context elements MAY be aggregated into an intermediate context
representation.

Example:

Authentication Context

+

Entitlement Context

+

Policy Context

+

Transaction Context

+

Resource Context

+

Risk Context

+

Delegation Context

↓

Distributed Security Context

Aggregation SHALL retain the provenance of each component.

---

# 23. Context Reduction

Not every received context element SHALL be passed to the policy engine.

Context reduction MAY remove:

- Irrelevant Attributes
- Expired Information
- Untrusted Information
- Redundant Information
- Out-of-Scope Information
- Sensitive Information Not Required for Evaluation

Context reduction SHALL NOT remove security constraints required by policy.

---

# 24. Context Reconstruction

The policy engine SHALL receive a reconstructed authorization context
containing the information required to make the authorization decision.

Representative reconstructed components include:

- Subject
- Authentication Assurance
- Session
- Device State
- Entitlement
- Policy
- Transaction
- Resource
- Action
- Delegation
- Risk
- Temporal State
- Consumption State

The reconstructed context SHALL be traceable to the original verified
sources.

---

# 25. Context Completeness

The system SHALL determine whether sufficient context exists for policy
evaluation.

Representative completeness states:

| State | Meaning |
|---|---|
| COMPLETE | Required context is available and verified |
| PARTIAL | Some optional context is unavailable |
| INSUFFICIENT | Required context is missing |
| INVALID | Required context failed verification |
| CONFLICTED | Required context contains unresolved conflict |

An INSUFFICIENT, INVALID, or CONFLICTED state SHALL NOT silently result in
authorization.

---

# 26. Context Freshness

The system SHALL evaluate whether each context element remains sufficiently
fresh for the requested operation.

Freshness MAY depend on:

- Context Type
- Risk Level
- Transaction Type
- Resource Sensitivity
- Policy Requirements
- Service Boundary
- External Trust Level

High-risk operations MAY require more recent context than low-risk
operations.

---

# 27. Context Binding

The reconstructed context SHALL be bound to the operation being evaluated.

Binding MAY include:

- Subject
- Transaction
- Resource
- Action
- Parameters
- Session
- Device
- Entitlement
- Policy Version

A context valid for one transaction SHALL NOT automatically authorize an
unrelated transaction.

---

# 28. Policy Evaluation Input

The reconstructed context becomes the input to Policy Evaluation.

The sequence is:

Verified Distributed Context

↓

Normalized Context

↓

Correlated Context

↓

Conflict Resolution

↓

Context Completeness Check

↓

Context Freshness Check

↓

Reconstructed Authorization Context

↓

Policy Evaluation

---

# 29. Authorization Decision Generation

Policy Evaluation SHALL produce an authorization result based on the
reconstructed context.

Representative results include:

- ALLOW
- DENY
- CONDITIONAL
- REAUTH
- REENTITLE
- REAUTHORIZE
- RECOVER

The resulting decision SHALL reference the context and policy version used
for evaluation.

---

# 30. Context-to-Decision Traceability

The system SHOULD maintain traceability between:

Authentication

→ Entitlement

→ Policy

→ Transaction

→ Resource

→ Risk

→ Delegation

→ Reconstructed Context

→ Policy Evaluation

→ Authorization Decision

This trace allows later reconstruction of the authorization rationale.

---

# 31. Context Immutability

Once a security-relevant context element has been accepted for a decision,
the element SHOULD be immutable for the duration of that decision unless
the protocol explicitly supports controlled update.

A material context change SHALL invalidate or trigger reevaluation of the
affected decision.

---

# 32. Context Update

Security context MAY be updated during a transaction.

Representative triggers include:

- Reauthentication
- Entitlement Change
- Device State Change
- Risk Change
- Policy Change
- Transaction State Change
- Delegation Change
- Resource State Change

A material update SHALL trigger the applicable reevaluation process.

---

# 33. Cross-Service Context Propagation

When reconstructed context is propagated to another service, the receiving
service SHALL verify the context according to the trust relationship.

The propagated context SHALL preserve:

- Provenance
- Integrity
- Scope
- Validity
- Correlation
- Policy Semantics
- Entitlement Semantics

The receiving service SHALL NOT infer authority from fields that were not
part of the originating authorization semantics.

---

# 34. Context Transformation

A service MAY transform context representation.

Example:

Distributed Context

↓

Canonical Security Context

↓

Service-Specific Context

Transformation SHALL preserve:

- Subject Meaning
- Transaction Meaning
- Resource Scope
- Action Scope
- Entitlement Meaning
- Policy Meaning
- Temporal Constraints
- Consumption Constraints

Transformation SHALL NOT expand authority.

---

# 35. External Context

Context obtained from an external trust provider SHALL be subject to
explicit trust rules.

External context MAY require:

- Independent Verification
- Signature Validation
- Trust Anchor Validation
- Freshness Check
- Scope Restriction
- Reauthorization

External context SHALL NOT automatically override internal authoritative
context.

---

# 36. Context Failure Handling

When required context cannot be verified or reconstructed, the system MAY
return:

- REJECT
- REAUTH
- REENTITLE
- REAUTHORIZE
- RECOVER

The system SHALL preserve the transaction correlation for the failure.

---

# 37. Security Context Minimization

Only the context necessary to evaluate and enforce the requested operation
SHOULD be propagated.

Context minimization reduces:

- Unnecessary Exposure
- Cross-Service Leakage
- Confusion
- Uncontrolled Authority Propagation

Minimization SHALL NOT remove required authorization constraints.

---

# 38. Audit and Traceability

Audit records SHOULD identify:

- Context Source
- Context Version
- Verification Result
- Correlation Identifier
- Reconstruction Identifier
- Policy Evaluation Identifier
- Authorization Decision Identifier
- Service Request Identifier
- Service Execution Identifier

The audit chain SHOULD permit reconstruction of the security context used
to authorize an operation.

---

# 39. Security Properties

The Distributed Authorization Context Reconstruction Model provides:

- Distributed context verification
- Context provenance
- Context normalization
- Context correlation
- Conflict detection
- Authority ranking
- Context aggregation
- Context reduction
- Context reconstruction
- Context completeness
- Context freshness
- Context binding
- Policy continuity
- Entitlement continuity
- Delegation continuity
- Cross-service trust
- Context minimization
- Context-to-decision traceability
- Controlled context update
- Reauthorization control
- Audit traceability

---

# 40. Non-Goals

Figure27 does not define:

- A specific token format
- A specific cryptographic algorithm
- A specific policy language
- A specific service mesh
- A specific cloud provider
- A specific database
- A specific programming language
- A specific identity provider
- A specific API gateway

Implementation details MAY be defined elsewhere.

---

# 41. Design Freeze Decision

Figure27 is designated as the reference model for distributed
authorization context reconstruction within the NEW-shot2play Protocol
Suite Version 2.0.

Future modifications require Design Freeze review.

