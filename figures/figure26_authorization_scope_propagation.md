# Figure26 Authorization Scope Propagation and Delegation Boundary Model

## Document Information

| Item | Value |
|---|---|
| Figure ID | Figure26 |
| Title | Authorization Scope Propagation and Delegation Boundary Model |
| Version | NEW-shot2play Protocol Suite Version 2.0 |
| Revision | Design Definition Version 2.0 |
| Status | Design Definition |
| Purpose | Define controlled propagation of authorization scope across services, resources, and delegated processing boundaries |

---

# 1. Purpose

Figure26 defines how an authorization scope MAY propagate across
multiple service components while preserving the security constraints
established by the original authorization decision.

The model prevents authorization obtained for one protected operation
from becoming unrestricted authority across other services or resources.

The central principle is:

Authorization Scope

↓

Controlled Propagation

↓

Boundary Verification

↓

Service Execution

Propagation SHALL preserve the security semantics of the originating
authorization decision.

---

# 2. Core Principle

An authorization decision SHALL define an explicit scope.

The scope MAY include:

- Subject
- Transaction
- Resource
- Resource Scope
- Action
- Policy
- Policy Version
- Entitlement
- Time Window
- Service Scope
- Transaction Scope
- Parameter Constraints
- Consumption Constraints

A downstream component SHALL NOT assume authority beyond the defined
scope.

---

# 3. Scope Representation

The authorization scope SHOULD be represented by a verifiable security
context.

Representative elements include:

- Authorization Decision Identifier
- Subject Identifier
- Transaction Identifier
- Resource Identifier
- Resource Scope
- Action
- Policy Identifier
- Policy Version
- Entitlement Identifier
- Validity Period
- Service Scope
- Delegation Scope
- Consumption State
- Integrity Protection

The representation MAY be carried as a signed object, authenticated
token, secure internal reference, or equivalent trusted mechanism.

---

# 4. Scope Origin

The scope originates from the Authorization Domain after Policy
Evaluation.

The origin sequence is:

Authentication

↓

Entitlement

↓

Policy Evaluation

↓

Authorization Decision

↓

Scope Establishment

The scope SHALL reflect the policy and entitlement state used to produce
the authorization decision.

---

# 5. Scope Propagation

A service MAY propagate an authorization scope to a downstream service
when the protocol explicitly permits propagation.

Propagation SHALL NOT automatically expand the original scope.

The downstream service SHALL receive only the authority necessary for the
authorized operation.

---

# 6. Propagation Modes

The protocol MAY define multiple propagation modes.

Representative modes include:

- Direct Propagation
- Restricted Propagation
- Derived Scope
- Delegated Scope
- Reauthorization Required
- Propagation Prohibited

Each mode SHALL have explicitly defined semantics.

---

# 7. Direct Propagation

Direct Propagation means that a downstream service receives the same
authorization scope without semantic expansion.

Example:

Service A

↓

Service B

Resource = R1

Action = READ

If the original scope permits only READ access to R1, Service B SHALL NOT
convert the propagated scope into WRITE access.

---

# 8. Restricted Propagation

Restricted Propagation means that a downstream service receives a
strictly narrower scope.

For example:

Original Scope:

Resource = R1

Actions = READ, WRITE

↓

Downstream Scope:

Resource = R1

Action = READ

The downstream scope is valid because it does not exceed the originating
scope.

---

# 9. Derived Scope

A downstream scope MAY be derived from the originating scope.

The derived scope SHALL satisfy:

Derived Scope ⊆ Originating Scope

The derivation function SHALL be deterministic or otherwise
verifiably constrained according to the protocol.

A derived scope SHALL NOT introduce authority absent from the originating
scope.

---

# 10. Delegated Scope

Delegation allows one trusted service to authorize another service to
perform a defined operation.

Delegation SHALL be explicit.

Representative delegation elements include:

- Delegating Subject
- Delegated Service
- Original Subject
- Transaction
- Resource
- Action
- Delegation Scope
- Delegation Validity
- Delegation Identifier
- Policy Reference
- Integrity Protection

Delegation SHALL NOT implicitly transfer all authority of the original
subject.

---

# 11. Delegation Boundary

A delegation boundary defines the maximum authority that may be exercised
by the delegated service.

The delegated service SHALL remain within:

- Authorized Resource Scope
- Authorized Action Scope
- Authorized Transaction
- Authorized Time Window
- Authorized Service Scope
- Authorized Parameter Constraints

Crossing a delegation boundary SHALL require additional authorization.

---

# 12. Service Boundary

Each service MAY represent an independent authorization boundary.

Representative services include:

- Authentication Service
- Entitlement Service
- Policy Service
- Authorization Service
- Enforcement Service
- Resource Service
- Payment Service
- Inventory Service
- Notification Service
- External Partner Service

A valid authorization decision in one service SHALL NOT automatically
constitute unrestricted authorization in another independent service.

---

# 13. Resource Boundary

Resources SHALL remain within their defined authorization scope.

For example:

Authorization:

Resource = Account A

Action = READ

shall not authorize:

Resource = Account B

Action = READ

unless the policy explicitly defines a resource scope covering both.

---

# 14. Action Boundary

Actions SHALL remain independently constrained.

For example:

READ

does not imply:

WRITE

WRITE

does not imply:

DELETE

EXECUTE

does not imply:

ADMINISTER

A downstream service SHALL enforce the action boundary.

---

# 15. Transaction Boundary

Authorization SHALL remain associated with the transaction for which it
was generated.

A downstream service SHALL verify transaction continuity where
transaction binding is required.

A propagated scope SHALL NOT be reused for an unrelated transaction.

---

# 16. Subject Continuity

The downstream service SHALL preserve the identity of the original
authorized subject.

Where delegation occurs, the system SHOULD distinguish:

- Original Subject
- Delegating Service
- Delegated Service
- Acting Principal

This prevents delegated processing from obscuring the authority chain.

---

# 17. Policy Continuity

The downstream service SHOULD preserve the policy context used to
establish the authorization scope.

Representative policy information includes:

- Policy Identifier
- Policy Version
- Evaluation Identifier
- Decision Identifier

If the downstream operation is governed by materially different policy
semantics, reauthorization MAY be required.

---

# 18. Entitlement Continuity

Where authorization depends on entitlement, the downstream service SHALL
preserve the relevant entitlement context.

A change in entitlement state MAY invalidate the propagated scope.

Representative entitlement information includes:

- Entitlement Identifier
- Entitlement Version
- Entitlement State
- Entitlement Validity
- Entitlement Transaction Binding

---

# 19. Temporal Boundary

A propagated scope SHALL NOT remain valid beyond its defined validity
period.

Representative temporal controls include:

- Not-Before
- Expiration
- Maximum Lifetime
- Service-Specific Lifetime

A downstream service MAY impose a shorter validity period.

A downstream service SHALL NOT extend the originating validity period
without a new authorization decision.

---

# 20. Parameter Boundary

Where security-relevant parameters are part of authorization, those
parameters SHALL remain within the permitted range.

Representative parameters include:

- Amount
- Quantity
- Destination
- Account
- Resource Subset
- Geographic Scope
- Time Window

A material parameter change SHALL trigger reevaluation or
reauthorization where required.

---

# 21. Consumption Boundary

Authorization MAY be subject to consumption constraints.

Representative constraints include:

- Single-use
- Limited-use
- Transaction-scoped
- Service-scoped
- Time-limited

A downstream service SHALL NOT reset or extend consumption state unless
explicitly authorized.

---

# 22. Scope Narrowing

Scope narrowing is permitted when the resulting scope remains within the
originating authority.

Example:

Originating Scope:

Resource = R1

Actions = READ, WRITE

↓

Service A Scope:

Resource = R1

Action = WRITE

↓

Service B Scope:

Resource = R1

Action = READ

Each downstream scope remains constrained by the original authorization
scope.

---

# 23. Scope Expansion

Scope expansion is prohibited without new authorization.

Example:

Originating Scope:

Resource = R1

Action = READ

↓

Requested:

Resource = R2

Action = WRITE

The downstream service SHALL reject the expansion request or invoke the
appropriate reauthorization path.

---

# 24. Propagation Verification

Before accepting a propagated authorization scope, the receiving service
SHALL verify, where applicable:

1. Integrity
2. Issuer
3. Subject
4. Original Transaction
5. Resource Scope
6. Action Scope
7. Policy Version
8. Entitlement State
9. Validity Period
10. Delegation Scope
11. Consumption State
12. Request Consistency

A required verification failure SHALL prevent execution.

---

# 25. Boundary Decision

The receiving service SHALL classify the requested operation.

Representative outcomes include:

| Outcome | Meaning |
|---|---|
| PROPAGATE | Existing scope may continue |
| NARROW | A reduced scope is issued |
| DELEGATE | Controlled delegation is permitted |
| REAUTHORIZE | New authorization decision is required |
| REJECT | Operation is outside scope |
| RECOVER | Controlled recovery is required |

---

# 26. Reauthorization Boundary

Reauthorization SHALL be required when the requested operation cannot be
derived safely from the existing authorization scope.

Representative triggers include:

- New Resource
- New Action
- New Transaction
- Material Parameter Change
- Policy Change
- Entitlement Change
- Expiration
- Service Boundary Crossing
- Delegation Beyond Existing Scope

---

# 27. Delegation Chain

A delegation chain MAY contain multiple trusted services.

Example:

Original Subject

↓

Authorization Domain

↓

Service A

↓

Delegated Service B

↓

Delegated Service C

Each delegation step SHALL preserve the original security context and
shall not exceed the authority received from the preceding step.

The effective scope SHALL remain bounded by the originating authorization.

---

# 28. Delegation Depth

The protocol MAY define a maximum delegation depth.

Representative controls include:

- Delegation Depth
- Maximum Delegation Count
- Delegation Expiration
- Delegation Policy
- Delegation Chain Identifier

When the maximum delegation depth is reached, further delegation SHALL
be prohibited unless a new authorization decision permits it.

---

# 29. Delegation Revocation

A delegated scope MAY be revoked independently of the originating
authorization.

Revocation MAY be triggered by:

- Subject Revocation
- Entitlement Revocation
- Policy Change
- Transaction Cancellation
- Security Event
- Service Compromise
- Explicit Delegation Revocation

Revoked delegation SHALL NOT authorize further execution.

---

# 30. Cross-Service Verification

When authorization crosses a service boundary, the receiving service
SHALL verify the security context according to its trust relationship
with the issuing service.

Representative trust models include:

- Direct Trust
- Federated Trust
- Cryptographic Trust
- Trusted Internal Reference
- Explicit Delegation Relationship

The receiving service SHALL NOT trust an authorization object merely
because it originated from an arbitrary upstream component.

---

# 31. External Boundary

Crossing into an external partner or third-party service SHALL constitute
a distinct trust boundary unless explicitly incorporated into the
protocol trust model.

The external service MAY require:

- Independent Authentication
- Independent Authorization
- Delegation Token
- Restricted Scope
- Reauthorization
- Contractual Trust Relationship

An internal authorization decision SHALL NOT automatically grant
unrestricted authority to an external party.

---

# 32. Security Context Preservation

The following security context SHOULD remain traceable across service
boundaries:

- Subject
- Transaction
- Entitlement
- Policy
- Authorization Decision
- Delegation
- Scope
- Consumption State
- Integrity Information

Context propagation SHALL preserve semantic meaning.

---

# 33. Context Transformation

A service MAY transform the representation of the security context.

For example:

Signed Authorization Object

↓

Internal Authorization Reference

↓

Service-Specific Enforcement Context

The transformation SHALL preserve the authorization semantics.

A representation change SHALL NOT constitute an authority expansion.

---

# 34. Audit Correlation

Cross-service authorization SHALL be traceable using correlated
identifiers.

Representative identifiers include:

- Original Transaction ID
- Original Decision ID
- Delegation Chain ID
- Service Request ID
- Service Execution ID
- Policy Evaluation ID
- Event ID

Audit records SHOULD allow reconstruction of the propagation chain.

---

# 35. Propagation Failure

If propagation verification fails, the receiving service SHALL NOT
silently execute the requested operation.

Representative responses include:

- REJECT
- REAUTHORIZE
- RECOVER
- REAUTH
- REENTITLE

The failure SHALL be associated with the current transaction.

---

# 36. Service Execution Constraint

A receiving service SHALL execute only the operation permitted by the
effective authorization scope.

The service SHALL NOT:

- Expand Resource Scope
- Expand Action Scope
- Extend Validity
- Reset Consumption
- Change Subject
- Change Transaction
- Bypass Policy
- Bypass Entitlement
- Bypass Required Reauthorization

without explicit protocol authorization.

---

# 37. Security Properties

The Authorization Scope Propagation and Delegation Boundary Model
provides:

- Explicit authorization scope
- Controlled propagation
- Scope narrowing
- Scope expansion prevention
- Service boundary enforcement
- Resource boundary enforcement
- Action boundary enforcement
- Transaction continuity
- Subject continuity
- Policy continuity
- Entitlement continuity
- Temporal constraints
- Parameter constraints
- Consumption constraints
- Delegation boundary
- Delegation depth control
- Delegation revocation
- Cross-service verification
- External trust boundary
- Security context preservation
- Audit correlation
- Reauthorization control

---

# 38. Non-Goals

Figure26 does not define:

- A specific token format
- A specific cryptographic algorithm
- A specific service mesh
- A specific API gateway
- A specific cloud provider
- A specific policy language
- A specific database
- A specific programming language
- A specific network topology

Implementation details MAY be defined elsewhere.

---

# 39. Design Freeze Decision

Figure26 is designated as the reference model for controlled
authorization scope propagation and delegation boundary enforcement
within the NEW-shot2play Protocol Suite Version 2.0.

Future modifications require Design Freeze review.

